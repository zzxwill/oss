# Introduction {#concept_eg1_jmx_wdb .concept}

In addition to PutObject, OSS also provides the multipart upload mode. You can upload files in the multipart upload mode in the following scenarios \(but not limited to the following\):

-   Resumable upload must be supported.
-   The files to be uploaded are larger than 100 MB.
-   The network conditions are poor, and the connection with the OSS server is frequently disconnected.
-   Before a file is uploaded, the size of the file cannot be determined.

