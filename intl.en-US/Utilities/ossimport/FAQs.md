# FAQs {#concept_gvd_4nh_wdb .concept}

-   1. UnsupportedClassVersionError

    Exception Executing command:

    ```
    Exception in thread "main" java.lang.UnsupportedClassVersionError: com/aliyun/ossimport2/OSSImport2 : Unsupported major.minor version 51.0
            at java.lang.ClassLoader.defineClass1(Native Method)
            at java.lang.ClassLoader.defineClassCond(ClassLoader.java:631)
            at java.lang.ClassLoader.defineClass(ClassLoader.java:615)
            at com.simontuffs.onejar.JarClassLoader.defineClass(JarClassLoader.java:693)
            at com.simontuffs.onejar.JarClassLoader.findClass(JarClassLoader.java:599)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:306)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:247)
            at com.simontuffs.onejar.Boot.run(Boot.java:300)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    ```

    Cause: the Java version is too low to be updated to 1.7 or later.

-   2. InvocationTargetException

    Submit task reporting exceptions using the submit command:

    ```
    Exception in thread "main" java.lang.reflect.InvocationTargetException
            at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
            at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.lang.reflect.Method.invoke(Method.java:497)
            at com.simontuffs.onejar.Boot.run(Boot.java:306)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    Caused by: java.lang.NullPointerException
            at com.aliyun.ossimport2.config.JobConfig.load(JobConfig.java:44)
            at com.aliyun.ossimport2. OSSImport2.doSubmitJob(OSSImport2.java:289)
            at com.aliyun.ossimport2. OSSImport2.main(OSSImport2.java:120)
            ... 6 more
    ```

    Reason: Check to see if the items in the configuration file are deleted or commented out, please fill in items that do not need to be configured after the equal sign and do not need to be deleted.

-   3. too many open files

    Reason: `ulimit -n` view system handle.

    -   If the value is less than 10 thousand, you can restart the process through `ulimit -n 65536`;
    -   If it was already set up relatively large, then use `sudo losf -n` to troubleshoot which processes have opened the handle.
-   4 Windows return seconds after Windows starts

    Cause: Most cases are caused by Java not installed or version less than 1.7, or by configuration file errors.

-   5. No jobs is running or finished

    When the submit command completes the task, use stat. View task status always displays:

    ```
    bash console.sh stat
    [WARN] List files dir not exist : /home/<user>/ossimport/workdir/master/jobs/
    no jobs is running or finished.
    ```

    Reason:

    -   The job was just submitted, and the master needs to scan the list of files first, when the task is not actually generated and distributed, printing the log is normal;
    -   After a long period of time, the error is still printed, usually without start.Command to start the process or to exit unexpectedly after the process has started. If you do not start the service, you only need to use start; otherwise, take a look `logs/ossimport.log`, find the cause of the exception and resolve it before you start the service process.
-   6. The STAT command always displays scanfinished: false

    Observe whether the total number of tasks is increasing:

    -   If there is more in the process, it is that the file list of the job is not complete, there are also new files in the list;
    -   Always unchanged, scanfinished will never be true if the job is configured with incremental Mode To scan the list of files regularly, depending on the interval configured by the user, check for new or modified documents;
    -   If it is not an incremental mode, the number of tasks does not increase, and the log is checked for exceptions.
-   7. The service process was dropped, but the log did not output the exception

    Reason: if the machine's available memory is less than 2 GB, the big probability is that there's not enough memory to be killed. Check the dmsg. Log whether there is a record of insufficient memory to be killed.

-   8. What needs to be done to restart the service after the process has been hung or killed?

    Call start directlyThe command starts the service, and the job that has been submitted does not need to be resubmitted, as long as it does not call the clean command, all submitted jobs have breakpoint records that do not redo the work that has been done.

-   9. Complete the task the OSS console displays a smaller amount of data than the source

    There is no change in the size of the bucket in the OSS console after the job has all been successfully uploaded or used locally. The size of `du` statistics varies greatly. Cause: the amount of Bucket data in the OSS console is delayed for 1 hour to update. `du` The command counts the block size, which is larger than the actual file, you can count the true size of the local directory by referring to the following command: `ls -lR <directory absolute path> | grep "\-rw" | awk '{sum+=$5}END{print sum}'`。

-   10. How do I handle the failed tasks shown by stat?

    Generally, you can use the retry  command to try again.

-   11. After some failed tasks, repeated retry won't succeed.

    Reason: view the file `$work_dir/master/jobs/$jobName/failed_tasks/$taskName/error.list` Get the relative path of the failed file, check if the file has permission to access, whether it is deleted, is flexible, whether garbled file name, etc.

