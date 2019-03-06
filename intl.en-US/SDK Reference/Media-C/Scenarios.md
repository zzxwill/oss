# Scenarios {#concept_32166_zh .concept}

The previous two sections described relative operations on the [client](reseller.en-US/SDK Reference/Media-C/Client operations.md#) and on the [server](reseller.en-US/SDK Reference/Media-C/Server operations.md#). This section explains how to connect the client and server for use. We recommend, that you to read previous two sections before you proceed with this section.

## Video monitoring { .section}

In the preface, we have introduced the devices like network cameras and its functions and usage are described as follows:

-   Procedure

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22609/155186934813710_en-US.png)

    This function involves the three roles such as the network camera, the app server, and Alibaba Cloud Object Storage Service \(OSS\). The network camera uses the client part of the OSS MEDIA C SDK and the app server uses the server part of the OSS C MEDIA SDK.

    Following is the procedure to upload a video clip captured by a network camera:

    1.  To upload a video clip on the OSS, the network camera sends a network request to the app server to get authorization.
    2.  After the app server receives the request, it performs a check to confirm that the upload permission can be granted to the network camera, and then requests a token with a specific validity period and with only the upload permission from Alibaba Cloud through the get\_token interface of the OSS MEDIA C SDK.
    3.  After Alibaba Cloud receives the request from the app server to get a token, it checks the user configuration. If the check verifies that the request can be allowed, it issues a temporary token \(including the temporary AccessKey ID, the temporary AccessKey Secret, and a temporary STS token\) with only the upload permission to the OSS and only validity for a specified duration and sends the token to the app server.
    4.  The app server forwards the token to the network camera once the temporary token is received.
    5.  After the network camera gets the token, you can upload video files to the OSS through the oss\_media\_write interface of the OSS MEDIA C SDK client.
    6.  You can also use the server part of the C SDK or use Java, C\#, Go, Python, PHP, Ruby SDKs on the app server to implement an HTTP service. This helps the other users to view and manage various video files on the webpage.
-   Example code

    A simple example project simulating operations on the client and on the server is as follows:

    ```language-c
    char* global_temp_access_key_id = NULL;
    char* global_temp_access_key_secret = NULL;
    char* global_temp_token = NULL;
    
    /* Authorization function */
    static void auth_func(oss_media_file_t *file) {
        file->endpoint = "your endpoint";
        file->is_cname = 0; 
        file->access_key_id = global_temp_access_key_id;
        file->access_key_secret = global_temp_access_key_secret;
        file->token = global_temp_token; 
    
        /* Effective time of this authorization */
        file->expiration = time(NULL) + 300;
    }
    
    /* Simulate sending a token to the client from the server */
    static void send_token_to_client(oss_media_token_t token) {
        global_temp_access_key_id = token.tmpAccessKeyId;
        global_temp_access_key_secret = token.tmpAccessKeySecret;
        global_temp_token = token.securityToken;
    }
    
    void get_and_use_token() {
        oss_media_token_t token;
    
        /* Server logic: Get a temporary token from Alibaba Cloud and send it to the client */
        {
            int ret;
            char *policy = NULL;
            oss_media_config_t config;
        
            policy = "{\n"
                     "\"Statement\": [\n"
                     "{"
                     "\"Action\": \"oss:*\",\n"
                     "\"Effect\": \"Allow\",\n"
                     "\"Resource\": \"*\"\n"
                     "}\n"
                     "],\n"
                     "\"Version\": \"1\"\n"
                     "}\n";
    
            init_media_config(&config);
    
    	/* Request a temporary authorization token from Alibaba Cloud */
            ret = oss_media_get_token_from_policy(&config, policy,
                     17 * 60, &token);
    
    	if (ret ! = 0) {
    	   printf ("Get token failed.");
    	   return;
    	}
    
    	/* Simulate sending the temporary token to the client */
            send_token_to_client(token);
        }
    
        /* Client logic: Use the temporary token to operate on files after the temporary token is obtained from the server */
        {
            int ret;
            int64_t write_size = 0;
            oss_media_file_t *file = NULL;
            char *content = NULL;
    	char *bucket_name;
    	char *object_key;
            oss_media_file_stat_t stat;
    
            content = "hello oss media file\n";
    	bucket_name = "<your bucket name>";
    	object_key = "key";
    
            /* Open the object */
            file = oss_media_file_open(bucket_name, object_key, "w", auth_func);
            if (file ! = NULL) {
    	    printf ("open file failed.");
    	    return;
    	}
    
            /* Write an object */
            write_size = oss_media_file_write(file, content, strlen(content));
            if (write_size ! = strlen(content)) {
    	    printf ("write file failed.");
    	    return;
    	}
    
            /* Close the object and release the resource */
            oss_media_file_close(file);
        }
    }
    
    ```

    **Note:** 

    -   Log on to the [RAM console](https://ram.console.aliyun.com/#/overview). Click Roles, from the left-side navigation pane. Locate the role and click Manage. Go to the Basic Information page, the policy is available.
    -   If precise control of permissions is not required, you can use the simpler oss\_media\_get\_token interface. The path parameter can be /\*, and the mode parameter can be rwa.
    -   For more information, see the [RAM and STS Guide](../../../../../reseller.en-US/Developer Guide/Hide/Access control/Overview.md#).

