# How to store remote attachments to your OSS instance with WordPress {#concept_pf4_3hb_wdb .concept}

## Preface {#section_qyh_jhb_wdb .section}

The website remote attachment function refers to directly storing uploaded attachments to a remote storage server, which is usually a remote FTP server, over the FTP.

Currently, Discuz forums, PHPWind forums, and WordPress websites support the remote attachment function.

This document instructs you on storing remote attachments from a WordPress-based forum.

## Preparation {#section_ngd_phb_wdb .section}

Apply for an OSS account and create a public-read bucket.  You must set the permission to public-read because it must allow anonymous access.

## Procedures {#section_mrj_qhb_wdb .section}

WordPress does not have inherent support for this function, but implements remote attachment using a third-party plug-in.  The WordPress we use is WordPress 4.3.1 and the plug-in is Hacklog Remote  Attachment. The specific configuration process is as follows:

1.  Log on to the WordPress website and select “Install Plug-in”. Search for the keyword “FTP” and choose to install Hacklog Remote Attachment.
2.  Configuration
    -   Set the FTP server address, that is, the address that runs the OSS-FTP. Generally, this is 127.0.0.1.
    -   Set “FTP service port No.” to the default “2048“.
    -   Set “FTP Account” in the format of AccessKeyID/BucketName, where “/“ does not mean “or“.
    -   Set “FTP Password” to AccessKeySecret.

        **Note:** To obtain the AccessKeyID and AccessKeySecret, you can log on to the Alibaba Cloud console and go to Access  Key Management.

    -   Set the FTP time-out to the default value, 30 seconds.
    -   Set “Remote Basic URL” to `http://BucketName.Endpoint/wp`.  Here, we want to test the bucket test-hz-jh-002  from the Hangzhou region. Therefore, we enter `http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/wp`.
    -   Set “FTP Remote Path”. We enter “wp”, that is, to save all attachments to the bucket’s wp directory.  Note that this field is related to the “Remote Basic URL” field.
    -   Set “HTTP Remote Path” to “.”  .

        For detailed information, see the figure below.

3.  Verification

    After the configuration is complete, click “Save” and a test starts automatically. The test results are shown at the top of the page.

4.  Post a new article and insert an image.

    Now you can write a new article and test the remote attachment function.  After creating an article, click “Add Media” to upload an attachment.

    Upload the attachment as shown in the following figure.

5.  When the attachment is uploaded, click “Post” to view your article.

    Right-click the image and click “Open image in new tab” to see the image URL.

    The image URL indicates that the image has been successfully uploaded to the OSS.


