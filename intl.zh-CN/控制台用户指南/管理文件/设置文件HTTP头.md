# 设置文件HTTP头 {#concept_pk1_sxl_vdb .concept}

您可以通过设置文件HTTP头来自定义HTTP请求的策略，例如缓存策略、文件强制下载策略等。

**说明：** 使用控制台批量设置HTTP头的限制数量为1000个文件。如需为更多文件设置HTTP头，请参见API文档[PutObject](../../../../intl.zh-CN/API 参考/关于Object操作/PutObject.md#)和Java SDK文档[管理文件元信息](https://www.alibabacloud.com/help/doc-detail/84840.htm)。

## 操作步骤 {#section_i1g_cyl_vdb .section}

1.  登录[OSS管理控制台](https://oss.console.aliyun.com/)。
2.  在左侧存储空间列表中，单击目标存储空间名称，打开该存储空间概览页面。
3.  单击**文件管理**页签。
4.  通过以下任一方式进入设置 HTTP 头页面：
    -   要设置一个或多个文件的HTTP头，选一个或多个文件，选择**批量操作** \> **设置HTTP头**。
    -   要设置单个文件的HTTP头，单击目标文件的文件名或对应的**设置**，在打开的预览页面中单击**设置HTTP头**。
    -   要设置单个文件的HTTP头，还可以选择目标文件对应的**更多** \> **设置HTTP头**。
5.  设置相关参数的值。您还可以添加自定义元信息。

    **说明：** 有关每个参数的详细描述，请参见[公共HTTP头定义](../../../../intl.zh-CN/API 参考/公共HTTP头定义.md#)。

6.  单击**确定**。

