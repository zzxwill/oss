# OSS常用工具汇总 {#concept_owg_knn_vdb .concept}

除控制台外，您还可以使用以下常用工具帮助您更高效地使用OSS。

|工具|简介|备注|
|:-|:-|:-|
|[ossbrowser](intl.zh-CN/常用工具/ossbrowser/快速开始.md#)| 图形化的Object管理工具。

 支持Windows、Linux、Mac平台。

 | 官方工具。

 提供类似Windows资源管理器的功能。您可以方便地浏览文件、上传下载文件、进行断点续传等。

 |
|[ossutil](intl.zh-CN/常用工具/ossutil/下载和安装.md#)| 命令行管理工具。

 提供方便、简洁、丰富的Object管理命令。

 | 官方工具。

 支持Windows、Linux、 Mac平台，不依赖于任何第三方组件，下载后即用不需要安装。

 |
|[osscmd](intl.zh-CN/常用工具/osscmd/快速安装.md#)| 命令行管理工具。

 提供完备的Bucket、Object管理命令。

 | 官方工具。

 基于Python2.5 - 2.7版本，支持多平台。

**说明：** ossutil将逐步取代osscmd。除ossutil不具备某些Bucket管理功能的特殊情况外，通常建议使用[ossutil](intl.zh-CN/常用工具/ossutil/下载和安装.md#)。

 |
|[ossfs](intl.zh-CN/常用工具/ossfs/快速安装.md#)| 挂载bucket到本地文件系统。

 通过本地文件系统操作 OSS 上的Object，实现数据的访问和共享。

 |官方工具。支持Linux平台。

|
|[ossftp](intl.zh-CN/常用工具/ossftp/如何快速安装OSS FTP.md#)| FTP工具。

 使用FTP协议来管理OSS的Object。使用FileZilla、WinSCP、FlashFXP等FTP客户端操作OSS。

 ossftp本质是FTP Server，用于接收FTP请求。将对文件、文件夹的操作映射为对OSS的操作。

 | 官方工具。

 基于Python2.7及以上，支持Windows、Linux、Mac平台。

 |
|[ossimport](intl.zh-CN/常用工具/ossimport/说明及配置.md#)| 数据同步工具。

 将本地或第三方云存储服务上的文件同步到OSS上。

 | 官方工具。

 依赖JRE7及以上。支持Windows、Linux平台。

 |
|[可视化图片服务工具](https://bbs.aliyun.com/read/239565.html)|可以直观地看到OSS图片服务处理的结果，可视化图片服务工具还是图片处理调试中不可或缺的工具。| 第三方工具。

 网页版。支持浏览器Chrome、Firefox、Safari。

 |
|[可视化签名工具](https://bbs.aliyun.com/read/233851.html)| 可视化签名工具。

 调试签名中遇到的问题。当您在实现OSS的签名过程遇到问题，请与该工具的签名对比，以便快速定位该错误。

 | 第三方工具。

 网页版。支持浏览器Chrome、Firefox、Safari。

 |
|[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)| OSS授权策略自动化生产工具。

 **说明：** 当您需要生成自己的授权策略时，通常推荐使用该工具。

 | 官方工具。

 网页版。支持浏览器Chrome、Firefox、Safari。

 |
|[ossprobe](intl.zh-CN/常用工具/ossprobe.md#)| 网络问题检查工具。

 对常见错误提供建议。当您访问OSS出错时，建议首先使用该工具进行检查。

 | 官方工具。

 支持Windows、Linux、Mac。

 |
|[oss-emulator](https://github.com/aliyun/oss-emulator)| 轻量级的OSS服务模拟器。

 适用于基于OSS应用的调试、性能测试等。

 | 官方工具。

 支持Windows、Linux、Mac。

 |

