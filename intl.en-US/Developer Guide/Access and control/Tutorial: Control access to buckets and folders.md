# Tutorial: Control access to buckets and folders {#concept_rf2_251_5db .concept}

This tutorial explains how to grant and control user access to OSS buckets and folders.  In this tutorial, we create a bucket with folders, and then create Resource Access Management \(RAM\)  users in your Alibaba Cloud primary account and grant these users incremental permissions to access your OSS bucket and its folders.

## Buckets and folders {#section_ppc_k13_5db .section}

The data model structure of Alibaba Cloud OSS is flat instead of hierarchical. All objects \(files\) are directly related to their corresponding buckets.  Therefore, OSS lacks the hierarchical structure of directories and subfolders as in a file system.  However, you can  emulate a folder hierarchy on the OSS console,  where you can arrange and manage files by folders, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/3896_en-US.png)

OSS is a distributed object storage service that uses a key-value pair format.  Users retrieve the content of an object based on its unique key \(object name\).  For example, the bucket named **example-company**  has three folders: **Development**, **Marketing** and **Private**. This bucket also has one object  **oss-dg.pdf**.

-   When you create the **Development** folder, the console creates an object with the key `Development/` .  Note that the key of a folder includes the delimiter `/`.
-   When you upload an object named **ProjectA.docx** into the **Development** folder, the console uploads the object and sets its key to  `Development/ProjectA.docx`.

    In the key, `Development` is the prefix and `/`  is the delimiter.   You can retrieve a list of all objects with a specific prefix and delimiter from a bucket.  In the console, you click the **Development**  folder, and the console lists the objects in the folder, as shown in the following figure.

    ![](images/1187_en-US.png)

    **Note:** When the console lists the **Development** folder in the **example-company** bucket, it sends OSS a request which specifies the prefix  `Development` and the delimiter `/`  .  The console responds with a folder list the same as that in the file system.   The preceding example shows that the bucket **example-company**  has two objects with the key  `Development/Alibaba Cloud.pdf`,  `Development/ProjectA.docx` and `Development/ProjectB.docx`.


The console uses object keys to resemble a logical hierarchy.  When you create a logical hierarchy of objects, you can manage access to individual folders, as described later in this tutorial.

Before going into the tutorial, you also need to know the concept: root-level bucket content.  Suppose the **example-company**  bucket has the following objects:

-   Development/Alibaba Cloud.pdf
-   Development/ProjectA.docx
-   Development/ProjectB.docx
-   Marketing/data2016.xlsx
-   Marketing/data2016.xlsx
-   Private/2017/images.zip
-   Private/2017/promote.pptx
-   oss-dg.pdf

These object keys resemble a logical hierarchy with Development, Marketing, and Private as root-level folders and oss-dg.pdf as a root-level object.  When you click the bucket name in the OSS console,  the console shows the top-level prefixes and a delimiter \( Development/、Marketing/ and Private/\) as root-level folders.  The object key oss-dg.pdf  does not have a prefix, so it appears as a root-level item.

![](images/1188_en-US.png)

## Request and response of OSS {#section_isl_bc3_5db .section}

Before granting permissions, we need to understand what request the console sends to OSS when a user clicks a bucket name, the response OSS returns, and how the console interprets the response.

