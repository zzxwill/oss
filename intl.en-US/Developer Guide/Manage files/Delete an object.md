# Delete an object {#concept_g42_bhd_5db .concept}

You can delete objects that have been uploaded to OSS buckets using one of the following methods:

-   Single deletion, in which only a specified object is deleted.
-   Batch deletion, in which up to 1,000 objects can be deleted at one time.
-   Auto deletion, in which large numbers of objects can be deleted according to certain rules. For example, to regularly delete objects that are created a specified number of days ago, or to regularly empty the entire bucket, we recommend that you use [Lifecycle management](intl.en-US/Developer Guide/Manage files/Manage object lifecycle.md#). Once the rules are specified, OSS uses these rules to recycle expired objects. This reduces the number of user attempts at deletion requests, and helps streamline the deletion process.

## Reference {#section_dk3_2hd_5db .section}

-   API: [Delete Object](../../../../intl.en-US/API Reference/Object operations/DeleteObject.md#) and [Delete Multiple Objects](../../../../intl.en-US/API Reference/Object operations/DeleteMultipleObjects.md#)
-   SDK: Java SDK [Delete Files](https://www.alibabacloud.com/help/doc-detail/32015.htm)
-   Console: [Delete Files](../../../../intl.en-US/Console User Guide/Manage objects/Delete an object.md#)

