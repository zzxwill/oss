# Options {#concept_64097_zh .concept}

This topic describes how to configure options.

## OSS\(options\) {#section_cwq_xmk_lfb .section}

-   \[accessKeyId\] \{String\}: accessKeyId you create on Alibaba Cloud console website
-   \[accessKeySecret\] \{String\}: accessKeySecret you create
-   \[bucket\] \{String\}: the default bucket you want to access. If you do not have any bucket, use putBucket\(\) to create one first.
-   \[endpoint\] \{String\}: oss region domain. It takes priority over region.
-   \[region\] \{String\}: the bucket data region location. The default value is oss-cn-hangzhou.
-   \[internal\] \{Boolean\}: access OSS with Alibaba Cloud internal network or not. The default value is false. If your servers are running on Alibaba Cloud too, you can set it to true to reduce costs.
-   \[secure\] \{Boolean\}: instruct OSS client to use HTTPS \(secure: true\) or HTTP \(secure: false\) protocol. For more information, click [here](reseller.en-US/SDK Reference/Node. js/FAQ.md#).
-   \[timeout\] \{String|Number\}: instance level timeout for all operations. The default value is 60s.

```
var oss = require('ali-oss');

var store = oss({
  accessKeyId: 'your access key',
  accessKeySecret: 'your access secret',
  bucket: 'your bucket name',
  region: 'oss-cn-hangzhou'
});

```

