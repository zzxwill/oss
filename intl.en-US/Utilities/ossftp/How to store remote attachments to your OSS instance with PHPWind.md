# How to store remote attachments to your OSS instance with PHPWind {#concept_vwc_m2b_wdb .concept}

## Preface {#section_cm4_p2b_wdb .section}

The website remote attachment function refers to directly storing uploaded attachments to a remote storage server, which is usually a remote FTP server, over the FTP.

Currently, Discuz forums, PHPWind forums, and WordPress websites support the remote attachment function.

This document instructs you on storing remote attachments from a PHPWind-based forum.

## Preparation {#section_cqh_52b_wdb .section}

Apply for an OSS account and create a public-read bucket. You must set the permission to public-read because it must allow anonymous access.

## Procedures {#section_smk_v2b_wdb .section}

The PHPWind we use is PHPWind 8.7 and the configuration process is as follows.

1.  Log on to the website.

    Go to the management interface and select**Global** \> **Upload Settings** \> **Remote Attachments**.

    ![](images/2813_en-US.png)

2.  Configure the function

    ![](images/2815_en-US.png)

    **Note:** 

    -   Set “Enable FTP uploads“ to Yes.
    -   Set “Website Attachment Address“ to [http://bucket-name.endpoint](http://bucket-name.endpoint/)*. Here, we want to test the bucket test-hz-jh-002 from the Hangzhou region. Therefore, we enter *[http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com](http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/), where the BucketName must match the endpoint.
    -   Set the FTP server address, that is, the address that runs the OSS-FTP. Generally, this is 127.0.0.1.
    -   Set “FTP service port No.” to the default “2048“.
    -   Set “Remote attachment directory” to “.“, that is, to create a directory for upload under the root directory of the bucket.
    -   Set “FTP Account” in the format of AccessKeyID/BucketName,  where “/“ does not mean “or“.
    -   Set “FTP Password” to AccessKeySecret. To obtain the AccessKeyID and AccessKeySecret, you can log on to the Alibaba Cloud console and go to Access  Key Management.
    -   Set the FTP time-out time. If you set it to “10”, a time-out response is sent if a request does not receive a response within 10 seconds.
3.  Verification

    PHPWind does not allow users to directly test the function by clicking a test button. Therefore, we must publish a post with an image to verify the function.

    ![](images/2817_en-US.png)

    Right-click the image and select “Open image in new tab”. The image is displayed in a new tab.

    ![](images/2818_en-US.png)

    The image URL indicates that the image has been uploaded to bucket test-hz-jh-002 in the OSS.


