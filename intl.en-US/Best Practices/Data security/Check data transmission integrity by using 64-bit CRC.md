# Check data transmission integrity by using 64-bit CRC {#concept_sb4_33f_vdb .concept}

## Background {#section_smh_l3f_vdb .section}

An error may occur when data is transmitted between the client and the server. Currently, OSS can return the 64-bit CRC value for an object uploaded in any mode. To check the data integrity, the client can compare the 64-bit CRC value with the locally calculated value.

-   OSS calculates 64-bit CRC value for newly uploaded object, stores the result as metadata of the object, and then adds the x-oss-hash-crc64ecma header to the returned response header, indicating its 64-bit CRC value. This 64-bit CRC is calculated according to ECMA-182 Standard.
-   For the object that already exists on OSS before the 64-bit CRC goes live, OSS does not calculate its 64-bit CRC value. Therefore, its 64-bit CRC value is not returned when such object is obtained.

## Operation instructions {#section_m14_43f_vdb .section}

-   Put Object / Append Object / Post Object / Multipart upload part returns the corresponding 64-bit CRC value. The client can get the 64-bit CRC value returned by the server after the upload is completed and can check it against the locally calculated value.

-   In the case of Multipart Complete, if all the parts have their respective 64-bit CRC values, then the 64-bit CRC value of the entire object is returned. Otherwise, the 64-bit CRC value is not returned \(for example, if a part has been uploaded before the 64-bit CRC goes live\).

-   Get Object / Head Object / Get ObjectMeta returns the corresponding 64-bit CRC value \(if any\). After Get Object is completed, the client can get the 64-bit CRC value returned by the server and check it against the locally calculated value.

    **Note:** The 64-bit CRC value of the entire object is returned for the range get object.

-   For copy related operations, for example, Copy Object/Upload Part Copy, the newly generated object/Part may not necessarily have the 64-bit CRC value.

## Python example {#section_vtv_r3f_vdb .section}

An example of complete Python code is as follows. It shows how to check data transmission integrity based on the 64-bit CRC value.

1.  Calculate the 64-bit CRC value.

    ```
    import oss2
    from oss2.models import PartInfo
    import os
    import crcmod
    import random
    import string
    do_crc64 = crcmod.mkCrcFun(0x142F0E1EBA9EA3693L, initCrc=0L, xorOut=0xffffffffffffffffL, rev=True)
    def check_crc64(local_crc64, oss_crc64, msg="check crc64"):
    if local_crc64 ! = oss_crc64:
    print "{0} check crc64 failed. local:{1}, oss:{2}.".format(msg, local_crc64, oss_crc64)
    return False
    else:
    print "{0} check crc64 ok.".format(msg)
    return True
    def random_string(length):
    return ''.join(random.choice(string.lowercase) for i in range(length))
    bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
    ```

2.  Verify Put Object.

    ```
    content = random_string(1024)
     key = 'normal-key'
     result = bucket.put_object(key, content)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(content))
     check_crc64(local_crc64, oss_crc64, "put object")
    ```

3.  Verify Get Object.

    ```
    result = bucket.get_object(key)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(result.resp.read()))
     check_crc64(local_crc64, oss_crc64, "get object")
    ```

4.  Verify Upload Part and Complete.

    ```
    part_info_list = []
     key = "multipart-key"
     result = bucket.init_multipart_upload(key)
     upload_id = result.upload_id
     part_1 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 1, part_1)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_1))
     #Check whether the uploaded part 1 data is complete
     check_crc64(local_crc64, oss_crc64, "upload_part object 1")
     part_info_list.append(PartInfo(1, result.etag, len(part_1)))
     part_2 = random_string(1024 * 1024)
     result = bucket.upload_part(key, upload_id, 2, part_2)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2))
     #Check whether the uploaded part 2 data is complete
     check_crc64(local_crc64, oss_crc64, "upload_part object 2")
     part_info_list.append(PartInfo(2, result.etag, len(part_2)))
     result = bucket.complete_multipart_upload(key, upload_id, part_info_list)
     oss_crc64 = result.headers.get('x-oss-hash-crc64ecma', '')
     local_crc64 = str(do_crc64(part_2, do_crc64(part_1)))
     #Check whether the final object on the OSS is consistent with the local file
     check_crc64(local_crc64, oss_crc64, "complete object")
    ```


## OSS SDK support {#section_g43_1lf_vdb .section}

Part of the OSS SDK already supports the data validation using crc64 for the upload and download, as shown in the following table:

|SDK|Support for CRC?|Example|
|:--|:---------------|:------|
|Java SDK |Yes |[CRCSample.java](https://github.com/aliyun/aliyun-oss-java-sdk/blob/master/src/samples/CRCSample.java)|
|Python SDK|Yes|[object\_check.py](https://github.com/aliyun/aliyun-oss-python-sdk/blob/master/examples/object_check.py)|
|PHP SDK |No |N/A |
|C\# SDK|No|None|
|C SDK|Yes|[oss\_crc\_sample.c](https://github.com/aliyun/aliyun-oss-c-sdk/blob/master/oss_c_sdk_sample/oss_crc_sample.c)|
|JavaScript SDK|No|None|
|Go SDK |Yes|[crc\_test.go](https://github.com/aliyun/aliyun-oss-go-sdk/blob/master/oss/crc_test.go)|
|Ruby SDK|No|None|
|iOS SDK|No|[OSSCrc64Tests.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSiOSTests/OSSCrc64Tests.m)|
|Android SDK|No|[OSSCrc64Tests.m](https://github.com/aliyun/aliyun-oss-ios-sdk/blob/master/AliyunOSSiOSTests/OSSCrc64Tests.m)|

