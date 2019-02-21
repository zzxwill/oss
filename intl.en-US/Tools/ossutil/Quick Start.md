# Quick Start {#concept_cnr_3d4_vdb .concept}

Ossutil allows you to manage OSS data easily using the command line. The current version does not provide complete bucket management and multipart management functions. These functions are available in subsequent versions. If you need these functions, you can use the osscmd command line tool instead.

## Download the tool {#section_cxp_kd4_vdb .section}

-   Current version

    Current version: 1.4.2

-   Runtime environment
    -   Windows/Linux/Mac
    -   Supporting architecture
        -   x86 \(32bit, 64bit\)
-   Download the binary program
    -   \[Linux x86 32bit\] [ossutil32](http://gosspublic.alicdn.com/ossutil/1.4.2/ossutil32)
    -   \[Linux x86 64bit\] [ossutil64](http://gosspublic.alicdn.com/ossutil/1.4.2/ossutil64)
    -   \[Windows x86 32bit\] [ossutil32.zip](http://gosspublic.alicdn.com/ossutil/1.4.2/ossutil32.zip)
    -   \[Windows x86 64bit\] [ossutil64.zip](http://gosspublic.alicdn.com/ossutil/1.4.2/ossutil64.zip)
    -   \[mac x86 64bit\] [ossutilmac64](http://gosspublic.alicdn.com/ossutil/1.4.2/ossutilmac64)
-   Install and use the binary program

    Download the binary program or corresponding compressed package for your operating system and run the binary program. \(If the binary program is not an executable file, run `chmod 755 ossutil` to make it executable.\) That is:

    -   For a Linux system: `./ossutil`
    -   For a Window system, either of the following two methods can be used \(64-bit operating system as an example\):
        -   Decompress the package, double-click the bat file, and enter `ossutil64.exe`.
        -   Decompress the package, run cmd to enter the directory where the binary program resides, and enter `ossutil64.exe`.
    -   For a MAC system: `./ossutilmac64`

## Quick start {#section_z1c_sd4_vdb .section}

-   Set ossutil language

    When running commands of ossutil, you can use the -L option to set the language. The value can be CH or EN, that is, Chinese or English. The value is case insensitive. The default value is CH \(Chinese\).The default language CH \(Chinese\), if CH \(Chinese \), you need to make sure that your system is UTF-8 encoded, otherwise it may display chaotic code.

    For example:

    `./ossutil help ls` is used to display the ls help in the default language.

    `./ossutil help ls -L ch` is used to display the ls help in Chinese.

    `./ossutil help ls -L en` is used to display the ls help in English.

    `./ossutil config -L ch` is used to run an interactive configuration command of ossutil config. The prompt language is Chinese.

    `./ossutil config -L en` is used to run an interactive configuration command of ossutil config. The prompt language is English.

    **Note:** Errors output by ossutil are in English by default, which are not affected by the preceding options.

-   Obtain the command list

    `./ossutil` or `./ossutil help`

    ```
    $./ossutil
    Usage: ossutil [command] [args...] [options...]
    Run ossutil help to display the command help.
    Commands:
      mb              cloud_url [options]
            Creates a bucket.
      ls              [cloud_url] [options]
            Lists buckets or objects.
      rm              cloud_url [options]
            Deletes a bucket or object.
      stat            cloud_url [options]
            Displays the description of a bucket or object.
      set-acl         cloud_url [acl] [options]
            Sets the ACL for a bucket or object.
      set-meta        cloud_url [meta] [options]
            Sets the meta information of the uploaded objects.
      cp              src_url dest_url [options]
            Uploads, downloads, or copies objects.
      restore         cloud_url [options]
            Restores an object from the frozen state to the readable state.
      create-symlink  cloud_url target_url [options]
            Creates a symbolic link.
      read-symlink    cloud_url [options]
            Reads the description of a symbolic link file.
    Additional Commands:
      help            [command]
            Obtains the help document of a command.
      config          [options]
            Creates a configuration file to store configuration items.
      hash            file_url [options]
            Computes the crc64 or MD5 of a local file.
      update          [options]
            Updates ossutil.
    ```

    ```
    $./ossutil  -L en
    Usage: ossutil [command] [args...] [options...]
    Please use 'ossutil help command' to show help of command
    Commands:
      mb              cloud_url [options]
            Make Bucket
      ls              [cloud_url] [options]
            List Buckets or Objects
      rm              cloud_url [options]
            Remove Bucket or Objects
      stat            cloud_url [options]
            Display meta information of bucket or objects
      set-acl         cloud_url [acl] [options]
            Set acl on bucket or objects
      set-meta        cloud_url [meta] [options]
            set metadata on already uploaded objects
      cp              src_url dest_url [options]
            Upload, Download, or Copy Objects
      restore         cloud_url [options]
            Restore Frozen State Object to Read Ready Status
      create-symlink  cloud_url target_url [options]
            Create symlink of object
      read-symlink    cloud_url [options]
            Display meta information of symlink object
    Additional Commands:
      help            [command]
            Get help about commands
      config          [options]
            Create configuration file to store credentials
      hash            file_url [options]
            Get crc64 or md5 of local file
      update          [options]
            Update ossutil
    ```

-   View the help document of a command

    `./ossutil help cmd`You are strongly advised to run the help command to view the help document before running a command.

    ```
    ./ossutil  help config -L ch
    SYNOPSIS
        Creates a configuration file to store configuration items.
    SYNTAX
        ossutil config [-e endpoint] [-i id] [-k key] [-t token] [-L language] [--output-dir outdir] [-c file]
    DETAIL DESCRIPTION
        This command is used to create a configuration file, store customized configuration items in the configuration file, and provide access information when the OSS is accessed using the configuration items. (Whether a command requires configuration items depends on whether it supports the --config-file option. For more information, see the command help.)
        You can specify the path for storing the configuration file. The default path is /home/admin/.ossutilconfig. If the configuration file (for example, a) exists, ossutil stores "a" in a.bak, creates file a again, and writes file a to the configuration. If a.bak already exists, it is overwritten by file "a".
        NOTE:
        (1) If the specified path of the configuration file is not the default path, set the --config-file option to your specified path of the configuration file. (If the --config-file option is not specified, the /home/admin/.ossutilconfig path is read by default when the command is run.)
        (2) Some configuration items can be set using options, such as the --endpoint and --access-key-id options, when a command is run (for more information about the options, see the help for each command). If you specify the options when running a command and configure the information in the configuration file, the priority is options > configuration file.
        (3) If you specify the --endpoint, --access-key-id, --access-key-secret, and --sts-token options when running a command, ossutil does not forcibly require a configuration file.
    Usage:
        This command can be used in 1) interactive mode or 2) non-interactive mode. The interactive mode is recommended because it guarantees higher security.
        1) ossutil config [-c file]
            This mode supports interactive information configuration. Ossutil interactively asks you about the following information:
            (1) config file
                Specifies the path of a configuration file. If you press Enter, ossutil uses the default configuration file in 
            /home/admin/.ossutilconfig.
                If you specify a configuration file, set the --config-file option to the path of your configuration file when running the command. For more information about commands that support the --config-file option, see the help of each command.
            (2) language
                During first configuration (the configuration file does not exist), ossutil requires you to set the language. The value can be CH (Chinese) or EN (English). If you press Enter, ossutil configures the language based on the value of the --language option. If you do not set the --language option, ossutil sets the language to CH by default.
                If a configuration file exists, ossutil configures the language based on the specified language option and language information in the configuration file.
                Ossutil reads the language option from the configuration file during operating. If this option does not exist or is invalid, the ossutil sets the language to CH by default.
                NOTE: This configuration item takes effect after the config command is successfully run. When the config command is executed, the displayed language is not affected by your configuration.
            (3) endpoint, accessKeyID, accessKeySecret
                Enter indicates that a configuration item is skipped. NOTE: The endpoint must be a second-level domain (SLD), for example, oss.aliyuncs.com.
                The preceding options are required.
            (4) stsToken
                To access the OSS using a temporary token, specify this option. Otherwise, press Enter to skip this option.
            (5) outputDir
                This option is used to configure the path of the directory where the output files reside. In interactive mode, configuration of this option is not supported. However, this option is valid in the configuration file.
                The default directory of the outputDir option is ossutil_output of the current directory. Ossutil generates all output files in this folder during operating. Currently, the output files include the report files that record operation errors of each file when exceptions occur for batch operations by running the cp command.
                For more information about the outputDir option and report files, see the cp command help.
                NOTE: If the outputDir option does not exist, ossutil automatically creates the directory when generating output files. If the outputDir option exists but is not a directory, an error is reported.
            The following interactive Bucket-Endpoint and Bucket-Cname options are removed, but they are still valid in the configuration file.
            (6) Bucket-Endpoint
                The Bucket-Endpoint option is used to independently configure the endpoint for each specified bucket. This option is before the default endpoint configuration in the configuration file.
                In this version, ossutil removes the Bucket-Endpoint pair configuration in interactive mode. However, this configuration item is still valid in the configuration file. Therefore, if you want to independently specify the endpoint for each bucket, you can make configuration in the configuration file. NOTE: The endpoint must be an SLD, for example, oss.aliyuncs.com.
                If the Bucket-Endpoint option is specified, ossutil searches for the endpoint corresponding to a bucket in the option when performing operations on the bucket. If being found, the endpoint overwrites the endpoint in the basic configuration. However, if the --endpoint option is specified when the command is run, the --endpoint option has the highest priority.
            (7) Bucket-Cname
                The Bucket-Cname option is used to independently configure the CNAME domain name (CDN domain) for each specified bucket. This option is before the configurations of the Bucket-Endpoint option and endpoint in the configuration file.
                In this version, ossutil removes the Bucket-Cname pair configuration in interactive mode. However, this configuration item is still valid in the configuration file. Therefore, if you want to independently specify the CNAME domain name for each bucket, you can make configuration in the configuration file.
                If the Bucket-Cname option is specified, ossutil searches for the CNAME domain name corresponding to a bucket in the option when performing operations on the bucket. If being found, the CNAME domain name overwrites the endpoints in the Bucket-Endpoint option and basic configuration. However, if the --endpoint option is specified when the command is run, the --endpoint option has the highest priority.
            Priority: --endpoint > Bucket-Cname > Bucket-Endpoint > endpoint > default endpoint
        2) ossutil config options
            If you specify any options except the --language and --config-file options when running the command, the command enters the non-interactive mode. All configuration items are specified using options.
    Configuration file format:
        [Credentials]
            language = CH
            endpoint = oss.aliyuncs.com
            accessKeyID = your_key_id
            accessKeySecret = your_key_secret
            stsToken = your_sts_token
            outputDir = your_output_dir
        [Bucket-Endpoint]
            bucket1 = endpoint1
            bucket2 = endpoint2
            ...
        [Bucket-Cname]
            bucket1 = cname1
            bucket2 = cname2
            ...
    SAMPLE
        ossutil config
        ossutil config -e oss-cn-hangzhou.aliyuncs.com -c ~/.myconfig
    OPTIONS
        -c, --config-file
            Specifies the configuration file path of ossutil. Ossutil reads configuration from the configuration file during startup and writes configuration to the file using the config command.
        -e, --endpoint
            Specifies the basic endpoint configuration of ossutil (the option value overwrites the corresponding settings in the configuration file). It must be an SLD.
        -i, --access-key-id
            Specifies the AccessKeyID used to access the OSS (the option value overwrites the corresponding settings in the configuration file).
        -k, --access-key-secret
            Specifies the AccessKeySecret used to access the OSS (the option value overwrites the corresponding settings in the configuration file).
        -t, --sts-token
            Specifies the STSToken used to access the OSS (the option value overwrites the corresponding settings in the configuration file). It is optional.
        --output-dir=ossutil_output
            Specifies the directory in which output files are located. The output files include the report files generated when errors occur for copying files in batches using the cp command. (For more information about the report files, see the cp command help.) The default value is the ossutil_output sub-directory in the current directory.
        -L CH, --language=CH
            Specifies the language of ossutil. The value can be CH or EN, and the default value is CH. If the value is CH, make sure that your system is UTF-8 encoded.
    ```

-   Configure ossutil

    When using a command to access the OSS, configure the AccessKey pair first. For more information about the AccessKey pair, see [RAM and STS introduction](../../../../../reseller.en-US/Developer Guide/Hide/Access control/What is RAM and STS.md#).

    ossutil can be configured to interactive mode or non-interactive mode.

    To view the help document of the configuration command, run `ossutil help config`.

    -   Configure ossutil in interactive mode

        `./ossutil config`

        ```
        $./ossutil config -L ch
        This command is used to create a configuration file and store configuration information in it.
        You can specify the path for storing the configuration file. The default path is /home/admin/.ossutilconfig. If you press Enter, the default path is used. If you specify another path, set the --config-file option to this path when running the command.
        ```

    -   Configure ossutil in non-interactive mode

        ```
        ./ossutil config -e oss.aliyuncs.com -i your_id -k your_key
        ```


