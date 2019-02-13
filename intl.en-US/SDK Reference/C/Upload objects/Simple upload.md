# Simple upload {#concept_90214_zh .concept}

This topic describes how to use simple upload.

For the complete code of simple upload, see [GitHub](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_put_object_sample.c).

## Upload data from memory to OSS {#section_hzy_5xx_kfb .section}

You can run the following code to upload data from memory to OSS:

```
#include "oss_api.h"
#include "aos_http_io.h"
const char *endpoint = "<yourEndpoint>";
const char *access_key_id = "<yourAccessKeyId>";
const char *access_key_secret = "<yourAccessKeySecret>";
const char *bucket_name = "<yourBucketName>";
const char *object_name = "<yourObjectName>";
const char *object_content = "More than just cloud.";
void init_options(oss_request_options_t *options)
{
    options->config = oss_config_create(options->pool);
    /* Use a char* string to initialize aos_string_t. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether the CNAME is used. The value 0 indicates that the CNAME is not used. */
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
    aos_list_t buffer;
    aos_buf_t *content = NULL;
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_list_init(&buffer);
    content = aos_buf_pack(oss_client_options->pool, object_content, strlen(object_content));
    aos_list_add_tail(&content->node, &buffer);
    /* Upload a file. */
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

## Upload local files { .section}

You can run the following code to upload a local file to OSS:

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
    /* Use a char* string to initialize aos_string_t. */
    aos_str_set(&options->config->endpoint, endpoint);
    aos_str_set(&options->config->access_key_id, access_key_id);
    aos_str_set(&options->config->access_key_secret, access_key_secret);
    /* Determine whether the CNAME is used. The value 0 indicates that the CNAME is not used. */
    options->config->is_cname = 0;
    /* Configure network parameters, such as timeout. */
    options->ctl = aos_http_controller_create(options->pool, 0);
}
int main(int argc, char *argv[])
{
    /* Call the aos_http_io_initialize method in main() to initialize global resources, such as networks and memories. */
    if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
        exit(1);
    }
    /* Memory pool used to manage memories, equivalent to apr_pool_t. The implementation code is included in the apr library. */
    aos_pool_t *pool;
    /* Re-create a memory pool. The second parameter is NULL, indicating that the new pool does not inherit any other memory pool. */
    aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information, such as endpoint, access_key_id, acces_key_secret, is_cname, and curl. */
    oss_request_options_t *oss_client_options;
    /* Allocate the memories in the memory pool to options. */
    oss_client_options = oss_request_options_create(pool);
    /* Initialize oss_client_options. */
    init_options(oss_client_options);
    /*  Initialization parameters. */
    aos_string_t bucket;
    aos_string_t object;
    aos_string_t file;
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL; 
    aos_status_t *resp_status = NULL; 
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);
    aos_str_set(&file, local_filename);
    /* Upload a file. */
    resp_status = oss_put_object_from_file(oss_client_options, &bucket, &object, &file, headers, &resp_headers);
    /* Determine whether the upload is successful. */
    if (aos_status_is_ok(resp_status)) {
        printf("put object from file succeeded\n");
    } else {
        printf("put object from file failed\n");
    }
    /* Release the memory pool, that is, memories allocated to resources during the request. */
    aos_pool_destroy(pool);
    /* Release the allocated global resources. */
    aos_http_io_deinitialize();
    return 0;
}
```

## FAQ: Files with Chinese characters in their names cannot be uploaded. {#section_nt1_34j_3gb .section}

For example, in Windows, you can upload a file with Chinese characters in its name after encoding its name in UTF-8. The example code is as follows:

