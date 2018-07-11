# Regions and endpoints {#concept_zt4_cvy_5db .concept}

## Regions and endpoints in a classic network {#section_plb_2vy_5db .section}

The Internet and intranet endpoints in each region for a classic network are as follows:

|Region name|OSS region|Internet endpoint|Internet endpoint protocol|Intranet endpoint for ECS access|Intranet endpoint protocol|
|:----------|:---------|:----------------|:-------------------------|:-------------------------------|:-------------------------|
|China East 1 \(Hangzhou\)|oss-cn-hangzhou|oss-cn-hangzhou.aliyuncs.com|HTTP and HTTPS|oss-cn-hangzhou-internal.aliyuncs.com|HTTP and HTTPS|
|East China 2|China East 2 \(Shanghai\)|oss-cn-shanghai.aliyuncs.com|HTTP and HTTPS|oss-cn-shanghai-internal.aliyuncs.com|HTTP and HTTPS|
|China North 1 \(Qingdao\)|oss-cn-qingdao|oss-cn-qingdao.aliyuncs.com|HTTP and HTTPS|oss-cn-qingdao-internal.aliyuncs.com|HTTP and HTTPS|
|North China 2|China North 2 \(Beijing\)|oss-cn-beijing.aliyuncs.com|HTTP and HTTPS|oss-cn-beijing-internal.aliyuncs.com|HTTP and HTTPS|
|China North 3 \(zhangjiakou\)|oss-cn-zhangjiakou|oss-cn-zhangjiakou.aliyuncs.com|HTTP and HTTPS|oss-cn-zhangjiakou-internal.aliyuncs.com|HTTP and HTTPS|
|China North 5 \(huhehaote\)|oss-cn-huhehaote|oss-cn-huhehaote.aliyuncs.com|HTTP and HTTPS|oss-cn-huhehaote-internal.aliyuncs.com|HTTP and HTTPS|
|China South 1 \(Shenzhen\)|oss-cn-shenzhen|oss-cn-shenzhen.aliyuncs.com|HTTP and HTTPS|oss-cn-shenzhen-internal.aliyuncs.com|HTTP and HTTPS|
|Hong Kong|oss-cn-hongkong|oss-cn-hongkong.aliyuncs.com|HTTP and HTTPS|oss-cn-hongkong-internal.aliyuncs.com|HTTP and HTTPS|
|US West 1 \(Silicon Valley\)|oss-us-west-1|oss-us-west-1.aliyuncs.com|HTTP and HTTPS|oss-us-west-1-internal.aliyuncs.com|HTTP and HTTPS|
|US East 1 \(Virginia\)|oss-us-east-1|oss-us-east-1.aliyuncs.com|HTTP and HTTPS|Oss-us-east-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 1 \(Singapore\)|oss-ap-southeast-1|oss-ap-southeast-1.aliyuncs.com|HTTP and HTTPS|oss-ap-southeast-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 2 \(Sydney\)|oss-ap-southeast-2|oss-ap-southeast-2.aliyuncs.com|HTTP and HTTPS|oss-ap-southeast-2-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 3 \(Kuala Lumpur\)|oss-ap-southeast-3|oss-ap-southeast-3.aliyuncs.com|HTTP and HTTPS|oss-ap-southeast-3-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 5 \(Jakarta\)|oss-ap-southeast-5|oss-ap-southeast-5.aliyuncs.com|HTTP and HTTPS|oss-ap-southeast-5-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific NE 1 \(Tokyo\)|oss-ap-northeast-1|oss-ap-northeast-1.aliyuncs.com|HTTP and HTTPS|oss-ap-northeast-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SOU 1 \(Mumbai\)|oss-ap-south-1|oss-ap-south-1.aliyuncs.com|HTTP and HTTPS|oss-ap-south-1-internal.aliyuncs.com|HTTP and HTTPS|
|EU Central 1 \(Frankfurt\)|oss-eu-central-1|oss-eu-central-1.aliyuncs.com|HTTP and HTTPS|oss-eu-central-1-internal.aliyuncs.com|HTTP and HTTPS|
|Middle East 1 \(Dubai\)|oss-me-east-1|oss-me-east-1.aliyuncs.com|HTTP and HTTPS|oss-me-east-1-internal.aliyuncs.com|HTTP and HTTPS|

