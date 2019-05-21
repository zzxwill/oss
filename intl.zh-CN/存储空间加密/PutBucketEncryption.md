# PutBucketEncryption {#concept_262214 .concept}

PutBucketEncryption接口用于配置Bucket的加密规则。

## 请求语法 {#section_45c_rqd_cp3 .section}

``` {#codeblock_ak9_pgk_vei}
PUT /?encryption HTTP/1.1
Date: GMT Date
Content-Length：ContentLength
Content-Type: application/xml
Host: BucketName.oss.aliyuncs.com
Authorization: SignatureValue
<?xml version="1.0" encoding="UTF-8"?>
<ServerSideEncryptionRule>
  <ApplyServerSideEncryptionByDefault>
    <SSEAlgorithm>AES256</SSEAlgorithm>
    <KMSMasterKeyID></KMSMasterKeyID>
  </ApplyServerSideEncryptionByDefault>
</ServerSideEncryptionRule>
```

## 请求元素 {#section_505_ygm_0ph .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|ServerSideEncryptionRule|容器|是| 服务端加密规则的容器。

 子元素：ApplyServerSideEncryptionByDefault

 |
|ApplyServerSideEncryptionByDefault|容器|是| 服务端默认加密方式的容器。

 子元素：SSEAlgorithm，KMSMasterKeyID

 |
|SSEAlgorithm|字符串|是| 设置服务端默认加密方式。

 取值：KMS、AES256

 |
|KMSMasterKeyID|字符串|否|当SSEAlgorithm值为KMS，且使用指定的密钥加密时，需输入密钥ID。其他情况下，必须为空。|

## 细节分析 {#section_st4_wua_4ty .section}

-   只有Bucket的拥有者及授权子账户才能为Bucket设置加密规则，否则返回403错误。
-   使用KMS密钥功能时会产生少量的KMS密钥API调用费用，费用详情请参考[KMS计费标准](../../../../cn.zh-CN/产品定价/计费方式.md#section_br1_k3j_kfb)。
-   当SSEAlgorithm取值不是KMS或者AES256，则返回400 InvalidEncryptionAlgorithmError错误，错误提示：The Encryption request you specified is not valid. Supported value: AES256/KMS。
-   当SSEAlgorithm取值为AES256，填写了KMSMasterKeyID，则返回400 InvalidArgument错误，错误提示：KMSMasterKeyID is not applicable if the default sse algorithm is not KMS。

## 示例 {#section_cji_p6q_bgl .section}

-   请求示例

    ``` {#codeblock_0qh_t1t_isn}
    PUT /?encryption HTTP/1.1
    Date: Tue, 20 Dec 2018 11:09:13 GMT
    Content-Length：ContentLength
    Content-Type: application/xml
    Host: oss-example.oss-cn-hangzhou.aliyuncs.com
    Authorization: OSS qn6qrrqxo2oawuk53otf****:ceOEyZavKY4QcjoUWYSpYbJ3****
    <?xml version="1.0" encoding="UTF-8"?>
    <ServerSideEncryptionRule>
      <ApplyServerSideEncryptionByDefault>
        <SSEAlgorithm>KMS</SSEAlgorithm>
        <KMSMasterKeyID>9468da86-3509-4f8d-a61e-6eab1eac****</KMSMasterKeyID>
      </ApplyServerSideEncryptionByDefault>
    </ServerSideEncryptionRule>
    ```

-   返回示例

    ``` {#codeblock_lui_yna_sge}
    HTTP/1.1 200 OK
    x-oss-request-id: 5C1B138A109F4E405B2D8AEF
    Date: Thu, 20 Dec 2018 11:11:06 GMT
    ```


