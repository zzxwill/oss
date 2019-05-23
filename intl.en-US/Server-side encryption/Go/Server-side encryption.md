# Server-side encryption {#concept_265990 .concept}

OSS supports server-side encryption for uploaded data. This means that when user data is uploaded, OSS encrypts the data and permanently stores the data. Then, when the data is downloaded by a user, OSS automatically decrypts the data, returns the original data to the user, and declares in the header of the response that the data has been encrypted on the server.

The following server-side encryption methods are available for different application scenarios:

-   Server-side encryption that uses CMKs managed by KMS for encryption and decryption \(SSE-KMS\)

    When uploading an object, you can use a specified CMK ID or the default CMK managed by KMS to encrypt and decrypt a large amount of data. This method is cost-effective because you do not need to send user data to the KMS server through networks for encryption and decryption.

    **Note:** API calling fees are incurred if you use CMKs to encrypt or decrypt data.

-   Server-side encryption fully managed by OSS \(SSE-OSS\)

    When you upload an object, OSS encrypts the object on the server side by using AES256 fully managed by OSS. In this method, OSS uses AES256 to encrypt each object with an individual key. Furthermore, the individual keys are encrypted by a CMK that is updated periodically for higher security. This method applies to encrypt or decrypt bulk data.


**Note:** 

-   Only one server-side encryption method can be used for an object at one time.
-   If you configure server-side encryption for a bucket, you can still configure the encryption method for a single object when uploading or copying it. In this case, the encryption method configured for the object takes precedence. For more information, see [PutObject](../../../../intl.en-US/API Reference/Object operations/PutObject.md#table_im4_mkw_bz).
-   For more information about server-side encryption, see [Server-side encryption](https://www.alibabacloud.com/help/doc-detail/119320.html).

## Configure server-side encryption for a bucket {#section_uri_y0t_bml .section}

You can run the following code to configure the default encryption method for a bucket. After the method is configured, all objects that are uploaded to the bucket without their encryption methods configured are encrypted in the configured default encryption method.

``` {#codeblock_alj_33e_qft}
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

  // Initializes an encryption rule. In this example, the encryption method is set to AES256.
  config := oss.ServerEncryptionRule{SSEDefault: oss.SSEDefaultRule{SSEAlgorithm: "AES256"}}
  err = client.SetBucketEncryption("yourBucketName", config)
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}             
```

## Obtain the server-side encryption settings of a bucket {#section_3vl_t28_29d .section}

You can run the following code to obtain the server-side encryption settings of a bucket:

``` {#codeblock_cmd_bti_eep}
package main

import (
  "fmt"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Obtains the server-side encryption rule for the bucket.
  ret, err := client.GetBucketEncryption("yourBucketName")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
  // Prints the obtained encryption rule.
  fmt.Println("Encrypt rule", ret.SSEDefault.SSEAlgorithm)
}
```

## Delete the server-side encryption settings of a bucket {#section_cjv_goy_8pp .section}

You can run the following code to delete the server-side encryption settings of a bucket:

``` {#codeblock_l0o_f7g_mct}
package main

import (
  "fmt"

  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  // Creates an OSSClient instance.
  client, err := oss.New("<yourEndpoint>", "<yourAccessKeyId>", "<yourAccessKeySecret>")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }

  // Deletes the encryption rule.
  err = client.DeleteBucketEncryption("yourBucketName")
  if err != nil {
    fmt.Println("Error:", err)
    os.Exit(-1)
  }
}
```

