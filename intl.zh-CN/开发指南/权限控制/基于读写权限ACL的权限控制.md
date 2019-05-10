# 基于读写权限ACL的权限控制 {#concept_blw_yqm_2gb .concept}

OSS 为权限控制提供访问控制列表（ACL）。ACL 是授予 Bucket 和 Object 访问权限的访问策略。 您可以在创建存储空间（Bucket）或上传文件（Object）时配置 ACL，也可以在创建 Bucket 或上传 Object 后的任意时间内修改 ACL。

**说明：** 基于读写权限 ACL 的 OSS API 接口详细信息请参考：

-   设置 Bucket ACL：[PutBucketACL](../../../../intl.zh-CN/API 参考/关于Bucket的操作/PutBucketACL.md#)
-   获取 Bucket ACL：[GetBucketACL](../../../../intl.zh-CN/API 参考/关于Bucket的操作/GetBucketAcl.md#)
-   设置 Object ACL：[PutObjectACL](../../../../intl.zh-CN/API 参考/关于Object操作/PutObjectACL.md#)
-   获取 Object ACL：[GetObjectACL](../../../../intl.zh-CN/API 参考/关于Object操作/GetObjectACL.md#)

## Bucket ACL {#section_dr5_nvm_2gb .section}

-   Bucket ACL 介绍

    Bucket ACL是 Bucket 级别的权限访问控制。目前有三种访问权限：public-read-write，public-read 和 private，含义如下：

    |权限值|中文名称|权限对访问者的限制|
    |:--|:---|:--------|
    |public-read-write|公共读写|任何人（包括匿名访问）都可以对该 Bucket 中的 Object 进行读/写/删除操作；所有这些操作产生的费用由该 Bucket 的 Owner 承担，请慎用该权限。|
    |public-read|公共读，私有写|只有该 Bucket 的 Owner 或者授权对象可以对存放在其中的 Object 进行写/删除操作；任何人（包括匿名访问）可以对 Object 进行读操作。|
    |private|私有读写|只有该 Bucket 的 Owner 或者授权对象可以对存放在其中的 Object 进行读/写/删除操作；其他人在未经授权的情况下无法访问该 Bucket 内的 Object。|

-   操作方式

    |操作方式|说明|
    |----|--|
    |[控制台](../../../../intl.zh-CN/控制台用户指南/管理存储空间/修改存储空间读写权限.md#)|Web应用程序，直观易用|
    |[图形化工具ossbrowser](../../../../intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)|图形化工具，易操作|
    |[命令行工具ossutil](../../../../intl.zh-CN/常用工具/命令行工具ossutil/有关Bucket的命令.md#ul_imw_f5s_vdb)|命令行工具，性能好|
    |[Java SDK](../../../../intl.zh-CN/SDK 示例/Java/存储空间.md#table_grn_jjf_2gb)|丰富、完整的各类语言SDK demo|
    |[Python SDK](../../../../intl.zh-CN/SDK 示例/Python/存储空间.md#section_rvh_l1j_kfb)|
    |[PHP SDK](../../../../intl.zh-CN/SDK 示例/PHP/存储空间.md#section_ond_15p_kfb)|
    |[Go SDK](../../../../intl.zh-CN/SDK 示例/Go/存储空间.md#)|
    |[C SDK](../../../../intl.zh-CN/SDK 示例/C/存储空间.md#)|
    |[.NET SDK](../../../../intl.zh-CN/SDK 示例/.NET/存储空间.md#)|
    |[Node.js SDK](../../../../intl.zh-CN/SDK 示例/Node.js/存储空间.md#ul_ict_gqk_lfb)|
    |[Ruby SDK](../../../../intl.zh-CN/SDK 示例/Ruby/存储空间.md#ul_px3_pnn_lfb)|


## Object ACL {#section_yft_rvm_2gb .section}

-   Object ACL 介绍

    Object ACL是Object 级别的权限访问控制。目前有四种访问权限：private, public-read, public-read-write, default。PutObjectACL 操作通过 Put 请求中的`x-oss-object-acl`头来设置，这个操作只有 Bucket Owner 有权限执行。

    Object ACL 的四种访问权限含义如下：

    |权限值|中文名称|权限对访问者的限制|
    |:--|:---|:--------|
    |public-read-write|公共读写|该 ACL 表明某个 Object 是公共读写资源，即所有用户拥有对该 Object 的读写权限。|
    |public-read|公共读，私有写|该 ACL 表明某个 Object 是公共读资源，即非 Object Owner 只有该 Object 的读权限，而 Object Owner 拥有该 Object 的读写权限。|
    |private|私有读写|该 ACL 表明某个 Object 是私有资源，即只有该 Object 的 Owner 拥有该 Object 的读写权限，其他的用户没有权限操作该 Object。|
    |default|默认权限|该 ACL 表明某个 Object 是遵循 Bucket 读写权限的资源，即 Bucket 是什么权限，Object 就是什么权限。|

    **说明：** 

    -   如果没有设置 Object 的权限，即 Object 的 ACL 为 default，Object 的权限和 Bucket 权限一致。
    -   如果设置了 Object 的权限，Object 的权限大于 Bucket 权限。例如，设置了 Object 的权限是 public-read，则无论 Bucket 是什么权限，该 Object 都可以被身份验证访问和匿名访问。
-   操作方式

    |操作方式|说明|
    |----|--|
    |[控制台](../../../../intl.zh-CN/控制台用户指南/上传、下载和管理文件/设置文件读写权限ACL.md#)|Web应用程序，直观易用|
    |[图形化工具ossbrowser](../../../../intl.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)|图形化工具，易操作|
    |[命令行工具ossutil](../../../../intl.zh-CN/常用工具/命令行工具ossutil/有关Object的命令.md#section_zbv_mfy_bgb)|命令行工具，性能好|
    |[Java SDK](../../../../intl.zh-CN/SDK 示例/Java/管理文件/管理文件访问权限.md#)|丰富、完整的各类语言SDK demo|
    |[Python SDK](../../../../intl.zh-CN/SDK 示例/Python/管理文件/管理文件访问权限.md#)|
    |[PHP SDK](../../../../intl.zh-CN/SDK 示例/PHP/管理文件/管理文件访问权限.md#)|
    |[Go SDK](../../../../intl.zh-CN/SDK 示例/Go/管理文件/管理文件访问权限.md#)|
    |[C SDK](../../../../intl.zh-CN/SDK 示例/C/管理文件/管理文件访问权限.md#)|
    |[.NET SDK](../../../../intl.zh-CN/SDK 示例/.NET/管理文件/管理文件访问权限.md#)|


## 更多参考 {#section_jj1_j33_wgb .section}

如果您希望仅允许指定用户访问您的文件，可参考：

-   [教程示例：基于Bucket Policy实现跨账号访问OSS](intl.zh-CN/开发指南/权限控制/跨账号授权/教程示例：基于Bucket Policy实现跨账号访问OSS.md#)
-   [跨账号授权OSS资源](https://www.alibabacloud.com/help/zh/doc-detail/69011.htm) 

