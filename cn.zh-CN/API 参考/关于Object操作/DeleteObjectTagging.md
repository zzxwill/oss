# DeleteObjectTagging {#concept_185788 .concept}

您可以通过DeleteObjectTagging删除指定对象的标签。

## 请求语法 {#section_d1u_u4m_70w .section}

``` {#codeblock_g5l_n4b_mbt}
DELETE /objectname?tagging
Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 示例 {#section_gv5_khz_dah .section}

-   请求示例

    ``` {#codeblock_gll_enn_p5a}
    DELETE /objectname?tagging
    Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
    Date: Tue, 09 Apr 2019 03:00:33 GMT
    Authorization: OSS LTAIbsTkySSptaz****/Zr0o6BKgAl7iiBtHN2JMC****
    ```

-   响应示例

    ``` {#codeblock_1f2_39b_nut}
    204 (No Content)
    content‐length: 0
    server: AliyunOSS
    x‐oss‐request‐id: 5CAC0AD16D0232E2051B****
    date: Tue, 09 Apr 2019 03:00:33 GMT
    ```


