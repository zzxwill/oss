# FAQ {#concept_ol2_dpb_wdb .concept}

-   Q: ossfs适合什么样的程序？
    -   ossfs能把oss bucket挂载到本地，如果您使用的软件没有支持OSS，但您又想让数据能自动同步到OSS，那么ossfs是很好的选择。
-   Q: ossfs有什么局限性？
    -   由于数据需要经过网络同步到云端，ossfs在性能和功能上可能与本地文件系统有差距。如果您想让数据库等对io要求很高的应用跑在ossfs挂载的盘上，请慎重考虑。和本地文件系统具体差异：
        -   随机或者追加写文件会导致整个文件的重写。
        -   元数据操作，例如list directory，性能较差，因为需要远程访问OSS服务器。
        -   文件/文件夹的rename操作不是原子的。
        -   多个客户端挂载同一个oss bucket时，依赖用户自行协调各个客户端的行为。例如避免多个客户端写同一个文件等等。
        -   不支持hard link。
-   Q: ossfs一定要阿里云的机器才能用么？
    -   ossfs不限制一定要阿里云的内网才可以使用，外网机器依然可以使用。
-   Q: ossfs能不能同时挂载多个OSS bucket？
    -   可以的，在passwd-ossfs文件中写入多个OSS配置信息即可。支持不同帐号的OSS。
-   Q: 我在yum/apt-get安装ossfs，遇到conflicts with file from package fuse-devel的错误，请问是怎么回事？
    -   您的系统中存在老版本的fuse，请先使用相关的包管理器卸载，再重新安装ossfs。
-   Q: ossfs工作不正常，如何debug？
    -   您可以使用在挂载时，加上-d -o f2参数，ossfs会把日志写入到系统日志中。在centos系统中，在/var/log/messages中。
    -   您也可以在挂载时使用-f -d -o f2参数，ossfs会把日志输出到屏幕上。
-   Q: 为什么我在mount时遇到 ossfs: unable to access MOUNTPOINT /tmp/ossfs: Transport endpoint is not connected这样的错误？
    -   请先umount对应的目录。
    -   请检查您在使用ossfs挂载时，填入的url参数是否正确，是否和bucket/access key id/access key secret匹配。
    -   特别注意：url中不包含bucket的名字。例如：您在oss控制台中看到bucket的域名是这样的：`ossfs-test-1.oss-cn-hangzhou.aliyuncs.com`。那么填入的url则是：`http://oss-cn-hangzhou.aliyuncs.com`。
-   Q: ossfs提示ossfs: unable to access MOUNTPOINT /tmp/odat: No such file or directory
    -   这是您未创建该目录导致的，在挂载前需要创建对应目录。
-   Q: 我把bucket挂载到本地后，ls目录，却收到operation not permitted错误，这是为什么？
    -   请检查您的bucket中，是否包含目录名含有不可见字符的OSS object。文件系统对文件/目录名有更严格的限制，因此会收到上述错误。使用其他工具对这些object重命名后，ls就能正确显示目录内容了。
