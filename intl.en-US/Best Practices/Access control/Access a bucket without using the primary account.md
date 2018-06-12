# Access a bucket without using the primary account {#concept_b12_1sz_5db .concept}

Assume that the user is a mobile developer and currently only has one bucket, ram-test-dev, for development, testing, and other functions. The user must stop using the primary account to access this bucket. This can avoid problems caused by AccessKey and password leaks. In the following example, replace AccessKey with your own AccessKey. The procedure is as follows:

1.  On the console, select **Products & Services \> Resource Access Management**.

    **Note:** The service must be activated first if you have never used it before.

2.  Click **Users** to go to the User Management page.
3.  The page shows that no user is created. Click **New User** on the upper right corner to create a subaccount with the same OSS access permissions as the primary account. Remember to select theAuto generate AccessKey for this user.
4.  The AccessKey for this account is generated and must be saved for later use.
5.  Return to User Managementinterface, which shows the newly created account named ram\_test.  When created, this subaccount does not have any permissions yet. Click the **Authorize** link on the right side and grant this subaccount full access permissions for OSS.

After authorization, click the **Management** link on the right side if you want to give the subaccount console logon or other permissions.

Now we can test the uploading and downloading operations. In the example, the AccessKey is ram\_test’s AccessKey. During the test, replace this with your own AccessKey.

```
$./osscmd get
oss://ram-test-dev/test.txt test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
100% The object test.txt is downloaded to test.txt, please check.
0.069(s) elapsed
```

```
$./osscmd put test.txt oss://ram-test-dev/test.txt --host=oss-cn-hangzhou.aliyuncs.com -i oOhue******Frogv -k OmVwFJO3qcT0******FhOYpg3p0KnA
100%
Object URL is: http://ram-test-dev.oss-cn-hangzhou.aliyuncs.com/test.txt
Object abstract path is: oss://ram-test-dev/test.txt
ETag is "E27172376D49FC609E7F46995E1F808F"
0.108(s) elapsed
```

As you can see, this subaccount can basically be used for all operations, so you can avoid leaking the primary account’s AccessKey.

