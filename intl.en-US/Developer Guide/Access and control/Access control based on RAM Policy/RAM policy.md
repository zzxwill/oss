# RAM policy {#concept_y5r_5rm_2gb .concept}

Resource Access Management \(RAM\) is a service provided by Alibaba Cloud for resource access control. RAM policies are configured based on users. By configuring RAM policies, you can manage multiple users in a centralized manner and control the resources that can be accessed by the users. For example, you can control the permission of a user so that the user can only read a specified bucket. A RAM user belongs to the Alibaba Cloud account under which it was created, and does not own any actual resources. That is, all resources belong to the corresponding Alibaba Cloud account.

## Policy examples {#section_rgn_pvx_5db .section}

-   Policies that grant full permissions

    A policy that grants full permissions allows applications to perform all operations on OSS.

    **Warning:** We recommend that you do not use a policy that grants full permissions for mobile applications because it is not secure.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Success|
    |Upload an object without a prefix, such as text.txt.|Success|
    |Download an object without a prefix, such as text.txt.|Success|
    |Upload an object with a prefix, such as user1/test.txt.|Success|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|

-   Read-only policies for all objects

    A read-only policy indicates that an application can list and download all objects in the bucket `app-base-oss`.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Failed|
    |Download an object without a prefix, such as test.txt.|Success|
    |Upload an object with a prefix, such as user1/test.txt.|Failed|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|

-   Read-only policies for objects with a specified prefix

    This kind of policy indicates that an application can list and download objects with the `user1/`prefix in the bucket `app-base-oss` but cannot download objects with other prefixes. Using this kind of policy, you can isolate applications with different prefixes in a bucket.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Failed|
    |Download an object without a prefix, such as text.txt.|Failed|
    |Upload an object with a prefix, such as user1/test.txt.|Failed|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|

-   Write-only policies for all objects

    A write-only policy for all objects indicates that an application can upload objects to the bucket `app-base-oss`.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Success|
    |Download an object without a prefix, such as text.txt.|Failed|
    |Upload an object with a prefix, such as user1/test.txt.|Success|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|

-   Write-only policies for objects with a specified prefix

    This kind of policy indicates that an application can upload objects with the prefix `user1/` to the bucket `app-base-oss`. However, the application cannot upload objects with other prefixes. Using this kind of policy, you can isolate applications with different prefixes in a bucket.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Failed|
    |Download an object without a prefix, such as test.txt.|Failed|
    |Upload an object with a prefix, such as user1/test.txt.|Success|
    |Download an object with a prefix, such as user1/test.txt.|Failed|
    |List objects without prefixes, such as test.txt.|Failed|
    |List objects with prefixes, such as user1/test.txt.|Failed|

-   Read/write policies for all objects

    A read/write policy for all objects indicates that an application can upload objects to the bucket `app-base-oss` and list, download, and delete all objects in the bucket.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Success|
    |Download an object without a prefix, such as text.txt.|Success|
    |Upload an object with a prefix, such as user1/test.txt.|Success|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|

-   Read/write policies for objects with a specified prefix

    This kind of policy indicates that an application can upload objects with the prefix `user1/` to the bucket `app-base-oss` and list, download, and delete all objects with the prefix in the bucket. However, the application cannot perform read or write operations on objects with other prefixes. Using this kind of policy, you can isolate applications with different prefixes in a bucket.

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

    |Operations on OSS|Result|
    |:----------------|:-----|
    |List all created buckets.|Failed|
    |Upload an object without a prefix, such as text.txt.|Failed|
    |Download an object without a prefix, such as text.txt.|Failed|
    |Upload an object with a prefix, such as user1/test.txt.|Success|
    |Download an object with a prefix, such as user1/test.txt.|Success|
    |List objects without prefixes, such as test.txt.|Success|
    |List objects with prefixes, such as user1/test.txt.|Success|


## Complex policy examples {#section_tcr_v1n_2gb .section}

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

The preceding example describes a complex authorization policy. By using this policy, a user can authorize other users through RAM or STS. In the policy, a statement is included \(a policy can include multiple statements\), in which Action, Resource, Effect, and Condition are specified.

This policy grant permissions to authorized users so that they can access your resources, such as `mybucket` and `mybucket/file*`. In addition, this policy supports the following operations: GetBucketAcl, GetBucket, PutObject, GetObject, and DeleteObject. Conditions included in Condition indicate that authentication is successful and authorized users can access related resources only when UserAgent is java-sdk and the source IP address is 192.168.0.1. The Prefix condition is used when the GetBucket \(ListObjects\) action is performed. For more information about the field, see OSS API documentation.

## Version {#section_nhn_ksm_2gb .section}

