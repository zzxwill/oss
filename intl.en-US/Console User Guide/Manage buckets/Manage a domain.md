# Manage a domain {#concept_n23_1h2_vdb .concept}

After uploading an object to a bucket, you can obtain an object address including two parts: an OSS domain name address \(<BucketName\>.<Endpoint\>\) and an object file name. To avoid possible cross-origin or security problems in your business, we recommend that you access OSS using a user-defined domain name. After the domain name is successfully bound, you also need to add a CNAME record pointing to the Internet domain name of the bucket to guarantee proper domain name-based access to the OSS.

**Note:** 

-   You must apply for an ICP license for your bound domain name. Otherwise, the domain name is not accessible.
-   Each bucket can be bound with a maximum of 20 domain names.

After a user-defined domain name is successfully bound, access addresses of the files stored in your OSS uses the user-defined domain name. For example, if your bucket test-1-001 is located at the Hangzhou node, the object file name is test001.jpg, and the bound user-defined domain name is hello-world.com, then the access address of this object is as follows:

-   Before binding: test-1-001.oss-cn-hangzhou.aliyuncs.com/test001.jpg
-   After successful binding: hello-world.com/test001.jpg

Custom domain names can be bound to OSS domain names through the console to implement custom domain name access storage space under the document, you can also configure the Ali cloud CDN to implement the acceleration function at the same time.

## Bind a domain name {#section_mhx_sh2_vdb .section}

1.  Go to the [OSS console](https://oss.console.aliyun.com/).
2.  On the left-side navigation pane, select a bucket from the bucket list to open the bucket overview page.
3.  Click the **Domain Names** tab.
4.  Click **Bind Self-Hosted Domain Name** to open the Bind Self-Hosted Domain Name dialog box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/15367439761703_en-US.png)

5.  Bind your domain.
    1.  In the **Self-Hosted Domain Name** text box, enter your domain name.
    2.  If you need CDN acceleration, open the **Enable Alibaba Cloud CDN** switch.

        **Note:** For more information about CDN acceleration, see the best practice [CDN-based OSS acceleration](../../../../intl.en-US/Best Practices/Bucket management/CDN-based OSS acceleration.md#).

    3.  If you want to add a CNAME record automatically, open the **Add CNAME Record Automatically** switch.

        **Note:** If the domain name has completed cloud resolution under another Alibaba Cloud account, then a CNAME record cannot be automatically added for this domain name under your account. In this case, you must add a CNAME record manually. For more information, see the **Procedure for domain name resolution** section.

6.  Click **Submit**.

    **Note:** If the domain name you want to bind has been maliciously bound by another user, the system message **Domain name conflict** is displayed. You can verify the ownership of the domain name **by adding a TXT record.** In this way, the domain name can be forcibly bound to the correct bucket and its binding to the previous bucket is released. For detailed procedure, see the Procedure for verifying domain name ownership section.

    If you need to unbind the domain name, click **Binding Configuration**, and then click **Unbind**.


## Upload an HTTPS certificate {#section_dbh_l42_vdb .section}

If you want your domain to access OSS through HTTPS, you must purchase an HTTPS certificate. You can purchase an HTTPS certificate from any certificate provider or from Alibaba Cloud Certificates Service \(see [Certificates Service Quick Start](https://www.alibabacloud.com/help/zh/doc-detail/28547.htm)\), and upload your certificate in the OSS console.

-   If Alibaba Cloud CDN is not enabled for OSS, you can upload your certificate in the OSS console:
    1.  On the **Domain Names** tab page, click **Upload Certificate** under **Action**.
    2.  On the Upload Certificate page, enter your public key and private key, and then click **Upload**.

        **Note:** For certificate format requirements, see [Certificate format description](https://www.alibabacloud.com/help/zh/doc-detail/66710.htm).

-   If Alibaba Cloud CDN is enabled for OSS, you must upload your certificate in the CDN console. For more information, see [HTTPS Security Acceleration](https://www.alibabacloud.com/help/zh/doc-detail/27118.htm).

## Procedure for verifying domain name ownership {#section_q5m_2q2_vdb .section}

1.  Click **Obtain TXT**. The system generates a TXT record based on your information.
2.  Log on to your DNS provider and add the corresponding TXT record.
3.  In the OSS console, click **I have added the TXT verification file. Continue submission**. If the system detects that the TXT record value for this domain name is as expected, the domain name ownership passes verification.

## Procedure for domain name resolution {#section_kn4_gr2_vdb .section}

1.  Go to the [Alibaba Cloud console](https://netcn.console.aliyun.com/core/domain/tclist?spm=a2c4g.11186623.2.11.uhGQXh). From the left-side navigation pane, click Alibaba Cloud DNS to enter the domain name resolution list page.
2.  Click the **Configure** link corresponding to the target domain name.
3.  Click **Add Record**.
4.  In the Add Record dialog box, select **CNAME** from the **Type** drop-down box, and enter the Internet domain name of the bucket in the **Value** text box.
5.  Click **Confirm**.

