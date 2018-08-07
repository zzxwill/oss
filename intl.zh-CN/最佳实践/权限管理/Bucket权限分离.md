# Bucket权限分离 {#concept_r3r_qpf_vdb .concept}

再来考虑一种场景，已经有别的用户在使用开发的App，那么可以考虑用单独的Bucket来存储用户的App数据，假定为ram-test-app。那么考虑到权限隔离的问题，就不应该让应用服务器访问到ram-test-app，即仅授予ram\_test\_pub这个账号ram-test-dev的读权限。这也是可以通过RAM的权限系统来完成的。具体操作步骤如下：

1.  由于系统中没有Bucket级别的默认策略，因此需要自定义策略。

    这里Bucket访问的策略如下，更详细的信息请参考 [RAM策略的说明](../../../../intl.zh-CN/开发指南/访问与控制/访问控制.md#)和[OSS授权问题FAQ](../../../../intl.zh-CN/常见问题/对象存储 OSS 授权.md#)。

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
             "acs:oss:*:*:ram-test-dev",
             "acs:oss:*:*:ram-test-dev/*"
           ]
         }
       ]
     }
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4418/15336251756280_zh-CN.png)

    设置完成之后可以在自定义授权策略列表中查看。

2.  在用户的授权管理中将该策略加入授权范围，并且在**用户管理** \> **管理** \> **用户授权策略**中将之前授予的OSS全部可读权限解除。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4418/15336251756281_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4418/15336251756282_zh-CN.png)

3.  测试权限设置的有效性。

    -   访问ram-test-dev的Object成功：

        ```
        $./osscmd get oss://ram-test-dev/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
        100%  The object test.txt is downloaded to test.txt, please check.
        0.047(s) elapsed
        ```

    -   访问ram-test-app的Object失败：

        ```
        $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
         Error Headers:
         [('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '5646ED53F9EEA2F3324191A2'), ('date', 'Sat, 14 Nov 2015 08:14:11 GMT'), ('content-type', 'application/xml')]
         Error Body:
         <?xml version="1.0" encoding="UTF-8"?>
         <Error>
           <Code>AccessDenied</Code>
           <Message>AccessDenied</Message>
           <RequestId>5646ED53F9EEA2F3324191A2</RequestId>
           <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
         </Error>
         Error Status:
         403
         get Failed!
        ```

    -   上传文件到oss-test-app失败：

        ```
        $./osscmd put test.txt oss://ram-test-app/test.txt  --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA           
         100%  Error Headers:
         [('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '5646ED7BB8DE437A912DC7A8'), ('date', 'Sat, 14 Nov 2015 08:14:51 GMT'), ('content-type', 'application/xml')]
         Error Body:
         <?xml version="1.0" encoding="UTF-8"?>
         <Error>
           <Code>AccessDenied</Code>
           <Message>AccessDenied</Message>
           <RequestId>5646ED7BB8DE437A912DC7A8</RequestId>
           <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
         </Error>
         Error Status:
         403
         put Failed!
        ```

    通过上文的设置，就成功的将ram-test-dev和ram-test-app的权限完全区分开了。

    上面介绍的主要是如何使用子账号的权限控制功能来分割权限，将信息泄露造成的危害降到最低。

    如果用户有更复杂的权限控制需求，也可以参考[RAM用户指南](https://www.alibabacloud.com/help/doc-detail/28645.htm)。


