# Manage lifecycle rules {#concept_y2g_szy_5db .concept}

You can configure lifecycle rules for OSS buckets or objects by using the PutBucketLifecycle interface to automatically delete expired objects and parts or change the storage class of expired objects to IA or Archive, saving storage costs.

**Note:** For more information about the PutBucketLifecycle interface, see [PutBucketLifecycle](../../../../reseller.en-US/API Reference/Bucket operations/PutBucketLifecycle.md#).

A lifecycle rule includes the following information:

-   Matching policy: Specifies the objects and parts that lifecycle rule applies to.
    -   Matched by prefix: Indicates that the lifecycle rule applies to objects and parts with the specified prefix. You can create multiple lifecycle rules for objects and parts with different prefixes. The prefix specified in each rule must be different.
    -   Matched by tag: Indicates that the lifecycle rule applies to objects with the specified tag key and tag value. You can specify multiple tags for a lifecycle rule so that the rule applies to all objects with these tags. Parts cannot be matched to lifecycle rules by tags.

        **Note:** The object tagging function is in the beta testing phase. You can [open a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) to apply for the trial of this function. For more information about the object tagging function, see [Object tagging](reseller.en-US/Developer Guide/Manage files/Object tagging.md#).

    -   Matched by prefix and tag: Indicates that the lifecycle rule applies to objects with the specified prefix and one or more specified tags.
    -   Matched to the entire bucket: Indicates that the lifecycle rule applies to all objects and parts in the bucket. You can create only one lifecycle rule that applies to a bucket.
-   Object expiration policy: Specifies the expiration time for objects and the operation to be performed on expired objects.

    -   Expiration date: Specifies an expiration date and the operation to be performed on expired objects. An object modified before the specified date expires, and the specified operation is performed on the object.
    **Note:** Operations that can be performed on an expired object include: convert the storage class of the object to IA, convert the storage class of the object to Archive, and delete the object.

-   Part expiration policy: Specifies the expiration time for parts and the operation to be performed on expired parts.
    -   Expiration period: Specifies an expiration period \(N days\). Parts are deleted N days after it is modified for the last time.
    -   Expiration date: Specifies an expiration date. All parts modified before the specified date are deleted.

A rule applies to an object if the object name prefix matches the prefix specified in the rule. For example, a bucket includes the following objects:

```
logs/program.log.1
logs/program.log.2
logs/program.log.3
doc/readme.txt
```

If the prefix specified in a rule is "logs/", the rule applies to the three objects prefixed with "logs/". If the prefix specified in the rule is "doc/readme.txt", it applies only to the object named doc/readme.txt.

You can also set overdue deletion rules. For example: if the last date of objects that are prefixed with logs/ is 30 days ago, the objects are deleted according to the specified overdue deletion time.

If an object matches an overdue rule, the OSS includes the x-oss-expiration header in the response to the GetObject or HeadObject requests for the object. The header contains two key-value pairs: expiry-date indicates the expiration date of the object, and rule-id indicates the matched rule ID.

## Operation method {#section_bdy_cv3_kgb .section}

|Method|Description|
|------|-----------|
|[OSS console](../../../../reseller.en-US/Console User Guide/Manage buckets/Set lifecycle.md#)|Web application that is easy to use|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage lifecycle rules.md#)|Various and complete SDK demos of different languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage lifecycle rules.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage lifecycle rules.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage lifecycle rules.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Manage lifecycle rules.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/C/Manage lifecycle rules.md#)|
|[Node.js SDK](../../../../reseller.en-US/SDK Reference/Node. js/Manage lifecycle rules.md#)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage lifecycle rules.md#)|

## Examples {#section_qpd_yzy_5db .section}

You can set lifecycle rules for a bucket through OSS APIs. A lifecycle rule is in XML format, as shown in the following example:

```
<LifecycleConfiguration>
<Rule>
<ID>delete logs after 10 days</ID>
<Prefix>logs/</Prefix>
<Status>Enabled</Status>
<Expiration>
<Days>10</Days>
</Expiration>
</Rule>
<Rule>
<ID>delete doc</ID>
<Prefix>doc/</Prefix>
<Status>Disabled</Status>
<Expiration>
<CreatedBeforeDate>2017-12-31T00:00:00.000Z</CreatedBeforeDate>
</Expiration>
</Rule>
<Rule>
<ID>delete xx=1</ID>
<Prefix>rule2</Prefix>
<Tag><Key>xx</Key><Value>1</Value></Tag>
<Status>Enabled</Status>
<Transition>
<Days>60</Days>
<StorageClass>Archive</StorageClass>
</Transition>
</Rule>
</LifecycleConfiguration>
			
```

Elements in the preceding example are described as follows:

-   `<ID>`: Indicates the unique identifier for a rule.
-   `<Status>`: Indicate the status of the lifecycle rule with two values: Enabled or Disabled. Only rules with the Enabled status is applied.
-   `<Prefix>`: Indicates that the rule applies only to objects with the specified prefix.
-   `<Expiration>`: Indicates the operation expiration period. The sub-element `<CreatedBeforeDate>` or `<Days>` indicates the absolute and relative expiration time respectively.
    -   CreatedBeforeDate: Specifies an expiration date and the operation to be performed on expired objects. An object modified before the specified date expires, and the specified operation is performed on the object.
    -   Days: Specifies an expiration period \(N days\) and the operation to be performed on expired objects. An object expires N days after it is modified for the last time, and the specified operation is performed on the object.

The first rule indicates that objects that are prefixed with logs/ and were modified 10 days ago are deleted. The second rule indicates that objects that are prefixed with doc/ and were modified before December 31, 2014 are deleted. However, the rule does not take effect because it is in Disabled status. The third rule indicates that the storage class of objects that are tagged with "xx=1" and were modified 60 days ago is converted to Archive.

## Detail analysis {#section_dtb_c1z_5db .section}

-   Prefix and tag
    -   The naming conventions for a prefix are the same as those for an object.
    -   If the prefix in a rule is empty, the rule applies to all objects in the bucket.
    -   Prefixes specified in rules for a bucket must be unique. For example, if two rules are configured for a bucket and the prefixes in the rules are logs/ and logs/program respectively, OSS returns an error.
    -   The key of a tag cannot be empty. A tag can include letters, numbers, spaces, and the following symbols:

        + ‑ = . \_ : /

    -   The prefixes in the matching policies \(matched by prefix and tag\) of different rules can be overlapped. For example, assume that rule 1 and rule 2 are configured for the same bucket. In rule 1, the specified prefix is `logs/` and the specified tag is "K1=V1". In rule 2, the specified prefix is `logs/program` and the specified tag is "K1=V2". Then, rule 1 applies to objects with the `logs` or `logs/program` prefix and the "K1=V1" tag. Rule 2 applies to objects with the `logs` prefix and the "K1=V1" tag.
-   Effective time
    -   If a rule is set to delete objects on a specified date, the date must be midnight UTC and comply with the ISO8601 format, for example, 2017-01-01T00:00:00.000Z. In this example, OSS deletes matched objects after midnight on January 1, 2017.
    -   If a rule is set to delete objects after a specified number of days, OSS sums up the last update time \(Last-Modified\) and the specified number of days, and then round the sum to the midnight UTC timestamp. For example, if the last update time of an object is 01:00 a.m. on April 12, 2014 and the number of days specified in the matched rule is 3, the expiry time is zero o'clock on April 16, 2017.
    -   OSS deletes the objects matched the rule at the specified time. Note that objects are usually deleted shortly after the specified time.
    -   The update time of an unmodified object is typically the time of its creation. If an object undergoes the put operation for multiple times, the last update time is the time of the last Put operation. If an object was copied to itself, the last update time is the time at when the object was last copied.
-   Fees

    Only successful lifecycle asynchronous request operations are recorded and charged.

-   Actions performed when rules conflict
    -   The prefixes and tags specified in two rules are the same.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1|abc|a=1|Matched objects are deleted after 20 days.|
        |rule2|abc|a=1|The storage class of matched objects is converted to Archive after 20 days.|

        Result: Objects with the abc prefix and the "a=1" tag are deleted after 20 days \(deletion is performed first\). The second rule does not take effect because the matched objects have already been deleted.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1|abc|a=1|The storage class of matched objects is converted to IA after 365 days.|
        |rule2|abc|a=1|The storage class of matched objects is converted to Archive before March, 1, 2018.|

        Result: If objects with the abc prefix and the "a=1" tag match the two rules at the same time, the storage of the objects is converted to Archive. If the objects only match one of the rules, the action specified in the matched rule is performed.

    -   The tags specified in two rules are the same, and the prefixes specified in the rules are overlapped.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1| |a=1|The storage class of matched objects is converted to IA after 20 days.|
        |rule2|abc|a=1|Matched objects are deleted after 120 days.|

        Result: The storage class of all objects with the "a=1" tag is converted to IA after 20 days. Objects with the abc prefix and the "a=1" tag are deleted after 120 days.

        |Rule|Prefix|Tag|Action|
        |----|------|---|------|
        |rule1| |a=1|The storage class of matched objects is converted to Archive after 10 days.|
        |rule2|abc|a=1|The storage class of matched objects is converted to IA after 20 days.|

        Result: The storage class of all objects with the "a=1" tag is converted to Archive. The second rule does not take effect on objects with the abc prefix and the "a=1" tag because the storage class of Archive objects cannot be converted to IA.


## FAQ {#section_e1n_vlx_dgb .section}

-   Is there a minimum storage period for a lifecycle rule if the rule is set to convert storage class or delete an expired object?

    If a lifecycle rule is set to convert the storage class \(from Standard to IA or Archive or from IA to Archive\) of objects, no minimum storage period is required for the rule. However, a lifecycle rule set to delete expired objects requires a minimum storage period. The minimum storage period is 30 days for objects of the IA storage class and 60 days for objects of the Archive storage class.

    The minimum storage period for a lifecycle rule set to delete an expired object indicates the time period from the time when the object is created until the time when the object is deleted. If the object is modified \(such as by CopyObject or AppendObject operations\) during this period, the minimum storage period is calculated from the last modified date.

    Example 1: The storage class of an object is converted from IA to Archive 10 days after it is created. The creation time of the object does not change. The converted object must be stored for at least 50 days.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4377/155566984834477_en-US.png)

    Example 2: If the storage class of an object is converted from IA to Archive through a CopyObject operation 10 days after it is created, fees of 20 days are incurred for the deletion. The creation time of the object updates. The converted object must be stored for at least 60 days.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4377/155566984834481_en-US.png)

-   Billing logic for requests

    The conversion of storage class or the deletion of expired objects performed according to lifecycle rules generate requests. The requests are charged by OSS. For example:

    -   If the storage class of 1000 objects are converted from Standard to Archive according to a lifecycle rule, 1000 POST requests are generated.
    -   If 1000 expired objects are deleted according to a lifecycle rule, 1000 Delete requests are generated.
    For more information, see [Billing methods](../../../../reseller.en-US/Pricing/Billing items.md#).

-   Are the conversion of storage class or the deletion of expired objects performed according to lifecycle rules recorded?

    The conversion of storage class or the deletion of expired objects performed according to lifecycle rules are recorded in logs. Fields in the logs are described as follows:

    -   Operation
        -   CommitTransition: Indicates that the rule is set to convert the storage class of objects.
        -   ExpireObject: Indicates that the rule is set to delete expired objects.
    -   Sync Request
        -   lifecycle: Indicates the operation to be performed by the lifecycle rule.

