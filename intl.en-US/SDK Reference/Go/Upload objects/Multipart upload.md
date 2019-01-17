# Multipart upload {#concept_f3c_r55_kfb .concept}

This topic describes how to upload an object in multiple parts

To enable multipart upload, perform the following steps:

1.  Initiate a multipart upload event.

    You can call Bucket.InitiateMultipartUpload to return the globally unique uploadId created in OSS.

2.  Start the multipart upload task.

    You can call Bucket.UploadPart to upload data in multiple parts.

    **Note:** 

    -   Parts with a same uploadId are sequenced by their part numbers. If you have uploaded a part and use the same part number to upload another part, the later part will replace the former part.
    -   The MD5 values of a data part is included in the ETag header and is returned to the user.
    -   OSS SDK automatically configures Content-MD5. OSS calculates the MD5 value of the uploaded data and compares it with the MD5 value calculated by SDK. If the two values are different, the InvalidDigest error code is returned.
3.  Complete the multipart upload task.

    After you upload all parts, call Bucket.CompleteMultipartUpload to combine these parts into a complete object.


The following code is used as a complete example that describes the process of multipart upload:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func main() {
    // Creates an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"
    locaFilename := "<yourLocalFilename>"
    // Obtains a bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    chunks, err := oss.SplitFileByPartNum(locaFilename, 3)
    fd, err := os.Open(locaFilename)
    defer fd.Close()
    // Step 1: Initiates a multipart upload event.
    imur, err := bucket.InitiateMultipartUpload(objectName)
    // Step 2: Uploads parts.
    var parts []oss.UploadPart
    for _, chunk := range chunks {
        fd.Seek(chunk.Offset, os.SEEK_SET)
        // Calls UploadPart to upload each part.
        part, err := bucket.UploadPart(imur, fd, chunk.Size, chunk.Number)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        parts = append(parts, part)
    }
    // Step 3: Completes the multipart upload task.
    cmur, err := bucket.CompleteMultipartUpload(imur, parts)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("cmur:", cmur)
}
```

## Cancel a multipart upload event {#section_hxj_tfx_kfb .section}

Run the following code to cancel a multipart upload event:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func main() {
    // Creates an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Obtains a bucket.
    bucket, err := client.Bucket("<yourBucketName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Initiates a multipart upload event.
    imur, err := bucket.InitiateMultipartUpload("<yourObjectName>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Cancels the multipart upload event.
    err = bucket.AbortMultipartUpload(imur)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
}
```

## List uploaded parts {#section_djc_yfx_kfb .section}

Run the following code to list parts uploaded in a multipart upload event:

```
package main
import (
    "fmt"
    "os"
    "github.com/aliyun/aliyun-oss-go-sdk/oss"
)
func main() {
    // Creates an OSSClient instance.
    client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    bucketName := "<yourBucketName>"
    objectName := "<yourObjectName>"
    locaFilename := "<yourLocalFilename>"
    uploadID := "<yourUploadID>"
    // Obtains a bucket.
    bucket, err := client.Bucket(bucketName)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Splits the local file into multiple parts (3 parts in this example).
    chunks, err := oss.SplitFileByPartNum(locaFilename, 3)
    fd, err := os.Open(locaFilename)
    defer fd.Close()
    // Initiates a multipart upload event.
    imur, err := bucket.InitiateMultipartUpload(objectName)
    uploadID = imur.UploadID
    fmt.Println("InitiateMultipartUpload UploadID: ", uploadID)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Uploads parts.
    var parts []oss.UploadPart
    for _, chunk := range chunks {
        fd.Seek(chunk.Offset, os.SEEK_SET)
        // Calls UploadPart to upload each part.
        part, err := bucket.UploadPart(imur, fd, chunk.Size, chunk.Number)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("UploadPartNumber: ", part.PartNumber, ", ETag: ", part.ETag)
        parts = append(parts, part)
    }
    // Lists uploaded parts based on InitiateMultipartUploadResult
    lsRes, err := bucket.ListUploadedParts(imur)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Prints the uploaded parts. 
    fmt.Println("\nParts:", lsRes.UploadedParts)
    for _, upload := range lsRes.UploadedParts {
        fmt.Println("List PartNumber:  ", upload.PartNumber, ", ETag: " ,upload.ETag, ", LastModified: ", upload.LastModified)
    }
    // Generates InitiateMultipartUploadResult based on objectName and UploadID, and then list all uploaded parts, which applies to scenarios where you know the objectName and UploadID.
    var imur_with_uploadid oss.InitiateMultipartUploadResult
    imur_with_uploadid.Key = objectName
    imur_with_uploadid.UploadID = uploadID
    // Lists uploaded parts.
    lsRes, err = bucket.ListUploadedParts(imur_with_uploadid)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    // Prints the uploaded parts.
    fmt.Println("\nListUploadedParts by UploadID: ", uploadID)
    for _, upload := range lsRes.UploadedParts {
        fmt.Println("List PartNumber:  ", upload.PartNumber, ", ETag: " ,upload.ETag, ", LastModified: ", upload.LastModified)
    }
    // Completes the multipart upload task.
    cmur, err := bucket.CompleteMultipartUpload(imur, parts)
    if err ! = nil {
        fmt.Println("Error:", err)
        os.Exit(-1)
    }
    fmt.Println("cmur:", cmur)
}
```

