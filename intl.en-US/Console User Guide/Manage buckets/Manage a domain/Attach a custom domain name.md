# Attach a custom domain name {#concept_ozw_m2r_5fb .concept}

After an object is uploaded to an OSS bucket, a URL is automatically generated for the object. You can use this URL to access the object in the bucket. To access an uploaded object by using a custom domain name, you must attach the custom domain name to the bucket where the object is stored and add a CNAME record that directs to the Internet domain name of the bucket. This topic describes how to attach a custom domain name to a bucket and how to add a CNAME record.

## Attach a domain name to a bucket {#section_ikx_r2r_5fb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/overview). In the left-side bucket list, click the name of the bucket that you want to attach a custom domain name.
2.  Click **Domain Names** \> **Bind Self-Hosted Domain Name**. In the Bind Self-Hosted Domain Name page, you can set the following parameters:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/15535631131703_en-US.png)

    -   Self-Hosted Domain Name: Enter the domain name that you want to attach, such as hello-world.com. The maximum length of a domain name is 63 characters.
    -   Enable Alibaba Cloud CDN: If you want to enable [CDN-based OSS acceleration](../../../../../intl.en-US/Developer Guide/Hide/CDN-based OSS acceleration.md#), see [Attach a CDN acceleration domain name](intl.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a CDN acceleration domain name.md#).
    -   Add CNAME Record Automatically: The record of a CNAME managed by your Alibaba Cloud account can be automatically added. To add a record of a domain name that is not managed by your Alibaba Cloud account, you must manually configure the DNS of your DNS provider. For more information, see [Manually add a CNAME record](#).
3.  Enter your self-hosted domain name and enable Add CNAME Record Automatically. Click **Submit**.

    **Note:** 

    -   If a Domain name conflict message is prompted, the domain name has been attached to a bucket owned by another user. You can click Obtain TXT and add a txt record of the domain name to the DNS of your DNS provider to verify the ownership of the domain name and forcibly attach the domain name to your bucket. However, if you forcibly attach a domain name to a new bucket, the domain name is detached from the bucket to which it is currently attached. For more information, see [Verify domain name ownership](#).
    -   If a message is prompted that indicates the domain name is not filed, you must [file](https://beian.aliyun.com/order/selfBaIndex.htm) the domain name first.
4.  If you want to detach the domain name from the bucket, click **Domain Names** \> **Binding Configuration** \> **Unbind**.

## Manually add a CNAME record {#dns .section}

The following steps apply only to scenarios where a CNAME record is not automatically added.

You must add a CNAME record to the DNS of your DNS provider. In this topic, Alibaba Cloud DNS is used as an example to describe the process of adding a CNAME record.

1.  Log on to the [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).
2.  In the domain name list, click **Configure** on the right of the domain name that you want to add a record.
3.  Click **Add Record** and enter the DNS information. The following table describes the parameters that you can configure.

    |Parameter|Description|
    |:--------|:----------|
    |**Type**|Select the type of record to which the domain name directs.In this example, select **CNAME**.

|
    |**Host**|Enter the host record according to the prefix of the domain name. For example:    -   If the domain name is www.aliyun.com, enter "www".
    -   If the domain name is aliyun.com, enter a character "@".
    -   If the domain name is abc.aliyun.com, enter "abc".
    -   If the domain name is a second-level domain name, such as a.aliyun.com or b.aliyun.com, enter an asterisk "\*".
|
    |**ISP Line**|Select the ISP line used to resolve the domain name.We recommend that you select **Default** to allow the system to select the optimal line.

|
    |**Value**|Enter the value of the record based on the selected record type.In this example, enter the Internet URL of the bucket.

|
    |**TTL**|Select the update period of the record. In this example, select the default value.|

4.  Click**OK**.

    **Note:** An added CNAME record takes effect immediately. However, modifications to a CNAME record take up to 72 hours to take effect.


## Check CNAME status {#section_xxj_q3r_5fb .section}

After a CNAME record is configured, the time period required for the record to take effect varies according to DNS providers. You can run the `ping` or `lookup` command to check the status of an added CNAME. If it directs to `*.oss-cn-*.aliyuncs.com`, the CNAME has taken effect.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/155356311335330_en-US.png)

## Verify domain name ownership {#Verification .section}

If your user domain name is attached to a bucket owned by another user, follow these steps to verify the ownership of the domain name and forcibly detach the domain name from the current bucket.

**Note:** The following steps apply only in scenarios where a Domain name conflict message is prompted when you [Attach a custom domain name](#).

1.  Click Obtain TXT to obtain a txt record generated by the system based on your information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/15535631131705_en-US.png)

2.  Add the txt record to the DNS of your DNS provider. For a domain name added to Alibaba Cloud DNS, you can add a record in the **Add Record** page following the procedures described in [Manually add a CNAME record](#). Then, you can configure the parameters as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155356311335316_en-US.png)

    -   Type: Select TXT.
    -   Host: Enter "@".
    -   Value: Enter the value generated on the**Bind Self-Hosted Domain Name** page in the OSS console.
    -   Retain default values for other parameters.
3.  On the **Bind Self-Hosted Domain Name** page, click **I have added the TXT record，Continue submission.**. If the system confirms that the information is correct, the verification is passed.

