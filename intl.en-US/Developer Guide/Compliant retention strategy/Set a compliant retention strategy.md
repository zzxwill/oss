# Set a compliant retention strategy {#task_lnq_csm_cfb .concept}

A compliant retention strategy is used to specify the protection period of objects in a bucket. No one can modify or delete a protected object during the protection period.

**Note:** 

-   For more information about compliant retention strategies, see [Introduction](reseller.en-US/Developer Guide/Compliant retention strategy/Introduction.md#).
-   The compliant retention strategy feature is in the beta testing phase in the China South 1 \(Shenzhen\) region and will be supported in all regions.

## Procedure {#section_loz_0pl_8hk .section}

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss).
2.  In the bucket list on the left, click the name of the bucket that you want to set a compliant retention strategy for.
3.  Click the **Basic Settings** tab, locate the **Retention Strategy** area, and click **Configure**.
4.  Click **Create Strategy**.
5.  In the Create Strategy dialog box, set the **Retention period** for the compliant retention strategy. The value range of **Retention period** is 1 day to 70 years.
6.  Click **OK**.

    **Note:** After a compliant retention strategy is created, it is in the IN\_PROGRESS state. You can **Lock** or **Delete** a compliant retention strategy in this state.

7.  Click **Lock**.
8.  Confirm the compliant retention strategy and click **OK**.

    **Note:** 

    -   After this step, the strategy is in the LOCKED state. You can click **Edit** to extend the retention period.
    -   If you try to delete or modify the data in a bucket protected by a compliant retention strategy in the console, the `The file is locked and cannot be operated` error message is returned.

## Calculate the retention time for an object {#section_sxh_avf_fed .section}

You can calculate the retention time for a protected object by adding the last update time of the object and the retention period specified in the compliant retention period. For example, a compliant retention strategy is set for a bucket, and the retention period specified in the strategy is 10 days. If the last update time of an object in the bucket is 2019-3-1 12:00, the object is protected until 2019-3-11 12:01. For more information about the compliant retention strategy, see [Introduction](reseller.en-US/Developer Guide/Compliant retention strategy/Introduction.md#ul_imr_nz5_cfb).

