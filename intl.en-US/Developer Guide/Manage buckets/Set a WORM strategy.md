# Set a WORM strategy {#concept_lnj_4rl_cfb .concept}

A Write Once Read Many \(WORM\) strategy is used to specify the protection period for files in the bucket. No one can modify or delete the files during the protection period.

**Note:** Currently, OSS only supports the WORM strategy at bucket level.

## Strategy description {#section_nk2_fz5_cfb .section}

Currently, OSS allows only one time-based WORM strategy, and the protection period \(lifecycle\) is 1 day to 70 years.

Assume that you created a bucket named examplebucket on June 1, 2013, and then uploaded file1.txt, file2.txt, and file3.txt to the bucket at different times. After that , a WORM strategy was created on July 1, 2014 with a protection period of 5 years on the bucket. The specific upload time for the preceding three files and their corresponding WORM strategy expiration dates are as follows:

|File name|Upload time|Expiration date of WORM strategy|
|:--------|:----------|:-------------------------------|
|file1.txt|June 1, 2013|May 31, 2018|
|file2.txt|July 1, 2014|June 30, 2019|
|file3.txt|September 30, 2018|September 29, 2023|

You can also specify the effective rules, modification rules and deletion rules for a time-based WORM strategy.

-   Effective rules

    When a time-based WORM strategy is created, the policy is in the InProgress state by default, and the state is valid for 24 hours. Within 24 hours of the validity period, the bucket resources under this policy are protected.

    -   Within 24 hours after the creation of the WORM strategy: The bucket owner and authorized users can modify or delete the strategy if it is not locked. If the WORM strategy is locked, then the policy cannot be modified and deleted, but you can extend the protection period.
    -   24 hours later after the creation of the WORM strategy: If the WORM strategy is not locked, then the policy automatically expires.
-   Modification rules

    For the InProgress and Locked state of the time-based WORM strategy, two kinds of modification rules are provided:

    -   When the WORM strategy is in InProgress state, it can be deleted only.
    -   When the WORM strategy is in Locked state, the protection period \(Lifecycle\) can be extended, but the strategy cannot be deleted.
-   Deletion rules

    A time-based WORM strategy is the metadata property of a bucket. When you delete a bucket, its corresponding WORM strategy and access policy are also deleted.


## Reference {#section_gq3_zz5_cfb .section}

Console：[Set a WORM strategy](../../../../intl.en-US/Console User Guide/Manage buckets/Set a WORM strategy.md#)

