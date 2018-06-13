# STS临时授权访问 {#concept_mgc_4tf_vdb .concept}

之前章节只用到了RAM的子账号功能，这些子账号都是可以长期正常使用的，发生泄露之后如果无法及时解除权限的话会很危险。

继续上文的例子，当开发者的app被用户使用之后，用户的数据要上传到OSS的ram-test-app这个Bucket，当app的用户数据很多的时候，需要考虑如何才能安全的授权给众多的app用户上传数据呢，以及如何保证多个用户之间存储的隔离。

类似这种需要临时访问的场景可以使用STS来完成。STS可以指定复杂的策略来对特定的用户进行限制，仅提供最小的权限。

## 创建角色 {#section_csx_hvf_vdb .section}

继续上一章节的例子，App用户有一个名为ram-test-app的Bucket来保存个人数据。创建角色的步骤如下：

1.  按照上文的流程创建一个子账号ram\_test\_app，不需要赋予任何权限，因为在扮演角色的时候会自动获得被扮演角色的所有权限。
2.  创建角色。这里创建两个角色，一个用于用户读取等操作，一个用于用户上传文件。
    -   打开访问控制的管理控制台，选择**角色管理** \> **新建角色**。
    -   选择角色类型。这里选择 用户角色。
    -   填写类型信息。因为角色是被阿里云账号使用过的，因此选择默认的即可。
    -   配置角色基本信息。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6284_zh-CN.png)
