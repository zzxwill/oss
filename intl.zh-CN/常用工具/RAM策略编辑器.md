# RAM策略编辑器 {#concept_mx2_yb4_vdb .concept}

## 地址 {#section_qst_zb4_vdb .section}

[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)

## 使用 {#section_vwl_2c4_vdb .section}

RAM授权策略由若干条规则组成，使用RAM策略编辑器，可以在界面上逐条添加/删除规则，并自动生成策略的JSON文本。用户添加完所有规则后，只需要将JSON文本拷贝，然后粘贴到访问控制（RAM）控制台的创建授权策略内容框内。

具体操作请参见[创建自定义授权策略](https://www.alibabacloud.com/help/doc-detail/28640.htm)。

RAM策略编辑器中，每条规则需要设置其Effect、Actions、Resources和Conditions：

-   Effect

    指定这条规则是允许访问（Allow）还是禁止访问（Deny）。

-   Actions

    指定访问资源的动作，可以选择多项。一般来说用户使用提供的通配动作就足够了：

    -   `oss:*`表示允许所有动作。
    -   `oss:Get*`表示允许所有的读动作。
    -   `oss:Put*`表示允许所有的写动作。
    更多信息请参见[RAM Policy Editor README](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md)。

-   Resources

    指定授权访问的OSS的资源，可以指定多个，每个是以下形式：

    -   表示某个bucket：`my-bucket` （此时对bucket下的文件没有权限）
    -   表示某个bucket下面所有文件：`my-bucket/*` （此时对bucket本身没有权限，例如ListObjects）
    -   表示某个bucket下某个目录：`my-bucket/dir` （此时对dir/下面的文件没有权限）
    -   表示某个bucket下某个目录下面所有文件：`my-bucket/dir/*` （此时对dir没有权限，例如ListObjects）
    -   填写完整的资源路径：`acs:oss:*:1234:my-bucket/dir`，其中`1234`为用户的User ID（在控制台查看）
    EnablePath

    当用户需要对某个目录授权时，往往还需要保证对上一层目录也有List权限，例如用户对`my-bucket/users/dir/*`赋予读写权限，为了在控制台（或其他工具）能够查看这个目录，用户还需要以下权限：

    ```
    ListObjects my-bucket
    ListObjects my-bucket/users
    ListObjects my-bucket/users/dir
    ```

    勾选EnablePath选项时，上面这些权限会自动添加。

-   Conditions

    指定授权访问时应该满足的条件，可以指定多个。


## 例子 {#section_lpb_xc4_vdb .section}

授权对`my-bucket`及其文件全部的权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4904/15330935272440_zh-CN.png)

更多例子请参见[RAM Policy Editor](https://github.com/aliyun/ram-policy-editor/blob/master/README-CN.md)。

