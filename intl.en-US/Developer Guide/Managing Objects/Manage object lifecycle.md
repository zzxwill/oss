# Manage object lifecycle {#concept_y2g_szy_5db .concept}

OSS provides  Object Lifecycle Management to manage objects for you. The lifecycle of a bucket can be configured to define various rules for the bucket’s objects.  Currently, you can use rules to delete matching objects.   Each rule is composed of the following:

-   Object name prefix

    This rule only applies to objects with a matching prefix.

-   Operation

    The operation you want to perform on the matching objects.

-   Date or number of days

    The operation is executed on objects on the specified date, or across a specified number of days, after the object’s last modification time.


A rule applies to an object if the object name prefix matches the rule prefix. For example, a bucket has the following objects:

```
logs/program.log. 1
logs/program.log. 2
logs/program.log. 3
doc/readme.txt
```

If the prefix of a rule is logs/, the rule applies to the first three objects that are prefixed with logs/. If the prefix of a rule is doc/readme.txt, the rule only applies to the object doc/readme.txt.

You can also set overdue deletion rules. For example: if the last date of objects that are prefixed with logs/ is 30 days ago, the objects are deleted according to the specified overdue deletion time.

When an object matches an overdue rule, the OSS includes the x-oss-expiration header in the response to the Get Object or Head Object requests.  The header contains two key-value pairs: expiry-date indicates the expiration date of the object; rule-id indicates the matched rule ID.

## Example {#section_qpd_yzy_5db .section}

You can set the lifecycle configurations of a bucket through the open interface of the OSS.  Lifecycle configurations are given in XML format. The following is a specific example.

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
                <CreatedBeforeDate>2014-12-31T00:00:00.000Z</CreatedBeforeDate>
                </Expiration>
                </Rule>
                </LifecycleConfiguration>
```

In the preceding example, all elements are described as follows:

-   `<ID>`: a unique identifier of each rule.
-   `<Status>`: Enabled or Disabled. OSS only supports the Enabled rules.
-   `<Prefix>`: the prefix.
-   `<Expiration>`: the operation expiration date.  The sub-elements `<CreatedBeforeDate>` and `<Days>` specify the absolute and relative expiry time, respectively.
    -   <CreatedBeforeDate\> indicates that files with a last modification time before 2014-12-31T00:00:00.000Z are deleted.  Objects modified after this time are not deleted.
    -   <Days\> indicates that files that were last modified more than 10 days ago are deleted.

In the first rule, the OSS deletes objects that are prefixed with logs/ and were last updated 10 days ago.  The second rule indicates that objects prefixed with doc/ that were last modified before December 31, 2014 are deleted, but the rule does not take effect because it is in disabled status.

## Detailed analysis {#section_dtb_c1z_5db .section}

-   The naming rules of the prefix are the same as those of the object.
-   When the prefix is empty, the rule applies to all objects in the bucket.
-   Each prefix of a rule must be unique. For example, if a bucket has two rules whose prefixes are logs/ and logs/program, OSS returns an error.
-   If a rule is set to delete objects on a specific date, the date must be midnight UTC and comply with the ISO8601 format, for example, 2014-01-01T00:00:00.000Z. In this example, OSS deletes matched objects after midnight on January 1, 2014.
-   If, in a rule to delete objects, the number of days is specified, OSS sums up the last update time \(Last-Modified\) and the specified number of days, and then round the sum to the midnight UTC timestamp. For example, if the last update time of an object is 01:00 a.m. on April 12, 2014 and the number of days specified in the matched rule is 3, the expiry time is  midnight on April 16, 2014.
-   OSS deletes the objects matched with the rule at the specified time.  Note that objects are usually deleted shortly after the specified time.
-   The update time of an unmodified object is typically the time of its creation. If an object undergoes the put operation multiple times, the last update time is the time of the last Put operation. If an object was copied to itself, the last update time is the time at when the object was last copied.

## Reference {#section_qp1_d1z_5db .section}

-   API：[Put Bucket Lifecycle](../intl.en-US/API Reference/Bucket operations/Put Bucket Lifecycle.md#)
-   Console：[Set lifecycle](../intl.en-US/Console User Guide/Manage buckets/Set lifecycle.md#)

