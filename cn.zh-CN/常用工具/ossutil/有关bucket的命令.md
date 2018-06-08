# 有关bucket的命令 {#concept_rjp_vgs_vdb .concept}

## Bucket的相关命令 {#section_ejm_1hs_vdb .section}

ossutil提供了创建、删除、列举Bucket、以及为Bucket设置acl的功能，关于Bucket更多的管理功能暂时不支持，如需要请使用[osscmd](cn.zh-CN/常用工具/osscmd/快速安装.md#)。

在使用这些命令前，请先使用config命令配置访问AK。

-   创建Bucket

    ```
    ossutil mb oss://bucket [--acl=acl] [--storage-class sc] [-c file]
    ```

    其中acl如果不指定，则默认为private权限，如果成功创建，ossutil会打印消耗时间并退出，否则会输出错误信息。可以通过`--storage-class`选项指定存储方式。

    关于创建Bucket的帮助信息，请使用ossutil help mb命令查看。

    ```
    $./ossutil mb oss://test
    0.220478(s) elapsed
    ```

-   删除Bucket

    关于删除Bucket的帮助信息，请使用ossutil help rm命令查看。注意：

    -   删除bucket必须设置-b选项；
    -   被删除的bucket可能被其他用户重新创建，从而不再属于您；
    -   bucket中的数据一旦被删除则无法恢复。
    （1）如果您的Bucket中没有数据

    ```
    ossutil rm oss://bucket -b
    ```

    ```
    $./ossutil rm oss://test -b
    Do you really mean to remove the Bucket: test(y or N)? y
    0.220478(s) elapsed
    ```

    （2\) 如果您的Bucket中有object或multipart等数据，需要先删除所有数据再删除Bucket，可以使用以下命令来一并删除所有数据和您的Bucket

    ```
    ossutil rm oss://bucket -bar
    ```

    关于删除Bucket的帮助信息，请使用ossutil help rm命令查看。

-   列举Buckets

    `./ossutil ls`或 `./ossutil ls oss://`

    可以使用-s选项来显示精简格式，更多帮助见: ossutil help ls

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

-   列举Bucket中的文件

    ossutil可以列举Bucket中的Object和UploadID，默认情况下显示Object，使用-m选项来显示UploadID，使用-a选项同时显示Object和UploadID。

    -   列举Object

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

    -   列举Object和Multipart

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

        可以使用-s选项显示精简模式。可以使用-d选项显示首层目录下的内容。

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

-   为Bucket设置acl

    创建Bucket时，Bucket默认的acl为private，可以通过set-acl命令来修改Bucket的acl。在设置Bucket的acl权限时，需要设置-b选项。

    将bucket1设置为private权限：

    ```
    ./ossutil set-acl oss://bucket1 private -b
    ```

    关于设置acl的更多信息请使用help set set-acl来查看。


