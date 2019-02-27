# 有关Object的命令 {#concept_lqt_vjs_vdb .concept}

ossutil提供了上传/下载/拷贝文件（Object）和文件夹、设置文件读写权限 ACL、设置和查看文件元信息（object meta）等功能。

使用这些命令前，请使用 config 命令配置访问 AK。

## 上传/下载/拷贝文件 {#section_qb3_y2y_bgb .section}

强烈建议在使用 cp 命令前，先使用 ossutil help cp 来查看帮助。

-   cp 命令简介

    使用 cp 命令进行上传/下载/拷贝文件时：

    -   使用 `-r` 选项来拷贝文件夹，对大文件默认使用分片上传并可进行断点续传（开启分片上传的大文件阈值可用 `--bigfile-threshold`选项来设置）。
    -   使用 `-f` 选项来默认强制上传，当目标端存在同名文件时，不询问，直接覆盖。
    -   使用 `--retry-times=${times}` 选项来指定当错误发生时 ossutil 重试次数，该值默认为 10，取值范围为：1-500。
    -   使用`--encoding-type url` 选项表示输入的 Object 名和文件名都是经过 url 编码。目前，ossutil 只支持 url encode，且 Bucket 名不支持 url encode。
    当批量上传/下载/拷贝文件时，如果某个文件出错，ossutil 默认会将错误信息记录在 report 文件，并跳过该文件，继续其他文件的操作。更多信息请参见 ossutil help cp。

    **说明：** 当出现存储空间（Bucket）不存在、AccessKeyID/AccessKeySecret 错误造成的权限验证非法等错误时，不再继续拷贝其他文件。

