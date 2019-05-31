# Wordpress如何存储远程附件到OSS {#concept_pf4_3hb_wdb .concept}

## 前言 {#section_qyh_jhb_wdb .section}

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。

目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

本文介绍如何基于Wordpress论坛存储远程附件。

## 准备工作 {#section_ngd_phb_wdb .section}

申请OSS账号，并且创建一个public-read的Bucket。权限设置为public-read主要是便于后续的匿名访问。

## 详细步骤 {#section_mrj_qhb_wdb .section}

wordpress本身不支持远程附件功能，但是可以通过第三方的插件来做远程附件。本文档示例中所用wordpress版本为4.3.1，所用插件为Hacklog Remote Attachment，以下为具体设置步骤。

1.  登录wordpress站点，选择安装插件，搜关键词FTP，选择Hacklog Remote Attachment安装。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960782822_zh-CN.png)

2.  设置。
    -   FTP服务器地址，即运行ossftp工具的地址，一般填127.0.0.1即可。
    -   FTP服务的端口号，默认为2048。
    -   FTP登录用户名，格式为AccessKeyID/BukcetName，注意这里的/不是或的意思。
    -   FTP的登录密码为AccessKeySecret。

        **说明：** 关于AccessKeyID和AccessKeySecret的获取，可以登录阿里云控制台的Access Key管理进行查看。

    -   FTP超时时间， 默认设置为30秒即可。
    -   远程基本URL填 http://BucketName.Endpoint/wp。这里测试所用bucket为test-hz-jh-002, 属于杭州区域的，所以这里填写的是`http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/wp`。
    -   FTP远程路径， 填wp表示所有附件都会存储在bucket的wp目录下。远程基本URL须与FTP远程路径对应。
    -   HTTP远程路径，填.即可。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792823_zh-CN.png)

3.  验证。

    设置完成后，点击保存的同时会进行测试，测试结果会在页面上方显示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792824_zh-CN.png)

4.  发布新文章， 并插入图片。

    现在开始撰写新文章，并测试远程附件。创建好文章后，单击添加媒体来上传附件。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792825_zh-CN.png)

    上传附件如下图所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792826_zh-CN.png)

5.  上传附件后，点击发布，即可看到刚撰写的文章。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792827_zh-CN.png)

    在图片上右键单击，选择**在新标签页中打开图片**，即可看查看图片的URL。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4867/15592960792829_zh-CN.png)

    通过图中的URL，我们可以判断图片已经上传到OSS的test-hz-jh-002 Bucket中。


