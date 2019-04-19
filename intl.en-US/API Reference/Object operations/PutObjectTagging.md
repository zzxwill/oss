# PutObjectTagging {#concept_185784 .concept}

Configures or updates the tags of an object.

## Request syntax {#section_1c8_hhm_fyk .section}

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

## Request elements {#section_zer_mce_lrg .section}

|Element|Type|Required?|Description|
|-------|----|---------|-----------|
|Tagging|Container|Yes|Sub-node: TagSet|
|TagSet|Container|Yes| Parent node: Tagging

 Sub-node: Tag

 |
|Tag|Container|No| Parent node: TagSet

 Sub-node: Key, Value

 |
|Key|String|No| Parent node: Tag

 Sub-node: None

 |
|Value|String|No| Parent node: Tag

 Sub-node: None

 |

## Detail analysis {#section_0bo_p69_h0g .section}

-   The requester must have the permission to perform the PutObjectTagging operation.
-   The Last-Modified time of an object is not updated if the tag of the object is modified.
-   A tag can contain letters, numbers, spaces, and the following symbols: plus sign \(+\), hyphen \(-\), equal sign \(=\), period \(.\), underscore \(\_\), colon \(:\), and forward slash \(/\).

## Examples {#section_85b_mu5_cx0 .section}

-   Request example:

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

-   Response exmple:

    ``` {#codeblock_k3y_8xt_tz0}
    200 (OK)
    content‐length: 0
    server: AliyunOSS
    x‐oss‐request‐id: 5C8F55ED461FB4A64C000004
    date: Mon, 18 Mar 2019 08:25:17 GMT
    ```


