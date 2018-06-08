# Wordpress如何存储远程附件到oss {#concept_pf4_3hb_wdb .concept}

## 前言 {#section_qyh_jhb_wdb .section}

网站远程附件功能是指将用户上传的附件直接存储到远端的存储服务器，一般是通过FTP的方式存储到远程的FTP服务器。

目前Discuz论坛、phpwind论坛、Wordpress个人网站等都支持远程附件功能。

本文介绍如何基于Wordpress论坛存储远程附件。

## 准备工作 {#section_ngd_phb_wdb .section}

申请OSS账号，并且创建一个public-read的bucket。这里需要权限为public-read是因为后面需要匿名访问。

## 详细步骤 {#section_mrj_qhb_wdb .section}

wordpress本身是不支持远程附件功能的，但是可以通过第三方的插件来做远程附件。作者所用wordpress版本为4.3.1, 所用插件为Hacklog Remote Attachment，以下为具体设置步骤。

1.  登录wordpress站点，选择安装插件，搜关键词FTP，选择Hacklog Remote Attachment安装。
2.  设置。
    -   FTP服务器地址, 即运行ossftp工具的地址，一般填127.0.0.1即可。
    -   FTP服务的端口号，默认为2048。
    -   FTP登录用户名，格式为AccessKeyID/BukcetName，注意这里的/不是或的意思。
    -   FTP的登录密码为AceessKeySecrete。

        **说明：** 关于AccessKeyID和AceessKeySecrete的获取，可以登录阿里云控制台的Access Key管理进行查看。

    -   FTP超时时间， 默认设置为30秒即可。
    -   远程基本URL填 `http://BucketName.Endpoint/wp`。这里测试所用bucket为test-hz-jh-002, 属于杭州区域的，所以这里填写的是`http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/wp`。
    -   FTP远程路径， 填wp表示所有附件都会存储在bucket的wp目录下。注意6和7要对应起来。
    -   HTTP远程路径，填.即可。

        具体信息见下图。

3.  验证。

    设置好之后，点击保存的同时，会做测试，测试结果会在页面上方显示，如下图所示表示测试成功。

4.  发布新文章， 并插入图片。

    现在开始写一篇新文章，并测试远程附件。创建好文章后，点击添加媒体来上传附件。

    上传附件如下图所示。

5.  上传完附件，点击发布，即可看到文章了。

    仍然通过右键点击图片，通过新建链接来打开图片即可看到图片的URL如下图所示。

    通过图片的URL，我们可以判定图片已经成功上传到了OSS。


