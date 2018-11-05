# Installation {#OSS_SDK_0002 .concept}

This topic describes how to install OSS Java SDK.

## Preparation {#section_zmb_2pm_1z .section}

-   Environment requirements

    You must use Java 1.6 or later versions.

-   Check the Java version

    Run `java -version` to check Java version.


## Download SDK {#section_p1g_2db_kfb .section}

-   [Direct download](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92588/APP_zh/1538981565531/aliyun-oss-java-sdk-2.8.3.zip)
-   [Download from GitHub](https://github.com/aliyun/aliyun-oss-java-sdk)
-   [Download previous versions](https://github.com/aliyun/aliyun-oss-java-sdk/releases)

## Install SDK {#section_dqn_fpm_1z .section}

-   Method One: Add dependencies to Maven \(recommended\)

    To use OSS Java SDK in Maven, you only need to add the corresponding dependencies to the pom.xml file. This example uses OSS Java SDK 2.8.3. Add the following content to <dependencies\>:

    ```
    <dependency>
        <groupId>com.aliyun.oss</groupId>
        <artifactId>aliyun-sdk-oss</artifactId>
        <version>2.8.3</version>
    </dependency>
    ```

-   Method Two: Import jar files into Eclipse

    Take OSS Java SDK 2.8.3 as an example. To import jar files into Eclipse, follow these steps:

    1.  Download the [Java SDK](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92588/APP_zh/1538981909662/aliyun_java_sdk_2.8.3.zip).
    2.  Decompress the Java SDK.
    3.  Copy the aliyun-sdk-oss-2.8.3.jar file and all files from the lib folder to your project.
    4.  Right-click your project name in Eclipse. Select **Properties** \> **Java Build Path** \> **Add JARs**.
    5.  Select all jar files you have copied in step 3 and import them to Libraries.
-   Method Three: Import jar files into Intellij IDEA

    Take OSS Java SDK 2.8.3 as an example. To import jar files into Intellij IDEA, follow these steps:

    1.  Download the [Java SDK](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/92588/APP_zh/1538981909662/aliyun_java_sdk_2.8.3.zip).
    2.  Decompress the Java SDK.
    3.  Copy the aliyun-sdk-oss-2.8.3.jar file and all jar files from the lib folder to your project.
    4.  Right-click your project name in Intellij IDEA. Select **File** \> **Project Structure** \> **Modules** \> **Dependencies** \> **+** \> **JARs or directories**.
    5.  Select all jar files you have copied in step 3 and import them to External Libraries.