-   上传文件
    -   将当前目录下 a.txt 文件上传到 oss://bucket/path

        ```
        $./ossutil cp a.txt oss://bucket/path
        ```

    -   上传单个文件并指定`--meta` 选项

        上传文件的同时可以使用`--meta` 选项设置Object 的 meta 信息，其内容格式为`header:value#header:value...`，如下将当前目录下文件 a.txt 上传并设置其 meta 信息：

        ```
        `$./ossutil cp a.txt oss://bucket/path --meta=Cache-Control:no-cache#Content-Encoding:gzip`
        ```

        **说明：** 当前可选的 Header 列表如下：

        ```
        Headers:
              Expires(time.RFC3339:2006-01-02T15:04:05Z07:00)
              X-Oss-Object-Acl
              Origin
              X-Oss-Storage-Class
              Content-Encoding
              Cache-Control
              Content-Disposition
              Accept-Encoding
              X-Oss-Server-Side-Encryption
              Content-Type
              以及以X-Oss-Meta-开头的header
        ```

        Header 不区分大小写，但 Value 区分大小写。设置后将用指定的 meta 代替原来的 meta。没有指定的 HTTP HEADER 将保留，没有指定的 user meta 将会被删除。

    -   上传文件夹：

        上传文件时，搭配 `-r` 选项可以将目标文件夹上传到 OSS。命令如下:

        ```
        $./ossutil cp -r dir oss://bucket/path
        ```

        **说明：** 若上传目标对象为符号软链接，且指向本地文件夹，则使用 cp 命令上传时，应当在软链接加上正斜线（/）。

        ```
        ./ossutil cp -r symbolic_link/ oss://bucket/path
        ```

    -   上传文件夹并指定 `--update` 选项

        批量上传时，若指定 `--update` 选项，只有当目标文件不存在，或源文件新于目标文件时，ossutil 才会执行上传操作。命令如下：

        ```
        $./ossutil cp -r dir oss://bucket/path --update
        ```

        该选项可用于当批量上传失败重传时，跳过已经成功的文件，实现增量上传。

    -   上传文件夹并指定 `--snapshot-path`选项

        批量上传时，若指定 `--snapshot-path` 选项，ossutil 在指定的目录下生成文件上传的快照信息，在下一次指定该选项上传时，ossutil 会读取指定路径下的快照信息进行增量上传。命令如下：

        ```
        $./ossutil cp -r dir oss://bucket/path --snapshot-path=path
        
        ```

        **说明：** 

        -   `--snapshot-path` 选项用于在某些场景下加速增量上传批量文件\(目前，下载和拷贝不支持该选项\)。例如，文件数较多且两次上传期间没有其他用户更改 OSS 上对应的 Object。
        -   `--snapshot-path` 命令通过在本地记录成功上传的文件的本地 lastModifiedTime，从而在下次上传时通过比较 lastModifiedTime 来决定是否跳过相同文件的上传，所以在使用该选项时，请确保两次上传期间没有其他用户更改了 OSS 上的对应 Object。当不满足该场景时，如果想要增量上传批量文件，请使用 `--update` 选项。
        -   ossutil 不会主动删除 snapshot-path 下的快照信息，为了避免快照信息过多，当用户确定快照信息无用时，请用户自行清理 snapshot-path。
        -   由于读写 snapshot 信息需要额外开销，当要批量上传的文件数比较少或网络状况比较好或有其他用户操作相同 Object 时，并不建议使用该选项。可以使用 `--update` 选项来增量上传。
        -   `--update` 选项和 `--snapshot-path` 选项可以同时使用，ossutil 会优先根据 snapshot-path 信息判断是否跳过上传，如果不满足跳过条件，再根据`--update` 判断是否跳过上传。
-   下载文件
    -   下载单个文件：

        ```
        $./ossutil cp oss://my-bucket/path/test1.txt /dir
        ```

        下载文件时，可以通过`--range` 选项进行指定范围下载。

        示例：将 test1.txt 的第 10 到第 20 个字符作为一个文件载到本地。

        ```
        $./ossutil cp oss://my-bucket/path/test1.txt /dir  --range=10-20
        Succeed: Total num: 1, size: 11. OK num: 1(download 1 objects).
        0.290769(s) elapsed
        ```

    -   下载文件夹：

        ```
        $./ossutil  cp -r oss://my-bucket/path  /dir 
        ```

    -   下载文件夹并指定 `--update` 选项

        批量下载时，若指定 `--update` 选项，只有当目标文件不存在，或源文件新于目标文件时，ossutil 才会执行下载操作。命令如下：

        ```
        $./ossutil cp -r oss://bucket/path  /dir  --update
        
        ```

        该选项可用于当批量下载失败重传时，跳过已经下载成功的文件。实现增量下载。

-   拷贝文件

    目前只支持拷贝 Object，不支持拷贝未完成的 Multipart，不支持跨 region 拷贝 Object。

    -   拷贝单个文件

        ```
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/
        
        ```

    -   拷贝单个文件并重命名

        ```
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/b
        
        ```

    -   拷贝单个文件并指定 `--meta` 选项

        拷贝文件的同时可以使用 `--meta` 选项设置 Object 的 meta 信息，其内容格式为`header:value#header:value...`。

        ```
        ./ossutil cp oss://bucket/path1/a oss://bucket/path2/ --meta=Cache-Control:no-cache
        ```

        **说明：** 当前可选的 Header 列表如下：

        ```
        Headers:
              Expires(time.RFC3339:2006-01-02T15:04:05Z07:00)
              X-Oss-Object-Acl
              Origin
              X-Oss-Storage-Class
              Content-Encoding
              Cache-Control
              Content-Disposition
              Accept-Encoding
              X-Oss-Server-Side-Encryption
              Content-Type
              以及以X-Oss-Meta-开头的header
        ```

        Header 不区分大小写，但 Value 区分大小写。设置后将用指定的 meta 代替原来的 meta。没有指定的 HTTP HEADER 将保留，没有指定的 user meta 将会被删除。

    -   同 region 不同 Bucket 之间的文件拷贝

        拷贝文件时，搭配 `-r` 选项可以实现批量文件拷贝功能。

        ```
        $./ossutil cp oss://your_src_bucket/path1/ oss://your_dest_bucket/path2/ -r
        
        ```

    -   同 region 不同 Bucket 之间的增量文件拷贝

        批量拷贝时，若指定 `--update` 选项，只有当目标文件不存在，或源文件新于目标文件时，ossutil 才会执行拷贝操作。命令如下：

        ```
        $./ossutil cp oss://your_src_bucket/path1/ oss://your_dest_bucket/path2/ -r --update
        ```

        该选项可用于当批量拷贝失败重传时，跳过已经拷贝成功的文件，实现增量拷贝。