When a user clicks a bucket name, the console sends the [GetBucket](../intl.en-US/API Reference/Bucket operations/GetBucket (List Object).md#) request to   OSS.  This request includes the following parameters:

-   `prefix` with an empty string as its value.
-   `delimiter` with `/` as its value.

An example request is as follows:

```
GET /? prefix=&delimiter=/ HTTP/1.1
Host: example-company.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

OSS returns a response that includes the `ListBucketResult` element:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 712
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
<Name>example-company</Name>
<Prefix></Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>oss-dg.pdf</Key>
        
    </Contents>
   <CommonPrefixes>
        <Prefix>Development</Prefix>
   </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Marketing</Prefix>
   </CommonPrefixes>
      <CommonPrefixes>
        <Prefix>Private</Prefix>
   </CommonPrefixes>
</ListBucketResult>
```

The key oss-dg.pdf does not contain the `/`  delimiter, so OSS returns the key in the  `<Contents/>` <Contents/\>  element.  All other keys in the bucket example-company contain the  `/` delimiter, so OSS groups these keys and returns a `CommonPrefixes/`  element for each of the prefix values Development/、Marketing/ and Private/ <CommonPrefixes/\> . The CommonPrefixes/ element includes a substring of the key name, which starts from the beginning of the key name and ends with \(but does not include\) the first occurrence of the specified  `/` delimiter.

The console interprets this result and displays the root-level items as follows:

![](images/1189_en-US.png)

Now, if a user clicks the **Development** folder, the console sends the [GetBucket](../intl.en-US/API Reference/Bucket operations/GetBucket (List Object).md#) request to  OSS.  This request includes the following parameters:

-   `prefix` with `Development/` as its value.
-   `delimiter` with `/` as its value.

An example request is as follows:

```
GET /? prefix=Development/&delimiter=/ HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Date: Fri, 24 Feb 2012 08:43:27 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:DNrnx7xHk3sgysx7I8U9I9IY1vY=
```

In response, OSS returns the object keys that begin with the specified prefix:

```
HTTP/1.1 200 OK
x-oss-request-id: 534B371674E88A4D8906008B
Date: Fri, 24 Feb 2012 08:43:27 GMT
Content-Type: application/xml
Content-Length: 712
Connection: keep-alive
Server: AliyunOSS
<? xml version="1.0" encoding="UTF-8"? >
<ListBucketResult xmlns=¡±http://doc.oss-cn-hangzhou.aliyuncs.com¡±>
<Name>example-company</Name>
<Prefix>Development/</Prefix>
<Marker></Marker>
<MaxKeys>100</MaxKeys>
<Delimiter>/</Delimiter>
    <IsTruncated>false</IsTruncated>
    <Contents>
        <Key>ProjectA.docx</Key>
        
    </Contents>
    <Contents>
        <Key>ProjectB.docx</Key>
        
    </Contents>
</ListBucketResult>
```

The console interprets this result and displays the object keys as follows:

![](images/1190_en-US.png)

## Tutorial example {#section_zr5_fd3_5db .section}

The tutorial example is as follows:

-   You create a bucket example-company and then add three folders \(Development, Marketing, and Private\) into it.

-   You have two users, Anne and Leo.  You want Anne to access only the Development folder and Leo to access only the Marketing folder, and you want to keep the Private  folder private.  In the tutorial example, you manage access by creating Alibaba Cloud identity and Resource Access Management \(RAM\) users \(Anne and Leo\) and granting them the necessary permissions.

-   RAM also supports the creation of user groups and granting of group-level permissions that apply to all users in the group.  This helps you better manage and control permissions.  For this example, both Anne and Leo need some common permissions.  You also create a group named  Staff and then add both Anne and Leo to the Staff group.  You first grant permissions by attaching a group policy to the Staff group.  Then you add user-specific permissions by attaching policies to specific users.


**Note:** The tutorial uses example-company as the bucket name, Anne and Leo as the RAM users, and Staff as the group name.  Because Alibaba Cloud OSS  requires that bucket names be globally unique, you must replace the bucket name with your own unique bucket name.

## Prepare for the tutorial {#section_qqx_3d3_5db .section}

In this example, you use your primary account credentials to create RAM users.  Initially, these users have no permissions.  You incrementally grant these users permissions to perform  specific OSS actions.  To test these permissions, you log on to the console with each user’s credentials.  As you incrementally grant permissions as a primary account owner and test permissions as a RAM  user, you have to log on and log off, each time using different credentials.  You can perform this testing with one browser,  but the process is more efficient if you can use two different browsers: one browser to connect to the Alibaba Cloud console with your primary account credentials and the other to connect with the  RAM user credentials.

To log on to the Alibaba Cloud console with your primary account credentials, go to https://account.alibabacloud.com/login/login.htm.  RAM users cannot log on by using the same link.  They must use the RAM user logon link.  As the primary account owner, you can provide this logon link to your users.

**Note:** For more information about RAM, see [Log on with a RAM user account](https://www.alibabacloud.com/help/doc-detail/43640.htm).

Provide a logon link for RAM users

1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
2.  In the left-side navigation pane, click  **Dashboard**.
3.  Find the URL after RAM User Logon Link:  You will provide this URL to RAM users to log on to the console with their RAM user name and password.

## Step 1.  Create a bucket {#section_a4f_5d3_5db .section}

In this step, you log on to the   OSS console with your primary account credentials, create a bucket, add folders \(Development, Marketing, Private\) to the bucket, and upload one or two sample documents in each folder.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Create a bucket named **example-company** .

    For detailed procedures, see [Create a bucket](../intl.en-US/Console User Guide/Manage buckets/Create a bucket.md#) in the OSS Console User Guide.

3.  Upload one file to the bucket.

    This example assumes that you upload the file oss-dg.pdf at the root level of the bucket.  You can upload your own file with a different file name.

    For detailed procedures, see   [Upload files](../intl.en-US/Console User Guide/Manage objects/Upload objects.md#) in the OSS Console User Guide.

4.  Create three folders named Development, Marketing, and Private.

    For detailed procedures, see [Create a folder](../intl.en-US/Console User Guide/Manage objects/Create a folder.md#)in the OSS Console User Guide.

5.  Upload one or two files to each folder.

    This example assumes that you upload objects in the bucket with the following object keys:

    -   Development/Alibaba Cloud.pdf
    -   Development/ProjectA.docx
    -   Development/ProjectB.docx
    -   Marketing/data2016.xlsx
    -   Marketing/data2016.xlsx
    -   Private/2017/images.zip
    -   Private/2017/promote.pptx
    -   oss-dg.pdf

## Step 2.  Create RAM users and a group {#section_vzz_xd3_5db .section}

In this step, you use the RAM console to add two RAM users, Anne and Leo, to your primary account.  You also create a group named Staff,  and then add both users to the group.

**Note:** In this step, do not attach any policies that grant permissions to these users.  In the following steps, you will incrementally grant permissions.

For detailed procedures on creating a RAM user, see [Create a RAM user](https://www.alibabacloud.com/help/doc-detail/28637.htm) in the RAM Quick Start.  Remember to create a logon password for each RAM user.

For detailed procedures on creating a group, see the Create a group section of [Groups](https://www.alibabacloud.com/help/doc-detail/28648.htm) in the RAM User Guide.

## Step 3:  Verify that RAM users have no permissions {#section_izl_b23_5db .section}

If you use two browsers, now you can use the other one to log on to the console by using one of the RAM user credentials.

1.  Open the RAM user logon page, and log on to the RAM console with Anne’s or Leo’s credentials.
2.  Open the OSS console.

    You find no buckets in the console, which means that Anne does not have any permissions on the bucket example-company.


## Step 4:  Grant group-level permissions {#section_xwr_223_5db .section}

We want both Anne and Leo to have the access and ability to perform the following tasks:

-   List all buckets owned by the primary account.

    To do this, Anne and Leo must have permission for the oss:ListBuckets action.

-   List root-level items, folders, and objects, in the example-company bucket.

    To do this, Anne and Leo must have permission for the oss:ListObjects action on the example-company bucket.


Step 4.1:  Grant permissions to list all buckets

In this step, you create a policy that grants users minimum permissions.  With the minimum permissions, users can list all buckets owned by the primary account.  You also attach the policy to the  Staff group, so you grant the group permission to get a list of buckets owned by the primary account.

1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
2.  Create a policy **AllowGroupToSeeBucketListInConsole**.
    1.  From the left-side navigation pane, click **Policies**, and then click**Create Authorization Policy**.
    2.  Click **Blank Template**.
    3.  In the Authorization Policy Name field, enter  **AllowGroupToSeeBucketListInConsole**.
    4.  In the Policy Content field, copy and paste the following policy.

        ```
        
        "Version": "1",
        "Statement": [
         
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets"
           
           "Resource": [
             "acs:oss:*:*:*"
           
         
        
        
        ```

        **Note:** A policy is a  JSON document.  In the policy, the Statement attribute is an array of objects, and each object describes a permission using a collection of name value pairs.  The preceding policy describes one specific permission.  The Effect  attribute value determines whether a specific permission is allowed or denied.  The Action attribute specifies the type of access.  In the policy, the oss:ListBuckets is a predefined OSS  action, which returns a list of all buckets owned by the authenticated sender.

3.  Attach the **AllowGroupToSeeBucketListInConsole** policy to the Staff group. 

    For detailed procedures on attaching a policy, see the [Attach policies to a RAM group](https://www.alibabacloud.com/help/doc-detail/28639.htm) section of **Attach policies to a RAM user** in the RAM Quick Start. 

    You can attach policies to RAM users and  groups in the RAM console.  In this example, we attach the policy to the group, because we want both Anne and Leo to be able to list the buckets.

4.  Test the permission.
    1.  Open the RAM user logon page, and log on to the RAM console with Anne’s or Leo’s credentials.
    2.  Open the OSS console.

        The console lists all of the buckets.

    3.  Click the example-company  bucket, and then click the **Files** tab.

        A message box is displayed, indicating that you have no corresponding access rights.

        ![](images/1193_en-US.png)


Step 4.2:  Grant permissions to list root-level content of a bucket

In this step, you grant permissions to allow all users to list all the items in the bucket example-company.  When users click  the example-company in the OSS console, they can see the root-level items in the bucket.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/1195_en-US.png)

1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
2.  Replace the existing policy **AllowGroupToSeeBucketListInConsole** that is attached to the Staff group with the following policy.  The following policy also allows the oss:ListObjects action.  Remember to replace example-company in the policy Resource with the name of your bucket.

    For detailed procedures, see the  [Modify a custom authorization policy](https://www.alibabacloud.com/help/doc-detail/28652.htm) section of **Authorization policies** in the RAM User Guide.  Note that you can modify a   RAM policy a maximum of five times.  If this is exceeded, you must delete the policy, created a new one, and then attach the policy to the Staff group again.

    ```
    
       "Version": "1",
       "Statement": [
         
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketAcl"
           
           "Resource": [
             "acs:oss:*:*:*"
           
           "Condition": {}
         
         
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           
           "Resource": [
             "acs:oss:*:*:example-company"
           
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 
               
               "oss:Delimiter": [
                 
               
             
           
         
       
     
    ```

    **Note:** 

    -   To list bucket content, users need permission to call the oss:ListObjects  action.  To make sure that they see only the root-level content, we add a condition that users must specify an empty prefix in the request, that is, they cannot click any of the root-level folders.  We also add a condition to require folder-style access by requiring user requests to include the delimiter parameter with the value  `/`.
    -   When a user logs on to the OSS console, the console checks the user’s identities for access to the OSS service.  To support bucket operations in the console, we also need to add the  oss:GetBucketAcl action.
3.  Test the updated permissions.
    1.  Open the RAM user logon page, and log on to the RAM console with Anne’s or Leo’s credentials.
    2.  Open the OSS console.

        The console lists all of the buckets.

    3.  Click the example-company bucket, and then click the **Files** tab.

        The console lists all the root-level items.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4349/1196_en-US.png)

    4.  Click any of the folders or the object **oss-dg.pdf**.

        A message box is displayed, indicating that you have no corresponding access rights.


Summary of the group policy

The result of the group policy that you have added is to grant the RAM users Anne and Leo the following minimum permissions:

-   The ability to list all buckets owned by the primary account.
-   The ability to see all root-level items in the example-company bucket.

However, they still have limited access.  In the following section, we grant further user-specific permissions including:

-   Provide Anne the ability to get and put objects in the Development folder.
-   Provide Bob the ability to get and put objects in the Finance folder.

For user-specific permissions, you attach a policy to the specific user, not to the entire group.  In the following section, you grant Anne permission to work within the Development folder.  You can repeat the steps to grant similar permission to Leo  to work in the Finance folder.

## Step 5:  Grant RAM user Anne specific permissions {#section_f3l_wf3_5db .section}

In this step, we grant additional permissions to Anne so that she can see the content of the Development folder and get and put objects in the folder.

Step 5.1:  Grant RAM user Anne permission to list the Development folder content

For Anne to list the  Development folder content,  you must attach a policy to her that grants permission for the oss:ListObjects action on the example-company bucket, and includes the condition that user must specify the prefix  `Development/`  in the request.

1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
2.  Create a policy **AllowListBucketsIfSpecificPrefixIsIncluded** that grants the RAM user Anne permission to list the  Development folder content.
    1.  From the left-side navigation pane, click **Policies**, and then click **Create Authorization Policy**.
    2.  Click **Blank Template**.
    3.  In the Authorization Policy Name field, enter  **AllowListBucketsIfSpecificPrefixIsIncluded**.
    4.  In the Policy Content field, copy and paste the following policy.

        ```
        
        "Version": "1",
        "Statement": [
         
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           
           "Resource": [
             "acs:oss:*:*:example-company"
           
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Development/*"
               
             
           
         
        
        
        ```

3.  Attach the policy to the RAM user Anne. 

    For detailed procedures on attaching a policy, see  [Attach policies to a RAM user](https://www.alibabacloud.com/help/doc-detail/28639.htm) in the RAM Quick Start.

4.  Test Anne’s permissions.
    1.  Open the RAM user logon page, and log on to the RAM console with Anne’s credentials.
    2.  Open the OSS console. The console lists all of the buckets.
    3.  Click the example-company bucket, and then click the **Files** tab. The console lists all the root-level items.
    4.  Click the Development/ folder. The console lists the objects in the folder.

Step 5.2 Grant RAM User Anne permissions to get and put objects in the Development folder

For Anne to get and put objects in the Development  folder, you must grant her permission to call the oss:GetObject and oss:PutObject actions, and includes the condition that user must specify the prefix Development/  in the request.

1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
2.  Replace the policy **AllowListBucketsIfSpecificPrefixIsIncluded** you created in the previous step with the following policy.

    For detailed procedures, see  the [Modify a custom authorization policy](https://www.alibabacloud.com/help/doc-detail/28652.htm) section of **Authorization policies** in the RAM User Guide.  Note that you can modify a RAM  policy a maximum of five times.  If this is exceeded, you must delete the policy, created a new one, and then attach the policy to the  user again.

    ```
    
       "Version": "1",
       "Statement": [
         
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           
           "Resource": [
             "acs:oss:*:*:example-company"
           
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Development/*"
               
             
           
         
         
           "Effect": "Allow",
           "Action": [
             "oss:GetObject",
             "oss:PutObject",
             "oss:GetObjectAcl"
           
           "Resource": [
             "acs:oss:*:*:example-company/Development/*"
           
           "Condition": {}
         
       
     
    ```

    **Note:** When a user logs on to the  OSS console, the console checks the user’s identities for access to the OSS service.  To support bucket operations in the console, we also need to add the oss:GetObjectAcl  action.

3.  Test the updated policy.
    1.  Open the RAM user logon page, and log on to the RAM console with Anne’s credentials.
    2.  Open the OSS console.

        The console lists all of the buckets.

    3.  In the OSS console, verify that Anne can now add an object and download an object in the Development folder.

Step 5.3 Explicitly deny RAM user Anne permissions to any other folders in the bucket

RAM user Anne can now list the root-level content in the example-company  bucket, and get and put objects in the Development folder.  If you want to strictly restrict the access permissions, you can explicitly deny Anne’s access to any other folders in the bucket.  If other policies grant Anne’s  access to any other folders in the bucket, this explicit policy overrides those permissions.

You can add the following statement to the RAM user Anne’s policy  **AllowListBucketsIfSpecificPrefixIsIncluded**. The following statement requires all requests that Anne sends to OSS to include the prefix parameter, and the parameter value can be either  Development/\*  or an empty string.

```

      "Effect": "Deny",
      "Action": [
        "oss:ListObjects"
      
      "Resource": [
        "acs:oss:*:*:example-company"
      
      "Condition": {
        "StringNotLike": {
          "oss:Prefix": [
            "Development/*",
            
          
        
      
    
```

Follow the preceding step to  update the policy  **AllowListBucketsIfSpecificPrefixIsIncluded** that you created for RAM user Anne.  Copy and paste the following policy to replace the existing one.

```

  "Version": "1",
  "Statement": [
    
      "Effect": "Allow",
      "Action": [
        "oss:ListObjects"
      
      "Resource": [
        "acs:oss:*:*:example-company"
      
      "Condition": {
        "StringLike": {
          "oss:Prefix": [
            "Development /*"
          
        
      
    
    
      "Effect": "Allow",
      "Action": [
        "oss:GetObject",
        "oss:PutObject",
        "oss:GetObjectAcl"
      
      "Resource": [
        "acs:oss:*:*:example-company/Development/*"
      
      "Condition": {}
    
    
      "Effect": "Deny",
      "Action": [
        "oss:ListObjects"
      
      "Resource": [
        "acs:oss:*:*:example-company"
      
      "Condition": {
        "StringNotLike": {
          "oss:Prefix": [
            "Development /*",
            
          
        
      
    
  

```

## Step 6:  Grant RAM user Leo specific permissions {#section_sfj_hh3_5db .section}

Now you want to grant Leo permission to the Marketing folder.  Follow the steps you used earlier to grant permissions to Anne, but replace the Development folder with the Marketing  folder.  For detailed procedures, see Step 5: Grant RAM user Anne specific permissions.

## Step 7:  Secure the Private folder {#section_m13_3h3_5db .section}

In this example, you have only two users.  You have granted all the minimum required permissions at the group level. In addition, you have granted user-level permissions only when you really need permissions at the individual user level.  This approach helps minimize the effort of managing permissions.  As the number of users increases, we want to make sure that we do not accidentally grant a user permission to the  Private folder.  Therefore we need to add a policy that explicitly denies access to the Private folder.  An explicit denial overrides any other permissions.  To make sure that the Private  folder remains private, you can add the following two deny statements to the group policy:

-   Add the following statement to explicitly deny any action on resources in the Private folder \(example-company/Private/\*\) .

    ```
    
        "Effect": "Deny",
        "Action": [
          "oss:*"
        
        "Resource": [
          "acs:oss:*:*:example-company/Private/*"
        
        "Condition": {}
      
    ```

-   You also deny permission for the ListObjects action when the request specifies the Private/ prefix.  In the console, if Anne or Leo clicks the Private  folder, this policy causes OSS  to return an error response.

    ```
    
        "Effect": "Deny",
        "Action": [
          "oss:ListObjects"
        
        "Resource": [
          "acs:oss:*:*:*"
        
        "Condition": {
          "StringLike": {
            "oss:Prefix": [
              "Private/"
            
          
        
      
    ```

-   Replace the Staff group policy  **AllowGroupToSeeBucketListInConsole** with an updated policy that includes the preceding deny statements.  After the updated policy is applied, none of the users in the group can access the Private folder in your bucket.

    1.  Log on to the [RAM console](https://ram.console.aliyun.com) with your primary account credentials.
    2.  Replace the existing policy **AllowGroupToSeeBucketListInConsole** that is attached to the Staff group with the following policy.  Remember to replace  example-company in the policy Resource with the name of your bucket.

        ```
        
        "Version": "1",
        "Statement": [
         
           "Effect": "Allow",
           "Action": [
             "oss:ListBuckets",
             "oss:GetBucketAcl"
           
           "Resource": [
             "acs:oss:*:*:*"
           
           "Condition": {}
         
         
           "Effect": "Allow",
           "Action": [
             "oss:ListObjects"
           
           "Resource": [
             "acs:oss:*:*:example-company"
           
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                  
                
               "oss:Delimiter": [
                 
               
             
           
         
         
           "Effect": "Deny",
           "Action": [
             "oss:*"
           
           "Resource": [
             "acs:oss:*:*:example-company/Private/*"
           
           "Condition": {}
         
         
           "Effect": "Deny",
           "Action": [
             "Oss: Loud"
           
           "Resource": [
             "acs:oss:*:*:*"
           
           "Condition": {
             "StringLike": {
               "oss:Prefix": [
                 "Private/"
               
             
           
         
        
        
        ```


## Cleanup {#section_fkk_c33_5db .section}

After you finish the tutorial, remove the users Anne and Leo in the RAM console. 

For detailed procedures, see [Delete a RAM user](https://www.alibabacloud.com/help/doc-detail/28647.htm) section of **Users** in the RAM User Guide.

To avoid any unnecessary charges, delete the objects and the bucket that you created for this tutorial.

