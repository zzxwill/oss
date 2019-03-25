# 如何构建RAM Policy {#concept_y5r_5rm_2gb .concept}

RAM （Resource Access Management）是阿里云提供的资源访问控制服务。RAM Policy是基于用户的授权策略。通过设置RAM Policy，您可以集中管理您的用户（比如员工、系统或应用程序），以及控制用户可以访问您名下哪些资源的权限。比如能够限制您的用户只拥有对某一个 Bucket 的读权限。子账号是从属于主账号的，并且这些账号下不能拥有实际的任何资源，所有资源都属于主账号。

**说明：** 您可以使用[RAM策略编辑器](../../../../../intl.zh-CN/常用工具/RAM策略编辑器.md#)快速生成RAM Policy。

## 常见Policy示例 {#section_rgn_pvx_5db .section}

-   完全授权的Policy

    完全授权的Policy表示允许应用可以对OSS进行任何操作。

    **警告：** 完全授权的Policy对移动应用来说是不安全的授权，不推荐使用。

    ```
    {
      "Statement": [
        {
          "Action": [
            "oss:*"
          ],
          "Effect": "Allow",
          "Resource": ["acs:oss:*:*:*"]
        }
      ],
      "Version": "1"
    }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|成功|
    |上传不带前缀的Object，test.txt|成功|
    |下载不带前缀的Object，test.txt|成功|
    |上传带前缀的Object，user1/test.txt|成功|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举Object，test.txt|成功|
    |带前缀的Object，user1/test.txt|成功|

-   不限制前缀的只读不写Policy

    此Policy表示应用对Bucket`app-base-oss`下所有的Object可列举，可下载。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:GetObject",
              "oss:ListObjects"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|失败|
    |下载不带前缀的Object，test.txt|成功|
    |上传带前缀的Object，user1/test.txt|失败|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举不带前缀的Object，test.txt|成功|
    |列举带前缀的Object，user1/test.txt|成功|

-   限制前缀的只读不写Policy

    此Policy表示应用对Bucket`app-base-oss`下带有前缀`user1/`的Object可列举、可下载，但无法下载其他前缀的Object。采用此种Policy，如果不同的应用对应不同的前缀，就可以达到在同一个Bucket中空间隔离的效果。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:GetObject",
              "oss:ListObjects"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/user1/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|失败|
    |下载不带前缀的Object，test.txt|失败|
    |上传带前缀的Object，user1/test.txt|失败|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举Object，test.txt|成功|
    |带前缀的Object，user1/test.txt|成功|

-   不限制前缀的只写不读Policy

    此Policy表示应用可以对Bucket`app-base-oss`下所有的Object进行上传。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:PutObject"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|成功|
    |下载不带前缀的Object，test.txt|失败|
    |上传带前缀的Object，user1/test.txt|成功|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举Object，test.txt|成功|
    |带前缀的Object，user1/test.txt|成功|

-   限制前缀的只写不读Policy

    此Policy表示应用可以对Bucket`app-base-oss`下带有前缀`user1/`的Object进行上传。但无法上传其他前缀的Object。采用此种Policy，如果不同的应用对应不同的前缀，就可以达到在同一个Bucket中空间隔离的效果。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:PutObject"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/user1/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|失败|
    |下载不带前缀的Object，test.txt|失败|
    |上传带前缀的Object，user1/test.txt|成功|
    |下载带前缀的Object，user1/test.txt|失败|
    |列举Object，test.txt|失败|
    |带前缀的Object，user1/test.txt|失败|

-   不限制前缀的读写Policy

    此Policy表示应用可以对Bucket`app-base-oss`下所有的Object进行列举、下载、上传和删除。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:GetObject",
              "oss:PutObject",
              "oss:DeleteObject",
              "oss:ListParts",
              "oss:AbortMultipartUpload",
              "oss:ListObjects"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|成功|
    |下载不带前缀的Object，test.txt|成功|
    |上传带前缀的Object，user1/test.txt|成功|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举Object，test.txt|成功|
    |带前缀的Object，user1/test.txt|成功|

-   限制前缀的读写Policy

    此Policy表示应用可以对Bucket`app-base-oss`下带有前缀`user1/`的Object进行列举、下载、上传和删除，但无法对其他前缀的Object进行读写。采用此种Policy，如果不同的应用对应不同的前缀，就可以达到在同一个Bucket中空间隔离的效果。

    ```
    {
        "Statement": [
          {
            "Action": [
              "oss:GetObject",
              "oss:PutObject",
              "oss:DeleteObject",
              "oss:ListParts",
              "oss:AbortMultipartUpload",
              "oss:ListObjects"
            ],
            "Effect": "Allow",
            "Resource": ["acs:oss:*:*:app-base-oss/user1/*", "acs:oss:*:*:app-base-oss"]
          }
        ],
        "Version": "1"
      }
    ```

    |对OSS的操作|结果|
    |:------|:-|
    |列举所有创建的Bucket|失败|
    |上传不带前缀的Object，test.txt|失败|
    |下载不带前缀的Object，test.txt|失败|
    |上传带前缀的Object，user1/test.txt|成功|
    |下载带前缀的Object，user1/test.txt|成功|
    |列举Object，test.txt|成功|
    |带前缀的Object，user1/test.txt|成功|


## 复杂Policy示例 {#section_tcr_v1n_2gb .section}

```
{
    "Version": "1",
    "Statement": [
        {
            "Action": [
                "oss:GetBucketAcl",
                "oss:ListObjects"
            ],
            "Resource": [
                "acs:oss:*:1775305056529849:mybucket"
            ],
            "Effect": "Allow",
            "Condition": {
                "StringEquals": {
                    "acs:UserAgent": "java-sdk",
                    "oss:Prefix": "foo"
                },
                "IpAddress": {
                    "acs:SourceIp": "192.168.0.1"
                }
            }
        },
        {
            "Action": [
                "oss:PutObject",
                "oss:GetObject",
                "oss:DeleteObject"
            ],
            "Resource": [
                "acs:oss:*:1775305056529849:mybucket/file*"
            ],
            "Effect": "Allow",
            "Condition": {
                "IpAddress": {
                    "acs:SourceIp": "192.168.0.1"
                }
            }
        }
    ]
}
```

这是一个较为复杂的授权 Policy，用户用这样的一个 Policy 通过 RAM 或 STS 服务向其他用户授权。Policy 当中有一个 Statement（一条 Policy 当中可以有多条 Statement）。Statement 里面规定了相应的 Action、Resource、Effect 和 Condition。

这条 Policy 把用户自己名下的`mybucket`和`mybucket/file*`这些资源授权给相应的用户，并且支持 GetBucketAcl、GetBucket、PutObject、GetObject 和 DeleteObject 这几种操作。Condition 中的条件表示 UserAgent为 java-sdk，源IP为192.168.0.1的时候鉴权才能通过，被授权的用户才能访问相关的资源。Prefix 这个 Condition 是在 GetBucket（ListObjects）的时候起作用的，关于这个字段的解释详见 OSS的 API 文档。

## Version {#section_nhn_ksm_2gb .section}

Version定义了Policy的版本，本文档中 Version 设置为1。

## Statement {#section_vmx_lsm_2gb .section}

通过Statement描述授权语义，其中可以根据业务场景包含多条语义，每条包含对 Action、Effect、Resource 和 Condition 的描述。每次请求系统会逐条依次匹配检查，所有匹配成功的 Statement 会根据 Effect 的设置不同分为通过（Allow）、禁止（Deny），其中禁止（Deny）的优先。如果匹配成功的都为通过，该条请求即鉴权通过。如果匹配成功有一条禁止，或者没有任何条目匹配成功，该条请求被禁止访问。

## Action {#section_x3c_nsm_2gb .section}

Action 分为三大类：

-   Service 级别操作，对应的是 GetService 操作，用来列出所有属于该用户的 Bucket 列表。
-   Bucket 级别操作，对应类似于 oss:PutBucketAcl、oss:GetBucketLocation之类的操作，操作的对象是 Bucket，它们的名称和相应的接口名称一一对应。
-   Object 级别操作，分为 oss:GetObject、oss:PutObject、oss:DeleteObject和oss:AbortMultipartUpload，操作对象是 Object。

如想授权某一类的 Object 的操作，可以选择这几种的一种或几种。另外，所有的 Action 前面都必须加上`oss:`，如上面例子所示。Action 是一个列表，可以有多个 Action。具体的 Action 和 API 接口的对应关系如下：

-   Service级别

    |API|Action|
    |:--|:-----|
    |GetService（ListBuckets）|oss:ListBuckets|

-   Bucket 级别

    |API|Action|
    |:--|:-----|
    |PutBucket|oss:PutBucket|
    |GetBucket（ListObjects）|oss:ListObjects|
    |PutBucketAcl|oss:PutBucketAcl|
    |DeleteBucket|oss:DeleteBucket|
    |GetBucketLocation|oss:GetBucketLocation|
    |GetBucketAcl|oss:GetBucketAcl|
    |GetBucketLogging|oss:GetBucketLogging|
    |PutBucketLogging|oss:PutBucketLogging|
    |DeleteBucketLogging|oss:DeleteBucketLogging|
    |GetBucketWebsite|oss:GetBucketWebsite|
    |PutBucketWebsite|oss:PutBucketWebsite|
    |DeleteBucketWebsite|oss:DeleteBucketWebsite|
    |GetBucketReferer|oss:GetBucketReferer|
    |PutBucketReferer|oss:PutBucketReferer|
    |GetBucketLifecycle|oss:GetBucketLifecycle|
    |PutBucketLifecycle|oss:PutBucketLifecycle|
    |DeleteBucketLifecycle|oss:DeleteBucketLifecycle|
    |ListMultipartUploads|oss:ListMultipartUploads|
    |PutBucketCors|oss:PutBucketCors|
    |GetBucketCors|oss:GetBucketCors|
    |DeleteBucketCors|oss:DeleteBucketCors|
    |PutBucketReplication|oss:PutBucketReplication|
    |GetBucketReplication|oss:GetBucketReplication|
    |DeleteBucketReplication|oss:DeleteBucketReplication|
    |GetBucketReplicationLocation|oss:GetBucketReplicationLocation|
    |GetBucketReplicationProgress|oss:GetBucketReplicationProgress|

-   Object 级别

    |API|Action|
    |:--|:-----|
    |GetObject|oss:GetObject|
    |HeadObject|oss:GetObject|
    |PutObject|oss:PutObject|
    |PostObject|oss:PutObject|
    |InitiateMultipartUpload|oss:PutObject|
    |UploadPart|oss:PutObject|
    |CompleteMultipart|oss:PutObject|
    |DeleteObject|oss:DeleteObject|
    |DeleteMultipartObjects|oss:DeleteObject|
    |AbortMultipartUpload|oss:AbortMultipartUpload|
    |ListParts|oss:ListParts|
    |CopyObject|oss:GetObject,oss:PutObject|
    |UploadPartCopy|oss:GetObject,oss:PutObject|
    |AppendObject|oss:PutObject|
    |GetObjectAcl|oss:GetObjectAcl|
    |PutObjectAcl|oss:PutObjectAcl|
    |RestoreObject|oss:RestoreObject|


## Resource {#section_hff_tsm_2gb .section}

Resource 指代的是 OSS 上面的某个具体的资源或者某些资源（支持\*通配），resource的规则是`acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`。对于所有 Bucket 级别的操作来说不需要最后的斜杠和\{object\_name\}，即`acs:oss:{region}:{bucket_owner}:{bucket_name}`。Resource 也是一个列表，可以有多个 Resource。其中的 region 字段暂时不做支持，设置为\*。

## Effect {#section_qhr_tsm_2gb .section}

Effect 代表本条的Statement的授权的结果，分为 Allow 和 Deny，分别指代通过和禁止。多条 Statement 同时匹配成功时，禁止（Deny）的优先级更高。

例如，期望禁止用户对某一目录进行删除，但对于其他文件有全部权限：

```
{
  "Version": "1",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "oss:*"
      ],
      "Resource": [
        "acs:oss:*:*:bucketname"
      ]
    },
    {
      "Effect": "Deny",
      "Action": [
        "oss:DeleteObject"
      ],
      "Resource": [
        "acs:oss:*:*:bucketname/index/*",
      ]
    }
  ]
}
```

## Condition {#section_zhg_wsm_2gb .section}

Condition 代表 Policy 授权的一些条件，上面的示例里面可以设置对于 acs:UserAgent 的检查、acs:SourceIp 的检查，还有 oss:Prefix 项用来在 GetBucket 的时候对资源进行限制。

OSS 支持的 Condition 如下：

|condition|功能|合法取值|
|:--------|:-|:---|
|acs:SourceIp|指定 ip 网段|普通的 ip，支持\*通配|
|acs:UserAgent|指定 http useragent 头|字符串|
|acs:CurrentTime|指定合法的访问时间|ISO8601格式|
|acs:SecureTransport|是否是 https 协议|“true”或者”false”|
|oss:Prefix|用作 ListObjects 时的 prefix|合法的object name|

## 最佳实践 {#section_twm_zsm_2gb .section}

OSS提供了[RAM策略编辑器](../../../../../intl.zh-CN/常用工具/RAM策略编辑器.md#)帮助您快速生成RAM Policy。您也可以使用图形化管理工具 ossbrowser 的[简化 Policy 授权](../../../../../intl.zh-CN/常用工具/图形化管理工具ossbrowser/权限管理.md#section_zyx_1k3_wdb)，一键完成对子账号授予特定 Bucket 或特定目录的权限 。

针对具体场景更多的授权策略配置示例，可以参考[教程示例：控制存储空间和文件夹的访问权限](intl.zh-CN/开发指南/权限控制/基于RAM Policy的权限控制/教程示例：使用RAM Policy控制存储空间和文件夹的访问权限.md#)和[OSS 授权常见问题](https://www.alibabacloud.com/help/faq-detail/58905.htm)。

