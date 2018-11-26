# Streaming download {#concept_88617_zh .concept}

## Download an object to a stream. {#section_ak3_hpw_kfb .section}

Run the following code to download a specified object to a stream:

```language-go
package main

import (
	"fmt"
	"os"
	"io/ioutil"
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

	// Download an object to a stream.
	body, err := bucket.GetObject("<yourObjectName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	// If you do not close the reader after the data is read, connection leaks may occur. Consequently, no connection is available and the program cannot run properly.
	defer body.Close()

	data, err := ioutil.ReadAll(body)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	fmt.Println("data:", string(data))
}

```

## Download an object to the local cache { .section}

Run the following code to download a specified object to the local cache:

```language-go
package main

import (
	"fmt"
	"os"
	"io"
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

	// Download the object to the local cache.
	body, err := bucket.GetObject("<yourObjectName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	defer body.Close()

	buf := new(bytes.Buffer)
	io.Copy(buf, body)
	fmt.Println("buf:", buf)
}

```

## Download an object to a local file stream { .section}

Run the following code to download a specified object to a local file stream:

```language-go
package main

import (
	"fmt"
	"os"
	"io"
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

	// Download an object to a local file stream.
	body, err := bucket.GetObject("<yourObjectName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	defer body.Close()

	fd, err := os.OpenFile("LocalFile", os.O_WRONLY|os.O_CREATE, 0660)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1) 
	}
	defer fd.Close()

	io.Copy(fd, body)
}

```

