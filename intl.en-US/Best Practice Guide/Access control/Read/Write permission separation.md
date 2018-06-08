# Read/Write permission separation {#concept_fs4_zsz_5db .concept}

When the users want to use an application server to provide external service, OSS can store back-end static resources. In this case, we recommend that the application server be granted the OSS read-only permission to reduce the risk of attacks. The read and write permission separation can be configured to grant the application server a user with the read-only permission.

1.  Create an account ram\_test\_pub. As shown in the following figure, select ReadOnly in the authorization management area:
2.  You can now use the AccessKey of the subaccount to test the upload and download permissions.  The AccessKey here is a ram\_test\_pub AccessKey and is to be replaced with your own AccessKey during the test.

```
$./osscmd get oss://ram-test-dev/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
100% The object test.txt is downloaded to test.txt, please check.
0.070(s) elapsed
```

```
$. /Osscmd put test.txt OSS: // Ram-test-dev/test.txt -- Host = porteroohue ****** frogv-K OmVwFJO3qcT0 * FhOYpg3p0KnA?           
100% Error Headers:
[('content-length', '229'), ('server', 'AliyunOSS'), ('connection', 'keep-alive'), ('x-oss-request-id', '5646E49C1790CF0F531BAE0D'), ('date', 'Sat, 14 Nov 2015 07:37:00 GMT'), ('content-type', 'application/xml')]
Error Body:
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>AccessDenied</Code>
  <Message>AccessDenied</Message>
  <RequestId>5646E49C1790CF0F531BAE0D</RequestId>
  <HostId>ram-test-dev.oss-cn-hangzhou.aliyuncs.com</HostId>
</Error>
Error Status:
403
put Failed!
```

With reference to the preceding example, we can conclude that the ram\_test\_pub account cannot be used to upload files.

