# Response to errors {#concept_njs_3hv_vdb .concept}

If an error occurs while accessing the Image Service, the Image Service returns an error code and error message. This enables you to locate and correct the error.

## Image Service error response format {#section_pks_phv_vdb .section}

An example of an error response message is given as follows:

```
<Error>
  <Code>BadRequest</Code>
  <Message>Input is not base64 decoding.</Message>
  <RequestId>52B155D2D8BD99A15D0005FF</RequestId>
  <HostId>userdomain</HostId>
</Error>
```

This error response message contains the following elements:

-   Code : An error code that the Image Service returns to the user.
-   Message : Detailed error information provided by the Image Service.
-   RequestId : A unique UUID used to identify an error request. When a problem cannot be solved, this ID can be sent to the Image Service engineers to help locate the cause of the error.
-   HostId : Used to identify the accessed Image Service cluster.

## Image Service error codes {#section_p5b_s3v_vdb .section}

|Error code|Description|HTTP status code|
|----------|-----------|----------------|
|InvalidArgument|Parameter error|400|
|Badrequest|Incorrect request|400|
|Missingargument|A parameter is missing|400|
|Imagetoolarge|Image size exceeds the limit|400|
|Watermarkerror|Watermark error|400|
|AccessDenied|Access is denied|403|
|Signaturedoesnotmatch| Signature does not match|403|
|Nosuchfile|Image does not exist|404|
|Nosuchstyle|Style does not exist|404|
|InternalError|Internal service error|500|
|NotImplemented|Method not implemented|501|

## Processing parameter restrictions {#section_yt2_53v_vdb .section}

Image Service has the following restrictions:

-   The source file size cannot exceed 20 MB.
-   Scaling operation: The scaled image size is limited. The product of its width and height cannot exceed 4096 x 4096, and the length of a single side cannot exceed 4096 x 4.
-   Rotation operation: The size of the image to be rotated is limited. The image width or height cannot exceed 4,096.
-   A maximum of 4 channels are allowed.

