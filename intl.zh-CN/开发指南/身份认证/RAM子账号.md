# RAM子账号 {#concept_wdd_hvk_2gb .concept}

在您的阿里云账号下面，通过 RAM 可以创建具有自己 AccessKey 的子用户。您的阿里云账号被称为主账号，创建出来的账号被称为子账号，使用子账号的 AccessKey 只能使用主账号授权的操作和资源。

## 使用场景 {#section_rkf_xxk_2gb .section}

如果您购买了云资源，您的组织里有多个用户需要使用这些云资源，这些用户只能共享使用您的云账号 AccessKey。这里有两个问题：

-   您的密钥由多人共享，泄露的风险很高。
-   您无法控制特定用户能访问哪些资源（比如 Bucket）的权限。

此时，您可以创建 RMA 子账号，并授予子账号对应的权限。之后，让您的用户通过子账号访问或管理您的资源。

## 具体实现 {#section_ux5_1yk_2gb .section}

关于RAM的详细介绍和RAM子账号的创建，请参见[RAM用户指南](../../../../../intl.zh-CN/用户指南/概述.md#)。通过构建RAM Policy实现对OSS资源访问的授权，详情请参见[RAM Policy](intl.zh-CN/开发指南/权限控制/基于RAM Policy的权限控制/如何构建RAM Policy.md#)。

