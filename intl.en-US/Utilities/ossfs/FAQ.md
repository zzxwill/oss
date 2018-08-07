# FAQ {#concept_ol2_dpb_wdb .concept}

-   Q: For what programs is ossfs suitable?
    -   ossfs mounts OSS buckets locally. If you want a program that does not support OSS to automatically sync the data to the OSS, ossfs is a great option.
-   Q: What are the limitations of ossfs?
    -   Because data must be synced to the cloud over the network, the performance and functions of ossfs may differ from those of local file systems. If you want to run a database or other applications with frequent I/O operations on a mounted ossfs disk, you must consider this carefully. ossfs differs from local file systems in the following ways:
        -   Random write and append operations overwrite the entire file.
        -   The performance of metadata operations, such as list directory, is poor because the system has to remotely access the OSS server.
        -   The file/folder rename operation is not atomic.
        -   When multiple clients are attached to a single OSS bucket, you must coordinate the actions of each client manually. For example, you must avoid multiple clients writing the same file.
        -   Hard link is not supported.
-   Q: Do I need to use Alibaba Cloud hosts for ossfs?
    -   ossfs does not need to be used with Alibaba Cloud intranet. It can be used on external Internet hosts.
-   Q: Can ossfs simultaneously mount multiple OSS buckets?
    -   Yes, write multiple OSS configuration information entries in the passwd-ossfs file. Buckets from different OSS accounts are supported.
-   Q: I installed ossfs at yum/apt-get and has an error: conflicts with file from package fuse-devel.
    -   There is an earlier version of fuse on your system. Please use the relevant package manager to uninstall and then reinstall ossfs.
-   Q: ossfs is not working properly, how do I debug?
    -   You can use the -d -o f2 parameter when mounting. ossfs will write log content into the system logs. On the centos system, in/var/log/messages.
    -   You can also use the -f -d -o f2 parameter when mounting, and ossfs prints the logs to the screen.
-   Q: When trying to mount a bucket, why do I receive the error “ossfs: unable to access MOUNTPOINT /tmp/ossfs: Transport endpoint is not connected”?
    -   First, run the umount command for the corresponding directory.
    -   When mounting with ossfs, check that the entered URL parameter is correct and the bucket, AccessKey ID, and AccessKey secret match.
    -   DO NOT include the bucket name in the URL. For example, if the bucket domain name is `ossfs-test-1.oss-cn-hangzhou.aliyuncs.com` on the OSS console, set the URL to `http://oss-cn-hangzhou.aliyuncs.com`.
-   Q: Why does ossfs display “ossfs: unable to access MOUNTPOINT /tmp/odat: No such file or directory”?
    -   This error occurs if the directory is not yet created. You must create the directory before mounting.
-   Q: Why does the “operation not permitted” error occur after I mount the bucket locally and run the ls command for the directory?
    -   In your bucket, check if the directory name contains any OSS objects with invisible characters. The file system has strict restrictions for file/directory names. If the directory name fails to meet the restrictions, this error occurs. Use another tool to rename these objects and run the ls command, the directory content can be correctly displayed.
