# Create a folder {#concept_ohr_sqg_vdb .concept}

Alibaba Cloud OSS does not has the term folder. All elements are stored as objects. To use a folder in the OSS console, you actually create an object with a size of 0 ending with a slash \(/\) used to sort the same type of files and process them in batches. By default, the OSS console displays objects ending with a slash as folders. These objects can be uploaded and downloaded normally. In the OSS console, you can use OSS folders like using folders in the Windows operating system.

**Note:** The OSS console displays any object ending with a slash as a folder, whether or not it contains data. The object can be downloaded only using an application programming interface \(API\) or software development kit \(SDK\).  For more information about how to create and use simulated folders, see [API - Get Bucket](../intl.en-US/API Reference/Bucket operations/GetBucket (List Object).md#) and Folder Simulation in [Java SDK- Object](https://www.alibabacloud.com/help/zh/doc-detail/32015.htm).

## Procedure {#section_xf5_dsg_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Click to open the target bucket.
3.  Select the **Files** tab.
4.  Click **Create Directory**, and enter a directory name.
5.  Click **OK**.

