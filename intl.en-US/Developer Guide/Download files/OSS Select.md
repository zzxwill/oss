# OSS Select {#concept_xsq_qws_2fb .concept}

By using OSS Select, you can use simple SQL statements to select content from objects in OSS to obtain only required data. OSS Select helps you reduce the amount of data transmitted from OSS to improve the data retrieval efficiency and save time.

**Note:** You can send requests to transmit SQL expressions to OSS. For more information about SQL statements supported by OSS Select and limits, see [SelectObject](../../../../reseller.en-US/API Reference/Object operations/SelectObject.md#).

## Operating methods {#section_bdy_cv3_kgb .section}

OSS SDK \(Currently, Java and Python SDKs support OSS Select.\)

## Limits {#section_pll_g3t_2fb .section}

-   Object format: OSS Select supports CSV objects that are UTF-8 encoded and conform to RFC 4180, including CSV-like objects such as TSV objects. In the objects, row and column delimiters and quote characters can be customized.
-   Encryption method: OSS Select supports objects encrypted by using server-side encryption fully managed by OSS \(SSE-OSS\) or server-side encryption that uses customer master keys \(CMKs\) managed by Key Management Service \(KMS\) for encryption \(SSE-KMS\).
-   Region: OSS Select is currently available only in China \(Shenzhen\).

