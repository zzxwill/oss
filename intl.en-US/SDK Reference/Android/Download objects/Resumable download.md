# Resumable download {#concept_tmw_n3b_3gb .concept}

In a resumable download, if the download process is interrupted, it can be restarted from where it stopped, which saves time and traffic.

For example, when you download a video using an application on your mobile phone, the download process is interrupted by default if the network mode of the mobile phone switches from Wi-Fi networks to the cellular data network. When the network mode switches back to Wi-Fi networks, you can manually restart the download process from where it stopped, that is, perform a resumable download.

## Detail analysis {#section_krl_yrc_3gb .section}

The If-Range HTTP request header field specifies the condition that needs to be met to make the Range header field functional.

-   The Range header field takes effect only when the condition included in the If-Range header field is met. In this case, the OSS server returns the 206 Partial Content code and the content specified by the Range header field.
-   If the condition included in the If-Range header field is not met, the OSS server returns a 200 OK status code and the complete resources that are requested.
-   Both the Last-Modified and ETag values can be used for verification. However, the two values cannot be used at the same time.

The If-Range header field is used in resumable downloads to verify whether the resources downloaded in the last paused download are changed.

When the `If-Range` header field is used, the client only needs to initiate one request. However, when `If-Unmodified-Since` or `If-Match` is used, if the specified condition is not met, a 412 pre-condition check failure status code is returned, and the client must initiate another request to obtain the resources.

## Implementation {#section_v1w_zrc_3gb .section}

For more information about how to perform resumable downloads on OSS objects in Android systems, see the following implementation code in iOS. If you want to perform resumable downloads, you can implement the method by yourself or download open source third-party frameworks.

You can run the following code to perform resumable downloads on OSS objects in iOS.

```
//1. Use SDK to get the download URL of the object.
String signedURLString = ossClient.presignConstrainedObjectURL(bucket, object, expires);

//2. Add a download task.

        mDownloadManager = DownloadManager.getInstance();
        mDownloadManager.add(signedURLString, new DownloadListner() {
            @Override
            public void onFinished() {
                Toast.makeText(MainActivity.this, "Download complete!", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onProgress(float progress) {
                pb_progress1.setProgress((int) (progress * 100));
                tv_progress1.setText(String.format("%. 2f", progress * 100) + "%");
            }

            @Override
            public void onPause() {
                Toast.makeText(MainActivity.this, "Download paused!", Toast.LENGTH_SHORT).show();
            }

            @Override
            public void onCancel() {
                tv_progress1.setText("0%");
                pb_progress1.setProgress(0);
                btn_download1.setText("Download");
                Toast.makeText(MainActivity.this, "Download canceled!", Toast.LENGTH_SHORT).show();
            }
        });
        
//3. Start the download.
        mDownloadManager.download(signedURLString);
        
//4. Pause the download.
        mDownloadManager.cancel(signedURLString);
        
//5. Continue to download.
        mDownloadManager.download(signedURLString);
```

**Note:** Currently, OSS servers do not support the `If-Range` field. Therefore, you cannot use `NSURLSessionDownloadTask` to perform resumable downloads in Android systems. When `NSURLSessionDownloadTask` is used to perform resumable downloads, the `If-Range` header field is sent to the OSS server to verify whether the downloaded object is changed.

