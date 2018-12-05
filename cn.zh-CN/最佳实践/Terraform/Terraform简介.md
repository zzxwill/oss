# Terraform简介 {#concept_qhs_4ms_zfb .concept}

Terraform 是一个开源的自动化的资源编排工具，支持多家云服务提供商。阿里云作为第三大云服务提供商，[terraform-alicloud-provider](https://www.terraform.io/docs/providers/alicloud/index.html) 已经支持了超过 90 多个 Resource 和 Data Source，覆盖 20 多个服务和产品，吸引了越来越多的开发者加入到阿里云 Terraform 生态的建设中。

[HashiCorp Terraform](https://www.terraform.io/) 是一个IT基础架构自动化编排工具，可以用代码来管理维护 IT 资源。Terraform 的命令行接口（CLI） 提供一种简单机制，用于将配置文件部署到阿里云或其他任意支持的云上，并对其进行版本控制。它编写了描述云资源拓扑的配置文件中的基础结构，例如虚拟机、存储帐户和网络接口。Terraform 的命令行接口（CLI）提供一种简单机制，用于将配置文件部署到阿里云或任何其他支持的云并对其进行版本控制。Terraform 是一个高度可扩展的工具，通过 Provider 来支持新的基础架构。您可以使用 Terraform 来创建、修改或删除 OSS、ECS、VPC、RDS、SLB 等多种资源。

## OSS Terraform Module 功能 {#section_upf_d4s_zfb .section}

OSS 的 Terraform Module 目前主要提供 Bucket 管理、文件对象管理的功能。例如：

-   Bucket 管理功能：
    -   创建 Bucket
    -   设置 Bucket ACL
    -   设置 Bucket CORS
    -   设置 Bucket Logging
    -   设置 Bucket 静态网站托管
    -   设置 Bucket Referer
    -   设置 Bucket Lifecycle
-   Object 管理功能：
    -   文件上传
    -   设置文件服务端加密方式
    -   设置 ACL
    -   设置对象元数据信息

## 参考文档 {#section_cdn_jky_zfb .section}

-   安装及使用 Terraform 请参见：[使用Terraform管理OSS](cn.zh-CN/最佳实践/Terraform/使用Terraform管理OSS.md#)
-   OSS Terraform Module 下载地址请参见：[terraform-alicloud-modules](https://github.com/terraform-alicloud-modules/terraform-alicloud-oss-object)

-   更多 OSS Terraform Module 介绍请参见：[alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html)


