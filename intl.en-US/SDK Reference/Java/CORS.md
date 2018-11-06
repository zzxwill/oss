# CORS {#concept_32018_zh .concept}

This topic describes how to perform CORS.

Cross-origin resource sharing \(CORS\) allows web applications to access resources that belong to another region. OSS provides CORS APIs for convenient cross-origin access control.

For more information, see [Cross-origin resource sharing](../../../../reseller.en-US/Developer Guide/Security management/Cross-origin resource sharing.md#) and [PutBucketcors](../../../../reseller.en-US/API Reference/Cross-Origin Resource Sharing/PutBucketcors.md#) in OSS Developer Guide.

## Configure CORS rules {#section_gbj_jr2_lfb .section}

Run the following code to configure CORS rules for the specified bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

SetBucketCORSRequest request = new SetBucketCORSRequest(bucketName);

// Create a CORS rule. A maximum of 10 rules can be configured for each bucket.
ArrayList<CORSRule> putCorsRules = new ArrayList<CORSRule>();

CORSRule corRule = new CORSRule();

ArrayList<String> allowedOrigin = new ArrayList<String>();
// Specify the source of the cross-origin access request.
allowedOrigin.add( "http://www.b.com");

ArrayList<String> allowedMethod = new ArrayList<String>();
// Specify the cross-region request methods (GET, PUT, DELETE, POST, and HEAD) that are allowed.
allowedMethod.add("GET");

ArrayList<String> allowedHeader = new ArrayList<String>();
// Specify whether the header specified in Access-Control-Request-Headers of pre-flight request (OPTIONS) is allowed.
allowedHeader.add("x-oss-test");

ArrayList<String> exposedHeader = new ArrayList<String>();
// Specify the response header that allows user access from applications.
exposedHeader.add("x-oss-test1");
// AllowedOrigins and AllowedMethods allow only one wildcard asterisk (*). Wildcard asterisks (*) indicate that all sources of the cross-origin requests and operations are allowed.
corRule.setAllowedMethods(allowedMethod);
corRule.setAllowedOrigins(allowedOrigin);
// AllowedHeaders and ExposeHeaders do not allow wildcard asterisks (*).
corRule.setAllowedHeaders(allowedHeader);
corRule.setExposeHeaders(exposedHeader);
// Specify the cache time (seconds) for the response of browser pre-flight (OPTIONS) requests to a specific resource.
corRule.setMaxAgeSeconds(10);

// A maximum of 10 rules is allowed.
putCorsRules.add(corRule);
// The existing rules will be replaced.
request.setCorsRules(putCorsRules);
ossClient.setBucketCORS(request);

// Close your OSSClient.
ossClient.shutdown();

```

## Obtain CORS rules { .section}

Run the following code to obtain CORS rules:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

ArrayList<CORSRule> corsRules;
// Obtain the CORS rule list.
corsRules =  (ArrayList<CORSRule>) ossClient.getBucketCORSRules(bucketName);
for (CORSRule rule : corsRules) {
    for (String allowedOrigin1 : rule.getAllowedOrigins()) {
    	// Obtain allowed sources of cross-origin requests.
    	System.out.println(allowedOrigin1);
    }

    for (String allowedMethod1 : rule.getAllowedMethods()) {
    	// Obtain the allowed cross-origin request method.
    	System.out.println(allowedMethod1);
    }

    if (rule.getAllowedHeaders().size() > 0){
    	for (String allowedHeader1 : rule.getAllowedHeaders()) {
    	    // Obtain the header list for allowed cross-origin requests.
    	    System.out.println(allowedHeader1);
    	}
    }

    if (rule.getExposeHeaders().size() > 0) {
        for (String exposeHeader : rule.getExposeHeaders()) {
    	    // Obtain the header for an allowed cross-origin request.
    	    System.out.println(exposeHeader);
        }
    }

    if ( null ! = rule.getMaxAgeSeconds()) {
        System.out.println(rule.getMaxAgeSeconds());
    }
}

// Close your OSSClient.
ossClient.shutdown();

```

## Delete CORS rules { .section}

Run the following code to delete all CORS rules for the specified bucket:

```language-java
// This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
String endpoint = "http://oss-cn-hangzhou.aliyuncs.com";
// It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
String accessKeyId = "<yourAccessKeyId>";
String accessKeySecret = "<yourAccessKeySecret>";
String bucketName = "<yourBucketName>";

// Create an OSSClient instance.
OSSClient ossClient = new OSSClient(endpoint, accessKeyId, accessKeySecret);

// Delete all CORS rules for a specified bucket.
ossClient.deleteBucketCORSRules(bucketName);

// Close your OSSClient.
ossClient.shutdown();

```

