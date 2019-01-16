# List objects {#concept_84841_zh .concept}

This topic describes how to list objects.

Objects are listed alphabetically. You can use ossClient.listObjects to list objects in a bucket. The following parameters can be used for listObjects:

-   ObjectListing listObjects\(String bucketName\): lists objects in a bucket. A maximum of 100 objects can be listed.
-   ObjectListing listObjects\(String bucketName, String prefix\): lists objects with the specified prefix in a bucket. A maximum of 100 objects can be listed.
-   ObjectListing listObjects\(ListObjectsRequest listObjectsRequest\): provides multiple filtering functions to flexibly query objects.

The following table describes parameters for ObjectListing.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|objectSummaries|Specifies the returned Object Meta.|List<OSSObjectSummary\> getObjectSummaries\(\)|
|prefix|Specifies the prefix you configure to list required objects.|String getPrefix\(\)|
|delimiter|Specifies a delimiter used to group objects.|String getDelimiter\(\)|
|marker|Specifies the initial object in the list.|String getMarker\(\)|
|maxKeys|Specifies the maximum number of objects that can be listed.|int getMaxKeys\(\)|
|nextMarker|Specifies the initial position for the next object.|String getNextMarker\(\)|
|isTruncated|Specifies whether the listed objects are truncated.-   If the objects are all listed without truncation, a false value is returned.
-   If the listed objects are truncated, a true value is returned.

|boolean isTruncated\(\)|
|commonPrefixes|Specifies a set of objects whose names share a prefix and end with a forward slash \(/\) delimiter.|List<String\> getCommonPrefixes\(\)|
|encodingType|Specifies the encoding type used in the response.|String getEncodingType\(\)|

## Simple list {#section_a2g_szb_kfb .section}

Run the following code to list objects in a specified bucket. A maximum of 100 objects can be listed by default.

```language-java
// This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";
String KeyPrefix = "<yourKeyPrefix>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// List objects. If you do not configure KeyPrifex, all objects in the bucket are listed. If you configure KeyPrifex, objects with a specified prefix in the bucket are listed.
ObjectListing objectListing = ossClient.listObjects(bucketName, KeyPrefix);
List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
for (OSSObjectSummary s : sums) {
    System.out.println("\t" + s.getKey());
}

// Close your OSSClient.
ossClient.shutdown();

```

## List objects with ListObjectsRequest { .section}

You can configure parameters for ListObjectsRequest to flexibly query objects. The following table describes parameters for ListObjectsRequest.

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|prefix|Specifies the prefix that must be included in the returned objects.|setPrefix\(String prefix\)|
|delimiter|Specifies a delimiter of a forward slash \(/\) used to group objects based on their names. The substring between the specified prefix and the first occurrence of the delimiter of a forward slash \(/\) is commonPrefixes.|setDelimiter\(String delimiter\)|
|marker|Lists the objects after the value of marker.|setMarker\(String marker\)|
|maxKeys|Specifies the maximum number of objects that can be listed. The default value is 100. The maximum value is 1,000.|setMaxKeys\(Integer maxKeys\)|
|encodingType|Specifies the encoding type of an object name in the response body. Only the URL encoding type is supported.|setEncodingType\(String encodingType\)|