3.  创建完角色之后，角色是没有任何权限的，因此这里和上文所述一样需要新建一个自定义的授权策略。授权策略如下：

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
         "acs:oss:*:*:ram-test-app",
         "acs:oss:*:*:ram-test-app/*"
       ]
     }
    ]
    }
    ```

    表示对ram-test-app拥有只读权限。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6285_zh-CN.png)

4.  建立完成后，即可在角色管理里面给RamTestAppReadOnly添加上ram-test-app的只读授权。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6286_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6287_zh-CN.png)

5.  按照上文同样的方法，建立一个RamTestAppWrite的角色，并且赋予写ram-test-app的自定义授权，授权如下：

    ```
    {
    "Version": "1",
    "Statement": [
     {
       "Effect": "Allow",
       "Action": [
         "oss:DeleteObject",
         "oss:ListParts",
         "oss:AbortMultipartUpload",
         "oss:PutObject"
       ],
       "Resource": [
         "acs:oss:*:*:ram-test-app",
         "acs:oss:*:*:ram-test-app/*"
       ]
     }
    ]
    }
    ```

    目前新建的两个角色为：RamTestAppReadOnly和RamTestAppWrite，分别表示了对于ram-test-app的读写权限。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6288_zh-CN.png)


## 临时授权访问 {#section_qcm_tyf_vdb .section}

创建了角色之后，接下来就可以使用临时授权来访问OSS了。

准备工作

在正式使用之前，还有一些工作需要完成。扮演角色也是需要授权的，否则任意子账号都可以扮演这些角色会带来不可预计的风险，因此有扮演对应角色需求的子账号需要显式的配置权限。

1.  在授权管理策略中新建两个自定义的授权策略，分别如下：![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4419/6289_zh-CN.png)

    ```
    {
    "Statement": [
     {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Resource": "acs:ram::1894189769722283:role/ramtestappreadonly"
     }
    ],
    "Version": "1"
    }
    ```

    使用相同的方法创建另一个自定义授权策略：

    ```
    {
    "Statement": [
     {
       "Action": "sts:AssumeRole",
       "Effect": "Allow",
       "Resource": "acs:ram::1894189769722283:role/ramtestappwrite"
     }
    ],
    "Version": "1"
    }
    ```

    这里Resource后面填写的内容表示某个角色ID，角色的ID可以在**角色管理** \> **角色详情** 中找到。

2.  将这两个授权赋给ram\_test\_app这个账号。

使用STS授权访问

现在一切准备就绪，可以正式使用STS来授权访问了。

这里使用一个简单的STS的python命令行工具 [sts.py](https://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/internal/oss/0.0.4/assets/tool/sts.py?spm=a2c4g.11186623.2.4.RUB3Bg&file=sts.py)。 具体的调用方法如下：

```
$python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappreadonly RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:ListObjects","oss:GetObject"],"Resource":["acs:oss:*:*:ram-test-app","acs:oss:*:*:ram-test-app/*"]}]}' DurationSeconds=1000 --id=id --secret=secret
```

-   RoleArn表示的是需要扮演的角色ID，角色的ID可以在**角色管理** \> **角色详情** 中找到。
-   RoleSessionName是一个用来标示临时凭证的名称，一般来说建议使用不同的应用程序用户来区分。
-   Policy表示的是在扮演角色的时候额外加上的一个权限限制。
-   DurationSeconds指的是临时凭证的有效期，单位是s，最小为900，最大为3600。
-   id和secret表示的是需要扮演角色的子账号的AccessKey。

这里需要解释一下Policy，这里传入的Policy是用来限制扮演角色之后的临时凭证的权限。最后临时凭证获得的权限是角色的权限和这里传入的Policy的交集。

在扮演角色的时候传入Policy的原因是为了灵活性，比如上传文件的时候可以根据不同的用户添加对于上传文件路径的限制，这点会在下面的例子展示。

现在我们可以来实际试验一下STS的作用，作为试验用的Bucket，先在控制台向ram-test-app传入一个test.txt的文本，内容为ststest。

首先使用ram\_test\_app这个子账号直接来访问。请将下面的AccessKey换成自己试验用的AccessKey。

```
[admin@NGIS-CWWF344M01C /home/admin/oss_test]
$./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
Error Headers:
[('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A94D444F4D8B2225E4AFE'), ('date', 'Tue, 17 Nov 2015 02:45:40 GMT'), ('content-type', 'application/xml')]
Error Body:
<?xml version="1.0" encoding="UTF-8"?>
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
$./osscmd put test.txt  oss://ram-test-app/test.txt  --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
100%  Error Headers:
[('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A94E5B1119B445B9F8C3A'), ('date', 'Tue, 17 Nov 2015 02:45:57 GMT'), ('content-type', 'application/xml')]
Error Body:
<?xml version="1.0" encoding="UTF-8"?>
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

因为ram\_test\_app这个子账号没有访问权限，因此访问失败。

使用临时授权下载

现在使用STS来下载文件，这里为了简单，传入的Policy和角色的Policy一致，过期时间使用默认的3600s，App的用户假定为usr001。步骤如下：

1.  使用STS来获取临时的凭证。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappreadonly RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:ListObjects","oss:GetObject"],"Resource":["acs:oss:*:*:ram-test-app","acs:oss:*:*:ram-test-app/*"]}]}'  --id=oOhue******Frogv --secret=OmVwFJO3qcT0******FhOYpg3p0KnA
     https://sts.aliyuncs.com/?SignatureVersion=1.0&Format=JSON&Timestamp=2015-11-17T03%3A07%3A25Z&RoleArn=acs%3Aram%3A%3A1894189769722283%3Arole%2Framtestappreadonly&RoleSessionName=usr001&AccessKeyId=oOhuek56i53Frogv&Policy=%7B%22Version%22%3A%221%22%2C%22Statement%22%3A%5B%7B%22Effect%22%3A%22Allow%22%2C%22Action%22%3A%5B%22oss%3AListObjects%22%2C%22oss%3AGetObject%22%5D%2C%22Resource%22%3A%5B%22acs%3Aoss%3A%2A%3A%2A%3Aram-test-app%22%2C%22acs%3Aoss%3A%2A%3A%2A%3Aram-test-app%2F%2A%22%5D%7D%5D%7D&SignatureMethod=HMAC-SHA1&Version=2015-04-01&Signature=bshxPZpwRJv5ch3SjaBiXLodwq0%3D&Action=AssumeRole&SignatureNonce=53e1be9c-8cd8-11e5-9b86-008cfa5e4938
     {
       "AssumedRoleUser": {
         "Arn": "acs:ram::1894189769722283:role/ramtestappreadonly/usr001", 
         "AssumedRoleId": "317446347657426289:usr001"
       }, 
       "Credentials": {
         "AccessKeyId": "STS.3mQEbNf******wa180Le", 
         "AccessKeySecret": "B1w7rCbR4dzGwNYJ******3PiPqKZ3gjQhAxb6mB", 
         "Expiration": "2015-11-17T04:07:25Z", 
         "SecurityToken": "CAESvAMIARKAASQQUUTSE+7683CGlhdGsv2/di8uI+X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ///c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK+mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk="
       }, 
       "RequestId": "8C009F64-F19D-4EC1-A3AD-7A718CD0B49B"
     }
    ```

2.  使用临时凭证来下载文件，这里sts\_token就是上面STS返回的SecurityToken。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS.3mQEbNf******wa180Le -k B1w7rCbR4dzGwNYJ******3PiPqKZ3gjQhAxb6mB --sts_token=CAESvAMIARKAASQQUUTSE+7683CGlhdGsv2/di8uI+X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ///c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK+mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk=
     100%  The object test.txt is downloaded to test.txt, please check.
     0.061(s) elapsed
    ```

3.  可见已经可以使用临时凭证来下载文件了，那再试着使用这个凭证来上传。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd put test.txt oss://ram-test-app/test.txt  --host=oss-cn-hangzhou.aliyuncs.com -i STS.3mQEbNf******wa180Le -k B1w7rCbR4dzGwNYJ******3PiPqKZ3gjQhAxb6mB --sts_token=CAESvAMIARKAASQQUUTSE+7683CGlhdGsv2/di8uI+X1BxG7MDxM5FTd0fp5wpPK/7UctYH2MJ///c4yMN1PUCcEHI1zppCINmpDG2XeNA3OS16JwS6ESmI50sHyWBmsYkCJW15gXnfhz/OK+mSp1bYxlfB33qfgCFe97Ijeuj8RMgqFx0Hny2BzGhhTVFMuM21RRWJOZnR5Yzl1T3dhMTgwTGUiEjMxNzQ0NjM0NzY1NzQyNjI4OSoGdXNyMDAxMJTrgJ2RKjoGUnNhTUQ1QpsBCgExGpUBCgVBbGxvdxI4CgxBY3Rpb25FcXVhbHMSBkFjdGlvbhogCg9vc3M6TGlzdE9iamVjdHMKDW9zczpHZXRPYmplY3QSUgoOUmVzb3VyY2VFcXVhbHMSCFJlc291cmNlGjYKGGFjczpvc3M6KjoqOnJhbS10ZXN0LWFwcAoaYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwLypKEDE4OTQxODk3Njk3MjIyODNSBTI2ODQyWg9Bc3N1bWVkUm9sZVVzZXJgAGoSMzE3NDQ2MzQ3NjU3NDI2Mjg5chJyYW10ZXN0YXBwcmVhZG9ubHk=        
     100%  Error Headers:
     [('content-length', '254'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A9A2A1790CF0F53C15C82'), ('date', 'Tue, 17 Nov 2015 03:08:26 GMT'), ('content-type', 'application/xml')]
     Error Body:
     <?xml version="1.0" encoding="UTF-8"?>
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

    由于扮演的角色只有下载的权限，因此上传失败。


使用临时授权上传

现在可以来试验一下使用STS上传。步骤如下：

1.  获取STS的临时凭证，App用户为usr001。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $python ./sts.py AssumeRole RoleArn=acs:ram::1894189769722283:role/ramtestappwrite RoleSessionName=usr001 Policy='{"Version":"1","Statement":[{"Effect":"Allow","Action":["oss:PutObject"],"Resource":["acs:oss:*:*:ram-test-app/usr001/*"]}]}'  --id=oOhue******Frogv --secret=OmVwFJO3qcT0******FhOYpg3p0KnA
     https://sts.aliyuncs.com/?SignatureVersion=1.0&Format=JSON&Timestamp=2015-11-17T03%3A16%3A10Z&RoleArn=acs%3Aram%3A%3A1894189769722283%3Arole%2Framtestappwrite&RoleSessionName=usr001&AccessKeyId=oOhuek56i53Frogv&Policy=%7B%22Version%22%3A%221%22%2C%22Statement%22%3A%5B%7B%22Effect%22%3A%22Allow%22%2C%22Action%22%3A%5B%22oss%3APutObject%22%5D%2C%22Resource%22%3A%5B%22acs%3Aoss%3A%2A%3A%2A%3Aram-test-app%2Fusr001%2F%2A%22%5D%7D%5D%7D&SignatureMethod=HMAC-SHA1&Version=2015-04-01&Signature=Y0OPUoL1PrCqX4X6A3%2FJvgXuS6c%3D&Action=AssumeRole&SignatureNonce=8d0798a8-8cd9-11e5-9f49-008cfa5e4938
     {
       "AssumedRoleUser": {
         "Arn": "acs:ram::1894189769722283:role/ramtestappwrite/usr001", 
         "AssumedRoleId": "355407847660029428:usr001"
       }, 
       "Credentials": {
         "AccessKeyId":  "STS.rtfx13******NlIJlS4U", 
         "AccessKeySecret": "2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM", 
         "Expiration": "2015-11-17T04:16:10Z", 
         "SecurityToken": "CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl"
       }, 
       "RequestId": "19407707-54B2-41AD-AAF0-FE87E8870B0D"
     }
    ```

2.  试验一下能否使用这个凭证来上传下载。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     Error Headers:
     [('content-length', '254'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A9C31FFFC811F24B6E7E3'), ('date', 'Tue, 17 Nov 2015 03:17:05 GMT'), ('content-type', 'application/xml')]
     Error Body:
     <?xml version="1.0" encoding="UTF-8"?>
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
     $./osscmd put test.txt oss://ram-test-app/test.txt  --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     100%  Error Headers:
     [('content-length', '254'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '564A9C3FB8DE437A91B16772'), ('date', 'Tue, 17 Nov 2015 03:17:19 GMT'), ('content-type', 'application/xml')]
     Error Body:
     <?xml version="1.0" encoding="UTF-8"?>
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

    这里出现了问题，上传test.txt失败了。将本小节开始的时候传入的Policy格式化之后如下：

    ```
    {
         "Version": "1",
             "Statement": [
             {
                 "Effect": "Allow",
                 "Action": [
                     "oss:PutObject"
                     ],
                 "Resource": [
                     "acs:oss:*:*:ram-test-app/usr001/*"
                     ]
             }
         ]
     }
    ```

    这个Policy的意义是仅允许用户向ram-test-app这个Bucket上传类似usr001/的文件，如果App用户是usr002的时候，就可以修改Policy为仅允许上传类似usr002/这种类型的文件，通过这种对不同的App用户设定不同的Policy的方式，可以做到不同App用户之间拥有独立的存储空间互不干扰的目的。

3.  重新试验，将上传的目标指定为ram-test-app/usr001/test.txt。

    ```
    [admin@NGIS-CWWF344M01C /home/admin/oss_test]
     $./osscmd put test.txt oss://ram-test-app/usr001/test.txt  --host=oss-cn-hangzhou.aliyuncs.com -i STS.rtfx13******NlIJlS4U -k 2fsaM8E2maB2dn******wpsKTyK4ajo7TxFr0zIM --sts_token=CAESkwMIARKAAUh3/Uzcg13YLRBWxy0IZjGewMpg31ITxCleBFU1eO/3Sgpudid+GVs+Olvu1vXJn6DLcvPa8azKJKtzV0oKSy+mwUrxSvUSRVDntrs78CsNfWoOJUMJKjLIxdWnGi1pgxJCBzNZ2YV/6ycTaZySSE1V6kqQ7A+GPwYoBSnWmLpdGhhTVFMucnRmeDEzRFlNVWJjTmxJSmxTNFUiEjM1NTQwNzg0NzY2MDAyOTQyOCoGdXNyMDAxMOPzoJ2RKjoGUnNhTUQ1QnYKATEacQoFQWxsb3cSJwoMQWN0aW9uRXF1YWxzEgZBY3Rpb24aDwoNb3NzOlB1dE9iamVjdBI/Cg5SZXNvdXJjZUVxdWFscxIIUmVzb3VyY2UaIwohYWNzOm9zczoqOio6cmFtLXRlc3QtYXBwL3VzcjAwMS8qShAxODk0MTg5NzY5NzIyMjgzUgUyNjg0MloPQXNzdW1lZFJvbGVVc2VyYABqEjM1NTQwNzg0NzY2MDAyOTQyOHIPcmFtdGVzdGFwcHdyaXRl
     100%  
     Object URL is: http://ram-test-app.oss-cn-hangzhou.aliyuncs.com/usr001%2Ftest.txt
     Object abstract path is: oss://ram-test-app/usr001/test.txt
     ETag is "946A0A1AC8245696B9C6A6F35942690B" 
     0.071(s) elapsed
    ```

    可见上传成功了。


## 总结 {#section_zf1_3dg_vdb .section}

本章主要介绍了使用STS来临时授权用户访问OSS。在典型的移动开发场景中，使用STS可以做到不同的App用户需要访问App的时候，可以通过获取到的临时授权来访问OSS。临时授权可以指定过期时间，因此大大降低了泄露的危害。在获取临时授权的时候，可以根据App用户的不同，传入不同的授权策略来限制用户的访问权限，比如限制用户访问的Object路径，从而达到隔离不同App用户的存储空间的目的。

