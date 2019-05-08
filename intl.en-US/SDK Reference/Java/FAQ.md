# FAQ {#concept_32024_zh .concept}

## Jar conflicts {#section_u5d_qgd_kfb .section}

-   Cause analysis

    If the following errors occur when you use OSS Java SDK, jar conflicts may exist in your project.

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/ssl/TrustStrategy
        at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
        at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
        at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:77)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.ssl.TrustStrategy
        at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
        at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
        at java.security.AccessController.doPrivileged(Native Method)
        at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
        ... 3 more
    					
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoSuchFieldError: INSTANCE
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<init>(DefaultHttpRequestWriterFactory.java:52)
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<init>(DefaultHttpRequestWriterFactory.java:56)
     at org.apache.http.impl.io.DefaultHttpRequestWriterFactory.<clinit>(DefaultHttpRequestWriterFactory.java:46)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:82)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:95)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<init>(ManagedHttpClientConnectionFactory.java:104)
     at org.apache.http.impl.conn.ManagedHttpClientConnectionFactory.<clinit>(ManagedHttpClientConnectionFactory.java:62)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager$InternalConnectionFactory.<init>(PoolingHttpClientConnectionManager.java:572)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:174)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:158)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:149)
     at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.<init>(PoolingHttpClientConnectionManager.java:125)
     at com.aliyun.oss.common.comm.DefaultServiceClient.createHttpClientConnectionManager(DefaultServiceClient.java:237)
     at com.aliyun.oss.common.comm.DefaultServiceClient.<init>(DefaultServiceClient.java:78)
     at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
     at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
     at OSSManagerImpl.upload(OSSManagerImpl.java:42)
     at OSSManagerImpl.main(OSSManagerImpl.java:63)
    					
    ```

    The cause is that OSS Java SDK uses Apache httpclient 4.4.1, while your project uses Apache httpclient or commons-httpclient jar that conflicts with Apache httpclient 4.4.1. To view the jar file and its version used for a project, open OSS Java SDK and run the `mvn dependency:tree` command. The following figure shows that a project that uses Apache httpclient 4.3.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22308/155728411413397_en-US.png)

-   There are two solutions to jar conflicts.

    -   Use a unified version. If your project uses the version that conflicts with Apache httpclient 4.4.1, use Apache httpclient 4.4.1 and delete Apache httpclient dependencies of other versions in the pom.xml file. If your project uses commons-httpclient, jar conflicts may also exist. In this case, delete commons-httpclient.
    -   Resolve dependency conflicts. If your project uses multiple third-party dependencies that have various Apache httpclient versions, dependency conflicts may exist in your project. Use exclusion to resolve the conflicts. For more information, see [Maven Guides](https://maven.apache.org/guides/introduction/introduction-to-optional-and-excludes-dependencies.html).
    OSS Java SDK is dependent on the following jar versions. Conflict solutions are similar to the httpclien method.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22308/155728411413398_en-US.png)


## Missing files { .section}

-   Cause analysis

    If the following errors occur when you use OSS Java SDK, your project may lack the necessary files for OSS Java SDK compilation and execution.

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/auth/Credentials
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:76)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.auth.Credentials
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 3 more
    					
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/apache/http/protocol/HttpContext
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:268)
            at com.aliyun.oss.OSSClient.<init>(OSSClient.java:193)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:76)
    Caused by: java.lang.ClassNotFoundException: org.apache.http.protocol.HttpContext
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 3 more
    					
    ```

    Or

    ```
    Exception in thread "main" java.lang.NoClassDefFoundError: org/jdom/input/SAXBuilder
            at com.aliyun.oss.internal.ResponseParsers.getXmlRootElement(ResponseParsers.java:645)
            at … … 
            at com.aliyun.oss.OSSClient.doesBucketExist(OSSClient.java:471)
            at com.aliyun.oss.OSSClient.doesBucketExist(OSSClient.java:465)
            at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:82)
    Caused by: java.lang.ClassNotFoundException: org.jdom.input.SAXBuilder
            at java.net.URLClassLoader$1.run(URLClassLoader.java:366)
            at java.net.URLClassLoader$1.run(URLClassLoader.java:355)
            at java.security.AccessController.doPrivileged(Native Method)
            at java.net.URLClassLoader.findClass(URLClassLoader.java:354)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:425)
            at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:308)
            at java.lang.ClassLoader.loadClass(ClassLoader.java:358)
            ... 11 more
    					
    ```

    Dependencies of OSS Java SDK are as follows:

    -   aliyun-sdk-oss-2.2.1.jar
    -   hamcrest-core-1.1.jar
    -   jdom-1.1.jar
    -   commons-codec-1.9.jar
    -   httpclient-4.4.1.jar
    -   commons-logging-1.2.jar
    -   httpcore-4.4.1.jar
    -   log4j-1.2.15.jar
    In the preceding dependencies, all files except the log4j-1.2.15.jar file are required. You need to add this file to enable access logging.

