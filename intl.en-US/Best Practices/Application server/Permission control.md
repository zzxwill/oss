# Permission control {#concept_i1m_lvx_5db .concept}

This document elaborates how to configure different policies to implement different permission controls based on the app server mentioned in [Set up direct data transfer for mobile apps](intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#) by taking the app-base-oss bucket in the Shanghai region as an example.

**Note:** 

-   The following illustration assumes you have already activated STS and have thoroughly read the [Set up direct data transfer for mobile apps](intl.en-US/Best Practices/Application server/Set up direct data transfer for mobile apps.md#) document.

-   The policies mentioned in the following content are covered in the specified policy file in the config.json file mentioned in the previous section.

-   The operations on OSS upon retrieving the STS token indicate the process of specifying the policy for the app server, the app server retrieving a temporary credential from the STS and the app using the temporary credential to access OSS.


## Common policies {#section_rgn_pvx_5db .section}

-   Full authorization policy

    For the ease of demonstration, the default policy is shown as follows. This policy indicates that the app is allowed to perform any operation on OSS.

    **Note:** This policy is neither secured nor recommended to use for mobile apps.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Successful|
    |Upload the object without a prefix, test.txt.|Successful|
    |Download the object without a prefix, test.txt.|Successful|
    |Upload the object with a prefix, user1/test.txt.|Successful|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|

-   Read-only policies with or without any prefixes

    This policy indicates the app can list and download all objects in the bucket app-base-oss.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Failed|
    |Download the object without a prefix, test.txt.|Successful|
    |Upload the object with a prefix, user1/test.txt.|Failed|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|

-   Read-only policies with a specified prefix

    This policy indicates the app can list and download all objects with the prefix of \*\*user1/\*\* in the bucket \*\*app-base-oss\*\*. However, the policy does not specify to download any objects with another prefix. By this way, different apps corresponding to different prefixes are spatially isolated in the bucket.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Failed|
    |Download the object without a prefix, test.txt.|Failed|
    |Upload the object with a prefix, user1/test.txt.|Failed|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|

-   Write-only policies with no specified prefixes

    This policy indicates that the app can upload all objects in the bucket app-base-oss.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Successful|
    |Download the object without a prefix, test.txt.|Failed|
    |Upload the object with a prefix, user1/test.txt.|Successful|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|

-   Write-only policies with a specified prefix

    This policy indicates the app can upload all objects with the user1/ prefix in the bucket app-base-oss. The app cannot upload any object with another prefix. In this way, different apps corresponding to different prefixes are spatially isolated in the bucket.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Failed|
    |Download the object without a prefix, test.txt.|Failed|
    |Upload the object with a prefix, user1/test.txt.|Failed|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Failed|
    |List the object with a prefix, user1/test.txt.|Failed|

-   Read/write policies with or without any prefixes

    This policy indicates that the app can list, download, upload, and delete all objects in the bucket `app-base-oss`.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Successful|
    |Download the object without a prefix, test.txt.|Successful|
    |Upload the object with a prefix, user1/test.txt.|Successful|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|

-   Read/write policies with a specified prefix

    This policy indicates the app can list, download, upload, and delete all objects with a prefix of `user1/` in the bucket `app-base-oss`. The policy does not specify to read or write any objects with another prefix. In this way, different apps corresponding to different prefixes are spatially isolated in the bucket.

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

    |Operations on OSS upon retrieving STS token|Result|
    |:------------------------------------------|:-----|
    |List all created buckets.|Failed|
    |Upload the object without a prefix, test.txt.|Failed|
    |Download the object without a prefix, test.txt.|Failed|
    |Upload the object with a prefix, user1/test.txt.|Successful|
    |Download the object with a prefix, user1/test.txt.|Successful|
    |List the object without a prefix, test.txt.|Successful|
    |List the object with a prefix, user1/test.txt.|Successful|


## Summary {#section_mnb_gyx_5db .section}

With the help of preceding examples, we can understand that:

-   You can create different policies for various app scenarios and then achieve differentiated permission control for different apps through slight modifications on the app server.
-   You can also optimize apps to save the process of making another request to the app server before the STS token expires. 
-   Tokens are actually issued by the STS. An app server customizes a policy, requests for a token from the STS, and then delivers this token to the app. Here, token is only a shorthand expression. However, a "token" actually contains an "AccessKeyId", an "AccessKeySecret", an "Expiration" value, and a "SecurityToken". These are used in the SDK provided by OSS to the app. For more information, see the implementation of the respective SDK.

More references:

-   [How to use RAM and STS in OSS](intl.en-US/Best Practices/Access control/Overview.md#)

-   [RAM documentation](https://www.alibabacloud.com/help/doc-detail/28672.htm) and [STS documentation](https://www.alibabacloud.com/help/doc-detail/28756.htm)

