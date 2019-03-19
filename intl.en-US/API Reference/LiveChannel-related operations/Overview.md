# Overview {#concept_njb_pbd_xdb .concept}

You can upload audio and video data to OSS through the RTMP protocol and store the data as audio and video files in specified formats. Before uploading audio and video data, you must create a LiveChannel to obtain the URL used to push video or audio streams. For more information, see the documents of APIs related to LiveChannel.

When uploading audio and video data to OSS through the RTMP protocol, you must pay attention to the following limits:

-   By using the RTMP protocol, you can only push video or audio streams but not pull the streams.
-   A LiveChannel must include a video stream in H264 format.
-   Audio streams are optional in a LiveChannel. Only audio streams in the AAC format are supported. Audio streams in other formats are discarded.
-   Only the HLS protocol is supported to store the uploaded video and audio data as files in specified formats.
-   Only one client can push streams to a LiveChannel at the same time.

