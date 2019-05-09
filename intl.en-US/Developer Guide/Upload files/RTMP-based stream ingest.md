# RTMP-based stream ingest {#concept_vbb_dmb_5db .concept}

OSS allows you to use Real-Time Messaging Protocol \(RTMP\) to ingest H.264-encoded video streams and Advanced Audio Coding \(AAC\)-encoded audio streams to OSS. Audio and video data uploaded to OSS can be played on demand or be used for live streaming in latency-insensitive scenarios.

When uploading audio and video data to OSS in compliance with RTMP, you need to note the following limits:

-   By using RTMP, you can only ingest video or audio streams but not pull the streams.
-   You must ingest video streams, which are in H.264 format.
-   Audio streams are optional, which must be in AAC format. OSS discards audio streams in other formats.
-   You can only use HTTP Live Streaming \(HLS\) to dump audio and video data.
-   A LiveChannel can receive streams ingested from only one client at a time.

The following sections describe how to ingest audio and video streams to OSS and how to play the uploaded audio and video data for video-on-demand \(VOD\) playback and live streaming.

## Ingest audio and video streams to OSS {#section_exp_fmb_5db .section}

-   Obtain an ingest URL

    Use the SDK to call the PutLiveChannel API, create a LiveChannel, and obtain the corresponding ingest URL. If the bucket ACL is set to public-read-write, you can directly use the obtained ingest URL. Otherwise, add a signature to the ingest URL.

    The following code uses the Python SDK as an example to show how to obtain ingest URLs without and with a signature, respectively.

    ```
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

    The following example shows the obtained ingest URLs:

    ```
    publish_url = rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel
    signed_publish_url = rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/your-channel? OSSAccessKeyId=LGarxxxxxxHjKWg6&playlistName=t.m3u8&Expires=1472201595&Signature=bjKraZTTyzz9%2FpYoomDx4Wgh%2FlM%3D" 
    ```

-   Use FFmpeg for stream ingest

    You can use FFmpeg to upload local video files to OSS by running the following command:

    ```
    ffmpeg -i 1.flv -c copy -f flv "rtmp://your-bucket.oss-cn-hangzhou.aliyuncs.com/live/test-channel? OSSAccessKeyId=LGarxxxxxxHjKWg6&Expires=1472199095&Signature=%2FAvRo7FTss1InBKgwn7Gz%2FUlp9w%3D"
    ```

-   Use OBS for stream ingest

    Click **Settings**. In the URL field, enter the ingest URL that you obtain in the preceding step, and click **OK**.

    As shown in the following figure, you need to note how the ingest URL is split.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4367/15573940751068_en-US.png)


## Play the audio and video data uploaded to OSS {#section_qss_fnb_5db .section}

-   Live streaming

    During stream ingest, you can use HLS to play the audio and video data that is being uploaded on the following platforms in different ways:

    -   On mobile platforms such as Android and iOS, enter the corresponding streaming URL of the LiveChannel in the browser.
    -   On the macOS platform, use Safari to play the content.
    -   On a PC, install the VLC media player to play the content.
    To smoothly play the uploaded audio and video data for live streaming, you can set FragDuration to a small value, for example, 2s. You can also set the group of pictures \(GOP\) to a fixed value, which is the same as that of FragDuration of the LiveChannel. The following figure shows how to set the GOP \(that is, Keyframe Interval\) in OBS.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4367/15573940751069_en-US.png)

-   VOD playback

    During stream ingest, OSS always uses live streams to push or update M3U8 objects. In VOD playback scenarios, you must call the PostVodPlaylist API after stream ingest to assemble an M3U8 object for VOD playback and use the object URL to play the uploaded audio and video data.

    In VOD playback scenarios, you can set a larger GOP to reduce the number of TS objects and the bit rate.


