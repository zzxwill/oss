# Conditional download {#concept_88623_zh .concept}

You can configure one or more conditions for downloads. If the specified conditions are met, the object can be downloaded. If the specified conditions are not met, an error code is returned and the object cannot be downloaded. You can configure the following conditions: You can configure the following conditions:

|Parameter|Description|Configuration method|
|:--------|:----------|:-------------------|
|IfModifiedSince|If the specified time is earlier than the time the object is modified, the object can be downloaded. Otherwise, an error \(304 Not modified\) is returned.|oss.IfModifiedSince|
|IfUnmodifiedSince|If the specified time is later than or equal to the time the object is modified, the object can be downloaded. Otherwise, an error \(412 Precondition failed\) is returned.|oss.IfUnmodifiedSince|
|IfMatch|If the specified ETag matches that of an object, the object can be downloaded. Otherwise, an error \(412 Precondition failed\) is returned.|oss.IfMatch|
|IfNoneMatch|If the specified ETag does not match that of an object, the object can be downloaded. Otherwise, an error \(304 Not modified\) is returned.|oss.IfNoneMatch|

Both If-Modified-Since and If-Unmodified-Since can exist at the same time as object download conditions. Both If-Match and If-None-Match can exist at the same time as object download conditions.

You can use Bucket.GetObjectDetailedMeta to Obtain the ETag.

```language-go
package main

import (
	"fmt"
	"os"
	"time"
	"github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
	// Create an OSSClient instance.
	client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// Obtain the bucket.
	bucket, err := client.Bucket("<yourBucketName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// Configure time conditions.
	date := time.Date(2015, time.November, 10, 23, 0, 0, 0, time.UTC)

	// The condition is not met, and the object will not be downloaded.
	err = bucket.GetObjectToFile("<yourObjectName>", "LocalFile", oss.IfModifiedSince(date))
	if err == nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// The conditions are met, and the object will be downloaded.
	err = bucket.GetObjectToFile("<yourObjectName>", "LocalFile", oss.IfUnmodifiedSince(date))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

