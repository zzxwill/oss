# Initialization {#concept_32028_zh .concept}

This topic describes how to initialize OSS Python SDK.

Most operations in OSS Python SDK are performed through oss2.Service and oss2.Bucket.

-   The oss2.Service class is used to list buckets.
-   The oss2.Bucket class is used to upload, download, and delete objects, and configure buckets.

To initialize the two classes, you must specify an endpoint. The oss2.Service class does not support access with the custom domain \(CNAME\). For more information about endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#) and [Bind a custom domain name](../../../../reseller.en-US/Developer Guide/Access and control/Bind a custom domain name.md#).

## Initialize the oss2.Service class { .section}

For more information, see the bucket listing part in [Manage buckets](reseller.en-US/SDK Reference/Python/Bucket.md#).

## Initialize the oss2.Bucket class. {#section_ld1_jbj_kfb .section}

-   Use a OSS domain name to initialize the class

    Run the following code to initialize the oss2.Bucket class with a OSS domain name.

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    # This case uses the Hangzhou endpoint as an example. Fill in the region endpoint name according to the actual circumstances.
    endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
    
    bucket = oss2. Bucket(auth, endpoint, '<yourBucketName>')
    
    ```

-   Use a custom domain name to initialize the class

    Run the following code to initialize the oss2.Bucket class with a custom domain name

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    
    # Use a custom domain name: my-domain.com as an example. is_cname=True indicates that the CNAME is enabled. Cname means binding a custom domain name to the storage space. CNAME indicates a custom domain bound to a bucket.
    cname = 'http://my-domain.com'
    bucket = oss2. Bucket(auth, cname, '<yourBucketName>', is_cname=True)
    
    ```

-   Set connection timeout

    Run the following code to set connection timeout:

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
    
    # Set the connection timeout to 30 seconds.
    bucket = oss2. Bucket(auth, endpoint, '<yourBucketName>', connect_timeout=30)
    
    ```

-   Disable CRC verification

    CRC verification is enabled by default during uploads and downloads to ensure data integrity during uploads and downloads. Run the following code to disable CRC verification:

    **Warning:** We recommended that you do not disable CRC verification. If you disable CRC verification, the data integrity during uploads and downloads is not guaranteed.

    ```language-python
    # -*- coding: utf-8 -*-
    import oss2
    
    # It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    auth = oss2. Auth('<yourAccessKeyId>', '<yourAccessKeySecret>')
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint = 'http://oss-cn-hangzhou.aliyuncs.com'
    
    bucket = oss2. Bucket(auth, endpoint, '<yourBucketName>', enable_crc=False)
    
    ```


