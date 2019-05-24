# Javascript client signature pass-through {#concept_frd_4gy_5db .concept}

## Backdrop {#section_gj5_pgy_5db .section}

The client is signed directly with JavaScript and then uploaded to OSS. Please see the background introduction in the Web-side direct transmission practice.

Example

The following is a description of the plupoad. An example of signing on the Javascript side and then passing data directly to OSS.

Use your phone to test if the upload is valid. Two-dimensional code: can use mobile phone \(WeChat, QQ, mobile phone browser, etc\) give it a try. \(This is not an advertisement, but a two-dimensional code for the above-mentioned web site, in order to let everyone see this implementation can run perfectly on the mobile phone .\)

File Upload is a public upload to a test. Bucket is cleaned up regularly, so don't pass on sensitive and important data.

code download

Click here: oss-h5-upload-js-direct.zip

Principle

-   The functionality of this example

    -   Direct submission of form data \(that is, postobject\) to OSS using procopad.
    -   Support HTML5, Flash, Silverlight, html4 and other protocols upload.
    -   Can run in PC browser, mobile phone browser, WeChat, etc.
    -   You can choose to upload multiple files.
    -   Display upload progress bar.
    -   You can control the size of the uploaded file.
    -   You can set the upload to the specified directory and set whether the Upload File name is a random file name or a local file name.
    The Postobject API details of OSS can be referred.

-   Plupload

    Procopad is a simple, easy-to-use and powerful file upload tool, Supports multiple upload methods, including HTML5, Flash, Silverlight, Html4. Intelligent Detection of the current environment, choosing the best way to do so, and adopting HTML5 is a priority.

-   Key code

    Because the OSS supports the post protocol. So all you need to do is bring the OSS signature when you send a POST request. The core code is as follows:

    ```
    VaR deliader = new pluopad. deliader ({
        Runtimes: 'html5, Flash, Silverlight, html4 ',
        Browse_button: 'selectedfile ',
        // Runtimes: 'Flash ',
        Container: Document. getelindbyid ('container '),
        Flash_swf_url: "lib/plupload-2.1.2/JS/Moxie.swf ',
        Silverlight_xap_url: 'lib/FIG/JS/moxie. xap ',
        URL: host,
        Multipart_params :{
            'Filename ':' $ {filename }',
            'Key': '$ {filename }',
            'Policy': policybase64,
            'Porter': Access Sid,
            'Success _ action_status': '200', // Let the server return 200, otherwise, 204 is returned by default
            'Signature ': Signature,
        },
    　....
    }
    ```

    One thing to note here is 'filename ': '$ \{Filename \}', The purpose of this piece of code is to indicate that the original file text is maintained after the upload. If you want to upload to a specific directory such as ABC, the file name remains the same as the original file name, so this should be written:

    ```
    Multipart_params :{
            'Filename ': 'abc/' + '$ {filename }',
            'Key': '$ {filename }',
            'Policy': policybase64,
            'Porter': Access Sid,
            'Success _ action_status': '200', // Let the server return 200, otherwise, 204 is returned by default
            'Signature ': Signature,
        },
    ```

-   Set to random file name

    Sometimes you need to set the user-uploaded file to a random file name, And the suffix is consistent with the client file. In the example, two radios are used to distinguish, If you want to be fixed to a random file name at the time of delivery, you can change the function to the following:

    ```
    Function fig (){
        G_object_name_type = 'random _ name ';
    }
    ```

    If you want to fix the file that is set to the user when you pass it over, you can change the function:

    ```
    Function fig (){
        G_object_name_type = 'Local _ name ';
    }
    ```

-   Set the upload directory

    Files can be uploaded to the specified directory, and directory-related settings can be experienced in the example, if you want your code to be uploaded to a fixed directory such as ABC, you can change it as follows, note '/' End.

    ```
    Function get_dirname ()
    {
        G_dirname = "ABC /"; 
    }
    ```

-   Upload signatures

    Signing signature is primarily a signing of policytext, and the simplest example is:

    ```
    VaR policytext = {
        "Expiration": "00: 00: 00.000z ", // set the failure time for this policy, after which the failure time is exceeded, there's no way to upload files through this policy.
        "Conditions ":[
        ["Content-Length-range", 0, 1048576000] // set the size limit for the uploaded file, if this size is exceeded, the file was uploaded to OSS, and it will be reported wrong.
        ]
    }
    ```

-   Cross Domain CORS

    **Note:** Make sure that the bucket property CORS setting supports the POST method. Because this HTML is uploaded directly to OSS, cross-domain requests are generated. You must set the allow cross-domain inside the bucket property.

    Set the following figure:

    **Note:** In earlier versions of the IE browser, pluopad is executed in flash mode. Crossdomain. xml must be set , The setting method can be referred to: click here

-   CAUTION

    Writing the access key ID and the access key secret inside the code poses a risk of leakage. It is recommended to use back-end signatures upload scheme: Post-server signatures after direct transmission of web pages


