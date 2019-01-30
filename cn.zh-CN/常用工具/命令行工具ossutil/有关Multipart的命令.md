# 有关Multipart的命令 {#concept_drn_rzs_vdb .concept}

ossutil提供了列举未完成的Multipart上传任务的ID，即UploadID。删除指定Object的已上传完成的文件及未上传完成的任务（UploadID）的功能。

关于Multipart Upload的更多介绍，请参见[断点续传](../../../../../cn.zh-CN/开发指南/上传文件（Object）/分片上传和断点续传.md#)。

**说明：** ossutil在上传/拷贝文件时，对于大文件自动进行断点续传，而不提供UploadPart命令。

-   列举UploadID

    使用加 -m参数的ls命令列举对应前缀的Object下的所有未完成的Multipart上传任务的ID，使用加 -a 参数的ls命令列举对应前缀的Object下所有已上传完成的文件和未完成的Multipart上传任务的ID。

    ```
    $ ossutil ls oss://bucket1/obj1 -m
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.070070(s) elapsed
    ```

    ```
    $ ossutil ls oss://bucket1/obj1 -a
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-05 14:36:21 +0000 CST        241561      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1/test.txt
    2016-04-08 14:50:47 +0000 CST       6476984      Standard    4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/obj1/sample.txt
    Object Number is: 2
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    UploadId Number is: 2
    0.091229(s) elapsed
    ```

-   删除指定Object的数据

    使用rm命令，可删除指定的Object下未完成的Multipart上传任务的ID。

    例如：bucket1下有如下文件，使用 ls命令列出已上传完成的文件和未完成的Multipart上传任务的ID。

    ```
    $ ossutil ls oss://bucket1 -a
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-05 14:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-05 14:36:21 +0000 CST        241561      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1/test.txt
    2016-04-08 14:50:47 +0000 CST       6476984      Standard    4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is: 3
    InitiatedTime                     UploadID                               ObjectName
    2017-01-13 03:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-13 03:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-13 03:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-20 11:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is: 4
    0.191289(s) elapsed
    ```

    使用加 -m 参数的 rm 命令来删除指定未完成的Multipart上传任务的ID：

    ```
    $./ossutil rm -m oss://bucket1/obj1/test.txt
    Succeed: Total 1 uploadIds. Removed 1 uploadIds. 
    0.900715(s) elapsed
    ```

    使用加-m 和-r参数的 rm 命令来删除对应前缀的所有未完成的Multipart上传任务的ID：

    ```
    $./ossutil rm -m oss://bucket1/ob -r 
    Do you really mean to remove recursively multipart uploadIds of oss:bucket1/ob(y or N)? y 
    Succeed: Total 4 uploadIds. Removed 4 uploadIds. 
    1.922915(s) elapsed
    ```

    使用加-a和 -r 参数的 rm 命令来删除对应前缀的所有已上传完成的文件和未完成的Multipart上传任务的ID：

    ```
    $./ossutil rm  oss://hello-hangzws-1/obj -a -r
    Do you really mean to remove recursively objects and multipart uploadIds of oss://obj(y or N)? y
    Succeed: Total 1 objects, 3 uploadIds. Removed 1 objects, 3 uploadIds.
    ```


