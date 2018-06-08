# What is RAM and STS {#concept_nsb_brz_5db .concept}

RAM and STS are permission management systems provided by Alibaba Cloud.

RAM is primarily used to control account system permissions.  RAM enables users to create subaccounts within the range of primary account permissions. Different subaccounts can be allocated different permissions for authorization management.

STS is a security credential \(token\) management system that grants temporary access permissions. STS allows users to grant access rights to the temporary accounts.

Why RAM and STS?

RAM and STS are designed to resolve the core issue such as how to securely grant access permissions to other users without disclosing the primary account’s AccessKey. Disclosure of AccessKey poses a serious security threat because unauthorized users may operate account resources and the risk of data leakage or stealing of important information is high.

RAM provides a long-term permission control mechanism. Various subaccounts assign different permissions to the different users. This way, even the disclosure of subaccount information would not cause a global information leakage. However, subaccounts have long-term validity. 

**Note:** Therefore, AccessKey of subaccounts must not be disclosed.

On the contrary, STS provides temporary access authorization by returning a temporary AccessKey and the token. This information can be provided directly to the temporary accounts, allowing them access to OSS. Generally, the permissions obtained from STS are more restrictive and only valid for a limited period of time. Thus, the disclosure of this information has little effect on the system.

These functions are further illustrated with the help of examples.

Basic concepts

The following are some explanations of the basic concepts:

-   Subaccount: A subaccount is created from the Alibaba Cloud primary accounts. Once created, it is assigned an independent password and permissions. Each subaccount has its own AccessKey and can perform authorized operations similar to the primary account.  Generally, subaccounts can be understood as users with certain permissions or operators with permissions to perform specific operations.
-   Role: Role is a virtual concept for certain operation permissions. However, it does not have independent logon passwords or AccessKeys.

    **Note:** Subaccounts can assume roles. When a role is assumed, the permissions granted for a subaccount are the permissions of the role.

-   Policy: Policies are rules used to define permissions; for example, they permit users to read or write certain resources.
-   Resource: Resources are the cloud resources that users can access like all OSS buckets, a certain OSS bucket, or a certain object in a specific OSS bucket.

A subaccount and roles have the same relationship to each other as you and your identities. At work, you may be an employee, while at home you may be a father. In different scenarios, you may assume different roles. Different roles are assigned corresponding permissions. The concept of “employee” or “father” is not an actual entity that can be the subject of actions. These concepts are only complete when an individual assumes them. This illustrates an important concept: a role may be assumed by multiple people at the same time.

**Note:** Once the role is assumed, this individual automatically obtains all the permissions of the role.

The following example provides better understanding of the concept:

-   Assume that Alice is the the Alibaba Cloud user and she has two private OSS buckets, alice\_a and alice\_b. Alice has full permission for both buckets.

-   To avoid leaking her Alibaba Cloud account AccessKey, which would pose a major security risk, Alice uses RAM to create two subaccounts, Bob and Carol. Bob has read/write permission for alice\_a and Carol has read/write permission for alice\_b.  Bob and Carol both have their own AccessKeys. This way, if one is leaked, only the corresponding bucket is affected and Alice can easily cancel the leaked user permissions on the console.

-   Now, for some reason, Alice must authorize another person to read the objects in alice\_a. In this situation, she must not only disclose Bob’s AccessKey. Rather, she can create a new role like AliceAReader, and grant this role the read permission for alice\_a.  However, note that, at this time, AliceAReader cannot be used because no AccessKey corresponds to this role. AliceAReader is currently only a virtual entity with the permission to access alice\_a.

-   To obtain temporary authorization, Alice can call the STS’s AssumeRole interface to notify STS that Bob wants to assume the AliceAReader role. If successful, STS returns a temporary AccessKeyId, AccessKeySecret, and SecurityToken, which serve as the access credentials.  When these credentials are given to a temporary account, the user obtains temporary permission to access alice\_a. The credentials’ expiration time is specified when the AssumeRole interface is called.


Why are RAM and STS so complex?

Initially, RAM and STS concepts seem to be complex. This is because flexibility is given to permission control at the cost of simplicity.

Subaccounts and roles are separated to separate the entity that executes operations from the virtual entity that represents a permissions set. If a user requires many permissions including the read and write permissions but each operation only requires part of the total permission set, you can create two roles, one with the read permission and the other with the write permission. Then create a user who does not have any permission but can assume these two roles. When the user needs to read or write data, the user can temporarily assume the role with the read permission or the role with the write permission. This reduces the risk of permission leaks for each operation.  Additionally, roles can be used to grant permissions to other Alibaba Cloud users, making the collaboration easier.

Here, flexibility does not mean you have to use all these functions. You only need to use the subset of the functions as required. For example, if you do not need to use temporary access credentials that have an expiration time, you can only use the RAM subaccount function, without STS.

In what follows, we use examples to create a RAM and STS user guide and provide instructions.  For the operations in these examples, we do our best to use console and command line operations to reduce the actual amount of codes that must be used. If you must use code to perform these operations, we recommend that you see the RAM and STS API Manual.

Test tool

During testing, we use osscmd, a tool in the OSS PythonSDK that allows you to directly work on OSS through the command line. osscmd can be obtained from [PythonSDK](../intl.en-US/Utilities/osscmd/Quick installation.md#).

Typical osscmd usage:

```
Download files
./osscmd get oss://BUCKET/OBJECT LOCALFILE --host=Endpoint -i AccessKeyId -k AccessKeySecret
Here, replace BUCKET and OBJECT with your own bucket and object, and the endpoint format must be similar to oss-cn-hangzhou.aliyuncs.com. For AccessKeyId and AccessKeySecret, use the information corresponding to your own account
Upload files
./osscmd put LOCALFILE oss://BUCKET/OBJECT --host=Endpoint -i AccessKeyId -k AccessKeySecret
The meaning of each field is the same as for the download example
```

