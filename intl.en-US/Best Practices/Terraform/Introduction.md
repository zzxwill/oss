# Introduction {#concept_qhs_4ms_zfb .concept}

Terraform is an open-source automatic resource orchestration tool that supports multiple cloud service providers. Alibaba Cloud \(referenced as [terraform-alicloud-provider](https://www.terraform.io/docs/providers/alicloud/index.html) in Terraform\) allows developers to easily build, update, and version their infrastructure in the Alibaba Cloud Terraform ecosystem by supporting over 90 resources and data sources across more than 20 products and services.

[HashiCorp Terraform](https://www.terraform.io/) is an automatic IT infrastructure orchestration tool that can manage and maintain IT resources by using code. The easy to use Command Line Interface \(CLI\) of Terraform allows you to deploy configuration files on Alibaba Cloud or any other supported cloud, and control the versions of the configuration files. The CLI provides code for the infrastructures \(such as VMs, storage accounts, and network interfaces\) defined in the configuration files that describe the cloud resource topology. The Command Line Interface \(CLI\) of Terraform provides a simple mechanism, which is used to deploy configuration files on Alibaba Cloud or any other supported cloud and control the versions of the configuration files. Terraform is a highly scalable tool that supports new infrastructures through providers. You can use Terraform to create, modify, or delete multiple resources, such as ECS, VPC, RDS, and SLB.

## Functions of OSS Terraform module {#section_upf_d4s_zfb .section}

You can use the OSS Terraform module to manage buckets and objects. For example:

-   Bucket management functions:
    -   Creates a bucket.
    -   Configures an ACL for a bucket.
    -   Configures Cross-Origin Resource Sharing \(CORS\) for a bucket.
    -   Sets logging for a bucket.
    -   Configures static website hosting for a bucket.
    -   Configures referers for a bucket.
    -   Configures the lifecycle rules of a bucket.
-   Object management functions:
    -   Uploads an object.
    -   Configures server-end encryption for an object.
    -   Sets an ACL for an object.
    -   Sets Object Meta.

## References {#section_cdn_jky_zfb .section}

-   For the installation and usage of Terraform, see [Use Terraform to manage OSS](reseller.en-US/Best Practices/Terraform/Use Terraform to manage OSS.md#).
-   To download the OSS Terraform module, see [terraform-alicloud-modules](https://github.com/terraform-alicloud-modules/terraform-alicloud-oss-object).

-   For more information about the OSS Terraform module, see [alicloud\_oss\_bucket](https://www.terraform.io/docs/providers/alicloud/r/oss_bucket.html).


