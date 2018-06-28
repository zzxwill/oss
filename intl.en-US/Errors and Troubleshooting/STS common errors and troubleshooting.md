# STS common errors and troubleshooting {#concept_irv_kjj_wdb .concept}

STS AssumeRole common errors and causes are listed as follows:

|SN|Error|Cause|
|:-|:----|:----|
|1|ErrorCode: NoPermission ErrorMessage: Roles may not be assumed by root accounts.|AssumeRole is called using the primary account’s key. Use the subaccount’s key.|
|2|Errorcode: missingsecuritytoken errormessage: securitytoken is mandatory for this action.|AssumeRole is called using the temporary account’s key. Use the subaccount’s key.|
|3|Error code: InvalidAccessKeyId.NotFound Error message: Specified access key is not found|The AccessKeyId is invalid. Check whether it is entered correctly. No spaces are left at both sides of AccessKeyId.|
|4|Error code: InvalidAccessKeyId.Inactive Error message: Specified access key is disabled.|The subaccount’s key used is disabled. Enable or replace the key. You can navigate to **Resource Access Management ** \> **User Management** \> **Management** \> **User Details** \> **User AccessKey** on the console to check whether the key is disabled and enable it.|
|5|ErrorCode: InvalidParameter.PolicyGrammar ErrorMessage: The parameter Policy has not passed grammar check.|The authorization policy specified during role play is invalid. You may specify or not specify an authorization policy for AssumeRole. If an authorization policy is specified, the permission for a temporary account is a combination of the specified authorization policy and the permissions for the role. If no authorization policy is specified, the permission for a temporary account is the permissions for the role. When this error is reported, check the specified authorization policy. It is not recommended that an authorization policy is specified when a temporary account assumes a role. If you do need a authorization policy , use the [RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html) to generate it.|
|6|ErrorCode: InvalidParameter.RoleSessionNameErrorMessage: The parameter RoleSessionName is wrongly formed.|The RoleSessionName specified for AssumeRole is invalid. This parameter is used to identify different tokens to indicate who is using a specific token, which facilitates audit. Format: ^\[a-zA-Z0-9.@-\_\]+$. The parameter has a length of 2 to 32 characters.For more information, see[Role assuming operation interface](https://www.alibabacloud.com/help/doc-detail/28763.htm). For example, the names like `a, 1, abc\*abc, and Teenage Mutant Ninja Turtles` are invalid.

|
|7|ErrorCode: InvalidParameter.DurationSeconds Error message: The Min/Max value of DurationSeconds is 15min/1hr.|When the role is assumed, the specified expiration time is invalid. In other words, the parameter AssumeRoleRequest.setDurationSeconds is invalid. When the role is assumed, the expiration time in seconds can be specified. The valid duration time is between 900 and 3600 seconds. For example, `assumeRoleRequest.setDurationSeconds(60L * 20)` means that it is valid within 20 minutes.|
|8|ErrorCode: NoPermissionErrorMessage: No permission perform sts:AssumeRole on this Role. Maybe you are not authorized to perform sts:AssumeRole or the specified role does not trust you| -   Cause 1: The subaccount of AssumeRole has no permissions. You must grant the subaccount an AliyunSTSAssumeRoleAccess system authorization policy. You must navigate to**Resource Access Management** \> **User Management** \> **Authorization** \> **Optional Authorization Policy Names**to grant the subaccount an AliyunSTSAssumeRoleAccess permission.
-   Cause 2: The account ID for the subaccount sending a request for assuming a role does not match the “Trusted Account ID” for the role. The role creator needs to confirm and modify the account ID. The account ID for the subaccount is the ID of the primary user who has created this subaccount. The account ID for the role is the account ID for the primary user who has created this role.  You must navigate to **Resource Access Management** \> **Role Management** \> **Management** \> **Role Details** \> **Editing Basic Information**to confirm and modify it.
-   Cause 3: The role type is incorrect. If the type of roles is classified into “User Role” and “Service Role”, the service role is not allowed to use AssumeRole to assume a temporary account. 

 |

**Note:** 

-   For examples of assuming the Java role, see [GitHub](https://github.com/baiyubin/aliyun-sts-java-sdk-demo).
-   For examples of assuming role due to other causes, see [STS SDK user guide.](https://yq.aliyun.com/articles/57895)

