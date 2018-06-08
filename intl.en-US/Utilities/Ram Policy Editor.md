# Ram Policy Editor {#concept_mx2_yb4_vdb .concept}

## Address {#section_qst_zb4_vdb .section}

[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)

## Usage {#section_vwl_2c4_vdb .section}

RAM authorization policies are composed of several rules. Using the RAM policy editor, you can add or delete rules one by one in the interface, and then a JSON file is automatically generated for the policy. After adding all the policy rules, copy the JSON file and paste it in the created authorization policy content box on the Access Control console. 

For detailed operation, see [Create an authorization policy](https://www.alibabacloud.com/help/doc-detail/28640.htm).

In the RAM policy editor, you must set these fields for each rule: Effect, Actions, Resources, and Conditions.

-   Effect

    Specify whether access to this rule is allowed or denied.

-   Actions

    Specify resource access actions. You can select one or more actions. Generally, it is sufficient to use the wildcard action provided for users:

    -   `oss:*`: allows all actions
    -   `oss:Get*` allows all read actions
    -   `oss:Put*` allows all write actions
    For more information, see [RAM Policy Editor README](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md).

-   Resources

    Specify the resources of the OSS authorized to access. You can specify multiple ones, and each would be represented in the following format:

    -   A bucket: `my-bucket`  \(with no permission on objects in the bucket\)
    -   All objects in a bucket: `my-bucket/*` \(with no permission on the bucket itself, such as ListObjects\)
    -   A directory in a bucket: `my-bucket/dir`  \(with no permission on objects under dir/\)
    -   All objects under a directory in a bucket: `my-bucket/dir/*`  \(with no permission on dir, such as ListObjects\)
    -   Complete resource path: `acs:oss:*:1234:my-bucket/dir`, `1234` is the user ID \(viewed in the console\)
    EnablePath

    When you want to grant permissions to a directory, you usually need to grant the List permission on its upper level directory. For example, if you want to grant read and write permissions to `my-bucket/users/dir/*`, you also need to grant the following permissions so as to view this directory in the console \(or in other tools\):

    ```
    ListObjects my-bucket
    ListObjects my-bucket/users
    ListObjects my-bucket/users/dir
    ```

    When the EnablePath option is selected, the preceding permissions are automatically added.

-   Conditions

    Specify the conditions that must be met for authorized access. You can specify multiple ones.

    For more information, see [RAM Policy Editor README](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md).


## Example {#section_lpb_xc4_vdb .section}

To grant all permissions for `my-bucket` and its files:

![](images/2440_en-US.png)

For more examples, see [RAM Policy Editor README](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md).

