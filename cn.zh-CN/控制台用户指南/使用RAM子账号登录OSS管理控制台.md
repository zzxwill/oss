# 使用RAM子账号登录OSS管理控制台 {#concept_r3h_c1y_5db .concept}

RAM 子账号是您通过 RAM 控制台创建的 RAM 用户账号，使用 RAM 子账号同样可以登录 OSS 管理控制台。

## RAM 子账号使用场景 {#section_zt3_qjw_dgb .section}

您可以在以下场景中使用 RAM 子账号登录 OSS 控制台：

-   主账号的 Bucket 内存放了企业内部共享文件，您可以创建 RAM 子账号给员工，并授予相应的访问权限，员工可以使用 RAM 子账号登录 OSS 控制台查看这些共享文件。
-   公司有部分合作伙伴需要定期查看一些资料，您可以将资料放在指定的 Bucket 内，并创建 RAM 子账号，授予指定 Bucket 的访问权限，合作伙伴可以定期使用 RAM 子账号登录 OSS 控制台查看共享文件。
-   开发环境需要使用阿里云账号，不方便将主账号拿来测试，可以创建 RAM 子账号用于测试。
-   其他使用场景。

    使用 RAM 子账号登录 OSS 管理控制台的步骤如下：

    1.  [创建 RAM 子账号](#)。
    2.  [给 RAM 子账号授权](#)。
    3.  [使用 RAM 子账号登录 OSS 控制台](#)。

## 创建 RAM 子账号 {#section_tf2_lcy_5db .section}

1.  登录[RAM 控制台](https://ram.console.aliyun.com)。
2.  单击**人员管理** \> **用户** \> **新建用户**创建 RAM 用户。
3.  填写新建用户的信息，并根据需要配置**访问方式**，单击**添加用户**可一次添加多个用户。详细配置方法请参见[RAM 用户操作手册](../../../../../cn.zh-CN/用户指南/身份管理/用户管理/用户.md#) 中的“创建RAM用户”章节。
4.  用户信息填写完毕后单击**确定**，之后单击**返回**。

## 给 RAM 子账号授权 {#section_gxr_gly_5db .section}

1.  打开 RAM 控制台[用户列表](https://ram.console.aliyun.com/users)。
2.  选择您需要授权的 RAM 用户，单击对应用户的**添加权限**。
3.  根据您的需求添加对应的权限。系统只提供部分策略，您可根据需要添加自定义权限。更多信息请参见[权限策略管理](../../../../../cn.zh-CN/用户指南/权限管理/权限策略管理.md#)。

    **说明：** 

    为确保 RAM 子账号登录控制台后能正常使用 OSS 控制台的功能，除授予 OSS 相应的访问权限外，还需要 MNS、CloudMonitor、CDN 的访问权限，如下图所示：![RAM 子账号](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4737/15471912631495_zh-CN.PNG)


## 使用 RAM 子账号登录 OSS 管理控制台 {#section_c2h_bmy_5db .section}

使用 RAM 子账号登录 OSS 管理控制台的步骤如下：

1.  登录[RAM 控制台](https://ram.console.aliyun.com)。
2.  在概览页面查看您的**用户登录地址**。打开该链接，使用 RAM 子账号的用户名和密码进行登录。

    ![RAM 子账号](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4737/154719126311370_zh-CN.png)

3.  打开 [OSS 管理控制台](https://oss.console.aliyun.com)进行管理。

更多信息请参见[RAM 用户操作手册](../../../../../cn.zh-CN/用户指南/身份管理/用户管理/用户.md#)。

