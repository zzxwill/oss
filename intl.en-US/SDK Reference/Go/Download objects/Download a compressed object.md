# Download a compressed object {#concept_88624_zh .concept}

This topic describes how to download a compressed object.

You can download a compressed object. Currently, GZIP is supported for object compression. Bucket.GetObject and Bucket.GetObjectToFile support compression in downloads.

```language-go
package main

import (
	"fmt"
	"os"
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

	// Download an compressed object.
	err = bucket.GetObjectToFile("<yourObjectName>", "LocalFile.gzip", oss.AcceptEncoding("gzip"))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

