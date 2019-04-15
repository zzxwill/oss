# HLS基础接口 {#concept_32164_zh .concept}

OSS MEDIA C SDK 客户端部分支持将接收到的H.264、AAC格式封装为TS、M3U8格式，然后写到OSS上，用户通过对应的m3u8地址就可以欣赏视频音频了。

## 接口 {#section_q5m_gzq_lfb .section}

HLS相关基础接口都位于oss\_media\_hls.h中，目前提供的接口有：

-   oss\_media\_hls\_open
-   oss\_media\_hls\_write\_frame
-   oss\_media\_hls\_begin\_m3u8
-   oss\_media\_hls\_write\_m3u8
-   oss\_media\_hls\_end\_m3u8
-   oss\_media\_hls\_flush
-   oss\_media\_hls\_close

下面详细介绍各个接口的功能和注意事项。

-   基础结构体介绍

    ```language-c
    /**
     *  OSS MEDIA HLS FRAME的元数据
     */
    typedef struct oss_media_hls_frame_s {
        stream_type_t stream_type;
        frame_type_t frame_type;
        uint64_t pts;
        uint64_t dts;
        uint32_t continuity_counter;
        uint8_t  key:1;
        uint8_t *pos;
        uint8_t *end;
    } oss_media_hls_frame_t;
    
    /**
     *  OSS MEDIA HLS的描述信息
     */
    typedef struct oss_media_hls_options_s {
        uint16_t video_pid;
        uint16_t audio_pid;
        uint32_t hls_delay_ms;
        uint8_t encrypt:1;
        char    key[OSS_MEDIA_HLS_ENCRYPT_KEY_SIZE];
        file_handler_fn_t handler_func;
        uint16_t pat_interval_frame_count;
    } oss_media_hls_options_t;
    
    /**
     *  OSS MEDIA HLS FILE的描述信息
     */
    typedef struct oss_media_hls_file_s {
        oss_media_file_t *file;
        oss_media_hls_buf_t *buffer;
        oss_media_hls_options_t options;
        int64_t frame_count;
    } oss_media_hls_file_t;
    					
    ```

    **说明：** 

    -   stream\_type：流类型，目前支持st\_h264和st\_aac。
    -   frame\_type：帧类型，目前支持ft\_non\_idr、ft\_idr、ft\_sei、ft\_sps、ft\_pps、ft\_aud等。
    -   pts：显示时间戳。
    -   dts：解码时间戳。
    -   continuity\_counter：递增计数器，从0-15，起始值不一定取0，但必须是连续的。
    -   key：是否为关键帧。
    -   pos：当前帧数据的起始位置\(包含\)。
    -   end：当前帧数据的结束位置\(不包含\)。
    -   video\_pid：视频的pid。
    -   audio\_pid：音频的pid。
    -   hls\_delay\_ms：延迟毫秒数。
    -   encrypt：是否使用AES-128加密。目前暂不支持使用AES-128加密。
    -   key：使用加密时的秘钥。目前暂不支持使用加密时的秘钥。
    -   handler\_func：文件操作回调函数。
    -   pat\_interval\_frame\_count：隔多少帧插入一个pat、mpt表。
-   打开HLS文件

    ```language-c
    /**
     *  @brief  打开一个OSS HLS文件
     *  @param[in]  bucket_name oss上存储文件的存储空间名称
     *  @param[in]  object_key  oss上的文件名称
     *  @param[in]  auth_func   授权函数，设置access_key_id/access_key_secret等
     *  @return:
     *      返回非NULL时成功，否则失败
     */
    oss_media_hls_file_t* oss_media_hls_open(char *bucket_name, char *object_key, auth_fn_t auth_func);
    					
    ```

    **说明：** 示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_sample.c)。

-   关闭HLS文件

    ```language-c
    /**
     *  @brief  关闭OSS HLS文件
     */
    int oss_media_hls_close(oss_media_hls_file_t *file);
    					
    ```

    **说明：** 示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_sample.c) 。

