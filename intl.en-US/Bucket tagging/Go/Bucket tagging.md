# Bucket tagging {#concept_265987 .concept}

By using the bucket tagging function, you can classify your buckets to manage them. For example, you can view the bills of buckets with specified tags or specify that only buckets with specified tags are returned in ListBucket operations.

-   Only the bucket owner and authorized RAM users can set tags for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   You can add a maximum of 20 tags \(key-value pairs\) for a bucket.
-   The maximum size of a key is 64 bytes. The key of a tag cannot be null and cannot be prefixed with `http://`, `https://`, or `Aliyun`.
-   The maximum size of a tag value is 128 bytes. The value of a tag can be null.
-   The key and value of a tag must be UTF-8 encoded.
-   If you use PutBucketTagging to add tags for a bucket, the original tags added for the bucket are completely overwritten.

## Add tags for a bucket {#section_asr_38j_dqu .section}

You can run the following code to add tags for a bucket:

``` {#codeblock_bvo_avf_ojs}
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Initializes the tags.
  tag1 := oss.Tag{
    Key:   "key1",
    Value: "value1",
  }
  tag2 := oss.Tag{
    Key:   "key2",
    Value: "value2",
  }
  tagging := oss.Tagging{
    Tags: []oss.Tag{tag1, tag2},
  }
  // Add the tags for the bucket.
  err = client.SetBucketTagging("yourBucketName", tagging)
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

## Obtain the tags added for a bucket {#section_5t2_so5_jl8 .section}

You can run the following code to obtain the tags added for a bucket:

``` {#codeblock_pm3_rii_byw}
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains the tags added for the bucket.
  ret, err := client.GetBucketTagging("yourBucketName")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Prints the number of tags added for the bucket.
  fmt.Println("Tag length: ", len(ret.Tags))
}
```

## List buckets with a specified tag {#section_vzl_qwy_q7p .section}

Run the following code to list buckets with a specified tag:

``` {#codeblock_i8j_dm2_5qe}
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Searches for the bucket based on TagKey.
  ret, err := client.ListBuckets(oss.TagKey("yourTaggingKey"))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Prints the bucket information.
  for _, bucket := range ret.Buckets{
    fmt.Println("bucket:", bucket)
  }
  // Searches for the bucket based on TagKey and TagValue.
  // TagValue must be used with TagKey. You can leave the value of TagValue as null. In this case, TagValue is not used as a filter to search for the bucket.
  res, err := client.ListBuckets(oss.TagKey("yourTaggingKey"), oss.TagValue("yourTaggingValue"))
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Prints the bucket information.
  for _, b := range res.Buckets{
    fmt.Println("bucket:", b)
  }
}
```

## Delete the tags added for a bucket {#section_pc6_9dz_682 .section}

You can run the following code to delete the tags added for a bucket:

``` {#codeblock_9v2_onq_jmd}
package main

import (
  "fmt"
  "os"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Deletes the tags added for the bucket.
  err = client.DeleteBucketTagging("yourBucketName")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

