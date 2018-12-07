# Direct transfer after adding a signature on the server {#concept_en4_sjy_5db .concept}

## Background {#section_x4w_tjy_5db .section}

Direct signature by JS clients has a serious hidden security hazard in that the OSS AccessId/AcessKey are exposed on the front-end which may be accessible to others. This document explains how to get a signature from and upload a policy to the backend PHP code.

The logic for uploading a signature to the backend is as follows:

1.  Before uploading an image, the client obtains the uploaded policy and signature from the application server.
2.  The client directly uploads the obtained signature to the OSS.

## Signature sample uploaded to the backend {#section_ugn_1ky_5db .section}

-   Download sample:
    -   Click [here](http://oss-demo.aliyuncs.com/oss-h5-upload-js-php/index.html) to download a test sample on a PC browser.
    -   You can test whether the upload was effective on a mobile phone. You can use a mobile phone app \(such as WeChat, QQ, and mobile browsers\) to scan the QR code.

        This is not an advertisement, but a QR code for the preceding URL. This operation allows you to see whether the service works as intended on mobile phones.

-   Download code:

    Click [here](http://gosspublic.alicdn.com/web-upload/oss-h5-upload-js-php.zip) to download the code.

    This example adopts the backend signature, and uses PHP language.

    -   Click [here](https://gosspublic.alicdn.com/AppPostPolicyServer.zip) for the example of a backend signature using Java language.
    -   Click [here](http://gosspublic.alicdn.com/web-upload/post_policy.py) for the example of a backend signature using Python language.
    -   Click [here](http://gosspublic.alicdn.com/web-upload/post_policy.go) for the example of a backend signature using Go language.
    Usage of other languages:

    1.  Download the corresponding language example.
    2.  Modify the example code. For instance, set the listening port, and then start running.
    3.  At `upload.js` in `oss-h5-upload-js-php.zip`, change the variable serverUrl to the address configured at step 2. For example, = serverUrl=`http://1.2.3.4:8080` or serverUrl=`http://abc.com/post/`.

## Principle of constructing a Post signature on the server end {#section_pcq_kky_5db .section}

The OSS PostObject method is used for uploads. You can construct a PostObject request in the browser using Plupload and send the request to the OSS. Signatures are implemented on the server in PHP. In the same principle, the server can be compiled in Java, .NET, Ruby, Go, or Python language. The core logic is to construct a Post signature. The Java and PHP examples are provided here. The following steps are required: 

1.  The webpage requests the signature through JavaScript from the server end.
2.  After JavaScript gets the signature, it uploads the signature to the OSS through Plupload.

-   Implementation
    1.  Populate the fields with your ID, key, and bucket.

        Modify php/get.php:

        -   Set the variable $id to AccessKeyId.
        -   Set $key to AccessKeySecret.
        -   Set $host to bucket+endpoint.

            **Note:** For information on the endpoint, see [Basic OSS concepts](../../../../intl.en-US/Developer Guide/Basic concepts.md#).

            ```
            $id= 'xxxxxx';
              $key= 'xxxxx';
              $host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com
            ```

    2.  You must set CORS for the bucket to guarantee browser safety.

        **Note:** Make sure that the CORS settings of the bucket attribute support the POST method. This is because, HTML directly uploads data to OSS and produces a cross-origin request in the process. Hence, you must allow cross-original requests in the bucket attributes.

        For procedure, see [Set CORS](../../../../intl.en-US/Console User Guide/Manage buckets/Set Cross-Origin Resource Sharing (CORS).md#). The settings are as follows:

        **Note:** In earlier-version IE browsers, Plupload is executed in flash. You must set crossdomain.xml.


## Details of core logic {#section_hs2_dly_5db .section}

-   Set random object names

    You often need to name uploaded objects randomly, if they have the same suffix as the objects on the client. In this example, two radios are used to differentiate. If you want to fix the settings to apply random names to the uploaded objects, you can change the function to the following:

    ```
    function check_object_radio() {
        g_object_name_type = 'random_name';
    }
    ```

    If you want to set uploads to the user's objects, you can change the function to the following:

    ```
    function check_object_radio() {
        g_object_name_type = 'local_name';
    }
    ```

-   Set the upload directory

    The upload directory is specified by the server end \(in PHP\), which enhances security. Each client is only allowed to upload objects to a specific directory. This guarantees security by isolation. The following code changes the upload directory address to abc/ \(the address must end with `/`\).

    ```
    $dir = 'abc/';
    ```

-   Set the filtering conditions for uploaded objects

    We often need to set the filtering conditions for uploads. For example, only allowing image uploads, setting the size of uploaded objects, and disallowing repeated uploads. You can use the filters parameter for this.

    ```
    var uploader = new plupload.Uploader({
        ……
        filters: {
            mime_types : [ //Only images and zip objects are allowed to be uploaded
            { title : "Image files", extensions : "jpg,gif,png,bmp" },
            
            ], 
            max_file_size : '400kb', //Only objects with a maximum size of 400 KB are allowed to be uploaded
            prevent_duplicates : true //Repeated objects are not allowed to be selected
        },
    ```

    Use the Plupload attribute filters to set filtering conditions.

    Explanations of the preceding setting values:

    -   mime\_types: Restrict extensions of the uploaded objects.

    -   max\_file\_size: Restrict the size of the uploaded objects.

    -   prevent\_duplicates: Restrict repeated uploads.

        **Note:** The filter conditions are not required. You can comment out the filtering condition, if you do not need it.

-   Get uploaded object names

    If you want to know the name of the uploaded object, you can use Plupload to call the FileUploaded event, as follows:

    ```
    FileUploaded: function(up, file, info) {
                if (info.status == 200)
                {
                    document.getElementById(file.id).getElementsByTagName('b')[0].innerHTML = 'upload to oss success, object name:' + get_uploaded_object_name(file.name);
                }
                else
                {
                    document.getElementById(file.id).getElementsByTagName('b')[0].innerHTML = info.response;
                }
        }
    ```

    You can use the following functions to get the names of the objects uploaded to OSS. The file.name property records the names of the uploaded local objects.

    ```
    get_uploaded_object_name(file.name)
    ```

-   Upload signatures

    JavaScript can get the policyBase64, accessid, and signature variables from the backend. The following is the core code for getting the three variables:

    ```
    phpUrl = './php/get.php'
            xmlhttp.open( "GET", phpUrl, false );
            xmlhttp.send( null );
            var obj = eval ("(" + xmlhttp.responseText+ ")");
            host = obj['host']
            policyBase64 = obj['policy']
            accessid = obj['accessid']
            signature = obj['signature']
            expire = parseInt(obj['expire'])
            key = obj['dir']
    ```

    Parse xmlhttp.responseText \(the following only serves as an example. The actual format may vary, but the values of signature, accessid, and policy must exist\).

    ```
    {"accessid":"6MKOqxGiGU4AUk44",
    "host":"http://post-test.oss-cn-hangzhou.aliyuncs.com",
    "policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0xMS0wNVQyMDoyMzoyM1oiLCJjxb25kaXRpb25zIjpbWyJjcb250ZW50LWxlbmd0aC1yYW5nZSIsMCwxMDQ4NTc2MDAwXSxbInN0YXJ0cy13aXRoIiwiJGtleSIsInVzZXItZGlyXC8iXV19",
    "signature":"I2u57FWjTKqX/AE6doIdyff151E=",
    "expire":1446726203,"dir":"user-dir/"}
    ```

    -   accessid: It is the Accessid of the user request. However, disclosing Accessid does not impact data security.
    -   host: The domain name to which the user wants to send an upload request.
    -   policy: A policy for uploading user forms. It is a Base64-encoded string.
    -   signature: A signature string for the policy variable.
    -   expire: It is the expiration time of the current upload policy. This variable is not sent to OSS, because it is already indicated in the policy.
    Parse policy. The decoded content of the policy is as follows:

    ```
    {"expiration":"2015-11-05T20:23:23Z",
    "conditions":[["content-length-range",0,1048576000],
    ["starts-with","$key","user-dir/"]]
    ```

    For more information about Policy, see [Policy basic elements](https://www.alibabacloud.com/help/doc-detail/28663.htm).

    The key content of the PolicyText specifies the final expiration time of this policy. Before its expiry, this policy may be used to upload objects. Therefore, it is not necessary to obtain a signature from the backend for each upload.

    Here, we use the following designs: 

    -   For initial uploads, a signature is obtained for each object upload.
    -   For subsequent uploads, the current time is compared with the signature time to see whether the signature has expired.
        -   If the signature expires, a new signature is obtained.
        -   If the signature has not expired, the same signature is used. The expired variable is used here.
    The core code is as follows:

    ```
    now = timestamp = Date.parse(new Date()) / 1000;
    [color=#000000]//This determines whether the time specified by the expire variable is earlier than the current time. If so, a new signature is obtained. 3s is the buffer duration.[/color]
        if (expire < now + 3)
    {  
    　　   ..... 
    　　 phpUrl = './php/get.php'
    　　 xmlhttp.open( "GET", phpUrl, false );
    　　 xmlhttp.send( null );
    　　   ......
    }
    return .
    ```

    We see that **starts-with** has been added to the policy content. This indicates the name of the object to be uploaded must start with the user-dir \(this string can be customized\).

    This setting is added because, in many scenarios, one bucket is used for one app and contains the data of different users. To prevent the data from being overwritten, a specific prefix is added to the objects uploaded by a specific user to OSS.

    However, an issue occurs. Once the users obtains this policy, they can modify the upload prefix before the expiration time to upload objects to another user's directory. To resolve this issue, you can set the application server to specify the prefix of the uploaded objects by a specific user at the time of upload. In this case, no one can upload objects with another user's prefix even after obtaining the policy. This guarantees data security.


## Summary {#section_sxn_4ny_5db .section}

In the sample mentioned in this document, the webpage end requests the signature from the server end during uploads from the webpage end, and then objects are uploaded directly, with no pressure on the server end. This approach is safe and reliable.

However in this sample, the backend program is not immediately aware of the number or identity of objects uploaded. You can use upload callback to see which objects were uploaded. This sample cannot implement multipart and breakpoint.

## Related Documents {#section_zym_pny_5db .section}

-   [Basic concepts](../../../../intl.en-US/Developer Guide/Basic concepts.md#)
-   [Set Cross-Origin Resource Sharing \(CORS\)](../../../../intl.en-US/Console User Guide/Manage buckets/Set Cross-Origin Resource Sharing (CORS).md#)
-   [Overview of direct transfer on Web client](intl.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)
-   [Directly add a signature on the server, transfer the file, and set upload callback](intl.en-US//Directly add a signature on the server, transfer the file, and set upload callback.md#)
-   [Set up direct data transfer for mobile apps](intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#)

