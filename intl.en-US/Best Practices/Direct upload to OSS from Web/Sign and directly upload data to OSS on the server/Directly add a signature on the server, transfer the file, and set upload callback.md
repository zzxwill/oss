# Directly add a signature on the server, transfer the file, and set upload callback {#concept_qp2_g4y_5db .concept}

## Background {#section_rt2_q4y_5db .section}

For the background information, see [Overview of direct transfer on Web client](reseller.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#) .

The usage of [Direct transfer after adding a signature on the server](reseller.en-US/Best Practices/Direct upload to OSS from Web/Direct transfer after adding a signature on the server.md#) solution experiences a few issues. Once the user uploads data, the application server has to be updated with the files user uploads, the file names, image size \(if any images are uploaded\), and so on. Hence, the upload callback function is developed for OSS.

-   User request logic

    1.  The user obtains the upload policy and callback settings from the application server.
    2.  The application server returns the upload policy and callback settings.
    3.  The user sends a file upload request directly to OSS.
    4.  Once the file data is uploaded and before a response is sent by OSS to the user, OSS sends a request to the user’s server based on the user’s callback settings.
    5.  If the server returns success, OSS returns success to the user. If the server returns failed, OSS returns failed to the user. This makes sure the application server is be notified of all images that the user has successfully uploaded.
    6.  The application server returns information to OSS.
    7.  OSS returns the information returned by the application server to the user.
    In brief, the user needs to upload a file to the OSS server. And, it is assumed that the user’s application server is notified once the upload is completed. In this case, a callback function is required to be set to update user’s application server. Due to this, OSS starts the upload once it receives user’s upload request. It does not return the result to the user directly after uploading, but notifies the user’s application server first with a system-generated message such as “I completed uploading”; then, the application server notifies OSS by sending “I got it. Please pass on the information to my owner” message. After sending these notifications, OSS transfers the result to the user.

-   Example

    Sample test of user's Computer Browser: [Click here to experience the upload callback example](http://oss-demo.aliyuncs.com/oss-h5-upload-js-php-callback/index.html)

    Use your phone to test if the upload is valid. You can use a cell phone \(WeChat, QQ, mobile browsers, and so on\) to scan the QR code \(this is not an advertisement, but the QR code for the URL provided above. The operation allows you to see whether the services work perfectly on cell phones.\)

-   Download code

    Click [here](http://gosspublic.alicdn.com/web-upload/oss-h5-upload-js-php-callback.zip) to download the code. 

    The example adopts a backend signature and uses PHP language.

    -   Click [here](https://gosspublic.alicdn.com/AppPostPolicyServer.zip) for the example of a backend signature using Java language.
    -   Click [here](http://gosspublic.alicdn.com/web-upload/post_policy_callback.go) for the example of a backend signature using Go language.
    -   Click [here](http://gosspublic.alicdn.com/web-upload/post_policy_callback.py) for the example of a backend signature using Python language.
    Usage of other languages:

    1.  Download the corresponding language example.
    2.  Modify the example code, for example, set the listening port, and then start running.
    3.  At upload.js in `oss-h5-upload-js-php-callback.zip`, change the variable severUrl to the address configured at step 2. For example, severUrl = `http://1.2.3.4:8080` or serverUrl= `http://abc.com/post/`.
-   Quick start guide

    Follow the steps to upload a file to OSS through the Webpage, and OSS sends a callback notification to the application server set by the user.

    1.  Set your own  id, key, and bucket.

        Setting method: Modify `php/get.php`, and set the variable $id to AccessKeyId, $key to AccessKeySecret, and $host to  bucket+endpoint.

        ```
        $id= '******';
         $key= 'xxxxx';
         $host = 'http://post-test.oss-cn-hangzhou.aliyuncs.com
        ```

    2.  To guarantee browsing security, CORS must be set for bucket.
    3.  Set your own callback URL. It is also known as your own callback server address. For example, `http://abc.com/test.html` \(can be accessed through public network\). OSS sends the file uploading information to the application server through the callback URL \(`http://abc.com/test.html`\) set by you after the file is uploaded. Setting method: Modify php/get.php \(for this callback server code instance, see the following content\).

        ```
        $callbackUrl = "http://abc.com/test.html";
        ```

        For more information such as uploading signature and setting a random file name, [click here for uploading details](reseller.en-US/Best Practices/Direct upload to OSS from Web/Direct transfer after adding a signature on the server.md#).

-   Core code analysis

    The following content is to be added to the code:

    ```
    new_multipart_params = {
    &nbsp;&nbsp;&nbsp;&nbsp; 'key' : key + '${filename}',
    &nbsp;&nbsp;&nbsp;&nbsp; 'policy': policyBase64,
    &nbsp;&nbsp;&nbsp;&nbsp; 'OSSAccessKeyId': accessid,
    &nbsp;&nbsp;&nbsp;&nbsp; 'success_action_status' : '200', //Instructs the server to return 200. Otherwise, the server returns 204 by default.
    &nbsp;&nbsp;&nbsp;&nbsp; 'callback': 　callbackbody,
    &nbsp;&nbsp;&nbsp;&nbsp; 'signature': signature,
     };
    ```

    The preceding callbackbody  is returned by the PHP server. In this example, the following content is obtained by running the PHP scripts on the backend:

    ```
    {"accessid":"6MKO******4AUk44",
    "host":"http://post-test.oss-cn-hangzhou.aliyuncs.com",
    "policy":"eyJleHBpcmF0aW9uIjoiMjAxNS0xMS0wNVQyMDo1M******iLCJjdb25kaXRpb25zIjpbWyJjdb250ZW50LWxlbmd0aC1yYW5nZSIsMCwxMDQ4NTc2MDAwXSxbInN0YXJ0cy13aXRoIiwiJGtleSIsInVzZXItZGlyXC8iXV19",
    "signature":"VsxOcOudx******z93CLaXPz+4s=",
    "expire":1446727949,
    "callback":"eyJjYWxsYmFja1VybCI6Imh0dHA6Ly9vc3MtZGVtby5hbGl5dW5jcy5jdb206MjM0NTAiLCJjYWxsYmFja0hvc3QiOiJvc3MtZGVtby5hbGl5dW5jcy5jdb20iLCJjYWxsYmFja0JvZHkiOiJmaWxlbmFtZT0ke29iamVjdH0mc2l6ZT0ke3NpemV9Jm1pbWVUeXBlPSR7bWltZVR5cGV9JmhlaWdodD0ke2ltYWdlSW5mby5oZWlnaHR9JndpZHRoPSR7aW1hZ2VJdbmZvLndpZHRofSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==","dir":"user-dirs/"}
    ```

    The preceding callbackbody is the Base64 encoded callback content in the returned results.

    The decoded content is as follows:

    ```
    {"callbackUrl":"http://oss-demo.aliyuncs.com:23450",
    "callbackHost":"oss-demo.aliyuncs.com",
    "callbackBody":"filename=${object}&size=${size}&mimeType=${mimeType}&height=${imageInfo.height}&width=${imageInfo.width}",
    "callbackBodyType":"application/x-www-form-urlencoded"}
    ```

    Content analysis:

    -   callbackUrl: Specifies the URL request sent by OSS to this host.
    -   callbackHost: Specifies the Host header to be included in the request header when this request is sent by the OSS.
    -   callbackBody: Specifies the content sent to the application server upon OSS request. This can include a file name, size of the file, type, and image and its size \(if any\).
    -   callbackBodyType: Specifies the Content-Type requested to be sent.
-   Callback application server

    Step 4 and 5 is important in the user’s request logic. When OSS interacts with the application server. The following are a few questions explained with answers.

    -   Question: If I am a developer, how can I confirm that the request was sent from OSS?

        Answer: When OSS sends a request, it constructs a signature with the application server. Both use signatures to guarantee security.

    -   Question: How is this signature constructed? Is there any sample code?

        Answer: Yes. The preceding example shows the sample code of the application server callback: `http://oss-demo.aliyuncs.com:23450` \(only supports Linux now\).

        The preceding code runs as follows: [callback\_app\_server.py.zip](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/callback_app_server.py.zip?spm=a2c4g.11186623.2.12.Sf8gc1&file=callback_app_server.py.zip)

        Running solution: Directly run the file python callback\_app\_server.py under the Linux system. 

        The program automatically implements a simple http server. To run this program, you may need to install the system environment on which the RSA depends.

    -   Question: Why the callback request received by my application server does not have an Authorization header?

        Answer: Some Web servers resolve the Authorization header automatically, for example, apache2. Therefore, it is set not to resolve this header. Using apache2 as an example, the specific setting method is as follows:

        1.  Start the rewrite module, and run the command: a2enmod rewrite.
        2.  Modify the configuration file /etc/apache2/apache2.conf \(it varies with the installation path of apache2\). Set Allow Override to All, and then add the following content:

            ```
            RewriteEngine on         
            RewriteRule .* - [env=HTTP_AUTHORIZATION:%{HTTP:Authorization},last]
            ```

    The sample program demonstrates how to check the signature received by the application server. You must add the code for parsing the format of the callback content received by the application server.

    Callback application server versions in other languages

    -   Java version:
        -   Download address: [click here](https://gosspublic.alicdn.com/images/AppCallbackServer.zip?spm=a2c4g.11186623.2.13.Sf8gc1&file=AppCallbackServer.zip)
        -   Running method: Extract the archive and run `java -jar oss-callback-server-demo.jar 9000` \(9000 is the port number and can be changed as required\).

            **Note:** This jar runs on java 1.7. If any issue occurs, you may make changes based on the provided code. This is a maven project.

    -   PHP version:
        -   Download address: [click here](https://gosspublic.alicdn.com/callback-php-demo.zip?spm=a2c4g.11186623.2.14.Sf8gc1&file=callback-php-demo.zip)
        -   Running method: Deploy a program to an Apache environment. Due to the characteristics of PHP language, retrieving headers depends on the environment. You can make changes to the example based on your own environment.
    -   Python version:
        -   Download address: [click here](https://gosspublic.alicdn.com/images/callback_app_server.py.zip?spm=a2c4g.11186623.2.15.Sf8gc1&file=callback_app_server.py.zip)
        -   Running method: Extract the archive and directly run python callback\_app\_server.py. The program implements a simple HTTP  server. To run this program, you may be required to install the system environment on which the RSA depends.
    -   Ruby version:
        -   Download address: [click here](https://github.com/rockuw/oss-callback-server?spm=a2c4g.11186623.2.16.Sf8gc1)
        -   Running method: ruby aliyun\_oss\_callback\_server.rb.

## Summary {#section_bcm_sty_5db .section}

-   Example 1: Describes how to add a signature directly on the JavaScript client and upload a file in the form to OSS directly. [oss-h5-upload-js-direct.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-direct.tar.gz?spm=a2c4g.11186623.2.17.Sf8gc1&file=oss-h5-upload-js-direct.tar.gz)
-   Example 2: Describes how to obtain a signature from the backend using the PHP script and then upload the file in a form to OSS directly. [oss-h5-upload-js-php.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-php.tar.gz?spm=a2c4g.11186623.2.18.Sf8gc1&file=oss-h5-upload-js-php.tar.gz)
-   Example 3: Describes how to obtain a signature from the backend using the PHP script, and perform callback after uploading, and then, upload the form directly to OSS. Consequently, OSS calls back the application server and returns the result to the user. [oss-h5-upload-js-php-callback.tar.gz](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/sample/oss-h5-upload-js-php-callback.tar.gz?spm=a2c4g.11186623.2.19.Sf8gc1&file=oss-h5-upload-js-php-callback.tar.gz)

