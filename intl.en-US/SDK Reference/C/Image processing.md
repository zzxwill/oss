# Image processing {#concept_48113_zh .concept}

Image Processing \(IMG\) is a massive, secure, cost-effective and highly reliable image processing service. After source images are uploaded to OSS, you can process images on any Internet device at any anytime, from anywhere through simple RESTful APIs.

For more information about IMG, see [Image Processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#).

For the complete code of IMG, see [GitHub](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_image_sample.c).

## Basic features {#section_brj_5wg_kfb .section}

OSS offers the following IMG features:

-   [Retrieve dominant image tones](../../../../reseller.en-US/Image Processing Guide/Obtain image information/Retrieve dominant image tones.md#)
-   [Format conversion](../../../../reseller.en-US/Image Processing Guide/Convert formats/Format conversion.md#)
-   [Resize images](../../../../reseller.en-US/Image Processing Guide/Resize images.md#)
-   [Incircle](../../../../reseller.en-US/Image Processing Guide/Crop images/Incircle.md#)
-   [Adaptive orientation](../../../../reseller.en-US/Image Processing Guide/Rotate images/Adaptive orientation.md#)
-   [Brightness](../../../../reseller.en-US/Image Processing Guide/Apply effects/Brightness.md#)
-   [Add watermarks](../../../../reseller.en-US/Image Processing Guide/Add watermarks.md#): allows you to add images, texts, or image-text watermarks to another image.
-   [Image processing](../../../../reseller.en-US/Image Processing Guide/Image processing.md#)
-   [Image processing access rules](../../../../reseller.en-US/Image Processing Guide/Image processing access rules.md#): calls multiple IMG functions.

## Usage {#section_gc5_swg_kfb .section}

IMG uses standard HTTP GET. You can configure IMG parameters in QueryString of a URL.

If the ACL of an image object is private read and write, only authorized users are allowed for access.

-   Anonymous access

    You can use the following format in level-3 domains to access a processed image:

    ```
    http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction>,<yourParamValue>
    ```

    |Parameter|Description|
    |:--------|:----------|
    |bucket|Specifies the name of a bucket.|
    |endpoint|Specifies endpoint used to access a region.|
    |object|Specifies the name of an image object.|
    |image|Specifies the reserved identifier for IMG.|
    |action|Specifies the operations on an image, such as scaling, cropping, and rotating.|
    |param|Specifies the parameter that indicates the operation on an image.|

    -   Basic operations

        For example, scale an image to a width of 100 px. Adjust the height based on the ratio.

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

    -   Customized image styles

        You can use the following format in level-3 domains to access a processed image:

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=style/<yourStyleName>
        ```

        -   style: specifies a reserved identifier of a customized image style.
        -   yourStyleName: specifies the name of a custom image style. It is the name specified in the rule that is created on the OSS console.
        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=style/oss-pic-style-w-100
        ```

    -   Cascade operations

        Cascade operations allow you to perform multiple operations on an image sequentially.

        ```
        http://<yourBucketName>.<yourEndpoint>/<yourObjectName>?x-oss-process=image/<yourAction1>,<yourParamValue1>/<yourAction2>,<yourParamValue2>/...
        ```

        Example:

        ```
        http://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100/rotate,90
        ```

    -   HTTPS access

        IMG supports access through HTTPS. Example:

        ```
        https://image-demo.oss-cn-hangzhou.aliyuncs.com/example.jpg?x-oss-process=image/resize,w_100
        ```

-   Authorized access

    Authorized access allows you to customize an image style, HTTPS access, and cascade operations.

    Use the following code to generate a signed URL for IMG:

    ```
    #include "oss_api.h"
    #include "aos_http_io.h"
    const char *endpoint = "<yourEndpoint>";
    const char *access_key_id = "<yourAccessKeyId>";
    const char *access_key_secret = "<yourAccessKeySecret>";
    const char *bucket_name = "<yourBucketName>";
    const char *object_name = "<yourObjectName>";
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
        /* Initialize parameters. */
        aos_string_t bucket;
        aos_string_t object;
        aos_table_t *params = NULL;
        aos_http_request_t *req;
        char *url_str;
        apr_time_t now;
        int64_t expire_time; 
        aos_str_set(&bucket, bucket_name);
        aos_str_set(&object, object_name);
        /* Start image processing. */
        params = aos_table_make(pool, 1);
        apr_table_set(params, OSS_PROCESS, "image/resize,m_fixed,w_100,h_100");
        req = aos_http_request_create(pool);
        req->method = HTTP_GET;
        req->query_params = params;
        /* Specify the expiration time (expire_time) in seconds. */
        now = apr_time_now();
        expire_time = now / 1000000 + 10 * 60;
        /* Generate a signed URL. */
        url_str = oss_gen_signed_url(oss_client_options, &bucket, &object, expire_time, req);
        printf("url: %s\n", url_str);
        /* Release the memory pool, that is, the memories allocated to resources during the request. */
        aos_pool_destroy(pool);
        /* Release the allocated global resources. */
        aos_http_io_deinitialize();
        return 0;
    }
    ```

-   Access with SDK

    You can use SDK to access and process an image object.

    SDK allows you to customize image styles, HTTPS access, and cascade operations.

    -   Basic operations

        Run the following code to perform basic operations on an image:

        ```
        #include "oss_api.h"
        #include "aos_http_io.h"
        const char *endpoint = "<yourEndpoint>";
        const char *access_key_id = "<yourAccessKeyId>";
        const char *access_key_secret = "<yourAccessKeySecret>";
        const char *bucket_name = "<yourBucketName>";
        const char *object_name = "<yourObjectName>";
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
            /* Allocate memories in the memory pool to options. */
            oss_client_options = oss_request_options_create(pool);
            /* Initialize oss_client_options. */
            init_options(oss_client_options);
            /* Initialize parameters. */
            aos_string_t bucket;
            aos_string_t object;
            aos_string_t file;
            aos_table_t *headers = NULL;
            aos_table_t *params = NULL;
            aos_table_t *resp_headers = NULL;
            aos_status_t *resp_status = NULL;
            aos_str_set(&bucket, bucket_name);
            aos_str_set(&object, object_name);
            /* Scale the image.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/resize,m_fixed,w_100,h_100");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-resize.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Crop the image.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/crop,w_100,h_100,x_100,y_100,r_1");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-crop.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Rotate the image.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/rotate,90");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-rotate.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Sharpen the image.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/sharpen,100");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-sharpen.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Add watermarks.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/watermark,text_SGVsbG8g5Zu-54mH5pyN5YqhIQ");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-watermark.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Convert the image format.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/format,png");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-format.png");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Obtain image information.
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/info");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-info.txt");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Release the memory pool, that is, the memories allocated to resources during the request. */
            aos_pool_destroy(pool);
            /* Release the allocated global resources. */
            aos_http_io_deinitialize();
            return 0;
        }
        ```

    -   Customize an image style

        Run the following code to customize an image style:

        ```
        #include "oss_api.h"
        #include "aos_http_io.h"
        const char *endpoint = "<yourEndpoint>";
        const char *access_key_id = "<yourAccessKeyId>";
        const char *access_key_secret = "<yourAccessKeySecret>";
        const char *bucket_name = "<yourBucketName>";
        const char *object_name = "<yourObjectName>";
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
            /* Initialize parameters. */
            aos_string_t bucket;
            aos_string_t object;
            aos_string_t file;
            aos_table_t *headers = NULL;
            aos_table_t *params = NULL;
            aos_table_t *resp_headers = NULL;
            aos_status_t *resp_status = NULL;
            aos_str_set(&bucket, bucket_name);
            aos_str_set(&object, object_name);
            /* Customize the image style. */
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "style/<yourCustomStyleName>");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-custom-style.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Release the memory pool, that is, the memories allocated to resources during the request. */
            aos_pool_destroy(pool);
            /* Release the allocated global resources. */
            aos_http_io_deinitialize();
            return 0;
        }
        ```

    -   Cascade operations

        Run the following code to perform cascade operations on an image:

        ```
        #include "oss_api.h"
        #include "aos_http_io.h"
        const char *endpoint = "<yourEndpoint>";
        const char *access_key_id = "<yourAccessKeyId>";
        const char *access_key_secret = "<yourAccessKeySecret>";
        const char *bucket_name = "<yourBucketName>";
        const char *object_name = "<yourObjectName>";
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
            /* Initialize parameters. */
            aos_string_t bucket;
            aos_string_t object;
            aos_string_t file;
            aos_table_t *headers = NULL;
            aos_table_t *params = NULL;
            aos_table_t *resp_headers = NULL;
            aos_status_t *resp_status = NULL;
            aos_str_set(&bucket, bucket_name);
            aos_str_set(&object, object_name);
            /* Perform cascade operations. */
            params = aos_table_make(pool, 1);
            apr_table_set(params, OSS_PROCESS, "image/resize,m_fixed,w_100,h_100");
            /* Download processed images to a local file. */
            aos_str_set(&file, "example-cascade.jpg");
            resp_status = oss_get_object_to_file(oss_client_options, &bucket, &object, headers, params, &file, &resp_headers);
            if (aos_status_is_ok(resp_status)) {
                printf("get object to file succeeded\n");
            } else {
                printf("get object to file failed\n");  
            }
            /* Release the memory pool, that is, the memories allocated to resources during the request. */
            aos_pool_destroy(pool);
            /* Release the allocated global resources. */
            aos_http_io_deinitialize();
            return 0;
        }
        ```


## IMG tools { .section}

You can use the IMG viewer [ImageStyleViever](https://gosspublic.alicdn.com/image/index.html) to directly view the IMG result.

