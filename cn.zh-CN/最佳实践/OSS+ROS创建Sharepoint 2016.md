# OSS+ROS创建Sharepoint 2016 {#concept_htx_xbn_vdb .concept}

## 目标 {#section_vy1_1cn_vdb .section}

通过阿里云的服务快速创建 Sharepoint2016。

## 背景 {#section_ygm_ccn_vdb .section}

-   对象存储 OSS

    海量、安全、低成本、高可靠的云存储服务，提供99.999999999%的数据可靠性。使用RESTful API，可以在互联网任何位置存储和访问。容量和处理能力可弹性扩展，并能提供多种可选择的存储类型，全面优化存储成本。

-   资源编排 ROS

    资源编排（Resource Orchestration）是一种简单易用的云计算资源管理及自动化运维服务。用户通过模板描述多个云计算资源的依赖关系及配置等，并自动完成所有资源的创建和配置，从而达到自动化部署及运维等目的。编排模板同时也是一种标准化的资源和应用交付方式，可以随时编辑修改，使基础设施即代码（Infrastructure as Code）成为可能。

-   PowerShell

    PowerShell是Windows下的一种命令行外壳程序及环境脚本，用户可以编写 .ps1 脚本或利用 .Net Framework 进行脚本的编写和运行，与 Liunx 下的.sh 脚本类似。

-   Registry

    Windows注册表，本示例中用到了以下的KEY：

    HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon:

    AutoAdminLogon： 是否开启管理员自动登录，1: True 0: False

    DefaultUserName： 自动登录管理员的账号

    DefaultPassword： 自动登录管理员的密码

    在整个最佳实践当中，我们会组合使用阿里云的下列服务以完成整体工作：

    1.  阿里云OSS：我们将使用该服务提供的存储能力，在通过本地网络下载 SharePoint 的安装文件后，在 OSS 上创建“存储空间”并保存安装文件，再以内网的形式提供给需要安装 SharePoint 的机器。
    2.  阿里云资源编排ROS：我们将用到下文提到的 ROS 模板，通过 ROS 自动实现从 ECS 创建到 Sharepoint 安装的整个过程。你只需在使用 ROS 进行自动化安装前输入必要的参数即可。

## 步骤 {#section_vk3_jcn_vdb .section}

1.  新建 Bucket

    首先，在 OSS 中创建新的 Bucket，其中 Bucket ACL 建议设置为私有。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2308_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2309_zh-CN.png)

    Bucket 创建完成后，请从[此处](https://www.microsoft.com/en-us/download/details.aspx?spm=a2c4g.11186623.2.4.H1NvQs&id=51493)下载 img 文件，并存放至 OSS 中。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2310_zh-CN.png)

    这里建议将 img 文件的读写权限设置成公共读。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2312_zh-CN.png)

    该步骤完成后，我们可以获得一个阿里云提供的 URL 以访问该文件。

