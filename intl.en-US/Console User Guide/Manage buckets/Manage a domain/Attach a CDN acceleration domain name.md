# Attach a CDN acceleration domain name {#concept_eyf_fvh_wfb .concept}

You can read objects in OSS buckets by using the Alibaba Cloud CDN-based acceleration service. The acceleration service uses OSS buckets as origin sites and distributes the content from the origin sites to edge nodes. With its precise scheduling system, Alibaba Cloud CDN assigns requests to optimal edge nodes so that end users can quickly read their required content, easing Internet traffic congestion and improving response time.

To enable the Alibaba Cloud CDN-based acceleration service, you must direct your self-hosted domain name to a CDN acceleration domain name assigned by Alibaba Cloud CDN. Afterwards, all requests for your self-hosted domain name are redirected to the CDN edge nodes.

You can enable the Alibaba Cloud CDN-based acceleration service by the following two methods:

-   Attach your self-hosted domain name to the domain name of an OSS bucket and enable the CDN-based acceleration service. For more information, see [Method 1: Enable the CDN-based acceleration service through the OSS console](#).
-   Direct the domain name of an OSS bucket to a CDN acceleration domain name, and then attach your self-hosted domain name to the CDN acceleration domain name \(CNAME\). For more information, see [Method 2: Enable the CDN-based acceleration service through the CDN console](#).

## Method 1: Enable the CDN-based acceleration service through the OSS console. {#one .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/overview). In the left-side bucket list, click the name of the bucket to which you want to attach a custom domain name.
2.  Click **Domain Names** \> **Bind Self-Hosted Domain Name**. In the Bind Self-Hosted Domain Name page, you can set the following parameters:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/15506291411703_en-US.png)

    -   Self-Hosted Domain Name: Enter the domain name that you want to attach, such as hello-world.com. The maximum length of a domain name is 63 characters.
    -   Enable Alibaba Cloud CDN: Enable the [CDN-based acceleration](../../../../../reseller.en-US/Developer Guide/Hide/CDN-based OSS acceleration.md#)service.
    -   Add CNAME Record Automatically: The record of a CNAME managed by your Alibaba Cloud account can be automatically added. To add a record of a domain name that is not managed by your Alibaba Cloud account, you must manually configure the DNS of your DNS provider. For more information, see [Manually add a CNAME record](#).
3.  Enter your self-hosted domain name, and enable **Enable Alibaba Cloud CDN** and Add CNAME Record Automatically.
4.  Click **Submit**.

    **Note:** If a domain name conflict message is prompted, the domain name is currently attached to a bucket owned by another user. To resolve this issue, click Obtain TXT and add a txt record of the domain name to the DNS of your DNS provider to verify the ownership of the domain name and forcibly attach the domain name to your bucket. However, if you forcibly attach a domain name to a new bucket, the domain name is detached from the bucket to which it is currently attached. For more information, see [Verify domain name ownership](#).

5.  Updates to your domain name information take about one minutes to take effect. After updating a domain name, you can click **Binding configuration** to view **CDN Domain Name** and **OSS Access Domain Name**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155062914132592_en-US.png)

    ![](images/32592_en-US_source.png)


## Method 2: Enable the CDN-based acceleration service through the CDN console. {#two .section}

1.  Log on to the [Alibaba Cloud CDN console](https://cdn.console.aliyun.com/overview).
2.  Select **Domain Names** \> **Add Domain Name**.
3.  Enter the CDN acceleration domain name, and select the OSS bucket that you want to accelerate as the origin site.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155062914132707_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |**Domain**|Enter your domain name, such as ch.aliyun.com.|
    |**Resource Group**|Select the default resource group.|
    |**Business Type**|Select the optimal business type for your scenario based on the content you have stored in OSS and how it is generally used.|
    |**Origin Site Information**|Select the OSS domain name that you want to accelerate.|
    |**Port**|Select the access port as needed.|
    |**Acceleration Region**|Select the region in which you want to use the acceleration service.|

4.  Click **Next**.

    After you add a CDN acceleration domain name, a CNAME record is generated. You must add the CNAME record to the DNS of your DNS provider to enable the CDN-based acceleration service. For more information, see [Manually add a CNAME record](#dns).


## Manually add a CNAME record {#dns .section}

The following steps apply only to scenarios where a CNAME record is not automatically added.

You must add a CNAME record to the DNS of your DNS provider. In this topic, Alibaba Cloud DNS is used as an example to describe the process of adding a CNAME record.

1.  Log on to the Alibaba Cloud CDN console and open the [Domain Names](https://cdn.console.aliyun.com/#/domain/list) page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155062914134242_en-US.png)

2.  Copy the CNAME of the domain name that you want to add.
3.  Log on to the [Alibaba Cloud DNS console](https://dns.console.aliyun.com/#/dns/domainList).
4.  In the domain name list, click **Configure** on the right of the domain name that you want to add a record.
5.  Click **Add Record** and enter the DNS information. The following table describes the parameters that you can configure.

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
    |**Value**|Enter the value of the record based on the selected record type.In this example, enter the CNAME record that you copied in [Step 2](#).

|
    |**TTL**|Select the update period of the record. In this example, select the default value.|

6.  Click **OK**.

    **Note:** An added CNAME record takes effect immediately. However, modifications to a CNAME record take up to 72 hours to take effect.


## Check CNAME status {#section_xxj_q3r_5fb .section}

After a CNAME record is configured, the time period required for the record to take effect varies according to DNS providers. You can run the ping or lookup command to check the status of an added CNAME. If it directs to `*.*kunlun*.com`, the CNAME has taken effect.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155062914134256_en-US.png)

## Verify domain name ownership {#Verification .section}

If your self-hosted domain name is attached to a bucket owned by another user, follow these steps to verify the ownership of the domain name and forcibly detach the domain name from the current bucket.

**Note:** The following steps apply only in scenarios where a Domain name conflict message is prompted when you [Attach a custom domain name](#).

1.  Click Obtain TXT to obtain a txt record generated by the system based on your information.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4746/15506291411705_en-US.png)

2.  Add the txt record to the DNS of your DNS provider. For a domain name added to Alibaba Cloud DNS, you can add a record in the **Add Record** page following the procedures described in [Manually add a CNAME record](#). Then, you can configure the parameters as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/64596/155062914135316_en-US.png)

    -   Type: Select TXT.
    -   Host: Enter a character "@".
    -   Value: Enter the value generated on the **Bind Self-Hosted Domain Name** page in the OSS console.
    -   Retain default values for other parameters.
3.  On the **Bind Self-Hosted Domain Name** page, click **I have added the TXT record，Continue submission.**. If the system confirms that the information is correct, the verification is passed.

## Enable auto CDN cache update {#cdncache .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/overview).
2.  In the left-side bucket list, click the name of the bucket that you want to enable the auto CDN cache update function.
3.  Click the **Domain Names** tab.
4.  Enable **Auto CDN Cache Update** for the record that a self-hosted domain name is attached.

After the auto CDN cache update function is enabled, modifications to an object in the bucket are automatically updated to the CDN cache.

**Note:** If you detach your self-hosted domain name from the bucket, the auto CDN cache update function is not supported in the OSS console. However, you can update the CDN cache in the Alibaba Cloud CDN console.

## AccessDenied error {#section_k5j_kvh_wfb .section}

After attaching a self-hosted domain name to a bucket, you can access a target OSS resource using the URL that is composed of your self-hosted domain name and the path of the resource, such as `http://mydomain.cn/test/1.jpg`. However, if you access OSS only with your self-hosted domain name \(such as `http://mydomain.cn`\) an AccessDenied error occurs because the default page of the OSS static website is not configured. For more information about configuring the default page of an OSS static website, see [Host a static website](../../../../../reseller.en-US/Best Practices/Bucket management/Static website hosting.md#).

