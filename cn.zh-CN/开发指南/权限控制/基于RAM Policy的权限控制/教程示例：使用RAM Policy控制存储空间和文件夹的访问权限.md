# 教程示例：使用RAM Policy控制存储空间和文件夹的访问权限 {#concept_rf2_251_5db .concept}

本教程示例详细演示了如何使用 RAM Policy 控制用户对 OSS 存储空间和文件夹的访问。

在示例中，我们首先创建一个存储空间和文件夹，然后使用阿里云主账号创建访问管理（RAM）用户，并通过构建不同的 RAM Policy 为这些用户授予对所创建 OSS 存储空间及文件夹的增量权限。

## 存储空间和文件夹的基本概念 {#section_ppc_k13_5db .section}

阿里云 OSS 的数据模型为扁平型结构，所有文件都直接隶属于其对应的存储空间。因此，OSS 缺少文件系统中类似于目录与子文件夹的层次结构。但是，您可以在 OSS 控制台上模拟文件夹层次结构。在该控制台中，您可以按文件夹对相关文件进行分组、分类和管理，如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923403896_zh-CN.png)

OSS 提供使用键值（key）对格式的分布式对象存储服务。用户根据其唯一的key（对象名）检索对象的内容。例如，名为 example-company 的存储空间有三个文件夹：Development、 Marketing 和 Private，以及一个对象 oss-dg.pdf。

-   在创建 Development 文件夹时，控制台会创建一个 key 为`Development/`的对象。注意，文件夹的 key 包括分隔符 `/`。
-   当您将名为 ProjectA.docx 的对象上传到 Development 文件夹中时，控制台会上传该对象并将其key设置为`Development/ProjectA.docx`。

    在该key中，`Development`为前缀，而`/`为分隔符。您可以从存储空间中获取具有特定前缀和分隔符的所有对象的列表。在控制台中，单击 Development 文件夹时，控制台会列出文件夹中的对象，如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923401187_zh-CN.png)

    **说明：** 当控制台列出 example-company 存储空间中的 Development 文件夹时，它会向 OSS 发送一个用于指定前缀 `Development` 和分隔符`/`的请求。控制台的响应与文件系统类似，会显示文件夹列表。上例说明，存储空间 example-company 有三个对象，其 key 分别为 `Development/Alibaba Cloud.pdf`、`Development/ProjectA.docx` 及 `Development/ProjectB.docx`。


控制台通过对象的 key 推断逻辑层次结构。当您创建对象的逻辑层次结构时，您可以管理对个别文件夹的访问，如本教程后面描述的那样。

在本教程开始之前，您还需要知道“根级”存储空间内容的概念。假设 example-company 存储空间包含以下对象：

-   Development/Alibaba Cloud.pdf
-   Development/ProjectA.docx
-   Development/ProjectB.docx
-   Marketing/data2016.xlsx
-   Marketing/data2016.xlsx
-   Private/2017/images.zip
-   Private/2017/promote.pptx
-   oss-dg.pdf

这些对象的key构建了一个以 Development、Marketing 和 Private作为根级文件夹并以 oss-dg.pdf 作为根级对象的逻辑层次结构。当您单击 OSS 控制台中的存储空间名时，控制台会将一级前缀和一个分隔符（Development/、Marketing/ 和 Private/）显示为根级文件夹。对象 oss-dg.pdf 没有前缀，因此显示为根级别项。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923401188_zh-CN.png)

## OSS 的请求和响应逻辑 {#section_isl_bc3_5db .section}

在授予权限之前，我们需要清楚，当用户单击某个存储空间的名字时控制台向 OSS 发送的是什么请求、OSS 返回的是什么响应，以及控制台如何解析该响应。

当用户单击某个存储空间名时，控制台会将 [GetBucket](../../../../../cn.zh-CN/API 参考/关于Bucket的操作/GetBucket (ListObjects).md#) 请求发送至 OSS。此请求包括以下参数：

-   `prefix`，其值为空字符串。
-   `delimiter`，其值为`/`。

请求示例如下所示：

```
GET /?prefix=&delimiter=/ HTTP/1.1
Host: example-company.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

OSS 返回的响应包括`ListBucketResult`元素：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 712
Connection: keep-alive
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
<Name>example-company</Name>
<Prefix></Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>oss-dg.pdf</Key>
        ...
    </Contents>
   <CommonPrefixes>
        <Prefix>Development</Prefix>
   </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Marketing</Prefix>
   </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Private</Prefix>
   </CommonPrefixes>
</ListBucketResult>
```

由于oss-dg.pdf不包含`/`分隔符，因此 OSS 在`<Contents/>`元素中返回该 key。存储空间 example-company 中的所有其他 key 都包含`/`分隔符，因此 OSS 会将这些 key 分组，并为每个前缀值 Development/、Marketing/和 Private/返回一个 `<CommonPrefixes/>`元素。该元素是一个字符串，包含从这些 key 的第一个字符开始到第一次出现指定的`/`分隔符之间的字符。

控制台会解析此结果并显示如下的根级别项：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923411189_zh-CN.png)

