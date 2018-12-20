# Bucket Policy {#concept_jrf_4tm_2gb .concept}

Bucket Policy是基于资源的授权策略。相比于RAM Policy，Bucket Policy支持在控制台直接进行图形化配置操作，并且Bucket拥有者直接可以进行访问授权。

Bucket Policy常见的应用场景如下：

-   向其他账号的RAM用户授权访问。

    您可以授予其他账号的RAM用户访问您的OSS资源的权限。

-   向匿名用户授予带特定IP条件限制的访问权限。

    某些场景下，您需要向匿名用户授予带IP限制的访问策略。例如，企业内部的机密文档，只允许在企业内部访问，不允许在其他区域访问。由于企业内部人员较多，如果针对每个人配置RAM Policy，工作量非常大。此时，您可以基于Bucket Policy设置带IP限制的访问策略，从而高效方便地进行授权。


Bucket Policy的配置方法和教程视频请参见[使用Bucket Policy授权其他用户访问OSS资源](../../../../cn.zh-CN/控制台用户指南/管理文件/使用Bucket Policy授权其他用户访问OSS资源.md#)。

