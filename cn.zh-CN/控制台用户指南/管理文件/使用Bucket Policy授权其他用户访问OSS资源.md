# 使用Bucket Policy授权其他用户访问OSS资源 {#concept_ahc_tx4_j2b .concept}

Bucket Policy是阿里云OSS推出的针对Bucket的授权策略，您可以通过Bucket Policy授权其他用户访问您的OSS资源。

相比于[RAM Policy](../../../../../cn.zh-CN//授权管理/授权策略管理.md#)，Bucket Policy支持在控制台直接进行图形化配置操作，并且Bucket拥有者直接可以进行访问授权，通常用来将私有文件共享给指定的用户或群体。

## 配置步骤 {#section_nbp_by4_j2b .section}

1.  登录[OSS 管理控制台](https://oss.console.aliyun.com/)。
2.  在左侧存储空间列表中，单击需要授权的存储空间名称，打开该存储空间概览页面。
3.  单击文件管理页签，然后单击**授权**。
4.  在授权页面，单击**新增授权**。
5.  在新增授权页面，设置授权资源。
    -   整个Bucket：授权策略针对整个Bucket生效。
    -   指定资源：授权策略只针对指定的资源生效。您需要同时输入资源路径，例如：abc/myphoto.png。若是针对目录授权，则需在目录结尾处加上星号（\*），例如：abc/\*。
6.  设置**授权用户**区域：

    -   **当前账号**：您可以从下拉菜单中选择当前账号的子账号，授予 Bucket 访问权限。您的账号必须是主账号，或拥有此 Bucket 管理权限及 RAM 控制台ListUsers权限的子账号。
    -   **其他账号**：当您需要给其他账号授予 Bucket 访问权限，或您的账号无ListUsers权限时，请直接输入被授权账号的 UID。
    -   **匿名账号（\*）**：若您需要给所有用户授权，可以选择**匿名账号（\*）**。
    **说明：** 给 RAM 用户授予ListUsers权限可参考：

    -   [为 RAM 用户授权](../../../../../cn.zh-CN/快速入门/为 RAM 用户授权.md#)。
    -   ListUsers权限的 RAM 授权模板：

        ```
        {
            "Version": "1",
            "Statement": [
                {
                    "Effect": "Allow",
                    "Action": [
                        "ram:ListUsers"
                    ],
                    "Resource": [
                        "*"
                    ],
                    "Condition": {}
                }
            ]
        }
        ```

7.  设置授权操作。
    -   只读：对相关资源拥有查看、列表及下载权限。
    -   读/写：对相关资源有读和写权限。
    -   完全控制：对相关资源有所有操作的权限。
    -   拒绝访问：拒绝对相关资源的所有操作。

        **说明：** 若针对某用户同时配置了多个Bucket Policy规则，那么该用户所拥有的权限是所有Policy规则的叠加，但是拒绝访问优先。例如，针对某用户第一次设置了只读权限，第二次设置了读/写权限，那么该用户最终的权限为只读和读/写权限的叠加，即读/写权限。如果第三次设置了拒绝访问权限，则该用户最终的权限为拒绝访问。

8.  （可选）设置**条件**来限定此用户使用特定条件访问OSS资源。
    -   IP =：设置IP等于某一IP地址或IP地址段。
    -   IP ≠：设置IP不等于某一IP地址或IP地址段。

        **说明：** 

        -   IP地址，例如：10.10.10.10。如有多个IP地址，用英文逗号（,）分隔。
        -   IP地址段，例如：10.10.10.1/24。
    -   访问方式：设置使用HTTP/HTTPS访问OSS资源。
9.  单击**确定**。

## 场景案例 {#section_btk_dmm_4gb .section}

-   场景一：某公司A，希望其合作公司B可以访问Bucket内指定文件目录date/内的文件。实现步骤如下：
    1.  Bucket Policy配置步骤请参考[配置步骤](#)，其中：

        -   **授权资源**填写指定的文件目录date。
        -   **授权用户**选择其他账号，并填写B公司账号的子账号UID号。
        -   **授权操作**选择只读。
        -   **条件**不配置。
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15400/154839746838147_zh-CN.png)

    2.  B公司员工使用被授权的子账号访问A公司授权访问的资源。
        -   使用被授权子账号登录OSS控制台，在左边菜单栏单击**我的访问路径**后的加号（+），添加A公司授权访问的文件目录路径后直接访问。
        -   使用被授权子账号登录[ossutil](../../../../../cn.zh-CN/常用工具/命令行工具ossutil/快速开始.md#)，之后通过命令访问A公司授权访问的文件。
        -   使用被授权子账号登录[ossbrowser](../../../../../cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)，登录时在**预设OSS路径**栏输入被授权访问的文件目录。

-   场景二：某公司C，部分内部公开文档存放在OSS Bucket的file/目录内，现希望公司员工可以直接访问公开文档内的资料。

    考虑到员工较多，通过RAM Policy授权，工作量很大。此时，可以使用Bucket Policy设置带IP限制的访问策略，实现C公司需求，步骤如下：

    1.  Bucket Policy配置步骤请参考[配置步骤](#)，其中：

        -   **授权资源**填写指定的文件目录file。
        -   **授权用户**选择匿名账号（\*）。
        -   **授权操作**选择只读。
        -   **条件**选择IP =，并填写C公司的外网出口IP。
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15400/154839746838157_zh-CN.png)

    2.  C公司内部员工在公司内部使用如下方式访问公开文档。
        -   使用Bucket默认域名或[已绑定在Bucket上的自有域名](cn.zh-CN/控制台用户指南/管理存储空间/管理域名/绑定自定义域名.md#)加文件路径进行访问。例如，`http://mybucket.oss-cn-beijing.aliyuncs.com/file/myphoto.png`。
        -   使用阿里云的任意账号的子账号登录OSS控制台，在左边菜单栏单击**我的访问路径**后的加号（+），添加内部公开文档的目录路径后直接访问。
        -   使用阿里云的任意账号登录[ossutil](../../../../../cn.zh-CN/常用工具/命令行工具ossutil/快速开始.md#)，之后通过命令访问A公司授权访问的文件。
        -   使用阿里云的任意账号登录[ossbrowser](../../../../../cn.zh-CN/常用工具/图形化管理工具ossbrowser/快速开始.md#)，登录时在**预设OSS路径**栏输入被授权访问的文件目录。
-   场景三：公司D的产品资料存放在OSS Bucket的public/目录内，需授权给本公司内部员工和合作公司E访问，为了访问安全，仅允许使用HTTPS的方式访问。

    因为有多个需求，所以需要配置三条策略，实现步骤如下：

    1.  配置策略，授权给合作公司E访问权限，配置步骤参考[场景一](#)。
    2.  配置策略，授权给内部员工访问权限，配置步骤参考[场景二](#)。
    3.  配置策略，禁止使用HTTP的方式访问，配置参数如下：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/15400/154839746938166_zh-CN.png)

    4.  策略配置完成后，访问方式可参考场景一和场景二的访问方式，需要注意的是：
        -   通过网页访问时需使用HTTPS的方式访问，例如：`https://mybucket.oss-cn-beijing.aliyuncs.com/file/myphoto.png`。
        -   控制台访问本身是HTTPS的方式，无影响。
        -   ossbrowser在登录时需将**Endpoint**修改为自定义，之后输入对应的Endpoint的HTTPS域名，例如：`https://oss-cn-beijing.aliyuncs.com`。
        -   ossutil在配置**Endpoint**时，需输入对应的Endpoint的HTTPS域名，例如：`https://oss-cn-beijing.aliyuncs.com`。

## 教程视频 {#section_gk2_w4b_vfb .section}

-   授权单个用户访问 OSS 资源



-   授权多个用户访问 OSS 资源



-   拒绝特定 IP 地址访问 OSS 资源




