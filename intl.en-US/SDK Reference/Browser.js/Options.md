# Options {#concept_64095_zh .concept}

This topic describs how to configure options.

## OSS\(options\) introduction {#section_gpl_ctl_lfb .section}

-   \[accessKeyId\] \{String\}: AccessKey you create on Alibaba Cloud console website
-   \[accessKeySecret\] \{String\}: access secret you create
-   \[stsToken\] \{String\}: use temporary authorization method. For more information, see [Use STS to access OSS](reseller.en-US/SDK Reference/Node. js/Authorized access.md#).
-   \[bucket\] \{String\}: the bucket you create on the console.
-   \[endpoint\] \{String\}: oss region domain.
-   \[region\] \{String\}: the bucket data region location. The default region is oss-cn-hangzhou.
-   \[internal\] \{Boolean\}: access OSS with Alibaba Cloud internal network or not, default is false. If your servers are running on Alibaba Cloud too, you can set true to save lot of money.
-   \[secure\] \{Boolean\} instruct OSS client to use HTTPS \(secure: true\) or HTTP \(secure: false\) protocol. For more information, click [here](reseller.en-US/SDK Reference/Browser.js/FAQ.md#).
-   \[timeout\] \{String|Number\} instance level time-out for all operations. The default value is 60s.

Example:

```
// Assume that an OSS object has been introduced to the browser environment in the 'script' or 'npm' method.
let store = new OSS({
  accessKeyId: 'your access key',
  accessKeySecret: 'your access secret',
  bucket: 'your bucket name',
  region: 'oss-cn-hangzhou'
});

```

