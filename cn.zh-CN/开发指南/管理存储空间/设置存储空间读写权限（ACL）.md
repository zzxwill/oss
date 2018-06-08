# 设置存储空间读写权限（ACL） {#concept_fnt_4z1_5db .concept}

除了在创建存储空间的时候能够对存储空间的 ACL 进行设置，也可以之后根据自己的业务需求对存储空间的ACL进行修改。这个操作只有该存储空间的创建者有权限执行。目前存储空间有三种访问权限:

|权限值|中文名称|权限对访问者的限制|
|:--|:---|:--------|
|public-read-write|公共读写|任何人（包括匿名访问）都可以对该存储空间中的文件进行读写操作，所有这些操作产生的费用由该存储空间的创建者承担，请慎用该权限。|
|public-read|公共读，私有写|只有该存储空间的拥有者或者授权对象可以对该存储空间内的文件进行写操作，任何人（包括匿名访问）可以对该存储空间中的文件进行读操作。|
|private|私有读写|只有资源拥有者或者授权对象可以对该存储空间内的文件进行读写操作，其他人在未经授权的情况下无法访问该存储空间内的文件。|

详细解释请参见[访问权限](cn.zh-CN//访问控制.md#)。

## 功能使用参考 {#section_fmv_tz1_5db .section}

设置存储空间 ACL

-   控制台: [设置访问权限](../cn.zh-CN/控制台用户指南/管理存储空间/修改存储空间读写权限.md#)
-   SDK：Java SDK-[Bucket](https://help.aliyun.com/document_detail/32012.html) 中 设置Bucket ACL
-   API：[Put BucketACL](../cn.zh-CN/API 参考/关于Bucket的操作/PutBucketACL.md#)

获取存储空间 ACL

-   控制台：登录后可以在存储空间属性中查看
-   SDK：Java SDK-[Bucket](https://help.aliyun.com/document_detail/32012.html) 中 获取Bucket ACL
-   API：[Get BucketACL](../cn.zh-CN/API 参考/关于Bucket的操作/GetBucketAcl.md#)

