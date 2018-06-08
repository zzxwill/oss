# Example {#concept_h5k_vwb_wdb .concept}

## Install and configure osscmd {#section_sfx_ywb_wdb .section}

After you download SDK installer in Linux or Windows, unzip the downloaded packet to start using osscmd.

You can directly run python osscmd to get instructions for use. Every command has two modes for execution. Take querying the user-created bucket for example. The gs command \(short for “get service”\) is run.

-   Method 1: No ID or Key is specified, and osscmd reads the ID and Key from default files.

    ```
    $ python osscmd gs
    can't get accessid/accesskey, setup use : config --id=accessid --key=accesskey
    ```

    **Note:** In the case of such prompts, it indicates that the ID and Key are not properly configured. See the configuration command in Step 2.

    Once the ID and Key are properly configured and valid, run the command

    ```
    $ python osscmd gs
    2013-07-19 08:11 test-oss-sample
    Bucket Number is: 1
    ```

-   Method 2: Specify the ID and Key in the command and osscmd reads ID and Key from the command line. If the ID and Key are valid, run the command and the following result is shown.

    ```
    $ python osscmd gs --id=your_id --key=your_key --host=your_endpoint
    2013-07-19 08:11 test-oss-sample
    Bucket Number is: 1
    ```

    To configure users’ ID and Key to the default files, run the following commands. The default oss host is oss.aliyuncs.com.

    ```
    $python osscmd config --id=your_id --key=your_key --host=your_endpoint
    ```

    If you see a prompt saying “Your configuration is saved into” or similar, it indicates the ID and Key have been saved successfully.


## Basic operations {#section_bpd_nxb_wdb .section}

-   List created buckets

    ```
    $python osscmd getallbucket
    ```

    The output is empty if the OSS user didn’t create any buckets.

-   Create a bucket

    Create a bucket named mybucketname.

    ```
    $python osscmd createbucket mybucketname
    ```

    Creating a bucket named “mybucketname” may fail because the name of the bucket in OSS is globally unique and someone may have created this bucket. In this case, you must change the name. For example, you can add a specific date to the bucket name.

-   Check whether the bucket has been created successfully

    ```
    $python osscmd getallbucket
    ```

    If it fails, check the error message returned.

-   View objects

    After a bucket is successfully created, check the objects in the bucket.

    ```
    $python osscmd list oss://mybucketname/
    ```

    No objects is contained in the bucket, so the output is empty.

-   Upload an object

    Upload an object to the bucket. If the local file is named local\_existed\_file, its MD5 value is shown as follows.

    ```
    $ md5sum local_existed_file 7625e1adc3a4b129763d580ca0a78e44 local_existed_file
    $ python osscmd put local_existed_file oss://mybucketname/test_object
    ```

    **Note:** `The md5sum` command is used on Linux instead of Windows.

-   View object again

    If it is successfully created, check the object again in bucket.

    ```
    $python osscmd list oss://mybucketname/
    ```

-   Download an object

    Download an object from the bucket to local and compare the md5 value of the file downloaded.

    ```
    $ python osscmd get oss://mybucketname/test_object download_file
    $ md5sum download_file 
    7625e1adc3a4b129763d580ca0a78e44 download_file
    ```

    **Note:** The `md5sum`  command is used on Linux instead of Windows.

-   Delete an object

    $ python osscmd delete oss://mybucketname/test\_object

-   Delete a bucket

    **Note:** If a bucket contains objects, the bucket cannot be deleted.

    ```
    $ python osscmd deletebucket mybucketname
    ```


## Use lifecycle {#section_xhn_2zb_wdb .section}

-   Configure an XML text file for lifecycle

    ```
    <LifecycleConfiguration>
        <Rule>
            <ID>1125</ID>
            <Prefix>log_backup/</Prefix>
            <Status>Enabled</Status>
            <Expiration>
                <Days>2</Days>
            </Expiration>
        </Rule>
    </LifecycleConfiguration>
    ```

    This indicates deleting the objects of more than two days old to the current time and with the prefix of log\_backup/ in the bucket. For detailed rule configuration, see [API Reference](https://help.aliyun.com/document_detail/31964.html).

-   Write lifecycle

    ```
    python osscmd putlifecycle oss://mybucket lifecycle.xml
    0.150(s) elapsed
    ```

-   Read lifecycle

    ```
    python osscmd getlifecycle oss://mybucket
    <? xml version="1.0" encoding="UTF-8"? >
    <LifecycleConfiguration>
      <Rule>
        <ID>1125</ID>
        <Prefix>log_backup/</Prefix>
        <Status>Enabled</Status>
        <Expiration>
          <Days>2</Days>
        </Expiration>
      </Rule>
    </LifecycleConfiguration>
    0.027(s) elapsed
    ```

-   Delete lifecycle

    ```
    python osscmd deletelifecycle oss://mybucket
    0.139(s) elapsed
    ```

-   Read lifecyle

    ```
    python osscmd getlifecycle oss://mybucket
    Error Headers:
    [('content-length', '288'), ('server', 'AliyunOSS'), ('connection', 'close'), ('x-oss-request-id', '54C74FEE5D7F6B24E5042630'), ('date', 'Tue, 27 Jan 2015 08:44:30 GMT'), ('content-type', 'application/xml')]
    Error Body:
    <? xml version="1.0" encoding="UTF-8"? >
    <Error>
      <BucketName>mybucket</BucketName>
      <Code>NoSuchLifecycle</Code>
      <Message>No Row found in Lifecycle Table.</Message>
      <RequestId>54C74FEE5D7F6B24E5042630</RequestId>
      <HostId>mybucket.oss-maque-hz-a.alibaba.net</HostId>
    </Error>
    Error Status:
    404
    getlifecycle Failed!
    ```


## Anti-leech settings {#section_s5b_szb_wdb .section}

-   Allow access of blank referer

    ```
    $osscmd putreferer oss://test --allow_empty_referer=true
    0.004(s) elapsed
    ```

-   Get configured referer

    ```
    $osscmd getreferer oss://test
    <? xml version="1.0" encoding="UTF-8"? >
    <RefererConfiguration>
      <AllowEmptyReferer>true</AllowEmptyReferer>
      <RefererList />
    </RefererConfiguration>
    ```

-   Do not allow blank referer. Only allow test referer requests

    ```
    $osscmd putreferer oss://test --allow_empty_referer=false --referer='www.test.com'
    0.092(s) elapsed
    ```

-   Get configured referer

    ```
    $osscmd getreferer oss://test
    <? xml version="1.0" encoding="UTF-8"? >
    <RefererConfiguration>
      <AllowEmptyReferer>false</AllowEmptyReferer>
      <RefererList>
        <Referer>www.test.com</Referer>
      </RefererList>
    </RefererConfiguration>
    ```

-   Do not allow blank referer. Only allow test and test1 referer requests

    ```
    $osscmd putreferer oss://test --allow_empty_referer=false --referer='www.test.com,www.test1.com'
    ```

-   Get configured referer

    ```
    $osscmd getreferer oss://test
    <? xml version="1.0" encoding="UTF-8"? >
    <RefererConfiguration>
      <AllowEmptyReferer>false</AllowEmptyReferer>
      <RefererList>
        <Referer>www.test.com</Referer>
        <Referer>www.test1.com</Referer>
      </RefererList>
    </RefererConfiguration>
    ```


## Use logging {#section_jsg_f1c_wdb .section}

-   Set logging

    ```
    $osscmd putlogging oss://mybucket oss://myloggingbucket/mb
    ```

-   Get logging

    ```
    $osscmd getlogging oss://mybucket
    ```