2.  通过ROS新建资源栈

    ROS 以模板的形式申明资源（如 ECS 和 VPC）并配置资源之间的关系（如 ECS 属于哪个VPC），并支持在 ECS 部署结束后自动执行用户脚本。模板中定义的所有资源都属于一个栈，用户通过资源栈管理自己的云资源。

    1.  进入ROS管理控制台，单击 **资源栈管理** \> **新建资源栈**，开始创建。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2318_zh-CN.png)

    2.  在输入脚本页面当中，用户需要选择将脚本中所创建的机器部署到哪一个Region。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2319_zh-CN.png)

        参考脚本如下：

        ```
        {
        "ROSTemplateFormatVersion": "2015-09-01",
        "Description": "One simple ECS instance and a security group. The user only needs to specify the image ID.",
        "Parameters": {
         "NewDomainNetbiosName": {
           "Type": "String",
           "Default": "ADXING"
         },
         "InternetMaxBandwidthOut": {
           "Type": "String",
           "Description": "Set internet output bandwidth of instance. Unit is Mbps(Mega bit per second). Range is [0,200]. Default is 1.While the property is not 0, public ip will be assigned for instance.  ",
           "MinLength": "1",
           "MaxLength": "41"
         },
         "ZoneId": {
           "Type": "String",
           "Description": "The available zone Id",
           "AllowedValues": [
             "cn-shenzhen-c"
           ]
         },
         "DomainName": {
           "Type": "String",
           "Default": "adxing.com"
         },
         "SPFarmAccountPassword": {
           "NoEcho": true,
           "Type": "String",
           "Default": "Banana#12345"
         },
         "SPISOImageURI": {
           "Type": "String",
           "AllowedPattern": "^(?i)(s3|http|https):\\/\\/.+",
           "Default": "http://sharepointbucket.oss-cn-shenzhen-internal.aliyuncs.com/officeserver.img"
         },
         "ImageId": {
           "Type": "String",
           "Description": "Image Id, represents the image resource to startup one ECS instance,, <a href='#/product/cn-shenzhen/list/imageList' target='_blank'>View image resources</a>",
           "Default": "win2012r2_64_dtc_17196_en-us_40G_alibase_20170915.vhd"
         },
         "SPFarmAccount": {
           "Type": "String",
           "Default": "spFarmAcc"
         },
         "InstanceType": {
           "Type": "String",
           "Description": "The instance type",
           "AllowedValues": [
             "ecs.c5.xlarge",
             "ecs.s1.small",
             "ecs.n4.small",
             "ecs.n4.large",
             "ecs.n4.xlarge",
             "ecs.mn4.small",
             "ecs.mn4.large",
             "ecs.mn4.xlarge",
             "ecs.n1.small",
             "ecs.n1.medium",
             "ecs.n1.large"
           ],
           "Default": "ecs.c5.xlarge"
         },
         "DomainAdminPassword": {
           "NoEcho": true,
           "Type": "String",
           "Default": "Banana#12345"
         },
         "DomainAdminUser": {
           "Type": "String",
           "Default": "spAdmin"
         },
         "Password": {
           "NoEcho": true,
           "Type": "String",
           "Default": "Banana12345"
         }
        },
        "Resources": {
         "WebServer": {
           "Type": "ALIYUN::ECS::Instance",
           "Properties": {
             "InternetMaxBandwidthOut": {
               "Ref": "InternetMaxBandwidthOut"
             },
             "UserData": {
               "Fn::Base64": {
                 "Fn::Join": [
                   "",
                   [
                     "[powershell]\n",
                     "$webclient = New-Object System.Net.WebClient\n",
                     "$url = 'http://sharepointbucket.oss-cn-shenzhen.aliyuncs.com/ros/Archive.zip'\n",
                     "$file = 'C:/Archive.zip'\n",
                     "$webclient.DownloadFile($url,$file)\n",
                     "Expand-Archive -Path 'C:/Archive.zip' -DestinationPath 'C:/sp' -Force \n",
                     "$webclientSP = New-Object System.Net.WebClient\n",
                     "$officeserverfile = 'C:/officeserver.img'\n",
                     "$spURL = '",
                     {
                       "Ref": "SPISOImageURI"
                     },
                     "'\n",
                     "$webclientSP.DownloadFile($spURL,$officeserverfile)\n",
                     "[bat]\n",
                     "reg add 'HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon' /v AutoAdminLogon  /d 1 /f /reg:64\n",
                     "reg add 'HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon' /v DefaultUserName   /d administrator /f /reg:64 \n",
                     "reg add 'HKLM\\SOFTWARE\\Microsoft\\Windows NT\\CurrentVersion\\Winlogon' /v DefaultPassword   /d  ",
                     {
                       "Ref": "Password"
                     },
                     " /f /reg:64 \n",
                     "reg add 'HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\RunOnce' /v Install /d 'powershell.exe -Command c:/sp/STEP1.ps1 ",
                     "-DomainName  ",
                     {
                       "Ref": "DomainName"
                     },
                     " -NewDomainNetbiosName  ",
                     {
                       "Ref": "NewDomainNetbiosName"
                     },
                     " -DomainAdminPassword  ",
                     {
                       "Ref": "DomainAdminPassword"
                     },
                     " -DomainAdminUser  ",
                     {
                       "Ref": "DomainAdminUser"
                     },
                     " -SPFarmAccount  ",
                     {
                       "Ref": "SPFarmAccount"
                     },
                     " -SPFarmAccountPassword  ",
                     {
                       "Ref": "SPFarmAccountPassword"
                     },
                     " ' /f /reg:64 \n",
                     "shutdown -r -t 2\n"
                   ]
                 ]
               }
             },
             "SecurityGroupId": {
               "Ref": "SecurityGroup"
             },
             "ImageId": {
               "Ref": "ImageId"
             },
             "InstanceType": {
               "Ref": "InstanceType"
             },
             "Password": {
               "Ref": "Password"
             }
           }
         },
         "SecurityGroup": {
           "Type": "ALIYUN::ECS::SecurityGroup",
           "Properties": {
             "SecurityGroupIngress": [
               {
                 "PortRange": "-1/-1",
                 "Priority": 1,
                 "SourceCidrIp": "0.0.0.0/0",
                 "IpProtocol": "all",
                 "NicType": "internet"
               }
             ],
             "SecurityGroupEgress": [
               {
                 "PortRange": "-1/-1",
                 "Priority": 1,
                 "IpProtocol": "all",
                 "DestCidrIp": "0.0.0.0/0",
                 "NicType": "internet"
               }
             ]
           }
         }
        },
        "Outputs": {
         "InstanceId": {
           "Value": {
             "Fn::GetAtt": [
               "WebServer",
               "InstanceId"
             ]
           }
         },
         "PublicIp": {
           "Value": {
             "Fn::GetAtt": [
               "WebServer",
               "PublicIp"
             ]
           }
         },
         "SecurityGroupId": {
           "Value": {
             "Fn::GetAtt": [
               "SecurityGroup",
               "SecurityGroupId"
             ]
           }
         }
        }
        }
        ```

    3.  完成模版输入后，单击 **下一步**，对资源栈进行参数配置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2322_zh-CN.png)

        需要配置的参数如下（关于参数的描述，请参考[参数](https://help.aliyun.com/document_detail/28861.html) ）:

        ImageId：ECS 所使用的镜像，本示例中，我们使用 windows2012R2

        InternetMaxBandwidthOut：ECS 的出口带宽

        ZoneId：ECS 需要部署到的区域

        Password：ECS 的 Administrator 密码

        DomainName：示例中 Domain 的名称

        NewDomainNetbiosName：示例中的 NetbiosName

        DomainAdminPassword：Domain 管理员用户密码

        DomainAdminUser：Domain 用户名称

        SPFarmAccount：SharePoint 服务场管理员的账户名称

        SPFarmAccountPassword：SharePoint 服务场管理员的账户密码

        SPISOImageURI：SharePoint 的镜像地址，请使用将 img 文件上传至 OSS 后获取的URL

        InstanceType：ECS的规格。

    4.  模版创建完成后，在启动资源栈前输入参数内容。输入完成后，单击 **创建** 按钮。ROS会根据脚本以及输入的参数开始创建资源。

        资源创建完成后，可以在 **资源栈管理** 列表当中看到相应的资源栈。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2328_zh-CN.png)

        以上是整个执行流程中需要用到的参数文件。

