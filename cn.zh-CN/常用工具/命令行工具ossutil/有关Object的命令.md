# 有关Object的命令 {#concept_lqt_vjs_vdb .concept}

ossutil提供了上传/下载/拷贝文件和文件夹、设置文件读写权限ACL、设置和查看文件元信息（object meta）等功能。

使用这些命令前，请使用config命令配置访问AK。

## 上传/下载/拷贝文件 {#section_qb3_y2y_bgb .section}

强烈建议在使用cp命令前，先使用ossutil help cp来查看帮助。

使用cp命令进行上传/下载/拷贝文件时：

-   使用-r选项来拷贝文件夹，对大文件默认使用分片上传并可进行断点续传（开启分片上传的大文件阈值可用`--bigfile-threshold`选项来设置）。
-   使用-f选项来默认强制上传，当目标端存在同名文件时，不询问，直接覆盖。

当批量上传/下载/拷贝文件时，如果某个文件出错，ossutil默认会将错误信息记录在report文件，并跳过该文件，继续其他文件的操作。更多信息请参见ossutil help cp。

**说明：** 当出现Bucket不存在、AccessKeyID/AccessKeySecret错误造成的权限验证非法等错误时，不再继续拷贝其他文件。

-   上传文件

    ossutil支持特定场景下的增量上传策略。`--update`和`--snapshot-path`选项，请参见ossutil help cp。

    ossutil从1.0.0.Beta1版本开始，上传文件默认打开crc64。

    -   上传单个文件：

        ```
        $./ossutil cp a oss://my-bucket/path
        ```

    -   上传文件夹：

        ```
        $./ossutil cp -r dir oss://ossutil-test
        Succeed: Total num: 35, size: 464,606. OK num: 35(upload 34 files, 1 directories).
        0.896320(s) elapsed
        ```

-   下载文件
    -   下载单个文件：

        ```
        $./ossutil cp oss://my-bucket/path/test1.txt /dir
        ```

        下载文件时，可以通过--range选项进行指定范围下载。

        示例如下，将test1.txt的第10到第20个字符作为一个文件载到本地。

        ```
        $./ossutil cp oss://my-bucket/path/test1.txt /dir  --range=10-20
        Succeed: Total num: 1, size: 11. OK num: 1(download 1 objects).
        0.290769(s) elapsed
        ```

    -   下载文件夹：

        ```
        $./ossutil  cp -r oss://my-bucket/path  /dir 
        ```

        下载文件夹时，可以通过--update选项进行增量下载。

        示例如下，本地/dir目录下已存在path目录中的部分文件，并且这些文件的更新时间不晚于testdir目录中的文件，下载时则跳过这些文件。

        ```
        $./ossutil cp -r oss://my-bucket/path  /dir  --update
        Succeed: Total num: 9, size: 144,097. OK num: 9(download 4 objects, skip 5 objects), Skip size: 30,310.
        ```


