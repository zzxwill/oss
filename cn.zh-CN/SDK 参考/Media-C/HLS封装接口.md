# HLS封装接口 {#concept_32165_zh .concept}

OSS MEDIA C SDK 客户端部分支持将接收到的H.264、AAC封装为TS、M3U8格式后写入OSS，除了基础接口外，还提供封装好的录播、直播接口。

## 接口 {#section_sk3_wzq_lfb .section}

HLS相关封装接口都位于oss\_media\_hls\_stream.h中，目前提供的接口有：

-   oss\_media\_hls\_stream\_open
-   oss\_media\_hls\_stream\_write
-   oss\_media\_hls\_stream\_close

下面详细介绍各个接口的功能和注意事项。

-   基础结构体介绍

    ```language-c
    /**
     *  OSS MEDIA HLS STREAM OPTIONS的描述信息
     */
    typedef struct oss_media_hls_stream_options_s {
        int8_t is_live;
        char *bucket_name;
        char *ts_name_prefix;
        char *m3u8_name;
        int32_t video_frame_rate;
        int32_t audio_sample_rate;
        int32_t hls_time;
        int32_t hls_list_size;
    } oss_media_hls_stream_options_t;
    
    /**
     *  OSS MEDIA HLS STREAM的描述信息
     */
    typedef struct oss_media_hls_stream_s {
        const oss_media_hls_stream_options_t *options;
        oss_media_hls_file_t *ts_file;
        oss_media_hls_file_t *m3u8_file;
        oss_media_hls_frame_t *video_frame;
        oss_media_hls_frame_t *audio_frame;
        oss_media_hls_m3u8_info_t *m3u8_infos;
        int32_t ts_file_index;
        int64_t current_file_begin_pts;
        int32_t has_aud;
        aos_pool_t *pool;
    } oss_media_hls_stream_t;
    					
    ```

    **说明：** 

    -   is\_live：是否为直播模式。直播模式时，M3U8文件里面只包含最新的几个ts文件信息。
    -   bucket\_name：存储HLS文件的存储空间名称。
    -   ts\_name\_prefix：TS文件名称的前缀。
    -   m3u8\_name：M3U8文件名称。
    -   video\_frame\_rate：视频数据的帧率。
    -   audio\_sample\_rate：音频数据的采样率。
    -   hls\_time：每个ts文件最大持续时间。
    -   hls\_list\_size：直播模式时在M3U8文件中最多保留的ts文件个数。
-   打开HLS stream文件

    ```language-c
    /**
     *  @brief  打开一个oss hls文件
     *  @param[in]  auth_func   授权函数，设置access_key_id/access_key_secret等
     *  @param[in]  options     配置信息
     *  @return:
     *      返回非NULL时成功，否则失败
     */
    oss_media_hls_stream_t* oss_media_hls_stream_open(auth_fn_t auth_func,
                            const oss_media_hls_stream_options_t *options);
    					
    ```

    **说明：** 示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c?spm=a2c4g.11186623.2.10.753257afD3fSpP&file=hls_stream_sample.c)。

-   关闭HLS stream文件

    ```language-c
    /**
     *  @brief  关闭HLS stream文件
     */
    int oss_media_hls_stream_close(oss_media_hls_stream_t *stream);
    
    					
    ```

    **说明：** 示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c)。

-   写HLS stream文件

    ```language-c
    /**
     *  @brief  写入视频和音频数据
     *  @param[in]  video_buf 视频数据
     *  @param[in]  video_len 视频数据的长度，可以为0
     *  @param[in]  audio_buf 音频数据
     *  @param[in]  audio_len 音频数据的长度，可以为0
     *  @param[in]  stream    HLS stream
     *  @return:
     *      返回0时表示成功
     *      否则, 表示出现了错误
     */
    int oss_media_hls_stream_write(uint8_t *video_buf,
                                   uint64_t video_len,
                                   uint8_t *audio_buf,
                                   uint64_t audio_len,
                                   oss_media_hls_stream_t *stream);
    					
    ```

    示例程序：

    ```language-c
    static void write_video_audio_vod() {
        int ret;
        int max_size = 10 * 1024 * 1024;
        FILE *h264_file, *aac_file;
        uint8_t *h264_buf, *aac_buf;
        int h264_len, aac_len;
        
        oss_media_hls_stream_options_t options;
        oss_media_hls_stream_t *stream;
    
        /* 设置HLS stream的参数值 */
        options.is_live = 0;
        options.bucket_name = "<your buckete name>";
        options.ts_name_prefix = "vod/video_audio/test";
        options.m3u8_name = "vod/video_audio/vod.m3u8";
        options.video_frame_rate = 30;
        options.audio_sample_rate = 24000;
        options.hls_time = 5;
        
        /* 打开HLS stream */
        stream = oss_media_hls_stream_open(auth_func, &options);
        if (stream == NULL) {
            printf("open hls stream failed.\n");
            return;
        }
    
        /* 创建两个buffer用来存储音频和视频数据 */
        h264_buf = malloc(max_size);
        aac_buf = malloc(max_size);
    
        /* 读取一段视频数据和音频数据，然后调用接口写入OSS */
        {
            h264_file = fopen("/path/to/video/1.h264", "r");
            h264_len = fread(h264_buf, 1, max_size, h264_file);
            fclose(h264_file);
    
            aac_file = fopen("/path/to/audio/1.aac", "r");
            aac_len = fread(aac_buf, 1, max_size, aac_file);
            fclose(aac_file);
    
            ret = oss_media_hls_stream_write(h264_buf, h264_len, 
                    aac_buf, aac_len, stream);
            if (ret != 0) {
                printf("write vod stream failed.\n");
                return;
            }
        }
    
        /* 再读取一段视频数据和音频数据，然后调用接口写入OSS */
        {
            h264_file = fopen("/path/to/video/2.h264", "r");
            h264_len = fread(h264_buf, 1, max_size, h264_file);
            fclose(h264_file);
    
            aac_file = fopen("/path/to/audio/1.aac", "r");
            aac_len = fread(aac_buf, 1, max_size, aac_file);
            fclose(aac_file);
    
            ret = oss_media_hls_stream_write(h264_buf, h264_len, 
                    aac_buf, aac_len, stream);
            if (ret != 0) {
                printf("write vod stream failed.\n");
                return;
            }
        }   
    
        /* 写完数据后，关闭HLS stream */
        ret = oss_media_hls_stream_close(stream);
        if (ret != 0) {
            printf("close vod stream failed.\n");
            return;
        }
    
        /* 释放资源 */
        free(h264_buf);
        free(aac_buf);
    
        printf("convert H.264 and aac to HLS vod succeeded\n");
    }
    					
    ```

    **说明：** 

    -   目前的录播、直播接口都支持只有视频、只有音频或同时有音视频等。您可以通过示例程序观看效果。
    -   示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_stream_sample.c)
    -   目前仅提供初级的录播、直播接口。如果您有高级需求，建议模拟这两个接口，并使用基础接口自助实现高级定制功能。

