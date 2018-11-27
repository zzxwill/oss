# Bucket-related commands {#concept_rjp_vgs_vdb .concept}

Ossutil allows you to create, list, and delete buckets, and set the ACL for a bucket. Other bucket management functions are not supported currently. If you want to use these functions, see [osscmd](intl.en-US/Utilities/osscmd/Quick installation.md#).

Before running these commands, run the `config` command to configure the AccessKey pair.

-   Create a bucket

    ```
    ossutil mb oss://bucket [--acl=acl] [--storage-class sc] [-c file]
    ```

    If the ACL is not specified, the bucket has the private permission by default. After a bucket is created, ossutil prints the consumed time and exits. Otherwise, ossutil outputs error information. You can use the `--storage-class` option to specify the storage mode.

    Run `ossutil help mb` to view help information about creating a bucket.

    ```
    $./ossutil mb oss://test
    0.220478(s) elapsed
    ```

-   Delete a bucket

    Before you delete a bucket, note the following:

    -   The -b option must be specified for deleting a bucket.
    -   The deleted bucket will not longer belong to you and may be re-created by another user.
    -   Once deleted, data in the bucket cannot be recovered.
    -   If no data is stored in your bucket, run the following command to delete the bucket:

        ```
        ossutil rm oss://bucket -b
        ```

        ```
        $./ossutil rm oss://test -b
        Do you really mean to remove the Bucket: test(y or N)? y
        0.220478(s) elapsed
        ```

    -   If your bucket contains objects or multipart data, delete all data before deleting the bucket. You can run the following command to delete all data and your bucket:

        ```
        ossutil rm oss://bucket -bar
        ```

        Run `ossutil help rm` to view help information about deleting a bucket.

-   List buckets

    `./ossutil ls` or  `./ossutil ls oss://`

    Use the `-s` option to display the short format. Run `ossutil help ls` to view more help information. 

    ```
    $./ossutil ls
    CreationTime                                 Region    StorageClass    BucketName
    2016-10-21 16:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://go-sdk-test-bucket-xyz-for-object
    2016-12-01 15:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutil-test
    2016-07-18 17:54:49 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutilconfig
    2016-07-20 10:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://ossutilupdate
    2016-11-14 13:08:36 +0800 CST       oss-cn-hangzhou              IA    oss://yyyyy
    2016-08-25 09:06:10 +0800 CST       oss-cn-hangzhou         Archive    oss://ztzt
    2016-11-21 21:18:39 +0800 CST       oss-cn-hangzhou         Archive    oss://ztztzt
    Bucket Number is: 7
    0.252174(s) elapsed
    ```

-   List files in a bucket

    Ossutil can list the objects and UploadIDs in a bucket. The objects are displayed by default. You can use the `-m` option to display the UploadIDs and use the `-a` option to display the objects and UploadIDs simultaneously.

    -   List the objects

        ```
        ./ossutil ls oss://bucket
        ```

        ```
        $./ossutil ls oss://ossutil-test
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
        2016-12-01 15:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a1
        2016-12-01 15:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a2
        2016-12-01 15:06:45 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a3
        Object Number is: 3
        0.007379(s) elapsed
        ```

    -   List the objects and multiparts

        ```
        ./ossutil ls oss://bucket -a
        ```

        ```
        $ ossutil ls oss://bucket1 -a
        LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
        2015-06-05 14:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
        2015-06-05 14:36:21 +0000 CST        201933      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1
        2016-04-08 14:50:47 +0000 CST       6476984      Standard   4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
        Object Number is: 3
        InitiatedTime                     UploadID                           ObjectName
        2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
        2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
        2017-01-13 03:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
        2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
        UploadId Number is: 4
        0.191289(s) elapsed
        ```

        Use the `-s` option to display the short format.

        Use the `-d` option to display content in the level 1 directory.

        ```
        $ ossutil ls oss://bucket1 -d
        oss://bucket1/obj1
        oss://bucket1/sample.txt
        oss://bucket1/dir1/
        Object and Directory Number is: 3
        UploadID                            ObjectName
        15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
        2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
        3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
        A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
        UploadId Number is: 4
        0.119884(s) elapsed
        ```

-   Set the ACL for a bucket

    When a bucket is created, the default ACL for the bucket is private. You can run the `set-acl` command to modify the ACL for a bucket. You must specify the`-b`option when setting the ACL for a bucket.

    Grant the private permission for bucket1:

    ```
    ./ossutil set-acl oss://bucket1 private -b
    ```

    Run the help set `set-acl` command to view more information about setting the ACL.