3.  UserData 执行

    ROS 模板中 UserData 的执行分为五个过程，其中的四个过程是通过四个PS1文件执行完成的。各过程的描述如下：

    1.  通过 UserData 的方式对机器进行设定，使其在启动时下载 SharePoint 镜像，并启用自动登录。
    2.  安装 Domain 功能，并重启。
    3.  在新安装的 Domain 下创建用户，并安装 Sharepoint 所需要的模块及 MSSQL，并重启。
    4.  安装 SharePoint 服务并重启。
    5.  配置 SharePoint 服务。
    具体执行步骤如下：

    1.  机器首次启动后，系统将会下载 OSS 中的 SharePoint 文件，同时修改 Windows 的注册表，启用系统免密码自动登录。此外，系统还会设定在下一次启动时运行 Domain 的安装脚本。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2329_zh-CN.png)

    2.  SharePoint 文件下载完成后，系统将会重启，并且开始执行 Domain 的安装脚本。该脚本会对用户在 ROS 中设定的 Domain 进行配置。并将下一步需要执行的脚本写入注册表，等待下一次启动时运行。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2330_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2332_zh-CN.png)

    3.  创建用户并安装必要先决服务及软件。
        1.  系统将会根据用户在ROS当中设定的 DomainAdminUser 和 SPFarmAccount 创建2个用户，分别用于管理 Domain 和 SP 服务器场。
        2.  开始安装以下内容：.NET Framework Feature, ‘Application Server’ role, ‘Web Server’ role, WAS Feature, 及 Windows Identity Foundation Feature

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2336_zh-CN.png)

        3.  开始下载并安装 MS SQL SERVER EXPRESS 2012 版本（这里演示程序默认下载EXPRESS 版本的MSSQL）

            ```
            C:\SQLEXPR_x64_ENU.exe" /Q /IACCEPTSQLSERVERLICENSETERMS /ACTION=install  /ROLE=AllFeatures_WithDefaults  /INSTANCENAME=$($instanceName) /SQLSVCACCOUNT="$($serviceAccount)
            ```

        4.  关闭 IE ESC 选项，并离线下载和安装 SharePoint 2016 Preparation softer ware。因为某些原因，直接联网下载 Preparation softer ware 可能会失败，所以这里配置了脚本，使系统先下载文件，再进行安装。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2337_zh-CN.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2338_zh-CN.png)

    4.  当所有的先决服务和软件都安装成功后，脚本自动开始运行 SharePoint 的安装程序。安装程序会首先检查先决服务和软件是否安装完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2339_zh-CN.png)

        如果所有的准备工作都已完成，系统开始正式安装 SharePoint。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2340_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2341_zh-CN.png)

    5.  SharePoint 安装完成后，系统将进行更新，并开始进入下一个阶段，进行 SharePoint 的配置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2342_zh-CN.png)

        系统会在 MSSQL 中创建相应的数据库，并开放端口（9527）作为默认的管理端登录端口。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2343_zh-CN.png)

    6.  配置过程完成后，我们可以打开 [http://localhost:9527/default.aspx](http://localhost:9527/default.aspx) 进入 SharePoint 的管理站点。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2345_zh-CN.png)

        在管理员站点当中，我们可以创建自己的 Web application。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2347_zh-CN.png)

        在该网站集下，我们可以继续创建自己的站点。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2348_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2349_zh-CN.png)

        完成站点的创建后，我们就可以开始体验 SharePoint 上的功能了。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4440/2350_zh-CN.png)


