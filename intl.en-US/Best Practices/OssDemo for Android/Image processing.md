# Image processing {#concept_nhk_d4f_vdb .concept}

Image processing means that once the image is uploaded and displayed on the OssDemo, it is processed.  The differences between image processing and downloading an image are that:

-   The endpoint for processing an image is used.
-   Some processing parameters are added under object.

## Watermark an image {#section_yrb_f4f_vdb .section}

Logic of calling

1.  Upload an image to OSS. By default, the bucket is sdk-demo, the object is test, and the endpoint of the OSS is `oss-cn-hangzhou.aliyuncs.com`.
2.  Based on the image processing method, processing parameters are added under test for the required effect.
3.  Once these processing parameters are selected, OssDemo sends a request to the address of the sts\_server received by OssDemo.
4.  sts\_server returns AccessKeyId, AccessKeySecret, SecurityToken, and Expiration to OssDemo.
5.  Once you have received all this information, OssDemo calls SDK, creates an OssClient instance, and downloads the image.  The displayed effect is the effect produced after image processing.  However, the endpoint for image service is `img-cn-hangzhou.aliyuncs.com`.

Code

1.  Click **More** and the page showing the processed image is displayed.
2.  Add a watermark with a size of 100 at the lower-right corner of the previously uploaded image and get such an operating command. 

    Snippet of function implementation:

    ```
     In the ImageService class, 
     A method is provided to add the parameters necessary for a function to the object.
      //Add a text watermark to the image. All parameters other than font size are default values. You can modify the parameter values when necessary according to the image service documentation.
     public String textWatermark(String object, String text, int size) {
         String base64Text = Base64.encodeToString(text.getBytes(), Base64. URL_SAFE | Base64. NO_WRAP);
         String queryString = "@watermark=2&type=" + font + "&text=" + base64Text + "&size=" + String.valueOf(size);
         Log.d("TextWatermark", object);
         Log.d("Text", text);
         Log.d("QuerySyring", queryString);
         return (object + queryString);
     
    ```

3.  Call the SDK download interface to process the image.

    Snippet of function implementation:

    ```
    getImage(imageService.textWatermark(objectName, "OSS test", 100), 0, "text watermark at lower-right corner, size: 100");
     public void getImage(final String object, final Integer index, final String method) {
         GetObjectRequest get = new GetObjectRequest(bucket, object);
         Log.d("Object", object);
         OSSAsyncTask task = oss.asyncGetObejct(get, new UICallback<GetObjectRequest, GetObjectResult>(uiDispatcher) {
             @Override
             public void onSuccess(GetObjectRequest request, GetObjectResult result) {
                 // Request succeeded
                 InputStream inputStream = result.getObjectContent();
                 Log.d("GetImage", object);
                 Log.d("Index", String.valueOf(index));
                 try {
                     //Do not exceed the maximum display number.
                     adapter.getImgMap().put(index, new ImageDisplayer(1000, 1000).autoResizeFromStream(inputStream));
                     adapter.getTextMap().put(index, method + "\n" + object);
                     //Perform auto scaling based on the size of the corresponding view.
                     addCallback(new Runnable() {
                         @Override
                         public void run() {
                             adapter.notifyDataSetChanged();
                         
                     }, null);
                 
                 catch (IOException e) {
                     e.printStackTrace();
                 
                 super.onSuccess(request,result);
             
    ```

    How to deal with the failure in downloading the results is not mentioned in this document. You can see onFailure in source code, for more information. 


## Scaling, cropping, and rotating an image {#section_ctf_r4f_vdb .section}

This is similar to the watermarking process. In the ImageService, add a function for getting the processing commands. Add the processing parameters under the object. Finally, call the Get Object interface of the SDK to process the image.

```
//Scaling
getImage(imageService.resize(objectName, 100, 100), 1, "scale to 100*100");
//Cropping
getImage(imageService.crop(objectName, 100, 100, 9), 2, "crop the lower-right corner by 100*100");
//Rotating
getImage(imageService.rotate(objectName, 90), 3, "rotate by 90 degree");
```

