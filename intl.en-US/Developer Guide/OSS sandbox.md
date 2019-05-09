# OSS sandbox {#concept_j41_hvl_cgb .concept}

When your OSS bucket is under attack or is used to distribute illegal content, OSS automatically adds the bucket to the sandbox. The bucket in the sandbox can still respond to requests. However, the service quality is degraded, which your application may be aware of.

-   If your bucket is under attack, OSS automatically adds the attacked bucket to the sandbox. In this case, you must bear the cost incurred by the attack.
-   If your user uses your bucket to distribute illegal content that involves pornography, politics, and terrorism, OSS also adds the bucket to the sandbox. Users will be held liable for serious violations.

**Note:** After your bucket is added to the sandbox, you will receive SMS and email notifications.

## Preventive measures against attacks {#section_ztb_xl1_htk .section}

To prevent your bucket from being added to the sandbox due to attacks, you can use Anti-DDoS Pro to prevent DDoS attacks and HTTP floods. If your business is subject to attacks, you can use either of the following solutions to add preventive measures:

-   Solution 1: Attach a domain to the bucket and configure an Anti-DDoS Pro instance
    1.  Attach a custom domain to the bucket. For more information, see [Attach a custom domain](../../../../reseller.en-US/Console User Guide/Manage buckets/Manage a domain/Attach a custom domain name.md#).
    2.  Attach the Anti-DDoS Pro instance to the custom domain you have set.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79825/155738500534179_en-US.png)

        -   Domain Name: Specify the custom domain you want to attach the Anti-DDoS Pro instance to.
        -   Protocol: Select one as needed.
        -   Origin IP/Domain: Click Origin site domain. Enter the default domain of your OSS.
-   Solution 2: Configure a reverse proxy by using ECS and configure an Anti-DDoS Pro instance

    For security reasons, the IP address resolved from the default domain of a bucket changes dynamically. If you want to use a fixed IP address to access the bucket, you can use ECS to set up a reverse proxy. You can attach the EIP of an ECS instance to an Anti-DDoS Pro instance to prevent DDoS attacks and HTTP floods. You can set up the reverse proxy as follows:

    1.  Create an ECS instance in CentOS or Ubuntu. For more information, see [Create an ECS instance](../../../../reseller.en-US/Instances/Create an instance/Create an instance by using the wizard.md#).

        **Note:** If the bucket encounters sporadic bursts of network traffic or access requests, upgrade hardware configurations of ECS or set up ECS clusters.

    2.  Purchase appropriate [Anti-DDoS Pro](https://common-buy.aliyun.com/?spm=5176.10695662.958511.2.5f267a64VdiI7O&commodityCode=ddosBag#/buy) based on your business needs.
    3.  Attach the Anti-DDoS Pro instance to the domain you have set, as shown in [Step 3](#li_xo5_p0p_6az). Set **Domain Name** to the domain you want to attach to the EIP of the ECS instance and **Origin IP/Domain** to Origin Site IP. Enter the EIP of the ECS instance in the text box.

Solution comparison and analysis

|Solution|Advantage|Disadvantage|
|:-------|:--------|:-----------|
|Solution 1: Bind a domain to the bucket and configure an Anti-DDoS Pro instance|Simple configurations: You can perform the configurations through the GUI in the console.|Scenarios: Only buckets that are not added to the sandbox can be protected.|
|Solution 2: Configure a reverse proxy by using ECS and configure an Anti-DDoS Pro instance| -   Application scope: All buckets can be protected, regardless of whether they are added to the sandbox.
-   Scenarios: Use a fixed IP address to access OSS.

 | -   Complex configurations: You must set up an NGINX reverse proxy.
-   High costs: You need to purchase ECS to set up an NGINX reverse proxy.

 |

## Preventive measures against operations that involve illegal content {#section_1kt_88g_obj .section}

## What shall I do if a bucket is added to the sandbox {#section_p79_wub_3fx .section}

Alibaba Cloud does not provide migration services for buckets that are added to the sandbox. If your bucket is added to the sandbox, perform different operations based on different scenarios:

-   A bucket is added to the sandbox due to attacks
    -   If a bucket is added to the sandbox, configure security preventive measures, as shown in [Solution 2: Configure a reverse proxy by using ECS and configure an Anti-DDoS Pro instance](#li_d3u_pi9_pz7).

        **Note:** Create an ECS instance that is in the same region as your bucket. Set proxy\_pass to the intranet domain of the bucket.

    -   If the bucket in your account encounters multiple attacks, buckets created after that bucket are also automatically added to the sandbox. To address this issue, follow these steps:
        1.  Purchase Anti-DDoS Pro.
        2.  Use the ticket system to submit the application of Created Bucket Not added to the Sandbox.
        3.  After the application is approved, configure the Anti-DDoS Pro instance, as shown in [Solution 1: Attach a domain to the bucket and configure an Anti-DDoS Pro instance](#ul_2r9_qek_dvc).
-   A bucket is added to the sandbox due to the distribution of illegal content
    -   If a bucket is added to the sandbox, follow these steps:
        1.  Activate Content Moderation. Alibaba Cloud Content Moderation periodically scans your bucket to prevent the distribution of illegal content.
        2.  Use ECS to configure the reverse proxy, as shown in [Solution 2: Configure a reverse proxy by using ECS and configure an Anti-DDoS Pro instance](#li_d3u_pi9_pz7).

            **Note:** Create an ECS instance that is in the same region as your bucket. Set proxy\_pass to the intranet domain of the bucket.

    -   If you own buckets that distribute illegal content at one time or over multiple times, these buckets and newly created buckets are automatically added to the sandbox. To address this issue, follow these steps:
        1.  Purchase Content Moderation.
        2.  Use the ticket system to submit the application of Created Bucket Not added to the sandbox.
        3.  After the application is approved, configure Content Moderation to periodically detect illegal content distributed through your buckets.

