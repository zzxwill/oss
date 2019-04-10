# Installation {#concept_32043_zh .concept}

This topic describes how to install OSS Android SDK.

## Environment requirements {#section_q4s_p2f_lfb .section}

-   Android system version: 2.3 or later
-   You must have an Alibaba Cloud account and activate the OSS service.

## Download OSS Android SDK {#section_kzv_nrg_4fb .section}

-   Add OSS Android SDK.

    Add the following dependency in Android Studio:

    ```
    dependencies { compile 'com.aliyun.dpa:oss-android-sdk:+' }
    ```

-   Download OSS Android SDK from [Github](https://github.com/aliyun/aliyun-oss-android-sdk).

## Install OSS Android SDK {#section_ssw_vm1_1gb .section}

-   Method 1 \(recommended\): Add the following dependency in your Maven project.

    ```
    dependencies {
    	compile 'com.aliyun.dpa:oss-android-sdk:+'
    }
    
    ```

-   Method 2: Compile the source code into a jar package.
    1.  Clone the source code of the project and run the `gradle` command to compile the code into a jar package:

        ```
        # Clone the project.
        $ git clone https://github.com/aliyun/aliyun-oss-android-sdk.git
        
        # Enter the directory of the project.
        $ cd aliyun-oss-android-sdk/oss-android-sdk/
        
        # Run the package script. Note that jdk 1.7 is required.
        $ ../gradlew releaseJar
        
        # Enter the directory where the generated jar package is stored.
        $ cd build/libs && ls
        
        ```

    2.  Directly introduce the generated jar package.

        Introduce the following three jar packages to the libs directory: aliyun-oss-sdk-android-x.x.x.jar, [okhttp-3.x.x.jar](https://square.github.io/okhttp/#download), and [okio-1.x.x.jar](https://search.maven.org/remote_content?g=com.squareup.okio&a=okio&v=LATEST).


## Related settings {#section_cbs_gl1_hhb .section}

-   Permission settings

    The following permissions are required to use the OSS Android SDK.

    ```
    <uses-permission android:name="android.permission.INTERNET" />
    <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />
    <uses-permission android:name="android.permission.MOUNT_UNMOUNT_FILESYSTEMS" />
    
    ```

    **Note:** Ensure the preceding permissions are set in your AndroidManifest.xml file to let SDK work normally.

-   Proguard settings

    Add the following code to the Proguard settings:

    ```
    -keep class com.alibaba.sdk.android.oss.** { *; }
    -dontwarn okio.**
    -dontwarn org.apache.commons.codec.binary. **
    
    ```