现在，如果用户单击 Development 文件夹，控制台会将 [GetBucket](../../../../../cn.zh-CN/API 参考/关于Bucket的操作/GetBucket (ListObjects).md#) 请求发送至 OSS。此请求包括以下参数：

-   `prefix`，其值为 `Development/`。
-   `delimiter`，其值为`/`。

请求示例如下所示：

```
GET /?prefix=Development/&delimiter=/ HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

作为响应，OSS 返回以指定前缀开头的 key：

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 712
Connection: keep-alive
Server: AliyunOSS
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
<Name>example-company</Name>
<Prefix>Development/</Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>ProjectA.docx</Key>
        ...
    </Contents>
    <Contents>
        <Key>ProjectB.docx</Key>
        ...
    </Contents>
</ListBucketResult>
```

控制台会解析此结果并显示如下的 key：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923411190_zh-CN.png)

## 教程示例 {#section_zr5_fd3_5db .section}

本教程示例如下所示：

-   创建一个存储空间 example-company，然后向其中添加三个文件夹（Development、Marketing 和 Private）。
-   您有 Anne 和 Leo 两个用户。您希望 Anne 只能访问 Development 文件夹而 Leo 则只能访问 Marketing 文件夹，并且希望将 Private 文件夹保持私有。在教程示例中，通过创建访问控制（RAM）用户（Anne 和 Leo）来管理访问权限，并授予他们必要的权限。
-   RAM 还支持创建用户组并授予适用于组中所有用户的组级别权限。这有助于更好地管理权限。在本示例中，Anne 和 Leo 都需要一些公共权限。因此，您还要创建一个名为 Staff 的组，然后将 Anne 和 Leo 添加到该组中。首先，您需要给该组分配策略授予权限。然后，将策略分配给特定用户，添加特定用户的权限。

**说明：** 本教程示例使用 example-company 作为存储空间名、使用 Anne 和 Leo 作为 RAM 用户名并使用 Staff 作为组名。由于阿里云 OSS 要求存储空间名全局唯一，所以您需要用自己的存储空间名称替换本教程中的存储空间名。

## 示例准备 {#section_qqx_3d3_5db .section}

本示例使用阿里云主账号创建 RAM 用户。最初，这些用户没有任何权限。您将逐步授予这些用户执行特定 OSS 操作的权限。为了测试这些权限，您需要使用每个用户的 RAM 账号登录到控制台。当您作为主账号所有者逐步授予权限并作为 RAM 用户测试权限时，您需要每次使用不同账号进行登录和注销。您可以使用一个浏览器来执行此测试。如果您可以使用两个不同的浏览器，则该测试过程用时将会缩短：一个浏览器用于使用主账号连接到阿里云控制台，另一个浏览器用于使用 RAM 账号进行连接。

要使用您的主账号登录到阿里云控制台。 RAM 用户不能使用相同的链接登录。他们必须使用 RAM 用户登录链接。作为主账号所有者，您可以向 RAM 用户提供此链接。

