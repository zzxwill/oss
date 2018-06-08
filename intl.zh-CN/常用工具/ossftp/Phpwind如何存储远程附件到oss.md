# Phpwind如何存储远程附件到oss {#concept_vwc_m2b_wdb .concept}

## 前言 {#section_cm4_p2b_wdb .section}

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。

目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

本文介绍如何基于Phpwind论坛存储远程附件。

## 准备工作 {#section_cqh_52b_wdb .section}

申请OSS账号，并且创建一个public-read的bucket。这里需要权限为public-read是因为后面需要匿名访问。

## 详细步骤 {#section_smk_v2b_wdb .section}

测试所用版本为phpwind8.7, 以下为详细设置流程。

1.  登录站点

    进入管理界面，依次选择**全局** \> **上传设置** \> **远程附件**。

2.  开始设置

    **说明：** 

    -   使用FTP上传，请选择开启。
    -   站点附件地址，填写[http://bucket-name.endpoint](http://bucket-name.endpoint/)*这里测试所用bucket为test-hz-jh-002, 属于杭州区域的，所以这里填写的是*[http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com](http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/)，注意BucketName要和Endpoint匹配。
    -   FTP服务器地址, 即运行ossftp工具的地址，一般填127.0.0.1即可。
    -   FTP服务的端口号，默认为2048。
    -   FTP上传目录， 默认填 .即可，表示在bucket的根目录开始创建附件目录。
    -   FTP登录用户名，格式为AccessKeyID/BukcetName。注意这里的 / 不是或的意思。
    -   FTP的登录密码，为AceessKeySecrete关于AccessKeyID和AceessKeySecrete的获取，可以登录阿里云控制台的Access Key管理进行查看。
    -   FTP超时时间，设置为10就表示如果10秒内请求没有返回结果就会超时返回。
3.  发帖验证

    phpwind不能在设置完成后直接点击测试，我们这里发带图片的帖子来验证下。

    在图片点击右键，在新建标签页中打开图片，可以看到下图。

    通过图中的URL，我们可以判断图片已经上传到了OSS的test-hz-jh-002 Bucket中。


