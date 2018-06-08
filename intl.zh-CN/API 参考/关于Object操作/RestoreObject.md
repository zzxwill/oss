# RestoreObject {#reference_mfr_5bx_wdb .reference}

RestoreObject接口用于服务端执行解冻任务。

只针对归档类型的Object读取，需要调用RestoreObject接口让服务端执行解冻任务。如果一个Object是标准或者低频访问类型，不要调用该接口。

归档类型Object在执行Restore前后的状态变换过程如下：

1.  一个归档类型的Object初始时处于冷冻状态。
2.  提交一次Restore操作后，Object将处于解冻中的状态，服务端执行解冻。
3.  待服务端执行完成解冻任务后，Object进入解冻状态，此时用户可以读取Object。
4.  解冻状态默认持续1天，24小时内再次调用RestoreObject接口则解冻状态会自动延长24小时，最多可延长7天，之后，Object又回到初始时的冷冻状态。

状态变换过程中产生的相关费用如下：

-   对一个处于冷冻状态的Object执行Restore操作，会产生数据取回费用。
-   解冻状态最多延长7天。在此期间内不再重复收取数据取回费用。
-   解冻状态结束后，Object又回到冷冻状态，再次解冻的首次读取数据会收取数据取回费用。

## 请求语法 {#section_ivv_mcx_wdb .section}

```
POST /ObjectName?restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_zy4_4cx_wdb .section}

-   如果是针对该Object第一次调用RestoreObject接口，则返回202。
-   如果已经成功调用过RestoreObject接口，且服务端仍处于解冻中，再次调用时返回409, 错误码为：RestoreAlreadyInProgress。服务端返回该错误，代表服务端正在执行restore操作，用户只需要等待作业完成，最长等待时间4小时。
-   如果已经成功调用过RestoreObject接口，且服务端解冻已经完成，再次调用时返回200，且会将object的可下载时间延长一天，最多延长7天。
-   如果object不存在，则返回404。
-   如果针对非归档类型的Object提交restore，则返回400，错误码为：OperationNotSupported。

## 示例 {#section_qj3_scx_wdb .section}

**首次提交restore的请求示例：**

```
POST /oss.jpg?restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:28 GMT
Authorization: OSS e1Unnbm1rgdnpI:y4eyu+4yje5ioRCr5PB=
```

**返回示例**

```
HTTP/1.1 202 Accepted
Date: Sat, 15 Apr 2017 07:45:28 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

**再次调用，且restore没有完成时：**

```
POST /oss.jpg?restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdy4eyu+NIUs=
```

**返回示例**

```
HTTP/1.1 409 Conflict
Date: Sat, 15 Apr 2017 07:45:29 GMT
Content-Length: 556
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
<?xml version="1.0" encoding="UTF-8"?>
<Error>
  <Code>RestoreAlreadyInProgress</Code>
  <Message>The restore operation is in progress.</Message>
  <RequestId>58EAF141461FB42C2B000008</RequestId>
  <HostId>10.101.200.203</HostId>
</Error>
```

**再次调用，且restore已经完成时：**

```
POST /oss.jpg?restore HTTP/1.1
Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
Date: Sat, 15 Apr 2017 07:45:29 GMT
Authorization: OSS e1Unnbm1rgdnpI:u6O6FMJnn+WuBwbByZxm1+y4eyu+NIUs=
```

**返回示例**

```
HTTP/1.1 200 Ok
Date: Sat, 15 Apr 2017 07:45:30 GMT
Content-Length: 0
Connection: keep-alive
Server: AliyunOSS
x-oss-request-id: 5374A2880232A65C23002D74
```