**说明：** 有关 RAM 的详细信息，请参见[使用 RAM 用户账号登录](../../../../../cn.zh-CN/快速入门/RAM 用户登录控制台.md#)。

为 RAM 用户提供登录链接：

1.  使用主账号登录 [RAM 控制台](https://ram.console.aliyun.com)。
2.  在左侧导航栏中，单击**概览**。
3.  在**账号管理**区域，将 RAM 用户登录地址的 URL 提供给 RAM 用户，以便其使用 RAM 用户名和密码登录控制台。

## 步骤 1：创建存储空间 {#section_a4f_5d3_5db .section}

在此步骤中，您可以使用主账号登录到 OSS 控制台、创建存储空间、将文件夹（Development、Marketing、Private）添加到存储空间中，并在每个文件夹中上传一个或两个示例文档。

1.  使用主账号登录 [OSS 控制台](https://oss.console.aliyun.com/)。
2.  创建名为 **example-company** 的存储空间。

    有关详细过程，请参见 OSS 控制台用户指南中的[创建存储空间](../../../../../cn.zh-CN/控制台用户指南/管理存储空间/创建存储空间.md#) 。

3.  将一个文件上传到存储空间中。

    本示例假设您将文件 oss-dg.pdf 上传到存储空间的根级别。您可以用不同的文件名上传自己的文件。

    有关详细过程，请参见 OSS 控制台用户指南中的[上传文件](../../../../../cn.zh-CN/控制台用户指南/管理文件/上传文件.md#)。

4.  添加名为 Development、Marketing和 Private的三个文件夹。

    有关详细过程，请参见 OSS 控制台用户指南 中的[创建文件夹](../../../../../cn.zh-CN/控制台用户指南/管理文件/新建文件夹.md#)。

5.  将一个或两个文件上传到每个文件夹中。

    本例假设您将具有以下对象键的对象上传到存储空间中：

    -   Development/Alibaba Cloud.pdf
    -   Development/ProjectA.docx
    -   Development/ProjectB.docx
    -   Marketing/data2016.xlsx
    -   Marketing/data2016.xlsx
    -   Private/2017/images.zip
    -   Private/2017/promote.pptx
    -   oss-dg.pdf

## 步骤 2：创建 RAM 用户和组 {#section_vzz_xd3_5db .section}

在此步骤中，您使用 RAM 控制台将两个 RAM 用户 Anne 和 Leo 添加到主账号中。您还将创建一个名为 Staff 的组，然后将这两个用户添加到该组中。

**说明：** 在此步骤中，不要分配任何授予这些用户权限的策略。在以下步骤中，您将逐步为其授予权限。

有关创建 RAM 用户的详细过程，请参见 RAM 快速入门中的[创建 RAM 用户](../../../../../cn.zh-CN/快速入门/创建 RAM 用户.md#)。请为每个 RAM 用户创建登录密码。

有关创建组的详细过程，请参见 RAM 用户指南中的[创建组](../../../../../cn.zh-CN/用户指南/身份管理/组.md#)。

## 步骤 3：确认 RAM 用户没有任何权限 {#section_izl_b23_5db .section}

如果您使用两个浏览器，现在可以在另一个浏览器中使用其中一个 RAM 用户账号登录到控制台。

1.  打开 RAM 用户登录链接，并用 Anne 或 Leo 的账号登录到 RAM 控制台。
2.  打开 OSS 控制台。

    您发现控制台中没有任何存储空间，这意味着 Anne 不具有对存储空间 example-company 的任何权限。


## 步骤 4：授予组级别权限 {#section_xwr_223_5db .section}

我们希望 Anne 和 Leo 都能执行以下操作：

-   列出主账号所拥有的所有存储空间。

    为此，Anne 和 Leo 必须具有执行 `oss:ListBuckets` 操作的权限。

-   列出 example-company 存储空间中的根级别项、文件夹和对象。

    为此，Anne 和 Leo 必须具有对 example-company 存储空间执行 `oss:ListObjects` 操作的权限。


步骤 4.1.授予列出所有存储空间的权限

在此步骤中，创建一个授予用户最低权限的策略。凭借最低权限，用户可列出主账号所拥有的所有存储空间。您还将此策略分配给 Staff 组，以便授予获得主账号拥有的存储空间列表的组权限。

1.  使用主账号登录 [RAM 控制台](https://ram.console.aliyun.com)。
2.  创建策略 AllowGroupToSeeBucketListInConsole。
    1.  在左侧导航窗格中，单击**权限管理** \> **权限策略管理** \> **新建权限策略**。
    2.  在**策略名称**字段中，输入 AllowGroupToSeeBucketListInConsole。
    3.  选择**配置模式**为脚本配置。
    4.  在**策略内容**字段中，复制并粘贴以下策略。

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets"
           ],
           "Resource": [
             "acs:oss:*:*:*"
           ]
         }
        ]
        }
        ```

        **说明：** 

        -   RAM 策略为 JSON 格式。各字段定义如下：
            -   Statement：对象数组，每个对象使用名/值对的集合来描述权限。
            -   Effect：决定是允许还是拒绝特定的权限。
            -   Action：指定访问权限的类型。在本策略中，`oss:ListBuckets` 是预定义的 OSS 操作，可返回经过身份验证的发送者所拥有的所有储存空间的列表。
        -   推荐您使用 [RAM 策略编辑器](../../../../../cn.zh-CN/常用工具/RAM策略编辑器.md#)快速生成 RAM 策略。更详细的 RAM 策略介绍请参见[如何构建 RAM Policy](cn.zh-CN/开发指南/权限控制/基于RAM Policy的权限控制/如何构建RAM Policy.md#)。
3.  将 `AllowGroupToSeeBucketListInConsole` 策略分配给 Staff 组。

    有关分配策略的详细过程，请参见 RAM 快速入门[为 RAM 用户授权](../../../../../cn.zh-CN/快速入门/为 RAM 用户授权.md#)中的“为用户所属的用户组授权” 。

    可以将策略分配给 RAM 控制台中的 RAM 用户和组。在本例中，我们将策略分配给组，因为我们希望 Anne 和 Leo 都能够列出这些存储空间。

4.  测试权限。
    1.  打开 RAM 用户登录链接，并用 Anne 或 Leo 的账号登录到 RAM 控制台。
    2.  打开 OSS 控制台。

        控制台列出所有存储空间。

    3.  单击 example-company 存储空间，然后单击**文件**选项卡。

        此时将显示一个消息框，表明您没有相应的访问权限。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923411193_zh-CN.png)


步骤 4.2.授予列出存储空间根级内容的权限

在此步骤中，您授予权限，允许所有用户列出存储空间 example-company 中的所有项目。当用户在 OSS 控制台中单击 example-company 时，能够看到存储空间中的根级别项。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923411195_zh-CN.png)

1.  使用主账号登录[RAM 控制台](https://ram.console.aliyun.com)。
2.  用以下策略取代分配给 Staff 组的现有策略 `AllowGroupToSeeBucketListInConsole`，该策略还允许 `oss:ListObjects` 操作。请用您的存储空间名替换策略资源中的 example-company。

    有关详细过程，请参见 RAM 用户指南中[授权策略](../../../../../cn.zh-CN/用户指南/授权管理/授权策略管理.md#)的“修改自定义授权策略”部分。注意，您最多可对 RAM 策略进行五次修改。如果超过了五次，则需要删除该策略并创建一个新的策略，然后再次将新策略分配给 Staff 组。

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketAcl"
           ],
           "Resource": [
             "acs:oss:*:*:*"
           ],
           "Condition": {}
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 ""
               ],
               "oss:Delimiter": [
                 "/"
               ]
             }
           }
         }
       ]
     }
    ```

    **说明：** 

    -   要列出存储空间内容，用户需要调用 `oss:ListObjects` 操作的权限。为了确保用户仅看到根级内容，我们添加了一个条件：用户必须在请求中指定一个空前缀，也就是说，他们不能单击任何根级文件夹。我们还通过要求用户请求包含分隔符参数和值 `/` 来添加需要文件夹样式访问的条件。
    -   当用户登录到 OSS 控制台时，控制台检查用户的身份是否有访问 OSS 服务的权限。要在控制台中支持存储空间操作，我们还需要添加 `oss:GetBucketAcl` 操作。
3.  测试更新的权限。
    1.  打开 RAM 用户登录链接，并用 Anne 或 Leo 的账号登录到 RAM 控制台。
    2.  打开 OSS 控制台。

        控制台列出所有存储空间。

    3.  单击 example-company 存储空间，然后单击**文件**选项卡。

        控制台列出所有根级别项。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/15535923411196_zh-CN.png)

    4.  单击任何文件夹或对象 oss-dg.pdf。

        此时将显示一个消息框，表明您没有相应的访问权限。


组策略摘要

添加组策略的最终结果是授予 RAM 用户 Anne 和 Leo 以下最低权限：

-   列出主账号所拥有的所有存储空间。
-   查看 example-company 存储空间中的根级别项。

然而，他们可以进行的操作仍然有限。在以下部分中，我们将授予用户以下特定权限：

-   允许 Anne 在 Development 文件夹中获取和放入对象。
-   允许 Leo 在Finance文件夹中获取和放入对象。

对于用户特定的权限，您需要将策略分配给特定用户，而非分配给组。以下部分授予 Anne 在 Development 文件夹中操作的权限。您可以重复这些步骤，授予 Leo 在 Finance 文件夹中进行类似操作的权限。

## 步骤 5：授予 RAM 用户 Anne 特定权限 {#section_f3l_wf3_5db .section}

在此步骤中，我们向 Anne 授予额外的权限，使她可以看到 Development 文件夹的内容，并将对象放入文件夹中。

步骤 5.1.授予 RAM 用户 Anne 权限以列出 Development 文件夹内容

若要 Anne 能够列出 Development 文件夹内容，您必须为其分配策略。该策略必须能够授予其对 example-company 存储空间执行 `oss:ListObjects` 操作的权限，还必须包括要求用户在请求中指定前缀 Development/ 的条件。

1.  使用主账号登录[RAM 控制台](https://ram.console.aliyun.com)。
2.  创建策略 `AllowListBucketsIfSpecificPrefixIsIncluded`，授予 RAM 用户 Anne 权限以列出 Development 文件夹内容。
    1.  在左侧导航窗格中，单击**权限管理** \> **权限策略管理** \> **新建权限策略**。
    2.  在**策略名称**字段中，输入 AllowGroupToSeeBucketListInConsole。
    3.  选择**配置模式**为脚本配置。
    4.  在**策略内容**字段中，复制并粘贴以下策略。

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Development/*"
               ]
             }
           }
         }
        ]
        }
        ```

3.  将策略分配给 RAM 用户 Anne。

    有关分配策略的详细过程，请参见 RAM 快速入门中的[为 RAM 用户授权](../../../../../cn.zh-CN/快速入门/为 RAM 用户授权.md#)。

4.  测试 Anne 的权限。
    1.  打开 RAM 用户登录链接，并用 Anne 的账号登录到 RAM 控制台。
    2.  打开 OSS 控制台，控制台列出所有存储空间。
    3.  单击 example-company 存储空间，然后单击**文件**选项卡，控制台列出所有根级别项。
    4.  单击 Development/ 文件夹。控制台列出文件夹中的对象。

步骤 5.2 授予 RAM 用户 Anne 在 Development 文件夹中获取和放入对象的权限。

若要 Anne 能够在 Development 文件夹中获取和放入对象，您必须授予她调用 `oss:GetObject` 和`oss:PutObject` 操作的权限，包括用户必须在请求中指定前缀 Development/ 的条件。

1.  使用主账号登录 [RAM 控制台](https://ram.console.aliyun.com)。
2.  用以下策略取代您在之前步骤中创建的策略 `AllowListBucketsIfSpecificPrefixIsIncluded`。

    有关详细过程，请参见 RAM 用户指南中[授权策略](../../../../../cn.zh-CN/用户指南/授权管理/授权策略管理.md#)的“修改自定义授权策略”部分。注意，您最多可对 RAM 策略进行五次修改。如果超过了五次，则需要删除该策略并创建一个新的策略，然后再次将新策略分配给 Staff 组。

    ```
    {
       "Version": "1",
       "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Development/*"
               ]
             }
           }
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",
             "oss:PutObject",
             "oss:GetObjectAcl"
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Development/*"
           ],
           "Condition": {}
         }
       ]
     }
    ```

    **说明：** 当用户登录到 OSS 控制台时，控制台检查用户的身份是否有访问 OSS 服务的权限。要在控制台中支持存储空间操作，我们还需要添加 `oss:GetObjectAcl` 操作。

3.  测试更新的策略。
    1.  打开 RAM 用户登录链接，并用 Anne 的账号登录到 RAM 控制台。
    2.  打开 OSS 控制台。

        控制台列出所有存储空间。

    3.  在 OSS 控制台中，确认 Anne 现在可以在 Development 文件夹中添加对象并下载对象。

步骤 5.3 显式拒绝 RAM 用户 Anne 访问存储空间中任何其他文件夹的权限

RAM 用户 Anne 现在可以在 example-company 存储空间中列出根级内容，并将对象放入 Development 文件夹中。如果要严格限制访问权限，您可以显式拒绝 Anne 对存储空间中任何其他文件夹的访问。如果有授予 Anne 访问存储空间中任何其他文件夹的其他策略，则此显式策略将替代这些权限。

您可以将以下语句添加到 RAM 用户 Anne 的策略 `AllowListBucketsIfSpecificPrefixIsIncluded`，以要求 Anne 发送到 OSS 的所有请求包含前缀参数，该参数的值可以是 Development/\* 或空字符串。

```
{
      "Effect": "Deny",
      "Action": [
        "oss:ListObjects"
      ],
      "Resource": [
        "acs:oss:*:*:example-company"
      ],
      "Condition": {
        "StringNotLike": {
          "oss:Prefix": [
            "Development/*",
            ""
          ]
        }
      }
    }
```

按照之前的步骤更新您为 RAM 用户 Anne 创建的策略 `AllowListBucketsIfSpecificPrefixIsIncluded`。复制并粘贴以下策略以替换现有策略。

```
{
  "Version": "1",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "oss:ListObjects"
      ],
      "Resource": [
        "acs:oss:*:*:example-company"
      ],
      "Condition": {
        "StringLike": {
          "oss:Prefix": [
            "Development/*"
          ]
        }
      }
    },
    {
      "Effect": "Allow",
      "Action": [
        "oss:GetObject",
        "oss:PutObject",
        "oss:GetObjectAcl"
      ],
      "Resource": [
        "acs:oss:*:*:example-company/Development/*"
      ],
      "Condition": {}
    },
    {
      "Effect": "Deny",
      "Action": [
        "oss:ListObjects"
      ],
      "Resource": [
        "acs:oss:*:*:example-company"
      ],
      "Condition": {
        "StringNotLike": {
          "oss:Prefix": [
            "Development/*",
            ""
          ]
        }
      }
    }
  ]
}
```

## 步骤 6：授予 RAM 用户 Leo 特定权限 {#section_sfj_hh3_5db .section}

现在，您希望授予 Leo 访问 Marketing 文件夹的权限。请遵循之前用于向 Anne 授予权限的步骤，但应将 Development 文件夹替换为 Marketing 文件夹。有关详细过程，请参见步骤 5：授予 RAM 用户 Anne 特定权限。

## 步骤 7：确保 Private 文件夹安全 {#section_m13_3h3_5db .section}

在本例中，您仅拥有两个用户。您在组级别授予两个用户所有所需的最小权限，只有当您真正需要单个用户级别上的权限时，才授予用户级别权限。此方法有助于最大限度地减少管理权限的工作量。随着用户数量的增加，我们希望确保不意外地授予用户对 Private文件夹的权限。因此，我们需要添加一个显式拒绝访问 Private 文件夹的策略。显式拒绝策略会取代任何其他权限。若要确保Private 文件夹保持私有，可以向组策略添加以下两个拒绝语句：

-   添加以下语句以显式拒绝对 Private 文件夹 \(example-company/Private/\*\) 中的资源执行任何操作。

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:*"
        ],
        "Resource": [
          "acs:oss:*:*:example-company/Private/*"
        ],
        "Condition": {}
      }
    ```

-   您还要在请求指定了 Private/prefix 时拒绝执行 `ListObjects` 操作的权限。在控制台中，如果 Anne 或 Leo 单击 Private 文件夹，则此策略将导致 OSS 返回错误响应。

    ```
    {
        "Effect": "Deny",
        "Action": [
          "oss:ListObjects"
        ],
        "Resource": [
          "acs:oss:*:*:*"
        ],
        "Condition": {
          "StringLike": {
            "oss:Prefix": [
              "Private/"
            ]
          }
        }
      }
    ```

-   用包含前述拒绝语句的更新策略取代 Staff 组策略 `AllowGroupToSeeBucketListInConsole`。在应用更新策略后，组中的任何用户都不能访问您的存储空间中的 Private 文件夹。

    1.  使用主账号登录[RAM 控制台](https://ram.console.aliyun.com)。
    2.  用以下策略取代分配给 Staff 组的现有策略 `AllowGroupToSeeBucketListInConsole`。请用您的存储空间名替换策略资源中的 example-company。

        ```
        {
        "Version": "1",
        "Statement": [
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketAcl"
           ],
           "Resource": [
             "acs:oss:*:*:*"
           ],
           "Condition": {}
         },
         {
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:example-company"
           ],
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                  ""
                ],
               "oss:Delimiter": [
                 "/"
               ]
             }
           }
         },
         {
           "Effect": "Deny",
           "Action": [
             "oss:*"
           ],
           "Resource": [
             "acs:oss:*:*:example-company/Private/*"
           ],
           "Condition": {}
         },
         {
           "Effect": "Deny",
           "Action": [
             "oss:ListObjects"
           ],
           "Resource": [
             "acs:oss:*:*:*"
           ],
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Private/"
               ]
             }
           }
         }
        ]
        }
        ```


## 清理 {#section_fkk_c33_5db .section}

要进行清理，您需要在 RAM 控制台中删除用户 Anne 和 Leo。

有关详细过程，请参见 RAM 用户指南中[用户](../../../../../cn.zh-CN/用户指南/身份管理/用户.md#)的“删除 RAM 用户”部分。

为了确保您不再因存储而继续被收取费用，您还需要删除为本示例创建的对象和存储空间。

