# Bucket {#concept_32146_zh .concept}

A bucket serves as a container that stores objects. Objects belong to a bucket.

## Create a bucket {#section_zgm_l1b_kfb .section}

For the complete code of creating a bucket, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/create_bucket.go).

Run the following code to create a bucket:

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

	// Create a bucket. The default storage class and ACL of the created bucket are set to standard and private read respectively.
	err = client.CreateBucket("<yourBucketName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

For more information about bucket naming rules, see naming conventions in [Basic concepts](../../../../reseller.en-US/Developer Guide/Basic concepts.md#). You can specify the ACL and the storage class when you create a bucket, as shown in [Bucket-level permissions](../../../../reseller.en-US/Developer Guide/Manage buckets/Set bucket read and write permissions.md#) and [Storage class](../../../../reseller.en-US/Developer Guide/Storage classes/Introduction to storage classes.md#).

You can run the following code to create a bucket with specified permission:

```language-go
	// Create a bucket and set the bucket ACL to public read (which is private by default).
	err = client.CreateBucket("<yourBucketName>", oss.ACL(oss.ACLPublicRead))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

```

You can run the following code to creates a bucket of the archive class:

```language-go
	err = client.CreateBucket("<yourBucketName>", oss.StorageClass(oss.StorageArchive))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

```

You can run the followign code to create a bucket of the Infrequent Access class:

```language-go
	err = client.CreateBucket("<yourBucketName>", oss.StorageClass(oss.StorageIA))
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}

```

## List buckets { .section}

For the complete code of listing buckets, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/list_buckets.go).

-   List all buckets

    Buckets are displayed in alphabetical order. You can list all buckets or specify a bucket that meets certain conditions.

    Run the following code to list all buckets:

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
    
    	// List all buckets.
    	marker := ""
    	for {
    		lsRes, err := client.ListBuckets(oss.Marker(marker))
    		if err ! = nil {
    			fmt.Println("Error:", err)
    			os.Exit(-1)
    		}
    
    		// 100 buckets are listed by default. 
    		for _, bucket := range lsRes.Buckets {
    			fmt.Println("Bucket: ", bucket.Name)
    		}
    
    		if lsRes.IsTruncated {
    			marker = lsRes.NextMarker
    		} else {
    			break
    		}
    	}
    }
    
    ```

-   List buckets with a specified prefix

    Run the following code to list buckets with a specified prefix:

    ```
    // List buckets by a specified prefix.
        lsRes, err = client.ListBuckets(oss.Prefix("<yourBucketPrefix>"))
        if err ! = nil {
            fmt.Println("Error:", err)
            os.Exit(-1)
        }
        // Print the bucket list.
        fmt.Println("Buckets with prefix: ", lsRes.Buckets)
        for _, bucket := range lsRes.Buckets {
            fmt.Println("Bucket with prefix: ", bucket.Name)
        }
    ```

-   List buckets by marker

    The value of the marker parameter indicates the name of a bucket. Run the following code to list buckets after the specified bucket \(the value of marker\):

    ```language-go
    	// List buckets after the specified marker.
    	lsRes, err = client.ListBuckets(oss.Marker("<yourBucketMarker>"))
    	if err ! = nil {
    		fmt.Println("Error:", err)
    		os.Exit(-1)
    	}
    
    	// Print the bucket list.
    	fmt.Println("My buckets with marker :", lsRes.Buckets)
    	for _, bucket := range lsRes.Buckets {
    		fmt.Println("Bucket with marker: ", bucket.Name)
    	}
    
    ```

-   List a specified number of buckets

    Run the following code to list a specified number \(maxKeys\) of buckets:

    ```language-go
    	// Set the number of buckets that can be listed to 500. The default value is 100. The maximum value is 1,000.
    	lsRes, err = client.ListBuckets(oss.MaxKeys(500))
    	if err ! = nil {
    		fmt.Println("Error:", err)
    		os.Exit(-1)
    	}
    
    	// Print the bucket list. 
    	fmt.Println("My buckets max num:", lsRes.Buckets)
    	for _, bucket := range lsRes.Buckets {
    		fmt.Println("Bucket with maxKeys: ", bucket.Name)
    	}
    
    ```


## Determine whether a bucket exists { .section}

Run the following code to determine whether a specified bucket exists:

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

	// Determine whether a bucket exists.
	isExist, err := client.IsBucketExist("<yourBucketName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	fmt.Println("IsBucketExist result : ", isExist)
}

```

## Configure an ACL for a bucket { .section}

The ACL of a bucket includes the following permissions.

|Permission|Description|Value|
|:---------|:----------|:----|
|Private|The bucket owner and the authorized users can read and write objects in the bucket. Other users cannot perform any operation on the objects.|oss.ACLPrivate|
|Public read|The bucket owner and the authorized users can read and write objects in the bucket. Other users can only read the objects in the bucket. Authorize this permission with caution.|oss.ACLPublicRead|
|Public read-write|All users can read and write objects in the bucket. Authorize this permission with caution.|oss.ACLPublicReadWrite|

For more information about ACL, see [access control](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#) in the developer guide. For the complete code of configure ACL for a bucket, see [GitHub](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/sample/bucket_acl.go).

Use the following code to configure the ACL for a bucket:

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

	// Set the ACL for the bucket to public read.
	err = client.SetBucketACL("<yourBucketName>", oss.ACLPublicRead)
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
}

```

## Obtain the ACL for a bucket { .section}

Use the following code to obtain the ACL for a bucket:

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

	// Obtain the ACL for the bucket
	aclRes, err := client.GetBucketACL("<yourBucketName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	fmt.Println("Bucket ACL:", aclRes.ACL)
}

```

## Obtain the region of a bucket { .section}

Run the following code to obtain the region \(or location\) of a bucket:

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

	// Obtain the region of the bucket:
	loc, err := client.GetBucketLocation("<yourBucketName>")
    if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	fmt.Println("Bucket Location:", loc)
}

```

## Obtain bucket information { .section}

Use the following code to obtain bucket information:

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

	// Bucket information includes regions (region or location), creation dates (CreationDate), permissions (ACL), owners (Owner), and storage class (StorageClass).
	res, err := client.GetBucketInfo("<yourBucketName>")
	if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
	}
	fmt.Println("BucketInfo.Location: ", res.BucketInfo.Location)
	fmt.Println("BucketInfo.CreationDate: ", res.BucketInfo.CreationDate)
	fmt.Println("BucketInfo.ACL: ", res.BucketInfo.ACL)
	fmt.Println("BucketInfo.Owner: ", res.BucketInfo.Owner)
	fmt.Println("BucketInfo.StorageClass: ", res.BucketInfo.StorageClass)
	fmt.Println("BucketInfo.ExtranetEndpoint: ", res.BucketInfo.ExtranetEndpoint)
	fmt.Println("BucketInfo.IntranetEndpoint: ", res.BucketInfo.IntranetEndpoint)
}

```

## Delete a bucket { .section}

Before a bucket is deleted, ensure that all objects in the bucket, and fragments that are generated from multipart upload are deleted.

**Note:** To delete the fragments that are generated from multipart upload, use Bucket.ListMultipartUploads to list all fragments, and then use Bucket.AbortMultipartUpload to delete the fragments. For more information, see [Multipart upload](reseller.en-US/SDK Reference/Go/Upload objects/Multipart upload.md#).

Run the following code to delete a bucket:

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

	// Delete the bucket.
	err = client.DeleteBucket("<yourBucketName>")
    if err ! = nil {
		fmt.Println("Error:", err)
		os.Exit(-1)
    }
}

```

