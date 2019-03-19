# LiveChannel常见问题 {#concept_ynn_1qq_dgb .concept}

本文主要介绍 LiveChannel 中遇到的常见问题及解决方案。

## OSS livechannel 推流过程 {#section_pzt_vtr_2gb .section}

下图为 OSS livechannel 推流过程图解，了解这个过程，有助于排查 livechannel 推流过程中遇到的问题。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80008/155296020535194_zh-CN.png)

更多详情请参考：

-   [生成推流 URL](intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannel.md#)
-   [设置推流状态](intl.zh-CN/API 参考/关于LiveChannel的操作/PutLiveChannelStatus.md#)

## 案例：录制 M3u8 缺失 {#section_rzt_vtr_2gb .section}

问题分析：默认录制成品的 m3u8 只有最后 3 片，遵循的是 HLS 协议的默认规则。

解决方案：用 `PostVodPlaylist` 接口将指定时间范围内的 ts文件汇聚到一个 m3u8 索引内来解决。

**说明：** 

-   `EndTime` 必须大于 `StartTime`，且时间跨度不能大于 1 天。
-   OSS 会查询指定时间范围内的所有指定 LiveChannel 推流生成的 ts 文件，并将其拼装为一个播放列表。

## 案例：录制 m3u8 文件失败 {#section_w12_twr_2gb .section}

问题分析：录制的 m3u8 文件成功推流到来 OSS 才算录制成功。

解决方案：可以在客户端抓包查看是否有 publish succees 的标志，有这个标志才表示和 OSS 音频包交互成功。所以发现客户端推流有记录，但是就是没有录制视频的情况，可以抓包分析一下。

## 案例：客户端无法推流到 OSS {#section_vzt_vtr_2gb .section}

使用 ffmpeg 推流不成功：

```
ffmpeg -re -i 0_20180525105430445.aac -acodec aac -strict -2 -f flv rtmp://xxx.oss-cn-beijing.aliyuncs.com/live/test_1000?Expires=1540458859&OSSAccessKeyId=LTAlujianb6C9z&Signature=qwh31xQsanmao6ygCFJgovNIg%3D&playlistName=playlist.m3u8
```

解决方案：

-   使用 ffmpeg 推流不成功的时候建议用最原始的命令推流，不要加一些复杂的参数。
-   推流 URL 在有 & 符号时请将整个 URL 用 双引号（""） 囊括起来。例如，ffmpeg -re -i 0\_20180525105430445.aac -acodec aac -strict -2 -f flv "rtmp://xxx.oss-cn-beijing.aliyuncs.com/live/test\_1000?Expires=1540458859&OSSAccessKeyId=LTAlujianb6C9z&Signature=qwh31xQsanmao6ygCFJgovNIg%3D&playlistName=playlist.m3u8"。
-   尝试更换成 obs 推流测试，确认是否因 ffmpeg 问题导致的推流失败；

## 案例：录制 M3u8 文件卡顿 {#section_pbc_lxr_2gb .section}

转储类型为 HLS 时，写入当前 ts 文件的音视频数据时长达到`FragDuration` 指定的时长后，OSS 会在收到下一个关键帧的时候切换到下一个 ts 文件。如果 `max(2*FragDuration, 60s)` 后仍未收到下一个关键帧，OSS 强制切换文件，此时可能引起播放时卡顿。

## 案例：录制 M3u8 文件没有音频或视频 {#section_yzt_vtr_2gb .section}

以下几种情况会出现音频或者视频没有录制的情况：

-   `AVC header` 或者 `AAC header` 没有发送，可以通过抓包查看。
-   `RTMP message` 长度小于2，或者 `sequence header` 非常小。
-   音频 `Message` 超过缓冲区大小。
-   `codec_ ctx` 是解码上下文的关键信息，如果携带的音视频数据异常，也会导致录制失败。

## 案例：ffmpeg 推流到 OSS 录制没有音频 {#section_f15_vtr_2gb .section}

-   直接查看 ffmpeg 记录的日志，确定客户端是否有发送 `aac_header`。
-   在客户端抓个 RTMP 的包，查看是否发送了 `aac_header`。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80008/155296020535199_zh-CN.png)

