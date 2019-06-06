# Discuz如何存储远程附件到OSS {#concept_n3l_ftt_vdb .concept}

本文介绍如何基于Discuz论坛存储远程附件。

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

## 准备工作 {#section_v4b_jz1_wdb .section}

申请OSS账号，并且创建一个public-read的bucket。权限设置为public-read主要是便于后续的匿名访问。

## 详细步骤 {#section_zwg_kz1_wdb .section}

测试所用Discuz版本为Discuz! X3.1，以下是详细的设置流程：

1.  登录Discuz站点，进入管理界面后，先单击**全局**，然后单击**上传设置**。
2.  选择**远程附件**，然后开始设置。

    **说明：** 

    -   启用远程附件，请选择是。
    -   启用SSL连接，请选择否。
    -   FTP服务器地址， 即运行ossftp工具的地址，通常填写 127.0.0.1 即可。
    -   FTP服务的端口号，默认为 2048。
    -   FTP登录用户名，格式为 AccessKeyID/BukcetName。注意这里的 / 不是或的意思。
    -   FTP的登录密码，即AccessKeySecret。
    -   被动模式连接，默认选择是。
    **说明：** 

    -   远程附件目录，填 . 表示在Bucket的根目录下创建上传目录。
    -   远程访问URL， 填 `http://BucketName.Endpoint`即可。

        **说明：** 这里测试所用bucket为test-hz-jh-002，属于杭州区域，所以这里填写的是`http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com`。注意BucketName要和Endpoint匹配。

    -   超时时间，设置为0即可，表示服务器默认。
    -   完成设置后，可以点击测试远程附件。
3.  发帖验证

    发贴时上传图片附件。

    在图片上右键单击，选择**在新标签页中打开图片**。

    通过图中的URL，我们可以判断图片已经上传到OSS的test-hz-jh-002 Bucket中。


