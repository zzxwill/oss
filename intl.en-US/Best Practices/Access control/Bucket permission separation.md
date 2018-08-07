# Bucket permission separation {#concept_r3r_qpf_vdb .concept}

Another scenario is introduced in this section. If another user is using the developed app, you can use an individual bucket to store your app data. Assume that the bucket is the ram-test-app. In consideration of permission separation, the application server must not be allowed to access the ram-test-app; that is, the account ram\_test\_pub is permitted only to read ram-test-dev. This can also be realized through the RAM permission system. The procedure is as follows:

1.  Because the system has no default bucket-level policy, we must create a custom policy.

    The bucket access policy is shown as follows. For more information, see [RAM Policy Description](../../../../intl.en-US/Developer Guide/Access and control/Access control.md#) and [OSS Authorization FAQs](../../../../intl.en-US/FAQ/Authorization for OSS instances.md#) .

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

    After setting, we can see the policy in the custom authorization policy list.

2.  In user authorization management, add this policy to the selected authorization policy list. Also in **Users** \> **Management** \> **Authorization policy**, all previously granted OSS read permissions can be revoked.
3.  Test the validity of permission configured.

    -   The object in ram-test-dev can be accessed:

        ```
        $./osscmd get oss://ram-test-dev/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
        100% The object test.txt is downloaded to test.txt, please check.
        0.047(s) elapsed
        ```

    -   The object in ram-test-app cannot be accessed:

        ```
        $./osscmd get oss://ram-test-app/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
         Error Headers:
         [('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '5646ED53F9EEA2F3324191A2'), ('date', 'Sat, 14 Nov 2015 08:14:11 GMT'), ('content-type', 'application/xml')]
         Error Body:
         <? xml version="1.0" encoding="UTF-8"? >
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

    -   Files cannot be uploaded to oss-test-app:

        ```
        $./osscmd put test.txt oss://ram-test-app/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA           
         100% Error Headers:
         [('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '5646ED7BB8DE437A912DC7A8'), ('date', 'Sat, 14 Nov 2015 08:14:51 GMT'), ('content-type', 'application/xml')]
         Error Body:
         <? XML version = "1.0" encoding = "UTF-8 "? >
         <Error>
           <Code>AccessDenied</Code>
           <Message>AccessDenied</Message>
           <RequestId>5646ED7BB8DE437A912DC7A8</RequestId>
           <HostId>ram-test-app.oss-cn-hangzhou.aliyuncs.com</HostId>
         </Error>
         Error status:
         403
         put Failed!
        ```

    Using the preceding configuration, we have successfully separated the permissions for ram-test-dev and ram-test-app.

    The preceding section explains how to use the subaccount permission control function to separate permissions and minimize the potential risk of information leakage.

    If you want to implement more complex access control, see [RAM User Guide](https://www.alibabacloud.com/help/doc-detail/28645.htm).


