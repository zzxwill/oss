# 有关Bucket的命令 {#concept_rjp_vgs_vdb .concept}

ossutil提供了创建、删除、列举Bucket、以及为Bucket设置ACL的功能。

**说明：** 关于Bucket其他更多的管理功能暂不支持，如有需要，请使用[osscmd](cn.zh-CN/常用工具/osscmd/快速安装.md#)。

在使用这些命令前，请先使用config命令配置访问AK。

## 创建Bucket {#section_qk1_v2l_xgb .section}

-   创建Bucket

    ```
    ./ossutil mb oss://bucketname [--acl=ACL][--storage-class sc][-c file]
    ```

    如果成功创建，ossutil会打印消耗时间并退出，否则会输出错误信息。

    **说明：** 更多帮助信息请参见`ossutil help mb`。

-   创建Bucket时指定访问权限

    创建Bucket时，其权限默认为Private，可以通过`--acl`选项来指定创建的目标Bucket权限。`--acl`可配置参数为：

    -   private：私有
    -   public-read：公共读
    -   public-read-write：公共读写
    **说明：** 访问权限详情请参见[基于读写权限ACL的权限控制](../../../../../cn.zh-CN/开发指南/权限控制/基于读写权限ACL的权限控制.md#)。

    示例：创建一个公共读写的Bucket：

    ```
    ./ossutil mb oss://bucket --acl=public-read-write
    ```

-   创建Bucket时指定存储类型

    创建Bucket时，其存储类型默认为Standard，可以通过`--storage-class`选项来指定创建的目标Bucket的存储类型。`--storage-class`可配置参数如下：

    -   Standard：标准存储
    -   IA：低频访问
    -   Archive：归档存储
    **说明：** 存储类型详情请参见[存储类型介绍](../../../../../cn.zh-CN/开发指南/存储类型/存储类型介绍.md#)。

    示例：创建一个低频访问存储类型\(IA\)的Bucket:

    ```
    ./ossutil mb oss://bucket --storage-class IA
    ```


## 设置Bucket的ACL权限 {#section_pt4_qkl_xgb .section}

创建Bucket时，Bucket默认的ACL为Private，可以通过set-acl命令来修改Bucket的ACL。在设置Bucket的ACL权限时，需要设置`-b`选项。

示例，将bucket1设置为Private权限：

```
$./ossutil set-acl oss://bucket1 private -b

```

更多帮助信息请参见ossutil help set-acl。

## 删除Bucket {#section_wmp_z2l_xgb .section}

-   删除空Bucket

    ```
    ./ossutil rm oss://bucket -b
    ```

    **说明：** 

    -   删除Bucket必须设置`-b`选项。
    -   被删除的Bucket可能被其他用户重新创建，您不再拥有该Bucket。
    -   更多帮助信息请参见ossutil help rm。
-   清除Bucket数据并删除Bucket

    如果Bucket中有Object或Multipart等数据，需要先删除所有数据再删除Bucket。命令如下：

    ```
    ./ossutil rm oss://bucket -bar
    ```

    **警告：** 该命令将清除Bucket中所有数据，属于危险操作，请谨慎使用。


## 列举Bucket {#section_gjb_g3l_xgb .section}

-   列举所有的Bucket

    以下命令均可用于列举Bucket

    -   ```
./ossutil ls
```

    -   ```
./ossutil ls oss://
```

    **说明：** 可使用`-s`选项显示精简格式，更多帮助信息请参见ossutil help ls。

    示例：

    ```
    $./ossutil ls
    CreationTime                                 Region    StorageClass    BucketName
    2016-10-2116:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://go-sdk-test-bucket-xyz-for-object
    2016-12-0115:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutil-test
    2016-07-1817:54:49 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutilconfig
    2016-07-2010:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://ossutilupdate
    2016-11-1413:08:36 +0800 CST       oss-cn-hangzhou              IA    oss://yyyyy
    2016-08-2509:06:10 +0800 CST       oss-cn-hangzhou         Archive    oss://ztzt
    2016-11-2121:18:39 +0800 CST       oss-cn-hangzhou         Archive    oss://ztztzt
    Bucket Number is:7
    0.252174(s) elapsed
    
    ```

-   分页列举Bucket

    ```
    ./ossutil ls oss:// --limited-num=${num} --marker=${bucketname}
    ```

    当Bucket数目太多时，为了避免刷屏，可以使用`--limited-num`与`--marker`选项来分页列举Bucket。

    -   `--limited-num`用于控制分页展示条数。
    -   `--marker`用于控制分页从哪个Bucket开始列举，ossutil显示的结果从marker设定值之后按字母排序的第一个Bucket开始返回。该值一般为上一页查询显示的最后一个BucketName。
    示例：分页列举前两个Bucket。

    ```
    $ ./ossutil ls oss:// --limited-num=1 -s 
    oss://bucket1
    Bucket Number is:1
    0.303869(s) elapsed
    $ ./ossutil ls oss:// --limited-num=1 -s --marker=bucket1
    oss://bucket2
    Bucket Number is:1
    0.257636(s) elapsed
    ```


## 列举Object {#section_vqm_qkl_xgb .section}

-   列举指定Bucket下所有的Object

    ```
    ./ossutil ls oss://bucketname
    ```

    示例：

    ```
    $./ossutil ls oss://ossutil-test
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a1
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a2
    2016-12-0115:06:45 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a3
    Object Number is:3
    0.007379(s) elapsed
    
    ```

-   列举所有的Object和未完成的Multipart事件

    ```
    ./ossutil ls oss://bucket -a
    ```

    示例：

    ```
    $./ossutil ls oss://bucket1 -a 
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-0514:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-0514:36:21 +0000 CST        201933      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1
    2016-04-0814:50:47 +0000 CST       6476984      Standard   4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is:3
    InitiatedTime                     UploadID                           ObjectName
    2017-01-1303:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-1303:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-1303:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-2011:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is:4
    0.191289(s) elapsed
    
    ```

-   分页列举所有的Object

    ```
    ./ossutil ls oss://bucket --limited-num=${num} --marker=${obj}
    ```

    与[分页列举 Bucket](cn.zh-CN/常用工具/命令行工具ossutil/有关Bucket的命令.md#section_gjb_g3l_xgb) 类似，可以使用`--limited-num`与`--marker`选项来分页列举Object。示例：

    ```
    $./ossutil ls oss://ossutil-test --limited-num=1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a1
    Object Number is:1
    0.007379(s) elapsed
    $./ossutil ls oss://ossutil-test --limited-num=1 --marker=a1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a2
    Object Number is:1
    0.008392(s) elapsed
    
    ```

-   指定列举结果显示模式为精简模式

    ```
    ./ossutil ls oss://bucket -s
    ```

    示例：

    ```
    $./ossutil ls oss://ossutil-test
    oss://ossutil-test/a1
    oss://ossutil-test/a2
    oss://ossutil-test/a3
    Object Number is:3
    0.007379(s) elapsed  
    
    ```

-   模拟目录方式列举

    ```
    ./ossutil ls oss://bucket -d
    ```

    使用`-d`选项可以显示当前目录下的文件和子目录，而非递归显示所有子目录下的所有Object。示例：

    ```
    $./ossutil ls oss://bucket1 -s -d
    oss://bucket1/obj1
    oss://bucket1/sample.txt
    oss://bucket1/dir1/
    Object and Directory Number is:3 
    0.119884(s) elapsed
    
    ```

-   请求者付费模式下列举

    详情请参考[请求者付费模式相关命令](cn.zh-CN/常用工具/命令行工具ossutil/有关Object的命令.md#)。


## 列举未上传完成的分片上传任务 {#section_pmq_tlt_xgb .section}

-   列举未上传完成的分片上传任务

    ```
    ./ossutil ls oss://bucket -m
    ```

    使用 -m 选项可以当前操作的Bucket中未上传完成的Multipart事件。如下示例：

    ```
    $./ossutil ls oss://bucket1 -m 
    InitiatedTime                  UploadID                          ObjectName
    2017-01-1303:45:26 +0000 CST  15754AF7980C4DFB8193F190837520BB  oss://bucket1/obj1
    2017-01-1303:45:25 +0000 CST  3998971ACAF94AD9AC48EAC1988BE863  oss://bucket1/obj2
    2017-01-2011:16:21 +0800 CST  A20157A7B2FEC4670626DAE0F4C0073C  oss://bucket1/tobj
    UploadID Number is:30.009424(s) elapsed
    
    ```

-   更多有关分片上传操作命令，详见[有关Multipart的命令](http://www.baidu.com/)

## 探测OSS网络 {#section_njd_yzz_zgb .section}

probe命令是针对OSS访问的检测命令，可用于排查上传下载过程中因网络故障或基本参数设置错误导致的问题。执行probe命令后，ossutil会提示可能的错误原因，帮助您快速排查错误原因。

**说明：** 使用该命令前，建议先使用帮助命令ossutil probe help查看命令详情。

-   下载http\_url地址到本地，并输出探测报告

    ```
    $./ossutil probe --download --url http_url [--addr=domain_name] [file_name]
    ```

    通过文件URL将存储空间内的一个文件下载到本地来测试网络传输质量，并输出探测报告。

    -   --url：填写指定Bucket内文件的URL地址。

        -   公共读文件：直接输入文件URL，例如：`https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg`。
        -   私有文件：输入带签名的文件URL，并且在URL前后需加双引号（“”）。例如：`“https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg?Expires=1552015472&OSSAccessKeyId=TMP.xxxxxxxx5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3AxxxxxxoCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D”`。
        **说明：** 如何文件URL请参考[获取URL](https://help.aliyun.com/knowledge_detail/39607.html)。

    -   --addr=domain\_name（可选）：可在下载文件时同时向一个指定的域名或IP地址发起ping操作。不添加该选项，ossutil不进行额外的探测操作。
        -   增加`--addr=`选项，但是缺省值的话，默认向`www.aliyun.com`发起ping操作。
        -   增加`--addr=`选项，并指定一个域名或IP，则向指定的地址发起ping操作。
    -   file\_name（可选）：为下载的文件指定存储路径。如果不输入file\_name，则下载文件保存在当前目录下，文件名由工具自动判断；如果输入file\_name，则file\_name为文件名或者目录名，下载的文件名为file\_name或者保存在file\_name目录下。
-   下载指定Bucket中的Object，并输出探测报告

    ```
    $./ossutil probe --download --bucketname bucket-name [--object=object_name]
    [--addr=domain_name] [file_name]
    ```

    -   --bucketname：填写Bucket名。
    -   --object=（可选）：填写指定的Object路径可下载指定的Object。例如：path/myphoto.jpg。不指定`--object`，则工具会生成一个临时文件上传到`bucket-name`后再将其下载，下载结束后会将该临时文件从本地和Bucket中删除。
-   上传探测并输出探测报告

    ```
    $./ossutil probe --upload [file_name] --bucketname bucket-name [--object=obj
    ect_name] [--addr=domain_name] [--upmode]
    ```

    -   file\_name（可选）： 指定`file_name`可将指定的文件上传到`bucket-name`中；不指定`file_name`，则工具会生成一个临时文件上传到`bucket-name`中，探测结束后会将临时文件删除。
    -   --object=（可选）：输入文件名或文件路径，可指定Object上传后的名称，例如：path/myphoto.jpg。若不配置`--object`，则由工具自动生成上传后的Object名称，探测结束后会将该临时Object删除。
    -   --upmode（可选）：指定上传的方式，缺省该项则使用普通上传。可选值为：
        -   normal：普通上传
        -   append：追加上次
        -   multipart：分片上传

-   查看探测报告

    probe命令运行后，您可以看到任务执行的步骤及结果。

    -   执行步骤后出现×表示没有通过，否则表示通过。
    -   结果显示整个上传下载成功还是失败。当成功时，会给出文件的大小和上传下载时间；失败时，会给出导致错误的原因，或直接给出修改建议。

        **说明：** 并不是每次错误的检测都能提示出修改建议，对于没有提示修改建议的检测，请根据错误码提示，并结合[oss错误码ErrorCode](../../../../../cn.zh-CN/SDK 参考/Java/异常处理.md#section_gn4_55c_bhb)进行问题排查。

    probe命令执行完毕后，ossutil会在当前目录下生成一个以logOssProbe开头的文件，里面包含此次探测命令执行的详细信息。


