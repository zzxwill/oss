# GetBucket \(ListObjects\) {#reference_iwr_xlv_tdb .reference}

GetBucket接口用于列举存储空间（Bucket）中所有文件（Object）的信息。

**说明：** Object中自定义的元信息，在GetBucket请求时不会返回。

## 请求语法 {#section_kmy_mdw_bz .section}

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求参数 {#section_bqw_ndw_bz .section}

|名称|类型|是否必选|描述|
|:-|:-|:---|:-|
|Delimiter|字符串|否| 对Object名字进行分组的字符。所有Object名字包含指定的前缀，第一次出现Delimiter字符之间的Object作为一组元素（即CommonPrefixes）。

 默认值：无

 |
|Marker|字符串|否| 设定从Marker之后按字母排序开始返回Object。

 Marker用来实现分页显示效果，参数的长度必须小于1024字节。

 在做条件查询时，即使Marker在列表中不存在，也会从符合Marker字母排序的下一个开始打印。

 默认值：无

 |
|Max-keys|字符串|否| 指定返回Object的最大数。

 取值：大于0小于1000

 默认值：100

 **说明：** 如果因为Max-keys的设定无法一次完成列举，返回结果会附加一个`<NextMarker>`作为下一次列举的Marker。

 |
|Prefix|字符串|否| 限定返回文件的Key必须以Prefix作为前缀。

 如果把Prefix设为某个文件夹名，就可以列举以此Prefix开头的文件，即该文件夹下递归的所有文件和子文件夹。

 在设置Prefix的基础上增加设置Delimiter为 `/` 时，返回值就只列举该文件夹下的文件，文件夹下的子文件夹名返回在CommonPrefixes中，子文件夹下递归的所有文件和文件夹不显示。

 例如，一个Bucket中存在三个Object : fun/test.jpg， fun/movie/001.avi，fun/movie/007.avi。若设定Prefix为 fun/，则返回三个Object；如果增加设定Delimiter为 /，则返回fun/test.jpg和fun/movie/ 。

 **说明：** 

-   参数的长度必须小于1024字节。

-   使用Prefix查询时，返回的Key中仍会包含Prefix。

 默认值：无

 |
|Encoding-type|字符串|否| 对返回的内容进行编码并指定编码的类型。

 默认值：无

 可选值：url

 **说明：** Delimiter、Marker、Prefix、NextMarker以及Key使用UTF-8字符。如果Delimiter、Marker、Prefix、NextMarker以及Key中包含XML 1.0标准不支持的控制字符，您可以通过指定Encoding-type对返回结果中的Delimiter、Marker、Prefix、NextMarker以及Key进行编码。

 |

## 响应元素 {#section_cfq_tdw_bz .section}

|名称|类型|描述|
|--|--|--|
|Contents|容器| 保存每个返回Object元信息的容器。

 父节点：ListBucketResult

 |
|CommonPrefixes|字符串| 如果请求中指定了Delimiter参数，则会在返回的响应中包含CommonPrefixes元素。该元素表明以Delimiter结尾，并有共同前缀的Object名称的集合。

 父节点：ListBucketResult

 |
|Delimiter|字符串| 对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现Delimiter字符之间的Object作为一组元素CommonPrefixes。

 父节点：ListBucketResult

 |
|EncodingType|字符串| 指明了返回结果中编码使用的类型。如果请求的参数中指定了Encoding-type，那会对返回结果中的Delimiter、Marker、Prefix、NextMarker和Key这些元素进行编码。

 父节点：ListBucketResult

 |
|DisplayName|字符串| Object 拥有者的名字。

 父节点：ListBucketResult.Contents.Owner

 |
|ETag|字符串| ETag \(entity tag\) 在每个Object生成的时创建，用于标示一个Object的内容。

 父节点：ListBucketResult.Contents

 对于PutObject请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议您使用ETag来作为Object内容的MD5校验数据完整性。

 |
|ID|字符串| Bucket拥有者的用户ID。

 父节点：ListBucketResult.Contents.Owner

 |
|IsTruncated|枚举字符串| 请求中返回的结果是否被截断。

 返回值：true、false

-   true表示本次没有返回全部结果。
-   false表示本次已经返回了全部结果。

 父节点：ListBucketResult

 |
|Key|字符串| Object的Key。

 父节点：ListBucketResult.Contents

 |
|LastModified|时间| Object最后被修改的时间。

 父节点：ListBucketResult.Contents

 |
|ListBucketResult|容器| 保存GetBucket请求结果的容器。

 子节点：Name、Prefix、 Marker、MaxKeys、 Delimiter、IsTruncated、Nextmarker、Contents

 父节点：None

 |
|Marker|字符串| 标明这次GetBucket（ListObjects）的起点。

 父节点：ListBucketResult

 |
|MaxKeys|字符串| 响应请求内返回结果的最大数目。

 父节点：ListBucketResult

 |
|Name|字符串| Bucket名字。

 父节点：ListBucketResult

 |
|Owner|容器| 保存Bucket拥有者信息的容器。

 子节点：DisplayName、ID

 父节点：ListBucketResult

 |
|Prefix|字符串| 本次查询结果的前缀。

 父节点：ListBucketResult

 |
|Size|字符串| Object的字节数。

 父节点：ListBucketResult.Contents

 |
|StorageClass|字符串| Object的存储类型。

 父节点：ListBucketResult.Contents

 |

## 示例 {#section_ujb_k2w_bz .section}

-   **简单示例**

    请求示例

    ```
    GET / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1866
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix></Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>fun/movie/001.avi</Key>
            <LastModified>2012-02-24T08:43:07.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user-example</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>fun/movie/007.avi</Key>
            <LastModified>2012-02-24T08:43:27.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user-example</DisplayName>
            </Owner>
        </Contents>
    <Contents>
            <Key>fun/test.jpg</Key>
            <LastModified>2012-02-24T08:42:32.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user-example</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>oss.jpg</Key>
            <LastModified>2012-02-24T06:07:48.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user-example</DisplayName>
            </Owner>
        </Contents>
    </ListBucketResult>
    ```

-   **带Prefix参数**

    请求示例

    ```
    GET /?prefix=fun HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 1464
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix>fun</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter></Delimiter>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>fun/movie/001.avi</Key>
            <LastModified>2012-02-24T08:43:07.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user_example</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>fun/movie/007.avi</Key>
            <LastModified>2012-02-24T08:43:27.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user_example</DisplayName>
            </Owner>
        </Contents>
        <Contents>
            <Key>fun/test.jpg</Key>
            <LastModified>2012-02-24T08:42:32.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user_example</DisplayName>
            </Owner>
        </Contents>
    </ListBucketResult>
    ```

-   **带Prefix和Delimiter参数**

    请求示例

    ```
    GET /?prefix=fun/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
    ```

    返回示例

    ```
    HTTP/1.1 200 OK
    x-oss-request-id: 534B371674E88A4D8906008B
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Content-Type: application/xml
    Content-Length: 712
    Connection: keep-alive
    Server: AliyunOSS
    <?xml version="1.0" encoding="UTF-8"?>
    <ListBucketResult xmlns=”http://doc.oss-cn-hangzhou.aliyuncs.com”>
    <Name>oss-example</Name>
    <Prefix>fun/</Prefix>
    <Marker></Marker>
    <MaxKeys>100</MaxKeys>
    <Delimiter>/</Delimiter>
        <IsTruncated>false</IsTruncated>
        <Contents>
            <Key>fun/test.jpg</Key>
            <LastModified>2012-02-24T08:42:32.000Z</LastModified>
            <ETag>&quot;5B3C1A2E053D763E1B002CC607C5A0FE&quot;</ETag>
            <Type>Normal</Type>
            <Size>344606</Size>
            <StorageClass>Standard</StorageClass>
            <Owner>
                <ID>00220120222</ID>
                <DisplayName>user_example</DisplayName>
            </Owner>
        </Contents>
       <CommonPrefixes>
            <Prefix>fun/movie/</Prefix>
       </CommonPrefixes>
    </ListBucketResult>
    ```


## SDK {#section_egl_m2c_5gb .section}

此接口所对应的各语言SDK如下：

-   [Java](../../../../../cn.zh-CN/SDK 参考/Java/管理文件/列举文件.md)
-   [Python](../../../../../cn.zh-CN/SDK 参考/Python/管理文件/列举文件.md)
-   [PHP](../../../../../cn.zh-CN/SDK 参考/PHP/管理文件/列举文件.md)
-   [Go](../../../../../cn.zh-CN/SDK 参考/Go/管理文件/列举文件.md)
-   [C](../../../../../cn.zh-CN/SDK 参考/C/管理文件/列举文件.md)
-   [.NET](../../../../../cn.zh-CN/SDK 参考/.NET/管理文件/列举文件.md)
-   [Android](../../../../../cn.zh-CN/SDK 参考/Android/管理文件/列举文件.md)
-   [Node.js](../../../../../cn.zh-CN/SDK 参考/Node.js/管理文件.md)
-   [Browser.js](../../../../../cn.zh-CN/SDK 参考/Browser.js/管理文件.md)
-   [Ruby](../../../../../cn.zh-CN/SDK 参考/Ruby/管理文件.md)

## 错误码 {#section_dsv_grs_qgb .section}

|错误码|HTTP 状态码|描述|
|:--|:-------|:-|
|NoSuchBucket|404|目标Bucket不存在。|
|AccessDenied|403|没有访问该Bucket的权限。|
|InvalidArgument|400| -   Max-keys小于0或者大于1000。
-   Prefix，Marker，Delimiter参数的长度不符合要求。

 |

