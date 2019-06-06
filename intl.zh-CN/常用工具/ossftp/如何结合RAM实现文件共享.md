# 如何结合RAM实现文件共享 {#concept_f5f_dkb_wdb .concept}

本文主要介绍如何结合RAM服务，共享用户Bucket中的文件以及文件夹，同时让其他用户拥有该Bucket的只读权限，而Bucket的owner则可以对该bucket进行修改。

按如下操作步骤，即可实现结合RAM服务实现文件共享。

## 获取账号ID {#section_cpb_jkb_wdb .section}

获取您的账号ID。具体参考下图步骤。

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/15598063286264_zh-CN.png)

## 开通RAM {#section_ijs_rkb_wdb .section}

访问控制（Resource Access Management，RAM）是一个稳定可靠的集中式访问控制服务。您可以通过定制策略生成一个共享读的账户，然后使用用此账户登录FTP工具并读取您的文件。

RAM的位置请参考下图。

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/15598063296265_zh-CN.png)

## 新建授权策略 {#section_g5t_xkb_wdb .section}

开通RAM之后，进入RAM控制台，单击左侧的**授权管理**，按下图步骤操作创建新的授权策略。

填写授权策略时，如下图所示。

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/15598063296266_zh-CN.png) 

其中第1步和第2步自定义填写即可，第3步的策略内容填写可参考如下示例。

``` {#codeblock_sin_fqv_sw3}
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

**说明：** 将上面的`****************`替换为您自己的账户ID，`test-hz-john-001`替换为您自己的Bucket名，然后整体拷贝到策略内容，最后单击**新建授权策略**即可。

## 创建用户 {#section_a3y_hlb_wdb .section}

以上的授权策略生成了一种只读策略，下面新建一个用户并给予该用户只读策略。新建用户步骤如下：

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/15598063296267_zh-CN.png) 

**说明：** 注意保存新用户的AccessKey。

## 给用户授权 {#section_mtz_plb_wdb .section}

将之前创建的策略授权给该用户。

 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/15598063296268_zh-CN.png)

## 用子账户登录 {#section_prc_5lb_wdb .section}

用子账户的AccessKey和之前授权策略中的Bucket登录，即可下载文件夹或文件，但上传会失败。

