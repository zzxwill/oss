# API概览 {#reference_wrz_l2q_tdb .reference}

OSS提供的API接口如下：

## 关于Service操作 {#section_lmg_t2q_tdb .section}

|API|描述|
|---|--|
|[GetService](intl.zh-CN/API 参考/关于Service操作/GetService (ListBuckets).md#)|得到该账户下所有Bucket|

## 关于Bucket的操作 {#section_oqw_hb2_xdb .section}

|API|描述|
|:--|:-|
|[PutBucket](intl.zh-CN/API 参考/关于Bucket的操作/PutBucket.md#)|创建Bucket|
|[PutBucketACL](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketACL.md#)|设置Bucket访问权限|
|[PutBucketLogging](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketLogging.md#)|开启Bucket日志|
|[PutBucketWebsite](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketWebsite.md#)|设置Bucket为静态网站托管模式|
|[PutBucketReferer](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketReferer.md#)|设置Bucket的防盗链规则|
|[PutBucketLifecycle](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketLifecycle.md#)|设置Bucket中Object的生命周期规则|
|[GetBucket\(ListObject\)](intl.zh-CN/API 参考/关于Bucket的操作/GetBucket (ListObjects).md#)|列出Bucket中所有Object的信息|
|[GetBucketAcl](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketAcl.md#)|获得Bucket访问权限|
|[GetBucketLocation](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLocation.md#)|获得Bucket所属的数据中心位置信息|
|[GetBucketInfo](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketInfo.md#)|获取Bucket信息|
|[GetBucketLogging](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLogging.md#)|查看Bucket的访问日志配置情况|
|[GetBucketWebsite](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketWebsite.md#)|查看Bucket的静态网站托管状态|
|[GetBucketReferer](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketReferer.md#)|查看Bucket的防盗链规则|
|[GetBucketLifecycle](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLifecycle.md#)|查看Bucket中Object的生命周期规则|
|[DeleteBucket](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucket.md#)|删除Bucket|
|[DeleteBucketLogging](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketLogging.md#)|关闭Bucket访问日志记录功能|
|[DeleteBucketWebsite](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketWebsite.md#)|关闭Bucket的静态网站托管模式|
|[DeleteBucketLifecycle](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketLifecycle.md#)|删除Bucket中Object的生命周期规则|

## 关于Object的操作 {#section_tq2_kb2_xdb .section}

|API|描述|
|:--|:-|
|[PutObject](intl.zh-CN/API 参考/关于Object操作/PutObject.md#)|上传Object|
|[CopyObject](intl.zh-CN/API 参考/关于Object操作/CopyObject.md#)|拷贝一个Object成另外一个Object|
|[GetObject](intl.zh-CN/API 参考/关于Object操作/GetObject.md#)|获取Object|
|[AppendObject](intl.zh-CN/API 参考/关于Object操作/AppendObject.md#)|在Object尾追加上传数据|
|[DeleteObject](intl.zh-CN/API 参考/关于Object操作/DeleteObject.md#)|删除Object|
|[DeleteMultipleObjects](intl.zh-CN/API 参考/关于Object操作/DeleteMultipleObjects.md#)|删除多个Object|
|[HeadObject](intl.zh-CN/API 参考/关于Object操作/HeadObject.md#)|只返回某个Object的meta信息，不返回文件内容|
|[GetObjectMeta](intl.zh-CN/API 参考/关于Object操作/GetObjectMeta.md#)|返回Object的基本meta信息，包括该Object的ETag、Size（文件大小）、LastModified，不返回文件内容|
|[PostObject](intl.zh-CN/API 参考/关于Object操作/PostObject.md#)|使用Post上传Object|
|[PutObjectACL](intl.zh-CN/API 参考/关于Object操作/PutObjectACL.md#)|设置ObjectACL|
|[GetObjectACL](intl.zh-CN/API 参考/关于Object操作/GetObjectACL.md#)|获取ObjectACL信息|
|[Callback](intl.zh-CN/API 参考/关于Object操作/Callback.md#)|上传回调|
|[PutSymlink](intl.zh-CN/API 参考/关于Object操作/PutSymlink.md#)|创建软链接|
|[GetSymlink](intl.zh-CN/API 参考/关于Object操作/GetSymlink.md#)|获取软链接|
|[RestoreObject](intl.zh-CN/API 参考/关于Object操作/RestoreObject.md#)|解冻文件|
|[SelectObject](intl.zh-CN/API 参考/关于Object操作/SelectObject.md#)|用SQL语法查询Object内容|

## 关于Multipart Upload的操作 {#section_my2_rb2_xdb .section}

|API|描述|
|:--|:-|
|[InitiateMultipartUpload](intl.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md#)|初始化MultipartUpload事件|
|[UploadPart](intl.zh-CN/API 参考/关于MultipartUpload的操作/UploadPart.md#)|分块上传文件|
|[UploadPartCopy](intl.zh-CN/API 参考/关于MultipartUpload的操作/UploadPartCopy.md#)|分块复制上传文件|
|[CompleteMultipartUpload](intl.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)|完成整个文件的MultipartUpload上传|
|[AbortMultipartUpload](intl.zh-CN/API 参考/关于MultipartUpload的操作/AbortMultipartUpload.md#)|取消MultipartUpload事件|
|[ListMultipartUploads](intl.zh-CN/API 参考/关于MultipartUpload的操作/ListMultipartUploads.md#)|罗列出所有执行中的MultipartUpload事件|
|[ListParts](intl.zh-CN/API 参考/关于MultipartUpload的操作/ListParts.md#)|罗列出指定UploadID所属的所有已经上传成功Part|

## 跨域资源共享\(CORS\) {#section_xhx_sb2_xdb .section}

|API|描述|
|:--|:-|
|[PutBucketcors](intl.zh-CN/API 参考/跨域资源共享/PutBucketcors.md#)|在指定Bucket设定一个CORS的规则|
|[GetBucketcors](intl.zh-CN/API 参考/跨域资源共享/GetBucketcors.md#)|获取指定的Bucket目前的CORS规则|
|[DeleteBucketcors](intl.zh-CN/API 参考/跨域资源共享/DeleteBucketcors.md#)|关闭指定Bucket对应的CORS功能并清空所有规则|
|[OptionObject](intl.zh-CN/API 参考/跨域资源共享/OptionObject.md#)|跨域访问preflight请求|

