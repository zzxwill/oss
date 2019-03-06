# Installation {#concept_32068_zh .concept}

This topic describes how to install Node.js. Demos in the official documents are based on SDK 6.X. For SDKs of earlier versions, see [Document for 5.X SDK](https://github.com/ali-sdk/ali-oss/blob/5.x/README.md). For more information about upgrade your SDK to 6.X, see [Upgrade document](https://github.com/ali-sdk/ali-oss/blob/master/UPGRADING.md).

-    [GitHub](https://github.com/ali-sdk/ali-oss) 
-    [API document](https://github.com/ali-sdk/ali-oss#summary) 
-    [ChangeLog](https://github.com/ali-sdk/ali-oss/blob/master/CHANGELOG.md) 

## Environment preparations {#section_bv1_szj_lfb .section}

-   Prerequisites
    -   Activate OSS service. If you haven’t activated or do not know about Alibaba Cloud OSS, log on to the OSS product page for more information.
    -   The Alibaba Cloud account AccessKey gives you permission to access all APIs, thus, we recommend that you follow Alibaba Cloud Best Practices for creating an AccessKeyId and an AccessKeySecret. If you deploy the service on the server, you can use an RAM sub-account or STS for API access or daily O&M management and control. If you deploy the service on the client, we recommend that you use STS for API access. For more information, see [Access control](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).
-   Environment requirements

    The OSS JavaScript SDK is built on the [Node.js](https://nodejs.org/) environment.


## Usage methods { .section}

The OSS JavaScript SDK supports both synchronous and asynchronous use.

-   Synchronous mode: Synchronize the asynchronous mode based on `async` and `wait`.
-   Asynchronous mode: Similar to the callback mode. The API returns the [Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise), uses the `.then()` method to process the returned result and uses the `.catch()` method to process errors.

The `new OSS()` method is used to create a client in both the synchronous mode and asynchronous mode.

Next is to name examples to upload an object first and then download the object respectively.

-   Synchronous mode

    ```
    let client = new OSS(...);
    async function put () {
      try {
        // object indicates the uploaded object name, localfile indicates a local file or file path.
        let r1 = await client.put('object','localfile'); 
        console.log('put success: %j', r1);
        let r2 = await client.get('object');
        console.log('get success: %j', r2);
      } catch(e) {
        console.error('error: %j', err);
      }
    }
    Put ();
    ```

-   Asynchronous mode

    ```language-js
    let client = new OSS(...);
    
    // object indicates the uploaded object name, localfile indicates a local file or file path.
    client.put('object', 'localfile').then(function (r1) {
      console.log('put success: %j', r1);
      return client.get('object');
    }).then(function (r2) {
      console.log('get success: %j', r2);
    }).catch(function (err) {
      console.error('error: %j', err);
    });
    
    ```


## Installation { .section}

-   Use Node.js

    Supported Node.js versions:

    Node.js later than 8.0.0 is supported. Use ali-oss 4.x in Node.js environment earlier than 8.0.0.

    Use [npm](https://www.npmjs.com/) to install the SDK:

    ```language-bash
    npm install ali-oss
    
    ```

    Use it in your project:

    ```language-js
    let OSS = require('ali-oss');
    
    let client = new OSS({
      region: '<oss region>',
      //The Alibaba Cloud account AccessKey gives you permission to access all APIs, so we recommend to follow Alibaba Cloud Best Practices for deploying the service on the server using an RAM sub-account or STS, or deploying the service on the client using STS.
      accessKeyId: '<Your accessKeyId>',
      accessKeySecret: '<Your accessKeySecret>',
      bucket: '<Your bucket name>'
    });
    
    ```

-   If you face any network problems when using npm, you can use the npm image provided by Taobao: c[npm](https://npm.taobao.org/).

