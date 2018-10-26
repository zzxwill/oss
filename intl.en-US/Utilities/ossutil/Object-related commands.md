# Object-related commands {#concept_lqt_vjs_vdb .concept}

Ossutil allows you to upload/download/copy a file, set the ACL and meta of an object, and view the meta information of an object.

Run the config command to configure the AccessKey pair before running these commands.

-   Upload/Download/Copy a file

    You are strongly advised to use ossutil help cp to view the help information before running the cp command.

    You can run the cp command to upload/download/copy a file, and use the -r option to copy a folder. Ossutil implements multipart upload by default for large files and supports the resumable data transfer \(the threshold of large files for which multipart upload is enabled can be set using the `--bigfile-threshold` option.\)

    Use the -f option to forcibly upload a file by default. If a file exists with the same name on the target end, the file is overwritten directly.

    If an error occurs to a file during file uploading/downloading/copying in batches, ossutil logs the error information in the report file by default, skips this file, and performs operations on other files. \(Ossutil does not continue to copy other files if the bucket does not exist, or permission verification is invalid due to incorrect accessKeyID/accessKeySecret.\). For more information, see ossutil  help cp.

    Ossutil supports the incremental uploading policies `--update` and `--snapshot-path` in specific scenarios. For more information, see ossutil help cp.

    From ossutil 1.0.0. Beta1, crc64 is enabled by default during file uploading.

    \(1\) Upload a single file:

    ```
    $./ossutil cp a oss://ossutil-test
    Succeed: Total num: 1, size: 230. OK num: 1(upload 1 files).
    0.699795(s) elapsed
    ```

    \(2\) Upload a folder:

    ```
    $./ossutil cp -r dir oss://ossutil-test
    Succeed: Total num: 35, size: 464,606. OK num: 35(upload 34 files, 1 directories).
    0.896320(s) elapsed
    ```

    -   Performance tuning for uploading/downloading/copying a file

        In the cp command, the `--jobs` and `--parallel`options are used to control the number of concurrent operations. The `--jobs` option controls the number of concurrent operations enabled between files when multiple files are uploaded/downloaded/copied. The`--parallel` option controls the number of concurrent operations enabled for a large file when the large file is uploaded/downloaded/copied in multiparts.

        Ossutil calculates the number of parallel operations based on the file size by default \(this option does not work for small files, and the threshold for large files to be uploaded/downloaded/copied in multiparts can be controlled by the --bigfile-threshold option\). When large files are uploaded/downloaded/copied in batches, the actual number of concurrent operations is calculated by multiplying the number of jobs by the number of parallel operations. If the default number of concurrent operations set by ossutil cannot meet your performance requirements, you can adjust these two options to improve or reduce the performance.

        **Note:** 

        -   We recommend that you adjust the concurrent operations to a value smaller than100. If the network bandwidth, memory, and CPU are not fully occupied, you can set the concurrent operations to a bigger value.
        -   If the number of concurrent operations is too large, the uploading/downloading/copying performance of ossutil may be reduced, or even an EOF error may occur due to inter-thread resource switching and snatching. Therefore, adjust the values of `--jobs` and `--parallel` based on the actual machine conditions.
        To perform pressure testing, set the two options to small values first, and slowly adjust them to the optimal values. If the values of the --jobs and --parallel options are too large, an EOF error may occur due to the slow network transfer speed if machine resources are limited. In this case, appropriately reduce the values of the --jobs and --parallel options.

-   Configure the ACL of an object

    Ossutil uses the set-acl command to configure the ACL of an object. You can use the -r option to configure the ACLs of objects in batches.

    For more information, see ossutil help set-acl.

    ```
    $./ossutil set-acl oss://dest/a private
    0.074507(s) elapsed
    ```

    Configure the ACLs of objects in batches:

    ```
    $./ossutil set-acl oss://dest/a private -r
    Do you really mean to recursivlly set acl on objects of oss://dest/a(y or N)? y
    Succeed: Total 3 objects. Setted acl on 3 objects.
    0.963934(s) elapsed
    ```

-   Configure the meta of an object

    Ossutil uses the set-meta command to configure the meta information of an object. You can use the -r option to configure the metas of objects in batches.

    For more information, see ossutil help set-meta.

    ```
    ./ossutil set-meta oss://dest/a x-oss-object-acl:private -u
    ```

-   View the object description \(meta\)

    Ossutil uses the stat command to view the object description \(meta\).

    For more information, see ossutil help stat.

    ```
    $./ossutil stat oss://dest/a 
    ACL                         : default
    Accept-Ranges               : bytes
    Content-Length              : 230
    Content-Md5                 : +5vbQC/MSQK0xXSiyKBZog==
    Content-Type                : application/octet-stream
    Etag                        : FB9BDB402FCC4902B4C574A2C8A059A2
    Last-Modified               : 2017-01-13 15:14:22 +0800 CST
    Owner                       : aliyun
    X-Oss-Hash-Crc64ecma        : 12488808046134286088
    X-Oss-Object-Type           : Normal
    0.125417(s) elapsed
    ```

-   Restore an object from the frozen state to the readable state

    Ossutil uses the restore command to restore an object from the frozen state to the readable state. You can use the `-r` option to restore objects from the frozen state to the readable state in batches.

    For more information, see ossutil help restore.

    ```
    $./ossutil restore oss://utiltest/a
    0.037729(s) elapsed
    ```

-   Create a symbolic link

    Ossutil uses the create-symlink command to create a symbolic link.

    For more information, see ossutil help create-symlink.

    ```
    $./ossutil create-symlink oss://utiltest/b a
    0.037729(s) elapsed
    ```

-   Read the description of a symbolic link file

    Ossutil uses the read-symlink command to read the description of a symbolic link file.

    For more information, see ossutil help read-symlink.

    ```
    $./ossutil read-symlink oss://utiltest/b
    Etag                    : D7257B62AA6A26D66686391037B7D61A
    Last-Modified           : 2017-04-26 15:34:27 +0800 CST
    X-Oss-Symlink-Target    : a
    0.112494(s) elapsed
    ```