**Note:** 

-   We recommend that you use third-level domain names, that is, `bucket name` + `endpoint` format to share links or bind domain names \(CNAME\). For example, the third-level domain name for the bucket oss-sample in the region China East 2 \(Shanghai\) is oss-sample.oss-cn-shanghai.aliyuncs.com.
-   For SDKs, use `http://` or `https://` + `endpoint` as the initialization parameters. For example,  we recommend that you use `http://oss-cn-shanghai.aliyuncs.com` or `https://oss-cn-shanghai.aliyuncs.com` as the initialization parameter. Do not use a third-level domain name, that is, `http://bucket.oss-cn-shanghai.aliyuncs.com` as the initialization parameter.
-   By default, the Internet address oss.aliyuncs.com is directed to the Internet address of the region China East 1 \(Hangzhou\).
-   The intranet address oss-internal.aliyuncs.com is directed to the intranet address of the region China East 1 \(Hangzhou\).

## Regions and endpoints in a VPC network {#section_m14_zvy_5db .section}

To access OSS, ECS of a VPC network can use the following endpoints:

|Region name|OSS region|Endpoint of VPC network|Protocol|
|:----------|:---------|:----------------------|:-------|
|China East 1 \(Hangzhou\)|oss-cn-hangzhou|oss-cn-hangzhou-internal.aliyuncs.com|HTTP and HTTPS|
|China East 2 \(Shanghai\)|oss-cn-shanghai|oss-cn-shanghai-internal.aliyuncs.com|HTTP and HTTPS|
|China North 1 \(Qingdao\)|oss-cn-qingdao|oss-cn-qingdao-internal.aliyuncs.com|HTTP and HTTPS|
|China North 2 \(Beijing\)|oss-cn-beijing|oss-cn-beijing-internal.aliyuncs.com|HTTP and HTTPS|
|China North 3 \(zhangjiakou\)|oss-cn-zhangjiakou|oss-cn-zhangjiakou-internal.aliyuncs.com|HTTP and HTTPS|
|China North 5 \(huhehaote\)|oss-cn-huhehaote|oss-cn-huhehaote-internal.aliyuncs.com|HTTP and HTTPS|
|China South 1 \(Shenzhen\)|oss-cn-shenzhen|oss-cn-shenzhen-internal.aliyuncs.com|HTTP and HTTPS|
|Hong Kong|oss-cn-hongkong|oss-cn-hongkong-internal.aliyuncs.com|HTTP and HTTPS|
|US West 1 \(Silicon Valley\)|oss-us-west-1|oss-us-west-1-internal.aliyuncs.com|HTTP and HTTPS|
|US East 1 \(Virginia\)|oss-us-east-1|oss-us-east-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 1 \(Singapore\)|oss-ap-southeast-1|oss-ap-southeast-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 2 \(Sydney\)|oss-ap-southeast-2|oss-ap-southeast-2-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 3 \(Kuala Lumpur\)|oss-ap-southeast-3|oss-ap-southeast-3-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SE 5 \(Jakarta\)|oss-ap-southeast-5|oss-ap-southeast-5-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific NE 1 \(Tokyo\)|oss-ap-northeast-1|oss-ap-northeast-1-internal.aliyuncs.com|HTTP and HTTPS|
|Asia Pacific SOU 1 \(Mumbai\)|oss-ap-south-1|oss-ap-south-1-internal.aliyuncs.com|HTTP and HTTPS|
|EU Central 1 \(Frankfurt\)|oss-eu-central-1|oss-eu-central-1-internal.aliyuncs.com|HTTP and HTTPS|
|Middle East 1 \(Dubai\)|oss-me-east-1|oss-me-east-1-internal.aliyuncs.com|HTTP and HTTPS|

