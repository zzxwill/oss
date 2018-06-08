# OSS常用工具汇总 {#concept_owg_knn_vdb .concept}

OSS除了控制台还有以下常用工具，可以帮助您更高效的使用OSS。

|工具|简介|备注|
|:-|:-|:-|
|[ossbrowser](intl.zh-CN/常用工具/ossbrowser.md#)|图形化的Object管理工具。支持Windows、Linux、Mac平台。|官方工具。 提供类似Windows资源管理器的功能。用户可以方便的浏览文件、上传下载文件、支持断点续传等。|
|[ossutil](intl.zh-CN/常用工具/ossutil/下载和安装.md#)|命令行管理工具。提供方便、简洁、丰富的Object管理命令。|官方工具，支持Windows, Linux, Mac平台，不依赖于任何第三方组件，下载后即用不需要安装。|
|[osscmd](intl.zh-CN/常用工具/osscmd/快速安装.md#)|命令行管理工具。提供完备的Bucket、object管理命令。|官方工具。基于Python2.5 - 2.7版本，支持多平台。将逐步被ossutil替代，除非需要ossutil不具备的Bucket管理功能外，强烈推荐使用[ossutil](https://help.aliyun.com/document_detail/50452.html)。|
|[ossfs](intl.zh-CN/常用工具/ossfs/快速安装.md#)|挂载bucket到本地文件系统，能够通过本地文件系统操作OSS 上的对象，实现数据的访问和共享。|官方工具。支持Linux平台。|
|[ossftp](intl.zh-CN/常用工具/ossftp/如何快速安装OSS FTP.md#)|FTP工具，使用FTP协议来管理OSS的object，可以使用FileZilla、WinSCP、FlashFXP等FTP客户端操作OSS。OSSFTP本质是FTP Server, 接收FTP请求，将对文件、文件夹的操作映射为对OSS的操作。|官方工具。基于Python2.7及以上，支持Windows、Linux、Mac平台。|
|[ossimport](intl.zh-CN/常用工具/ossimport/说明及配置.md#)|数据同步工具。可以将本地或第三方云存储服务上的文件同步到OSS上。|官方工具。依赖JRE7及以上。支持Windows、Linux平台。|
|[可视化图片服务工具](https://bbs.aliyun.com/read/239565.html)|可以直观的看到OSS图片服务处理的结果，图片处理的调试不可或缺的工具。|第三方工具。网页版。支持浏览器Chrome、Firefox、Safari。|
|[可视化签名工具](https://bbs.aliyun.com/read/233851.html)|可视化签名工具。可以调试签名遇到的问题。当您实现OSS的签名中遇到问题，请跟该工具的签名对比，错误立现。|第三方工具。网页版。支持浏览器Chrome、Firefox、Safari。|
|[RAM Policy Editor](http://gosspublic.alicdn.com/ram-policy-editor/index.html)|OSS授权策略自动化生产工具。当您需要生成自己的授权策略时，强烈推荐使用该工具。|官方工具。网页版。支持浏览器Chrome、Firefox、Safari。|
|[ossprobe](intl.zh-CN/常用工具/ossprobe.md#)|网络问题检查工具。常见错误给出建议。当您的环境访问OSS出错误时，推荐首先使用该工具检查。|官方工具。支持Windows、Linux、Mac。|
|[oss-emulator](https://github.com/aliyun/oss-emulator)|轻量级的OSS服务模拟器，用于基于OSS应用的调试、性能测试等。|官方工具。支持Windows、Linux、Mac。|

