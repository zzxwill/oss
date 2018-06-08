# UploadPart {#reference_pnq_2px_wdb .reference}

After initiating a Multipart Upload event, you can upload data in parts based on the specified object name and Upload ID. Each uploaded part has a part number ranging from 1 to 10,000.

For the same Upload ID, this part number identifies not only this part of data but also the location of this part in the entire file. If you upload new data using the same part number, OSS overwrites the existing data identified by this part number. Except the last part, the minimum size of other parts is 100 KB. The size of the last part is not restricted.

## Request syntax {#section_efc_3px_wdb .section}

```
PUT /ObjectName? partNumber=PartNumber&uploadId=UploadId HTTP/1.1
Host: BucketName.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Content-Length: Size
Authorization: SignatureValue
```

## Detail analysis {#section_osz_jpx_wdb .section}

-   Before calling the Initiate Multipart Upload interface to upload a part of data, you must call this interface to obtain an Upload ID issued by the OSS server.
-   In the Multipart Upload mode, except the last part, all other parts must be larger than 100 KB. However, the Upload Part interface does not immediately verify the size of the uploaded part \(because it does not know whether the part is the last one\). It verifies the size of the uploaded part only when Multipart Upload is completed.
-   OSS puts the MD5 value of the part data received by the server in the ETag header and return it to the user.
-   The part number ranges from 1 to 10,000. If the part number exceeds this range, OSS returns the InvalidArgument error code.
-   If the x-oss-server-side-encryption request header is specified when the Initiate Multipart Upload interface is called, OSS encrypts the uploaded part and return the x-oss-server-side-encryption header in the Upload Part response header. The value of x-oss-server-side-encryption indicates the server-side encryption algorithm used for this part. For more information, see the Initiate Multipart Upload interface.
-   To make sure that the data transmitted over the network is free from errors, the user includes Content-MD5 in the request. The OSS calculates the MD5 value for the uploaded data and compares it with the MD5 value uploaded by the user. If they are inconsistent, OSS returns the InvalidDigest error code.

## Examples {#section_xbd_4px_wdb .section}

**Request example:**

```
PUT /multipart.data? partNumber=1&uploadId=0004B9895DBBB6EC98E36 HTTP/1.1
Host: oss-example.oss-cn-hangzhou.aliyuncs.com
Content-Length：6291456
Date: Wed, 22 Feb 2012 08:32:21 GMT
Authorization: OSS qn6qrrqxo2oawuk53otfjbyc:J/lICfXEvPmmSW86bBAfMmUmWjI=
[6291456 bytes data]
```

**Response example:**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Connection: keep-alive
ETag: 7265F4D211B56873A381D321F586E4A9
x-oss-request-id: 3e6aba62-1eae-d246-6118-8ff42cd0c21a
Date: Wed, 22 Feb 2012 08:32:21 GMT
```