-   上传/下载/拷贝文件的性能调优

    在 cp 命令中，通过 `--jobs` 项和 `--parallel` 项控制并发数。当 ossutil 自行设置的默认并发达不到用户的性能需求时，您可以自行调整该两个选项来升降性能。

    -   `--jobs` 项控制多个文件上传/下载/拷贝时，文件间启动的并发数。
    -   `--parallel`制分片上传/下载/拷贝一个大文件时，每一个大文件启动的并发数。
    默认情况下，ossutil 会根据文件大小来计算 parallel 个数（该选项对于小文件不起作用，进行分片上传/下载/拷贝的大文件文件阈值可由 `--bigfile-threshold` 选项来控制）。当进行批量大文件的上传/下载/拷贝时，实际的并发数为 jobs 个数乘以 parallel 个数。

    **警告：** 

    -   通常情况下，当 ECS 虚拟机或者服务器在网络、内存、CPU 等资源不是特别大的情况下，建议将并发数调整到 100 以下。如果网络、内存、CPU 等资源没有占满，可以适当增加并发数。
    -   如果并发数调得太大，由于线程间资源切换及抢夺等，ossutil 上传/下载/拷贝性能可能会下降。并发数过大可能会产生 EOF 错误。所以请根据实际的机器情况调整`--jobs` 和`--parallel` 选项的数值。如果要进行压测，可在一开始时调低这两项数值，然后逐渐调大直至找到最优值。

## 生成文件 URL {#section_vh3_qqm_xgb .section}

使用 sign 命令可以生成经过签名的文件 URL 供第三方用户临时访问 Object。

-   生成默认超时时间的文件 URL，默认超时时间为 60 秒

    ```
    $./ossutil sign oss://bucket/path/object
    
    ```

-   生成指定超时时间的文件 URL

    使用`--timeout` 选项，可以设置文件 URL 的超时时间，单位为秒，默认值为 60，取值范围为 0-9223372036854775807。

    ```
    $./ossutil sign oss://bucket/path/object --timeout=${time}
    ```

    **说明：** 更多信息请参见帮助命令 ./ossutil help sign.


## 删除文件 {#section_vj5_rqm_xgb .section}

-   删除单个 Object

    ```
    $./ossutil rm oss://bucket/path/object
    ```

-   删除指定前缀的所有 Object

    配合`-r`选项，可以递归执行删除操作删除指定前缀的所有Object。

    ```
    $./ossutil rm oss://bucket/path/ -r
    ```

    **说明：** 更多帮助信息请参考./ossutil help rm。


## 管理 Object {#section_stq_ysm_xgb .section}

-   设置 Object 的 ACL 权限

    ossutil使用set-acl命令来设置Object的ACL权限，可以使用`-r`选项递归操作设置Object的ACL权限。

    **说明：** Object的ACL权限分为：

    -   default：继承Bucket
    -   private：私有
    -   public-read：公共读
    -   public-read-write：公共读写
    -   设置指定 Object 的 ACL 权限

        ```
        $./ossutil set-acl oss://bucket/path/object private
        
        ```

    -   设置指定前缀的所有文件的 ACL 权限：

        ```
        $./ossutil set-acl oss://bucket/path/ private -r
        ```

    **说明：** 更多帮助信息请参见 ./ossutil help set-acl。

-   设置 Object 的 meta 信息

    ossutil 使用 set-meta 命令设置 Object 的 meta 信息，可以使用 `-r` 选项递归操作设置 Object 的 meta 信息。

    -   设置指定 Object 的 meta 信息

        ```
        $./ossutil set-meta oss://bucket/path/object x-oss-object-acl:private -u
        
        ```

    -   设置指定前缀的所有 Object 的 meta 信息：

        ```
        $./ossutil set-meta oss://bucket/path/ x-oss-object-acl:private -u -r
        
        ```

    **说明：** 更多帮助信息请参见 ./ossutil help set-meta。

