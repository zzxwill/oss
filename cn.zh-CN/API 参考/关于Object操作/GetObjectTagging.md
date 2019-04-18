# GetObjectTagging {#concept_185787 .concept}

您可以通过GetObjectTagging接口获取对象的标签信息。

## 请求语法 {#section_p51_b6l_gfq .section}

``` {#codeblock_4xc_ti7_x8z}
GET /objectname?tagging
Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 响应元素 {#section_al1_y45_hth .section}

|名称|类型|描述|
|--|--|--|
|Tagging|容器|子节点：TagSet|
|TagSet|容器| 父节点：Tagging

 子节点：Tag

 |
|Tag|容器| 父节点：TagSet

 子节点：Key，Value

 |
|Key|字符串| 父节点：Tag

 子节点：无

 |
|Value|字符串| 父节点：Tag

 子节点：无

 |

## 示例 {#section_kp1_9iq_9gz .section}

-   请求示例

    ``` {#codeblock_quv_67o_gp4}
    GET /objectname?tagging
    Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
    Date: Wed, 20 Mar 2019 02:02:36 GMT
    Authorization: OSS ************:********************
    ```

-   响应示例

    ``` {#codeblock_slm_5kl_3nh}
    200 (OK)
    content‐length: 209
    server: AliyunOSS
    x‐oss‐request‐id: 5C919F38461FB42826000002
    date: Wed, 20 Mar 2019 02:02:32 GMT
    content‐type: application/xml
    <?xml version="1.0" encoding="UTF‐8"?>
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


