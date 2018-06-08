# Introduction {#concept_eg1_jmx_wdb .concept}

In addition to the PUT Object interface, OSS also provides the Multipart. You can apply the Multipart Upload mode for you to upload files. You can apply the Multipart Upload mode in the following scenarios \(but not limited to the following\):

-   Breakpoint upload must be supported.
-   The files to be uploaded are larger than 100 MB.
-   The network conditions are poor, and the connection with the OSS server is frequently disconnected.
-   Before a file is uploaded, the size of the file cannot be determined.

