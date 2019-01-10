# RestoreObject {#reference_mfr_5bx_wdb .reference}

RestoreObject接口用于服务端执行解冻任务。

RestoreObject操作只针对归档类型（Archive）的Object，不适用于标准类型（Standard）和低频访问类型（IA）的Object。

归档类型的Object在执行Restore前后的状态变换过程如下：

1.  一个归档类型的Object初始时处于冷冻状态。
2.  提交一次Restore请求后，Object将处于解冻中的状态，服务端执行解冻。
3.  服务端完成解冻任务后，Object进入解冻状态，此时用户可以读取Object。
4.  解冻状态默认持续24小时，24小时内再次调用RestoreObject接口则解冻状态会自动延长24小时，最多可延长7天。解冻状态结束后，Object回到初始时的冷冻状态。

状态变换过程中产生的相关费用如下：

-   对一个处于冷冻状态的Object执行Restore操作，会产生数据取回费用。
-   解冻状态最多延长7天。在此期间不再重复收取数据取回费用。
-   解冻状态结束后，Object又回到冷冻状态，再次执行Restore操作会收取数据取回费用。

## 请求语法 {#section_ivv_mcx_wdb .section}

```
POST /ObjectName?restore HTTP/1.1
Host: archive-bucket.oss-cn-hangzhou.aliyuncs.com
Date: GMT Date
Authorization: SignatureValue
```

## 细节分析 {#section_zy4_4cx_wdb .section}

-   如果Object不存在，则返回404。
-   如果针对非归档类型的Object发送Restore请求，则返回400错误。错误码：OperationNotSupported。
-   如果针对该Object第一次调用RestoreObject接口，则返回202。
-   如果已经成功调用过RestoreObject接口，但Object未完成解冻，再次调用时返回409错误，错误码为：RestoreAlreadyInProgress，代表服务端正在执行Restore操作，用户只需要等待作业完成，最长等待时间为4小时。
-   如果已经成功调用过RestoreObject接口，且Object已完成解冻，再次调用时返回200 OK，且会将Object的可下载时间延长1天，最多延长7天。

## 示例 {#section_qj3_scx_wdb .section}

-   首次提交Restore请求，Object处于冷冻状态。

    请求示例：

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Authorization: OSS e1Unnbm1rgdnpI:y4eyu+4yje5ioRCr5PB=
    ```

    返回示例：

    ```
    HTTP/1.1 202 Accepted
    Date: Sat, 15 Apr 2017 07:45:28 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```

-   提交Restore请求，Object处于解冻中的状态。

    请求示例：

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rgdnpI:21qtGJ+ykDVmdy4eyu+NIUs=
    ```

    返回示例：

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

-   提交Restore请求，Object处于解冻状态。

    请求示例：

    ```
    POST /oss.jpg?restore HTTP/1.1
    Host: oss-archive-example.oss-cn-hangzhou.aliyuncs.com
    Date: Sat, 15 Apr 2017 07:45:29 GMT
    Authorization: OSS e1Unnbm1rgdnpI:u6O6FMJnn+WuBwbByZxm1+y4eyu+NIUs=
    ```

    返回示例：

    ```
    HTTP/1.1 200 Ok
    Date: Sat, 15 Apr 2017 07:45:30 GMT
    Content-Length: 0
    Connection: keep-alive
    Server: AliyunOSS
    x-oss-request-id: 5374A2880232A65C23002D74
    ```


