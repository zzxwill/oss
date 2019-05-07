# Quick start {#concept_cnr_3d4_vdb .concept}

Ossutil allows you to manage OSS data easily by using command lines and provides simple and easy-to-use commands for bucket and object management. Ossutil supports the following operating systems: Windows, Linux, and Mac.

## Download {#section_sqg_hrh_2nw .section}

-   The latest version of ossutil is 1.6.0.
-   Operating environment:
    -   Windows/Linux/Mac
    -   Supported architecture: x86 \(32bit, 64bit\)
-   Download ossutil from the following URLs according to your operating system:
    -   [Linux x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil32)
    -   [Linux x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil64) 

        **Note:** When copying the preceding URLs into the `wget` command to download data, delete the ?spm=xxxx part in the URLs.

    -   [Windows x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil32.zip)
    -   [Windows x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil64.zip)
    -   [mac x86 32bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutilmac32)
    -   [mac x86 64bit](http://gosspublic.alicdn.com/ossutil/1.6.0/ossutilmac64)

## Installation {#section_f1t_c1h_v4z .section}

Download a package according to your operating system and run the corresponding binary file.

-   Install ossutil in Linux \(Linux 64-bit system is used as an example\).
    1.  Download the ossutil installation file:

        ```
        wget http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil64
        								
        ```

    2.  Modify the permission of the file:

        ```
        chmod 755 ossutil64
        ```

    3.  Generate a configuration file by inputting information in interactive command lines:

        ```
        ./ossutil64 config
        This command generates a configuration file which stores the configuration information. Input the path of the configuration file. (The default path is /home/user/.ossutilconfig. If you press Enter, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path.) 
        The path is not specified, the following default configuration file is used: /home/user/.ossutilconfig
        The following parameters are ignored if you press Enter when configuring them. For more information about the parameters, run "help config". 
        Input the endpoint：http://oss-cn-shenzhen.aliyuncs.com 
        Input the accessKeyID：yourAccessKeyID 
        Input the accessKeySecret：yourAccessKeySecret
        Input the stsToken： 
        ```

        -   **endpoint**: Indicates the endpoint of the region where the bucket belongs to. For more information, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
        -   **accessKeyID**: For more information about how to view the AccessKeyID, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   **accessKeySecret**: For more information about how to view the AccessKeySecret, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   **stsToken**: This is option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave the value unspecified. For more information about how to generate an STS token, see [Temporary access credentials](../../../../reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#section_dvv_hkb_5db).

            **Note:** For more information about the configuration file, see [Modify the configuration file](#config).

-   Install ossutil in Windows \(Windows 64-bit system is used as an example\).
    1.  Download the ossutil installation package.
    2.  Decompress the package to the specified folder, and then double-click the ossutil.bat file.
    3.  Generate the configuration file. For more information about the parameters, see the description in the installation process of Linux.

        ```
        D:\ossutil>ossutil64.exe config
        ```

-   Install ossutil in Mac \(Mac 64-bit system is used as an example\).
    1.  Download the ossutil installation file:

        ```
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.6.0/ossutilmac64
        ```

    2.  Modify the permission of the file:

        ```
        chmod 755 ossutilmac64
        ```

    3.  Generate the configuration file. For more information about the parameters, see the description in the installation process of Linux.

        ```
        ./ossutilmac64 config
        ```


## Installation {#section_hr8_ke0_xym .section}

Download a package according to your operating system and run the corresponding binary file.

-   Install ossutil in Linux \(Linux 64-bit system is used as an example\).
    1.  Download the ossutil installation file:

        ```
        wget http://gosspublic.alicdn.com/ossutil/1.6.0/ossutil64
        								
        ```

    2.  Modify the permission of the file:

        ```
        chmod 755 ossutil64
        ```

    3.  Generate a configuration file by inputting information in interactive command lines:

        ```
        ./ossutil64 config
        This command generates a configuration file which stores the configuration information. Input the path of the configuration file. (The default path is /home/user/.ossutilconfig. If you press Enter, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path.) 
        The path is not specified, the following default configuration file is used: /home/user/.ossutilconfig
        The following parameters are ignored if you press Enter when configuring them. For more information about the parameters, run "help config". 
        Input the endpoint：http://oss-cn-shenzhen.aliyuncs.com 
        Input the accessKeyID：yourAccessKeyID 
        Input the accessKeySecret：yourAccessKeySecret
        Input the stsToken： 
        ```

        -   **endpoint**: Indicates the endpoint of the region where the bucket belongs to. For more information, see [Regions and endpoints](../../../../reseller.en-US/Developer Guide/Endpoint/Regions and endpoints.md#).
        -   **accessKeyID**: For more information about how to view the AccessKeyID, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   **accessKeySecret**: For more information about how to view the AccessKeySecret, see [Create an AccessKey](../../../../reseller.en-US/General Reference/Create an AccessKey.md#).
        -   **stsToken**: This is option is required only when you use a temporary STS token to access the OSS bucket. Otherwise, you can leave the value unspecified. For more information about how to generate an STS token, see [Temporary access credentials](../../../../reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#section_dvv_hkb_5db).

            **Note:** For more information about the configuration file, see [Modify the configuration file](#config)

-   Install ossutil in Windows \(Windows 64-bit system is used as an example\).
    1.  Download the ossutil installation package.
    2.  Decompress the package to the specified folder, and then double-click the ossutil.bat file.
    3.  Generate the configuration file. For more information about the parameters, see the description in the installation process of Linux.

        ```
        D:\ossutil>ossutil64.exe config
        ```

-   Install ossutil in Mac \(Mac 64-bit system is used as an example\).
    1.  Download the ossutil installation file:

        ```
        curl -o ossutilmac64 http://gosspublic.alicdn.com/ossutil/1.6.0/ossutilmac64
        ```

    2.  Modify the permission of the file:

        ```
        chmod 755 ossutilmac64
        ```

    3.  Generate the configuration file. For more information about the parameters, see the description in the installation process of Linux.

        ```
        ./ossutilmac64 config
        ```


## Usage {#section_enu_d4e_omz .section}

You can run commands to perform the following operations in ossutil:

-   View all commands.

    ```
    ./ossutil help
    					
    ```

-   View the help documentation of a specified command.

    ```
    ./ossutil help cmd
    ```

    For example:

    ```
    ./ossutil  help config
    					
    ```

-   Output log files in ossutil.

    In ossutil 1.4.3 and later, you can enable the `--loglevel=${level}` option when running commands to output the log file `ossutil.log` in the current working directory. The value of this option is null by default, which indicates that the log file is not output. You can set this option to two values: info or debug.

    -   Default value: Null, indicating that the log file is not generated.
    -   info: Indicates that operation records in ossutil are recorded in the log file.

        ```
        ./ossutil [command] --loglevel=info
        ```

    -   debug: Indicates that all HTTP access information and the original signature string are recorded in the log file to locate problems.

        ```
        ./ossutil [command] --loglevel=debug
        ```

-   View the version of ossutil.

    ```
    ./ossutil --version
    ```

-   Set the language of ossutil.

    You can use the `-L` option to set the language when running commands. You can set the value of this optional to CH or EN, which indicates Chinese or English individually. The value is not case-sensitive and is CH by default. Make sure your operating system is UTF-8 encoded if you set the language of ossutil to CH. Otherwise, characters may not display properly.

    -   Set the language of ossutil to Chinese:

        ```
        ./ossutil config -L ch
        ```

        **Note:** If you have generated a configuration file, this operation modifies the parameter in the configuration file.

    -   View the help information about the `ls` command in the default language:

        ```
        ./ossutil help ls
        ```

    -   View the help information about the `ls` command in Chinese:

        ```
        ./ossutil help ls -L ch
        ```

    **Note:** Error messages in ossutil are displayed in English and cannot be changed.

-   Modify the configuration file.

    You can modify the configuration file as follows when managing buckets in different regions:

    -   Specify the configuration file.

        ```
        ./ossutil config -c your_config_file_path
        							
        ```

        The configuration file is in the following format:

        ```
        [Credentials]
            language = CH 
            endpoint = oss-cn-xxx.aliyuncs.com
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
        							
        ```

        -   Bucket-Endpoint: Specify an individual endpoint for each specified bucket. If you configure the Bucket-Endpoint option, ossutil searches for the endpoint specified for a bucket when you perform operations on the bucket. If the endpoint specified for the bucket exists, ossutil manages the bucket through the endpoint. Otherwise, ossutil manages the bucket through the endpoint specified in the basic configuration.

            **Note:** You can select the protocol used to access OSS by specifying different formats of endpoints.

            -   If you specify an endpoint in the `oss-cn-xxx.aliyuncs.com` or `http://oss-cn-xxx.aliyuncs.com` format for a bucket, ossutil accesses the bucket through the HTTP protocol.
            -   If you specify an endpoint in the `https://oss-cn-xxx.aliyuncs.com` format for a bucket, ossutil accesses the bucket through the HTTPS protocol.
        -   Bucket-Cname: Specify an individual CDN acceleration domain name for each specified bucket. If you configure the Bucket-Cname option, ossutil searches for the CDN acceleration domain name specified for a bucket when you perform operations on the bucket. If the CDN acceleration domain name specified for the bucket exists, ossutil replaces the endpoint specified in the Bucket-Endpoint option and basic configuration. For more information about CDN acceleration domain names, see [../../../../dita-oss-bucket/SP\_21/DNOSS11827291/EN-US\_TP\_4408.md\#](../../../../reseller.en-US/Developer Guide/Hide/CDN-based OSS acceleration.md#).
        -   Priority of endpoint configurations: endpoint specified by the `--endpoint` parameter in commands \> endpoint specified in Bucket-Cname \> endpoint specified in Bucket-Endpoint \> endpoint specified in the basic configuration \> default endpoint.
    -   Generate a configuration file by inputting information in interactive command lines as follows:

        ```
        ./ossutil config
        This command generates a configuration file which stores the configuration information.
        Input the path of the configuration file. (The default path is /home/user/.ossutilconfig. If you press Enter, the file is generated in the default path. If you want to generate the file in another path, set the --config-file option to the path.) 
        The path is not specified, the following default configuration file is used: /home/user/.ossutilconfig
        The following parameters are ignored if you press Enter when configuring them. For more information about the parameters, run "help config".
        Input the stsToken:
        Input the endpoint: http://oss-cn-xxx.aliyuncs.com
        Input the accessKeyID: yourAccessKeyID
        Input the accessKeySecret: yourAccessKeySecret./ossutil64 config
        							
        ```

    -   Generate a configuration file by running a command as follows:

        ```
        ./ossutil config -e endpoint -i your_id -k your_secret
        							
        ```

    -   The parameters in the command are described as follows:

        ```
        -c, --config-file
            Indicates the path of the configuration file of ossutil. Ossutil reads the configuration from the configuration file when starting and writes the configuration into the file when running the config command.
        
        -e, --endpoint
            Specifies the endpoint in the basic configuration of ossutil. It must be a second level domain name.
        
        -i, --access-key-id
            Specifies the AccessKeyID used to access OSS.
        
        -k, --access-key-secret
            Specifies the AccessKeySecret used to access OSS.
        
        -t, --sts-token
            Specifies the STS token used to access OSS.
        
        --output-dir=ossutil_output
            Specifies the folder that stores the output files, including the report files generated when you run the cp command to copy files in batches.
        
        -L CH, --language=CH
            Specifies the language of ossutil. You can set the language to EN or CH. The default value is CH. If you set the language to CH, make sure your operating system is UTF-8 encoded.
        ```


## Reference {#section_zx6_dvo_tlj .section}

For more information about the commands in ossutil, see the following topics:

-   [Bucket-related commands](reseller.en-US/Tools/ossutil/Bucket-related commands.md#)
-   [Object-related commands](reseller.en-US/Tools/ossutil/Object-related commands.md#)
-   [Multipart-related commands](reseller.en-US/Tools/ossutil/Multipart-related commands.md#)

