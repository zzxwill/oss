# Set a retention strategy {#task_lnq_csm_cfb .task}

A retention strategy is used to specify the protection period of objects in a bucket. No one can modify or delete a protected object during the protection period. For more information, see [Set a retention strategy](../../../../../reseller.en-US/Developer Guide/Buckets/Set a WORM strategy.md#). Currently, you can set a retention strategy only for a bucket in the China South 1 \(Shenzhen\) region.

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss). 
2.  In the bucket list on the left, click the name of the target bucket. 
3.  Click the **Basic Settings** tab, locate the **Retention Strategy** area, and click **Configure**. 
4.  Click **Create Strategy** to open the Create Strategy dialog box. 
5.  Set the **Retention period** for the retention strategy. The value range of **Retention period** is 1 day to 70 years.
6.  Click **OK**. 

    **Note:** After a retention strategy is created, it is in the IN\_PROGRESS state. You can **Lock** or **Delete** a retention strategy in this state.

7.  Click **Lock**. 

    **Note:** You cannot delete a locked retention strategy or shorten the retention period of it. Therefore, set a retention strategy with caution.

8.  Confirm the retention strategy and click **OK**. 

    **Note:** After this step, the strategy is in the LOCKED state. You can click **Edit** to extend the retention period.


