# Use STS to access OSS {#concept_32077_zh .concept}

OSS can temporarily grant authorization for access through the Alibaba Cloud STS service.

To use STS, follow these steps:

1.  Create a RAM user in the console of the official website. For more information, see [Overview](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#).
2.  Create an STS role in the console and grant permission to the role of the RAM user. For more information, see [Overview](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#).
3.  Use the AccessKeyID/AccessKeySecret of the RAM user to apply for a temporary token from STS.
4.  Use the authentication information in the temporary token to create an OSS client.
5.  Use the OSS client to access OSS.

You must set the stsToken parameter to access OSS with STS, as shown in the following example:

```
let OSS = require('ali-oss');
let STS = OSS.STS;
let sts = new STS({
  access_key_id = '<The AccessKeyId of the RAM user>'
  access_key_secret = '<The AccessKeySecret of the RAM user>'
});
async function assumeRole () {
  try {
    let token = await sts.assumeRole(
    '<role-arn>', '<policy>', '<expiration>', '<session-name>');
    let client = new OSS({
      region: '<region>',
      accessKeyId: token.credentials.AccessKeyId,
      accessKeySecret: token.credentials.AccessKeySecret,
      stsToken: token.credentials.SecurityToken,
      bucket: '<bucket-name>'
    });
  } catch (e) {
    console.log(e);
  }
}
assumeRole();
```

You can customize an STS policy when applying for a temporary token from STS. The requested temporary permission is the intersection of the permission assigned to the role and the permission specified by the STS policy. The following code applies for the read-only permission on `my-bucket` using a specified STS policy and sets the temporary token validity period to 15 minutes.

```
let OSS = require('ali-oss');
let STS = OSS.STS;
let sts = new STS({
  access_key_id = '<The AccessKeyId of the RAM user>'
  access_key_secret = '<The AccessKeySecret of the RAM user>'
});
let policy = {
  "Statement": [
    {
      "Action": [
        "oss:Get*"
      ],
      "Effect": "Allow",
      "Resource": ["acs:oss:*:*:my-bucket/*"]
    }
  ],
  "Version": "1"
};
async function assumeRole () {
  try {
    let token = await sts.assumeRole(
    '<role-arn>', policy, 15 * 60, '<session-name>');
    let client = new OSS({
      region: '<region>',
      accessKeyId: token.credentials.AccessKeyId,
      accessKeySecret: token.credentials.AccessKeySecret,
      stsToken: token.credentials.SecurityToken,
      bucket: '<bucket-name>'
    });
  } catch (e) {
    console.log(e);
  }
}
assumeRole();
```