-   写HLS文件

    ```language-c
    /**
     *  @brief  写H.264或者AAC的一帧数据到oss上
     *  @param[in]  frame         h.264或者aac格式的一帧数据
     *  @param[out] file          hls file
     *  @return:
     *      返回0时表示成功
     *      否则, 表示出现了错误
     */
    int oss_media_hls_write_frame(oss_media_hls_frame_t *frame, oss_media_hls_file_t *file);
    					
    ```

    示例程序：

    ```language-c
    static void write_frame(oss_media_hls_file_t *file) {
        oss_media_hls_frame_t frame;
        FILE    *file_h264;
        uint8_t *buf_h264;
        int     len_h264, i;
        int     cur_pos = -1;
        int     last_pos = -1;
        int     video_frame_rate = 30;
        int     max_size = 10 * 1024 * 1024;
        char    *h264_file_name = "/path/to/example.h264";
    
        /* 读取H.264文件 */
        buf_h264 = calloc(max_size, 1);
        file_h264 = fopen(h264_file_name, "r");
        len_h264 = fread(buf_h264, 1, max_size, file_h264);
    
        /* 初始化frame结构体 */
        frame.stream_type = st_h264;
        frame.pts = 0;
        frame.continuity_counter = 1;
        frame.key = 1;
    
        /* 遍历H.264的数据，抽取出每帧数据，然后写入oss */
        for (i = 0; i < len_h264; i++) {
            /*  判断当前位置是否下一帧数据的开头，也就是当前帧的结尾 */
            if ((buf_h264[i] & 0x0F)==0x00 && buf_h264[i+1]==0x00 
                && buf_h264[i+2]==0x00 && buf_h264[i+3]==0x01) 
            {
                cur_pos = i;
            }
    
    	/* 如果获取到完整的一帧数据，就调用接口转为HLS格式后写入OSS */
            if (last_pos != -1 && cur_pos > last_pos) {
                frame.pts += 90000 / video_frame_rate;
                frame.dts = frame.pts;
                
                frame.pos = buf_h264 + last_pos;
                frame.end = buf_h264 + cur_pos;
    
                oss_media_hls_write_frame(&frame, file);
            }
    
            last_pos = cur_pos;
        }
    
        /* 关闭文件，释放资源 */
        fclose(file_h264);
        free(buf_h264);
    }
    					
    ```

    **说明：** 

    -   示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_sample.c)
    -   如果H.264的数据中缺少Access Unit Delimiter NALs（00 00 00 01 09 xx）时，需要添加这个NAL，否则无法在ipad，iphone，safari上播放。
    -   H.264的帧是通过0xX0，0x00，0x00，0x01分隔的；AAC的帧是通过0xFF，0x0X分隔的。
    -   当前帧为关键帧时，frame.key需要设置为1。
-   写M3U8文件

    ```language-c
    /**
     *  @brief  写M3U8文件的头部数据
     *  @param[in]  max_duration  TS文件最长持续时间
     *  @param[in]  sequence      TS文件起始编号
     *  @param[out] file          m3u8 file
     *  @return:
     *      返回0时表示成功
     *      否则, 返回-1时表示出现了错误
     */
    void oss_media_hls_begin_m3u8(int32_t max_duration, 
                                  int32_t sequence,
                                  oss_media_hls_file_t *file);
    
    /**
     *  @brief  写M3U8文件数据
     *  @param[in]  size          m3u8 item个数
     *  @param[in]  m3u8          m3u8 item的详细数据
     *  @param[out] file          m3u8 file
     *  @return:
     *      返回0时表示成功
     *      否则, 返回-1时表示出现了错误
     */
    int oss_media_hls_write_m3u8(int size,
                                 oss_media_hls_m3u8_info_t m3u8[],
                                 oss_media_hls_file_t *file);
    
    /**
     *  @brief  写M3U8文件的结束符等数据
     *  @param[out] file          m3u8 file
     */
    void oss_media_hls_end_m3u8(oss_media_hls_file_t *file);
    					
    ```

    示例程序：

    ```language-c
    static void write_m3u8() {
        char *bucket_name;
        char *key;
        oss_media_hls_file_t *file;
    
        bucket_name = "<your bucket name>";
        key = "<your m3u8 file name>";
    
        /* 打开一个HLS文件用来写M3U8格式的数据，文件名必须以.m3u8结尾 */
        file = oss_media_hls_open(bucket_name, key, auth_func);
        if (file == NULL) {
            printf("open m3u8 file[%s] failed.", key);
            return;
        }
    
        /* 构造3个ts格式文件的信息 */
        oss_media_hls_m3u8_info_t m3u8[3];
        m3u8[0].duration = 9;
        memcpy(m3u8[0].url, "video-0.ts", strlen("video-0.ts"));
        m3u8[1].duration = 10;
        memcpy(m3u8[1].url, "video-1.ts", strlen("video-1.ts"));
        
        /* 写入M3U8文件
        oss_media_hls_begin_m3u8(10, 0, file);
        oss_media_hls_write_m3u8(2, m3u8, file);
        oss_media_hls_end_m3u8(file);
    
        /* 关闭HLS文件 */
        oss_media_hls_close(file);
    
        printf("write m3u8 to oss file succeeded\n");
    }
    					
    ```

    **说明：** 

    -   目前使用的M3U8版本号是3。
    -   如果是录播，需要在结束的时候调用oss\_media\_hls\_end\_m3u8\(file\)接口写入结束符，否则可能无法播放；如果是直播，则不能调用此接口。
    -   示例代码请参考[GitHub](https://github.com/aliyun/aliyun-media-c-sdk/blob/master/sample/hls_sample.c)
    -   可以通过示例程序观看效果。
    -   Windows平台可以通过[VLC播放器](http://www.videolan.org/vlc/download-windows.html)观看，iPhone、iPad、Mac等可以直接使用Safari观看。

