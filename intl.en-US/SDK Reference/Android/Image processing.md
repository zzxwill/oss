# Image processing {#concept_qrm_ycn_mfb .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service.

After source images are uploaded to OSS, you can process the images on any Internet device at any anytime, from anywhere through simple `RESTful` APIs. You can establish your own image processing services based on the image processing service provided by OSS.

## Features {#section_rjj_vlp_wfb .section}

For more information about the features of OSS image processing service, see [OSS image processing](../../../../../reseller.en-US/Data Processing/Image Processing/Image processing.md#).

 [OSS image processing service](https://help.aliyun.com/document_detail/44686.html) provides the following features:

-   [Resize images](../../../../../reseller.en-US/Data Processing/Image Processing/Resize images.md#)
-   [Crop images](../../../../../reseller.en-US/Data Processing/Image Processing/Crop images/Incircle.md#)
-   [Rotate images](../../../../../reseller.en-US/Data Processing/Image Processing/Rotate images/Adaptive orientation.md#)
-   [Apply effects](../../../../../reseller.en-US/Data Processing/Image Processing/Apply effects/Brightness.md#)
-   [Convert format](../../../../../reseller.en-US/Data Processing/Image Processing/Convert formats/Format conversion.md#)
-   [Add watermarks](../../../../../reseller.en-US/Data Processing/Image Processing/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.

## Usage {#section_rtc_xlp_wfb .section}

-   Anonymous access

    ```
    String url = oss.presignPublicObjectURL(testBucket, testObject);
    		OSSLog.logDebug("signPublicURL", "get url: " + url);
    Add the x-oss-process:operation parameter into the generated URL, in which operation indicates the operation performed on the image.
    ```

-   Authorized access

    When processing images by using OSS Android SDK, call the `request.setxOssProcess()` method to configure image processing parameters, as shown in the following code:

    ```
    // Construct the image download request.
    GetObjectRequest get = new GetObjectRequest("<bucketName>", "example.jpg");
    
    // Process the image.
    request.setxOssProcess("image/resize,m_fixed,w_100,h_100");
    
    OSSAsyncTask task = oss.asyncGetObject(get, new OSSCompletedCallback<GetObjectRequest, GetObjectResult>() {
        @Override
        public void onSuccess(GetObjectRequest request, GetObjectResult result) {
            // The request is successful.
            InputStream inputStream = result.getObjectContent();
    
            byte[] buffer = new byte[2048];
            int len;
    
            try {
                while ((len = inputStream.read(buffer)) != -1) {
                    // Process the downloaded data.
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    
        @Override
        public void onFailure(GetObjectRequest request, ClientException clientExcepion, ServiceException serviceException) {
            // Handle the exceptions. You can add your own code.
        }
    });
    ```

    **Note:** To perform other operations on the image, replace the parameters of `request.setxOssProcess()`.

-   Access through SDK

    ```
    GetObjectRequest request = new GetObjectRequest("bucket-name", "image-name");
    request.setxOssProcess("image/resize,m_lfit,w_100,h_100");  // Configure image processing parameters.
    OSSAsyncTask task = ossClient.asyncGetObject(request, getCallback);
    ```


## Save image processing result {#section_omx_ylp_wfb .section}

Run the following code to save the image processing results:

```
OSSImagePersistRequest *request = [OSSImagePersistRequest new];
request.fromBucket = _privateBucketName;
request.fromObject = OSS_IMAGE_KEY;
request.toBucket = _privateBucketName;
request.toObject = @"image_persist_key"; 
// Scale the image.  
request.action = @"image/resize,w_100"; 

[[[ossClient imageActionPersist:request] continueWithBlock:^id _Nullable(OSSTask * _Nonnull task) {
    
    return nil;
}] waitUntilFinished];
```

