# Authorized access {#concept_32139_zh .concept}

## Use STS for temporary access authorization {#section_nyk_bzc_kfb .section}

OSS supports Alibaba Cloud Security Token Service \(STS\) for temporary access authorization. STS is a web service that provides a temporary access token to a cloud computing user. Through the STS, you can assign a third-party application or a RAM user \(you can manage the user ID\) an access credential with a custom validity period and permissions. For more information about STS, see [STS introduction](../../../../reseller.en-US/API reference/API reference (STS)/Introduction.md#).

STS advantages:

-   Your long-term key \(AccessKey\) is not exposed to a third-party application. You only need to generate an access token and send the access token to the third-party application. You can customize access permissions and the validity of this token.
-   You do not need to keep track of permission revocation issues. The access token automatically becomes invalid when it expires.

For more information about the process of access to OSS with STS, see [RAM and STS scenario practices](../../../../reseller.en-US/Developer Guide/Access and control/Access control.md#) in OSS Developer Guide.

## Create a signature request with STS { .section}

Run the following code to create a signature request with STS:

```language-c
#include "oss_api.h"
#include "aos_http_io.h"

const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *sts_token = "<yourStsToken>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
const char *object_content = "More than just cloud.";

void init_options(oss_request_options_t *options)
{
	options->config = oss_config_create(options->pool);
	/* Use a char* string to initialize the aos_string_t type. */
	aos_str_set(&options->config->endpoint, endpoint);
	aos_str_set(&options->config->access_key_id, access_key_id);
	aos_str_set(&options->config->access_key_secret, access_key_secret);
	aos_str_set(&options->config->sts_token, sts_token);
	/* Determine whether the CNAME is used. 0 indicates that the CNAME is not used. */
	options->config->is_cname = 0;
	/* Used to configure network parameters, such as timeout. */
	options->ctl = aos_http_controller_create(options->pool, 0);
}

int main(int argc, char *argv[])
{
	/* Call the aos_http_io_initialize method in main() to initialize global resources, such as networks and memories. */
	if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
		exit(1);
	}

	/* Memory pool used to manage memories, which is equivalent to apr_pool_t. The implementation code is included in the apr library. */
	aos_pool_t *pool;
	/* Re-create a new memory pool. The second parameter is NULL, indicating that it does not inherit from any other memory pools. */
	aos_pool_create(&pool, NULL);
	/* Create and initialize options. This parameter mainly includes global configuration information, such as endpoint, access_key_id, acces_key_secret, is_cname, and curl. */
	oss_request_options_t *oss_client_options;
	/* Allocate memories in the memory pool to options. */
	oss_client_options = oss_request_options_create(pool);
	/* Use oss_client_options to initialize client options. */
	init_options(oss_client_options);

    /*  Initialization parameters. */
	aos_string_t bucket;
	aos_string_t object;
	aos_list_t buffer;
	aos_buf_t *content = NULL;
    aos_table_t *headers = NULL;
	aos_table_t *resp_headers = NULL; 
	aos_status_t *resp_status = NULL; 
	/* Assign the char* data to the bucket. */
	aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);

    aos_list_init(&buffer);
    content = aos_buf_pack(oss_client_options->pool, object_content, strlen(object_content));
    aos_list_add_tail(&content->node, &buffer);
	
	/* Upload an object. */
    resp_status = oss_put_object_from_buffer(oss_client_options, &bucket, &object, &buffer, headers, &resp_headers);
    /* Determine whether the upload is successful. */
    if (aos_status_is_ok(resp_status)) {
        printf("put object from buffer succeeded\n");
    } else {
        printf("put object from buffer failed\n");      
    }    

	/* Release the memory pool, that is, memories allocated to resources during the request. */
	aos_pool_destroy(pool);

	/* Release allocated global resources. */
	aos_http_io_deinitialize();

	return 0;
}

```

## Sign a URL to authorize temporary access { .section}

You can provide a signed URL to a visitor for temporary access. When you sign a URL, you can specify the expiration time for a URL to restrict the period of access from visitors.

-   Use a signed URL to upload an object

    Run the following code to upload an object with a signed URL:

    ```
    #include "oss_api.h"
    #include "aos_http_io.h"
    const char *endpoint = "<yourEndpoint>";
    const char *access_key_id = "<yourAccessKeyId>";
    const char *access_key_secret = "<yourAccessKeySecret>";
    const char *bucket_name = "<yourBucketName>";
    const char *object_name = "<yourObjectName>";
    const char *local_filename = "<yourLocalFilename>";
    void init_options(oss_request_options_t *options)
    {
        options->config = oss_config_create(options->pool);
        /* Use a char* string to initialize the aos_string_t type. */
        aos_str_set(&options->config->endpoint, endpoint);
        aos_str_set(&options->config->access_key_id, access_key_id);
        aos_str_set(&options->config->access_key_secret, access_key_secret);
        /* Determine whether the CNAME is used. 0 indicates that the CNAME is not used. */
        options->config->is_cname = 0;
        /* Used to configure network parameters, such as timeout. */
        options->ctl = aos_http_controller_create(options->pool, 0);
    }
    int main(int argc, char *argv[])
    {
        /* Call the aos_http_io_initialize method in main() to initialize global resources, such as networks and memories. */
        if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
            exit(1);
        }
        /* Memory pool used to manage memories, which is equivalent to apr_pool_t. The implementation code is included in the apr library. */
        aos_pool_t *pool;
        /* Re-create a new memory pool. The second parameter is NULL, indicating that it does not inherit from any other memory pools. */
        aos_pool_create(&pool, NULL);
        /* Create and initialize options. This parameter mainly includes global configuration information, such as endpoint, access_key_id, acces_key_secret, is_cname, and curl. */
        oss_request_options_t *oss_client_options;
        /* Allocate memories in the memory pool to options. */
        oss_client_options = oss_request_options_create(pool);
        /* Use oss_client_options to initialize client options. */
        init_options(oss_client_options);
        /* Initialization parameters. */
        aos_string_t bucket;
        aos_string_t object;
        aos_string_t file;
        aos_table_t *headers = NULL;
        aos_table_t *resp_headers = NULL; 
        aos_status_t *resp_status = NULL; 
        aos_http_request_t *req;
        apr_time_t now;
        char *url_str;
        aos_string_t url;
        int64_t expire_time; 
        int one_hour = 3600;
        aos_str_set(&bucket, bucket_name);
        aos_str_set(&object, object_name);
        aos_str_set(&file, local_filename);
        expire_time = now / 1000000 + one_hour;
        headers = aos_table_make(pool, 0);
        req = aos_http_request_create(pool);
        req->method = HTTP_PUT;
        now = apr_time_now();  /* Unit: microseconds */
        expire_time = now / 1000000 + one_hour;
        /* Generate the signed URL. */
        url_str = oss_gen_signed_url(oss_client_options, &bucket, &object, expire_time, req);
        aos_str_set(&url, url_str);
        printf("Temporary upload URL: %s\n", url_str);
        /* Use the signed URL to upload an object. */
        resp_status = oss_put_object_from_file_by_url(oss_client_options, &url, &file, headers, &resp_headers);
        if (aos_status_is_ok(resp_status)) {
            printf("put objects by signed url succeeded\n");
        } else {
            printf("put objects by signed url failed\n");
        }
        /* Release the memory pool, that is, memories allocated to resources during the request. */
        aos_pool_destroy(pool);
        /* Release allocated global resources. */
        aos_http_io_deinitialize();
        return 0;
    }
    ```

-   Use a signed URL to download an object

    Run the following code to upload an object with a signed URL:

    ```
    #include "oss_api.h"
    #include "aos_http_io.h"
    const char *endpoint = "<yourEndpoint>";
    const char *access_key_id = "<yourAccessKeyId>";
    const char *access_key_secret = "<yourAccessKeySecret>";
    const char *bucket_name = "<yourBucketName>";
    const char *object_name = "<yourObjectName>";
    const char *local_filename = "<yourLocalFilename>";
    void init_options(oss_request_options_t *options)
    {
        options->config = oss_config_create(options->pool);
        /* Use a char* string to initialize the aos_string_t type. */
        aos_str_set(&options->config->endpoint, endpoint);
        aos_str_set(&options->config->access_key_id, access_key_id);
        aos_str_set(&options->config->access_key_secret, access_key_secret);
        /* Determine whether the CNAME is used. 0 indicates that the CNAME is not used. */
        options->config->is_cname = 0;
        /* Used to configure network parameters, such as timeout. */
        options->ctl = aos_http_controller_create(options->pool, 0);
    }
    int main(int argc, char *argv[])
    {
        /* Call the aos_http_io_initialize method in main() to initialize global resources, such as networks and memories. */
        if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
            exit(1);
        }
        /* Memory pool used to manage memories, which is equivalent to apr_pool_t. The implementation code is included in the apr library. */
        aos_pool_t *pool;
        /* Re-create a new memory pool. The second parameter is NULL, indicating that it does not inherit from any other memory pools. */
        aos_pool_create(&p, NULL);
        /* Create and initialize options. This parameter mainly includes global configuration information, such as endpoint, access_key_id, acces_key_secret, is_cname, and curl. */
        oss_request_options_t *oss_client_options;
        /* Allocate memories in the memory pool to options. */
        oss_client_options = oss_request_options_create(pool);
        /* Use oss_client_options to initialize client options. */
        init_options(oss_client_options);
        /*  Initialization parameters. */
        aos_string_t bucket;
        aos_string_t object;
        aos_string_t file;
        aos_table_t *headers = NULL;
        aos_table_t *params = NULL;
        aos_table_t *resp_headers = NULL; 
        aos_status_t *resp_status = NULL; 
        aos_http_request_t *req;
        apr_time_t now;
        char *url_str;
        aos_string_t url;
        int64_t expire_time; 
        int one_hour = 3600;
        aos_str_set(&bucket, bucket_name);
        aos_str_set(&object, object_name);
        aos_str_set(&file, local_filename);
        expire_time = now / 1000000 + one_hour;
        headers = aos_table_make(pool, 0);
        params = aos_table_make(pool, 0);
        req = aos_http_request_create(pool);
        req->method = HTTP_GET;
        now = apr_time_now();  /* Unit: microseconds */
        expire_time = now / 1000000 + one_hour;
        /* Generate the signed URL. */
        url_str = oss_gen_signed_url(oss_client_options, &bucket, &object, expire_time, req);
        aos_str_set(&url, url_str);
        /* Use the signed URL to download an object. */
        resp_status = oss_get_object_to_file_by_url(oss_client_options, &url, headers, params, &file, &resp_headers);
        if (aos_status_is_ok(resp_status)) {
            printf("get object succeeded\n");
        } else {
            printf("get object failed\n");
        }
        /* Release the memory pool, that is, memories allocated to resources during the request. */
        aos_pool_destroy(pool);
        /* Release allocated global resources. */
        aos_http_io_deinitialize();
        return 0;
    }
    ```


