# Bucket-related commands {#concept_rjp_vgs_vdb .concept}

This topic describes how to use the Alibaba Cloud OSS open-source tool ossutil to run bucket-related commands. Specifically, you create, delete, or list a bucket, or change the ACL of a bucket. You can also use this tool to manage bucket-related items such as objects, uncompleted multipart upload tasks, to manage Cross-Origin Resource Sharing \(CORS\) rules, log rules, or anti-leech rules, or to troubleshoot the OSS network.

**Note:** 

-   Before you can run bucket-related commands, you must first upgrade your ossutil to the latest version and run the config command to configure your AccessKey. For more information, see [Quick start](reseller.en-US/Tools/ossutil/Quick start.md#).
-   Bucket management functions besides those described here are not supported by ossutil. If you require such functions, use the osscmd tool. For more information, see [Quick installation](reseller.en-US/Tools/osscmd/Quick installation.md#).

## Create a bucket {#section_qk1_v2l_xgb .section}

-   Create a bucket

    ```
    ./ossutil mb oss://bucketname [--acl=ACL][--storage-class sc][-c file]
    ```

    If the bucket is created, ossutil prints the interval of time needed to create the bucket and exits. If the bucket failed to be created, ossutil outputs the corresponding error information.

    **Note:** For information about how to use the mb command, run the ossutil help mb command.

-   Create a bucket and set its ACL

    You can use the `--acl` parameter to set the ACL for a bucket. The default ACL is private. The following are available ACLs:

    -   private: Anonymous users are not allowed to to read from or write to objects in the bucket. A signature is required for access.
    -   public-read: Anonymous users are allowed only to read from objects in the bucket.
    -   public-read-write: Anonymous users are allowed to read from and write to objects in the bucket.
    **Note:** For more information on access control, see [Access control based on ACLs](../../../../reseller.en-US/Developer Guide/Access and control/Access control based on ACLs.md#).

    For example, run the following command to create a bucket and set its ACL to public-read-write:

    ```
    ./ossutil mb oss://bucket --acl=public-read-write
    ```

-   Create a bucket and set its storage class

    You can use the `--storage-class` parameter to set the storage class of a bucket. The default storage class is Standard. The following are available storage classes:

    -   Standard
    -   Infrequent Access
    -   Archive
    **Note:** For more information on storage classes, see [Overview](../../../../reseller.en-US/Developer Guide/Storage classes/Overview.md#).

    For example, run the following command to create a bucket and set its storage class to Infrequent Access:

    ```
    ./ossutil mb oss://bucket --storage-class IA
    ```


## Change the ACL for a bucket {#section_pt4_qkl_xgb .section}

You can run the set-acl command to change the ACL for a bucket. In this command, you must set the `-b` parameter.

For example, run the following command to change the ACL for a bucket to private:

```
./ossutil set-acl oss://bucket1 private -b
			
```

**Note:** For information about how to use the set-acl command, run the ossutil help set-acl command.

## Delete a bucket {#section_wmp_z2l_xgb .section}

-   Delete an empty bucket

    ```
    ./ossutil rm oss://bucket -b
    ```

    **Note:** 

    -   You must set the `-b` parameter when you delete a bucket.
    -   The bucket you delete may be re-created by another user. However, in such case, you will no longer own this bucket.
    -   For information about how to use the rm command, run the ossutil help rm command.
-   Clear and delete a bucket

    If a bucket contains object or multipart data, you must first delete the object or multipart data before you delete the bucket.

    ```
    ./ossutil rm oss://bucket -bar
    ```

    **Warning:** If you run the preceding command, all the data in your bucket is deleted.


## List buckets {#section_gjb_g3l_xgb .section}

-   List all your buckets

    You can run one of the following two commands to list all your buckets:

    -   ```
./ossutil ls
```

    -   ```
./ossutil ls oss://
```

    **Note:** The `-s` parameter is used to list your buckets in a simple structure. For information about how to use the ls command, run the ossutil help ls command.

    Example:

    ```
    ./ossutil ls
    CreationTime                                 Region    StorageClass    BucketName
    2016-10-2116:18:37 +0800 CST       oss-cn-hangzhou         Archive    oss://go-sdk-test-bucket-xyz-for-object
    2016-12-0115:06:21 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutil-test
    2016-07-1817:54:49 +0800 CST       oss-cn-hangzhou        Standard    oss://ossutilconfig
    2016-07-2010:36:24 +0800 CST       oss-cn-hangzhou              IA    oss://ossutilupdate
    2016-11-1413:08:36 +0800 CST       oss-cn-hangzhou              IA    oss://yyyyy
    2016-08-2509:06:10 +0800 CST       oss-cn-hangzhou         Archive    oss://ztzt
    2016-11-2121:18:39 +0800 CST       oss-cn-hangzhou         Archive    oss://ztztzt
    Bucket Number is:7
    0.252174(s) elapsed
    						
    ```

-   List your buckets by page

    ```
    ./ossutil ls oss:// --limited-num=${num} --marker=${bucketname}
    ```

    If you created a large number of buckets, you can use the `--limited-num` and `--marker` parameters to list your buckets by page.

    -   `--limited-num`: the number of buckets displayed on each page.
    -   `--marker`: the name of the bucket from which ossutil starts to list your buckets. Your buckets are sorted alphabetically and displayed by page. In most cases, ossutil lists your buckets starting from the bucket queried and displayed on the previous page.
    For example, run the following command to list the first two buckets by page:

    ```
    ./ossutil ls oss:// --limited-num=1 -s 
    oss://bucket1
    Bucket Number is:1
    0.303869(s) elapsed
    $ ./ossutil ls oss:// --limited-num=1 -s --marker=bucket1
    oss://bucket2
    Bucket Number is:1
    0.257636(s) elapsed
    ```


## List objects {#section_vqm_qkl_xgb .section}

-   List all the objects in a bucket

    ```
    ./ossutil ls oss://bucketname
    ```

    Example:

    ```
    ./ossutil ls oss://ossutil-test
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a1
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a2
    2016-12-0115:06:45 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a3
    Object Number is:3
    0.007379(s) elapsed
    							
    ```

-   List all your objects and uncompleted multipart upload tasks

    ```
    ./ossutil ls oss://bucket -a
    ```

    Example:

    ```
    ./ossutil ls oss://bucket1 -a 
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2015-06-0514:06:29 +0000 CST        201933      Standard   7E2F4A7F1AC9D2F0996E8332D5EA5B41        oss://bucket1/dir1/obj11
    2015-06-0514:36:21 +0000 CST        201933      Standard   6185CA2E8EB8510A61B3A845EAFE4174        oss://bucket1/obj1
    2016-04-0814:50:47 +0000 CST       6476984      Standard   4F16FDAE7AC404CEC8B727FCC67779D6        oss://bucket1/sample.txt
    Object Number is:3
    InitiatedTime                     UploadID                           ObjectName
    2017-01-1303:45:26 +0000 CST     15754AF7980C4DFB8193F190837520BB    oss://bucket1/obj1
    2017-01-1303:43:13 +0000 CST     2A1F9B4A95E341BD9285CC42BB950EE0    oss://bucket1/obj1
    2017-01-1303:45:25 +0000 CST     3998971ACAF94AD9AC48EAC1988BE863    oss://bucket1/obj2
    2017-01-2011:16:21 +0800 CST     A20157A7B2FEC4670626DAE0F4C0073C    oss://bucket1/tobj
    UploadId Number is:4
    0.191289(s) elapsed
    							
    ```

-   List all your objects by page

    ```
    ./ossutil ls oss://bucket --limited-num=${num} --marker=${obj}
    ```

    You can use the `--limited-num` and `--marker` parameters to list your objects by page. This is similar to listing your buckets by page in [Bucket-related commands](reseller.en-US/Tools/ossutil/Bucket-related commands.md#section_gjb_g3l_xgb).

    ```
    ./ossutil ls oss://ossutil-test --limited-num=1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:37 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a1
    Object Number is:1
    0.007379(s) elapsed
    $./ossutil ls oss://ossutil-test --limited-num=1 --marker=a1
    LastModifiedTime                    Size(B)  StorageClass   ETAG                                    ObjectName
    2016-12-0115:06:42 +0800 CST      10363812      Standard   61DE142E5AFF9A6748707D4A77BFBCFB        oss://ossutil-test/a2
    Object Number is:1
    0.008392(s) elapsed
    							
    ```

-   List your objects in a simple structure

    ```
    ./ossutil ls oss://bucket -s
    ```

    Example:

    ```
    ./ossutil ls oss://ossutil-test
    oss://ossutil-test/a1
    oss://ossutil-test/a2
    oss://ossutil-test/a3
    Object Number is:3
    0.007379(s) elapsed  
    ```

-   List your objects in a simulated directory structure

    ```
    ./ossutil ls oss://bucket -d
    ```

    If you do not want to list all the objects recursively in the subdirectories of your current directory, you can use the `-d` parameter to list the objects and subdirectories.

    Example:

    ```
    ./ossutil ls oss://bucket1 -s -d
    oss://bucket1/obj1
    oss://bucket1/sample.txt
    oss://bucket1/dir1/
    Object and Directory Number is:3 
    0.119884(s) elapsed
    						
    ```

-   List all the objects in a bucket to which a domain name is attached

    For more information, see [Object-related commands](reseller.en-US/Tools/ossutil/Object-related commands.md#).


## List uncompleted multipart upload tasks and relevant information {#section_pmq_tlt_xgb .section}

-   List your uncompleted multipart upload tasks

    ```
    ./ossutil ls oss://bucket -m
    ```

    You can use the `-m` parameter to list the uncompleted multipart upload tasks in your current bucket.

    Example:

    ``` {#codeblock_nrk_g7i_poy}
    ./ossutil ls oss://bucket1 -m 
    InitiatedTime                  UploadID                          ObjectName
    2017-01-1303:45:26 +0000 CST  15754AF7980C4DFB8193F190837520BB  oss://bucket1/obj1
    2017-01-1303:45:25 +0000 CST  3998971ACAF94AD9AC48EAC1988BE863  oss://bucket1/obj2
    2017-01-2011:16:21 +0800 CST  A20157A7B2FEC4670626DAE0F4C0073C  oss://bucket1/tobj
    UploadID Number is:30.009424(s) elapsed                            
    ```

-   List the parts to be uploaded for all your objects

    ```
    ./ossutil getallpartsize oss://bucket
    ```

-   List the parts to be uploaded for a specified object

    ```
    ./ossutil listpart oss://bucket/object uploadid
    ```

    The `uploadid` parameter specifies the upload task ID of an object whose parts are to be uploaded.


**Note:** For more information about multipart upload, see [Multipart-related commands](reseller.en-US/Tools/ossutil/Multipart-related commands.md#).

## Manage Cross-Origin Resource Sharing rules {#section_edx_l2n_hhb .section}

You can set the `method` parameter in the cors command to PUT, GET, or DELETE to add, change, query, or delete the CORS rule of a bucket. For more information, see [Set CORS rules](../../../../reseller.en-US/Developer Guide/Buckets/Set CORS rules.md#).

**Note:** For information about how to use the cors command, run the ossutil help cors command.

-   Add or change the CORS rule of a bucket

    ```
    ./ossutil cors --method put oss://bucket  local_xml_file
    ```

    Ossutil reads CORS rules from the local\_xml\_file configuration file. If no CORS rule is set for your bucket, ossutil adds the corresponding CORS rule obtained from the configuration file to your bucket. If a CORS rule is set for your bucket, ossutil changes this CORS rule to the CORS rule that is obtained from the configuration file.

    **Note:** The local\_xml\_file configuration file is in XML format as follows:

    ```
    <?xml version="1.0" encoding="UTF-8"?>
       <CORSConfiguration>
         <CORSRule>
             <AllowedOrigin>www.aliyun.com</AllowedOrigin>
             <AllowedMethod>PUT</AllowedMethod>
             <MaxAgeSeconds>10000</MaxAgeSeconds>
         </CORSRule>
     </CORSConfiguration>
    ```

-   Obtain the CORS rule of a bucket

    ```
    ./ossutil cors --method get oss://bucket  [local_xml_file]
    ```

    If the local\_xml\_file parameter is set, ossutil saves the obtained CORS rule to the local\_xml\_file configuration file on your computer. If this parameter is null, ossutil displays the obtained CORS rule on your screen.

-   Delete the CORS rule of a bucket

    ```
    ./ossutil cors --method delete oss://bucket
    ```


## Manage log rules {#section_rzd_3yn_hhb .section}

You can set the `method` parameter in the logging command to PUT, GET, or DELETE to add, change, query, or delete the log rule of a bucket. For more information, see [Set access logging](../../../../reseller.en-US/Developer Guide/Manage logs/访问日志存储.md#).

**Note:** For information about how to use the cors command, run the ossutil help logging command.

-   Add or change the log rule of a bucket

    ```
    ./ossutil logging --method put oss://bucket  oss://target-bucket/[prefix]
    ```

    If log management is disabled, run this command to save your bucket access logs as objects to the bucket specified by the target-bucket parameter. However, if log management is enabled, you can run this command to change the directory for storing your bucket access logs.

    The prefix parameter specifies the directory and prefix for storing your bucket access logs. If this parameter is set, ossutil saves your bucket access logs to the specified directory in the bucket specified by the target-bucket parameter. If this parameter is null, ossutil saves your bucket access logs to the root directory in the bucket specified by the target-bucket parameter. For log object naming conventions, see [Set logging](../../../../reseller.en-US/Console User Guide/Manage logs/Set logging.md#section_n4b_q3z_5db).

-   Obtain the log rule of a bucket

    ```
    ./ossutil logging --method get oss://bucket  [local_xml_file]
    ```

    If the local\_xml\_file parameter is set, ossutil saves the obtained log rule to the local\_xml\_file configuration file on your computer. If this parameter is null, ossutil displays the obtained log rule on your screen.

-   Delete the log rule of a bucket

    ```
    ./ossutil logging --method delete oss://bucket
    ```


## Manage anti-leech rules {#section_zyj_dqg_jhb .section}

You can set the `method` parameter in the referer command to PUT, GET, or DELETE to add, change, query, or delete the anti-leech rule of a bucket. For more information, see [Configure hotlinking protection](../../../../reseller.en-US/Developer Guide/Buckets/Configure hotlinking protection.md#).

**Note:** For information about how to use the referer command, run the ossutil help referer command.

-   Add or change the anti-leech rule of a bucket

    ```
    ./ossutil referer --method put oss://bucket referer-value [--disable-empty-referer]
    ```

    If no anti-leech rule is set for the bucket, ossutil adds the specified anti-leech rule. If an anti-leech rule already exists, ossutil changes this anti-leech rule to the specified anti-leech rule.

    -   `referer-value`: enables and specifies the Referer whitelist that includes a list of domain names separated by spaces. The whitelist can contain wildcard characters \(\*\) and question marks \(?\). Only the OSS access requests from the domains included in the whitelist are permitted.
    -   `--disable-empty-referer`: specifies whether the Referer field can be left unspecified. If the `--disable-empty-referer` parameter is used in the referer command, the Referer field cannot be left unspecified and only the OSS access requests whose HTTP or HTTPS headers contain this field are permitted. If the `--disable-empty-referer` parameter is not used, the Referer field can be left unspecified.
    For example, run the following command to set the anti-leech rule for a bucket while disallowing the Referer field to be left unspecified:

    ```
    ./ossutil referer --method put oss://ossutil-test www.test1.com www.test2.com --disable-empty-referer
    ```

-   Obtain the anti-leech rule of a bucket

    ```
    ./ossutil referer --method get oss://bucket  [local_xml_file]
    ```

    If the local\_xml\_file parameter is set, ossutil saves the obtained anti-leech rule to the local\_xml\_file configuration file on your computer. If this parameter is null, ossutil displays the obtained anti-leech rule on your screen.

-   Delete the anti-leech rule of a bucket

    ```
    ./ossutil referer --method delete oss://bucket
    ```


## Troubleshoot OSS network {#section_njd_yzz_zgb .section}

After you run the probe command, ossutil prompts you with possible causes to upload and download failures. This may include OSS network faults or inappropriate settings to basic parameters.

**Note:** For information about how to use the probe command, run the ossutil help probe command.

-   Download an object from a bucket by using the object URL and output a troubleshooting report

    ```
    ./ossutil probe --download --url http_url [--addr=domain_name] [file_name]
    ```

    After downloading an object from a bucket to your computer by using the object URL, you can test your network transmission quality and output a troubleshooting report.

    -   `--url`: the URL of an object in the bucket.
        -   If the ACL of the object is public-read, the URL does not carry a signature, for example, `https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg`.
        -   If the ACL of the object is private, the URL carries a signature and starts and ends with a double quotation mark \("\), for example, `"https://bucketname.oss-cn-beijing.aliyuncs.com/myphoto.jpg?Expires=1552015472&OSSAccessKeyId=TMP.xxxxxxxx5r9f1FV12y8_Qis6LUVmvoSCUSs7aboCCHtydQ0axN32Sn-UvyY3AAAwLAIUarYNLcO87AKMEcE5O3AxxxxxxoCFAQuRdZYyVFyqOW8QkGAN-bamUiQ&Signature=bIa4llbMbldrl7rwckr%2FXXvTtxw%3D"`.
    -   `--addr=domain_name`: the domain or IP address to which the ping command is initiated while the object is being downloaded. This parameter is optional. If you do not use this parameter, ossutil does not probe another domain or IP address.
        -   If you use the default parameter value, ossutil runs the ping command to check whether communications between your OSS network and `www.aliyun.com` are normal.
        -   If you specify a domain name or IP address, ossutil runs the ping command to check whether communications between your OSS network and the domain or IP address are normal.
    -   `file_name`: the directory for storing the downloaded object. This parameter is optional. If you do not use this parameter, ossutil saves the downloaded object to the current directory and determines the object name. If you use this parameter to specify an object or directory name, ossutil names the downloaded object by using the specified object name or saves the downloaded object to the specified directory.
-   Download an object from a bucket and output a troubleshooting report

    ```
    ./ossutil probe --download --bucketname bucket-name [--object=object_name]
    [--addr=domain_name] [file_name]
    ```

    -   `--bucketname`: the name of the bucket from which the object is downloaded.
    -   `--object=`: the directory where the downloaded object is stored. This parameter is optional. If you do not use this parameter, ossutil generates a temporary object, uploads it to the bucket specified by the `bucket-name` parameter, and then downloads this object. After this object is downloaded, ossutil deletes it from your local computer and bucket.
-   Check the upload result and output a troubleshooting report

    ```
    ./ossutil probe --upload [file_name] --bucketname bucket-name [--object=obj
    ect_name] [--addr=domain_name] [--upmode]
    ```

    -   `file_name`: the name of the object that you want to upload to the bucket specified by the `bucket-name` parameter. The `file_name` parameter is optional. If you do not use this parameter, ossutil generates a temporary object and uploads it to the specified bucket. After the probing is completed, ossutil deletes this temporary object.
    -   `--object=`: the name of an object or directory. This parameter is optional. An example parameter value is path/myphoto.jpg, which specifies the object name after the object is uploaded. If you do not use this parameter, ossutil generates a name for the uploaded object. After the probing is completed, ossutil deletes this object.
    -   `--upmode`: the upload method. This parameter is optional. The default parameter value is normal. The following are available upload methods:
        -   normal
        -   append
        -   multipart

-   Obtain a troubleshooting report

    After running the probe command, you can view each task execution step and the overall upload or download result.

    -   If a multiplication sign \(×\) appears following a step, then this step failed. If a multiplication sign \(×\) does not appear, this step succeeded.
    -   If the upload or download succeeded, ossutil outputs the object size and the time at which the object was uploaded or downloaded. If the upload or download failed, ossutil outputs the failure cause or troubleshooting advice.

        **Note:** Ossutil may not output troubleshooting advice for some errors. In this case, you can troubleshoot the problems based on the error codes by following the instructions provided in [Exception handling](../../../../reseller.en-US/SDK Reference/Java/Exception handling.md#section_gn4_55c_bhb).

    After running the probe command, ossutil generates an object whose name starts with probe in your current directory. This object contains details about the commands that you have run to troubleshoot problems.


