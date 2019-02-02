# GetBucket \(ListObject\) {#reference_iwr_xlv_tdb .reference}

GetBucket接口用于列举Bucket中所有Object的信息。

## 请求语法 {#section_kmy_mdw_bz .section}

```
GET / HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 请求参数 {#section_bqw_ndw_bz .section}

GetBucket（ListObject）时，可以通过Prefix、Marker、Delimiter以及max-keys对list做限定，获取指定结果。

|名称|类型|是否必需|描述|
|--|--|----|--|
|delimiter|字符串|否|对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现delimiter字符之间的Object作为一组元素（即CommonPrefixes）。默认值：无

|
|marker|字符串|否|设定结果从marker之后按字母排序的第一个开始返回。默认值：无

|
|max-keys|字符串|否|指定返回Object的最大数，默认为100。max-keys取值不能大于1000。默认值：100

如果因为max-keys的设定无法一次完成listing，返回结果会附加一个`<NextMarker>`作为继续listing的marker。

|
|prefix|字符串|否|限定返回的object key必须以prefix作为前缀。注意使用prefix查询时，返回的key中仍会包含prefix。默认值：无

|
|encoding-type|字符串|否|对返回的内容进行编码并指定编码的类型。默认值：无

可选值：url

**说明：** 由于XML 1.0标准不支持解析一些控制字符，比如ASCII值从0到10的字符。对于包含XML 1.0 标准不支持的控制字符，您可以通过指定encoding-type对返回的Delimiter、Marker、Prefix、NextMarker以及 Key进行编码。

|

## 响应元素\(Response Elements\) {#section_cfq_tdw_bz .section}

|名称|类型|描述|
|--|--|--|
|Contents|容器|保存每个返回Object meta的容器。父节点：ListBucketResult

|
|CommonPrefixes|字符串|如果请求中指定了delimiter参数，则在OSS返回的响应中包含CommonPrefixes元素。该元素表明以Delimiter结尾，并有共同前缀的Object名称的集合。父节点：ListBucketResult

|
|Delimiter|字符串|是一个用于对Object名字进行分组的字符。所有名字包含指定的前缀且第一次出现Delimiter字符之间的Object作为一组元素CommonPrefixes。父节点：ListBucketResult

|
|EncodingType|字符串|指明返回结果中编码使用的类型。如果请求的参数中指定了encoding-type，那会对返回结果中的Delimiter、Marker、Prefix、NextMarker和Key这些元素进行编码。父节点：ListBucketResult

|
|DisplayName|字符串|Object 拥有者的名字。父节点：ListBucketResult.Contents.Owner

|
|ETag|字符串|ETag \(entity tag\) 在每个Object生成的时候被创建，用于标示一个Object的内容。父节点：ListBucketResult.Contents

对于PutObject请求创建的Object，ETag值是其内容的MD5值；对于其他方式创建的Object，ETag值是其内容的UUID。ETag值可以用于检查Object内容是否发生变化。不建议用户使用ETag作为Object内容的MD5值校验数据完整性。

|
|ID|字符串|Bucket拥有者的用户ID。父节点：ListBucketResult.Contents.Owner

|
|IsTruncated|枚举字符串|请求中返回的结果是否被截断。 有效值：true、false

-   true 表示本次没有返回全部结果。
-   false 表示本次已经返回了全部结果。

父节点：ListBucketResult

|
|Key|字符串|Object的Key。父节点：ListBucketResult.Contents

|
|LastModified|时间|Object最后被修改的时间。父节点：ListBucketResult.Contents

|
|ListBucketResult|容器|保存GetBucket请求结果的容器。子节点：Name、Prefix、 Marker、MaxKeys、 Delimiter、IsTruncated、Nextmarker、Contents

父节点：None

|
|Marker|字符串|标明这次GetBucket（ListObject）的起点。父节点：ListBucketResult

|
|MaxKeys|字符串|响应请求内返回结果的最大数目。父节点：ListBucketResult

|
|Name|字符串|Bucket名字父节点：ListBucketResult

|
|Owner|容器|保存Bucket拥有者信息的容器。子节点：DisplayName、ID

父节点：ListBucketResult

|
|Prefix|字符串|本次查询结果的前缀。父节点：ListBucketResult

|
|Size|字符串|Object的字节数。父节点：ListBucketResult.Contents

|
|StorageClass|字符串|Object的存储类型，目前只支持 Standard 存储类型。父节点：ListBucketResult.Contents

|

## 细节分析 {#section_vnl_h2w_bz .section}

-   Object中用户自定义的meta，在GetBucket请求时不会返回。
-   如果访问的Bucket不存在，返回404 Not Found错误，错误码：NoSuchBucket。
-   如果没有访问该Bucket的权限，返回403 Forbidden错误，错误码：AccessDenied。
-   在做条件查询时，即使Marker在列表中不存在，也会从符合Marker字母排序的下一个开始打印。如果max-keys小于0或者大于1000，将返回400 Bad Request错误。错误码：InvalidArgument。
-   若Prefix，Marker，Delimiter参数不符合长度要求，返回400 Bad Request错误。错误码：InvalidArgument。
-   Prefix，Marker用来实现分页显示效果，参数的长度必须小于1024字节。
-   如果把Prefix设为某个文件夹名，就可以列举以此Prefix开头的文件，即该文件夹下递归的所有文件和子文件夹。

    在设置Prefix的基础上增加设置Delimiter为 / 时，返回值就只列举该文件夹下的文件，文件夹下的子文件夹名返回在CommonPrefixes中，子文件夹下递归的所有文件和文件夹不显示。

    如一个Bucket中存在三个Object : fun/test.jpg， fun/movie/001.avi，fun/movie/007.avi。若设定Prefix为 fun/，则返回三个Object；如果增加设定Delimiter为 /，则返回 fun/test.jpg 和 fun/movie/ 。


## 示例 {#section_ujb_k2w_bz .section}

-   简单的GetBucket示例

    请求示例：

    ```
    GET / HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
    ```

    返回示例：

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

-   带Prefix参数的示例

    请求示例：

    ```
    GET /?prefix=fun HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:BC+oQIXVR2/ZghT7cGa0ykboO4M=
    ```

    返回示例：

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

-   带Prefix和Delimiter参数的示例

    请求示例：

    ```
    GET /?prefix=fun/&delimiter=/ HTTP/1.1
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Date: Fri, 24 Feb 2012 08:43:27 GMT
    Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
    ```

    返回示例：

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


