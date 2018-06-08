# API概览 {#reference_wrz_l2q_tdb .reference}

OSS提供的API接口如下：

## 关于Service操作 {#section_lmg_t2q_tdb .section}

|API|描述|
|---|--|
|[GetService](intl.zh-CN/API 参考/关于Service操作/GetService (ListBuckets).md#)|得到该账户下所有Bucket|

## 关于Bucket的操作 {#section_oqw_hb2_xdb .section}

|API|描述|
|:--|:-|
|[Put Bucket](intl.zh-CN/API 参考/关于Bucket的操作/PutBucket.md#)|创建Bucket|
|[Put Bucket ACL](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketACL.md#)|设置Bucket访问权限|
|[Put Bucket Logging](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketLogging.md#)|开启Bucket日志|
|[Put Bucket Website](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketWebsite.md#)|设置Bucket为静态网站托管模式|
|[Put Bucket Referer](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketReferer.md#)|设置Bucket的防盗链规则|
|[Put Bucket Lifecycle](intl.zh-CN/API 参考/关于Bucket的操作/PutBucketLifecycle.md#)|设置Bucket中Object的生命周期规则|
|[Get Bucket Acl](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketAcl.md#)|获得Bucket访问权限|
|[Get Bucket Location](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLocation.md#)|获得Bucket所属的数据中心位置信息|
|[Get Bucket Logging](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLogging.md#)|查看Bucket的访问日志配置情况|
|[Get Bucket Website](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketWebsite.md#)|查看Bucket的静态网站托管状态|
|[Get Bucket Referer](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketReferer.md#)|查看Bucket的防盗链规则|
|[Get Bucket Lifecycle](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketLifecycle.md#)|查看Bucket中Object的生命周期规则|
|[Delete Bucket](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucket.md#)|删除Bucket|
|[Delete Bucket Logging](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketLogging.md#)|关闭Bucket访问日志记录功能|
|[Delete Bucket Website](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketWebsite.md#)|关闭Bucket的静态网站托管模式|
|[Delete Bucket Lifecycle](intl.zh-CN/API 参考/关于Bucket的操作/DeleteBucketLifecycle.md#)|删除Bucket中Object的生命周期规则|
|[Get Bucket\(List Object\)](intl.zh-CN/API 参考/关于Bucket的操作/GetBucket.md#)|获得Bucket中所有Object的信息|
|[Get Bucket Info](intl.zh-CN/API 参考/关于Bucket的操作/GetBucketInfo.md#)|获取Bucket信息|

## 关于Object的操作 {#section_tq2_kb2_xdb .section}

|API|描述|
|:--|:-|
|[Put Object](intl.zh-CN/API 参考/关于Object操作/PutObject.md#)|上传object|
|[Copy Object](intl.zh-CN/API 参考/关于Object操作/CopyObject.md#)|拷贝一个object成另外一个object|
|[Get Object](intl.zh-CN/API 参考/关于Object操作/GetObject.md#)|获取Object|
|[Delete Object](intl.zh-CN/API 参考/关于Object操作/DeleteObject.md#)|删除Object|
|[Delete Multiple Objects](intl.zh-CN/API 参考/关于Object操作/DeleteMultipleObjects.md#)|删除多个Object|
|[Head Object](intl.zh-CN/API 参考/关于Object操作/HeadObject.md#)|获得Object的meta信息|
|[Post Object](intl.zh-CN/API 参考/关于Object操作/PostObject.md#)|使用Post上传Object|
|[Append Object](intl.zh-CN/API 参考/关于Object操作/AppendObject.md#)|在Object尾追加上传数据|
|[Put Object ACL](intl.zh-CN/API 参考/关于Object操作/PutObjectACL.md#)|设置Object ACL|
|[Get Object ACL](intl.zh-CN/API 参考/关于Object操作/GetObjectACL.md#)|获取Object ACL信息|
|[Callback](intl.zh-CN/API 参考/关于Object操作/Callback.md#)|上传回调|

## 关于Multipart Upload的操作 {#section_my2_rb2_xdb .section}

|API|描述|
|:--|:-|
|[Initiate Multipart Upload](intl.zh-CN/API 参考/关于MultipartUpload的操作/InitiateMultipartUpload.md#)|初始化MultipartUpload事件|
|[Upload Part](intl.zh-CN/API 参考/关于MultipartUpload的操作/UploadPart.md#)|分块上传文件|
|[Upload Part Copy](intl.zh-CN/API 参考/关于MultipartUpload的操作/UploadPartCopy.md#)|分块复制上传文件|
|[Complete Multipart Upload](intl.zh-CN/API 参考/关于MultipartUpload的操作/CompleteMultipartUpload.md#)|完成整个文件的Multipart Upload上传|
|[Abort Multipart Upload](intl.zh-CN/API 参考/关于MultipartUpload的操作/AbortMultipartUpload.md#)|取消Multipart Upload事件|
|[List Multipart Uploads](intl.zh-CN/API 参考/关于MultipartUpload的操作/ListMultipartUploads.md#)|罗列出所有执行中的Multipart Upload事件|
|[List Parts](intl.zh-CN/API 参考/关于MultipartUpload的操作/ListParts.md#)|罗列出指定Upload ID所属的所有已经上传成功Part|

## 跨域资源共享\(CORS\) {#section_xhx_sb2_xdb .section}

|API|描述|
|:--|:-|
|[Put Bucket cors](intl.zh-CN/API 参考/跨域资源共享/PutBucketcors.md#)|在指定Bucket设定一个CORS的规则|
|[Get Bucket cors](intl.zh-CN/API 参考/跨域资源共享/GetBucketcors.md#)|获取指定的Bucket目前的CORS规则|
|[Delete Bucket cors](intl.zh-CN/API 参考/跨域资源共享/DeleteBucketcors.md#)|关闭指定Bucket对应的CORS功能并清空所有规则|
|[Option Object](intl.zh-CN/API 参考/跨域资源共享/OptionObject.md#)|跨域访问preflight请求|