-   查看 Object 的 meta 信息

    ossutil 使用 stat 命令来查看 Object 的 meta 信息。

    ```
    $./ossutil stat oss://bucket/path/object
    
    ```

    示例：查看oss://my-bucket/path/a 的 meta 信息。

    ```
    $./ossutil stat oss://my-bucket/path/a 
    ACL                         : default
    Accept-Ranges               : bytes
    Content-Length              :230
    Content-Md5                 : +5vbQC/MSQK0xXSiyKBZog==
    Content-Type                : application/octet-stream
    Etag                        : FB9BDB402FCC4902B4C574A2C8A059A2
    Last-Modified               :2017-01-1315:14:22 +0800 CST
    Owner                       : aliyun
    X-Oss-Hash-Crc64ecma        :12488808046134286088
    X-Oss-Object-Type           : Normal
    0.125417(s) elapsed
    
    ```

    **说明：** 更多帮助信息请参见 ./ossutil help stat。

-   恢复冷冻状态的 Object 为可读状态

    ossutil 使用 restore 命令恢复冷冻状态的 Object 为可读状态，可以使用 `-r` 选项递归操作恢复冷冻状态的 Object 为可读状态。

    -   恢复指定的冷冻文件为可读状态：

        ```
        $./ossutil restore oss://bucket/path/object
        
        ```

    -   恢复指定前缀的所有冷冻文件为可读状态：

        ```
        $./ossutil restore oss://bucket/path/ -r
        
        ```

        **说明：** 

        -   restore 操作解冻过程需要 1 分钟时间，解冻中无法读取 Object。
        -   解冻状态默认持续 1 天，对解冻状态的 Object 调用 restore 命令，会将 Object 的解冻状态延长一天，最多可以延长到 7 天，之后 Object 又回到初始时的冷冻状态。
        -   更多帮助信息请参见 ./ossutil help restore。
-   符号链接（软链接）
    -   创建符号链接

        ossutil 使用 create-symlink 命令创建符号链接。

        ```
        $./ossutil create-symlink oss://bucket/path/symlink-object oss://bucket/path/object
        
        ```

        示例：创建指向文件 a.txt 的符号链接 b。

        ```
        $./ossutil create-symlink oss://my-bucket/path/b oss://my-bucket/path/a.txt
        
        ```

        **说明：** 创建符号链接时：

        -   不检查目标文件是否存在。
        -   不检查目标文件类型是否合法。
        -   不检查目标文件是否有权限访问。
        -   以上检查在 GetObject 等需要访问目标文件的 API 接口时进行。
        -   如果试图添加的文件已经存在，并且有访问权限。新添加的文件将覆盖原来的文件。
        更多帮助信息请参见 ./ossutil help create-symlink。

    -   读取符号链接文件的描述信息

        ossutil 使用 read-symlink 命令读取符号链接文件的描述信息。

        ```
        $./ossutil read-symlink oss://bucket/path/symlink-object
        
        ```

        示例：读取符号链接文件 b 的描述信息。

        ```
        $./ossutil read-symlink oss://my-bucket/path/b
        Etag                    : D7257B62AA6A26D66686391037B7D61A
        Last-Modified           :2017-04-2615:34:27 +0800 CST
        X-Oss-Symlink-Target    : a
        ```

        **说明：** 更多帮助信息请参见 ./ossutil help read-symlink。


## 请求者付费模式 {#section_qpx_wvm_xgb .section}

