# OSS performance and scalability best practice {#concept_xtt_pln_vdb .concept}

## Partitions and naming conventions {#section_wdc_5ln_vdb .section}

OSS automatically partitions user data by file names encoded in UTF-8 to process massive data and meet the needs for high request rates. However, if you use sequential prefixes \(such as timestamps and sequential numbers\) as part of the names when uploading a large number of objects, there may be lots of file indexes stored in a single partition. In this way, when the request rates exceed 2,000 operations per second \(downloading, uploading, deleting, copying, and obtaining metadata are each counted as one operation, while deleting or enumerating more than one files in batch is considered as multiple operations\), the following results may occur:

-   This partition becomes a hotspot partition, leading to the exhausted I/O capacity and low request rate limited automatically by the system.
-   With a hotspot partition, the partitioned data is constantly rebalanced, which may increase the processing time.

Therefore, the horizontal scaling capability of OSS is affected, thus resulting in limited request rate.

To address these issues, you must delete the sequential prefixes in the file names. Instead, you can add random prefix in file names. In this way, the file indexes \(and I/O loads\) are evenly distributed in different partitions.

The following shows the examples of changing sequential prefixes into random prefixes.

-   Example 1: Add hex hash prefixes into file names

    As shown in this example, you may use a combination of dates and customer IDs \(including sequential timestamp prefixes\) in file names:

    ```
    sample-bucket-01/2017-11-11/customer-1/file1
    sample-bucket-01/2017-11-11/customer-2/file2
    sample-bucket-01/2017-11-11/customer-3/file3
    
    sample-bucket-01/2017-11-12/customer-2/file4
    sample-bucket-01/2017-11-12/customer-5/file5
    sample-bucket-01/2017-11-12/customer-7/file6
    
    ```

    In this case, you can calculate a hash value for the customer ID, that is, the MD5 \(customer-id\), and combine a hash prefix of several characters as the prefix to the file name. If you use a 4-character hash prefix, the file names are as follows:

    ```
    sample-bucket-01/2c99/2017-11-11/customer-1/file1
    sample-bucket-01/7a01/2017-11-11/customer-2/file2
    sample-bucket-01/1dbd/2017-11-11/customer-3/file3
    
    sample-bucket-01/7a01/2017-11-12/customer-2/file4
    sample-bucket-01/b1fc/2017-11-12/customer-5/file5
    sample-bucket-01/2bb7/2017-11-12/customer-7/file6
    
    ```

    In this case, a 4-character hex hash value is used as the prefix, and each character can be any one of the 16 values \(0-f\), so there are 16^4=65,536 possible character combinations. Technically, the data in the storage system is constantly partitioned into up to 65,536 partitions. Leveraging the performance bottleneck limit \(2,000 operations per second\) and the request rate of your service, you can determine a proper number of hash buckets.

    If you want to list all the files with a specific date in the file name, for example, files with 2017-11-11 in the name in sample-bucket-01, you must enumerate the files in sample-bucket-01 \(acquire all files in sample-bucket-01 in batch by multiple calls of the List Object API\) and combine files with this date in the file names.

-   Example 2: Reverse the file name

    In this example, you may use a UNIX timestamp with millisecond precision to generate file names, which is also a sequential prefix:

    ```
    sample-bucket-02/1513160001245.log
    sample-bucket-02/1513160001722.log
    sample-bucket-02/1513160001836.log
    sample-bucket-02/1513160001956.log
    
    sample-bucket-02/1513160002153.log
    sample-bucket-02/1513160002556.log
    sample-bucket-02/1513160002859.log
    
    ```

    As mentioned in the preceding paragraph, if you use the sequential prefix in file names, the performance may be affected when the request rate exceeds a certain limit. To address this issue, you can reverse the timestamp prefix to exclude the sequential prefix. The result is as follows:

    ```
    sample-bucket-02/5421000613151.log
    sample-bucket-02/2271000613151.log
    sample-bucket-02/6381000613151.log
    sample-bucket-02/6591000613151.log
    
    sample-bucket-02/3512000613151.log
    sample-bucket-02/6552000613151.log
    sample-bucket-02/9582000613151.log
    
    ```

    The first three digits of the file name represent the millisecond, which can be any one of the 1,000 values. The forth digit changes every second. Similarly, the fifth digit changes every 10 seconds… In this way, the prefixes are randomly specified and the loads are distributed evenly to multiple partitions, thus avoiding the performance bottleneck.


