# Multipart download {#concept_zjn_31c_5db .concept}

OSS provides a "start object download from specified point" function. This allows users to spilt large objects into multiple downloads, which improves speed and reliability of downloads. If a download is paused or interrupted, it resumes at the point of interruption once restarted.

Similar to a simple upload, the user must have read permission for the object. Multipart downloads are supported when the Range parameter is set. If the Range parameter is specified in the request header, the returned message contains the length of the entire file and the range returned in this response. For example, Content-Range: bytes 0-9/44 indicates that the length of the entire file is 44, and the range in the response body is 0–9. If the range requirement is not met, the system transfers the entire file and does not include Content-Range in the result. The return code is 206.

## Reference {#section_t14_k1c_5db .section}

-   API：[Get Object](../intl.en-US/API Reference/Object operations/GetObject.md#)
-    

## Additional links {#section_gyr_l1c_5db .section}

-   [File Upload Methods](intl.en-US/Developer Guide/Upload files/Simple upload.md#)
-   [Upload Callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [Mobile Client Development and Download Scenario Introduction](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Cloud Processing for Uploaded Images](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud Processing after uploading audio and video files](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Secure Download Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized Third-party Download for Download Security](intl.en-US/Developer Guide/Download Files/Authorized third-party download.md#)
-   [Copying, Deleting, and Managing Uploaded Files](intl.en-US/Developer Guide/Managing Objects/Object Meta.md#)

