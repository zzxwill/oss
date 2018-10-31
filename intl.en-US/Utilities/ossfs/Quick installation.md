# Quick installation {#concept_kkp_lmb_wdb .concept}

ossfs allows you to mount Alibaba Cloud OSS buckets to local files in Linux systems. In the system, you can quickly use the local file system to perform operations on OSS objects, achieving data sharing.

## Main features {#section_y3r_cnb_wdb .section}

The ossfs is constructed based on S3FS and incorporates all S3FS functions. The main features include:

-   Supports most functions of the POSIX file system, including file reading/writing, directories, link operations, permissions, UID/GID, and extended attributes.
-   Uploads large files using the OSS multipart function.
-   Guarantees data integrity with MD5 checksum.

## Limitations {#section_udr_dnb_wdb .section}

Compared with a local file system, the functions and performance provided by ossfs have certain limitations. These include:

-   Random write or any appended operations causes rewrite of the entire file.
-   The performance of metadata operations, such as list directory, is poor because the system has to remotely access the OSS server.
-   The file/folder rename operation is not atomic.
-   When multiple clients are attached to a single OSS bucket, you must coordinate the actions of each client manually. For example, you must avoid multiple clients writing the same file.
-   Hard link is not supported.
-   This system is not suitable for highly-concurrent read/write scenarios, as this greatly increases the system load.

## Installation and use {#section_rhp_2nb_wdb .section}

-   Installation package download

    |Released Linux| Download|
    |:-------------|:--------|
    |Ubuntu 16.04 \(x64\)|[ossfs\_1.80.5\_ubuntu16.04\_amd64.deb](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32196/cn_zh/1527232098199/ossfs_1.80.5_ubuntu16.04_amd64.deb?spm=a2c4g.11186623.2.4.xMFgkl&file=ossfs_1.80.5_ubuntu16.04_amd64.deb)|
    |Ubuntu 14.04 \(x64\)|[ossfs\_1.80.5\_ubuntu14.04\_amd64.deb](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32196/cn_zh/1527232140988/ossfs_1.80.5_ubuntu14.04_amd64.deb?spm=a2c4g.11186623.2.5.xMFgkl&file=ossfs_1.80.5_ubuntu14.04_amd64.deb)|
    |CentOS 7.0 \(x64\)|[ossfs\_1.80.5\_centos7.0\_x86\_64.rpm](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32196/cn_zh/1527232195135/ossfs_1.80.5_centos7.0_x86_64.rpm?spm=a2c4g.11186623.2.6.xMFgkl&file=ossfs_1.80.5_centos7.0_x86_64.rpm)|
    |CentOS 6.5 \(x64\)|[ossfs\_1.80.5\_centos6.5\_x86\_64.rpm](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/32196/cn_zh/1527232237335/ossfs_1.80.5_centos6.5_x86_64.rpm?spm=a2c4g.11186623.2.7.xMFgkl&file=ossfs_1.80.5_centos6.5_x86_64.rpm)|

    Due to the lower version of the Linux distribution, the kernel version is relatively lower. The ossfs is prone to disconnection or other problems during the running process. Therefore, users are advised to upgrade the operating system to CentOS 7.0 or Ubuntu 14.04 or later.

-   Installation method
    -   Run the following commands to install ossfs for Ubuntu:

        ```
        sudo apt-get update
        sudo apt-get install gdebi-core
        sudo gdebi your_ossfs_package
        ```

    -   Run the following command to install ossfs for CentOS 6.5 or later:

        ```
        sudo yum localinstall your_ossfs_package
        ```

    -   Run the following command to install ossfs for CentOS 5:

        ```
        sudo yum localinstall your_ossfs_package --nogpgcheck
        ```

-   Usage

    Set bucket name and AccessKeyId/Secret and save it to the /etc/passwd-ossfs file. Note that the permissions for this file must be set correctly. We suggest setting it to 640.

    ```
    echo my-bucket:my-access-key-id:my-access-key-secret > /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    ```

    Mount the OSS bucket  to the specified directory.

    ```
    ossfs my-bucket my-mount-point -ourl=my-oss-endpoint
    ```

    Example

    Mount the bucket `my-bucket` to the `/tmp/ossfs` directory. The AccessKeyId is `faint`, the AccessKeySecret is `123`, and the OSS endpoint is `http://oss-cn-hangzhou.aliyuncs.com`.

    ```
    echo my-bucket:faint:123 > /etc/passwd-ossfs
    chmod 640 /etc/passwd-ossfs
    mkdir /tmp/ossfs
    ossfs my-bucket /tmp/ossfs -ourl=http://oss-cn-hangzhou.aliyuncs.com
    ```

    **Note:** If you use an Alibaba Cloud ECS instance to provide ossfs services, you can use the intranet endpoints. In this example, you can replace the OSS endpoint with `oss-cn-hangzhou-internal.aliyuncs.com` to save bandwidth costs. For more information about intranet endpoints, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Regions and endpoints.md#)

    Unmount the bucket:

    ```
    fusermount -u /tmp/ossfs
    ```

    For more information, see [GitHub ossfs](https://github.com/aliyun/ossfs#ossfs).


## Release log {#section_rn4_l4b_wdb .section}

For more information, see [GitHub ChangeLog](https://github.com/aliyun/ossfs/blob/master/ChangeLog)

