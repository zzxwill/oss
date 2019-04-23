# FAQ {#concept_uys_ylb_wdb .concept}

-   Permissions issue:
    -   Can't list bucket can't log in

        The reason is usually the accesskeyid that is used And the accesskeysecret belong to the sub-account, and the sub-account does not have a list Bucket permissions.

        If it wasn't in the bucket The Enpoint column configures the bucket's access domain name, And when you access the bucket via ossftp, ftpserver tries to get through Service to get the region of the bucket. At this point if User Account No list The bucket permission causes the login to fail.

        The solution is in the bucket. The three-level access domain name that is configured in the endpoints, such.

    -   List file is reported wrong after login is successful

        This is typically the accesskeyid used. And accesskeysecret belong to the sub-account, and the sub-account does not have List objects \(equivalent to get Bucket\) permissions.

-   Other questions:
    -   List file timeout causes the connection to be disconnected after the login is successful

        The reason is generally that there are too many files or folders in the bucket root directory. After logging in to FTP, ftpserver tries to list all the files/folders in the bucket root directory, you can list 1000 files/folders at a time. If there are more than 1 million files/folders in the root directory, this will result in more than 1000 HTTP requests, which can easily lead to a timeout.

    -   A machine running ftpserver failed data transfer due to a port Restriction

        Because the control port of the FTP Protocol differs from the data port, when the ftpserver is working in passive mode, whenever you need to transfer data, ftpserver opens 1 random port, waiting for the client to connect. So when the ftpserver machine has a port limit, it may cause the data to fail to transfer properly.

        The workaround is when running ftpserver. py, by specifying -- Passive\_ports\_start and -- Passive\_ports\_end parameter to set the start and end ranges of the local port, and then open the ports for that range.

    -   The connection between the client and the ftpserver is often disconnected

        Each FTP client typically has a timeout setting, which can be set to not time out. Take the filezilla tool for example, in the settings-\> connection, you can set the timeout to 0.


