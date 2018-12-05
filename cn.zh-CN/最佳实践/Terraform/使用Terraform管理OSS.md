# 使用Terraform管理OSS {#concept_nqx_wps_zfb .concept}

本文主要介绍如何安装配置 Terraform 及使用 Terraform 管理 OSS。

## 安装和配置 Terraform {#section_twh_cqs_zfb .section}

使用 Terraform 前，您需要参考以下步骤安装并配置 Terraform：

1.  前往 [Terraform 官网](https://www.terraform.io/downloads.html) 下载适用于您的操作系统的程序包，本文以 Linux 系统为例。
2.  将程序包解压到 /usr/local/bin。如果将可执行文件解压到其他目录，则需要将路径加入到全局变量。
3.  运行 Terraform 验证路径配置，若显示可用的 Terraform 选项的列表，表示安装完成。

    ```
    [root@test bin]#terraform
    Usage: terraform [-version] [-help] <command> [args]
    ```

4.  创建 RAM 用户，并为其授权。

    1.  登录 [RAM 控制台](https://ram.console.aliyun.com/#/overview)。
    2.  创建名为 Terraform 的 RAM 用户，并为该用户创建 AccessKey。具体步骤参见 [创建 RAM 用户](https://help.aliyun.com/document_detail/28637.html#concept-gpm-ccf-xdb)。
    3.  为 RAM 用户授权。您可以根据实际的情况为 Terraform 授予合适的管理权限。具体步骤参见 [为 RAM 用户授权](https://help.aliyun.com/document_detail/28639.html#concept-t13-3gf-xdb)。
    **说明：** 请不要使用主账号的 AccessKey 配置 Terraform 工具。

5.  因为每个 Terraform 项目都需要创建 1 个独立的执行目录，所以先创建一个测试目录terraform-test。

    ```
    [root@test bin]#mkdir terraform-test
    ```

6.  进入 terraform-test 目录：

    ```
    [root@test bin]#cd terraform-test
    [root@test terraform-test]#
    ```

7.  创建配置文件。Terraform 在运行时，会读取该目录空间下所有\*.tf和\*.tfvars 文件。因此，您可以按照实际用途将配置信息写入到不同的文件中。下面列出几个常用的配置文件：

    ```
    provider.tf                -- provider 配置
    terraform.tfvars      -- 配置 provider 要用到的变量
    varable.tf                  -- 通用变量
    resource.tf                -- 资源定义
    data.tf                        -- 包文件定义
    output.tf                    -- 输出
    ```

    如创建 provider.tf文件时，您可按以下格式配置您的身份认证信息：

    ```
    [root@test terraform-test]# vim provider.tf
    provider "alicloud" {
        region           = "cn-beijing"
        access_key  = "LTA**********NO2"
        secret_key   = "MOk8x0*********************wwff"
    ```

    更多配置信息请参考：[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)。

8.  初始化工作目录：

    ```
    [root@test terraform-test]#terraform init
    
    
    Initializing provider plugins...
    - Checking for available provider plugins on https://releases.hashicorp.com...
    - Downloading plugin for provider "alicloud" (1.25.0)...
    
    
    
    
    The following providers do not have any version constraints in configuration,
    so the latest version was installed.
    
    
    To prevent automatic upgrades to new major versions that may contain breaking
    changes, it is recommended to add version = "..." constraints to the
    corresponding provider blocks in configuration, with the constraint strings
    suggested below.
    
    
    * provider.alicloud: version = "~> 1.25"
    
    
    Terraform has been successfully initialized!
    
    
    You may now begin working with Terraform. Try running "terraform plan" to see
    any changes that are required for your infrastructure. All Terraform commands
    should now work.
    
    
    If you ever set or change modules or backend configuration for Terraform,
    rerun this command to reinitialize your working directory. If you forget, other
    commands will detect it and remind you to do so if necessary.
    
    
    ```

    **说明：** 每个 Terraform 项目在新建 Terraform 工作目录，并创建配置文件后，都需要初始化工作目录。


以上操作完成之后，您就可以使用 Terraform 工具了。

## 使用 Terraform 管理 OSS {#section_gmt_2zs_zfb .section}

Terraform 安装完成之后，您就可以通过 Terraform 的操作命令管理 OSS 了，下面介绍几个常用的操作命令：

-   terraform plan：预览功能，允许在正式执行之前查看将要执行那些操作。

    例如，您添加了一个创建 Bucket 的配置文件 test.tf ：

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    使用 terraform plan 可查看到将会执行的操作：

    ```
    [root@test terraform-test]# terraform plan
    Refreshing Terraform state in-memory prior to plan...
    The refreshed state will be used to calculate this plan, but will not be
    persisted to local or remote state storage.
    
    
    ------------------------------------------------------------------------
    
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
    
    Terraform will perform the following actions:
    
      + alicloud_oss_bucket.bucket-acl
          id:                <computed>
          acl:               "private"
          bucket:            "figo-chen-2020"
          creation_date:     <computed>
          extranet_endpoint: <computed>
          intranet_endpoint: <computed>
          location:          <computed>
          logging_isenable:  "true"
          owner:             <computed>
          referer_config.#:  <computed>
          storage_class:     <computed>
    
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    
    ------------------------------------------------------------------------
    
    Note: You didn't specify an "-out" parameter to save this plan, so Terraform
    can't guarantee that exactly these actions will be performed if
    "terraform apply" is subsequently run.
    ```

-   terraform apply：执行工作目录中的配置文件。

    例如,您想创建一个名为 figo-chen-2020 的 Bucket，您需要先添加了一个创建 Bucket 的配置文件 test.tf ：

    ```
    [root@test terraform-test]#vim test.tf
    resource "alicloud_oss_bucket" "bucket-acl"{
      bucket = "figo-chen-2020"
      acl = "private"
    }
    ```

    之后使用 terraform apply 命令执行配置文件即可。

    ```
    [root@test terraform-test]#terraform  apply
    
    An execution plan has been generated and is shown below.
    Resource actions are indicated with the following symbols:
      + create
    
    Terraform will perform the following actions:
    
      + alicloud_oss_bucket.bucket-acl
          id:                <computed>
          acl:               "private"
          bucket:            "figo-chen-2020"
          creation_date:     <computed>
          extranet_endpoint: <computed>
          intranet_endpoint: <computed>
          location:          <computed>
          logging_isenable:  "true"
          owner:             <computed>
          referer_config.#:  <computed>
          storage_class:     <computed>
    
    
    Plan: 1 to add, 0 to change, 0 to destroy.
    
    Do you want to perform these actions?
      Terraform will perform the actions described above.
      Only 'yes' will be accepted to approve.
    
      Enter a value: yes
    
    alicloud_oss_bucket.bucket-acl: Creating...
      acl:               "" => "private"
      bucket:            "" => "figo-chen-2020"
      creation_date:     "" => "<computed>"
      extranet_endpoint: "" => "<computed>"
      intranet_endpoint: "" => "<computed>"
      location:          "" => "<computed>"
      logging_isenable:  "" => "true"
      owner:             "" => "<computed>"
      referer_config.#:  "" => "<computed>"
      storage_class:     "" => "<computed>"
    alicloud_oss_bucket.bucket-acl: Creation complete after 1s (ID: figo-chen-2020)
    
    Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
    ```

    **说明：** 此配置运行后，若 figo-chen-2020 这个 Bucket 不存在，则创建一个 Bucket；若已存在，且为 Terraform 创建的空 Bucket，则会删除原有 Bucket 并重新生成。

-   `terraform destroy`：可删除通过 Terraform 创建的空的 Bucket。
-   导入 Bucket：若 Bucket 不是通过 Terraform 创建，可通过命令导入现有的 Bucket。

    首先，创建一个 main.tf 的文件，并写入配置：

    ```
    [root@test terraform-test]#vim main.tf
    resource "alicloud_oss_bucket" "bucket" { 
     # (resource arguments)
    }
    ```

    使用如下命令导入现有的 Bucket：

    ```
    terraform import alicloud_oss_bucket.bucket bucketname
    ```


## 参考文档 {#section_afq_hhy_zfb .section}

-   更多 Bucket 配置操作示例请参考：[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)
-   更多Object相关配置操作示例请参考：[alicloud\_oss\_bucket\_object](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket_object.html)

