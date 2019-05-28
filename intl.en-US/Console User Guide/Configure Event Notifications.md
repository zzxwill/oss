# Configure Event Notifications {#concept_xrf_44y_5db .concept}

The Event Notification features supported by OSS, the ability to notify you of relevant actions on the OSS resource you are concerned about in a timely manner. For example:

-   New data has been uploaded from the picture content sharing platform, audio and video platform to OSS.
-   Related content on OSS has been updated.
-   The important files on the OSS are deleted.
-   The synchronization of data on OSS has been completed.

The OSS Event Notification is asynchronous and does not affect normal OSS operations. The configuration of Event Notification consists of two parts: **Rules** and **Message notification**.

-   Rules: used to describe when OSS is required for Message notification.
-   Message notification: Based on the implementation of the Ali cloud messaging service MnS, various notification methods are provided.

The OSS Event Notification overall architecture is shown in the following figure:

![](images/1523_en-US.png)

## Create method {#section_jyx_dpy_5db .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the list of left storage spaces, click the target storage space name to open the storage space overview page.
3.  Click the **Event Notification** tab, and then click **Create rule** to locate the Create rule page.

    ![](images/1525_en-US.png)

4.   In the **Rule name** box, enter the rule name.

    **Note:** 

    -   You can create up to 10 rules in the same product under the same region, the new rule takes effect in about 10 minutes.
    -   Any two rules cannot have an intersection, and any two resource descriptions cannot have an intersection.
5.  In **Resource descriptions** area, click**Add** to add one or more resource descriptions.

    **Note:** 

    -   Resource Description: can be full name, prefix, suffix, and pre-suffix, different resource descriptions cannot have intersection.
        -   Full name: Enter the full name of an object to pay precise attention to this object.
        -   Pre-Suffix: sets the pre-Suffix of the object to focus on all or part of the object in a bucket. For example, for a bucket named`nightbucket`:
            -   Pay attention to all of these files, the prefix and suffix are not filled in.
            -   Note that all the files in the directory movie, fill in the prefix with movie/, but with no suffix.
            -   Note that all of the .jpg images, leave the prefix empty and fill in the suffix with .jpg.
            -   Note that mp3 format video under the movie directory, prefix the movie/ with the suffix .mp3.
    -   The OSS resource includes the bucket and the object, which are connected by “/”. Take bucket \(movie\) and object \(hello. avi\) for example.
        -   Full name: movie/hello.avi;
        -   Prefixes and suffixes: prefix movie/, and suffix avi, it means all objects suffixes with .avi in the movie .
6.  In the **Event type** drop-down list, select one or more events that require message notification.

    **Note:** 

    -   The event type corresponds to different operations of the OSS resource, see the list of event types below for specific types and meanings.
    -   You can select multiple events that you want to trigger notifications. The same event cannot be configured multiple times on the same resource.
7.  In the **Receive terminal** area, click **Add** to add one or more receive terminals.

    **Note:** 

    -   The OSS Event Notification function generates a Message description after an action rule matches, and publish the message to the topic of the MNS, and then according to the subscription on that topic, push messages to a specific receive terminal.
    -   For how to handle HTTP push messages for MnS and consume messages from queues, see [Message Service Product Overview](https://help.aliyun.com/document_detail/27414.html).

## List of event types {#section_ols_yvy_5db .section}

|Event Type|Meaning|
|----------|-------|
|ObjectCreated:PutObject|Create/override files: simple upload|
|ObjectCreated:PostObject|Create/override files: Form upload|
|ObjectCreated:CopyObject|Create/override files: Copy files|
|ObjectCreated:InitiateMultipartUpload|Create/override file: initialize a multipart upload task|
|ObjectCreated:UploadPart|Create/override file: Upload multipart|
|Objectcreated: sealadpartcopy|Create/override file: Copy data from an existing file to upload a multipart|
|ObjectCreated:CompleteMultipartUpload|Creating/overwriting files: Completing multiparts upload|
|ObjectCreated:AppendObject|Creating/overwriting files: appending an upload|
|ObjectDownloaded:GetObject|Download files: simple download|
|ObjectRemoved:DeleteObject|Delete file: One|
|ObjectRemoved:DeleteObjects|Delete Files: Multiple|
|ObjectReplication:ObjectCreated|Synchronous files: generated|
|ObjectReplication:ObjectRemoved|Synchronizing files: Deleted|
|ObjectReplication:ObjectModified|Synchronizing files: Modified|

