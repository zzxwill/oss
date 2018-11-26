# Resumable upload {#concept_88603_zh .concept}

You can use resumable upload to split a file you want to upload into several parts and upload them simultaneously. After you have uploaded all parts, you can combine these parts into a complete object. Thus, you can complete uploading the entire object.

Current upload progress is recorded into a checkpoint file during the upload. If a part fails to be uploaded during the process, the object is uploaded from the part recorded in the checkpoint file when the upload restarts, that is, the resumable upload. After the upload is complete, the checkpoint is deleted.

**Note:** 

-   The SDK records the upload progress information in the checkpoint file. Therefore, you must ensure that the program has the write permission to the checkpoint file. Do not modify the checkpoint file because it includes verification information. If the checkpoint file is corrupted, all parts will be upload again.
-   If the local file is changed during file upload, all the parts will be uploaded again.

You can use `Bucket.UploadFile` to achieve resumable upload. You can configure the following parameters.

|Parameter|Description|
|:--------|:----------|
|objectKey|Specifies the name of the object uploaded to OSS.|
|filePath|Specifies the path of the local file.|
|partSize|Specifies the upload part size, ranging from 100 KB to 5 GB.|
|options|Specifies the options, including:-   Routines: Specifies the number of parts that are uploaded at the same time. The default value is 1, which means that parts are not uploaded concurrently.
-   Checkpoint: Specifies whether the resumable upload is enabled and configures the checkpoint file. Resumable upload is disabled by default. For example, `oss.Checkpoint(true, "")` indicates that the resumable upload is enabled, and the checkpoint file is the `file.cp` stored in the directory that the local file needs to be uploaded is stored. The `file` is the name of the local file. You can also use `oss.Checkpoint(true, "your-cp-file.cp")` to specify the checkpoint file.
-   Other metadata information: See [Manage Object Meta](reseller.en-US/SDK Reference/Go/Manage objects/Manage Object Meta.md#).

|

Run the following code for resumable upload:

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

	// The part size is 100 KB. Parts are uploaded through three coroutines concurrently. The resumable upload is enabled.
	// <yourObjectName>" is the objectKey, "LocalFile" is the filePath, and 100*1024 is the partSize.
	err = bucket.UploadFile("<yourObjectName>", "LocalFile", 100*1024, oss.Routines(3), oss.Checkpoint(true, ""))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

