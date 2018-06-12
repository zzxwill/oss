# STS temporary access authorization {#concept_mgc_4tf_vdb .concept}

In the previous documents, we used only the RAM user functions. These user accounts are for long-term normal use. This poses as a serious risk if the RAM user permissions cannot be promptly revoked in case of information leakage.

In the previous example, assume that our developer’s app allows users to upload data to the OSS bucket am-test-app and currently, the number of app users is large. In this case, how can the app securely grant data upload permissions to many users and how can it be certain of storage isolation among multiple users?

In such scenarios, we need to grant users temporary access using STS.  STS can be used to specify a complex policy that restricts specified users by only granting them the minimum necessary permissions.

## Create a role {#section_csx_hvf_vdb .section}

Based on the example in the previous document, the app user has a bucket, ram-test-app, to store personal data.  A role can be created as follows:

1.  Create a RAM user account named ram\_test\_app using the process illustrated in the previous documents. Do not grant this account any permissions, because it inherits the permissions of a role which it assumes.
2.  Create roles. Here you must create two roles for users to perform read operations and to upload files respectively.
    -   Log on to the RAM console and select **Roles** \> **Create Role**.
    -   Select a role type.  Here you must select User role.
    -   Enter the role type information. Because this role has been used by its own Alibaba Cloud account. Use the default setting.
    -   Configure basic role information.

        ![](images/1921_en-US.png)

3.  When the role was created, it did not have any permissions. Therefore, we must create a custom authorization policy using the process described earlier.   The following is the authorization policy:

    ```
    
    "Version": "1",
    "Statement": [
     
       "Effect": "Allow",
       "Action": [
         "oss:ListObjects",
         "oss:GetObject"
       
       "Resource": [
         "acs:oss:*:*:ram-test-app",
         "acs:oss:*:*:ram-test-app/*"
       
     
    
    
    ```

    This indicates read-only permission for ram-test-app.

    ![](images/1923_en-US.png)

4.  After the policy is established, give the role RamTestAppReadOnly the ram-test-app read-only permission on the role management page.

    ![](images/1924_en-US.png)

    ![](images/1925_en-US.png)

5.  Perform the same procedure to create the role RamTestAppWrite and use a custom authorization policy to grant ram-test-app write permission. The authorization policy is as follows:

    ```
    
    "Version": "1",
    "Statement": [
     
       "Effect": "Allow",
       "Action": [
         "oss:DeleteObject",
         "oss:ListParts",
         "oss:AbortMultipartUpload",
         "oss:PutObject"
       
       "Resource": [
         "acs:oss:*:*:ram-test-app",
         "acs:oss:*:*:ram-test-app/*"
       
     
    
    
    ```

    Now we have created two roles, RamTestAppReadOnly and RamTestAppWrite, with read-only and write permissions for ram-test-app, respectively.

    ![](images/1926_en-US.png)


## Temporary access authorization {#section_qcm_tyf_vdb .section}

After creating roles, we can use them to grant temporary access to OSS.

Preparation

Authorization is required for assuming roles. Otherwise, any RAM user could assume these roles, which can lead to unpredictable risks. Therefore, to assume corresponding roles, a RAM user needs to have explicitly configured permissions.

