# Manage lifecycle rules {#concept_32124_zh .concept}

OSS allows you to set lifecycle rules for buckets to automatically remove expired objects and save the storage space.

You can configure lifecycle rules for OSS buckets or objects to automatically delete expired objects and parts or change the storage class of expired objects to IA or Archive, saving storage costs. A lifecycle rule includes the following elements:

-   Rule ID: Identifies a lifecycle rule. A rule ID is unique in a bucket.
-   Strategy: You can set one of the following two application strategies for a rule. The strategy for all rules set for a bucket must be the same.
    -   The rule applies to objects with specified prefixes. You can create multiple rules with this strategy. Prefixes specified in different rules must be different.
    -   The rule applies to the entire bucket. You can create only one rule with this strategy.
-   Expiration period:
    -   Specifies the expiration period \(N days\) of a lifecycle rule. An object expires N days after it is modified for the last time.
    -   Specifies an expiration time. Objects modified before the time expire.
-   Status: Indicates whether the rule is enabled or disabled.

For more information, see [Lifecycle rules](../../../../reseller.en-US/Developer Guide/Manage files/Manage lifecycle rules.md#).

## Configure lifecycle rules {#section_nbs_prn_lfb .section}

You can use `Bucket#lifecycle=` to configure lifecycle rules:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket.lifecycle = [
  LifeCycleRule.new(
    :id => 'rule1', :enabled => true, :prefix => 'foo/', :expiry => 3),
  LifeCycleRule.new(
    :id => 'rule2', :enabled => false, :prefix => 'bar/', :expiry => Date.new(2016, 1, 1))
]
			
```

## View lifecycle rules { .section}

You can use `Bucket#lifecycle` to view lifecycle rules:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

rules = bucket.lifecycle
puts rules
			
```

## Clear lifecycle rules { .section}

You can use `Bucket#lifecycle=` to configure an empty rule array to clear lifecycle rules:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket.lifecycle = []
			
```

