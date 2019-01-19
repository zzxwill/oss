# 什么是对象存储 OSS {#concept_ybr_fg1_tdb .concept}

阿里云对象存储服务（Object Storage Service，简称 OSS），是阿里云提供的海量、安全、低成本、高可靠的云存储服务。它具有与平台无关的RESTful API接口，能够提供99.999999999%（11个9）的数据可靠性和99.99%的服务可用性。您可以在任何应用、任何时间、任何地点存储和访问任意类型的数据。

您可以使用阿里云提供的API、SDK接口或者OSS迁移工具轻松地将海量数据移入或移出阿里云OSS。数据存储到阿里云OSS以后，您可以选择标准类型（Standard）的阿里云OSS服务作为移动应用、大型网站、图片分享或热点音视频的主要存储方式，也可以选择成本更低、存储期限更长的低频访问类型（Infrequent Access）和归档类型（Archive）的阿里云OSS服务作为不经常访问数据的备份和归档。

## 相关概念 {#section_h4j_rlb_n2b .section}

-   存储类型（Storage Class）

    OSS提供标准、低频访问、归档三种存储类型，全面覆盖从热到冷的各种数据存储场景。其中标准存储类型提供高可靠、高可用、高性能的对象存储服务，能够支持频繁的数据访问；低频访问存储类型适合长期保存不经常访问的数据（平均每月访问频率1到2次），存储单价低于标准类型；归档存储类型适合需要长期保存（建议半年以上）的归档数据，在三种存储类型中单价最低。详情请参见[存储类型介绍](../../../../../cn.zh-CN/开发指南/存储类型/存储类型介绍.md#)。

-   存储空间（Bucket）

    存储空间是您用于存储对象（Object）的容器，所有的对象都必须隶属于某个存储空间。您可以设置和修改存储空间属性用来控制地域、访问权限、生命周期等，这些属性设置直接作用于该存储空间内所有对象，因此您可以通过灵活创建不同的存储空间来完成不同的管理功能。

-   对象/文件（Object）

    对象是 OSS 存储数据的基本单元，也被称为OSS的文件。对象由元信息（Object Meta），用户数据（Data）和文件名（Key）组成。对象由存储空间内部唯一的Key来标识。对象元信息是一个键值对，表示了对象的一些属性，比如最后修改时间、大小等信息，同时您也可以在元信息中存储一些自定义的信息。

-   地域（Region）

    地域表示 OSS 的数据中心所在物理位置。您可以根据费用、请求来源等综合选择数据存储的地域。详情请参见[OSS已开通的Region](../../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

-   访问域名（Endpoint）

    Endpoint 表示OSS对外服务的访问域名。OSS以HTTP RESTful API的形式对外提供服务，当访问不同地域的时候，需要不同的域名。通过内网和外网访问同一个地域所需要的域名也是不同的。具体的内容请参见[各个Region对应的Endpoint](../../../../../cn.zh-CN/开发指南/访问域名（Endpoint）/访问域名和数据中心.md#)。

-   访问密钥（AccessKey）

    AccessKey，简称 AK，指的是访问身份验证中用到的AccessKeyId 和AccessKeySecret。OSS通过使用AccessKeyId 和AccessKeySecret对称加密的方法来验证某个请求的发送者身份。AccessKeyId用于标识用户，AccessKeySecret是用户用于加密签名字符串和OSS用来验证签名字符串的密钥，其中AccessKeySecret 必须保密。


## 相关服务 {#section_jcv_xlb_n2b .section}

您把数据存储到OSS以后，就可以使用阿里云提供的其他产品和服务对其进行相关操作。

以下是您会经常使用到的阿里云产品和服务：

-   云服务器ECS：提供简单高效、处理能力可弹性伸缩的云端计算服务。请参见[ECS产品详情页面](https://www.aliyun.com/product/ecs)。
-   内容分发网络CDN：将源站资源缓存到各区域的边缘节点，供您就近快速获取内容。请参见[CDN产品详情页面](https://www.aliyun.com/product/cdn)。
-   E-MapReduce：构建于阿里云云服务器 ECS 上的大数据处理的系统解决方案，基于开源的 Apache Hadoop 和 Apache Spark，方便您分析和处理自己的数据。请参见[E-MapReduce产品详情页面](https://www.aliyun.com/product/emapreduce)。
-   媒体处理：将存储于OSS的音视频转码成适合在PC、TV以及移动终端上播放的格式。并基于海量数据深度学习，对音视频的内容、文字、语音、场景多模态分析，实现智能审核、内容理解、智能编辑。请参见[媒体处理产品详情页面](https://www.aliyun.com/product/mts)。

## 使用OSS {#section_o5k_1mb_n2b .section}

阿里云提供了Web服务页面，方便您管理对象存储OSS。您可以登录OSS管理控制台，操作存储空间和对象。关于管理控制台的操作，请参见控制台用户指南。

阿里云也提供了丰富的API接口和各种语言的SDK包，方便您灵活地管理对象存储OSS。请参见[OSS API参考](../../../../../cn.zh-CN/API 参考/API概览.md#)和[OSS SDK参考](https://help.aliyun.com/document_detail/52834.html)。

## OSS定价 {#section_sv5_nmb_n2b .section}

传统的存储服务供应商会要求您购买预定量的存储和网络传输容量，如果超出此容量，就会关闭对应的服务或者收取高昂的超容量费用；如果没有超过此容量，又需要您按照全部容量支付费用。

对象存储OSS仅按照您的实际使用容量收费，您无需预先购买存储和流量容量，随着您业务的发展，您将享受到更多的基础设施成本优势。

关于对象存储OSS的价格，请参见[OSS详细价格信息](https://www.aliyun.com/price/product#/oss/detail)。关于OSS的计量计费方式，请参见[OSS计量项和计费项](../../../../../cn.zh-CN/计量计费/计量项和计费项.md#)。

## 学习路径图 {#section_bxq_qdc_n2b .section}

您可以通过[OSS产品学习路径图](https://help.aliyun.com/learn/learningpath/oss.html)快速了解OSS，学习相关的基础操作，并利用丰富的API、SDK包和便捷工具进行二次开发。

## 视频 {#section_rmr_xzl_42b .section}

观看以下视频，快速了解OSS。

