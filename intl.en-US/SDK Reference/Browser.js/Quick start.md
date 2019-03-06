# Quick start {#concept_32069_zh .concept}

This topic describes how to quickly access the OSS service in the BrowserJS-SDK, including uploading/downloading files and viewing file lists.

**Note:** Since the Alibaba Cloud account AccessKey gives you permission to access all APIs, we recommend that you not use the AK that can cause configuration leak by following the Alibaba Cloud Security Best Practices. Therefore, you must create and use STS \(temporary account permission\) to authorize the client.

For more information about browser usage, see [Browser Application](reseller.en-US/SDK Reference/Browser.js/Browser.md#).

For demo project, see[example](https://github.com/ali-sdk/ali-oss/tree/master/example).

## Bucket settings {#section_dp4_gkl_lfb .section}

The [CORS settings](../../../../../reseller.en-US/Developer Guide/Buckets/Cross-origin resource sharing.md#) of the bucket must be enabled to access OSS directly in a browser:

-   Set allowed origins to `*` 
-   Set allowed methods to `PUT, GET, POST, DELETE, and HEAD`.
-   Set allowed headers to `*`.
-   Set expose headers to `ETag` `x-oss-request-id`.

**Note:** Set the CORS rule as the first option.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22571/155186560313696_en-US.png)

## Use the SDK { .section}

Currently, browsers do not support direct bucket-related operations \(for example, list buckets, and get/set bucket logging/referer/website/cors\), but support object-related operations \(for example, upload/download files, and view file lists\).

-   Include the SDK

    First, include the following tags in the `<head>` of the HTML file:

    ```language-html
    <script src="http://gosspublic.alicdn.com/aliyun-oss-sdk-x.x.x.min.js"></script>
    
    ```

    For more introduction methods, see [Installation](reseller.en-US/SDK Reference/Browser.js/Installation.md#).

    Create a client using `new OSS.Wapper()`. OSS.Wrapper provides an asynchronous API similar to the callback method to process the returned results in `.then()` and process errors in `.catch()`.

-   Build an STS Server and obtain temporary authorization information from the client
    1.  Activate the STS service for your account. If the service has been activated, you can skip this step. To activate the STS service, see [STS Activation](../../../../../reseller.en-US/Developer Guide/Hide/Access control/STS temporary access authorization.md#). For more information on how to configure OSS authorization policy, see [RAM and STS authorization policy configuration](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).
    2.  Apply the exclusive STS signature service through the STS SDK or API. To help you quickly get started with the STS service, we provide demo programs in multiple languages:

        -   NodeJS: [Download address](https://github.com/ali-sdk/ali-oss/blob/master/example/server/app.js#L9) 
        -   PHP: [Download address](http://oss-demo.aliyuncs.com/app-server/sts-server.zip?spm=5176.doc31920.2.5.Fve3RI&file=sts-server.zip) 
        -   Java: [Download address](https://gosspublic.alicdn.com/AppTokenServerDemo.zip?spm=5176.doc31920.2.6.Fve3RI&file=AppTokenServerDemo.zip) 
        -   Ruby: [Download address](https://github.com/rockuw/sts-app-server?spm=5176.doc31920.2.7.Fve3RI) 
        **Note:** The demo program is only used as reference for STS feature implementation. To implement STS features in the production environment, you must develop codes required for authentication and other functions.

    3.  Initiate requests to the STS service built in the frontend \(browser\) to obtain the STS temporary authorization information.

        ```language-html
        <script type="text/javascript">
          //OSS. urlib is the logic of sending requests encapsulated within the SDK. Developers can use any library that sends requests to send requests to 'sts-server '.
          OSS.urllib.request("http://your_sts_server/", {method: 'GET'}, (err, response) => {
              if (err) {
        	    return alert(err);
              }
              try {
                result = JSON.parse(response);
              } catch (e) {
                  errmsg = 'parse sts response info error: ' + e.message;
                  return alert(errmsg);
              }
             // console.log(result)
           })
        </script>
        
        ```

-   View the object list

    ```language-html
    <script type="text/javascript">
    OSS.urllib.request("http://your_sts_server/", {method: 'GET'}, (err, response) => {	
      if (err) {
    	return alert(err);
      }
    
      try {
        result = JSON.parse(response);
      } catch (e) {
        return alert('parse sts response info error: ' + e.message);
      }
      
      let client = new OSS({
        accessKeyId: result.AccessKeyId,
    	accessKeySecret: result.AccessKeySecret,
    	stsToken: result.SecurityToken,
    	endpoint: '<oss endpoint>',
    	bucket: '<Your bucket name>'
      });
      
      client.list({
        'max-keys': 10
      }).then(function (result) {
        console.log(result);
      }).catch(function (err) {
        console.log(err);
      });
    });
    </script>
    
    ```

    The parameter `region` refers to the region used when you applied for OSS, such as `oss-cn-hangzhou`. The completeregion list can be viewed in [OSS Nodes](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

    Open the HTML file in the browser, and open the Chrome Developer Console to view the result in List files.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22571/155186560313697_en-US.png)

    **Note:** A browser is an untrusted environment. An extremely high risk occurs if you store the AccessKeyID and AccessKeySecret in a browser. We recommend that you only use plaintext settings for testing purposes. `STS authentication mode` or `self-signed mode` is recommended for business applications. For more information, see [Resource Access Management](reseller.en-US/SDK Reference/Android/Access control.md#) and [Direct Transfer on a Mobile Device](../../../../../reseller.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#).

-   Upload an object

    We use the `multipartUpload` API to upload objects. For more information about `multipartUpload`, see [Multipart upload](reseller.en-US/SDK Reference/Browser.js/Upload objects.md#).

    ```language-html
    <body>
      <input type="file" id="file" />
      <script type="text/javascript">
        document.getElementById('file').addEventListener('change', function (e) {
          let file = e.target.files[0];
          let storeAs = 'upload-file';
          console.log(file.name + ' => ' + storeAs);
    	  //OSS. urlib is the logic of sending requests encapsulated within the SDK. Developers can use any library that sends requests to send requests to 'sts-server '.
    	  OSS.urllib.request("http://your_sts_server/", {method: 'GET'}, (err, response) => {
    		  if (err) {
    			return alert(err);
    		  }
    		  try {
    			result = JSON.parse(response);
    		  } catch (e) {
    			return alert('parse sts response info error: ' + e.message);
    		  }
    		  let client = new OSS({
    			accessKeyId: result.AccessKeyId,
    			accessKeySecret: result.AccessKeySecret,
    			stsToken: result.SecurityToken,
    			endpoint: '<oss endpoint>',
    			bucket: '<Your bucket name>'
    		  });
              //storeAs indicates the uploaded object name and file indicates the uploaded file.
    		  client.multipartUpload(storeAs, file).then(function (result) {
    			console.log(result);
    		  }).catch(function (err) {
    			console.log(err);
    		  });
    		});
    	});
      </script>
    </body>
    
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22571/155186560313698_en-US.png)

-   Download objects

    You cannot directly operate on files in the browser. Therefore, the download link of a file is generated using the signed URL method, andyou can download the file by clicking the link.

    ```language-html
    <body>
      <input type="button" id="download" value="Download" />
      <script type="text/javascript">
        document.getElementById('download').addEventListener('click', function (e) {
          let objectKey = 'test/download_file';
          let saveAs = 'download_file';
          console.log(objectKey + ' => ' + saveAs);
    	  //OSS. urlib is the logic of sending requests encapsulated within the SDK. Developers can use any library that sends requests to send requests to 'sts-server '.
    	  OSS.urllib.request("http://your_sts_server/", {method: 'GET'}, (err, response) => {
    		  if (err) {
    			return alert(err);
    		  }
    		  try {
    			result = JSON.parse(response);
    		  } catch (e) {
    			return alert('parse sts response info error: ' + e.message);
    		  }
    		  let client = new OSS({
    			accessKeyId: result.AccessKeyId,
    			accessKeySecret: result.AccessKeySecret,
    			stsToken: result.SecurityToken,
    			endpoint: '<oss endpoint>',
    			bucket: '<Your bucket name>'
    		  });
    		  let result = client.signatureUrl(objectKey, {
    			expires: 3600,
    			response: {
    			  'content-disposition': 'attachment; filename="' + saveAs + '"'
    			}
    		  });
    		  console.log(result);
    		  window.location = result;
    		});
        });
      </script>
    </body>
    
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22571/155186560313699_en-US.png)


