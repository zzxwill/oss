# 使用STS访问 {#concept_32122_zh .concept}

OSS可以通过阿里云STS服务，临时进行授权访问。

更多有关STS的内容请参考：[阿里云STS]()。

使用STS时请按以下步骤进行：

1.  在官网控制台创建子账号，参考[OSS STS](../../../../cn.zh-CN/最佳实践/权限管理/权限管理概述.md#)。
2.  在官网控制台创建STS角色并赋予子账号扮演角色的权限，参考[OSS STS](../../../../cn.zh-CN/最佳实践/权限管理/权限管理概述.md#)。
3.  使用子账号的AccessKeyId/AccessKeySecret向STS申请临时token。
4.  使用临时token中的认证信息创建OSS的Client。
5.  使用OSS的Client访问OSS服务。

在使用STS访问OSS时，需要设置`:sts_token`参数，如下面的例子所示：

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

在向STS申请临时token时，还可以指定自定义的STS Policy。这样申请的临时权 限是所扮演角色的权限与Policy指定的权限的交集。下面的例子将通过指定 STS Policy申请对`my-bucket`的只读权限，并指定临时token的过期时间为15分 钟：

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