## List all part upload events for a bucket {#section_dxh_bgx_kfb .section}

Call `Bucket.ListMultipartUploads` to list all ongoing multipart upload events \(events that are initiated but not completed or events that are canceled\). You can configure the following parameters:

|Parameter|Description|
|:--------|:----------|
|Delimiter|Specifies a delimiter of a forward slash \(/\) used to group object names. All objects whose names contain the specified prefix and between which the delimiter occurs for the first time form a group of elements.|
|MaxUploads|Specifies the maximum number of part upload events. The maximum value \(also the default value\) you can set is 1,000.|
|KeyMarker|Lists all multipart upload events in which object names start with the letter that follows the KeyMarker value in alphabetical order. This parameter can be used together with UploadIDMarker to specify the start point of the returned result.|
|Prefix|Specifies the prefix that must be included in the returned object name. If you use a prefix for query, the returned object name will contain the prefix.|
|UploadIDMarker|UploadIDMarker is used together with the KeyMarker parameter to specify the start point of the returned result.-   This parameter is ignored if KeyMarker is not configured.
-   If KeyMarker is configured, the query result contains:
    -   Multipart upload events in which all object names start with the letter that follows the KeyMarker value in alphabetical order.
    -   Multipart upload events in which the object names equal to the value of KeyMarker but the upload IDs of the objects are larger than the value of UploadIDMarker.

|

-   Use default parameters

    ```
    package main
    import (
        "fmt"
        "os"
        "github.com/aliyun/aliyun-oss-go-sdk/oss"
    )
    func main() {
        // Creates an OSSClient instance.
        client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        // Obtains a bucket.
        bucketName := "<yourBucketName>"
        bucket, err := client.Bucket(bucketName)
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        // Lists all multipart upload events.
        keyMarker := ""
        uploadIdMarker := ""
        for {
            // 1000 records are listed at a time by default. 
            lsRes, err := bucket.ListMultipartUploads(oss.KeyMarker(keyMarker), oss.UploadIDMarker(uploadIdMarker))
            if err ! = nil {
                fmt.Println("Error:", err)
                os.Exit(-1)
            }
            // Prints multipart upload events. 
            for _, upload := range lsRes.Uploads {
                fmt.Println("Upload: ", upload.Key, ", UploadID: ",upload.UploadID)
            }
            if lsRes.IsTruncated {
                keyMarker = lsRes.NextKeyMarker
                uploadIdMarker = lsRes.NextUploadIDMarker
            } else {
                break
            }
        }
    }
    ```

-   Run the following code to list multipart upload events with a specified prefix:

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.Prefix("<yourObjectNamePrefix>"))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```

-   Run the following code to list 100 multipart upload events in maximum at a time:

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.MaxUploads(100))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```

-   Run the following code to list multipart upload events with a specified prefix and a maximum number:

    ```
        lsRes, err := bucket.ListMultipartUploads(oss.Prefix("<yourObjectNamePrefix>"), oss.MaxUploads(100))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        fmt.Println("Uploads:", lsRes.Uploads)
    ```


