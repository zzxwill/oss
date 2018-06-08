# 有关Bucket命令 {#concept_av1_r1c_wdb .concept}

## config {#section_bbx_w1c_wdb .section}

命令说明：

```
config --id=[accessid] --key=[accesskey] --host=[host] --sts_token=[sts_token]
```

使用示范：

-   `python osscmd config --id=your_id --key=your_key`
-   ```
python osscmd config --id=your_id --key=your_key
        --host=oss-internal.aliyuncs.com
```


## getallbucket\(gs\) {#section_alb_gbc_wdb .section}

命令说明：

`getallbucket(gs)`

用来显示用户创建的bucket。gs是get service的简写。\(gs\)表示和getallbucket是同样的效果。

使用示范：

-   `python osscmd getallbucket`
-   `python osscmd gs`

## createbucket\(cb,mb,pb\) {#section_uhk_mbc_wdb .section}

命令说明：

`createbucket(cb,mb,pb) oss://bucket --acl=[acl]`

创建bucket的命令，cb是create bucket的简写，mb是make bucket的简写，pb是put bucket的简写，oss://bucket表示bucket。--acl参数可以传入，也可以不传入。这几个命令都是同样的效果。

使用示范：

-   `python osscmd createbucket oss://mybucket`
-   `python osscmd cb oss://myfirstbucket --acl=public-read`
-   `python osscmd mb oss://mysecondbucket --acl=private`
-   `python osscmd pb oss://mythirdbucket`

## deletebucket\(db\) {#section_hzk_rbc_wdb .section}

命令说明：

`deletebucket(db) oss://bucket`

删除bucket的命令，db是delete bucket的简写。deletebucket和db是同样的效果。

使用示范：

-   `python osscmd deletebucket oss://mybucket`
-   `python osscmd db oss://myfirstbucket`

## deletewholebucket {#section_cgy_5bc_wdb .section}

注意： 该命令十分危险，将会删除所有的数据，并且不可恢复。请慎重使用。 **命令说明：**

`deletewholebucket oss://bucket`

删除bucket及其内部object以及multipart相关的内容。

使用示范：

-   `python osscmd deletewholebucket oss://mybucket`

## getacl {#section_mmg_zbc_wdb .section}

命令说明：

`getacl oss://bucket`

获取bucket的访问控制权限

使用示范：

-   `python osscmd getacl oss://mybucket`

## setacl {#section_wly_kcc_wdb .section}

命令说明：

`setacl oss://bucket --acl=[acl]`

修改bucket的访问控制权限。acl只允许为private，public-read，public-read-write三个当中的一个。

使用示范：

-   `python osscmd setacl oss://mybucket --acl=private`

## putlifecycle {#section_mjs_4cc_wdb .section}

命令说明：

`putlifecycle oss://mybucket lifecycle.xml`

设置lifecycle规则。其中lifecycle.xml为xml格式的lifecycle配置文件，详细的规则配置可以参考[API文档](../intl.zh-CN/API 参考/关于Bucket的操作/PutBucketLifecycle.md#)

使用示范：

-   `python osscmd putlifecycle oss://mybucket lifecycle.xml`

其中lifecycle.xml为XML格式的lifecycle配置规则，举例为：

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

命令说明：

`osscmd getlifecycle oss://bucket`

获取该Bucket lifecycle规则。

使用示范：

-   `python osscmd getlifecycle oss://mybucket`

## deletelifecycle {#section_zym_zcc_wdb .section}

命令说明：

`osscmd deletelifecycle oss://bucket`

删除该bucket下所有的lifecycle规则。

使用示范：

-   `python osscmd deletelifecycle oss://mybucket`

## putreferer {#section_rqv_cdc_wdb .section}

命令说明：

```
osscmd putreferer oss://bucket --allow_empty_referer=[true|false]
        --referer=[referer]
```

设置防盗链规则。其中参数`allow_empty_referer`用来设置是否允许为空，为必选参数。参数`referer`来设置允许访问的白名单，比如“www.test1.com,www.test2.com”，以“,”作为分隔。详细的配置规则参考[产品文档](../intl.zh-CN/开发指南/安全管理/设置防盗链.md#)。

使用示范：

-   ```
python osscmd putreferer oss://mybucket --allow_empty_referer=true
          --referer="www.test1.com,www.test2.com"
```


## getreferer {#section_nts_gdc_wdb .section}

命令说明：

`osscmd getreferer oss://bucket`

获取该Bucket下防盗链设置规则。

使用示范：

-   `python osscmd getreferer oss://mybucket`

## putlogging {#section_v22_jdc_wdb .section}

命令说明：

`osscmd putlogging oss://source_bucket oss://target_bucket/[prefix]`

其中source\_bucket表示需要记录日志的bucket，而target\_bucket则是用来存放产生的日志。同时可以对源bucket产生的日志文件设置前缀，方便用户归类查询。

使用示范：

-   `python osscmd getlogging oss://mybucket`

## getlogging {#section_igc_rdc_wdb .section}

命令说明：

`osscmd getlogging oss://bucket`

获取该bucket的logging设置规则，返回为一个xml。

使用示范：

-   `python osscmd getlogging oss://mybucket`

