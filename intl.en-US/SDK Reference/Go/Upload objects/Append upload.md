# Append upload {#concept_88602_zh .concept}

You cannot perform copyObject for appended objects. When you call the `AppendObject`, if the target object does not exist, a new appendable object is created. If the target object exists, the uploaded content is appended to the end of the object.``

Run the following code to upload a file with append upload:

```language-go
package main

import (
	"fmt"
	"os"
	"strings"
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

	var nextPos int64 = 0
	// If the object is appended for the first time, the append position is 0, and the returned value is the position for the next append. The position for the next append is the current length of the object.
	nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue1"), nextPos)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// Start the second append.
	nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectAppendValue2"), nextPos)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

	// You can append content to an object for multiple times.
}

```

You can specify the Object Meta only when you append an object for the first time \(that is, the append postion is 0\).

```language-go
	// Specify the Object Meta in the first append upload.
	nextPos, err = bucket.AppendObject("<yourObjectName>", strings.NewReader("YourObjectValue"), 0, oss.Meta("MyProp", "MyPropVal"))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

```

