# STS临时授权访问OSS {#concept_xzh_nzk_2gb .concept}

OSS 可以通过阿里云 STS \(Security Token Service\) 进行临时授权访问。阿里云 STS 是为云计算用户提供临时访问令牌的Web服务。通过 STS，您可以为第三方应用或子用户（即用户身份由您自己管理的用户）颁发一个自定义时效和权限的访问凭证。

**说明：** 关于STS的更多信息，请参见[RAM和STS介绍](https://help.aliyun.com/document_detail/102082.html)。

## 使用场景 {#section_f3l_11l_2gb .section}

对于您本地身份系统所管理的用户，比如您的 App 的用户、您的企业本地账号、第三方 App ，将这部分用户称为联盟用户。这些联盟用户可能需要直接访问 OSS 资源。此外，还可以是您创建的能访问您的阿里云资源应用程序的用户。

对于这部分联盟用户，通过阿里云 STS 服务为阿里云账号（或 RAM 用户）提供短期访问权限管理。您不需要透露云账号（或 RAM 用户）的长期密钥（如登录密码、AccessKey），只需要生成一个短期访问凭证给联盟用户使用即可。这个凭证的访问权限及有效期限都可以由您自定义。您不需要关心权限撤销问题，访问凭证过期后会自动失效。

通过 STS 生成的凭证包括安全令牌（SecurityToken）、临时访问密钥（AccessKeyId, AccessKeySecret）。使用AccessKey 方法与您在使用阿里云账户或 RAM 用户 AccessKey 发送请求时的方法相同。需要注意的是在每个向 OSS 发送的请求中必须携带安全令牌。

## 实现原理 {#section_ssr_k1l_2gb .section}

以一个移动 App 举例。假设您是一个移动 App 开发者，打算使用阿里云 OSS 服务来保存App 的终端用户数据，并且要保证每个 App 用户之间的数据隔离，防止一个 App 用户获取到其它 App 用户的数据。你可以使用 STS 授权用户直接访问 OSS。

使用 STS 授权用户直接访问 OSS 的流程如下：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4347/1547114966983_zh-CN.png)

1.  App 用户登录。App 用户和云账号无关，它是 App 的终端用户，AppServer 支持 App 用户登录。对于每个有效的 App 用户来说，需要 AppServer 能定义出每个 App 用户的最小访问权限。
2.  AppServer 请求 STS 服务获取一个安全令牌（SecurityToken）。在调用 STS 之前，AppServer 需要确定 App 用户的最小访问权限（用 Policy 语法描述）以及授权的过期时间。然后通过扮演角色（AssumeRole）来获取一个代表角色身份的安全令牌。
3.  STS 返回给 AppServer 一个有效的访问凭证，包括一个安全令牌（SecurityToken）、临时访问密钥（AccessKeyId, AccessKeySecret）以及过期时间。
4.  AppServer 将访问凭证返回给 ClientApp。ClientApp 可以缓存这个凭证。当凭证失效时，ClientApp 需要向 AppServer 申请新的有效访问凭证。比如，访问凭证有效期为1小时，那么 ClientApp 可以每 30 分钟向 AppServer 请求更新访问凭证。
5.  ClientApp 使用本地缓存的访问凭证去请求 Aliyun Service API。云服务会感知 STS 访问凭证，并会依赖 STS 服务来验证访问凭证，正确响应用户请求。

STS 安全令牌、角色管理和使用相关内容详情，请参考 [RAM 角色管理](../../../../../intl.zh-CN/用户指南/身份管理/RAM 角色身份/理解 RAM 角色.md#)。调用 STS 服务接口[AssumeRole](../../../../../intl.zh-CN/API 参考（STS）/操作接口/AssumeRole.md#)来获取有效访问凭证即可。

## 操作步骤 {#section_t4q_4zr_ggb .section}

假设有一个名为 ram-test 的 Bucket 用于存储用户数据，现将利用用户子账号结合 STS 实现 OSS 权限控制访问。

您可以通过 OSS SDK 与 STS SDK 的结合使用，实现使用 STS 临时授权访问 OSS 实例。

1.  创建用户子账号。
    1.  登录 [RAM 访问控制管理控制台](https://ram.console.aliyun.com)。
    2.  在RAM 访问控制页面，单击**用户**。
    3.  在用户页面，单击**新建用户**。
    4.  在新建用户页面，用户账号信息填写**登录名称**、**显示名称**，访问方式下勾选**编程访问**，并单击**确定**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735383_zh-CN.jpg)

    5.  单击**权限管理** \> **添加权限**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735405_zh-CN.jpg)

    6.  在添加权限页面，为已创建子账号添加**AliyunSTSAssumeRoleAccess** 权限。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735411_zh-CN.jpg)

        **说明：** 尽量不要赋予子账号其他任意权限，因为在扮演角色的时候会自动获得被扮演角色的所有\(部分\)权限。

2.  创建权限策略。
    1.  登录 [RAM 访问控制管理控制台](https://ram.console.aliyun.com)。
    2.  在RAM 访问控制页面，单击**权限策略管理**。
    3.  单击**新建权限策略**。
    4.  在新建自定义权限策略页面，填写**策略名称**、**备注**，配置模式选择**可视化配置**或**脚本配置**。

        以脚本配置为例，对 ram-test 添加 ListObjects 与 GetObject 等只读权限，在**策略内容**中配置脚本如下：

        ```
        {
        	"Version": "1",
        	"Statement": [
         	{
           		"Effect": "Allow",
           		"Action": [
             		"oss:ListObjects",
             		"oss:GetObject"
           		],
           		"Resource": [
             		"acs:oss:*:*:ram-test",
             		"acs:oss:*:*:ram-test/*"
           		]
         	}
        	]
        }
        ```

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735423_zh-CN.jpg)

