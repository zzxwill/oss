# Exception handling {#concept_32066_zh .concept}

There are two types of exceptions in the iOS SDK: ClientError and ServerError.

ClientError indicates parameter and network errors. ServerError indicates abnormal responses from the OSS server.

|Error type|Error domain|Code|UserInfo|
|----------|:-----------|:---|:-------|
|ClientError|com.aliyun.oss.clientError|0|OSSClientErrorCodeNetworkingFailWithResponseCode0, indicating that the connection is abnormal.|
|1|OSSClientErrorCodeSignFailed, indicating that the signature failure occurs.|
|2|OSSClientErrorCodeFileCantWrite, indicating that the object fails to be written to the server.|
|3|OSSClientErrorCodeInvalidArgument, indicating that the parameter is invalid.|
|4|OSSClientErrorCodeNilUploadid, indicating that the upload ID for the resumable upload task fails to be obtained.|
|5|OSSClientErrorCodeTaskCancelled, indicating that the task is cancelled.|
|6|OSSClientErrorCodeNetworkError, indicating that the network is abnormal.|
|7|OSSClientErrorCodeCannotResumeUpload, indicating that the resumable upload task fails.|
|8|OSSClientErrorCodeNotKnown, indicating that an unknown error occurs.|
|ServerError|com.aliyun.oss.serverError|\(-1 \* httpResponseCode\)|The directory obtained by parsing the corresponding XML file.|

