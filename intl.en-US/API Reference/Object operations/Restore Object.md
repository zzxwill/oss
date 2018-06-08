# Restore Object {#reference_mfr_5bx_wdb .reference}

RestoreObject interface is used to have the server restore the object.

To read an object of the Archive type, perform the Restore operation to have the server restore the object. If the type of an object is Standard or IA, do not call the Restore interface.

The status change process of the archived object before and after the Restore operation is shown as follows:

1.  An object of the Archive type is archived at first.
2.  After a Restore request is initiated, the server starts to restore the object.
3.  After the server completes restoration, the object is restored and you can read the object.
4.  The restoration status lasts for one day by default and can be prolonged to seven days at most. After the status is expired, the object is archived again.

Performing the Restore operation on an archived object incurs data retrieval costs.

-   Initiating another Restore request for
-   an archived object that is being restored or already restored
-   does not incur any further data retrieval costs.

## Request syntax {#section_ivv_mcx_wdb .section}

```
POST /ObjectName? restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## Detail analysis {#section_zy4_4cx_wdb .section}

-   If the Restore interface is called for the object for the first time, the system returns 202.
-   If the restore interface has been called and the restoration is already in progress, when the interface is called for the second time, the system returns 409 with the error code “RestoreAlreadyInProgress”, which means that the server is performing the Restore operation and you only have to wait for completion for at most four hours.
-   If the Restore interface has been called and the restoration is completed, when the interface is called again, the system returns 200 and the time available for download is prolonged for one day \(seven days at most\).
-   If the object does not exist, the system returns 404.
-   If the Restore request is initiated for an object not of the Archive type, the system returns Error 400 with the error code “OperationNotSupported”.

## Example {#section_qj3_scx_wdb .section}

**Example of the Restore request initiated for the first time**

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:28 GMT
Authorization: OSS e1Unnbm1rgdnpI:y4eyu+4yje5ioRCr5PB=
```

**Return example**

```
HTTP/1.1 202 Accepted
Date: Sat, 15 Apr 2017 07:45:28 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

**Example of a request called again when restoration is not completed**

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdy4eyu+NIUs=
```

**Return example**

```
HTTP/1.1 409 Conflict
Date: Sat, 15 Apr 2017 07:45:29 GMT
Content-Length: 556
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
<? xml version="1.0" encoding="UTF-8"? >
<Error>
  <Code>RestoreAlreadyInProgress</Code>
  <Message>The restore operation is in progress.</Message>
  <RequestId>58EAF141461FB42C2B000008</RequestId>
  <HostId>10.101.200.203</HostId>
</Error>
```

**Example of a request called again when restoration is completed**

```
POST /oss.jpg? restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:u6O6FMJnn+WuBwbByZxm1+y4eyu+NIUs=
```

**Return example**

```
HTTP/1.1 200 Ok
Date: Sat, 15 Apr 2017 07:45:30 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