-   Q: There are a lot of files in one of my directories. Why is ls so slow?
    -   Assuming that there are n files in a directory, then the ls of this directory requires at least a minimum of n oss http requests. When there are many files, this can cause serious performance problems.
    -   You can optimize in two ways:
        -   Increase stat cache size with the -omax\_stat\_cache\_size=xxx parameter, so that the first time ls will be slow, but the subsequent ls will be fast, because the metadata of the file is in the local cache. The default is 1000, which costs about 4 MB of memory, please adjust to the appropriate value according to the size of your machine's memory.
        -   Use the ls -f command, which eliminates n HTTP requests with OSS.
        -   See [issue 13](https://github.com/aliyun/ossfs/issues/13) for details.
-   Q: How do I set permissions during ossfs mounting?
    -   If you want to allow other users to access mounted folders, specify the allow\_other parameter as follows when running ossfs:
        -   ```
ossfs your_bucket your_mount_point -ourl=your_endpoint -o
                  allow_other
```

    -   Why does the allow\_other parameter still have no access to the file?
        -   Note: allow\_other is the permission granted to other users in the Mount directory, not the file inside! If you want to change the files in the folder, use the chmod command.
    -   allow\_other gives the Mount directory 777 permission by default, and I want to have the Mount directory permission 770, what should I do?
        -   You can set by umask, see [here](https://github.com/aliyun/ossfs/issues/5).
-   Q: If you want to allow the mounting of folders \(/tmp/ossfs\) that belong to another user, 

    -   Method 1: If you want  to allow the mounting of folders \(/tmp/ossfs\) that belong to another user, you need to create the mount folder as user and use ossfs:
        -   `sudo -u user mkdir /tmp/ossfs`
        -   `sudo -u user ossfs bucket-name /tmp/ossfs`
    -   Method 2: first get the uid/gid information for the specified user by the id command. For example, to get uid/gid information for a www user: id www; then specify the uid/gid parameter when you mount:

        -   ```
ossfs your_bucket your_mountpoint -ourl=your_url -ouid=your_uid
                    -ogid=your_gid
```

        Note: uid/gid are numbers.

-   Q: I am not the root user, how does umount ossfs mount the directory?
    -   fusermount -u your\_mountpoint
-   Q: How can I mount ossfs automatically when the device starts up?
    -   Step 1: Write the bucket name, access key id/secret, and other information into /etc/passwd-ossfs, and change the permissions for this file to 640.
        -   ```
echo your_bucket_name:your_access_key_id:your_access_key_secret >
                    /etc/passwd-ossfs
```

        -   `chmod 640 /etc/passwd-ossfs`
-   Step 2: Make the appropriate settings \(the setting methods differ for different system versions\).
    -   Step 2A: Use the fstab method to automatically mount the ossfs \(applies to Ubuntu 14.04 and CentOS 6.5\).
        -   Add the following command in /etc/fstab:
        -   ```
ossfs#your_bucket_name your_mount_point fuse _netdev,url=your_url,allow_other 0
                    0
```

        -   In the preceding command, replace ‘your\_xxx’ with your actual bucket name and other information.
        -   Save the /etc/fstab file. Run the `mount -a` command. If no error is reported, the settings are correct.
        -   Now, Ubuntu 14.04 can automatically mount the ossfs. For CentOS 6.5, also run the following command:
        -   `chkconfig netfs on`
    -   Step 2B: Mount ossfs using a boot script \(applies to CentOS 7.0 and later\).
        -   Create the file ossfs in the /etc/init.d/ directory. Copy the content in the [Template File](https://github.com/aliyun/ossfs/blob/master/scripts/automount.template) to the new file. Here, replace ‘your\_xxx’ with your own information.
        -   Run the command: `chmod a+x /etc/init.d/ossfs`.
        -   The preceding command grants execution permission to the new ossfs script. You can now run this script. If no errors occur in the script content, the OSS bucket has been mounted to the specified directory.
        -   Run the command: `chkconfig ossfs on`.
        -   The preceding command sets the ossfs boot script as another service, so it is automatically started when the device starts up.
        -   ossfs can now automatically mount upon startup. To sum up, if you use Ubuntu 14.04 or CentOS 6.5, perform Steps 1 and 2A; if you use CentOS 7.0, perform Steps 1 and 2B.
-   Q: How do I solve the fusermount: failed to open current directory: Permission denied error?
    -   This is a fuse bug. It requires the current user to have read permission for the current directory \(unmounted directory\). To solve this problem, run the cd command to change to a directory with read permission and then run the ossfs command again.
-   Q: I need to use a www user to mount ossfs. In this case, how do I set up automatic mounting?

    -   See the answer to the preceding question. Perform Step 1 as stated. Perform Step 2B with the command in the /etc/init.d/ossfs file changed to:

        `sudo -u www ossfs your_bucket your_mountpoint -ourl=your_url`

    -   Set the boot script to allow the use of sudo to edit /etc/sudoers. Change the `Defaults requiretty` line to `#Defaults requiretty` \(comment out this line\).
-   Q: How do I solve the `fusermount: failed to open current directory: Permission denied` error?
    -   This is a [fuse bug](https://bugs.debian.org/cgi-bin/bugreport.cgi?bug=584541). It requires the current user to have read permission for the current directory \(unmounted directory\).  To solve this problem, run the cd command to change to a directory with read permission and then run the ossfs command again.

-   Q: How do I avoid the cost of scanning files by using ECS to mount ossfs?

    -   The program scans a directory mounted by ossfs to convert to a request to OSS, if the number of requests is high, costs will be incurred \(1 cent/10 thousand times \). If it is [updatedb](https://linux.die.net/man/8/updatedb), you can skip it by modifying /etc/updatedb.conf. The specific practice is:

        1.  Add `fuse.ossfs` to `PRUNEFS =`.
        2.  Add the mounted directory to the `PRUNEPATHS =`.
    -   How do I determine which process swept my catalog?
        1.  First install auditd: sudo apt-get install auditd.
        2.  Start auditd: sudo service auditd start.
        3.  Set the monitor mount directory : auditctl -w /mnt/ossfs
        4.  In the auditorium log, you can see which processes have accessed this directory: ausearch -i | grep /mnt/ossfs
-   Q: what is the content-type file that uses ossfs to upload to OSS all "application/ocdet-stream? what happened?
    -   ossfs queries /etc/mime.types content to determine the Content-Type of the file, please check that the file exists, if it does not exist, you need to add:
        1.  For ubuntu, you can add it via udo apt-get install mime-support.
        2.  For centos can be added via sudo Yum install mailcap
        3.  You can also manually add one row per format, each in the form of: Application/JavaScript JS
-   Q: How do I start ossfs using the supervisor?

1.  To install the supervisor, run the sudo apt-Get install supervisor in Ubuntu
2.  Create a directory and edit the ossfs STARTUP script:

    ```
    mkdir /root/ossfs_scripts
    vi /root/ossfs_scripts/start_ossfs.sh
    ```

    Write the following data:

    ```
    # Unload
    fusermount -u /mnt/ossfs
    # Re-mounted, you must add-F parameter to run ossfs, let ossfs run at the front desk
    exec ossfs my-bucket my-mount-point -ourl=my-oss-endpoint -f
    ```

3.  Edit/etc/Supervisor/supervisord. conf to add the following paragraph at the end:

    ```
    [program:ossfs]
    command=bash /root/ossfs_scripts/start_ossfs.sh
    logfile=/var/log/ossfs.log
    log_stdout=true
    log_stderr=true
    logfile_maxbytes=1MB
    logfile_backups=10
    ```

4.  Run Supervisor:

    ```
    supervisord
    ```

    supervisord

5.  Confirm that everything is fine:

    ```
    ps aux | grep supervisor  # should be able to see the supervisor Process
    ps aux | grep ossfs #  should be able to see ossfs Process
    kill -9 ossfs  # Kill ossfs process, the supervisor should restart it, do not use killall, because killall sends sigterm, the process Exits normally, and the Supervisor no longer reruns ossfs.
    ps aux | grep ossfs # should be able to see ossfs Process
    ```

    If an error occurs, check /var/log/supervisor/supervisord.log and /var/log/ossfs.log.


-   Q: encounter "fuse: Warning: Library too old, some operations may not work?

    This occurs because of the libfuse version that ossfs uses at compile time Higher than the libfuse version linked to at run time. This is often due to the user's own installation of libfuse. Install ossfs with the RPM package we provide, without having to install libfuse again.

    The RPM bag that we provide on the box and the box contains the box, if there is a chain in the running environment and ossfs is linked to an older version of fuse, the preceding warning will appear.


1.  How do I confirm the fuse version of The ossfs runtime link?
    -   Run LDD $ \(which ossfs\) | grep Fuse
    -   For example, the result is "/lib64/libfuse. So. 2 ", then you can see the version of fuse through LS-L/lib64/libfuse.
2.  How do I link ossfs to the correct version?
    -   First find the directory of libfuse with rpm-QL ossfs | grep fuse.
    -   For example, the result is "/usr/lib/libfuse. So. 2 ", via fig =/usr/lib ossfs... Run ossfs
3.  Can I ignore this warning?
    -   You better not see this bug.

-   Q: Why do I see file information with ossfs \(for example, size\) not consistent with what other tools see?

Because ossfs, by default, caches the file's meta-information \(including size/permissions, etc \), this does not require every time ls requests are sent to OSS to speed up. If the user passes other programs \(such as SDK/website console/osscmd, etc\) the file has been modified so that it is possible to see the file information in ossfs, not updated in a timely manner.

If you want to disable ossfs caching, you can add the following paramete `-omax_stat_cache_size=0`

