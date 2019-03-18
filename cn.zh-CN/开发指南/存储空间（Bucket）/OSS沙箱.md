# OSS沙箱 {#concept_j41_hvl_cgb .concept}

阿里云 OSS 不承担网络攻击的防护义务，当您的 OSS Bucket 遭受攻击或有被攻击的风险时，OSS会自动将您被攻击的 Bucket 切入沙箱。沙箱中的 Bucket 仍可以正常响应请求，但服务质量将被降级，您的应用可能会有明显感知。

对于多次被攻击的 Bucket，OSS 将停止此 Bucket 的服务，若您的 Bucket 遭受攻击，您需要自行承担因攻击而产生的全额费用。

**说明：** 当您的 Bucket 切入到沙箱中后，您将会收到短信和邮件通知。

## 预防措施 {#section_pjx_m1m_cgb .section}

为防止您的 Bucket 因攻击原因被切入沙箱，您可使用高防 IP 来抵御 DDoS 攻击和 CC 攻击。如果您的业务有可能遭受攻击，可以按照如下两种方案添加预防措施。

-   方案一：绑定域名并配置高防 IP
    1.  OSS 绑定自定义域名，配置步骤请参考[绑定自定义域名](../../../../../cn.zh-CN/控制台用户指南/管理存储空间/管理域名/绑定自定义域名.md#)。
    2.  根据业务情况，购买合适的[高防 IP](https://common-buy.aliyun.com/?spm=5176.10695662.958511.2.5f267a64VdiI7O&commodityCode=ddosBag#/buy)。
    3.  将高防 IP 绑定到您已设置好的自定义域名上。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79825/155289406434179_zh-CN.png)

        -   防护网站：填写您绑定的 OSS 自定义域名。
        -   协议类型：按照您的实际访问方式选择。
        -   源站IP/域名：选择源站域名，并填写您 OSS 的默认访问域名。
        其他步骤及参数填写，请参考文档[设置高防 IP](https://help.aliyun.com/document_detail/63559.html)。

-   方案二：配置 ECS 反向代理并配置高防 IP

    基于安全考虑，Bucket 默认域名解析的 IP 是随机变化的，若您期望使用固定 IP 方式访问，推荐使用 ECS 搭建反向代理的方式进行访问 OSS。ECS 上的 EIP 可以绑定高防 IP 以抵御 DDoS 攻击和 CC 攻击。具体可以按照如下方式进行配置：

    1.  创建一个 CentOS 或 Ubuntu 的 ECS 实例，详情请参考[创建 ECS 实例](../../../../../cn.zh-CN/实例/实例生命周期/创建实例/使用向导创建实例.md#)。

        **说明：** 若 Bucket 有很大的网络流量或访问请求，请提高 ECS 硬件配置或者搭建 ECS 集群。

    2.  配置 ECS 反向代理方式访问 OSS，详细配置可参考[配置 OSS 反向代理](../../../../../cn.zh-CN/最佳实践/使用ECS实例反向代理OSS/基于CentOS的ECS实例实现OSS反向代理.md#)。
    3.  根据业务情况，购买合适的[高防 IP](https://common-buy.aliyun.com/?spm=5176.10695662.958511.2.5f267a64VdiI7O&commodityCode=ddosBag#/buy)。
    4.  参考方案一的[步骤3](#)。其中， **防护网站**填写您 ECS 绑定的域名，**源站 IP/域名**填写 ECS 的外网 IP。

方案优劣势分析

|方案名称|优势|劣势|
|:---|:-|:-|
|方案一：绑定域名并配置高防 IP|配置简单：支持控制台图形化设置。|应用场景有局限性：只能对未进入沙箱的 Bucket 提供防护。|
|方案2：配置 ECS 反向代理并配置高防 IP| -   解决方案具有通用性：能够为已进入沙箱和未进入沙箱的 Bucket 提供防护。
-   适合通过固定 IP 访问 OSS 的场景。

 | -   配置复杂:需要用户自定搭建 Nginx 反向代理。
-   成本高：需要额外购买 ECS 搭建反向代理。

 |

## Bucket 已进入沙箱如何处理 {#section_fj2_5wl_cgb .section}

-   针对已经进入沙箱的Bucket，阿里云不提供迁出服务。因此，针对已经进入沙箱的 Bucket，请按照[方案二](#)配置安全防护措施。

    **说明：** 建议在 Bucket 所在的 Region 搭建 ECS，并且 proxy\_pass 填写 Bucket 内网域名地址。

-   若您的账号下的 Bucket 曾多次遭受攻击。那么，您后续新建的 Bucket 默认也会进入沙箱。此时，针对新建 Bucket 的安全访问措施如下：
    1.  购买高防 IP。
    2.  通过工单系统提交“新建 Bucket 默认不进入沙箱申请”。
    3.  申请通过后，参照[方案一](#)进行配置。

