# RTMP推流上传 {#concept_vbb_dmb_5db .concept}

OSS支持使用RTMP协议推送H264编码的视频流+AAC编码的音频流到OSS。推送到OSS的音视频数据可以点播播放；在对延迟不敏感的应用场景，也可以做直播用途。

通过RTMP协议上传音视频数据目前有以下限制：

-   只能使用RTMP推流的方式，不支持拉流。
-   必须包含视频流，且视频流格式为H264。
-   音频流是可选的，并且只支持AAC格式，其他格式的音频流会被丢弃。
-   转储只支持HLS协议。
-   一个LiveChannel同时只能有一个客户端向其推流。

下面会分别介绍如何推送音视频流到OSS，以及如何点播，直播播放。

## 向OSS推送音视频数据 {#section_exp_fmb_5db .section}

-   获得推流地址

    使用SDK调用PutLiveChannel接口，创建一个LiveChannel，并获取对应的推流地址。如果Bucket的权限控制（ACL）为 公共读写（public-read-write），那么可以直接使用得到的推流地址进行推流；否则需要进行签名操作。

    以使用python SDK为例，获取未签名以及签名推流地址的代码如下：

    ```
    # 以使用python sdk为例
    from oss2 import *
    from oss2.models import *
    host = "oss-cn-hangzhou.aliyuncs.com" #just for example
    accessid = "your-access-id"
    accesskey = "your-access-key"
    bucket_name = "your-bucket"
    channel_name = "test-channel"
    auth = Auth(accessid, accesskey)
    bucket = Bucket(auth, host, bucket_name)
    channel_cfg = LiveChannelInfo(target = LiveChannelInfoTarget())
    channel = bucket.create_live_channel(channel_name, channel_cfg)
    publish_url = channel.publish_url
    signed_publish_url = bucket.sign_rtmp_url("test-channel", "playlist.m3u8", 3600)
    ```

    获得的推流地址示例如下：

    ```
    publish_url = rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel
    signed_publish_url = rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/your-channel?OSSAccessKeyId=LGarWrijh8HjKWg6&playlistName=t.m3u8&Expires=1472201595&Signature=bjKraZTTyzz9%2FpYoomDx4Wgh%2FlM%3D"
    ```

-   使用ffmpeg进行推流

    可以使用ffmpeg推送本地的视频文件到OSS，命令如下：

    ```
    ffmpeg -i 1.flv -c copy -f flv "rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel?OSSAccessKeyId=LGarWrijh8HjKWg6&Expires=1472199095&Signature=%2FAvRo7FTss1InBKgwn7Gz%2FUlp9w%3D"
    ```

-   使用OBS进行推流

    首先点击 **Settings**，在 URL框中输入前面步骤获取的推流地址，然后点击 **OK** 开始推流即可。

    如下图所示，请注意推流地址的拆分方式：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4367/1068_zh-CN.png)


## 播放推送到OSS的音视频数据 {#section_qss_fnb_5db .section}

-   直播场景

    在推流的过程中，可以通过HLS协议播放正在推送的内容，各个平台的播放方法如下：

    -   在Android、iOS等移动平台，直接在浏览器输入LiveChannel对应的播放地址即可。
    -   Mac OS可以使用safari浏览器进行播放。
    -   PC端可以安装vlc播放器进行播放。
    为了直播流畅，可以设置比较小的FragDuration，例如2s；另外，GOP的大小最好固定且与LiveChannel的FragDuration配置一致。OBS的GOP （即 keyframe Interval）设置方法如下：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4367/1069_zh-CN.png)

-   点播场景

    推流的过程中，OSS总是以直播的方式更新m3u8文件。所以对于点播的场景，需要在推流结束后，调用PostVodPlaylist接口来组装一个点播用的m3u8文件，然后使用该文件地址来播放。

    对于点播的场景，可以设置较大的GOP来减少ts文件数，降低码率。


