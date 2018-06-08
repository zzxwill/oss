# Multipart commands {#concept_bvd_chc_wdb .concept}

## init {#section_w2y_rhc_wdb .section}

Command instructions:

`init oss://bucket/object`

Initiate and generate an Upload ID. The Upload ID can be used in combination with the multiupload command.

Example:

-   `python osscmd init oss://mybucket/myobject`

## listpart {#section_z2y_rhc_wdb .section}

Command instructions:

`listpart oss://bucket/object --upload_id=xxx`

Show the uploaded parts of an Upload ID in the designated object. See OSS API Reference for related concepts. The Upload ID must be designated.

Command instructions:

-   ```
python osscmd listpart oss://mybucket/myobject --upload_id=
          75835E389EA648C0B93571B6A46023F3
```


## listparts {#section_bfy_rhc_wdb .section}

Command instructions:

`listparts oss://bucket`

Show the uncompleted multipart Upload ID and objects in the bucket. When you want to delete a bucket but system prompts that the bucket is not empty, this command can be used to check whether multipart contents are contained.

Example:

-   `python osscmd listparts oss://mybucket`

## getallpartsize {#section_dfy_rhc_wdb .section}

Command instructions:

`getallpartsize oss://bucket`

Show the total size of parts of the existing Upload ID in the bucket.

Example:

-   `python osscmd getallpartsize oss://mybucket`

## cancel {#section_ffy_rhc_wdb .section}

Command instructions:

`cancel oss://bucket/object --upload_id=xxx`

Terminate the Multipart Upload event of the Upload ID.

Example:

-   ```
python osscmd cancel oss://mybucket/myobject --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```


## Multiupload \(multi\_upload, MP\) {#section_hfy_rhc_wdb .section}

Command instructions:

```
multiupload(multi_upload,mp) localfile oss://bucket/object --check_md5=false
        --thread_num=10
```

Upload local files to the OSS by multipart.

Example:

-   `python osscmd multiupload /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object`
-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object`

Command instructions:

```
multiupload(multi_upload,mp) localfile oss://bucket/object --upload_id=xxx --thread_num=10
        --max_part_num=1000 --check_md5=false
```

Upload local files to the OSS by multipart. The part count of the local file is defined in max\_part\_num. This command first judges whether the ETag of corresponding parts of the Upload ID is consistent with the MD5 value of the local file. If yes, the upload is skipped. So if an Upload ID is generated before use, it is included as a parameter. Even if the upload fails, it can be resumed by repeating the multiupload command. `--check_md5=false` indicates that when the files are being uploaded, the Content-MD5 request header does not undergo verification. True indicates that the Content-MD5 request header undergoes verification.

Example:

-   ```
python osscmd multiupload /tmp/localfile.txt oss://mybucket/object --upload_id=
          D9D278DB6F8845E9AFE797DD235DC576
```

-   ```
python osscmd multiup_load /tmp/localfile.txt oss://mybucket/object
        --thread_num=5
```

-   `python osscmd mp /tmp/localfile.txt oss://mybucket/object --max_part_num=100`**copylargefile**

Command instructions:

```
copylargefile oss://source_bucket/source_object oss://target_bucket/target_object
        --part_size=10*1024*1024 --upload_id=xxx
```

When copying a large file of over 1G, the object can be copied to the destination location through multipart \(The source bucket and the destination bucket must be in the same region\). The upload\_id is an optional parameter. If you want to resume the transmission of a multipart copy event, you can include the upload\_id. The part\_size is used to define the part size. A single part must be 100KB at minimal, and up to 10,000 parts are supported. If the set value of part\_size conflicts with the OSS limit, the application automatically adjusts the part size.

Example:

-   ```
python osscmd copylargefile oss://source_bucket/source_object
          oss://target_bucket/target_object --part_size=10*1024*1024
```


## uploadpartfromfile \(upff\) {#section_lfy_rhc_wdb .section}

Command instructions:

```
uploadpartfromfile (upff) localfile oss://bucket/object --upload_id=xxx
        --part_number=xxx
```

This command is mainly used for test and not recommended for actual use.

## uploadpartfromstring\(upfs\) {#section_mfy_rhc_wdb .section}

Command instructions:

```
uploadpartfromstring(upfs) oss://bucket/object --upload_id=xxx --part_number=xxx
        --data=xxx
```

This command is mainly used for test and not recommended for actual use.

