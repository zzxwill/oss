# Exception handling {#concept_64055_zh .concept}

If a request error occurs during SDK usage, an exception is thrown

containing the following information:

-   status: the HTTP status code of the error request
-   code: OSS error code
-   message: OSS error message
-   requestId: the UUID that uniquely identifies the request. Seek help from OSS development engineers by providing this RequestId if necessary.

For more information about common error messages in OSS, see [OSS Error Response](../../../../../reseller.en-US/Errors and Troubleshooting/OSS error response.md#).

## Debugging {#section_j5s_f2m_lfb .section}

In a browser environment, set the `localStorage.debug` variable in the console to enable debugging:

```language-js
localStorage.debug = 'ali-oss'

```

