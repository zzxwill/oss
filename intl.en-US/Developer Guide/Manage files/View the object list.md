# View the object list {#concept_uzd_syy_5db .concept}

You can use this feature to view the objects uploaded to your bucket. Up to 1,000 objects in a selected bucket can be displayed at one time.  The following four parameters provide users with extended capabilities:

|Name|Function|
|:---|:-------|
|Delimiter|Groups object name characters.  All objects whose names are found between the specified prefix and the first occurrence of the Delimiter act as a group of elements:  CommonPrefixes.|
|Marker|Sets up the returned results to begin from the first entry after the Marker, and is sorted in alphabetical order.|
|MaxKeys|Limits the maximum number of objects returned for one request. If this parameter specified, the default value is 100. The MaxKeys value cannot exceed 1,000.|
|Prefix|Indicates that only the objects whose keys contain the specified prefix are returned. Note that keys returned from queries using a prefix still contains the prefix.|

## Folder simulation {#section_bby_5yy_5db .section}

OSS does not support folders, or directory sorting. All elements are stored as objects. Creating a simulated folder means creating an object with a size of 0 that can then be uploaded and downloaded. The console displays any object ending with "/" as a folder. So you can use the preceding method to create a simulated folder.

Users can use a combination of Delimiters and Prefixes to simulate folder functions as follows: The combination of Delimiter and Prefix is as follows:

-   Setting the Prefix as the name of a folder enumerates the files starting with this prefix,  recursively returning all files and subfolders \(directories\) in this folder.  The file names are shown in Contents.
-   Setting the Delimiter as "/" means that the returned values enumerate the files in the folder and the subfolders \(directories\) returned in the CommonPrefixes section. Recursive files and folders in subfolders are not displayed.

```
For example:
In this example, the OSS bucket oss-sample, contains the following objects:
File D
Directory A/File C
Directory A/File D
Directory A/Directory B/File B
Directory A/Directory B/Directory C/File A
Directory A/Directory C/File A
Directory A/Directory D/File B
Directory B/File A
1. List first-level directories and files
Based on the API request conventions, you must set the Prefix to "", and the Delimiter to "/":
The returned results are as follows:
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>oss-sample</Name>
  <Prefix></Prefix>
  <Marker></Marker>
  <MaxKeys>1000</MaxKeys>
  <Delimiter>/</Delimiter>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>File D</Key>
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
We can see that:
Contents returns the first-level file: "File D".
CommonPrefixes returns the first-level directories: "Directory A/" and "Directory B/", but the files in these directories are not shown.
2. List second-level directories and files under Directory A
Based on the API request conventions, you must set the Prefix to "Directory A", and the Delimiter to "/":
The returned results are as follows:
<?xml version="1.0" encoding="UTF-8"?>
<ListBucketResult>
  <Name>oss-sample</Name>
  <Prefix>Directory A/</Prefix>
  <Marker></Marker>
  <MaxKeys>1000</MaxKeys>
  <Delimiter>/</Delimiter>
  <IsTruncated>false</IsTruncated>
  <Contents>
    <Key>Directory A/File C</Key>
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
    <Key>Directory A/File D</Key>
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
We can see that:
Contents returns the second-level files: "Directory A/File C" and "Directory A/File D".
CommonPrefixes returns the second-level directories: "Directory A/Directory B/", "Directory A/Directory C/", and "Directory A/Directory D/". The file names under these directories are not shown.
```

## Reference {#section_nx4_1zy_5db .section}

-   API: [GetBucket](../../../../intl.en-US/API Reference/Bucket operations/GetBucket (List Object).md#)
-   SDK: Java SDK-[List objects in a bucket](https://www.alibabacloud.com/help/doc-detail/32015.htm)

