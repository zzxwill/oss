# Set a WORM strategy {#task_lnq_csm_cfb .task}

A Write Once Read Many \(WORM\) strategy is used to specify the protection period of files in the bucket. No one can modify or delete files during the protection period.

1.  Log on to the [OSS console](https://partners-intl.console.aliyun.com/#/oss). 
2.  In the bucket list on the left, click the name of the target bucket. 
3.  Click the **Basic Settings** tab, locate WORM Settings area, and click **Configure**. 
4.  Click **Create Strategy** to open the Create Strategy dialog box. 
5.  Set the **Lifecycle** for the WORM strategy. The value range of **Lifecycle** is 1 day to 70 years.
6.  Click **OK**. After a WORM strategy is created, it is in IN\_PROGRESS state.
7.  Click **Lock**. After a WORM strategy is locked, it cannot be deleted, but you can click **Edit** to extend the lifecycle of the strategy.

