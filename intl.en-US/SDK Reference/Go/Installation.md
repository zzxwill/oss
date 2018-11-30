# Installation {#concept_87712_zh .concept}

## Preparation { .section}

-   Environment requirements

    You must use Golang 1.6 and later versions.

    Refer to [Installing Go from source](https://golang.org/doc/install/source) to download and install the compiling and running environment of Go. After Go is installed, set the GOPATH to the path of your code. For more information about GOPATH, run `go help gopath`.

-   Check the version of Go

    Run `go version` to check the version of Go.


## Download SDK { .section}

-    [Download from GitHub](https://github.com/aliyun/aliyun-oss-go-sdk) 
-    [Download previous versions](https://github.com/aliyun/aliyun-oss-go-sdk/releases) 

## Install SDK { .section}

Run the following command to install OSS Go SDK:

```language-bash
go get github.com/aliyun/aliyun-oss-go-sdk/oss

```

**Note:** No messages are printed to the interface during installation. Please wait. In case of installation time-out, run the command again.

## Check SDK version { .section}

Run the following code to check the version of OSS Go SDK:

```language-bash
package main

import (
  "fmt"
  "github.com/aliyun/aliyun-oss-go-sdk/oss"
)

func main() {
  fmt.Println("OSS Go SDK Version: ", oss.Version)
}

```

