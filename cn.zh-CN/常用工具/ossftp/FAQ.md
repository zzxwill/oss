# FAQ {#concept_uys_ylb_wdb .concept}

-   权限问题：
    -   can’t list bucket 导致无法登录

        原因一般是使用的AccessKeyId 和AccessKeySecret属于子账号，而子账号没有list bucket权限。

        如果没有在bucket enpoint一栏配置bucket的访问域名，在通过ossftp访问该bucket时，ftpserver尝试通过get service操作，来获取bucket的region。此时如果用户账号没有list bucket权限，会导致登录失败。

        解决办法是在bucket endpoints中配置对应的三级访问域名，例如bucket-a.oss-cn-hangzhou.aliyuncs.com。

    -   登录成功后list文件报错

        一般是使用的AccessKeyId 和AccessKeySecret属于子账号，而子账号没有list objects \(等价于get bucket\)权限。

-   其他问题：
    -   登录成功后list文件超时导致连接断开

        原因一般是bucket根目录下的文件数或文件夹过多。 登录ftp后，ftpserver会尝试将bucket根目录下的所有文件／文件夹list出来，每次可以list出1000个文件／文件夹。如果根目录下有100万以上的文件／文件夹，则会导致1000次以上的http请求，非常容易导致超时。

    -   运行ftpserver的机器由于端口限制导致数据传输不成功

        由于ftp协议的控制端口和数据端口不同，当ftpserver工作在被动模式下，每当需要传输数据时，ftpserver会打开1个随机端口，等待客户端来连接。所以当ftpserver所在机器有端口限制时，可能会导致数据无法正常传输。

        解决办法是当运行ftpserver.py时，通过指定 --passive\_ports\_start 和 --passive\_ports\_end参数来设置本地端口的起止范围，然后将该范围的端口都打开。

    -   客户端和ftpserver之间的连接经常断开

        各个ftp客户端一般都有超时设置，可以设置为不超时。以FileZilla工具为例，在设置-\>连接里，可以将超时设置为0。


