# Simple upload {#concept_88601_zh .concept}

For the complete code of simple upload, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/put_object.go).

## Upload a string {#section_yn4_4dx_kfb .section}

You can run the following code to upload a string:

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

	// Upload a string.
	err = bucket.PutObject("<yourObjectName>", strings.NewReader("yourObjectValue"))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

## Upload a byte array { .section}

You can run the following code to upload a byte array:

```language-go
package main

import (
	"fmt"
	"os"
	"bytes"
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

	// Upload a byte array.
	err = bucket.PutObject("<yourObjectName>", bytes.NewReader([]byte("yourObjectValueByteArrary")))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
    }
}

```

## Upload a file stream { .section}

You can run the following code to upload a file stream:

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

	// Read the local file.
	fd, err := os.Open("<yourLocalFile>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	defer fd.Close()

	// Upload a file stream.
	err = bucket.PutObject("<yourObjectName>", fd)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

## Upload a local file { .section}

You can run the following code to upload a local file:

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

	// Upload a local file.
	err = bucket.PutObjectFromFile("<yourObjectName>", "<yourLocalFile>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

