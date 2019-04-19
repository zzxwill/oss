# 使用STS进行临时授权 {#concept_32122_zh .concept}

OSS 可以通过阿里云 STS \(Security Token Service\) 进行临时授权访问。

阿里云 STS 是为云计算用户提供临时访问令牌的 Web 服务。通过 STS，您可以为第三方应用或子用户（即用户身份由您自己管理的用户）颁发一个自定义时效和权限的访问凭证。STS 更详细的解释请参见[STS介绍](../../../../cn.zh-CN/API 参考（STS）/简介.md#)。

STS 的优势如下：

-   您无需透露您的长期密钥（AccessKey）给第三方应用，只需生成一个访问令牌并将令牌交给第三方应用。您可以自定义这个令牌的访问权限及有效期限。
-   您无需关心权限撤销问题，访问令牌过期后自动失效。

使用 STS 访问 OSS 的流程请参见开发指南中的[STS临时授权访问OSS](../../../../cn.zh-CN/开发指南/身份认证/STS临时授权访问OSS.md#)。

使用STS访问OSS时，需要设置:sts\_token参数：

```language-ruby
require 'aliyun/sts'
require 'aliyun/oss'

sts = Aliyun::STS::Client.new(
  access_key_id: '<子账号的AccessKeyId>',
  access_key_secret: '<子账号的AccessKeySecret>')

token = sts.assume_role('<role-arn>', '<session-name>')

client = Aliyun::OSS::Client.new(
  endpoint: '<endpoint>',
  access_key_id: token.access_key_id,
  access_key_secret: token.access_key_secret,
  sts_token: token.security_token)

bucket = client.get_bucket('my-bucket')
		
```

向STS申请临时token时，还可以指定自定义的STS Policy。这样申请的临时权限是所扮演角色的权限与Policy指定的权限的交集。以下示例将通过指定STS Policy，申请对`my-bucket`的只读权限，并指定临时token的过期时间为15分钟：

```language-ruby
require 'aliyun/sts'
require 'aliyun/oss'

sts = Aliyun::STS::Client.new(
  access_key_id: '<子账号的AccessKeyId>',
  access_key_secret: '<子账号的AccessKeySecret>')

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

更详细的用法和参数说明请参考[API文档](http://www.rubydoc.info/gems/aliyun-sdk/)。

