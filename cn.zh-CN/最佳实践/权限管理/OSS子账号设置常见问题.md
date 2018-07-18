# OSS子账号设置常见问题 {#concept_ktx_ktz_5db .concept}

## 如何生成STS临时账户？如何应用STS临时账户访问资源？ {#section_w1h_ntz_5db .section}

请参考：[STS临时授权访问](intl.zh-CN/最佳实践/权限管理/STS临时授权访问.md#)

## 子账号授权权限，客户端/控制台登录报错 {#section_edc_4tz_5db .section}

## 如何为子账户授权单个bucket权限？ {#section_xxv_4tz_5db .section}

## 如何为子账户授权bucket下某个目录的权限？ {#section_brw_ptz_5db .section}

## 如何为子账户授权某个bucket的只读权限？ {#section_chl_qtz_5db .section}

请参考：[授权一个子用户列出并读取一个 Bucket 中的资源](https://www.alibabacloud.com/help/doc-detail/58905.htm#concept-ohn-ypx-ydb-section-jpq-1px-ydb)

## OSS SDK 调用报错：InvalidAccessKeyId {#section_y12_rtz_5db .section}

请参考：[STS常见问题及排查](../../../../intl.zh-CN/常见错误排除/STS常见问题及排查.md#)

## STS调用报错：Access denied by authorizer’s policy {#section_kt5_rtz_5db .section}

详细错误：ErrorCode: AccessDenied ErrorMessage: Access denied by authorizer’s policy.

该错误的原因是：临时用户访问无权限，该临时用户角色扮演指定授权策略，该授权策略无权限。

更多STS错误提示以及对应原因，请参考：[OSS 权限问题及排查](../../../../intl.zh-CN/常见错误排除/OSS 权限问题及排查.md#)

