# ossprobe {#concept_frt_pxg_wdb .concept}

## Introduction {#section_ts5_vxg_wdb .section}

The ossprobe is an OSS access detection tool used to troubleshoot problems caused by network errors or incorrect settings of basic parameters during the upload and download processes. If an error occurs after you run a command to upload or download data, the ossprobe displays the possible cause to help you identify the error quickly.

## Version {#section_us5_vxg_wdb .section}

Version: 1.0.0

## Main features {#section_ngl_xxg_wdb .section}

-   Checking whether the network environment is normal
-   Checking whether basic parameters are correct
-   Testing the upload and download speeds

## Platforms {#section_j5g_yxg_wdb .section}

-   Linux 
-   Windows
-   Mac

## Download software {#section_nsf_zxg_wdb .section}

-   [windows64 ossprobe](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/tool/ossprobe.exe)
-   [linux64 ossprobe](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/tool/ossprobe)
-   [mac ossprobe](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/tool/macossprobe/ossprobe)

## Detect download problems {#section_q4y_zxg_wdb .section}

-   Usage

    ```
    ossprobe --download [-i AccessKeyId] [-k AccessKeySecret] [-p EndPoint] [-b BucketName] [-o ObjectName] [-t LocalPath]  
            [-f Url] [-a Address]
        -f --from Object的Url
        -i --id AccessKeyId
        -k --key AccessKeySecret
        -p --endpoint EndPoint
        -b --bucket BucketName
        -o --object ObjectName
        -t --to Save path for the downloaded content. By default, it is the path to a temporary file in the current directory.
         -a --addr Network address for detection. The default address is www.aliyun.com. If you are using private cloud, select an accessible address in the private cloud.
    ```

-   Example

    To check whether URL-based download is normal \([How to obtain a URL](../intl.en-US/Quick Start/Share an object.md#)\), run the following commands:

    |Method|Command|
    |:-----|:------|
    |Download from a specified URL|`ossprobe --download -f Url`|
    |Download from a specified URL and save the downloaded content to a specified file|`ossprobe --download -f Url -t tmp/example.txt`|
    |Download from a specified URL and detect the network condition of a specified address|`ossprobe --download -f Url -a Addr`|

    To check whether download using specified parameters \(AccessKeyID, AccessKeySecret, EndPoint, and BucketName\) is normal, run the following commands:

    |Method| Command|
    |:-----|:-------|
    |Download a random file|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b
              Bucketname
    ```

 |
    |Download a specified file|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName
    ```

 |
    |Download a specified file and save the downloaded content to a specified local file|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -t tmp/example.txt
    ```

 |
    |Download a random file and detect the network condition of a specified address|     ```
ossprobe --download -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -a
              Addr
    ```

 |

    **Note:** 

    -   The file you downloaded is a binary executable program, and you must add the ossprobe executable permissions through `chmod +x ossprobe` in the Linux system.
    -   By default, the -t parameter indicates the path to a temporary file in the current directory \(the file name format is ossfilestore20160315060101\).
    -   If the -t parameter indicates a directory, a temporary file is generated in the directory to save data \(the file name format is ossfilestore20160315060101\).
    -   If a file is downloaded from a URL, the file is named after the last string following the forward slash “/“ in the URL. For example, if the URL is `http://aliyun.com/a.jpg`, then the file is saved as `a.jpg`.

## Detect upload problems {#section_lb2_kyg_wdb .section}

-   Usage

    ```
    ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint  -b BucketName [-m normal|append|multipart]  
            [-s UploadFilePath] [-o ObjectName] [-a Addr]
        -i --id AccessKeyID
        -k --key AccessKeySecret
        -p --endpoint EndPoint
        -b --bucket BucketName
        -s --src Path to the file you want to upload. By default, it is the path to a local temporary file.
        -m --mode File upload mode. The default is normal upload.
        -o --object Uploaded object name. By default, the object name is the name of the uploaded file if -s is not null. If -s is null, by default, the object name is the name of the temporary file starting with tem.
        -a --addr Network address for detection. The default address is the address of the Alibaba Cloud website. If you are using private cloud, select an accessible address in the private cloud.
    ```

-   Example

    |Method|Command|
    |:-----|:------|
    |Generate a temporary file and upload it in normal mode|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b
            BucketName
    ```

 |
    |Generate a temporary file and upload it in append mode|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m append
    ```

 |
    |Generate a temporary file and upload it in multipart mode|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m multipart
    ```

 |
    |Upload specified content in multipart mode|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -o
              ObjectName -m multipart -s src
    ```

 |
    |Upload specified content in multipart mode and specify the object name|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -m
              multipart -s src -o example.txt
    ```

 |
    |Generate a temporary file, upload it in normal mode, and detect the network condition of a specified address|     ```
ossprobe --upload -i AccessKeyId -k AccessKeySecret -p EndPoint -b BucketName -a
              Addr
    ```

 |

    **Note:** The name of a randomly generated file starts with ossuploadtmp.


## Platform differences {#section_x1b_bzg_wdb .section}

-   For Windows, press Win+R to bring up the “Run” dialog box, enter cmd, and press Enter. On the command-line interface \(CLI\), enter the path to the tool and enter related detection parameters to run the tool.

    ![](images/2981_en-US.jpg)

-   For Linux and Mac, open the terminal. On the displayed interface, enter the path to the tool and enter related detection parameters to run the tool. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4879/2982_en-US.jpg)


## View report data {#section_kkh_3zg_wdb .section}

After command execution, a report named logOssProbe20060102150405.txt is generated \(the numbers following logOssProbe indicate the formatted date of report generation\). The possible error cause is printed in command line mode. If you think the error message is not specific, you can view the report. If the problem persists, you can submit a ticket attached with the detection report.

Console display

The console displays the following main information:

-   After execution, the steps marked with × fail, whereas the steps not marked with × are successful.
-   The result indicates whether the upload or download operation is successful. If the upload or download operation is successful, the console displays the file size and upload/download time.
-   The “Suggested Change” column shows the error cause or change suggestions.
-   If you are familiar with OSS error codes, you can perform troubleshooting based on the error message returned by OSS.
-   The “Log Info” columns shows the log name and address, allowing you to find the log.

**Note:** 

No change suggestions may be given when an error is detected. When this happens, perform troubleshooting based on the returned error code by referring to [OSS error code](https://www.alibabacloud.com/help/doc-detail/32157.htm).

Log files

Different from console display, log files contain network detection details. Ping is used to detect a specified network or the network of a specified EndPoint, tracert is used to detect the route for EndPoint access, and nslookup is used for DNS detection.

## References {#section_dmw_4zg_wdb .section}

-   [OSS error codes](https://www.alibabacloud.com/help/doc-detail/32157.htm)
-   [How to obtain a URL](../intl.en-US/Quick Start/Share an object.md#)

