# Multipart-related  {#concept_drn_rzs_vdb .concept}

## commands {#section_spb_xzs_vdb .section}

Ossutil allows you to list an UploadID and delete all UploadIDs of the specified object.

For more information about the multipart, see [Multipart upload](../intl.en-US/Developer Guide/Upload files/Multipart upload.md#).

**Note:** When uploading/copying a large file, ossutil automatically implements multipart upload and resumable data transfer, without running the UploadPart command.

-   List an UploadID

    Use the -m option to list all incomplete UploadIDs of the specified object, and use the -a option to list objects and UploadIDs.

    ```
    $ ossutil ls oss://bucket1/obj1 -m
    InitiatedTime UploadID ObjectName
    2017-01-13 03:45:26 + 0000 CST glasoss: // bucket1/obj1
    2017-01-13 03:43:13 +0000 CST 2A1F9B4A95E341BD9285CC42BB950EE0 oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

-   Delete all UploadIDs of the specified object

    Use the -m option to delete all incomplete UploadIDs of the specified object. If the -r option is specified simultaneously, incomplete UploadIDs of all objects that use the specified object as the prefix are deleted.

    Assume that bucket1 contains the following objects:

    ```
    $ ossutil ls oss://bucket1 -a
    LastModifiedTime Size(B) StorageClass ETAG ObjectName
    2015-06-05 14:06:29 +0000 CST 201933 Standard 7E2F4A7F1AC9D2F0996E8332D5EA5B41 oss://bucket1/dir1/obj11
    2015-06-05 14:36:21 +0000 CST 241561 Standard 6185CA2E8EB8510A61B3A845EAFE4174 oss://bucket1/obj1
    2016-04-08 14:50:47 +0000 CST 6476984 Standard 4F16FDAE7AC404CEC8B727FCC67779D6 oss://bucket1/sample.txt
    Object Number is: 3
    InitiatedTime UploadID ObjectName
    2017-01-13 03:45:26 +0000 CST 15754AF7980C4DFB8193F190837520BB oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST 2A1F9B4A95E341BD9285CC42BB950EE0 oss://bucket1/obj1
    2017-01-13 03:45:25 +0000 CST 3998971ACAF94AD9AC48EAC1988BE863 oss://bucket1/obj2
    2017-01-20 11:16:21 +0800 CST A20157A7B2FEC4670626DAE0F4C0073C oss://bucket1/tobj
    UploadId Number is: 4
    0.191289(s) elapsed
    ```

    Delete the two UploadIDs of obj1:

    ```
    $./ossutil rm -m oss://bucket1/obj1
    Succeed: Total 2 uploadIds. Removed 2 uploadIds.
    1.922915(s) elapsed
    ```

    Delete the three UploadIDs of obj1 and obj2:

    ```
    $./ossutil rm -m oss://bucket1/ob
    Succeed: Total 4 uploadIds. Removed 4 uploadIds.
    1.922915(s) elapsed
    ```

    Delete obj1 and the three UploadIDs of obj1 and obj2 simultaneously:

    ```
    $./ossutil rm oss://dest1/.a -a -r -f
    Do you really mean to remove recursively objects and multipart uploadIds of oss://dest1/.a(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


