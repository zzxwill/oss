# Storage class conversion {#concept_p13_zmz_5db .concept}

## Lifecycle Object Transition {#section_oyb_bnz_5db .section}

OSS supports three [storage classes](../../../../intl.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#): Standard, Infrequent Access, and Archive.

The Object Transition mechanism is now available in OSS Lifecycle Management function in all regions across China. The following storage classes are supported for automatic conversion:

-   Standard -\> Infrequent Access
-   Standard -\> Archive
-   Infrequent Access -\> Archive

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4410/15362220611620_en-US.png)

**Examples**

You can configure lifecycle policies for objects with a given prefix in one bucket as follows:

-   They are converted to Infrequent Access class after being stored for 30 days.
-   They are converted to Archive class after being stored for 180 days.
-   They are deleted automatically after being stored for 360 days.

You can complete the configuration of the preceding lifecycle policies in the console. For more information, see [Set lifecycle](../../../../intl.en-US/Console User Guide/Manage buckets/Set lifecycle.md#).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4410/15362220611622_en-US.png)

**Note:** If the following three parameters are configured:

Transition to IA After, Transition to Archive After, and Delete All Objects After Specified Days, then the number of days set for each parameter must meet the following criteria:

Days for converting to Infrequent Access  < Days for converting to Archive < Specified days for deleting

**Notes**

After the Object type conversion, the storage cost is calculated based on the unit price of converted storage class.

Notes for Infrequent Access and Archive storage types:

-   Minimum billable size: 

    Objects smaller than 128 KB are charged as 128 KB.

-   Minimum storage period: 

    The stored data is required to be saved for at least 30 days. Charges will be incurred if you delete files that are stored for less than 30 days.

-   Restore time of Archive type: 

    It takes one minute for Archive type Object to restore the data to a readable state. If real-time read is required in the business scenario, we recommend that you convert the file to the Infrequent Access storage class instead of Archive class. Otherwise, after converting the file to the Archive class, the data cannot be read in real time.

-   Data access charges: 

    Both Infrequent Access and Archive classes are required to pay data access charges as a separate charge item to outbound traffic. If the average access frequency per Object is higher than once per month, you are not advised to convert the data to Infrequent Access or Archive class.


## Storage classes conversion in other ways {#section_zdt_5nz_5db .section}

For conversions from Archive type to Standard class or Infrequent Access class, or from Infrequent Access class to Standard class, you can read the Object and rewrite it to the Bucket of corresponding storage class. The default storage class of Object is determined by the Bucket.

For example, for the conversion of Infrequent Access Object in the Bucket of Standard type to Standard Object, you can read and rewrite the Object. Based on the type of the Bucket, the newly-written Object is of Standard storage class.

For the Object that has been converted to Archive class, you can only read it after performing Restore operation and restore it to a readable state.

For mor einformation, see [Create and use the Archive bucket](../../../../intl.en-US/Developer Guide/Storage classes/Create and use the Archive bucket.md#).

