# 如何结合RAM实现文件共享 {#concept_f5f_dkb_wdb .concept}

## 简介 {#section_wzb_fkb_wdb .section}

本文主要介绍如何结合RAM服务，共享用户bucket中的文件/文件夹, 让其他用户只读，而bucket的owner可以修改。

```
思路就是: 开通RAM -> 新建只读授权策略 -> 创建子用户 -> 向子用户授权  ->  FTP登录验证
```

## 获取账户ID {#section_cpb_jkb_wdb .section}

获取你的account ID。具体参考下图步骤。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/2833_zh-CN.png)

## 开通RAM {#section_ijs_rkb_wdb .section}

访问控制（Resource Access Management，RAM）是一个稳定可靠的集中式访问控制服务。您可以通过定制策略，产生一个共享读的账户，使得用户可以用此账户登录FTP 工具并 读取您的文件。

RAM的位置请参考下图。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/2834_zh-CN.png)

## 新建授权策略 {#section_g5t_xkb_wdb .section}

开通RAM之后，进入RAM控制台，点击左侧的授权管理，按下图步骤依次操作来创建新的授权策略。

填写授权策略时，如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/2836_zh-CN.png)

其中第1步和第2步自定义填写即可，第3步的策略内容才是最关键的地方。

```
{
  "Version": "1",
  "Statement": [
    {
      "Action": [
        "oss:GetObject",
        "oss:HeadObject"
      ],
      "Resource": [
        "acs:oss:*:****************:test-hz-john-001/*"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "oss:ListObjects",
        "oss:GetBucketAcl",
        "oss:GetBucketLocation"
      ],
      "Resource": [
        "acs:oss:*:****************:test-hz-john-001"
      ],
      "Effect": "Allow"
    },
    {
      "Action": [
        "oss:ListBuckets"
      ],
      "Resource": [
        "acs:oss:*:****************:*"
      ],
      "Effect": "Allow"
    }
  ]
}
```

将上面的`****************`替换为你自己的账户ID，`test-hz-john-001`替换为你自己的bucket名，然后整体拷贝到策略内容里面，最后点击**新建授权策略**即可。

## 创建用户 {#section_a3y_hlb_wdb .section}

上面的授权策略生命了一种只读策略，下面新建一个用户并给予他这种策略。先是创建用户，步骤如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/2837_zh-CN.png)

**说明：** 注意保存新用户的access\_key。

## 给用户授权 {#section_mtz_plb_wdb .section}

下面将之前创建的策略授权给该用户。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/2838_zh-CN.png)

## 用子账户登录 {#section_prc_5lb_wdb .section}

用子账户的access\_key和之前授权策略中的bucket登录，即可下载文件夹或文件，但上传会失败。

