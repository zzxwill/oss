# Progress bars {#concept_88606_zh .concept}

You can use progress bars to indicate the upload or download progress.

The following code is used as an example to describe how to view progress information with Bucket.PutObjectFromFile:

```language-go
package main

import (
	"fmt"
	"os"
	"github.com/aliyun/aliyun-oss-go-sdk/oss"
)

// Define a progress bar listener.
type OssProgressListener struct {
}

// Define the progress event handler.
func (listener *OssProgressListener) ProgressChanged(event *oss.ProgressEvent) {
	switch event.EventType {
	case oss.TransferStartedEvent:
		fmt.Printf("Transfer Started, ConsumedBytes: %d, TotalBytes %d.\n",
			event.ConsumedBytes, event.TotalBytes)
	case oss.TransferDataEvent:
		fmt.Printf("\rTransfer Data, ConsumedBytes: %d, TotalBytes %d, %d%%.",
			event.ConsumedBytes, event.TotalBytes, event.ConsumedBytes*100/event.TotalBytes)
	case oss.TransferCompletedEvent:
		fmt.Printf("\nTransfer Completed, ConsumedBytes: %d, TotalBytes %d.\n",
			event.ConsumedBytes, event.TotalBytes)
	case oss.TransferFailedEvent:
		fmt.Printf("\nTransfer Failed, ConsumedBytes: %d, TotalBytes %d.\n",
			event.ConsumedBytes, event.TotalBytes)
	default:
	}
}

func main() {
	// Create an OSSClient instance.
	client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	bucketName := "<yourBucketName>"
	objectName := "<yourObjectName>"
	localFile := "<yourLocalFile>"

	// Obtain a bucket.
	bucket, err := client.Bucket(bucketName)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// Upload with a progress bar displayed.
	err = bucket.PutObjectFromFile(objectName, localFile, oss.Progress(&OssProgressListener{}))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

