# Browser {#concept_64056_zh .concept}

This topic describes the application of browsers.

## Supported browser {#section_dmt_1bm_lfb .section}

-   IE \(\>=10\) and Edge
-   Mainstream Chrome/Firefox/Safari versions
-   Mainstream Android/iOS/WindowsPhone versions

**Note:** For IE, you must introduce the promise library before introducing the SDK:

## Support for browsers of earlier versions { .section}

The OSS SDK perform operations through File APIs so errors may occur in browsers of earlier versions. You can use third-party plug-ins to implement OSS APIs to perform operations, such as upload and download. For detailed examples, see [Overview of direct transfer on Web client](../../../../../reseller.en-US/Best Practices/Direct upload to OSS from Web/Javascript client signature pass-through.md#).

-   Settings
    -   Bucket settings

        To access OSS through browser, you must set the [CORS](../../../../../reseller.en-US/Developer Guide/Buckets/Cross-origin resource sharing.md#) of the bucket as follows:

        -   Set Source to `*`.
        -   Set Method to `GET, POST, PUT, DELETE, HEAD`.
        -   Set Allowed Headers to `*`.
        -   Set Expose Header to `ETag`.
        **Note:** Set the preceding CORS rule to the first option.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22580/155186587813704_en-US.png)

    -   STS settings

        To prevent exposing the AccessKeyId and AccessKeySecret in the webpage, Alibaba Cloud adopts STS for temporary authorization. The authorization server is maintained by users. Only authenticated users are allowed to apply for a temporary permission from the authorization server. For more information on how to set RAM users and STS policies, see Overview. For information about how to establish your own authorization servers, see [Node.js STS AppServer](https://github.com/rockuw/node-sts-app-server).

-   For example:

    Here, we use SDK to develop a webpage app which offers four features:

    -   Upload objects
    -   Upload text
    -   List objects
    -   Download objects
    For complete code, see [oss-in-browser](https://github.com/rockuw/oss-in-browser). The expected result is as follows:

    Let’s take upload objects as an example in the following text:

    1.  Create a webpage

        -   An object choice box to select the object to be uploaded
        -   A textbox to input the name of the object to be uploaded to the OSS.
        -   A button to start the upload.
        -   A progress bar to display the upload progress
        The following code creates four controls. `class="xxx"` serves to apply the [Bootstrap](http://getbootstrap.com/) style and can be ignored here.

        ```language-html
        <div class="form-group">
          <label>Select file</label>
          <input type="file" id="file" />
        </div>
        <div class="form-group">
          <label>Store as</label>
          <input type="text" class="form-control" id="object-key-file" value="object" />
        </div>
        <div class="form-group">
          <input type="button" class="btn btn-primary" id="file-button" value="Upload" />
        </div>
        <div class="form-group">
          <div class="progress">
            <div id="progress-bar"
                 class="progress-bar"
                 role="progressbar"
                 aria-valuenow="0"
                 aria-valuemin="0"
                 aria-valuemax="100" style="min-width: 2em;">
              0%
            </div>
          </div>
        </div>
        
        ```

    2.  Add a style

        You can add some styles to the webpage to enhance its look. Here, we use [Bootstrap](http://getbootstrap.com/). Add the style in the `<head>` tag as follows:

        ```language-html
        <head>
          <title>OSS in Browser</title>
          <link rel="stylesheet" href="bootstrap.min.css" />
        </head>
        
        ```

        `bootstrap.min.css` can be downloaded from the official website of Bootstrap.

    3.  Add a script

        Before adding a script, the webpage is static and does not respond to any click. We can add some JavaScript so that it can upload/download/list objects.

        First, introduce the SDK in the `<head>` tag:

        ```language-html
        <head>
          <title>OSS in Browser</title>
          <link rel="stylesheet" href="bootstrap.min.css" />
          <script type="text/javascript" src="http://gosspublic.alicdn.com/aliyun-oss-sdk-x.x.x.min.js"></script>
          <script type="text/javascript" src="app.js"></script>
        </head>
        
        ```

        `app.js` contains the code that implements the object uploading. The content is as follows:

        ```language-js
        const appServer = 'http://localhost:3000';
        const bucket = 'js-sdk-bucket-sts';
        const region = 'oss-cn-hangzhou';
        
        const urllib = OSS.urllib;
        
        const applyTokenDo = function (func) {
          const url = appServer;
          return urllib.request(url, {
            method: 'GET'
          }).then(function (result) {
            const creds = JSON.parse(result.data);
            const client = new OSS({
              region: region,
              accessKeyId: creds.AccessKeyId,
              accessKeySecret: creds.AccessKeySecret,
              stsToken: creds.SecurityToken,
              bucket: bucket
            });
        
            return func(client);
          });
        };
        
        let currentCheckpoint;
        const progress = async function progress(p, checkpoint) {
          currentCheckpoint = checkpoint;
          const bar = document.getElementById('progress-bar');
          bar.style.width = `${Math.floor(p * 100)}%`;
          bar.innerHTML = `${Math.floor(p * 100)}%`;
        };
        
        let uploadFileClient;
        const uploadFile = function (client) {
          if (! uploadFileClient || Object.keys(uploadFileClient).length === 0) {
            uploadFileClient = client;
          }
          
          const file = document.getElementById('file').files[0];
          const key = document.getElementById('object-key-file').value.trim() || 'object';
          console.log(`${file.name} => ${key}`);
        
          const options = {
            progress,
            partSize: 100 * 1024,
            meta: {
              year: 2017,
              people: 'test',
            },
          };
        
          return client.multipartUpload(key, file, options).then( (res) => {
            console.log('upload success: %j', res);
        	currentCheckpoint = null;
            uploadFileClient = null;
          }).catch((err) => {
            if (uploadFileClient && uploadFileClient.isCancel()) {
              console.log('stop-upload!') ;
            } else {
              console.error(err);
            }
          });
        };
        
        window.onload = function () {
          document.getElementById('file-button').onclick = function () {
            applyTokenDo(uploadFile);
          }
        };
        
        ```

        The following are the steps to uploading an object:

        1.  Apply for a temporary permission from the authorization server. The authorization server is a service built by the website developer to provide temporary authorization for end users. Developers can provide different permissions for different users during authorization \(see [Overview](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#)\). For authorization server information, see [GitHub](https://github.com/rockuw/node-sts-app-server). To make it more convenient, the authorization server in the example returns the temporary credential to the user directly.
        2.  Create an OSS client using a temporary key.
        3.  Upload the selected files through the `multipartUpload` interface and set the progress bar through the `progress` parameter.
        4.  Other features

            For text content uploading, object list retrieval, and object downloading features, see the example code: [OSS in Browser](https://github.com/rockuw/oss-in-browser).


