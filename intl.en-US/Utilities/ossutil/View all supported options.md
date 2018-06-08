# View all supported options {#concept_j45_dg4_vdb .concept}

You can use the -h option to view all options supported by ossutil.

```
$./ossutil -h
Usage of ossutil:
Options:
  -s --short-format Used to display the short format. If this option is not specified, the long format is displayed by default.
       --snapshot-path= Used to accelerate incremental uploading of files in batches in some scenarios. (File downloading and copying do not support this option currently.)  This option is used when ossutil uses the cp command to upload files. Ossutil takes a snapshot of file uploads and stores it in the specified directory. It reads the snapshot from the specified directory for incremental upload when this option is used the next time. The specified snapshot directory must be a writable directory in the local file system. If the directory does not exist, ossutil creates a file to record the snapshot information. If the directory already exists, ossutil reads the snapshot information in the directory, performs incremental uploading accordingly, and updates the snapshot information. (Ossutil only uploads files that fail to be uploaded last time and have been locally modified.) Note: By using this option, the local lastModifiedTime of files that have been successfully uploaded is recorded and compared with that of files to be uploaded next time to determine whether to skip uploading of same files. When using this option, make sure that the corresponding objects on the OSS are not modified during the two uploading periods. In other scenarios than this one, use the --update option to incrementally upload files in batches. In addition, ossutil does not actively delete the snapshot information under snapshot-path. To avoid too much snapshot information, clear snapshot-path when confirming that the snapshot information is useless.
  -j --jobs= Specifies the number of concurrent tasks when multiple files are operated. The value ranges from 1 to 10000, and the default value is 5.
  -v --version Used to display ossutil version (1.0.0.Beta2) and exit.
       --output-dir= Specifies the directory in which output files are located. The output files include the report file generated when an error occurs for copying files in batches using the cp command. (For more information about the report file, see the cp command help.)  The default value is the ossutil_output sub-directory in the current directory.
       --parallel= Specifies the number of concurrent tasks operated in a single file. The value ranges from 1 to 10000. The default value is determined by ossutil based on the operation type and file size.
  -L --language= Specifies the language of ossutil. The value can be CH or EN, and the default value is CH.
  -t --sts-token= Specifies the STSToken used to access the OSS (the option value overwrites the corresponding settings in the configuration file). It is optional.
  -m --multipart Indicates that the operation objects are the incomplete Multipart events in the bucket, rather than the default objects.
  -b --bucket Used to operate a bucket, confirming that an operation is for the bucket.
       --delete Used to delete an operation.
  -e --endpoint= Specifies the basic endpoint configuration of ossutil (the option value overwrites the corresponding settings in the configuration file). It must be a second-level domain.
  -k --access-key-secret= Specifies the AccessKeySecret used to access the OSS (the option value overwrites the corresponding settings in the configuration file).
       --bigfile-threshold= Specifies the threshold for enabling the resumable data transfer for large files. The value ranges from 0 B to 9223372036854775807 B, and the default value is 100 MB.
       --retry-times= Specifies the number of retries when an error occurs. The value ranges from 1 to 500, and the default value is 3.
  -a --all-type Indicates that the operation objects are the objects and incomplete Multipart events in the bucket.
  -r --recursive Indicates a recursive operation. If this option is specified, commands supporting this option operate on all objects meeting the criteria in the bucket. Otherwise, the commands operate only on a single object specified in the URL.
  -f --force Indicates a forcible operation without asking.
  -u --update Indicates an update operation.
  -c --config-file= Specifies the configuration file path of ossutil. Ossutil reads configuration from the configuration file during startup and writes configuration to the file using the config command.
  -i --access-key-id= Specifies the AccessKeyID used to access the OSS (the option value overwrites the corresponding settings in the configuration file).
       --acl= Used to configure the ACL.
  -d --directory Used to return files and sub-directories in the current directory, rather than recursively displaying all objects in all sub-directories.
       --checkpoint-dir= Specifies the checkpoint directory path (the default value is .ossutil_checkpoint). If a resumable data transfer fails, ossutil automatically creates this directory and records the checkpoint information in the directory. If a resumable data transfer succeeds, ossutil deletes this directory. If this option is specified, make sure that the specified directory can be deleted.
       --type= Specifies the calculation type. The value can be crc64 or md5, and the default value is crc64.
  -h --help Show usage message
```

All commands of ossutil supports part of the preceding options. Use the ossutil help command to check options supported by each command.

