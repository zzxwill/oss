# ossprobe（即将下线） {#concept_frt_pxg_wdb .concept}

ossprobe是一款针对oss访问的检测工具，用于排查上传下载过程中，因网络故障或基本参数设置错误导致的问题。您在执行上传下载命令后，ossprobe会提示可能的错误原因，帮助您快速找出错误。

**说明：** ossprobe的操作命令已整合到ossutil中，ossprobe工具将于2019年4月30日前下架，给您带来不便敬请谅解。ossutil中ossprobe的命令详情请参考[ossprobe命令](intl.zh-CN/常用工具/命令行工具ossutil/有关Bucket的命令.md#section_njd_yzz_zgb)。

## 版本 {#section_us5_vxg_wdb .section}

版本号：1.0.0

## 主要功能 {#section_ngl_xxg_wdb .section}

-   检测网络环境是否正常
-   检查基本参数是否正常
-   测试基本上传下载速度

## 支持平台 {#section_j5g_yxg_wdb .section}

-   linux
-   windows
-   mac

## 软件下载 {#section_nsf_zxg_wdb .section}

-   [windows64 ossprobe](http://gosspublic.alicdn.com/ossprobe/windows64-ossprobe.exe)
-   [linux64 ossprobe](http://gosspublic.alicdn.com/ossprobe/linux64-ossprobe)
-   [mac ossprobe](http://gosspublic.alicdn.com/ossprobe/mac-ossprobe)

## 检测下载问题 {#section_q4y_zxg_wdb .section}

-   用法

    ```
    ossprobe --download  [-i AccessKeyId] [-k AccessKeySecret] [-p EndPoint] [-b BucketName] [-o ObjectName] [-t LocalPath]  
            [-f Url] [-a Address]
        -f   --from       Object的Url
        -i   --id         AccessKeyId
        -k   --key        AccessKeySecret
        -p   --endpoint   EndPoint
        -b   --bucket     BucketName
        -o   --object     ObjectName
        -t   --to         保存下载内容的文件路径，默认为当前目录下的临时文件的路径。
        -a   --addr       检测的网络地址，默认的地址为www.aliyun.com，如果您使用的是专有云，请一定要选择该网络内可以访问的地址。
    ```

-   示例

    检测Url下载是否正常（[获取Url的方法](../../../../../intl.zh-CN/快速入门/下载文件.md#)），可以使用下面的命令：

    |方式|命令|
    |:-|:-|
    |从指定Url下载|`ossprobe --download -f Url`|
    |从指定Url下载到指定文件|`ossprobe --download -f Url -t tmp/example.txt`|
    |从指定Url下载、并检测指定地址网络状况|`ossprobe --download -f Url -a Addr`|

    检测指定参数\(AccessKeyID, AccessKeySecret, EndPoint, BucketName\)下载是否正常，可以通过以下命令检测：

    |方式|命令|
    |:-|:-|
    |下载随机文件|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b
              BucketName
    ```

 |
    |下载指定的文件|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName
    ```

 |
    |下载指定的文件并保存到本地指定文件|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -t tmp/example.txt
    ```

 |
    |下载随机文件、并检测指定地址网络状况|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -a
              Addr
    ```

 |

    **说明：** 

    -   用户下载的是二进制可执行程序，在linux上需要通过`chmod +x ossprobe`添加ossprobe的可执行权限。
    -   -t的参数的默认值是当前目录下的一个临时文件的路径\(文件名格式为：ossfilestore20160315060101\)。
    -   如果-t的参数值为一个目录，那么就在该目录产生一个临时文件\(文件名格式为：ossfilestore20160315060101\)，用于保存数据。
    -   采用Url下载时，保存文件名取Url中以“/”分割之后最后的一个字符串，比如说Url为`http://aliyun.com/a.jpg`，文件名就是`a.jpg`。

## 检测上传问题 {#section_lb2_kyg_wdb .section}

-   用法

    ```
    ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint  -b BucketName [-m normal|append|multipart]  
            [-s UploadFilePath] [-o ObjectName] [-a Addr]
        -i    --id        AccessKeyID
        -k    --key       AccessKeySecret
        -p    --endpoint  EndPoint
        -b    --bucket    BucketName
        -s    --src       待上传文件路径，默认为本地临时文件的路径。
        -m    --mode      文件上传方式，默认为normal上传。
        -o    --object    上传后的object名称，当-s非空，默认值为上传文件名。当-s为空，默认值为以tem开头的临时文件的文件名。
        -a    --addr      检测的网络地址，默认为阿里云的官网地址，如果您使用的是专有云，请一定要选择该网络内可以访问的地址。
    ```

-   示例

    |方式|命令|
    |:-|:-|
    |生成临时文件，并采用normal方式上传|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b
            BucketName
    ```

 |
    |生成临时文件，并采用append方式上传|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m append
    ```

 |
    |生成临时文件，并采用multipart方式上传|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m multipart
    ```

 |
    |采用multipart上传方式，上传指定的内容|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m multipart -s src
    ```

 |
    |采用multipart上传指定的内容，并给出Object名称|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -m
              multipart -s src -o example.txt
    ```

 |
    |生成临时文件，采用normal方式上传，并检测指定地址网络状况|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -a
              Addr
    ```

 |

    **说明：** 随机产生的文件名以ossuploadtmp开头。


## 平台差异 {#section_x1b_bzg_wdb .section}

-   windows按下Win + R调出运行对话框，输入命令cmd并按回车执行。 在弹出的命令行终端界面中，输入该工具的所在的路径，然后填入相关检测参数后即可执行。
-   linux&mac打开终端，在弹出的终端界面中，输入该工具的所在的路径，然后填入相关检测参数后即可执行。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4879/15547159432982_zh-CN.jpg)


## 查看报告结果 {#section_kkh_3zg_wdb .section}

命令运行结束时，生成一个文件名为logOssProbe20060102150405.txt（logOssProbe后面的数字为当前时间的格式化输出）的报告文件。可能的错误原因会在命令行打印。如果错误提示不够具体，用户可以查看报告进行排查问题。如果还是无法解决，您也可以在工单里附上检测报告。

控制台显示

控制台显示的主要内容有：

-   执行步骤后出现×表示没有通过，否则表示通过。
-   结果显示整个上传下载成功还是失败。当成功时，会给出文件的大小和上传下载时间。
-   修改建议项提示导致错误的原因，或直接给出修改建议。
-   用户如果对oss错误码比较了解，也可以通过oss返回的详细的错误信息进行排查。
-   日志信息提示日志名称和日志的地址，方便用户查找具体的日志。

**说明：** 

并不是每次错误的检测都能提示出修改建议，对于没有提示修改建议的检测，请根据错误码提示，并结合[oss错误码ErrorCode](https://www.alibabacloud.com/help/doc-detail/32157.htm)进行问题排查。

日志文件

日志文件不同于控制台显示主要是可以查看详细的网络检测过程，ping可以查看到指定的网络是否正常，可以查看到指定的EndPoint的网络是否正常，tracert可以查看到访问EndPoint的路由情况，最后一个nslookup可以查看DNS是否正常。

## 参考资料 {#section_dmw_4zg_wdb .section}

-   [oss错误码列表](https://www.alibabacloud.com/help/doc-detail/32157.htm)
-   [获取Url的方法](../../../../../intl.zh-CN/快速入门/下载文件.md#)

