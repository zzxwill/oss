# Object commands {#concept_w2w_l2c_wdb .concept}

## ls\(list\) {#section_j35_z2c_wdb .section}

Command instructions:

`ls(list) oss://bucket/[prefix] [marker] [delimiter] [maxkeys]`

List object in the bucket.

Example:

-   `python osscmd ls oss://mybucket/folder1/folder2`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1 /`
-   `python osscmd ls oss://mybucket/`
-   `python osscmd list oss://mybucket/ "" "" 100`

Command instructions:

`ls(list) oss://bucket/[prefix] --marker=xxx --delimiter=xxx --maxkeys=xxx`

List object in the bucket. Where name\_type specifies the encoding used in the transmission. when the URL encoding is specified, the display of objects with control characters is supported.

Example:

-   `python osscmd ls oss://mybucket/folder1/folder2 --delimiter=/`
-   `python osscmd ls oss://mybucket/folder1/folder2 --marker=a`
-   `python osscmd ls oss://mybucket/folder1/folder2 --maxkeys=10`

## mkdir {#section_pt5_wgc_wdb .section}

Command instructions:

`mkdir oss://bucket/dirname`

Create an object that ends with '/' and has a size of 0.

Example:

-   `python osscmd mkdir oss://mybucket/folder`

## listallobject {#section_rt5_wgc_wdb .section}

Command instructions:

`listallobject oss://bucket/[prefix]`

Show all objects in the bucket, and the prefix can be specified.

Example:

-   `python osscmd listallobject oss://mybucket`
-   `python osscmd listallobject oss://mybucket/testfolder/`

## deleteallobject {#section_tt5_wgc_wdb .section}

Command instructions:

`deleteallobject oss://bucket/[prefix]`

Delete all objects in the bucket, and the prefix can be specified.

Example:

-   `python osscmd deleteallobject oss://mybucket`
-   `python osscmd deleteallobject oss://mybucket/testfolder/`

## downloadallobject {#section_vt5_wgc_wdb .section}

Command instructions:

`downloadallobject oss://bucket/[prefix] localdir --replace=false --thread_num=5`

Download the objects in the bucket to a local directory, with the directory structure unchanged. The prefix can be specified for downloading. —replace=false indicates that if a local file already exists with the same name, it is not replaced during the download.  —replace=true indicates that the local file with the same name is replaced. You can also configure the download thread with thread\_num.

Example:

-   `python osscmd downloadallobject oss://mybucket /tmp/folder`
-   `python osscmd downloadallobject oss://mybucket /tmp/folder –-replace=false`
-   `python osscmd downloadallobject oss://mybucket /tmp/folder –-replace=true --thread_num=5`

## downloadtodir {#section_xt5_wgc_wdb .section}

Command instructions:

`downloadtodir oss://bucket/[prefix] localdir --replace=false`

Download the objects in the bucket to a local directory, with the directory structure unchanged. The prefix can be specified for downloading. —replace=false indicates that if a local file already exists with the same name, it is not replaced during the download. —replace=true indicates that the local file with the same name is replaced. It achieves the same effect with the downloadallobject.

Example:

-   `python osscmd downloadtodir oss://mybucket /tmp/folder`
-   `python osscmd downloadtodir oss://mybucket /tmp/folder –-replace=false`
-   `python osscmd downloadtodir oss://mybucket /tmp/folder –-replace=true`

## uploadfromdir {#section_zt5_wgc_wdb .section}

Command instructions:

`uploadfromdir localdir oss://bucket/[prefix] --check_point=check_point_file --replace=false --check_md5=false --thread_num=5`

Upload local files into the bucket. For example, the localdir is `/tmp/`.

If the following three files need to be uploaded: `a/b`, `a/c`, and `a`, they become `oss://bucket/a/b`, `oss://bucket/a/c`, and `oss://bucket/a` after being uploaded into the OSS. If the prefix is specified as mytest, the uploaded files to OSS become `oss://bucket/mytest/a/b`, `oss://bucket/mytest/a/c`, and `oss://bucket/mytest/a`.

`--check_point=check_point_file` is the specified file. After the files are specified, osscmd puts the uploaded local files into check\_point\_file as time stamps, and the uploadfromdir command compares the time stamps of the files being uploaded with that recorded in check\_point\_file. If the files change, they are re-uploaded. Otherwise the file is skipped. The check\_point\_file does not exist by default. `--replace=false` indicates that if a local file already exists with the same name, it is not replaced during the download. —replace=true indicates that the local file with the same name is replaced. `--check_md5=false` indicates that when the files are being uploaded, the Content-MD5 request header does not undergo verification. True indicates that the Content-MD5 request header undergoes verification.

