# Set lifecycle {#concept_bmx_p2f_vdb .concept}

You can define and manage the lifecycle of all or a subset of objects in a bucket by specifying a key name prefix in the console. Lifecycle rules are generally applied to operations such as batch file management and automatic fragment deletion.

-   For objects that match such a rule, the system makes sure that data is purged or converted to another storage type within two days of the effective date.
-   Data deleted in batch based on a lifecycle rule can never be restored, so use caution when configuring such a rule.

## Procedure {#section_oqf_kff_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  In the left-side bucket list, click the name of the target bucket to open the overview page of the bucket.
3.  Click the **Basic Settings** tab, locate the **Lifecycle** area, and then click **Edit**.
4.  Click **Create Rule**to open the **Create Lifecycle Rule** dialog box.
5.  Configure the lifecycle rule.
    -   **Status**: Specify the status of the rule, whether it is enabled or disabled.
    -   **Policy**: Select an object matching policy. You can select either **Match by Prefix** \(matching by object name prefix\) or **Apply to  Bucket** \(matching all objects in the bucket\).
    -   **Prefix**: If you select **Match by Prefix**  for the **Policy**, enter the prefix of the object name.  For example, you have stored some image objects in a bucket, and the names of these objects are prefixed with img/ . To perform lifecycle management on these objects, enter  img/in this field.
    -   **Delete Files**
        -   **Expiration Period**: Specify the number of days for which an object file is retained since it was last modified. Once the period expires, the system triggers the rule and deletes the file or converts it to another storage type \(Infrequent Access or Archive\).  For example, if it is set to 30 days, objects last modified on January 1, 2016 are scanned and deleted or converted to another storage type by the backend program on January 31, 2016. Configuration options include:
            -   Transition to IA after specified days
            -   Transition to Archive after specified days
            -   Delete all objects after specified Days
        -   **Expiration date**: Delete all the files that were last modified before the specified date or convert them to another storage type \(Infrequent Access or Archive\). For example, if it is set to 2012-12-21, objects last modified before this date are scanned and deleted or converted to another storage type by the backend program.  Configuration options include:
            -   Transition to IA after specified date
            -   Transition to Archive after specified date
            -   Delete files before specified date
        -   **Not Enabled**: Disable auto-deletion of files or storage type conversion.
    -   **Delete Fragments**
        -   **Expiration Period**: Specify the number of days for a multipart upload event is retained since it was initialized. Once the period expires, the system triggers the rule and deletes the event.  For example, if it is set to 30 days, events initialized on January 1, 2016 are scanned and deleted by the backend program on January 31, 2016.
        -   **Expiration date**: Delete all multipart upload events initialized before the specified date. If it is set to 2012-12-21, the upload events initialized before this date are scanned and deleted by the backend program.
        -   **Note Enabled**: Disable auto-deletion of fragments.
6.  Click **OK**.

**Note:** After a lifecycle rule is saved successfully, you can **edit** or  **delete**  it in the policy list.

