# View the object list {#concept_uzd_syy_5db .concept}

You can use the GetBucket API of OSS to list the objects that you upload in a bucket.

**Note:** For more information about the GetBucket API, see [GetBucket](../../../../reseller.en-US/API Reference/Bucket operations/GetBucket (ListObject).md#).

You can call the GetBucket API to view the list of up to 1,000 objects in a bucket at a time. The following table describes the parameters that you can use to list objects in various ways.

|Name|Description|
|:---|:----------|
|Delimiter|The character used to group objects by name. If you specify the Delimiter parameter in the request, the response contains the CommonPrefixes element. This element indicates a set of object names that share a specified prefix and end with the first specified delimiter.|
|Marker|The starting position of the object list. Objects are sorted in alphabetical order and those objects following this marker are listed.|
|MaxKeys|The maximum number of objects that are returned. The default value is 100, and the maximum value is 1000.|
|Prefix|The prefix that must be contained in the names \(key\) of the returned objects. Note that if you use a prefix to query objects, the returned key values still contain the prefix.|

## Operating methods {#section_bdy_cv3_kgb .section}

|Operating method|Description|
|----------------|-----------|
|[Console](https://oss.console.aliyun.com/overview)|Web application that lists the objects in a bucket on the Files tab of the bucket overview page|
|[ossbrowser](../../../../reseller.en-US/Tools/ossbrowser/Quick start.md#)|Graphical tool, which is easy to operate|
|[ossutil](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#ul_imw_f3s_vdb)|Command-line tool, which delivers good performance|
|[Java SDK](../../../../reseller.en-US/SDK Reference/Java/Manage objects/List objects.md#)|SDK demos in various languages|
|[Python SDK](../../../../reseller.en-US/SDK Reference/Python/Manage objects/List objects.md#)|
|[PHP SDK](../../../../reseller.en-US/SDK Reference/PHP/Manage objects/List objects.md#)|
|[Go SDK](../../../../reseller.en-US/SDK Reference/Go/Manage objects/List objects.md#)|
|[C SDK](../../../../reseller.en-US/SDK Reference/C/Manage objects/List objects.md#)|
|[.NET SDK](../../../../reseller.en-US/SDK Reference/. NET/Manage objects/List objects.md#)|
| |
|[Node.js SDK](../../../../reseller.en-US//Manage objects.md#section_vk4_ksk_lfb)|
|[Browser.js SDK](../../../../reseller.en-US/SDK Reference/Browser.js/Manage objects.md#section_ahf_xvl_lfb)|
|[Ruby SDK](../../../../reseller.en-US/SDK Reference/Ruby/Manage objects.md#)|

## Folder simulation {#section_bby_5yy_5db .section}

OSS does not support folders. All elements are stored as objects. To simulate a folder, you can create an empty object whose name ends with a forward slash \(/\). This object can be uploaded and downloaded. The console displays any object whose name ends with a forward slash \(/\) as a folder. Therefore, you can use the preceding method to create a simulated folder.

You can use the Delimiter and Prefix parameters together to simulate folders as follows:

-   If you set the Prefix parameter to a folder name in the request, the objects whose names start with this prefix are listed, including all recursive objects and subfolders \(directories\) in this folder. The object names are listed in the Contents element.
-   If you also set the Delimiter parameter to a forward slash \(/\) in the request, the objects whose names start with the specified prefix and subfolders \(directories\) in the folder are listed. The subfolders \(directories\) are listed in the CommonPrefixes element, excluding recursive objects and folders in these subfolders.

Example:

The OSS bucket `oss-sample` contains the following objects:

```
Object D
Directory A/Object C
Directory A/Object D
Directory A/Directory B/Object B
Directory A/Directory B/Directory C/Object A
Directory A/Directory C/Object A
Directory A/Directory D/Object B
Directory B/Object A
```

-   List level-1 directories and objects

    Based on the API request conventions, leave the Prefix parameter empty and set the Delimiter parameter to a forward slash \(/\). The response is as follows:

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>oss-sample</Name>
      <Prefix></Prefix>
      <Marker></Marker>
      <MaxKeys>1000</MaxKeys>
      <Delimiter>/</Delimiter>
      <IsTruncated>false</IsTruncated>
      <Contents>
        <Key>Object D</Key>
        <LastModified>2015-11-06T10:07:11.000Z</LastModified>
        <ETag>"8110930DA5E04B1ED5D84D6CC4DC9080"</ETag>
        <Type>Normal</Type>
        <Size>3340</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <CommonPrefixes>
        <Prefix>Directory A/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory B/</Prefix>
      </CommonPrefixes>
    </ListBucketResult>
    ```

    In the preceding response:

    The Contents element contains the level-1 object: Object D. The CommonPrefixes element contains the level-1 directories: Directory A/ and Directory B/, but does not list the objects in these directories.

-   List level-2 directories and objects in Directory A

    Based on the API request conventions, set the Prefix parameter to Directory A and the Delimiter parameter to a forward slash \(/\). The response is as follows:

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <ListBucketResult>
      <Name>oss-sample</Name>
      <Prefix>Directory A/</Prefix>
      <Marker></Marker>
      <MaxKeys>1000</MaxKeys>
      <Delimiter>/</Delimiter>
      <IsTruncated>false</IsTruncated>
      <Contents>
        <Key>Directory A/Object C</Key>
        <LastModified>2015-11-06T09:36:00.000Z</LastModified>
        <ETag>"B026324C6904B2A9CB4B88D6D61C81D1"</ETag>
        <Type>Normal</Type>
        <Size>2</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <Contents>
        <Key>Directory A/Object D</Key>
        <LastModified>2015-11-06T09:36:00.000Z</LastModified>
        <ETag>"B026324C6904B2A9CB4B88D6D61C81D1"</ETag>
        <Type>Normal</Type>
        <Size>2</Size>
        <StorageClass>Standard</StorageClass>
        <Owner>
          <ID>oss</ID>
          <DisplayName>oss</DisplayName>
        </Owner>
      </Contents>
      <CommonPrefixes>
        <Prefix>Directory A/Directory B/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory A/Directory C/</Prefix>
      </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Directory A/Directory D/</Prefix>
      </CommonPrefixes>
    </ListBucketResult>
    ```

    In the preceding response:

    The Contents element contains the level-2 objects: Directory A/Object C and Directory A/Object D. The CommonPrefixes element contains the level-2 directories: Directory A/Directory B/, Directory A/Directory C/, and Directory A/Directory D/, but does not list the objects in these directories.