The Version field specifies the version of the policy. For the configuration method in this document, it is set to 1.

## Statement {#section_vmx_lsm_2gb .section}

A statement describes the authorization semantics. According to different scenarios, a statement can include multiple semantics which include Action, Effect, Resource, and Condition individually. When receiving a request, the system checks all Statements in the policy. All Statements that match the request are classified into two categories based on their Effect settings: Allow or Deny, in which Deny statements have higher priority when the system determines whether the authentication is successful. If all matched statements are classified into Allow, the request passes the authentication. If a matched statement is classified into Deny, or no statement matches the request, the request is rejected.

## Action {#section_x3c_nsm_2gb .section}

Actions can be classified into three categories:

-   Service-level actions: include the GetService action used to list the buckets owned by a user.
-   Bucket-level actions: indicate actions performed on buckets, such as oss:PutBucketAcl and oss:GetBucketLocation. The name of each action corresponds to an API.
-   Object-level actions: indicate actions performed on objects, such as oss:GetObject, oss:PutObject, oss:DeleteObject, and oss:AbortMultipartUpload.

To authorize a type of actions on objects, you can select one or more of the preceding actions. In addition, all action names must be prefixed with `oss:`, as shown in the preceding example. The Action field is a list that can include multiple actions. The following tables show the mapping relationship between actions and APIs.

-   Service-level actions

    |API|Action|
    |:--|:-----|
    |GetService（ListBuckets）|oss:ListBuckets|

-   Bucket-level actions

    |API|Action|
    |:--|:-----|
    |PutBucket|oss:PutBucket|
    |GetBucket \(ListObjects\)|oss:ListObjects|
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

-   Actions on objects

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
    |DeleteMultipleObjects|oss:DeleteObject|
    |AbortMultipartUpload|oss:AbortMultipartUpload|
    |ListParts|oss:ListParts|
    |CopyObject|oss:GetObject,oss:PutObject|
    |UploadPartCopy|oss:GetObject,oss:PutObject|
    |AppendObject|oss:PutObject|
    |GetObjectAcl|oss:GetObjectAcl|
    |PutObjectAcl|oss:PutObjectAcl|
    |RestoreObject|oss:RestoreObject|


## Resource {#section_hff_tsm_2gb .section}

The resource field indicates specified resources or a kind of resources \(which can be represented by a wildcard \*\). The format of a resource is as follows: `acs:oss:{region}:{bucket_owner}:{bucket_name}/{object_name}`. The "/\{object\_name\}" part is not required for the names of bucket-level actions. The format of a resource for a bucket-level action is as follows: `acs:oss:{region}:{bucket_owner}:{bucket_name}`. The Resource field is a list that can include multiple resources. The region field is not supported currently and is set to \* in the preceding example.

## Effect {#section_qhr_tsm_2gb .section}

The Effect field indicates the authorization result of this statement and has two values:Allow and Deny. When multiple statements match a request, statements in which the value of Effect is Deny has higher priority.

For example, the following policy prohibits users from deleting a specified directory but allows them to perform all operations on other objects.

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

The Condition field indicates the conditions for the authorization policy. In the preceding example, you can set checking conditions for acs:UserAgent and acs:SourceIp, and use oss:Prefix as a condition to restrict resources when the GetBucket action is performed.

OSS supports the following conditions

|Condition|Function|Valid value|
|:--------|:-------|:----------|
|acs:SourceIp|Specifies the source IP address or IP range.|IP address or IP range, wildcards \(\*\) supported|
|acs:UserAgent|Specifies the http useragent header.|String|
|acs:CurrentTime|Specifies a valid access time.|Time in the ISO8601 format|
|acs:SecureTransport|Indicates whether the HTTPS protocol is used.|"true" or "false"|
|oss:Prefix|Indicates the prefix used when the ListObjects action is performed.|Valid object name|

## Best practice {#section_twm_zsm_2gb .section}

OSS provides [Ram Policy Editor](../../../../reseller.en-US/Tools/RAM Policy Editor.md#) that can help you generate a RAM policy quickly. You can also [Grant permissions with a simple policy](../../../../reseller.en-US/Tools/ossbrowser/Permission management.md#section_zyx_1k3_wdb) by using ossbrowser, a graphical management tool to authorize a RAM user so that it can access specified buckets or directories.

For more examples of configuring authorization policies in different scenarios, see [Tutorial: control access to buckets and objects](reseller.en-US/Developer Guide/Access and control/Access control based on RAM Policy/Tutorial: Use RAM Policy to control access to buckets and folders.md#) and [Authorization for OSS](https://partners-intl.aliyun.com/help/doc-detail/58905.htm).