-   Q: 我的一个目录下有非常多的文件，为什么ls该目录很慢？
    -   假设一个目录下有n个文件，那么ls该目录至少需要n次OSS http requests。在文件非常多的时候，这可能造成严重的性能问题。
    -   您可以采用下面两个办法优化：
        -   通过-omax\_stat\_cache\_size=xxx参数增大stat cache的size，这样第一次ls会较慢，但是后续的ls就快了，因为文件的元数据都在本地cache中。默认这个值是1000，大约消耗4MB内存，请根据您机器内存大小调整为合适的值。
        -   使用ls -f命令，这个命令会消除与OSS的n次http请求。
        -   具体参见[issue 13](https://github.com/aliyun/ossfs/issues/13)
-   Q: ossfs挂载时如何设置权限？
    -   如果要允许其他用户访问挂载文件夹，可以在运行ossfs的时候指定`allow_other`参数：
        -   ```
ossfs your_bucket your_mount_point -ourl=your_endpoint -o
                  allow_other
```

    -   为什么使用allow\_other参数，仍然不能访问文件？
        -   注意：allow\_other是赋予挂载目录其他用户访问的权限，不是里面的文件！如果您要更改文件夹中的文件，请用chmod命令。
    -   allow\_other默认赋予挂载目录777权限，我想让挂载目录的权限为770，该怎么办？
        -   可以通过umask来设置，参见[这里](https://github.com/aliyun/ossfs/issues/5)。
-   Q: 如果要使挂载的文件夹\(/tmp/ossfs\)属于某个user:

    -   方法一： 如果要使挂载的文件夹\(/tmp/ossfs\)属于某个user，则需要以user的身份创建挂载文件夹和使用ossfs：
        -   `sudo -u user mkdir /tmp/ossfs`
        -   `sudo -u user ossfs bucket-name /tmp/ossfs`
    -   方法二： 首先通过id命令获得指定用户的uid/gid信息。例如获取www用户的uid/gid信息：id www；然后挂载时指定uid/gid参数：

        -   ```
ossfs your_bucket your_mountpoint -ourl=your_url -ouid=your_uid
                    -ogid=your_gid
```

        注意：uid/gid都是数字。

-   Q: 我不是root用户，如何umount ossfs挂载的目录
    -   fusermount -u your\_mountpoint
-   Q: 如何开机自动挂载ossfs？
    -   Step 1 首先请参考使用说明，把bucket name，access key id/secret等信息写入/etc/passwd-ossfs，并将该文件权限修改为640。
        -   ```
echo your_bucket_name:your_access_key_id:your_access_key_secret >
                    /etc/passwd-ossfs
```

        -   `chmod 640 /etc/passwd-ossfs`
-   Step 2 接下来针对不同的系统版本，设置方式有所不同
    -   Step 2A 通过fstab的方式自动mount（适用于ubuntu14.04, centos6.5）
        -   在/etc/fstab中加入下面的命令
        -   ```
ossfs#your_bucket_name your_mount_point fuse _netdev,url=your_url,allow_other 0
                    0
```

        -   其中上述命令中的your\_xxx信息需要根据您的bucket name等信息填入。
        -   保存/etc/fstab文件。执行`mount -a`命令，如果没有报错，则说明设置正常。
        -   到这一步，ubuntu14.04就能自动挂载了。centos6.5还需要执行下面的命令：
        -   `chkconfig netfs on`
    -   Step 2B 通过开机自启动脚本mount（适用于centos7.0及以上的系统）
        -   在/etc/init.d/目录下建立文件ossfs，把[模板文件](https://github.com/aliyun/ossfs/blob/master/scripts/automount.template)中的内容拷贝到这个新文件中。并将其中的your\_xxx内容改成您自己的信息。
        -   执行命令：`chmod a+x /etc/init.d/ossfs`
        -   上述命令是把新建立的ossfs脚本赋予可执行权限。您可以执行该脚本，如果脚本文件内容无误，那么此时oss中的bucket已经挂载到您指定的目录下了。
        -   执行命令：`chkconfig ossfs on`
        -   上述命令是把ossfs启动脚本作为其他服务，开机自动启动。
        -   好了，现在ossfs就可以开机自动挂载了。总结起来，如果您是ubuntu14.04和centos6.5，您需要执行Step 1 ＋ Step 2A；如果您是centos7.0系统，您需要执行Step 1 ＋ Step 2B。
-   Q: 遇到fusermount: failed to open current directory: Permission denied错误如何解决？
    -   这是fuse的一个bug，它要求当前用户对当前目录（非挂载目录）有读权限。解决的办法就是cd到一个有读权限的目录再运行ossfs命令
-   Q: 我需要以www用户挂载ossfs，此时如何设置开机自动挂载？

    -   参照上面的问题的解答，Step 1照做，对Step 2B稍加修改，修改`/etc/init.d/ossfs`中的命令为：

        `sudo -u www ossfs your_bucket your_mountpoint -ourl=your_url`

    -   设置自启动脚本中允许使用sudo，编辑/etc/sudoers，将其中的`Defaults requiretty`这行改为`#Defaults requiretty`（注释掉）
-   Q: 遇到`fusermount: failed to open current directory: Permission denied`错误如何解决？
    -   这是[fuse的一个bug](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=584541)，它要求当前用户对当前目录（非挂载目录）有读权限。解决的办法就是cd到一个有读权限的目录再运行ossfs命令。

-   Q: 使用ECS挂载ossfs，如何避免因后台程序扫描文件而产生费用？

    -   程序扫描ossfs挂载的目录，会转换成向OSS的请求，如果请求次数很多，会产生费用（1分钱/1万次）。如果是[updatedb](https://linux.die.net/man/8/updatedb)，可以通过修改/etc/updatedb.conf让它跳过。具体做法是：

        1.  在`PRUNEFS =`后面加上`fuse.ossfs`
        2.  在`PRUNEPATHS =`后面加上挂载的目录
    -   如何确定是哪个进程扫了我的目录？
        1.  首先安装auditd: sudo apt-get install auditd
        2.  启动auditd: sudo service auditd start
        3.  设置监视挂载目录: auditctl -w /mnt/ossfs
        4.  在auditlog中可以查看是哪些进程访问了这个目录：ausearch -i | grep /mnt/ossfs
-   Q: 使用ossfs上传到OSS的文件Content-Type全是”application/octet-stream”是怎么回事？
    -   ossfs通过查询/etc/mime.types中的内容来确定文件的Content-Type，请检查这个文件是否存在，如果不存在，则需要添加：
        1.  对于ubuntu可以通过sudo apt-get install mime-support来添加
        2.  对于centos可以通过sudo yum install mailcap来添加
        3.  也可以手动添加，每种格式一行，每行格式为：application/javascript js
-   Q: 如何使用supervisor启动ossfs？

1.  安装supervisor，在ubuntu中执行sudo apt-get install supervisor
2.  建立一个目录，编辑ossfs的启动脚本：

    ```
    mkdir /root/ossfs_scripts
    vi /root/ossfs_scripts/start_ossfs.sh
    ```

    写入如下数据：

    ```
    # 卸载
    fusermount -u /mnt/ossfs
    # 重新挂载，必须要增加-f参数运行ossfs，让ossfs在前台运行
    exec ossfs my-bucket my-mount-point -ourl=my-oss-endpoint -f
    ```

3.  编辑/etc/supervisor/supervisord.conf，在最后加入下面一段：

    ```
    [program:ossfs]
    command=bash /root/ossfs_scripts/start_ossfs.sh
    logfile=/var/log/ossfs.log
    log_stdout=true
    log_stderr=true
    logfile_maxbytes=1MB
    logfile_backups=10
    ```

4.  运行supervisor：

    ```
    supervisord
    ```

    supervisord

5.  确认一切正常：

    ```
    ps aux | grep supervisor # 应该能看到supervisor进程
    ps aux | grep ossfs # 应该能看到ossfs进程
    kill -9 ossfs # 杀掉ossfs进程，supervisor应该会重启它, 不要使用killall, 因为killall发送SIGTERM，进程正常退出，supervisor不再去重新运行ossfs
    ps aux | grep ossfs # 应该能看到ossfs进程
    ```

    如果出错，请检查/var/log/supervisor/supervisord.log和/var/log/ossfs.log。


-   Q: 遇到”fuse: warning: library too old, some operations may not work”怎么办？

    出现的原因是：ossfs编译时所使用的libfuse版本 比运行时链接到的libfuse版本高。这往往是用户自行安装了libfuse导致的。使用我们提供的rpm包安装ossfs，无需再安装libfuse。

    在CentOS-5.x和CentOS-6.x上我们提供的rpm包里包含了libfuse-2.8.4，如果在运行的时候环境中有libfuse-2.8.3，并且ossfs被链接到了旧版本的fuse上，就会出现上述warning。


1.  如何确认ossfs运行时链接的fuse版本？
    -   运行ldd $\(which ossfs\) | grep fuse
    -   例如结果是”/lib64/libfuse.so.2”，那么通过ls -l /lib64/libfuse\*可以看到fuse的版本
2.  如何让ossfs链接到正确的版本？
    -   首先通过rpm -ql ossfs | grep fuse找到libfuse的目录
    -   例如结果是”/usr/lib/libfuse.so.2”，则通过LD\_LIBRARY\_PATH=/usr/lib ossfs …运行ossfs
3.  我能忽略这个WARNING吗？
    -   最好不要，见这个bug

-   Q: 为什么用ossfs看到的文件信息（例如大小）与其他工具看到的不一致？

因为ossfs默认会缓存文件的元信息（包括大小/权限等），这样就不需要每次ls的时候向OSS发送请求，加快速度。 如果用户通过其他程序（例如SDK/官网控制台/osscmd等）对文件进行了修改，那么有可能在ossfs中看到的文件信息 没有及时更新。

如果想禁止ossfs的缓存，那么可以在挂载的时候加上如下参数：`-omax_stat_cache_size=0`

