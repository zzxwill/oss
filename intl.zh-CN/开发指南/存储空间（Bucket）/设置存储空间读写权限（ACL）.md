# 设置存储空间读写权限（ACL） {#concept_fnt_4z1_5db .concept}

您可以在创建存储空间（Bucket）时设置存储空间的访问权限（ACL），也可以在创建Bucket后根据自己的业务需求修改存储空间的ACL，该操作只有存储空间的拥有者可以执行。

存储空间有三种访问权限：

|权限值|中文名称|权限对访问者的限制|
|:--|:---|:--------|
|public-read-write|公共读写|任何人（包括匿名访问者）都可以对该存储空间内文件进行读写操作。**警告：** 互联网上任何用户都可以对该 Bucket 内的文件进行访问，并且向该 Bucket 写入数据。这有可能造成您数据的外泄以及费用激增，若被人恶意写入违法信息还可能会侵害您的合法权益。除特殊场景外，不建议您配置公共读写权限。

|
|public-read|公共读，私有写|只有该存储空间的拥有者可以对该存储空间内的文件进行写操作，任何人（包括匿名访问者）都可以对该存储空间中的文件进行读操作。**警告：** 互联网上任何用户都可以对该 Bucket 内文件进行访问，这有可能造成您数据的外泄以及费用激增，请谨慎操作。

|
|private|私有读写|只有该存储空间的拥有者可以对该存储空间内的文件进行读写操作，其他人无法访问该存储空间内的文件。|

## 操作方式 {#section_bdy_cv3_kgb .section}

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

## 更多参考 {#section_skh_zd4_vgb .section}

-   [权限控制概述](intl.zh-CN/开发指南/权限控制/权限控制概述.md#)
-   [跨账号授权概述](intl.zh-CN/开发指南/权限控制/跨账号授权/跨账号授权概述.md#)

