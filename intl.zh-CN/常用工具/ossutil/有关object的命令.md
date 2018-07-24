# 有关object的命令 {#concept_lqt_vjs_vdb .concept}

ossutil提供了上传/下载/拷贝文件、设置object的acl、设置object的meta、查看object的meta信息等功能。

使用这些命令前请使用config命令配置访问AK。

-   上传/下载/拷贝文件

    强烈建议在使用cp命令前使用ossutil help cp先查看帮助。

    可以使用cp命令进行上传/下载/拷贝文件，使用-r选项来拷贝文件夹，对大文件默认使用分片上传并可进行断点续传（开启分片上传的大文件阈值可用`--bigfile-threshold`选项来设置）。

    使用-f选项来默认强制上传，当目标端存在同名文件时，不询问，直接覆盖。

    当批量上传/下载/拷贝文件时，如果某个文件出错，ossutil默认会将错误信息记录在report文件，并跳过该文件，继续其他文件的操作（当错误为Bucket不存在、accessKeyID/accessKeySecret错误造成的权限验证非法等错误时，不再继续其他文件拷贝）。更多信息请见ossutil help cp。

    ossutil支持特定场景下的增量上传策略：`--update`和`--snapshot-path`选项，请参见ossutil help cp。

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

-   上传/下载/拷贝文件的性能调优

    在cp命令中，通过`jobs`项和`-parallel`项控制并发数。`-jobs`项控制多个文件上传/下载/拷贝时，文件间启动的并发数。`-parallel`制分片上传/下载/拷贝一个大文件时，每一个大文件启动的并发数。

    默认情况下，ossutil会根据文件大小来计算parallel个数（该选项对于小文件不起作用，进行分片上传/下载/拷贝的大文件文件阈值可由—bigfile-threshold选项来控制），当进行批量大文件的上传/下载/拷贝时，实际的并发数为jobs个数乘以parallel个数。该两个选项可由用户调整，当ossutil自行设置的默认并发达不到用户的性能需求时，用户可以自行调 整该两个选项来升降性能。

    **说明：** 

    -   如果并发数调得太大，由于线程间资源切换及抢夺等，ossutil上传/下载/拷贝性能可能会下降，所以请根据实际的机器情况调整这两个选项的数值，如果要进行压测，可以一开始将两个数值调低，慢慢调大寻找最优值。
    -   如果--jobs选项和--parallel选项值太大，在机器资源有限的情况下，可能会因为网络传输太慢，产生EOF错误，这个时候请适当降低--jobs选项和--parallel选项值。
-   设置object的acl

    ossutil使用set-acl命令设置object的acl，使用-r选项可以批量设置object的acl。

    更多信息见：ossutil help set-acl

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

-   设置object的meta

    ossutil使用set-meta命令设置object的meta信息，使用-r选项可以批量设置object的meta。

    更多信息见：ossutil help set-meta

    ```
    ./ossutil set-meta oss://dest/a x-oss-object-acl:private -u
    ```

-   查看object描述信息（meta）

    ossutil使用stat命令查看object的描述（meta）信息。

    更多信息见：ossutil help stat

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

-   恢复冷冻状态的object为可读状态

    ossutil使用restore命令恢复冷冻状态的object为可读状态。可以使用`-r`选项批量恢复冷冻状态的objects为可读状态。

    更多信息见：ossutil help restore

    ```
    $./ossutil restore oss://utiltest/a
    0.037729(s) elapsed
    ```

-   创建符号链接

    ossutil使用create-symlink创建符号链接。

    更多信息见：ossutil help create-symlink

    ```
    $./ossutil create-symlink oss://utiltest/b a
    0.037729(s) elapsed
    ```

-   读取符号链接文件的描述信息

    ossutil使用read-symlink读取符号链接文件的描述信息。

    更多信息见：ossutil help read-symlink

    ```
    $./ossutil read-symlink oss://utiltest/b
    Etag                    : D7257B62AA6A26D66686391037B7D61A
    Last-Modified           : 2017-04-26 15:34:27 +0800 CST
    X-Oss-Symlink-Target    : a
    0.112494(s) elapsed
    ```


