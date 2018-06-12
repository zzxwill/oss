# How to integrate RAM for file sharing {#concept_f5f_dkb_wdb .concept}

## Introduction {#section_wzb_fkb_wdb .section}

This document instructs you on integrating the RAM service to share files and folders in user buckets. Other users have read-only permission, while the bucket owner can edit the objects.

```
Process: Activate RAM -> Create a read-only authorization policy  -> Create sub-accounts -> Grant permissions to the sub-accounts -> Verify FTP logon
```

## Retrieve account ID {#section_cpb_jkb_wdb .section}

Retrieve your account ID,  as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/6264_en-US.png)

## Activate RAM {#section_ijs_rkb_wdb .section}

Resource Access Management \(RAM\)  is an Alibaba Cloud service designed for controlling resource access.  By creating a policy, you can create a shared read account. Users can use this account to log on to the FTP  tool 

and read your files.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/6265_en-US.png)

## Create an authorization policy {#section_g5t_xkb_wdb .section}

After activating RAM, go to the RAM console and click Policies on the left side. Follow the steps shown in the following diagram to create a new authorization policy:

Enter the authorization policy as follows:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/6266_en-US.png)

Specify policy name and remarks \(fields 1 and 2\) as needed. Policy Content in field 3 determines the policy.

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

In the preceding example, replace `****************` with your own account ID and replace `test-hz-john-001` with your bucket name. Then, copy all the content and paste it in the policy content. Finally, click **New Authorization Policy**.

## Create an account {#section_a3y_hlb_wdb .section}

The preceding authorization policy produces a read-only policy. Then, we create an account and grant this policy to the account.  Follow these steps to create an account:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/6267_en-US.png)

**Note:** Remember to record the new account’s access\_key.

## Authorize the account {#section_mtz_plb_wdb .section}

After that, we grant the new policy to the account.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4868/6268_en-US.png)

## Log on with the sub-account {#section_prc_5lb_wdb .section}

Use the sub-account’s access\_key and the bucket in the authorization policy to log on. Now, you can download files and folders, but upload operations fail.