-   12. How do I upload a file with a bad file name to OSS?

    Need to first use `export LANG="<your file name encode>"`, `ls`  use encode\>", ls  after checking the file name。 Command to clear the original job and resubmit the job again with the submit command.

-   13. java.nio.file.AccessDeniedException

    Exception reported: ava.nio.file.AccessDeniedException. Cause: There is no permission to access the configuration file directory.

-   14. Task status displays 0, but job display completes

    The task status displays 0, but the job display completes as follows:

    ```
    [2015-12-28 16:12:35] [INFO] JobName:dir_data
    [2015-12-28 16:12:35] [INFO] Pending Task Count:0
    [2015-12-28 16:12:35] [INFO] Dispatched Task Count:0
    [2015-12-28 16:12:35] [INFO] Succeed Task Count:0
    [2015-12-28 16:12:35] [INFO] Failed Task Count:0
    [2015-12-28 16:12:35] [INFO] Is Scan Finished:true
    [2015-12-28 16:12:35] [INFO] JobState:SUCCEED
    ```

    Reason:

    -   The `srcPrefix` fills in the error, resulting in the *List* not coming out of the file;
    -   There are only directories and no files under `srcPrefix`, because the concept of directories is simulated by OSS, will not be truly uploaded.
-   15. The bucket you are attempting to access must be addressed using the specified endpoint

    Log reporting exception:

    ```
    Exception:com.aliyun.oss.OSSException: The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.
    <Error>
      <Code>AccessDenied</Code>
      <Message>The bucket you are attempting to access must be addressed using the specified endpoint. Please send all future requests to this endpoint.</Message>
      <RequestId>56EA98DE815804**21B23EE6</RequestId>
      <HostId>my-oss-bucket.oss-cn-qingdao.aliyuncs.com</HostId>
      <Bucket>my-oss-bucket</Bucket>
      <Endpoint>oss-cn-hangzhou.aliyuncs.com</Endpoint>
    </Error>
    ```

    Reason: `srcDomain` of Bucket Or `destDomain`  fill in the error, please follow the list of domain names Fill in the correct domain name.