1.  Create two custom authorization policies in authorization policy management.

    ![](images/1934_en-US.png)

    ```
    
    "Statement": [
     
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Resource": "acs:ram::1894189769722283:role/ramtestappreadonly"
     
    
    "Version": "1"
    
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/1941_en-US.png)

    ```
    
    "Statement": [
     
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Resource": "acs:ram::1894189769722283:role/ramtestappwrite"
     
    
    "Version": "1"
    
    ```

    Here, the content entered after Resource is a role’s ID. Role IDs can be found in**Roles** \> **Role Details** .

    ![](images/1946_en-US.png)

2.  Grant the two authorization policies to the account ram\_test\_app.

    ![](images/1953_en-US.png)


Use STS to grant access permissions

Now, we are ready with the platform to officially use STS to grant access permissions.

Here we use a simple STS Python command line tool [sts.py](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/tool/sts.py?spm=a2c4g.11186623.2.4.RUB3Bg&file=sts.py). The calling method is as follows:

```
$python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappreadonly RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:ListObjects","oss:GetObject"],"Resource":["acs:oss:*:*:ram-test-app","acs:oss:*:*:ram-test-app/*"]}]}' DurationSeconds=1000 --id=id --secret=secret
```

-   RoleArn: indicates the ID of a role to be assumed. Role IDs can be found in**Roles** \> **Role Details** .
-   RoleSessionName: indicates the name of the temporary credentials. Generally, we recommend that you separate this using different application users.
-   Policy: indicates a permission restriction, which is added when the role is assumed.
-   DurationSeconds: indicate the validity time of the temporary credentials in seconds. The minimum value is 900, and the maximum value is 3600.
-   id and secret: indicate the AccessKey of the RAM user to assume a role.

Here, we need to explain what is meant by “Policy”. The policy mentioned here is used to restrict the temporary credential permissions after a role is assumed. Ultimately, the permissions obtained by means of temporary credentials are overlapping permissions of the role and the policy passed in.

When a role is assumed, a policy can be entered to increase the flexibility. For example, when uploading the files, we can add different upload path restrictions for different users. This is shown in the following example.

Now, let’s test the STS function. To test the bucket, first use the console to put the file test.txt in ram-test-app, with the content ststest.

Firstly, use the RAM user account ram\_test\_app to directly access the file. Next, replace AccessKey with your own AccessKey used in the test.

```
[admin@NGIS-CWWF344M01C /home/admin/oss_test]
$./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
Error Headers:
[('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A94D444F4D8B2225E4AFE'), ('date', 'Tue, 17 Nov 2015 02:45:40 GMT'), ('content-type', 'application/xml')]
Error Body:
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>AccessDenied</Code>
  <Message>AccessDenied</Message>
  <RequestId>564A94D444F4D8B2225E4AFE</RequestId>
  <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
Error Status:
403
get Failed!
[admin@NGIS-CWWF344M01C /home/admin/oss_test]
$./osscmd put test.txt oss://ram-test-app/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
100% Error Headers:
[('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A94E5B1119B445B9F8C3A'), ('date', 'Tue, 17 Nov 2015 02:45:57 GMT'), ('content-type', 'application/xml')]
Error Body:
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>AccessDenied</Code>
  <Message>AccessDenied</Message>
  <RequestId>564A94E5B1119B445B9F8C3A</RequestId>
  <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
Error Status:
403
put Failed!
```

Without access permission, access attempts using the RAM user account ram\_test\_app are failed.

Use temporary authorization for downloads

Now, we use STS to download files. To make it simple to understand, the entered policy and the role policy are the same. The expiration time is set to 3600s, and the app user here is usr001. The steps are as follows:

1.  Use STS to obtain a temporary credential.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappreadonly RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:ListObjects","oss:GetObject"],"Resource":["acs:oss:*:*:ram-test-app","acs:oss:*:*:ram-test-app/*"]}]}' --id=oOhue******Frogv --secret=OmVwFJO3qcT0******FhOYpg3p0KnA
     Https://sts.aliyuncs.com /? Signatureversion = 1.0 & format = JSON & timestamp = glas% 3a07% 3a25z & rolearn = ACS % 3-Aram % 3A % 3A1894189769722283% 3 Arole % 2 framtestappreadonly & RoleSessionName = usr001 & the Access Key ID = oOhuek56i53Frogv & Policy = % 7b % 22 version % 22% 3A % 221% 22% 2C % 22 Statement % 22% 3A % 1B % 7b % 22 effect % 22% 3A % 22 allow % 22% 2C % 22 action % 22% 3A % 1B % 22oss % 3 alistobjects % 22% 2C % 22oss % 3 agetobject % 22% 5D % 2C % 22 Resource % 22% 3A % 4B % 22acs % 3 aoss % 3A % 2a % 3A % 2a % fig % 22% 2C % 22acs % 3 aoss % 3A % 2a % 3A % 2a % 3aram-test-app % Sch % 2a % 22% 5d % 7D % 5d % 7D & seaguremethod = HMAC-SHA1 & Version = 2015-04-01 & Signature = bshxPZpwRJv5ch3SjaBiXLodwq0 % 3D & Action = AssumeRole & signaturenonce = 53e1be9c-8cd8-11e5-9b86-008cfa5e4938
     
       "AssumedRoleUser": {
         "Arn": "acs:ram::1894189769722283:role/ramtestappreadonly/usr001", 
         "AssumedRoleId": "317446347657426289:usr001"
        
       "Credentials": {
         "AccessKeyId": "STS. 3mQEbNf******wa180Le", 
         "AccessKeySecret": "B1w7rCbR4dzGwNYJ******3PiPqKZ3gjQhAxb6mB", 
         "Expiration": "2015-11-17T04:07:25Z", 
         "SecurityToken": "CAESvAMIARKAASQQUUTSE+7683CGlhdGsv2/di8uI+X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ///c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK+mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk="
        
       "RequestId": "8C009F64-F19D-4EC1-A3AD-7A718CD0B49B"
     
    ```

2.  Use the temporary credential to download files. Here, sts\_token is the SecurityToken returned by the STS.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS. 3mqebnf *** wa180le-k*** *** 3pipqkz3gjqhaxb6mb *** -- sts_token = caesvamiarkaasqquutse + 7683CGlhdGsv2/di8uI + X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ/c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK + mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk =
     100% The object test.txt is downloaded to test.txt, please check.
     0.061(s) elapsed
    ```

3.  As you can see, we can use the temporary credentials to download the file. Next, we test if we can use them to upload a file.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd put test.txt oss://ram-test-app/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS. 3mQEbNf******wa180Le -k B1w7rCbR4dzGwNYJ******3PiPqKZ3gjQhAxb6mB --sts_token=CAESvAMIARKAASQQUUTSE+7683CGlhdGsv2/di8uI+X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ///c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK+mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk=        
     100% Error Headers:
     [('Content-Length', '254'), ('server ', 'aliyunoss '), ('Connection', 'keep-alive '), ('x-OSS-request-id', '564a9a2a1790cf0f53c15c82 '), ('date', 'tue, 17 2015 03:08:26 GMT '), ('content-type', 'application/XML ')]
     Error Body:
     <? xml version="1.0" encoding="UTF-8"? >
     <Error>
       <Code>AccessDenied</Code>
       <Message>Access denied by authorizer's policy.</Message>
       <RequestId>564A9A2A1790CF0F53C15C82</RequestId>
       <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
     </Error>
     Error Status:
     403
     put Failed!
    ```

    The file upload is failed. This is because the assumed role only has download permission hence.


Use temporary authorization for uploads

Now, we try to use STS to upload a file. The steps are as follows:

1.  Obtain an STS temporary credential. The app user is usr001.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappwrite RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:PutObject"],"Resource":["acs:oss:*:*:ram-test-app/usr001/*"]}]}' --id=oOhue******Frogv --secret=OmVwFJO3qcT0******FhOYpg3p0KnA
     https://sts.aliyuncs.com/?SignatureVersion=1.0&Format=JSON&Timestamp=2015-11-17T03%3A16%3A10Z&RoleArn=acs%3Aram%3A%3A1894189769722283%3Arole%2Framtestappwrite&RoleSessionName=usr001&AccessKeyId=oOhuek56i53Frogv&Policy=%7B%22Version%22%3A%221%22%2C%22Statement%22%3A%5B%7B%22Effect%22%3A%22Allow%22%2C%22Action%22%3A%5B%22oss%3APutObject%22%5D%2C%22Resource%22%3A%5B%22acs%3Aoss%3A%2A%3A%2A%3Aram-test-app%2Fusr001%2F%2A%22%5D%7D%5D%7D&SignatureMethod=HMAC-SHA1&Version=2015-04-01&Signature=Y0OPUoL1PrCqX4X6A3%2FJvgXuS6c%3D&Action=AssumeRole&SignatureNonce=8d0798a8-8cd9-11e5-9f49-008cfa5e4938
     
       "AssumedRoleUser": {
         "Arn": "acs:ram::1894189769722283:role/ramtestappwrite/usr001", 
         "AssumedRoleId": "355407847660029428:usr001"
        
       "Credentials": {
         "AccessKeyId": "STS.rtfx13******NlIJlS4U", 
         "AccessKeySecret": "2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM", 
         "Expiration": "2015-11-17T04:16:10Z", 
         "SecurityToken": "CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl"
        
       "RequestId": "19407707-54B2-41AD-AAF0-FE87E8870B0D"
     
    ```

2.  Let us test if we can use the credentials to upload and download.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     Error Headers:
     [('content-length', '254'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A9C31FFFC811F24B6E7E3'), ('date', 'Tue, 17 Nov 2015 03:17:05 GMT'), ('content-type', 'application/xml')]
     Error Body:
     <? xml version="1.0" encoding="UTF-8"? >
     <Error>
       <Code>AccessDenied</Code>
       <Message>Access denied by authorizer's policy.</Message>
       <RequestId>564A9C31FFFC811F24B6E7E3</RequestId>
       <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
     </Error>
     Error Status:
     403
     get Failed!
     [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd put test.txt oss://ram-test-app/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     100% Error Headers:
     [('content-length', '254'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A9C3FB8DE437A91B16772'), ('date', 'Tue, 17 Nov 2015 03:17:19 GMT'), ('content-type', 'application/xml')]
     Error Body:
     <? xml version="1.0" encoding="UTF-8"? >
     <Error>
       <Code>AccessDenied</Code>
       <Message>Access denied by authorizer's policy.</Message>
       <RequestId>564A9C3FB8DE437A91B16772</RequestId>
       <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
     </Error>
     Error Status:
     403
     put Failed!
    ```

    The test.txt upload fails. We have formatted the entered policy discussed at the beginning of this document, which is as follows:

    ```
    
         "Version": "1",
             "Statement": [
             
                 "Effect": "Allow",
                 "Action": [
                     "oss:PutObject"
                     
                 "Resource": [
                     "acs:oss:*:*:ram-test-app/usr001/*"
                     
             
         
     
    ```

    This policy indicates that users are only allowed to upload files like usr001/ to the ram-test-app bucket. If the app user is usr002, the policy can be changed to only allow for the uploading of files like usr002/. By setting different policies for different app users, we can isolate the storage space of different app users.

3.  Retry the test and specify the upload destination as ram-test-app/usr001/test.txt.

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd put test.txt oss://ram-test-app/usr001/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     100%  
     Object URL is: http://ram-test-app.oss-cn-hangzhou.aliyuncs.com/usr001%2Ftest.txt
     Object abstract path is: oss://ram-test-app/usr001/test.txt
     ETag is "946A0A1AC8245696B9C6A6F35942690B" 
     0.071(s) elapsed
    ```

    The upload is successful.


## Summary {#section_zf1_3dg_vdb .section}

This document describes how to grant users temporary access authorization for OSS using STS.  In typical mobile development scenarios, STS can be used to grant temporary authorizations to access OSS when different app users need to access the app. The temporary authorization can be configured with expiration time to greatly reduce the hazards caused by leakages. When obtaining temporary authorization, we can enter different authorization policies for different app users to restrict their access permissions. For example, to restrict the object paths accessible to users. This isolates the storage space of different app users.

