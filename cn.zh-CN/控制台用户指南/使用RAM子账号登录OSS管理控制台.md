# 使用RAM子账号登录OSS管理控制台 {#concept_r3h_c1y_5db .concept}

基于阿里云OSS管理控制台，您可以使用直观的界面进行相应的操作。除了使用阿里云账号，您还可以使用子账号（RAM用户）登录OSS管理控制台。

使用RAM子账号登录OSS管理控制台的步骤如下：

1.  创建RAM用户。
2.  给子账号授权。
3.  使用子账号登录控制台。

## 创建RAM用户 {#section_tf2_lcy_5db .section}

使用阿里云账号登录到[RAM控制台](https://ram.console.aliyun.com)，选择**用户管理** \> **新建用户**来创建RAM用户。具体操作方法请参见[RAM用户操作手册](https://help.aliyun.com/document_detail/28647.html?spm=a2c4g.11186623.2.7.77WgjY)中的“创建RAM用户”章节。

## 给子账号授权 {#section_gxr_gly_5db .section}

使用阿里云账号登录到[RAM控制台](https://ram.console.aliyun.com)，选择对应的RAM用户，单击**授权**，进行授权操作。具体操作方法请参见[RAM授权帮助文档](https://help.aliyun.com/document_detail/28651.html)。

为确保子账号登录控制台后能正常使用OSS控制台的功能，除授予OSS相应的访问权限外，还需要MNS、CloudMonitor、CDN的访问权限，如下图所示：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4737/15360761451495_zh-CN.PNG)

## 使用子账号登录控制台 {#section_c2h_bmy_5db .section}

使用子账号登录控制台的操作步骤如下：

1.  使用阿里云账号登录到[RAM控制台](https://ram.console.aliyun.com)，单击**用户管理**。
2.  选择对应的RAM用户，单击**管理**，配置相关信息。
3.  打开**启用控制台登录**，设置RAM用户登录控制台的密码。
4.  在概览页面查看您的**RAM用户登录链接**，打开该链接，使用RAM用户名和密码进行登录。

详情请参见[RAM用户操作手册](https://help.aliyun.com/document_detail/28647.html?spm=a2c4g.11186623.2.7.77WgjY)。

