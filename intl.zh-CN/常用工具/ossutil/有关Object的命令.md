# 有关Object的命令 {#concept_lqt_vjs_vdb .concept}

ossutil提供了上传/下载/拷贝文件、设置Object的acl、设置Object的meta、查看Object的meta信息等功能。

使用这些命令前，请使用config命令配置访问AK。

-   上传/下载/拷贝文件

    强烈建议在使用cp命令前，先使用ossutil help cp来查看帮助。

    使用cp命令进行上传/下载/拷贝文件时：

    -   使用-r选项来拷贝文件夹，对大文件默认使用分片上传并可进行断点续传（开启分片上传的大文件阈值可用`--bigfile-threshold`选项来设置）。
    -   使用-f选项来默认强制上传，当目标端存在同名文件时，不询问，直接覆盖。
    当批量上传/下载/拷贝文件时，如果某个文件出错，ossutil默认会将错误信息记录在report文件，并跳过该文件，继续其他文件的操作。更多信息请参见ossutil help cp。

    **说明：** 当出现Bucket不存在、AccessKeyID/AccessKeySecret错误造成的权限验证非法等错误时，不再继续拷贝其他文件。

    ossutil支持特定场景下的增量上传策略。`--update`和`--snapshot-path`选项，请参见ossutil help cp。

    ossutil从1.0.0.Beta1版本开始，上传文件默认打开crc64。

    -   上传单个文件：

        ```
        $./ossutil cp a oss://ossutil-test
        Succeed: Total num: 1, size: 230. OK num: 1(upload 1 files).
        0.699795(s) elapsed
        ```

    -   上传文件夹：

        ```
        $./ossutil cp -r dir oss://ossutil-test
        Succeed: Total num: 35, size: 464,606. OK num: 35(upload 34 files, 1 directories).
        0.896320(s) elapsed
        ```

    ossutil从1.4.2版本开始，支持对开通了[请求者付费模式](../../../../intl.zh-CN/控制台用户指南/管理存储空间/设置请求者付费模式.md#)的Bucket内文件进行上传、下载和拷贝的操作。

    -   上传文件到开通了请求者付费模式的Bucket：

        ```
        ./ossutil cp ~/Downloads/test.mp4 oss://payer/ --payer=requester
        ```

    -   从开通了请求者付费模式的Bucket下载文件：

        ```
        ./ossutil cp oss://payer/test.mp4 ~/Downloads/ --payer=requester
        ```

    -   从开通了请求者付费模式的Bucket拷贝文件到普通Bucket：

        ```
        ./ossutil cp oss://payer/test.mp4 oss://hello-hangzws/  --payer=requester
        ```

    -   从普通Bucket拷贝数据到开通了请求者付费模式的Bucket：

        ```
        ./ossutil cp oss://hello-hangzws/test.mp4 oss://payer/ --payer=requester
        ```

-   修改文件存储类型

    **说明：** 修改5GB以下文件的存储类型，可使用set-meta命令；如果想修改超过5GB以上文件的存储类型，必须使用cp命令。

    -   通过set-meta命令修改文件存储类型。
        -   设置单个文件的存储类型为 IA 类型：

            ```
            ./ossutil set-meta oss://hello-hangzws/0104_6.jpg X-Oss-Storage-Class:IA -u
            ```

        -   设置文件目录的所有文件存储类型为 Standard 类型：

            ```
            ./ossutil set-meta oss://hello-hangzws/abc/ X-Oss-Storage-Class:Standard -r -u
            ```

    -   通过 cp 命令上传文件，并通过`--meta`选项修改对象存储类型。

        -   上传单个文件时，指定存储类型为 IA 类型：

            ```
            ossutil cp ~/Downloads/sys.log  oss://hello-hangzws/test/ --meta X-oss-Storage-Class:IA
            ```

        -   上传文件目录，指定存储类型为 IA 类型：

            ```
            ./ossutil cp ~/libs3/ oss://hello-hangzws/test/ --meta X-oss-Storage-Class:IA -r
            ```

        -   修改已有文件的存储类型为 Archive 类型：

            ```
            ./ossutil cp oss://hello-hangzws/0104_6.jpg oss://hello-hangzws/0104_6.jpg --meta X-oss-Storage-Class:Archive
            ```

        -   修改已有文件目录下所有文件的文件类型为 Standard 类型：

            ```
            ./ossutil cp oss://hello-hangzws/test/ oss://hello-hangzws/test/ --meta X-oss-Storage-Class:Standard -r
            ```

        **说明：** 

        -   存储类型为归档类型的文件不能通过set-meta或者cp命令直接转换成其他类型，必须先通过restore命令将该文件转换成低频存储，之后再使用set-meta或者cp命令转换文件类型。
        -   使用cp覆写文件时涉及到数据覆盖操作。如果**低频型**或**归档型**文件分别在创建后 30 和 60 天内被覆盖，则它们会产生提前删除费用。比如，低频型文件创建10天后，被覆写修改成归档型或标准型，则会产生20天的提前删除费用。
-   上传/下载/拷贝文件的性能调优

    在cp命令中，通过`--jobs`项和`--parallel`项控制并发数。当ossutil自行设置的默认并发达不到用户的性能需求时，您可以自行调整该两个选项来升降性能。

    -   `--jobs`项控制多个文件上传/下载/拷贝时，文件间启动的并发数。
    -   `--parallel`制分片上传/下载/拷贝一个大文件时，每一个大文件启动的并发数。
    默认情况下，ossutil会根据文件大小来计算parallel个数（该选项对于小文件不起作用，进行分片上传/下载/拷贝的大文件文件阈值可由`--bigfile-threshold`选项来控制）。当进行批量大文件的上传/下载/拷贝时，实际的并发数为jobs个数乘以parallel个数。

    **警告：** 

    -   通常情况下，当ECS虚拟机或者服务器在网络、内存、CPU等资源不是特别大的情况下，建议将并发数调整到100以下。如果网络、内存、CPU等资源没有占满，可以适当增加并发数。
    -   如果并发数调得太大，由于线程间资源切换及抢夺等，ossutil上传/下载/拷贝性能可能会下降。并发数过大可能会产生EOF错误。所以请根据实际的机器情况调整`--jobs`和`--parallel`选项的数值。如果要进行压测，可在一开始时调低这两项数值，然后逐渐调大直至找到最优值。
-   设置Object的acl

    ossutil使用set-acl命令设置Object的acl，使用-r选项可以批量设置Object的acl。更多信息请参见ossutil help set-acl。

    ```
    $./ossutil set-acl oss://dest/a private
    0.074507(s) elapsed
    ```

    批量设置acl：

    ```
    $./ossutil set-acl oss://dest/a private -r
    Do you really mean to recursivlly set acl on objects of oss://dest/a(y or N)? y
    Succeed: Total 3 objects. Setted acl on 3 objects.
    0.963934(s) elapsed
    ```

-   设置Object的meta

    ossutil使用set-meta命令设置Object的meta信息，使用-r选项可以批量设置Object的meta。更多信息请参见ossutil help set-meta。

    ```
    ./ossutil set-meta oss://dest/a x-oss-object-acl:private -u
    ```

    **说明：** 您可以在上传文件的同时设置Object的meta信息，详情请参见[ossutil的过滤参数include/exclude](https://yq.aliyun.com/articles/600175)。

-   查看Object描述信息（meta）

    ossutil使用stat命令查看Object的描述（meta）信息。更多信息请参见ossutil help stat。

    ```
    $./ossutil stat oss://dest/a 
    ACL                         : default
    Accept-Ranges               : bytes
    Content-Length              : 230
    Content-Md5                 : +5vbQC/MSQK0xXSiyKBZog==
    Content-Type                : application/octet-stream
    Etag                        : FB9BDB402FCC4902B4C574A2C8A059A2
    Last-Modified               : 2017-01-13 15:14:22 +0800 CST
    Owner                       : aliyun
    X-Oss-Hash-Crc64ecma        : 12488808046134286088
    X-Oss-Object-Type           : Normal
    0.125417(s) elapsed
    ```

-   恢复冷冻状态的Object为可读状态

    ossutil使用restore命令恢复冷冻状态的Object为可读状态。使用`-r`选项批量恢复冷冻状态的Object为可读状态。更多信息请参见ossutil help restore。

    ```
    $./ossutil restore oss://utiltest/a
    0.037729(s) elapsed
    ```

-   创建符号链接

    ossutil使用create-symlink创建符号链接。更多信息请参见ossutil help create-symlink。

    ```
    $./ossutil create-symlink oss://utiltest/b a
    0.037729(s) elapsed
    ```

-   读取符号链接文件的描述信息

    ossutil使用read-symlink读取符号链接文件的描述信息。更多信息请参见ossutil help read-symlink。

    ```
    $./ossutil read-symlink oss://utiltest/b
    Etag                    : D7257B62AA6A26D66686391037B7D61A
    Last-Modified           : 2017-04-26 15:34:27 +0800 CST
    X-Oss-Symlink-Target    : a
    0.112494(s) elapsed
    ```