**Note:** The logs in the check\_point\_file involve all the uploaded files. When too many files are uploaded, the check\_point\_file is sizable.

Example:

-   `python osscmd uploadfromdir /mytemp/folder oss://mybucket`
-   `python osscmd uploadfromdir /mytemp/folder oss://mybucket --check_point_file=/tmp/mytemp_record.txt`
-   `python osscmd uploadfromdir C:\Documents and Settings\User\My Documents\Downloads oss://mybucket --check_point_file=C:\cp.txt`

## put {#section_b55_wgc_wdb .section}

Command instructions:

`put localfile oss://bucket/object --content-type=[content_type] --headers="key1:value1#key2:value2" --check_md5=false`

When uploading a local file into the bucket, you can specify the object content-type, or specify customized headers. `--check_md5=false` indicates that when the files are being uploaded, the Content-MD5 request header does not undergo verification. True indicates that the Content-MD5 request header undergoes verification.

Example:

-   `python osscmd put myfile.txt oss://mybucket`
-   `python osscmd put myfile.txt oss://mybucket/myobject.txt`
-   `python osscmd put myfile.txt oss://mybucket/test.txt --content-type=plain/text --headers=“x-oss-meta-des:test#x-oss-meta-location:CN”`
-   `python osscmd put myfile.txt oss://mybucket/test.txt --content-type=plain/text`

## Upload {#section_d55_wgc_wdb .section}

Command instructions:

`upload localfile oss://bucket/object --content-type=[content_type] --check_md5=false`

Upload local files in object group. Not recommended. `--check_md5=false` indicates that when the files are being uploaded, the Content-MD5 request header does not undergo verification. True indicates that the Content-MD5 request header undergoes verification.

Example:

-   `python osscmd upload myfile.txt oss://mybucket/test.txt --content-type=plain/text`

## get {#section_f55_wgc_wdb .section}

Command Instructions:

`get oss://bucket/object localfile`

Download the object to local.

Example:

-   `python osscmd get oss://mybucket/myobject /tmp/localfile`

## multiget\(multi\_get\) {#section_h55_wgc_wdb .section}

Command instructions:

`multiget(multi_get) oss://bucket/object localfile --thread_num=5`

Download the object to local in multithreading. The thread count can be configured.

Example:

-   `python osscmd multiget oss://mybucket/myobject /tmp/localfile`
-   `python osscmd multi_get oss://mybucket/myobject /tmp/localfile`

## cat {#section_j55_wgc_wdb .section}

Command instructions:

`cat oss://bucket/object`

Read the meta information of the object and print it out. Do not use it when the object size is large.

Example:

-   `python osscmd cat oss://mybucket/myobject`

## meta {#section_l55_wgc_wdb .section}

Command instructions:

`meta oss://bucket/object`

Read the meta information of the object and print it out. The meta information includes the content-type, file length, custom meta, etc.

Example:

-   `python osscmd meta oss://mybucket/myobject`

## copy {#section_n55_wgc_wdb .section}

Command instructions:

`copy oss://source_bucket/source_object oss://target_bucket/target_object --headers="key1:value1#key2:value2"`

Copy the source object of the source bucket to the destination object in the destination bucket.

Example:

-   `python osscmd copy oss://bucket1/object1 oss://bucket2/object2`

## rm\(delete,del\) {#section_p55_wgc_wdb .section}

Command instructions:

`rm(delete,del) oss://bucket/object --encoding_type=url`

Delete object. When the encoding-type is URL encoding, the strings to be deleted also need to be URL encoding.

Example:

-   `python osscmd rm oss://mybucket/myobject`
-   `python osscmd delete oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/my%01object --encoding_type=url`

## signurl\(sign\) {#section_r55_wgc_wdb .section}

Command instructions:

`signurl(sign) oss://bucket/object --timeout=[timeout_seconds]`

Generate a URL containing the signature and specify the timeout value. This is applicable to the scenario where the private bucket provides the specified object for others’ accesses.

Example:

-   `python osscmd sign oss://mybucket/myobject`
-   `python osscmd signurl oss://mybucket/myobject`

