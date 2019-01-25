# 基于读写权限ACL的权限控制 {#concept_blw_yqm_2gb .concept}

OSS 为权限控制提供访问控制列表（ACL）。ACL是授予Bucket和Object访问权限的访问策略。 您可以在创建Bucket或上传Object时配置ACL，也可以在创建Bucket或上传Object后的任意时间内修改ACL。

## Bucket ACL {#section_dr5_nvm_2gb .section}

Bucket ACL是Bucket级别的权限访问控制。目前有三种访问权限：public-read-write，public-read 和 private，含义如下：

|权限值|中文名称|权限对访问者的限制|
|:--|:---|:--------|
|public-read-write|公共读写|任何人（包括匿名访问）都可以对该 Bucket 中的 Object 进行读/写/删除操作；所有这些操作产生的费用由该 Bucket 的 Owner 承担，请慎用该权限。|
|public-read|公共读，私有写|只有该 Bucket 的 Owner 或者授权对象可以对存放在其中的 Object 进行写/删除操作；任何人（包括匿名访问）可以对 Object 进行读操作。|
|private|私有读写|只有该 Bucket 的Owner 或者授权对象可以对存放在其中的 Object 进行读/写/删除操作；其他人在未经授权的情况下无法访问该 Bucket 内的 Object。|

Bucket ACL的权限设定和读取方法如下：

-   控制台：[创建 Bucket时设置ACL](../../../../../intl.zh-CN/控制台用户指南/管理存储空间/创建存储空间.md#)、[修改Bucket ACL](../../../../../intl.zh-CN/控制台用户指南/管理存储空间/修改存储空间读写权限.md#) 
-   API：[PutBucketACL](../../../../../intl.zh-CN/API 参考/关于Bucket的操作/PutBucketACL.md#)、[GetBucketACL](../../../../../intl.zh-CN/API 参考/关于Bucket的操作/GetBucketAcl.md#)
-   SDK：[设置Bucket ACL](../../../../../intl.zh-CN/SDK 参考/Java/存储空间.md#section_frn_jjf_2gb)

## Object ACL {#section_yft_rvm_2gb .section}

Object ACL是Object 级别的权限访问控制。目前有四种访问权限：private, public-read, public-read-write, default。PutObjectACL 操作通过 Put 请求中的`x-oss-object-acl`头来设置，这个操作只有 Bucket Owner 有权限执行。

Object ACL的四种访问权限含义如下：

|权限值|中文名称|权限对访问者的限制|
|:--|:---|:--------|
|public-read-write|公共读写|该 ACL 表明某个 Object 是公共读写资源，即所有用户拥有对该 Object 的读写权限。|
|public-read|公共读，私有写|该 ACL 表明某个Object 是公共读资源，即非 Object Owner 只有该 Object 的读权限，而 Object Owner 拥有该 Object 的读写权限。|
|private|私有读写|该 ACL 表明某个Object 是私有资源，即只有该 Object 的 Owner 拥有该 Object 的读写权限，其他的用户没有权限操作该 Object。|
|default|默认权限|该 ACL 表明某个Object 是遵循 Bucket读写权限的资源，即 Bucket 是什么权限，Object 就是什么权限。|

**说明：** 

-   如果没有设置 Object 的权限，即 Object 的 ACL 为 default，Object 的权限和 Bucket 权限一致。
-   如果设置了 Object 的权限，Object 的权限大于 Bucket 权限。举个例子，如果设置了 Object 的权限是 public-read，则无论 Bucket 是什么权限，该 Object 都可以被身份验证访问和匿名访问。

Object ACL权限设定和读取方法如下：

-   控制台：[上传文件时设置Object ACL](../../../../../intl.zh-CN/控制台用户指南/管理文件/上传文件.md#)、[修改Object ACL](../../../../../intl.zh-CN/控制台用户指南/管理文件/设置文件读写权限ACL.md#)
-   API：[PutObjectACL](../../../../../intl.zh-CN/API 参考/关于Object操作/PutObjectACL.md#)、[GetObjectACL](../../../../../intl.zh-CN/API 参考/关于Object操作/GetObjectACL.md#)
-   SDK：[管理Object ACL](../../../../../intl.zh-CN/SDK 参考/Java/管理文件/管理文件访问权限.md#)

