# Preface {#concept_32042_zh .concept}

This article describes how to install and use the OSS Android SDK.

## Introduction {#section_klp_h2f_lfb .section}

This document assumes that you have already activated the Alibaba Cloud OSS service and created an AccessKeyID and an AccessKeySecret. In the document, ID refers to the AccessKeyID and KEY indicates the AccessKeySecret.

If you have not yet activated or do not know about the OSS service, log on to the [OSS product page](https://www.alibabacloud.com/product/oss) for more help.

## Environment requirements {#section_q4s_p2f_lfb .section}

-   Android 2.3 or later
-   You must have registered an Alibaba Cloud account with the OSS activated.

## Download the SDK { .section}

-   Android SDK

    Android Studio environment:

    ```
    dependencies { compile 'com.aliyun.dpa:oss-android-sdk:+' }
    ```

-   GitHub address: [click to view details](https://github.com/aliyun/aliyun-oss-android-sdk).
-   Sample address: [click to view details](https://github.com/aliyun/aliyun-oss-android-sdk/tree/master/app/src/main/java/com/alibaba/sdk/android/oss).

## Compile the JAR package from the source code { .section}

You can run the gradle command for packaging after cloning the project source code:

```
# Clone the project.
$ git clone https://github.com/aliyun/aliyun-oss-android-sdk.git

# Enter the directory.
$ cd aliyun-oss-android-sdk/oss-android-sdk/

# Run the packaging script. JDK 1.7 is required.
$ ./gradlew releaseJar

# Enter the directory generated after packaging and the JAR package will be generated in this directory.
$ cd build/libs && ls

```

