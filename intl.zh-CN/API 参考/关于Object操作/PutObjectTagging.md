# PutObjectTagging {#concept_185784 .concept}

您可以通过PutObjectTagging接口设置或更新对象的标签（Object Tagging）。

## 请求语法 {#section_1c8_hhm_fyk .section}

``` {#codeblock_wer_tpd_el5}
PUT /objectname?tagging
Content‐Length: 114
Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
<Tagging>
  <TagSet>
    <Tag>
      <Key>Key</Key>
      <Value>Value</Value>
    </Tag>
  </TagSet>
</Tagging>
				
```

## 请求元素 {#section_zer_mce_lrg .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|Tagging|容器|是|子节点：TagSet|
|TagSet|容器|是| 父节点：Tagging

 子节点：Tag

 |
|Tag|容器|否| 父节点：TagSet

 子节点：Key，Value

 |
|Key|字符串|否| 父节点：Tag

 子节点：无

 |
|Value|字符串|否| 父节点：Tag

 子节点：无

 |

## 细节分析 {#section_0bo_p69_h0g .section}

-   请求者需要有PutObjectTagging权限。
-   更改Tagging不会更新Object Last‑Modified时间。
-   标签合法字符集包括大小写字母、数字、空格和以下符号：

    +‑=.\_:/


## 示例 {#section_85b_mu5_cx0 .section}

-   请求示例

    ``` {#codeblock_f55_5ve_57k}
    PUT /objectname?tagging
    Content‐Length: 114
    Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
    Date: Mon, 18 Mar 2019 08:25:17 GMT
    Authorization: OSS ***********:*********************
    <Tagging>
      <TagSet>
        <Tag>
          <Key>a</Key>
          <Value>1</Value>
        </Tag>
        <Tag>
          <Key>b</Key>
          <Value>2</Value>
        </Tag>
      </TagSet>
    </Tagging>
    ```

-   返回示例

    ``` {#codeblock_k3y_8xt_tz0}
    200 (OK)
    content‐length: 0
    server: AliyunOSS
    x‐oss‐request‐id: 5C8F55ED461FB4A64C000004
    date: Mon, 18 Mar 2019 08:25:17 GMT
    ```