```
#include "oss_api.h"
#include "aos_http_io.h"
#include <wchar.h>
const char endpoint = "<yourEndpoint>";
const char access_key_id = "<yourAccessKeyId>";
const char access_key_secret = "<yourAccessKeySecret>";
const char bucket_name = "<yourBucketName>";
const char object_name = "<yourObjectName>";
const char local_filename_ch[] = "c:\\test.txt";
void init_options(oss_request_options_t options)
{
　　options->config = oss_config_create(options->pool);
　　/* Use a char* string to initialize aos_string_t. */
　　aos_str_set(&options->config->endpoint, endpoint);
　　aos_str_set(&options->config->access_key_id, access_key_id);
　　aos_str_set(&options->config->access_key_secret, access_key_secret);
　　/* Determine whether the CNAME is used. The value 0 indicates that the CNAME is not used. */
　　options->config->is_cname = 0;
　　/* Used to configure network parameters, such as timeout. */
　　options->ctl = aos_http_controller_create(options->pool, 0);
}

char* multibyte_to_utf8(const char * ch, char *str, int size)
{
　　if (! ch || ! str || size <= 0)
　　	return NULL;

　　int chlen = strlen(ch);
　　int multi_byte_cnt = 0;
　　for (int i = 0; i < chlen - 1; i++) {
    	if ((ch[i] & 0x80) && (ch[i + 1] & 0x80)) {
            i++;
            multi_byte_cnt++;
    	}
　　}
　　if (multi_byte_cnt == 0)
    	return ch;

　　int len = MultiByteToWideChar(CP_ACP, 0, ch, -1, NULL, 0);
　　if (len <= 0)
    	return NULL;
　　wchar_t *wch = malloc(sizeof(wchar_t) * (len + 1));
　　if (! wch)
    	return NULL;
　　wmemset(wch, 0, len + 1);
　　MultiByteToWideChar(CP_ACP, 0, ch, -1, wch, len);

　　len = WideCharToMultiByte(CP_UTF8, 0, wch, -1, NULL, 0, NULL, NULL);
　　if ((len <= 0) || (size < len + 1)) {
    	free(wch);
    	return NULL;
　　}
　　memset(str, 0, len + 1);
　　WideCharToMultiByte(CP_UTF8, 0, wch, -1, str, len, NULL, NULL);
　　free(wch);
　　return str;
}

int main(int argc, char argv[])
{
　　/* Call the aos_http_io_initialize method in main() to initialize global resources, such as networks and memories. */
　　if (aos_http_io_initialize(NULL, 0) ! = AOSE_OK) {
　　	exit(1);
　　}
　　/* Memory pool used to manage memories, equivalent to apr_pool_t. The implementation code is included in the apr library. */
　　aos_pool_t pool;
　　/* Re-create a memory pool. The second parameter is NULL, indicating that the new pool does not inherit any other memory pool. */
　　aos_pool_create(&pool, NULL);
    /* Create and initialize options. This parameter includes global configuration information, such as endpoint, access_key_id, acces_key_secret, is_cname, and curl. */
　　oss_request_options_t oss_client_options;
　　/* Allocate the memories in the memory pool to options. */
　　oss_client_options = oss_request_options_create(pool);
　　/* Initialize oss_client_options. */
　　init_options(oss_client_options);
　　/* Initialization parameters. */
　　aos_string_t bucket;
　　aos_string_t object;
    aos_string_t file;
    aos_table_t *headers = NULL;
    aos_table_t *resp_headers = NULL;
    aos_status_t *resp_status = NULL;
    aos_str_set(&bucket, bucket_name);
    aos_str_set(&object, object_name);

    /* Encode the file name with multiple Chinese characters in UTF-8. */
    char local_filename_buf[1024];
　　char * local_filename = multibyte_to_utf8(local_filename_ch, local_filename_buf, 1024);
　　aos_str_set(&file, local_filename);

　　/* Upload a file. */
    resp_status = oss_put_object_from_file(oss_client_options, &bucket, &object, &file, headers, &resp_headers);
　　/* Determine whether the upload is successful. */
　　if (aos_status_is_ok(resp_status)) {
    	printf("put object from file succeeded\n");
    } else {
    	printf("put object from file failed\n");
　　}
　　/* Release the memory pool, that is, memories allocated to resources during the request. */
　　aos_pool_destroy(pool);
　　/* Release the allocated global resources. */
　　aos_http_io_deinitialize();
　　return 0;
}
```

