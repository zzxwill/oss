# Use STS to access OSS {#concept_32122_zh .concept}

OSS can temporarily grant authorization for access through the Alibaba Cloud STS service.

For more information about STS, see [Concepts]().

Follow these steps to use the STS:

1.  Create a RAM user in the console of the official website. For more information, see [Overview](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#).
2.  Create an STS role in the console and grant permission to the role of the RAM user. For more information, see [Overview](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#).
3.  Use the AccessKeyID/AccessKeySecret of the RAM user to apply for a temporary token from STS.
4.  Use the authentication information in the temporary token to create an OSS client.
5.  Use the OSS client to access OSS.

You must set the `:sts_token` parameter to access OSS through STS. See the following example:

```language-ruby
require 'aliyun/sts'
require 'aliyun/oss'

sts = Aliyun::STS::Client.new(
  access_key_id: '<AccessKeyId of the RAM user>',
  access_key_secret: '<AccessKeySecret of the RAM user>')

token = sts.assume_role('<role-arn>', '<session-name>')

client = Aliyun::OSS::Client.new(
  endpoint: '<endpoint>',
  access_key_id: token.access_key_id,
  access_key_secret: token.access_key_secret,
  sts_token: token.security_token)

bucket = client.get_bucket('my-bucket')

```

You can customize an STS policy when applying for a temporary token from STS. The requested temporary permission is the intersection of the permission assigned to the role and the permission specified by the STS policy. The following code applies for the read-only permission on `my-bucket` using a specified STS policy and sets the temporary token validity period to 15 minutes:

```language-ruby
require 'aliyun/sts'
require 'aliyun/oss'

sts = Aliyun::STS::Client.new(
  access_key_id = '<The AccessKeyId of the RAM user>'
  access_key_secret = '<The AccessKeySecret of the RAM user>'

policy = Aliyun::STS::Policy.new
policy.allow(['oss:Get*'], ['acs:oss:*:*:my-bucket/*'])

token = sts.assume_role('<role arc>', '<session name>', policy, 15 * 60)

client = Aliyun::OSS::Client.new(
  endpoint: 'ENDPOINT',
  access_key_id: token.access_key_id,
  access_key_secret: token.access_key_secret,
  sts_token: token.security_token)

bucket = client.get_bucket('my-bucket')

```

For detailed usage and parameters, see [API documentation](http://www.rubydoc.info/gems/aliyun-sdk/).

