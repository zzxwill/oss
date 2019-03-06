# Exception handling {#concept_32083_zh .concept}

If a request error occurs during SDK usage, an exception is thrown

containing the following information:

-   status: the HTTP status code of the error request
-   code: OSS error code
-   message: OSS error message
-   requestId: the UUID that uniquely identifies the request. Seek help from OSS development engineers by providing this RequestId if necessary.

For more information about common error messages in OSS, see [OSS Error Response](../../../../../reseller.en-US/Errors and Troubleshooting/OSS error response.md#).

## Debugging {#section_bhh_3yk_lfb .section}

In Node.js, enable debugging by setting the `DEBUG` environment variable:

```language-bash
DEBUG=ali-oss node app.js

```

In a browser environment, set the `localStorage.debug` variable in the console to enable debugging:

```language-js
localStorage.debug = 'ali-oss'

```

