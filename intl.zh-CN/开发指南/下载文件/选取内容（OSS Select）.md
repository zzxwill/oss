# 选取内容（OSS Select） {#concept_xsq_qws_2fb .concept}

通过 OSS Select，您可以使用简单的结构化查询语言 \(SQL\) 语句选取 OSS 中文件的内容，仅获取需要的数据。使用 OSS Select 可以减少从 OSS 传输的数据量，从而提高数据获取效率，节约时间。

**说明：** 您可以在请求中将 SQL 表达式传递给 OSS。有关 OSS Select 支持的 SQL 语句和限制，请参考[SelectObject](../../../../intl.zh-CN/API 参考/关于Object操作/SelectObject.md#)。

## 操作方式 {#section_bdy_cv3_kgb .section}

OSS SDK（目前 Java 和 Python 的 SDK 已支持 OSS Select）。

## 限制 {#section_pll_g3t_2fb .section}

-   文件格式：OSS Select 支持编码为 UTF-8 且符合 RFC 4180 标准的 CSV 文件（包括TSV 等类 CSV 文件）。文件的行列分隔符及 Quote 字符都可以自由定义。
-   加密方式：OSS Select 支持 OSS 完全托管和通过 KMS 托管主密钥进行加密的文件。
-   地域：OSS Select 目前只在华南1（深圳）开放。