3.  创建角色。
    1.  登录 [RAM 访问控制管理控制台](https://ram.console.aliyun.com)。
    2.  在RAM 访问控制页面，单击**RAM 角色管理**。
    3.  在 RAM 角色管理页面，单击**新建 RAM 角色**。
    4.  在新建 RAM 角色页面，填写**RAM 角色名称**，本示例 RAM 角色名称为 RamOssTest，选择可信实体类型及受信云账号 ID 保留默认选项。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735434_zh-CN.jpg)

    5.  单击已创建 RAM 角色 RamOssTest 右侧对应的**添加权限** 。
    6.  在添加权限页面，选择**自定义权限策略**，添加步骤 2 中创建的策略 Ramtest。

        添加策略后，页面如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80860/154711496735437_zh-CN.jpg)

        **说明：** ARN 代表需要扮演角色的 ID。

4.  通过 STS API 获取 STS AK 与 SecurityToken。

    通过调用 STS SDK 请求 STS 服务获取一个安全令牌。STS SDK 的安装及使用详见[STS Java SDK安装及使用](../../../../../intl.zh-CN/SDK参考/SDK参考（STS）/Java SDK/安装.md#)。

    以 STS Java SDK 为例：

    ```
    public class StsServiceSample {
        public static void main(String[] args) {
            String endpoint = "sts.aliyuncs.com";
            String accessKeyId = "<access-key-id>";
            String accessKeySecret = "<access-key-secret>";
            String roleArn = "<role-arn>";
            String roleSessionName = "session-name";
            String policy = "{\n" +
                    "    \"Version\": \"1\", \n" +
                    "    \"Statement\": [\n" +
                    "        {\n" +
                    "            \"Action\": [\n" +
                    "                \"oss:*\"\n" +
                    "            ], \n" +
                    "            \"Resource\": [\n" +
                    "                \"acs:oss:*:*:*\" \n" +
                    "            ], \n" +
                    "            \"Effect\": \"Allow\"\n" +
                    "        }\n" +
                    "    ]\n" +
                    "}";
            try {
                // 添加endpoint（直接使用STS endpoint，前两个参数留空，无需添加region ID）
                DefaultProfile.addEndpoint("", "", "Sts", endpoint);
                // 构造default profile（参数留空，无需添加region ID）
                IClientProfile profile = DefaultProfile.getProfile("", accessKeyId, accessKeySecret);
                // 用profile构造client
                DefaultAcsClient client = new DefaultAcsClient(profile);
                final AssumeRoleRequest request = new AssumeRoleRequest();
                request.setMethod(MethodType.POST);
                request.setRoleArn(roleArn);
                request.setRoleSessionName(roleSessionName);
                request.setPolicy(policy); // 若policy为空，则用户将获得该角色下所有权限
                request.setDurationSeconds(1000L); // 设置凭证有效时间
                final AssumeRoleResponse response = client.getAcsResponse(request);
                System.out.println("Expiration: " + response.getCredentials().getExpiration());
                System.out.println("Access Key Id: " + response.getCredentials().getAccessKeyId());
                System.out.println("Access Key Secret: " + response.getCredentials().getAccessKeySecret());
                System.out.println("Security Token: " + response.getCredentials().getSecurityToken());
                System.out.println("RequestId: " + response.getRequestId());
            } catch (ClientException e) {
                System.out.println("Failed：");
                System.out.println("Error code: " + e.getErrCode());
                System.out.println("Error message: " + e.getErrMsg());
                System.out.println("RequestId: " + e.getRequestId());
            }
        }
    }
    ```

    参数说明如下：

    -   AccessKeyId、AccessKeySecret：子账号 AK 信息。
    -   RoleArn：需要扮演的角色 ID。
    -   RoleSessionName：用来标识临时凭证的名称，建议使用不同的应用程序用户来区分。
    -   Policy：在扮演角色的时候额外添加的权限限制。

        **说明：** 这里传入的 Policy 是用来限制扮演角色之后的临时凭证的权限。临时凭证最后获得的权限是角色的权限和这里传入的 Policy 的交集。扮演角色的时候传入Policy的原因是为了灵活性，比如上传文件的时候可以根据不同的用户添加对于上传文件路径的限制等。

    -   DurationSeconds：设置临时凭证的有效期，单位是 s，最小为 900，最大为 3600。
5.  通过 STS AK 与 SecurityToken 访问 OSS。

    获取 STS AK 与 SecurityToken 后，您可以使用 STS 凭证构造签名请求。

    以下是 Java SDK 的示例，其他示例请参见[Python](../../../../../intl.zh-CN/SDK 参考/Python/授权访问.md#section_zx1_55k_kfc)、[PHP](../../../../../intl.zh-CN/SDK 参考/PHP/授权访问.md#section_m2g_jwr_kfc)、[Go](../../../../../intl.zh-CN/SDK 参考/Go/授权访问.md#section_ygd_qxw_kfc) SDK 授权访问的“使用 STS 进行临时授权”一节。

    ```
    // Endpoint以杭州为例，其它Region请按实际情况填写。
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // 阿里云主账号AccessKey拥有所有API的访问权限，风险很高。强烈建议您创建并使用RAM账号进行API访问或日常运维，请登录 https://ram.console.aliyun.com 创建RAM账号。
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String securityToken = "<yourSecurityToken>";
    
    // 用户拿到STS临时凭证后，通过其中的安全令牌（SecurityToken）和临时访问密钥（AccessKeyId和AccessKeySecret）生成OSSClient。
    // 创建OSSClient实例。注意，这里使用到了上一步生成的STS凭证
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret, securityToken);
    
    // OSS操作。
    
    // 关闭OSSClient。
    ossClient.shutdown();
    ```