-   16. The request signature we calculated does not match the signature you provided

    Log reporting exception:

    ```
    Exception:com.aliyun.oss.OSSException: The request signature we calculated does not match the signature you provided. Check your key and signing method.
    [ErrorCode]: SignatureDoesNotMatch
    [RequestId]: xxxxxxx
    [HostId]: xxx.oss-cn-shanghai.aliyuncs.com
    ```

    Reason: Check whether the `destAccessKey`和`destSecretKey` and the scanner are wrong. Please refer [Access control](../../../../intl.en-US/Developer Guide/Access and control/Access control.md#).

-   17. InvocationTargetException

    submit command submit task times exception:

    ```
    submit job:/disk2/ossimport2/local_job.cfg
    Exception in thread "main" java.lang.reflect.InvocationTargetException
            at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
            at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:57)
            at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
            at java.lang.reflect.Method.invoke(Method.java:606)
            at com.simontuffs.onejar.Boot.run(Boot.java:306)
            at com.simontuffs.onejar.Boot.main(Boot.java:159)
    Caused by: java.lang.NullPointerException
            at com.aliyun.ossimport2. OSSImport2.doSubmitJob(OSSImport2.java:289)
            at com.aliyun.ossimport2. OSSImport2.main(OSSImport2.java:120)
            ... 6 more
    ```

    Reason: Check Configuration item workingdir in `conf/sys.properties`  Whether to configure, configure correctly, and verify that the configuration file path is the correct path.

-   18. Do you support setting up agents?

    This feature is not supported. 

-   19. Why is it expensive for OSS to migrate to OSS?

    Refer to  [endpoint](../../../../intl.en-US/Developer Guide/Access and control/Endpoints.md#) The domain name in the help, After configuring the internal network domain name, will not charge the traffic fee, but the cost of the number of visits is still charging.

-   20. The synchronization process shows that the source file does not exist

    Reason: The Master first lists the list of files, and then moves the data according to the list of files. When list When you finish, certain files on the source end are deleted, you will find that the source file does not exist. This type of file is skipped and output to the error list.

-   21. Turn on incremental mode, will the OSS be deleted after locally deleted?

    Turns on incremental mode, if the OSSS is deleted after local deletion, the delete operation is not synchronized.

-   22. Turn on incremental mode, some new documents are not synchronized

    The incremental mode uses the last modification of the contrast file to determine whether the file is incremental, the Linux mV, CP, MV, Windows Rsync Operations such as the `-t` or `-a` parameters do not modify the last modification time of the file, files modified by these operations are not scanned.

-   23. The number of tasks shooting the migration has always shown 0

    Reasons: again, the more complex, mainly divided into two situations:

    -   ```
[2016-07-21 10:21:46] [INFO] [name=YoupaiList, totalRequest=1729925, avgLatency=38,
          recentLatency=300000]
```

        This log, if the `recentLatency=30000`, is generally normal. List, beat list is slow, usually run up to 30 seconds of timeout, 30 seconds to list out a few files to return a few files, such as the case slowly list tasks It is normal to come out;

    -   The `recentLatency` is very small, and the general case is that the account password is wrong, and so on, because another error in the SDK returns only null \), Does not return the error result, so you can only get another error code that is returned by catching the package.
-   24. What do `srcAccessKey`和`srcSecretKey` and fig fill in again during the migration

    Fill in the operator's account number and password. .

-   25. HTTP is always displayed during another shot migration Error 429

    Also shot to limit the SDK access interval, if the access is a little faster, it will limit the speed, please contact us again for Customer Service Release restrictions. Ossimport itself will try this situation again.

-   26. The execution of Unknown command "Java", Unknown command "nohup" and so on.

    Reason: The command used is not installed, please use yum or `apt-get` or `zypper` Wait for the command to install the corresponding command.

-   27. Task does not match configuration file

    The job configuration file appears to be correct, but running looks pretty different from the job profile configuration. Only `sys.properties` properties Changes and then reboots to take effect, and once the job's configuration file is submitted, the modification does not take effect and is required Clean drops the original job, and then resubmits the new configuration file.

-   28. The bucket name “xxx/xx” is invalid

    Log reporting exception:

    ```
    java.lang.IllegalArgumentException: The bucket name "xxx/xx" is invalid. A bucket name must: 1) be comprised of lower-case characters, numbers or dash(-); 2) start with lower case or numbers; 3) be between 3-63 characters long.
    ```

    Reason: check if the `destBucket` configuration item \(s\) are filling correctly, and the bucket is not carrying ***/*** and other paths.

-   29. com.aliyun.oss.ClientException: Unknown

    Log reporting exception:

    ```
    com.aliyun.oss.ClientException: Unknown
    [ErrorCode]: NonRepeatableRequest
    [RequestId]: Cannot retry request with a non-repeatable request entity.  The cause lists the reason the original request failed.
    ```

    As well as, usually when the network is full, ossimport will try again, if you still fail after retrying, you can call after the task is complete The retry command retries again.

-   30. Connect to xxx.oss-cn-beijing-internal.aliyuncs.com:80 timed out

    Log reporting exception:

    ```
    Unable to execute HTTP request: Connect to xxx.oss-cn-beijing-internal.aliyuncs.com:80 timed out
    [ErrorCode]: ConnectionTimeout
    [RequestId]: Unknown
    ```

    Reason: Non-ECS machines cannot use the internl domain name.

-   31. The specified bucket is not valid

    Log reporting exception:

    ```
    com.aliyun.oss.OSSException: The specified bucket is not valid.
    [ErrorCode]: InvalidBucketName
    [RequestId]: 57906B4DD0EBAB0FF553D661
    [HostId]: you-bucket.you-bucketoss-cn-hangzhou-internal.aliyuncs.com
    ```

    Reason: From the configuration file The `destDomian` configured domain name cannot have a bucket name.

-   32. Can the srcPrefix in the configuration file specify a file individually?

    No,`srcPrefix`only supports directories or prefix levels, A single file upload can be done with other, simpler tools.

-   33. Unable to execute HTTP request: The Difference between … is too large.

    Log reporting exception:

    ```
    Unable to execute HTTP request: The Difference between the request time and the current time is too large.
    [ErrorCode]: RequestTimeTooSkewed
    [RequestId]: xxxxxxx
    ```

    Reason:

    -   The Local Machine Time is not good, with a difference of more than 15 minutes from the server time, which is mostly the case.
    -   It may be that the concurrency is too high, especially for high CPU usage, leading to slow upload during concurrency.
-   34. No route to host

    An error is shown in the logs: `No route to host`. This is probably caused by network interruptions due to a local firewall or **iptables**.

-   35. Unknown http list file format

    The error is displayed using the http mode log because the specified HTTP list file is not in the right format:

    -   One reason is that the files may be copied from another system. You can use the `mac2unix` or `doc2unix` command to convert the file formats.
    -   There are some rows in the file that do not meet the rules, such as a row with fewer than two columns.
-   36. The boject key “/xxxxx.jpg” is invalid

    Log reporting exception:

    ```
    Exception:java.lang.IllegalArgumentException: The boject key "/xxxxx.jpg" is invalid. An object name should be between 1 - 1023 bytes long when encoded as UTF-8 and cannot contain LF or CR os unsupported chars in XML1.0, and cannot begin with "/" or "\".
    ```

    Reason:

    -   Checks whether the `srcPrefix` is as a directory but does not end in;
    -   Check that the  `destPrefix`  starts with/or.

