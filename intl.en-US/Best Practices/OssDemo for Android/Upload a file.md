# Upload a file {#concept_rfx_ymf_vdb .concept}

## Simple upload {#section_m35_zmf_vdb .section}

Simple upload means the Put Object interface in the OSS API is called to upload the selected files to OSS on a one-off basis.

Logic of calling

1.  Once you select the Upload option, you can select the files to be uploaded.
2.  Once the processing parameters are selected, OssDemo sends a request to the address of the sts\_server received by OssDemo.
3.  sts\_server returns AccessKeyId, AccessKeySecret, SecurityToken, and Expiration to OssDemo.
4.  Once you have received the preceding information, OssDemo calls SDK, creates an OssClient instance, and implements simple upload.

Code

1.  Generate a button control.

    ```
    Location:
     res/layout/content_main.xml
     Content:
     <Button
         style="? android:attr/buttonStyleSmall"
         android:layout_height="wrap_content"
         Android: applet_width = "wraps _ content"
         android:text="@string/upload"
         android:id="@+id/upload" />
    ```

2.  Click “Upload” and select the files to be uploaded. 

    Snippet of function implementation:

    ```
    Button upload = (Button) findViewById(R.id.upload);
     upload.setOnClickListener(new View.OnClickListener() {
         @Override
         public void onClick(View v) {
             Intent i = new Intent(
                     Intent.ACTION_PICK,
                     android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
             startActivityForResult(i, RESULT_UPLOAD_IMAGE);
         }
     }
    ```

3.  Call the upload interface of the SDK.

    Snippet of function implementation:

    ```
    @Override
     Protected void onactivityresult (INT requestcode, int overthrow code, intent data ){
         super.onActivityResult(requestCode, resultCode, data);
         if ((requestCode == RESULT_UPLOAD_IMAGE || requestCode == RESULT_PAUSEABLEUPLOAD_IMAGE) && resultCode == RESULT_OK && null ! = data) {
             Uri selectedImage = data.getData();
             String [] filepathcolumn = {mediastore. Images. Media. Data };
             Cursor cursor = getContentResolver().query(selectedImage,
                     filePathColumn, null, null, null);
             cursor.moveToFirst();
             int columnIndex = cursor.getColumnIndex(filePathColumn[0]);
             String picturePath = cursor.getString(columnIndex);
             Log.d("PickPicture", picturePath);
             cursor.close();
             try {
                 Bitmap bm = ImageDisplayer.autoResizeFromLocalFile(picturePath);
                 displayImage(bm);
                 File file = new File(picturePath);
                 displayInfo("file: " + picturePath + "\n:size" + String.valueOf(file.length()));
             }
             catch (IOException e) {
                 e.printStackTrace();
                 displayInfo(e.toString());
             }
             //Perform simple upload or resumable upload based on the specified operation.
             if (requestCode == RESULT_UPLOAD_IMAGE) {
                 final EditText editText = (EditText) findViewById(R.id.edit_text);
                 String objectname = edittext. gettext (). tostring ();
                 //Call the simple upload interface to upload the files.
                 ossService.asyncPutImage(objectName, picturePath, getPutCallback(), new ProgressCallbackFactory<PutObjectRequest>().get());
             }
         }
     }
    ```

    How to deal with the uploading results is not mentioned here. You may see onSuccess and onFailure in source code.


## Resumable upload based on multipart upload {#section_jgq_4nf_vdb .section}

Call the Multipart Upload interface of the OSS API to achieve a resumable upload effect.

Logic of calling

1.  Once you select the Upload option, you can select the files to be uploaded.
2.  Once the processing parameters are selected, OssDemo sends a request to the address of the sts\_server received by OssDemo.
3.  sts\_server returns AccessKeyId, AccessKeySecret, SecurityToken, and Expiration to OssDemo.
4.  Once you have received the preceding information, OssDemo calls SDK, creates an OssClient instance, and implements a multipart upload.
5.  If you have clicked **Pause** but the multipart upload process is still in progress, you can continue the upload remaining part, by clicking **Continue**. This can achieve the resumable upload effect.

Code

1.  Generate a button control.

    ```
    Location:
     res/layout/content_main.xml
     Content:
     <Button
         style="? android:attr/buttonStyleSmall"
         android:layout_height="wrap_content"
         android:layout_width="wrap_content"
         android:text="@string/multipart_upload"
         android:id="@+id/multipart_upload" />
    ```

2.  Click “Upload” and select the files to be uploaded. 

    Snippet of function implementation:

    ```
    Button multipart_upload = (Button) findViewById(R.id.multipart_upload);
     multipart_upload.setOnClickListener(new View.OnClickListener() {
         @Override
         public void onClick(View v) {
             //To make it simple, only one resumable upload task is running.
             Intent i = new Intent(
                     Intent.ACTION_PICK,
                     android.provider.MediaStore.Images.Media.EXTERNAL_CONTENT_URI);
             startActivityForResult(i, RESULT_PAUSEABLEUPLOAD_IMAGE);
         }
     }
     );
    ```

3.  Click “Upload” for resumable upload of the remaining part.

    Snippet of function implementation:

    ```
     Click "Upload": //The multipart upload interface of the SDK is called. task = ossService.asyncMultiPartUpload(objectName, picturePath, getMultiPartCallback().addCallback(new Runnable() { @Override public void run() { pauseTaskStatus = TASK_NONE; multipart_resume.setEnabled(false); multipart_pause.setEnabled(false); task = null; } }}, new ProgressCallbackFactory<PauseableUploadRequest>().get()); From the encapsulating logic for the SDK at the underlying layer, we can see that resumable upload is implemented by asyncUpload in the multiPartUploadManager.//During resumable upload, the returned task can be used to pause the task. public PauseableUploadTask asyncMultiPartUpload(String object, String localFile, @NonNull final OSSCompletedCallback<PauseableUploadRequest, PauseableUploadResult> userCallback, final OSSProgressCallback<PauseableUploadRequest> userProgressCallback) { if (object.equals("")) { Log.w("AsyncMultiPartUpload", "ObjectNull"); return null; } File file = new File(localFile); if (! file.exists()) { Log.w("AsyncMultiPartUpload", "FileNotExist"); Log.w("LocalFile", localFile); return null; } Log.d("MultiPartUpload", localFile); PauseableUploadTask task = multiPartUploadManager.asyncUpload(object, localFile, userCallback, userProgressCallback); return task; }
    ```


