# Bucket commands {#concept_av1_r1c_wdb .concept}

## config {#section_bbx_w1c_wdb .section}

Command instructions:

```
config --id=[accessid] --key=[accesskey] --host=[host] --sts_token=[sts_token]
```

Example:

-   `python osscmd config --id=your_id --key=your_key`
-   ```
python osscmd config --id=your_id --key=your_key
        --host=oss-internal.aliyuncs.com
```


## getallbucket\(gs\) {#section_alb_gbc_wdb .section}

Command instructions:

`getallbucket(gs)`

Show the bucket the user has created. The gs is the short form of get service. The gs achieves the same effect with getallbucket.

Example:

-   `python osscmd getallbucket`
-   `python osscmd gs`

## createbucket\(cb,mb,pb\) {#section_uhk_mbc_wdb .section}

Command instructions:

`createbucket(cb,mb,pb) oss://bucket --acl=[acl]`

Create bucket commands. The cb is short for create bucket, mb is short for make bucket, pb is short for put bucket and oss://bucket indicates the bucket. The —acl parameter can be included but it is not required. The several commands all achieve the same effect.

Example:

-   `python osscmd createbucket oss://mybucket`
-   `python osscmd cb oss://myfirstbucket --acl=public-read`
-   `python osscmd mb oss://mysecondbucket --acl=private`
-   `python osscmd pb oss://mythirdbucket`

## deletebucket\(db\) {#section_hzk_rbc_wdb .section}

Command instructions:

`deletebucket(db) oss://bucket`

Delete bucket commands. The db is short for delete bucket. Deletebucket achieves the same effect with db.

Example:

-   `python osscmd deletebucket oss://mybucket`
-   `python osscmd db oss://myfirstbucket`

## deletewholebucket {#section_cgy_5bc_wdb .section}

Note: This command is highly risky as it erases all the data and the erased data cannot be restored. Use it with caution. **Command instructions:**

`deletewholebucket oss://bucket`

Delete bucket, its objects, and the multipart contents.

Example:

-   `python osscmd deletewholebucket oss://mybucket`

## getacl {#section_mmg_zbc_wdb .section}

Command instructions:

`getacl oss://bucket`

Get bucket access and control privilege.

Example:

-   `python osscmd getacl oss://mybucket`

## setacl {#section_wly_kcc_wdb .section}

Command instructions:

`setacl oss://bucket --acl=[acl]`

Modify bucket access and control privilege. The acl can only be one of the three, private, public-read, or public-read-write.

Example:

-   `python osscmd setacl oss://mybucket --acl=private`

## putlifecycle {#section_mjs_4cc_wdb .section}

Command instructions:

`putlifecycle oss://mybucket lifecycle.xml`

Set lifecycle rules. The lifecycle.xml is the configuration file of lifecycle. For detailed rule configuration, see [API Reference](../intl.en-US/API Reference/Bucket operations/Put Bucket Lifecycle.md#).

Example:

-   `python osscmd putlifecycle oss://mybucket lifecycle.xml`

The lifecycle.xml contains the configuration rules of lifecycle. For example,

```
<LifecycleConfiguration>
    <Rule>
        <ID>1125</ID>
        <Prefix>log_backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
            <Days>2</Days>
        </Expiration>
    </Rule>
</LifecycleConfiguration>
```

## getlifecycle {#section_cnj_wcc_wdb .section}

Command instructions:

`osscmd getlifecycle oss://bucket`

Get rules of the bucket lifecycle.

Example:

-   `python osscmd getlifecycle oss://mybucket`

## deletelifecycle {#section_zym_zcc_wdb .section}

Command instructions:

`osscmd deletelifecycle oss://bucket`

Delete all the lifecycle rules under the bucket.

Example:

-   `python osscmd deletelifecycle oss://mybucket`

## putreferer {#section_rqv_cdc_wdb .section}

Command instructions:

```
osscmd putreferer oss://bucket --allow_empty_referer=[true|false]
        --referer=[referer]
```

Set anti-leech rules. The `allow_empty_referer` parameter is requisite and used to set whether it is allowed to be null. The referer parameter is used to set the allowed whitelist for access, e.g., “www.test1.com,www.test2.com”, with “,” as the separator. For detailed rule configuration, see [Product documentation](../intl.en-US/Developer Guide/Security management/Anti-leech settings.md#).

Example:

-   ```
python osscmd putreferer oss://mybucket --allow_empty_referer=true
          --referer="www.test1.com,www.test2.com"
```


## getreferer {#section_nts_gdc_wdb .section}

Command instructions:

`osscmd getreferer oss://bucket`

Get the anti-leech rules of the bucket.

Example:

-   `python osscmd getreferer oss://mybucket`

## putlogging {#section_v22_jdc_wdb .section}

Command instructions:

`osscmd putlogging oss://source_bucket oss://target_bucket/[prefix]`

The source\_bucket indicates the bucket for logs, and the target\_bucket indicates where the logs can be stored. You can set a prefix for the log files generated in the source bucket for the convenience of categorized query.

Example:

-   `python osscmd getlogging oss://mybucket`

## getlogging {#section_igc_rdc_wdb .section}

Command instructions:

`osscmd getlogging oss://bucket`

Get the logging rules of the bucket and an XML file is returned.

Example:

-   `python osscmd getlogging oss://mybucket`

