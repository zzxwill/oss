# Quick installation for OSS FTP {#concept_d4b_g2t_vdb .concept}

## Introduction {#section_ntf_h2t_vdb .section}

The OSS FTP is a special FTP server that maps the operations on files and folders into your OSS instance upon receiving a common FTP request. This utility allows you to use the FTP protocol to manage files stored on your OSS instance.

**Note:** OSS SDK is designed for the production environment, and OSS FTP is mainly for individual users.

-   Key features
    -   **Cross-Platform**: This utility can run on Windows, Linux, and Mac operating systems, either 32 or 64 bit, either on a graphic or command-line interface.
    -   **Free of Installation**: You can run this utility directly after extraction.
    -   **Free of Configuration**: You can run the utility without any further configurations.
    -   **Transparent**: The FTP utility was written in Python, so you can see the complete source code. We will soon make the open source available on GitHub.
-   Key functions

    -   Supports file/folder upload, download, delete, and other operations
    -   Supports multipart upload of large files
    -   Supports most FTP commands and can satisfy daily needs
    **Note:** 

    -   Currently, for the ease of installation and deployment, OSS  FTP V1.0 does not support TLS encryption. The FTP protocol implements plaintext transmission. To prevent password leaks, we recommend that you run the  FTP server and client on the same machine and access using 127.0.0.1:port.
    -   The utility does not support rename and move operations.
    -   Do not include any Chinese characters in the extract-to path of the installation package.
    -   The FTP server’s management control page may fail to be opened on early IE browsers.
    -   Supported Python versions: Python 2.6 and Python 2.7

## Downloads {#section_xj4_vpt_vdb .section}

-   Windows: [ossftp-1.0.3-win.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-win.zip)

    Now that Python 2.7 is not installed on Windows by default, it is contained in the installation package and is ready for use after extraction, without the hassle of installation and configuration.

-   Linux/Mac: [ossftp-1.0.3-linux-mac.zip](http://gosspublic.alicdn.com/ossftp/ossftp-1.0.3-linux-mac.zip)

    Because Python 2.7 or Python 2.6 is installed on Linux and Mac systems by default, the installation packages for Linux and Mac do not contain an executable Python program, but only relevant dependent libraries.


## Running {#section_brg_bqt_vdb .section}

First, extract the downloaded file. Then, select an appropriate running mode based on environmental conditions.

-   Windows: Double-click start.vbs to run it.
-   Linux: Start the terminal and run it.

    ```
    $ bash start.sh
    ```

-   Mac: Double-click start.command or run it on a terminal.

    ```
    $ bash start.command
    ```


The preceding process starts an FTP server, which listens to port 2048 at 127.0.0.1 by default. In addition, for ease of control over the status of the FTP  server, the program also activates a web server, which listens to port 8192 at 127.0.0.1. If your system has a graphic interface, the control page is automatically opened.

**Note:** In most situations, you do not need to configure any settings before running the FTP server. If you make any configuration, remember to restart it to make the changes take effect.

## Connecting to the FTP Server {#section_uwr_rqt_vdb .section}

We recommend using the [FileZilla Client](https://filezilla-project.org/?spm=a2c4g.11186623.2.6.bqHidZ) to connect to the FTP server. After download and installation, connect to the FTP server as follows:

-   Host: 127.0.0.1
-   Logon type: normal
-   User: access\_key\_id/bucket\_name
-   Password: access\_key\_secret

    **Note:** 

    -   The slash sign \(/\) means that both, not either items are required. For example, the user could be `tSxyiUM3NKswPMEp/test-hz-jh-002`.
    -   For more information about access\_key\_id and **access\_key\_secret**, see [OSS Access Control](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#).

## Advanced use {#section_xdq_krt_vdb .section}

-   Manage the ftpserver from the console page

    -   Modify the Listener Address

        If you want to access the ftpserver over a network, you must modify the listener address because the default address, 127.0.0.1, only allows local access. You can change it to an intranet IP or Internet IP.

    -   Modify the Listening Port 

        Modify the ftpserver’s listening port. We suggest using a port over 1024 because ports below 1024 require administrator permissions.

    -   Modify the Log Level

        Set the ftpserver’s log level. The FTP  server’s log is output to the `data/ossftp/` directory.  You can view it only by pressing the Log button on the console page. The default log level is INFO and little information is printed in the log. If you need more detailed log information, you can change the level to DEBUG. If you want to reduce log output, you can set the log level to WARNING or ERROR.

    -   Set Bucket Endpoints

        By default, the ftpserver searches for the bucket’s location information, so it can send subsequent requests to the corresponding \(such as`oss-cn-hangzhou.aliyuncs.com` or `oss-cn-beijing.aliyuncs.com`\).  The ftpserver first tries to access the OSS instance over the intranet.  If you set bucket endpoints,  for example, `test-bucket-a.oss-cn-hangzhou.aliyuncs.com`,  when you access test-bucket-a, you go to the`oss-cn-hangzhou.aliyuncs.com` domain name.

    -   Set Display Language 

        By setting cn/en, the display language of the FTP control page can be modified to Chinese/English.

    **Note:** 

    -   The system must be restarted for modifications to take effect.
    -   All the preceding modifications are actually changes to the ftp directory’s config.json file. Thus, you can also modify this file directly.
-   Directly start ftpserver \(Linux/Mac\)

    You can only run the ftpserver.py file in the ossftp directory to  avoid web\_server overhead.

    ```
    $ python ossftp/ftpserver.py &
    ```

    The configuration modification method is the same to the preceding method.


## Potential problems {#section_g3r_zrt_vdb .section}

-   If you encounter an error when connecting to the FTP server.

    The error may be caused by two possible causes:

    -   There may be an error in the entered access\_key\_id or access\_key\_secret.

        Solution: Enter the correct information and try again.

    -   The used access\_key information may be a RAM sub-account access\_key for a sub-account without list buckets permission.

        Solution: When using a sub-account, specify bucket endpoints on the console page to tell the ftpserver which endpoint must be used to access a certain bucket. Also, the sub-account must have the required permissions. For information on implementing access control by using RAM to access OSS, see [RAM](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#).  The details about permissions are as follow:

        -   Read-only: 

            The OSS-FTP must have these permissions: \[‘ListObjects’, ‘GetObject’, ‘HeadObject’\]. For information on creating a RAM sub-account with Read-only permission, see the graphic tutorial [How to Integrate RAM for File Sharing](reseller.en-US/Utilities/ossftp/How to integrate RAM for file sharing.md#).

        -   Upload files: 

            If you want to allow a RAM sub-account to upload files, assign \[‘PutObject’\] permission.

        -   Delete files

            If you want to allow a RAM sub-account to delete files, assign \[‘DeleteObject’\] permission.

-   If you are running the FTP server on Linux,  you may encounter the following error when using FileZilla to connect to the server:

    ```
    501 can't decode path (server filesystem encoding is ANSI_X3.4-1968)
    ```

    This is usually generated when errors occur in local Chinese code. Input the following command in the terminal where you want to run start.sh. Then, restart the program.

    ```
    $ export LC_ALL=en_US.UTF-8; export LANG="en_US.UTF-8"; locale
    ```