ossutil 从 1.4.2 版本开始，支持对开通了[请求者付费模式](../../../../../cn.zh-CN/控制台用户指南/管理存储空间/设置请求者付费模式.md#)的Bucket内文件进行上传、下载和拷贝的操作。

-   上传文件到开通了请求者付费模式的 Bucket：

    ```
    $./ossutil cp dir/test.mp4 oss://payer/ --payer=requester
    ```

-   从开通了请求者付费模式的 Bucket 下载文件：

    ```
    $./ossutil cp oss://payer/test.mp4 dir/ --payer=requester
    ```

-   从开通了请求者付费模式的 Bucket 拷贝文件到普通 Bucket：

    ```
    $./ossutil cp oss://payer/test.mp4 oss://my-bucket/path  --payer=requester
    ```

-   从普通 Bucket 拷贝数据到开通了请求者付费模式的 Bucket：

    ```
    $./ossutil cp oss://my-bucket/path/test.mp4 oss://payer --payer=requester
    ```

-   列举开通了请求者付费模式的 Bucket 内的 Object：

    ```
    $./ossutil ls oss://bucket --payer=requester
    ```


## 修改文件存储类型 {#section_cnl_x2y_bgb .section}

修改 5GB 以下文件的存储类型，可使用 set-meta 命令；如果想修改 5GB 以上文件的存储类型，必须使用 cp 命令。

-   通过 set-meta命令修改文件存储类型。
    -   设置单个文件的存储类型为 IA 类型：

        ```
        $./ossutil set-meta oss://my-bucket/path/0104_6.jpg X-Oss-Storage-Class:IA -u
        ```

    -   设置文件目录的所有文件存储类型为 Standard 类型：

        ```
        $./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard -r -u
        ```

-   通过 cp 命令上传文件，并通过 `--meta` 选项修改对象存储类型。

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

    -   存储类型为归档类型的文件不能通过 set-meta 或者 cp 命令直接转换成其他类型，必须先通过 restore 命令将该文件转换成低频存储，之后再使用 set-meta 或者 cp 命令转换文件类型。
    -   使用 cp 覆写文件时涉及到数据覆盖操作。如果**低频型**或**归档型**文件分别在创建后 30 和 60 天内被覆盖，则它们会产生提前删除费用。例如，低频型文件创建 10 天后，被覆写修改成归档型或标准型，则会产生 20 天的提前删除费用。

## 通过过滤参数 include/exclude 批量操作符合指定条件的文件 {#section_dxl_tw3_kgb .section}

您可以使用过滤参数 include/exclude，在 cp、set-meta、set-acl 操作时批量选择对应条件的 Object。

-   include/exclude 参数支持格式：

    -   \*：通配符，匹配所有字符。例如：\*.txt 表示匹配所有 txt 格式的文件。
    -   ?：匹配单个字符，例如：abc?.jpg 表示匹配所有文件名为 abc+ 任意单个字符的 jpg 格式的文件，如 abc1.jpg
    -   \[sequence\]：匹配序列的任意字符，例如：abc\[1-5\].jpg 表示匹配文件名为 abc1.jpg~abc5.jpg 的文件
    -   \[!sequence\]：匹配不在序列的任意字符，例如：abc\[!0-7\].jpg，表示匹配文件名不为 abc0.jpg~abc7.jpg 的文件。
    **说明：** 

    -   不支持带目录的格式，例如：--include "/usr//test/.jpg"。
    -   include 和 exclude 可以出现多次，当多个规则出现时，这些规则按从左往右的顺序应用。
    -   指定 include/exclude 选项时，需同时指定 recursive（-r）选项。

-   include/exclude 参数使用示例
    -   上传/下载时过滤文件
        -   上传所有文件格式为 txt 的文件：

            ```
            $./ossutil cp dir/ oss://my-bucket/path --include "*.txt" -r
            ```

        -   下载所有文件格式不为 jpg 的文件：

            ```
            $./ossutil cp dir/ oss://my-bucket/path --exclude "*.jpg" -r
            ```

        -   复制所有文件名包含 abc 且不是 jpg 和 txt 格式的文件：

            ```
            $./ossutil cp oss://my-bucket1/path oss://my-bucket2/path --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -r
            ```

    -   set-meta 时过滤文件
        -   将所有文件格式为 jpg 的文件设置为低频存储：

            ```
            $./ossutil64 set-meta oss://my-bucket/path X-Oss-Storage-Class:IA --include "*.jpg" -u -r
            
            ```

        -   将所有文件名包含 abc 且文件格式不为 jpg 和 txt 的文件设置为标准存储：

            ```
            $./ossutil set-meta oss://my-bucket/path X-Oss-Storage-Class:Standard --include "*abc*" --exclude "*.jpg" --exclude "*.txt" -u -r
            ```

    -   set-acl 时过滤文件
        -   将所有文件类型为 .jpg 的文件读写权限设置为私有：

            ```
            $./ossutil64 set-acl oss://my-bucket/path private  --include "*.jpg"  -r
            
            ```

    -   综合示例：上传指定条件（所有文件名不包含 2 且文件格式为 jpg 和 txt）的文件并设定文件存储类型及读写权限：

        ```
        $./ossutil cp dir/ oss://my-bucket/path -rf --include "*.txt"  --include "*.jpg" --exclude "*2*" --meta  X-Oss-Storage-Class:Standard --acl public-read
        ```