-   List a specified number of objects

    Run the following code to list a specified number of objects:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Configure the maximum number.
    final int maxKeys = 200;
    // List objects.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMaxKeys(maxKeys));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t" + s.getKey());
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List objects by a specified prefix

    Run the following code to list objects with a specified prefix:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Specify a prefix.
    final String keyPrefix = "<yourkeyPrefix>";
    
    // List objects with a specified prefix. By default, a maximum of 100 objects can be listed.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withPrefix(keyPrefix));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t" + s.getKey());
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List objects by marker

    The marker parameter indicates the name of an object from which the listing begins. Run the following code to specify an object \(specified with marker\) after which the listing begins:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    final String marker = "<yourMarker>";
    
    // List objects placed after the object (specified with marker). By default, a maximum of 100 objects can be listed.
    ObjectListing objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMarker(marker));
    List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
    for (OSSObjectSummary s : sums) {
        System.out.println("\t" + s.getKey());
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List all objects on one or more pages

    Use the following code to list all objects on one or more pages. The maximum number of objects listed on each page is specified with maxKeys.

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    final int maxKeys = 200;
    String nextMarker = null;
    ObjectListing objectListing;
    
    do {
        objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).withMarker(nextMarker).withMaxKeys(maxKeys));
        
        List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t" + s.getKey());
        }
        
        nextMarker = objectListing.getNextMarker();
        
    } while (objectListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   List objects by a specified prefix on one or more pages

    Run the following code to list objects by a specified prefix on one or more pages:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // A maximum of 200 objects can be listed on each page.
    final int maxKeys = 200;
    final String keyPrefix = "<yourkeyPrefix>";
    String nextMarker = "<yourNextMarker>";
    ObjectListing objectListing;
    
    do {
        objectListing = ossClient.listObjects(new ListObjectsRequest(bucketName).
                withPrefix(keyPrefix).withMarker(nextMarker).withMaxKeys(maxKeys));
        
        List<OSSObjectSummary> sums = objectListing.getObjectSummaries();
        for (OSSObjectSummary s : sums) {
            System.out.println("\t" + s.getKey());
        }
        
        nextMarker = objectListing.getNextMarker();
        
    } while (objectListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

-   Specify the name of an object for encoding.

    You must encode the name of an object if the name contains the following special characters. OSS supports encoding with URL.

    -   Single quotation marks \(‘\)
    -   Double quotations marks \(“\)
    -   ampersands \(&\)
    -   angle brackets \(< \>\)
    -   Pause marker \(、\)
    -   Chinese characters
    Run the following code to encode the name of a specified object:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    final int maxKeys = 200;
    final String keyPrefix = "<yourkeyPrefix>";
    String nextMarker = "<yourNextMarker>";
    ObjectListing objectListing;
    
    do {
        ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
        listObjectsRequest.setPrefix(keyPrefix);
        listObjectsRequest.setMaxKeys(maxKeys);
        listObjectsRequest.setMarker(nextMarker);
        
        // Specify the name of an object for encoding.
        listObjectsRequest.setEncodingType("url");
        
        objectListing = ossClient.listObjects(listObjectsRequest);
        
        // Decode the object.
        for (OSSObjectSummary objectSummary: objectListing.getObjectSummaries()) {
            System.out.println("Key:" + URLDecoder.decode(objectSummary.getKey(), "UTF-8"));
        }
        
        // Decode the commonPrefixes parameter.
        for (String commonPrefixes: objectListing.getCommonPrefixes()) {
            System.out.println("CommonPrefixes:" + URLDecoder.decode(commonPrefixes, "UTF-8"));
        }
        
        // Decode the nextMarker parameter.
        if (objectListing.getNextMarker() ! = null) {
            nextMarker = URLDecoder.decode(objectListing.getNextMarker(), "UTF-8");
        }
    } while (objectListing.isTruncated());
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```


## Folder simulation { .section}

OSS does not use folders. All elements are stored as objects. To simulate a folder on the OSS console, you actually create a 0 MB object whose name ends with a forward slash \(/\). This object can be uploaded and downloaded. By default, the OSS console displays an object whose name ends with a forward slash \(/\) as a folder.

The delimiter and prefix parameters can be used to simulate folder functions.

-   Prefix: specifies the prefix as the name of a folder. The folder is used to list all files \(in the folder\) and subfolders \(directories in the folder\) that start with this prefix. These files and subfolders are included in the Objects list.
-   Delimiter: If the delimiter is specified as a forward slash \(/\), only the files and subfolders \(directories\) are displayed. Subfolders \(directories\) are included in the CommonPrefixes list, while the files and folders in subfolders are not displayed.

For more information about folders, see [Folder simulation](../../../../../reseller.en-US/Developer Guide/Manage files/View the object list.md#). For the complete code of creating an object, see [GitHub](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CreateFolderSample.java).

Assume that the following objects are stored in a bucket: oss.jpg, fun/test.jpg, fun/movie/001.avi, and fun/movie/007.avi. Forward slashes \(/\) are used as delimiters for folders. The subsequent examples show how to simulate folder functions.

-   List all objects in a bucket

    Run the following code to list all objects in a bucket:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Create ListObjectsRequest.
    ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
    
    // List objects.
    ObjectListing listing = ossClient.listObjects(listObjectsRequest);
    
    // Print all objects.
    System.out.println("Objects:");
    for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
        System.out.println(objectSummary.getKey());
    }
    
    // Print all commonPrefixes.
    System.out.println("CommonPrefixes:");
    for (String commonPrefix : listing.getCommonPrefixes()) {
        System.out.println(commonPrefix);
    }
    
    // Disable the OSSClient.
    // Close your OSSClient.
    
    ```

    The returned results are as follows:

    ```
    Objects:
    fun/movie/001.avi
    fun/movie/007.avi
    fun/test.jpg
    oss.jpg
    
    CommonPrefixes:
    
    ```

-   List all objects in a specified folder

    Run the following code to list all objects in a specified folder:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Create ListObjectsRequest.
    ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
    // Configure the Prefix parameter to obtain all objects in the fun folder.
    listObjectsRequest.setPrefix("fun/");
    
    // Recursively list all objects in the fun folder.
    ObjectListing listing = ossClient.listObjects(listObjectsRequest);
    
    // Print all objects.
    System.out.println("Objects:");
    for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
        System.out.println(objectSummary.getKey());
    }
    
    // Print all commonPrefixes.
    System.out.println("\nCommonPrefixes:");
    for (String commonPrefix : listing.getCommonPrefixes()) {
        System.out.println(commonPrefix);
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    The returned results are as follows:

    ```
    Objects:
    fun/movie/001.avi
    fun/movie/007.avi
    fun/test.jpg
    
    CommonPrefixes:
    
    ```

-   List objects and subfolders in a folder

    Use the following code to list objects and subfolders in a folder:

    ```language-java
    // This example uses endpoint China East 1 (Hangzhou). Specify the actual endpoint based on your requirements.
    String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    String accessKeyId = "<yourAccessKeyId>";
    String accessKeySecret = "<yourAccessKeySecret>";
    String bucketName = "<yourBucketName>";
    
    // Create an OSSClient instance.
    OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    
    // Create ListObjectsRequest.
    ListObjectsRequest listObjectsRequest = new ListObjectsRequest(bucketName);
    
    // Set forward slashes (/) as the delimiter for folders.
    listObjectsRequest.setDelimiter("/");
    
    // List all objects and folders in the fun folder.
    listObjectsRequest.setPrefix("fun/");
    
    ObjectListing listing = ossClient.listObjects(listObjectsRequest);
    
    // Print all objects.
    System.out.println("Objects:");
    // List objects in the fun folder in a objectSummaries list.
    for (OSSObjectSummary objectSummary : listing.getObjectSummaries()) {
        System.out.println(objectSummary.getKey());
    }
    
    // Print all commonPrefixes.
    System.out.println("\nCommonPrefixes:");
    // commonPrefixs lists all subfolders in the fun folder. The fun/movie/001.avi and fun/movie/007.avi objects are not listed, because they are objects in the movie folder (a subfolder of the fun folder).
    for (String commonPrefix : listing.getCommonPrefixes()) {
        System.out.println(commonPrefix);
    }
    
    // Close your OSSClient.
    ossClient.shutdown();
    
    ```

    The returned results are as follows:

    ```
    Objects:
    fun/test.jpg
    
    CommonPrefixes:
    fun/movie/
    
    ```


