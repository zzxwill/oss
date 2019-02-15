# Set a retention strategy {#concept_lnj_4rl_cfb .concept}

A retention strategy is used to specify the protection period for objects stored in a bucket. No one can modify or delete objects protected by a retention strategy within the protection period of the strategy.

**Note:** Currently, you can set a retention strategy only for a bucket in the China South 1 \(Shenzhen\) region.

## Strategy description {#section_nk2_fz5_cfb .section}

Currently, you can only add one time-based retention strategy with a retention period ranging from 1 day to 70 years.

Assume that you created a bucket named examplebucket on June 1, 2013, and then uploaded three objects named file1.txt, file2.txt, and file3.txt to the bucket at different times. After that, a retention strategy was created for the bucket on July 1, 2014 with a protection period of 5 years. The following table describes the upload dates and expiration dates of the preceding three objects.

|Object name|Upload date|Expiration date|
|:----------|:----------|:--------------|
|file1.txt|June 1, 2013|May 31, 2018|
|file2.txt|July 1, 2014|June 30, 2019|
|file3.txt|September 30, 2018|September 29, 2023|

You can also specify the effective rules, modification rules and deletion rules for a time-based retention strategy.

-   Effective rules

    When a time-based retention strategy is created for a bucket, it is in the InProgress state by default, and the state is valid for 24 hours. Within the validity period, resources that apply to the strategy in the bucket are protected.

    -   Within 24 hours after a retention strategy is created for a bucket, the bucket owner and authorized users can modify or delete the strategy if it is not locked. If the retention strategy is locked, it cannot be modified and deleted. However, you can extend the protection period of the strategy.
    -   After a retention strategy is created for 24 hours, the strategy automatically expires if it is not locked.
-   Modification rules

    Two modification rules are provided for time-based retention strategies in the InProgress and Locked states.

    -   A retention strategy in the InProgress state can be deleted only.
    -   A retention strategy in the Locked state cannot be modified or deleted. You can only extend the protection period of the strategy.
-   Deletion rules
    -   A time-based retention strategy is a metadata property of a bucket. When a bucket is deleted, the retention strategy and access strategies set for the bucket are also deleted. Therefore, the owner of an empty bucket can delete the retention strategy set for the bucket by deleting the bucket.
    -   Within 24 hours after a retention strategy is created for a bucket, the bucket owner and authorized users can modify or delete the strategy if it is not locked.
    -   If a bucket stores objects which are in the protection period of the retention strategy, you cannot delete neither the bucket nor the retention strategy set for the bucket.

## Reference {#section_gq3_zz5_cfb .section}

For more information about how to set a retention strategy in the OSS console, see [Set a retention strategy](../../../../../reseller.en-US/Console User Guide/Manage buckets/Set a retention strategy.md#).

