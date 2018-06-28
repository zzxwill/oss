# How to store remote attachments to your OSS instance with Discuz {#concept_n3l_ftt_vdb .concept}

## Preface {#section_vtk_gtt_vdb .section}

The website remote attachment function refers to directly storing uploaded attachments to a remote storage server, which is usually a remote FTP server, over the FTP.

Currently, Discuz forums, PHPWind forums, and WordPress websites support the remote attachment function.

This document instructs you on storing remote attachments from a Discuz-based forum.

## Preparation {#section_v4b_jz1_wdb .section}

Apply for an OSS account and create a public-read bucket. You must set the permission to public-read because it must allow anonymous access.

## Procedures {#section_zwg_kz1_wdb .section}

Here the Discuz version we use is Discuz!  X3.1 and the detailed configuration process is shown as follows.

1.  Log on to the Discuz website and go to the management interface. Click **Global** and then **Upload Settings**.
2.  Select **Remote Attachments** and configure the function.

    **Note:** 

    -   Set “Enable remote attachment” to Yes. 
    -   Set “Enable SSL connection” to No. 
    -   Set the “FTP Server Address”, that is, the address that runs the OSS-FTP. Generally, this is “127.0.0.1“. 
    -   Set “FTP service port No.” to the default “2048“. 
    -   Set “FTP Account” in the format of AccessKeyID/BucketName,  where “/“ does not mean “or“.
    -   Set “FTP Password” to AccessKeySecret. 
    -   Set “Passive Mode Connection” to the default Yes.
    **Note:** 

    -   Set “Remote Attachment Directory” to “.“,  that is, to create a directory for upload under the root directory of the bucket.
    -   Set “Remote URL” to `http://BucketName.Endpoint`.

        **Note:** Here, we want to test the bucket test-hz-jh-002 from the  Hangzhou region. Therefore, we enter `http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com`,  where the BucketName must match the endpoint. 

    -   Set the time-out time to 0, that is, to use the default setting of the service.
    -   After the configuration is complete, click “Test Remote Attachment”. If the test is successful, an information box is displayed.
3.  Verification

    Ok, now let’s publish a post on the forum to test the function. On any board, create a post and upload an image as attachment in the post.

    Right-click the image and select “Open image in new tab”.

    In the browser, you can see the image URL is http://test-hz-jh-002.oss-cn-hangzhou.aliyuncs.com/forum/201512/18/171012mzvkku2z3na2w2wa.png. This indicates that the image has been uploaded to test-hz-jh-002 in the OSS.


