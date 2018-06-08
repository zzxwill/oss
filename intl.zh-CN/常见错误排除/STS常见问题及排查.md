# STS常见问题及排查 {#concept_irv_kjj_wdb .concept}

STS AssumeRole（角色扮演）常见错误及原因如下：

|序号|错误|原因|
|:-|:-|:-|
|1|ErrorCode: NoPermission ErrorMessage: Roles may not be assumed by root accounts.|使用主用户的密钥调用AssumeRole，请使用子用户的密钥。|
|2|ErrorCode: MissingSecurityToken ErrorMessage: SecurityToken is mandatory for this action.|使用临时用户的密钥调用AssumeRole，请使用子用户的密钥。|
|3|Error code: InvalidAccessKeyId.NotFound Error message: Specified access key is not found|AccessKeyId无效，请检查是否写错，特别是前后不能有空格。|
|4|Error code: InvalidAccessKeyId.Inactive Error message: Specified access key is disabled.|使用的子用户的密钥，已经被禁止，请启用密钥或更换密钥。 密钥是否被禁止，请通过控制台的**访问控制** \> **用户管理** \> **管理** \> **用户详情** \> **用户AccessKey** 确认，并开启。|
|5|ErrorCode: InvalidParameter.PolicyGrammar ErrorMessage: The parameter Policy has not passed grammar check.|角色扮演时指定的授权策略无效。AssumeRole时可以指定授权（Policy），也可以不指定。如果指定授权策略，则临时用户的权限是指定的授权策略和角色权限的交集；如果不指定授权策略，临时用户的权限是角色的权限。报该错误时，请检查指定的授权策略Policy。不推荐临时用户扮演角色时指定授权策略。如果的确需要使用授权策略，强烈建使用 [RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html) 生成授权策略。|
|6|ErrorCode: InvalidParameter.RoleSessionNameErrorMessage: The parameter RoleSessionName is wrongly formed.|角色扮演时指定RoleSessionName无效。此参数用来区分不同的Token，以标明谁在使用此Token，便于审计。格式：^\[a-zA-Z0-9.@-\_\]+$，2-32个字符。了解更多请参看 [扮演角色操作接口](https://help.aliyun.com/document_detail/28763.html)。如命名`a，1，abc\*abc，忍者神龟`等都是非法的。

|
|7|ErrorCode: InvalidParameter.DurationSeconds Error message: The Min/Max value of DurationSeconds is 15min/1hr.|角色扮演时指定的过期时间无效，即AssumeRoleRequest.setDurationSeconds参数无效。角色扮演时可以指定过期时间，单位为秒，有效时间是900 ~ 3600，如`assumeRoleRequest.setDurationSeconds(60L * 20)`，20分钟内有效。|
|8|ErrorCode: NoPermissionErrorMessage: No permission perform sts:AssumeRole on this Role. Maybe you are not authorized to perform sts:AssumeRole or the specified role does not trust you| -   原因1：AssumeRole的子用户没有权限，请给子用户授予AliyunSTSAssumeRoleAccess系统授权策略。请在**访问控制** \> **用户管理** \> **授权** \> **可授权策略名称**中给子用户授权 AliyunSTSAssumeRoleAccess。
-   原因2：申请角色扮演的子用户的云账号ID与角色的“受信云账号ID”不符，请角色创建者确认并修改。子用户的云账号ID，即创建子用户的主用户的ID；角色的云账号ID，即创建角色的主用户的云账号ID。请在如下位置确认修改：**访问控制** \> **角色管理** \> **管理** \> **角色详情** \> **编辑基本信息**中确认修改。
-   原因3：角色的类型错误，如果角色的类型分为“用户角色”和“服务角色”，服务角色不能使用AssumeRole扮演临时用户。

 |

**说明：** 

-   Java角色扮演的示例，请参看 [GitHub](https://github.com/baiyubin/aliyun-sts-java-sdk-demo)。
-   其它的原因的角色扮演示例，请参考 [STS SDK使用手册](https://help.aliyun.com/document_detail/28791.html)。
-   您想更多了解阿里云访问控制（RAM），请参看 [阿里云访问控制初探](https://yq.aliyun.com/articles/57895) 。

