# Quick start {#concept_32145_zh .concept}

This topic describes how to use OSS Go SDK to perform routine operations such as bucket creation, object uploads, and object downloads.

## Create a bucket {#section_xlg_c2w_kfb .section}

Run the following code to create a bucket:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func handleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}
func main() {
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint := "http://oss-cn-hangzhou.aliyuncs.com"
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    accessKeyId := "<yourAccessKeyId>"
    accessKeySecret := "<yourAccessKeySecret>"
    bucketName := "<yourBucketName>"
    // Create an OSSClient instance.
    client, err := oss.New(endpoint, accessKeyId, accessKeySecret)
    if err ! = nil {
        handleError(err)
    }
    // Create a bucket.
    err = client.CreateBucket(bucketName)
    if err ! = nil {
        handleError(err)
    }
}
```

For more information about endpoints, see [Regions and endpoints](../../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).

## Upload objects { .section}

Run the following code to upload a file to OSS:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func handleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}
func main() {
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint := "http://oss-cn-hangzhou.aliyuncs.com"
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    accessKeyId := "<yourAccessKeyId>"
    accessKeySecret := "<yourAccessKeySecret>"
    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"
    localFileName := "<yourLocalFileName>"
    // Create an OSSClient instance.
    client, err := oss.New(endpoint, accessKeyId, accessKeySecret)
    if err ! = nil {
        handleError(err)
    }
    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        handleError(err)
    }
    // Upload a file.
    err = bucket.PutObjectFromFile(objectName, localFileName)
    if err ! = nil {
        handleError(err)
    }
}
```

## Download objects { .section}

Run the following code to download an object to local:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func handleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}
func main() {
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint := "http://oss-cn-hangzhou.aliyuncs.com"
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    accessKeyId := "<yourAccessKeyId>"
    accessKeySecret := "<yourAccessKeySecret>"
    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"
    downloadedFileName := "<yourDownloadedFileName>"
    // Create an OSSClient instance.
    client, err := oss.New(endpoint, accessKeyId, accessKeySecret)
    if err ! = nil {
        handleError(err)
    }
    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        handleError(err)
    }
    // Download the object.
    err = bucket.GetObjectToFile(objectName, downloadedFileName)
    if err ! = nil {
        handleError(err)
    }
}
```

## List objects { .section}

Run the following code to list objects in a specified bucket. You can list a maximum of 100 objects by default.

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func HandleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}
func main() {
    // Create an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        HandleError(err)
    }
    // Obtain the bucket.
    bucketName := "<yourBucketName>"
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        HandleError(err)
    }
    // List objects.
    marker := ""
    for {
        lsRes, err := bucket.ListObjects(oss.Marker(marker))
        if err ! = nil {
            HandleError(err)
        }
        // Print the listed objects. A maximum of 100 objects are listed by default. 
        for _, object := range lsRes.Objects {
            fmt.Println("Bucket: ", object.Key)
        }
        if lsRes.IsTruncated {
            marker = lsRes.NextMarker
        } else {
            break
        }
    }
}
```

## Delete objects { .section}

Run the following code to delete a specified object:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func handleError(err error) {
    fmt.Println("Error:", err)
    os.Exit(-1)
}
func main() {
    // This example uses endpoint China (Hangzhou). Specify the actual endpoint based on your requirements.
    endpoint := "http://oss-cn-hangzhou.aliyuncs.com"
    // It is highly risky to log on with AccessKey of an Alibaba Cloud account because the account has permissions on all the APIs in OSS. We recommend that you log on as a RAM user to access APIs or perform routine operations and maintenance. To create a RAM account, log on to https://ram.console.aliyun.com.
    accessKeyId := "<yourAccessKeyId>"
    accessKeySecret := "<yourAccessKeySecret>"
    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"
    // Create an OSSClient instance.
    client, err := oss.New(endpoint, accessKeyId, accessKeySecret)
    if err ! = nil {
        handleError(err)
    }
    // Obtain the bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        handleError(err)
    }
    // Delete the object.
    err = bucket.DeleteObject(objectName)
    if err ! = nil {
        handleError(err)
    }
}
```

