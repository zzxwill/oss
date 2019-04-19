# GetObjectTagging {#concept_185787 .concept}

Obtains the tags of an object.

## Request syntax {#section_p51_b6l_gfq .section}

``` {#codeblock_4xc_ti7_x8z}
GET /objectname?tagging
Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Response elements {#section_al1_y45_hth .section}

|Element|Type|Description|
|-------|----|-----------|
|Tagging|Container|Sub-node: TagSet|
|TagSet|Container| Parent node: Tagging

 Sub-node: Tag

 |
|Tag|Container| Parent node: TagSet

 Sub-node: Key, Value

 |
|Key|String| Parent node: Tag

 Sub-node: None

 |
|Value|String| Parent node: Tag

 Sub-node: None

 |

## Examples {#section_kp1_9iq_9gz .section}

-   Request example:

    ``` {#codeblock_quv_67o_gp4}
    GET /objectname?tagging
    Host: bucketname.oss‐cn‐hangzhou.aliyuncs.com
    Date: Wed, 20 Mar 2019 02:02:36 GMT
    Authorization: OSS ************:********************
    ```

-   Response example:

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