-   Solutions

    Add OSS Java SDK dependencies to your project. Methods:

    -   If your project is in Eclipse, see [Method 2: Import a JAR package](reseller.en-US/SDK Reference/Java/Installation.md#section_dqn_fpm_1z) to your Eclipse project in Java SDK.
    -   If your project is in Ant, add OSS Java SDK dependencies to the lib directory.
    -   If you directly use the .javac or .java files, run the `-classpath` command or the `-cp` command to specify the path where OSS Java SDK dependencies are stored or save the Java SDK dependencies to classpath.

## Connection timeout { .section}

-   Cause analysis

    If the following errors occur when the OSS Java SDK program is run, possible causes are endpoint errors or unavailable networks:

    ```
    com.aliyun.oss.ClientException: SocketException
        at com.aliyun.oss.common.utils.ExceptionFactory.createNetworkException(ExceptionFactory.java:71)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:116)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequestImpl(ServiceClient.java:121)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequest(ServiceClient.java:67)
        at com.aliyun.oss.internal.OSSOperation.send(OSSOperation.java:92)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:140)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:111)
        at com.aliyun.oss.internal.OSSBucketOperation.getBucketInfo(OSSBucketOperation.java:1152)
        at com.aliyun.oss.OSSClient.getBucketInfo(OSSClient.java:1220)
        at com.aliyun.oss.OSSClient.getBucketInfo(OSSClient.java:1214)
        at com.aliyun.oss.demo.HelloOSS.main(HelloOSS.java:94)
    Caused by: org.apache.http.conn.HttpHostConnectException: Connect to oss-test.oss-cn-hangzhou-internal.aliyuncs.com:80 [oss-test.oss-cn-hangzhou-internal.aliyuncs.com/10.84.135.99] failed: Connection timed out: connect
        at org.apache.http.impl.conn.DefaultHttpClientConnectionOperator.connect(DefaultHttpClientConnectionOperator.java:151)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.connect(PoolingHttpClientConnectionManager.java:353)
        at org.apache.http.impl.execchain.MainClientExec.establishRoute(MainClientExec.java:380)
        at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:236)
        at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
        at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
        at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
        at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:113)
        ... 9 more
    					
    ```

-   Solution

    You can use [ossutil](../../../../reseller.en-US/Tools/ossutil/Bucket-related commands.md#section_njd_yzz_zgb) for fast troubleshooting.


## org.apache.http.NoHttpResponseException: The target server failed to respond {#section_wgy_2kd_kfb .section}

-   Cause analysis

    The following error occurs when the OSS Java SDK program is run:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22308/155728411413399_en-US.png)

    The preceding error may occur if expired connections are used. This error occurs only when an earlier version of Java SDK 2.1.2 is used.

-   Solutions

    Upgrade OSS Java SDK to 2.1.2 or a later version.


## OSS Java SDK stops responding when the Java program is called { .section}

-   Cause analysis

    OSS Java SDK stops responding when the Java program is called. Run the `jstack -l pid` command to view the stacks. The exception is as follows:

    ```
    "main" prio=6 tid=0x000000000291e000 nid=0xc40 waiting on condition [0x0000000002dae000]
    java.lang.Thread.State: WAITING (parking)
        at sun.misc.Unsafe.park(Native Method)
        - parking to wait for  <0x00000007d85697f8> (a java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject)
        at java.util.concurrent.locks.LockSupport.park(LockSupport.java:186)
        at java.util.concurrent.locks.AbstractQueuedSynchronizer$ConditionObject.await(AbstractQueuedSynchronizer.java:2043)
        at org.apache.http.pool.PoolEntryFuture.await(PoolEntryFuture.java:138)
        at org.apache.http.pool.AbstractConnPool.getPoolEntryBlocking(AbstractConnPool.java:306)
        at org.apache.http.pool.AbstractConnPool.access$000(AbstractConnPool.java:64)
        at org.apache.http.pool.AbstractConnPool$2.getPoolEntry(AbstractConnPool.java:192)
        at org.apache.http.pool.AbstractConnPool$2.getPoolEntry(AbstractConnPool.java:185)
        at org.apache.http.pool.PoolEntryFuture.get(PoolEntryFuture.java:107)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager.leaseConnection(PoolingHttpClientConnectionManager.java:276)
        at org.apache.http.impl.conn.PoolingHttpClientConnectionManager$1.get(PoolingHttpClientConnectionManager.java:263)
        at org.apache.http.impl.execchain.MainClientExec.execute(MainClientExec.java:190)
        at org.apache.http.impl.execchain.ProtocolExec.execute(ProtocolExec.java:184)
        at org.apache.http.impl.execchain.RedirectExec.execute(RedirectExec.java:110)
        at org.apache.http.impl.client.InternalHttpClient.doExecute(InternalHttpClient.java:184)
        at org.apache.http.impl.client.CloseableHttpClient.execute(CloseableHttpClient.java:82)
        at com.aliyun.oss.common.comm.DefaultServiceClient.sendRequestCore(DefaultServiceClient.java:113)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequestImpl(ServiceClient.java:123)
        at com.aliyun.oss.common.comm.ServiceClient.sendRequest(ServiceClient.java:68)
        at com.aliyun.oss.internal.OSSOperation.send(OSSOperation.java:94)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:146)
        at com.aliyun.oss.internal.OSSOperation.doOperation(OSSOperation.java:113)
        at com.aliyun.oss.internal.OSSObjectOperation.getObject(OSSObjectOperation.java:229)
        at com.aliyun.oss.OSSClient.getObject(OSSClient.java:629)
        at com.aliyun.oss.OSSClient.getObject(OSSClient.java:617)
        at samples.HelloOSS.main(HelloOSS.java:49)
    					
    ```

    The cause is due to the connection leak in the connection pool \(possibly because ossObject is left unclosed after it is used\).

-   Solutions

    Check your program and ensure that no connection leak occurs. Use the following code to close ossObject:

    ```
    // Read an object.
    OSSObject ossObject = ossClient.getObject(bucketName, objectName);
    // Perform operations on OSS.
    // Close your ossObject.
    ossObject.close();
    					
    ```


## Connection closure { .section}

-   Cause analysis

    If the following errors occur when you use ossClient.getObject:

    ```
    Exception in thread "main" org.apache.http.ConnectionClosedException: Premature end of Content-Length delimited message body (expected: 11990526; received: 202880
        at org.apache.http.impl.io.ContentLengthInputStream.read(ContentLengthInputStream.java:180)
        at org.apache.http.impl.io.ContentLengthInputStream.read(ContentLengthInputStream.java:200)
        at org.apache.http.impl.io.ContentLengthInputStream.close(ContentLengthInputStream.java:103)
        at org.apache.http.impl.execchain.ResponseEntityProxy.streamClosed(ResponseEntityProxy.java:128)
        at org.apache.http.conn.EofSensorInputStream.checkClose(EofSensorInputStream.java:228)
        at org.apache.http.conn.EofSensorInputStream.close(EofSensorInputStream.java:174)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at com.aliyun.oss.event.ProgressInputStream.close(ProgressInputStream.java:147)
        at java.io.FilterInputStream.close(FilterInputStream.java:181)
        at samples.HelloOSS.main(HelloOSS.java:39)
    					
    ```

    The cause is that the time between two consecutive data reading exceeds one minute. OSS will close the connections if the time between two consecutive data reading exceeds one minute.

-   Solution

    If you only need to read part of the data during unfixed periods of time, use [Range download](reseller.en-US/SDK Reference/Java/Download objects/Range download.md#) to avoid connection closure during data reading.


## Memory leaks { .section}

-   Cause analysis

    Memory leaks occur when the OSS Java SDK program is run for a period of time \(from several hours to several days based on access traffic\). We recommend that you use [Eclipse Memory Analyzer \(MAT](http://www.eclipse.org/mat/downloads.php?)\) to analyze memory usage. For more information about usage methods, see [Use MAT for Head Dump File Analysis](https://www.ibm.com/developerworks/cn/opensource/os-cn-ecl-ma/).

    If the analysis result is similar to the following figure \(PoolingHttpClientConnectionManager occupies 96 percent of memory\), the cause may be that new OSSClient is run multiple times while ossClient.shutdown is not called. Consequently, memory leaks occur.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22308/155728411413400_en-US.png)

-   a

    Ensure that ossClient.shutdown is enabled after OSSClient is enabled.


## InterruptedException occurs when ossClient.shutdown is called { .section}

-   Error Cause

    The following exception is thrown when OSSClient.shutdown is called:

    ```
    java.lang.InterruptedException: sleep interrupted
            at java.lang.Thread.sleep(Native Method)
            at com.aliyun.oss.common.comm.IdleConnectionReaper.run(IdleConnectionReaper:76)
    					
    ```

    The cause is that the backend thread \(IdleConnectionReaper\) of ossClient will close connections periodically. If ossClient.shutdown is called when IdleConnectionReaper is in the Sleep mode, the preceding exception occurs.

-   Solution

    OSS Java SDK 2.3.0 has rectified this error.

    For versions earlier than OSS Java SDK 2.3.0, use the following code to ignore the error:

    ```
    try {
        ossClient.shutdown();
    } catch(Exception e) {
    }
    					
    ```


## Other errors { .section}

For more information about how to rectify other errors returned by OSS, see [Errors and troubleshooting](../../../../reseller.en-US/Errors and Troubleshooting/OSS 403.md#).