ossutil从1.4.2版本开始，支持对开通了[请求者付费模式](../../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置请求者付费模式.md#)的Bucket内文件进行上传、下载和拷贝的操作。

-   上传文件到开通了请求者付费模式的Bucket：

    ```
    $./ossutil cp dir/test.mp4 oss://payer/ --payer=requester
    ```

-   从开通了请求者付费模式的Bucket下载文件：

    ```
    $./ossutil cp oss://payer/test.mp4 dir/ --payer=requester
    ```

-   从开通了请求者付费模式的Bucket拷贝文件到普通Bucket：

    ```
    $./ossutil cp oss://payer/test.mp4 oss://my-bucket/path  --payer=requester
    ```

-   从普通Bucket拷贝数据到开通了请求者付费模式的Bucket：

    ```
    $./ossutil cp oss://my-bucket/path/test.mp4 oss://payer --payer=requester
    ```


## 修改文件存储类型 {#section_cnl_x2y_bgb .section}

修改5GB以下文件的存储类型，可使用set-meta命令；如果想修改5GB以上文件的存储类型，必须使用cp命令。

-   通过set-meta命令修改文件存储类型。
    -   设置单个文件的存储类型为 IA 类型：

        ```
        $./ossutil set-meta oss://my-bucket/path/0104_6.jpg X-Oss-Storage-Class:IA -u
        ```

    -   设置文件目录的所有文件存储类型为 Standard 类型：

        ```
        $./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard -r -u
        ```

-   通过 cp 命令上传文件，并通过`--meta`选项修改对象存储类型。

    -   上传单个文件时，指定存储类型为 IA 类型：

        ```
        $./ossutil cp dir/sys.log  oss://my-bucket/path --meta X-oss-Storage-Class:IA
        ```

    -   上传文件目录，指定存储类型为 IA 类型：

        ```
        $./ossutil cp dir/ oss://my-bucket/path --meta X-oss-Storage-Class:IA -r
        ```

    -   修改已有文件的存储类型为 Archive 类型：

        ```
        $./ossutil cp oss://my-bucket/path/0104_6.jpg oss://my-bucket/path/0104_6.jpg --meta X-oss-Storage-Class:Archive
        ```

    -   修改已有文件目录下所有文件的文件类型为 Standard 类型：

        ```
        $./ossutil cp oss://my-bucket/path/ oss://my-bucket/path/ --meta X-oss-Storage-Class:Standard -r
        ```

    **说明：** 

    -   存储类型为归档类型的文件不能通过set-meta或者cp命令直接转换成其他类型，必须先通过restore命令将该文件转换成低频存储，之后再使用set-meta或者cp命令转换文件类型。
    -   使用cp覆写文件时涉及到数据覆盖操作。如果**低频型**或**归档型**文件分别在创建后 30 和 60 天内被覆盖，则它们会产生提前删除费用。比如，低频型文件创建10天后，被覆写修改成归档型或标准型，则会产生20天的提前删除费用。

## 上传/下载/拷贝文件的性能调优 {#section_x5j_w2y_bgb .section}

在cp命令中，通过`--jobs`项和`--parallel`项控制并发数。当ossutil自行设置的默认并发达不到用户的性能需求时，您可以自行调整该两个选项来升降性能。

-   `--jobs`项控制多个文件上传/下载/拷贝时，文件间启动的并发数。
-   `--parallel`制分片上传/下载/拷贝一个大文件时，每一个大文件启动的并发数。

默认情况下，ossutil会根据文件大小来计算parallel个数（该选项对于小文件不起作用，进行分片上传/下载/拷贝的大文件文件阈值可由`--bigfile-threshold`选项来控制）。当进行批量大文件的上传/下载/拷贝时，实际的并发数为jobs个数乘以parallel个数。

**警告：** 

-   通常情况下，当ECS虚拟机或者服务器在网络、内存、CPU等资源不是特别大的情况下，建议将并发数调整到100以下。如果网络、内存、CPU等资源没有占满，可以适当增加并发数。
-   如果并发数调得太大，由于线程间资源切换及抢夺等，ossutil上传/下载/拷贝性能可能会下降。并发数过大可能会产生EOF错误。所以请根据实际的机器情况调整`--jobs`和`--parallel`选项的数值。如果要进行压测，可在一开始时调低这两项数值，然后逐渐调大直至找到最优值。

## 管理Object {#section_zbv_mfy_bgb .section}

-   设置Object的acl

    ossutil使用set-acl命令设置Object的acl，使用-r选项可以批量设置Object的acl。更多信息可使用帮助命令ossutil help set-acl查看。

    ```
    $./ossutil set-acl oss://my-bucket/path/a private
    ```

    批量设置acl：

    ```
    $./ossutil set-acl oss://my-bucket/path/a private -r
    Do you really mean to recursivlly set acl on objects of oss://dest/a(y or N)? y
    Succeed: Total 3 objects. Setted acl on 3 objects.
    0.963934(s) elapsed
    ```

-   设置Object的meta

    ossutil使用set-meta命令设置Object的meta信息，使用-r选项可以批量设置Object的meta。更多信息可使用帮助命令ossutil help set-meta查看。

    ```
    $./ossutil set-meta oss://my-bucket/path/a x-oss-object-acl:private -u
    ```

-   查看Object描述信息（meta）

    ossutil使用stat命令查看Object的描述（meta）信息。更多信息可使用帮助命令ossutil help stat查看。

    ```
    $./ossutil stat oss://my-bucket/path/a 
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

    ossutil使用restore命令恢复冷冻状态的Object为可读状态。使用`-r`选项批量恢复冷冻状态的Object为可读状态。更多信息可使用帮助命令ossutil help restore查看。

    ```
    $./ossutil restore oss://my-bucket/path/a
    ```

-   创建符号链接

    ossutil使用create-symlink创建符号链接。更多信息可使用帮助命令ossutil help create-symlink查看。

    ```
    $./ossutil create-symlink oss://my-bucket/path/b a
    ```

-   读取符号链接文件的描述信息

    ossutil使用read-symlink读取符号链接文件的描述信息。更多信息可使用帮助命令ossutil help read-symlink查看。

    ```
    $./ossutil read-symlink oss://my-bucket/path/b
    Etag                    : D7257B62AA6A26D66686391037B7D61A
    Last-Modified           : 2017-04-26 15:34:27 +0800 CST
    X-Oss-Symlink-Target    : a
    ```


## 通过过滤参数include/exclude批量操作符合指定条件的文件 {#section_dxl_tw3_kgb .section}

您可以使用过滤参数include/exclude，在cp、set-meta、set-acl操作时批量选择对应条件的Object。

-   include/exclude参数支持格式：

    -   \*：通配符，匹配所有字符。例如：\*.txt表示匹配所有txt格式的文件。
    -   ?：匹配单个字符，例如：abc?.jpg表示匹配所有文件名为abc+任意单个字符的.jpg格式的文件，如abc1.jpg
    -   \[sequence\]：匹配序列的任意字符，例如：abc\[1-5\].jpg，表示匹配文件名为abc1.jpg~abc5.jpg的文件
    -   \[!sequence\]：匹配不在序列的任意字符，例如：abc\[!0-7\].jpg，表示匹配文件名不为abc0.jpg~abc7.jpg的文件。
    **说明：** 

    -   不支持带目录的格式，例如：--include "/usr//test/.jpg"。
    -   include和exclude可以出现多次，当多个规则出现时，这些规则按从左往右的顺序应用。
    -   指定include/exclude选项时，需同时指定recursive（-r）选项。

-   include/exclude参数使用示例
    -   上传/下载时过滤文件
        -   上传所有文件格式为txt的文件：

            ```
            $./ossutil cp dir/ oss://my-bucket/path --include "*.txt" -r
            ```

        -   下载所有文件格式不为jpg的文件：

            ```
            $./ossutil cp dir/ oss://my-bucket/path --exclude "*.jpg" -r
            ```

        -   复制所有文件名包含abc且不是jpg和txt格式的文件：

            ```
            $./ossutil cp oss://my-bucket1/path oss://my-bucket2/path --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
            ```

    -   set-meta时过滤文件
        -   将所有文件格式为jpg的文件设置为低频存储：

            ```
            $./ossutil64 set-meta oss://my-bucket/path X-Oss-Storage-Class:IA --include "*.jpg" -u -r
            
            ```

        -   将所有文件名包含abc且文件格式不为jpg和txt的文件设置为标准存储：

            ```
            $./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -u -r
            ```

    -   set-acl时过滤文件
        -   将所有文件类型为.jpg的文件读写权限设置为私有：

            ```
            $./ossutil64 set-acl oss://my-bucket/path private  --include "*.jpg"  -r
            
            ```

    -   上传指定条件的文件并设定文件类型及读写权限：

        ```
        $./ossutil cp dir/ oss://my-bucket/path -rf --include "*.txt"  --include "*.jpg" --exclude "*2*" --meta  X-Oss-Storage-Class:Standard --acl public-read
        ```


