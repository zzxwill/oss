# HTTP 下载 {#concept_eqp_5th_dhb .concept}

您可以在不使用 SDK 的情况下，直接使用 HTTP 下载存放在 OSS 中的文件。

HTTP 下载包括直接使用浏览器下载，或者使用`wget`, `curl`等命令行工具下载。文件的 URL 由SDK生成。使用`signatureUrl`方法生成可下载的HTTP地址，URL 的有效时间默认为半小时。

HTTP 下载 OSS 中的文件的示例代码如下：

```language-js
let OSS = require('ali-oss');

let client = new OSS({
  region: '<Your region>',
  accessKeyId: '<Your AccessKeyId>',
  accessKeySecret: '<Your AccessKeySecret>',
  bucket: 'Your bucket name'
});

let url = client.signatureUrl('object-name');
console.log(url);

let url = client.signatureUrl('object-name', {expires: 3600});
console.log(url);

// signed URL for PUT
let url = client.signatureUrl('object-name', {method: 'PUT'});
console.log(url);

```

