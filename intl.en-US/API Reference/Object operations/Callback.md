# Callback {#reference_b3p_cyw_wdb .reference}

To perform a callback, you only need to attach the relevant callback parameters to the request sent to OSS.

APIs that currently support callbacks are PutObject, PostObject, and CompleteMultipartUpload.

## Construct the callback parameter {#section_jb1_y3x_wdb .section}

The callback parameter is composed of a JSON string encoded in Base64. It is critical that you specify the request callback server URL \(callbackUrl\) and callback content \(callbackBody\). Detailed JSON fields are as follows:

|Field|Meaning |Required?|
|:----|:-------|:--------|
|callbackUrl| -   After a file is uploaded successfully, OSS sends a callback request to this URL. The request method is POST and the body is the content specified for callbackBody. Under normal circumstances, if this URL must respond to “HTTP/1.1 200 OK”,  the response body must be in the JSON format and the response header Content-Length must be a valid value and not exceeding 3 MB.
-   This function allows users to set up to 5 URLs, separated by “;”. OSS sends requests one by one until the first successful response is returned.
-   If no URL is configured or the value is null, it is regarded that callback is not configured.
-   HTTPS addresses are supported.
-   To make sure that Chinese characters are correctly processed, the callbackUrl must be encoded. For example, `http://example.com/中文.php?key=value&http://example.com/Chinese.php?key=value&Chinese Name=Chinese Value` needs to be encoded into  `http://example.com/%E4%B8%AD%E6%96%87.php?key=value&%E4%B8%AD%E6%96%87%E5%90%8D%E7%A7%B0=%E4%B8%AD%E6%96%87%E5%80%BC.`

 |Yes|
|callbackHost| -   The host header value for initiating callback requests. It is valid only when the callbackUrl is set. 
-   If no callbackHost is set, the URL in callbackUrl is resolved and the host generated after resolving is entered in callbackHost.

 |No|
|callbackBody| -   The value of the request body when a callback is initiated, for example, key=$\(key\)&etag=$\(etag\)&my\_var=$\(x:my\_var\). 
-   It supports OSS system variables, custom variables, and constants. The supported system variables are described in the following table. Custom variables are supported by transmission through callback-var in PutObject and CompleteMultipart. In Post Object operations, each variable is transmitted through a form field.

 |Yes|
|callbackBodyType| -   The Content-Type of the callback requests initiated. It supports application/x-www-form-urlencoded and application/json, and the former is the default value.
-   If the Content-Type is set to application/x-www-form-urlencoded, the variables in callbackBody are replaced by URL encoded values. If the Content-Type is set to application/json, these variables are replaced according to the JSON format.

 |No|

JSON string examples are as follows:

```

"callbackUrl":"121.101.166.30/test.php",
"callbackHost":"oss-cn-hangzhou.aliyuncs.com",
"callbackBody":"{\"mimeType\":${mimeType},\"size\":${size}}",
"callbackBodyType":"application/json"

```

```

"callbackUrl":"121.43.113.8:23456/index.html",
"callbackBody":"bucket=${bucket}&object=${object}&etag=${etag}&size=${size}&mimeType=${mimeType}&imageInfo.height=${imageInfo.height}&imageInfo.width=${imageInfo.width}&imageInfo.format=${imageInfo.format}&my_var=${x:my_var}"

```

Here, the system variables that can be set for callbackBody include the following. In specific, the imageInfo is for the image format. It must be left empty for a non-image format:

|System variable|Meaning|
|:--------------|:------|
|bucket|bucket|
|object|object|
|etag|The file’s etag, that is, the etag field returned to the user.|
|size|The object size. During the CompleteMultipartUpload operation, this is the size of the whole object.|
|mimeType|The resource type. For jpeg images, the resource type is image/jpeg|
|imageInfo.height|The image height|
|imageInfo.width| The image width|
|imageInfo.format| The image format, such as jpg and png|

## Custom parameters {#section_fnw_rfx_wdb .section}

You can use the callback-var parameter to configure custom parameters.

Custom parameters are a map of key-values. You can configure the required parameters to the map. When initiating a POST callback request, OSS puts these parameters and the system parameters described in the preceding section in the body of the POST request, so that these parameters can be easily obtained by the callback recipient.

You can construct custom parameters in the same way as constructing the callback parameter. The custom parameters can also be transmitted in the JSON format. The JSON string is a map containing key-values of all custom parameters.

**Note:** It must be particularly noted that, the keys of the custom parameters must start with x: and be in the lower case. Otherwise, OSS returns an error.

Assume that you must set two custom parameters x:var1 and x:var2, and the values of the two parameters are value1 and value2 respectively, the JSON format constructed is as follows:

```

"x:var1":"value1",
"x:var2":"value2"

```

## Construct callback requests {#section_a45_yfx_wdb .section}

After the callback and callback-var parameters are constructed, you can transmit the parameters to OSS with three methods. The callback parameter is required, and the callback-var parameter is optional. If you configure no custom parameter, the callback-var field does not need to be added. The aforesaid three methods are as follows:

-   Including parameters in the URL.
-   Including parameters in the header.
-   Using form fields to include parameters in the body of a POST request.

    **Note:** You can only use this method to specify the callback parameter when using POST to upload an object.


The three methods are alternative; otherwise, OSS returns an InvalidArgument error.

To include a parameter in OSS request, first you must use Base64 to encode the preceding constructed JSON string, and include the string in OSS request using the methods described as follows:

-   To include parameters in the URL, use `callback=[CallBack]` or `callback-var=[CallBackVar]` as a URL parameter to send it with the request. When CanonicalizedResource of the signature is calculated, callback or callback-var is taken into consideration as a sub-resource.
-   To include parameters in the header, use `x-oss-callback=[CallBack]` or `x-oss-callback-var=[CallBackVar]` as a head to send it with the request. When CanonicalizedOSSHeaders of the signature is calculated, x-oss-callback-var and x-oss-callback are taken into consideration. An example is provided as follows:

    ```
    PUT /test.txt HTTP/1.1
    Host: callback-test.oss-test.aliyun-inc.com
    Accept-ncoding: identity
    Content-Length: 5
    x-oss-callback-var: eyJ4Om15X3ZhciI6ImZvci1jYWxsYmFjay10ZXN0In0=
    User-Agent: aliyun-sdk-python/0.4.0 (Linux/2.6.32-220.23.2.ali1089.el5.x86_64/x86_64;2.5.4)
    x-oss-callback: eyJjYWxsYmFja1VybCI6IjEyMS40My4xMTMuODoyMzQ1Ni9pbmRleC5odG1sIiwgICJjYWxsYmFja0JvZHkiOiJidWNrZXQ9JHtidWNrZXR9Jm9iamVjdD0ke29iamVjdH0mZXRhZz0ke2V0YWd9JnNpemU9JHtzaXplfSZtaW1lVHlwZT0ke21pbWVUeXBlfSZpbWFnZUluZm8uaGVpZ2h0PSR7aW1hZ2VJbmZvLmhlaWdodH0maW1hZ2VJbmZvLndpZHRoPSR7aW1hZ2VJbmZvLndpZHRofSZpbWFnZUluZm8uZm9ybWF0PSR7aW1hZ2VJbmZvLmZvcm1hdH0mbXlfdmFyPSR7eDpteV92YXJ9In0=
    Host: callback-test.oss-test.aliyun-inc.com
    Expect: 100-Continue
    Date: Mon, 14 Sep 2015 12:37:27 GMT
    Content-Type: text/plain
    Authorization: OSS mlepou3zr4u7b14:5a74vhd4UXpmyuudV14Kaen5cY4=
    Test
    ```

-   It is slightly complicated to include the callback parameter when POST is used to upload an object, because the callback parameter must be included using an independent form field. See the following example:

    ```
    --9431149156168
    Content-Disposition: form-data; name="callback"
    eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkmdGFibGU9JHt4OnRhYmxlfSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==
    ```

    If custom parameters are used, you cannot directly include the callback-var parameter in the form field. Each custom parameter must be included using an independent form field. For example, if the JSON of a custom parameter is:

    ```
    
    "x:var1":"value1",
    "x:var2":"value2"
    
    ```

    The form field of the POST request are as follows:

    ```
    --9431149156168
    Content-Disposition: form-data; name="callback"
    eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkmdGFibGU9JHt4OnRhYmxlfSIsImNhbGxiYWNrQm9keVR5cGUiOiJhcHBsaWNhdGlvbi94LXd3dy1mb3JtLXVybGVuY29kZWQifQ==
    --9431149156168
    Content-Disposition: form-data; name="x:var1"
    value1
    --9431149156168
    Content-Disposition: form-data; name="x:var2"
    value2
    ```

    At the same time, you can add callback conditions in the policy \(if callback is not added, upload verification is not performed on this parameter\). For example:

    ```
    { "expiration": "2014-12-01T12:00:00.000Z",
      "conditions": [
        {"bucket": "johnsmith" },
        {"callback": "eyJjYWxsYmFja1VybCI6IjEwLjEwMS4xNjYuMzA6ODA4My9jYWxsYmFjay5waHAiLCJjYWxsYmFja0hvc3QiOiIxMC4xMDEuMTY2LjMwIiwiY2FsbGJhY2tCb2R5IjoiZmlsZW5hbWU9JChmaWxlbmFtZSkiLCJjYWxsYmFja0JvZHlUeXBlIjoiYXBwbGljYXRpb24veC13d3ctZm9ybS11cmxlbmNvZGVkIn0="},
        ["starts-with", "$key", "user/eric/"],
      
    
    ```


## Initiate callback requests {#section_fgy_2hx_wdb .section}

If the file is uploaded successfully, OSS uses the POST method to send the specific content to the application server based on the callback parameter and the custom parameters \(the callback-var parameter\) in the user’s request.

```
POST /index.html HTTP/1.0
Host: 121.43.113.8
Connection: close
Content-Length: 0
Content-Type: application/x-www-form-urlencoded
User-Agent: ehttp-client/0.0.1
bucket=callback-test&object=test.txt&etag=D8E8FCA2DC0F896FD7CB4CB0031BA249&size=5&mimeType=text%2Fplain&imageInfo.height=&imageInfo.width=&imageInfo.format=&x:var1=for-callback-test
```

## Return callback results {#section_ald_hhx_wdb .section}

For example, the application server returns the following request for response:

```
HTTP/1.0 200 OK
Server: BaseHTTP/0.3 Python/2.7.6
Date: Mon, 14 Sep 2015 12:37:27 GMT
Content-Type: application/json
Content-Length: 9

```

## Return upload results {#section_rhz_jhx_wdb .section}

The following content is sent to the client:

```
HTTP/1.1 200 OK
Date: Mon, 14 Sep 2015 12:37:27 GMT
Content-Type: application/json
Content-Length: 9
Connection: keep-alive
ETag: "D8E8FCA2DC0F896FD7CB4CB0031BA249"
Server: AliyunOSS
x-oss-bucket-version: 1442231779
x-oss-request-id: 55F6BF87207FB30F2640C548

```

It must be noted that, in the case of requests such as CompleteMultipartUpload, the returned request body includes content \(for example, information in XMl format\). After using the upload callback function, the original body content is overwritten, such as ‘“a”:”b”‘. Take this into consideration for judgment and processing.

## Callback signature {#section_btz_phx_wdb .section}

When the callback parameter is set, OSS sends the POST callback request to the user’s application server based on the callbackUrl set by the user. After receiving the callback request, if you expect the application server to check whether the callback request is initiated by OSS, you can include a signature in the callback request to verify the OSS identity.

-   Generate signatures

    The signature occurs at the OSS side, and is signed using the RSA Asymmetric Encryption. You can encrypt the signature using a private key as follows:

    ```
    authorization = base64_encode(rsa_sign(private_key, url_decode(path) + query_string + ‘\n’ + body, md5))
    ```

    **Note:** Instructions: The private\_key indicates a private key which is only known to OSS. The path indicates the resource path of the callback request. The query\_string indicates a query string. The body indicates the message body of the callback. The signature thus consists of the following steps:

    -   Obtain the string to be signed: The resource path URL is decoded, added by the initial query string, a carriage return and the callback message body.
    -   RSA signature: Use a private key to sign the expected string. The hashing function for signature is MD5.
    -   Use Base64 to encode the signed result to get the final signature. Put the signature in the authorization header of the callback request.
    An example is provided as follows:

    ```
    POST /index.php? id=1&index=2 HTTP/1.0
    Host: 121.43.113.8
    Connection: close
    Content-Length: 18
    authorization: kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==
    Content-Type: application/x-www-form-urlencoded
    User-Agent: ehttp-client/0.0.1
    x-oss-pub-key-url: aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==
    bucket=yonghu-test
    ```

    The path is `/index.php`, query\_string is `? id=1&index=2`，body is `bucket=yonghu-test`, and the final signature result is `kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==`

-   Verify signature

    Signature verification is an inverse process of signature. The signature is verified by the application server, and the process is as follows:

    ```
    Result = rsa_verify(public_key, md5(url_decode(path) + query_string + ‘\n’ + body), base64_decode(authorization))
    ```

    The fields have the same meanings as described during the signature process. The public\_key indicates a public key. The authorization indicates the signature in the callback header. The signature verification consists of the following steps:

    1.  The x-oss-pub-key-url header of the callback request stores the Base64-encoded URL of the public key. The header must be decoded with Base64 to obtain the public key as follows:

        ```
        public_key = urlopen(base64_decode(x-oss-pub-key-url header))
        ```

        It must be noted that, the value of the `x-oss-pub-key-url` header must start with `http://gosspublic.alicdn.com/` or `https://gosspublic.alicdn.com/`, so as to make sure that the public key is provided by OSS.

    2.  Obtain the Base64-decoded signature

        ```
        signature = base64_decode(authorization header)
        ```

    3.  Obtain the string to be signed the same way as described in the signature process.

        ```
        sign_str = url_decode(path) + query_string + ‘\n’ + body
        ```

    4.  Verify the signature

        ```
        result = rsa_verify(public_key, md5(sign_str), signature)
        ```

    The preceding sample is used as an example:

    1.  Obtain the URL of the public key, that is, with Base64 decoding the `aHR0cDovL2dvc3NwdWJsaWMuYWxpY2RuLmNvbS9jYWxsYmFja19wdWJfa2V5X3YxLnBlbQ==` to `http://gosspublic.alicdn.com/callback_pub_key_v1.pem`.
    2.  The signature header `kKQeGTRccDKyHB3H9vF+xYMSrmhMZjzzl2/kdD1ktNVgbWEfYTQG0G2SU/RaHBovRCE8OkQDjC3uG33esH2txA==` is decoded with Base64 \(The decoded result cannot be displayed because it is a nonprintable string\).
    3.  Obtain the string to be singed, that is,  url\_decode\(“index.php”\) + “?id=1&index=2” + “\\n” + “bucket=yonghu-test” Then perform the
    4.   MD5 check.
-   Application server example

    Python is used as an example to demonstrate how an application server verifies a signature. In this example, the M2Crypto library must be installed.

    ```
    import httplib
    import base64
    import md5
    import urllib2
    from BaseHTTPServer import BaseHTTPRequestHandler, HTTPServer
    from M2Crypto import RSA
    from M2Crypto import BIO
    def get_local_ip():
        try:
            csock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
            csock.connect(('8.8.8.8', 80))
            (addr, port) = csock.getsockname()
            csock.close()
            return addr
        except socket.error:
            return ""
    class MyHTTPRequestHandler(BaseHTTPRequestHandler):
        
        def log_message(self, format, *args):
            return
        
        def do_POST(self):
            #get public key
            pub_key_url = ''
            try:
                pub_key_url_base64 = self.headers['x-oss-pub-key-url']
                pub_key_url = pub_key_url_base64.decode('base64')
                if not pub_key_url.startswith("http://gosspublic.alicdn.com/") and not pub_key_url.startswith("https://gosspublic.alicdn.com/"):
                    self.send_response(400)
                    self.end_headers()
                    return
                url_reader = urllib2.urlopen(pub_key_url)
                #you can cache it
                pub_key = url_reader.read() 
            except:
                print 'pub_key_url : ' + pub_key_url
                print 'Get pub key failed!'
                self.send_response(400)
                self.end_headers()
                return
            #get authorization
            authorization_base64 = self.headers['authorization']
            authorization = authorization_base64.decode('base64')
            #get callback body
            content_length = self.headers['content-length']
            callback_body = self.rfile.read(int(content_length))
            #compose authorization string
            auth_str = ''
            pos = self.path.find('?')
            if -1 == pos:
                auth_str = urllib2.unquote(self.path) + '\n' + callback_body
            else:
                auth_str = urllib2.unquote(self.path[0:pos]) + self.path[pos:] + '\n' + callback_body
            print auth_str
            #verify authorization
            auth_md5 = md5.new(auth_str).digest()
            bio = BIO.MemoryBuffer(pub_key)
            rsa_pub = RSA.load_pub_key_bio(bio)
            try:
                result = rsa_pub.verify(auth_md5, authorization, 'md5')
            except:
                result = False
            if not result:
                print 'Authorization verify failed!'
                print 'Public key : %s' % (pub_key)
                print 'Auth string : %s' % (auth_str)
                self.send_response(400)
                self.end_headers()
                return
            #do something accoding to callback_body
            #response to OSS
            resp_body = '{"Status":"OK"}'
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.send_header('Content-Length', str(len(resp_body)))
            self.end_headers()
            self.wfile.write(resp_body)
    class MyHTTPServer(HTTPServer):
        def __init__(self, host, port):
            HTTPServer.__init__(self, (host, port), MyHTTPRequestHandler)
    if '__main__' == __name__:
        server_ip = get_local_ip()
    server_port = 23451
    server = MyHTTPServer(server_ip, server_port)
    server.serve_forever()
    ```

    Application servers implemented in other languages are as follows:

    Java version:

    -   Download address: [click here](https://gosspublic.alicdn.com/images/AppCallbackServer.zip).
    -   Running method: Extract the package and run `java -jar oss-callback-server-demo.jar 9000` \(9000 is the port number and can be designated as needed\)
    PHP version:

    -   Download address: [click here](https://gosspublic.alicdn.com/callback-php-demo.zip)
    -   Running method: Deploy the program to an Apache environment. The characteristics of the PHP language determine that the environment is depended on to retrieve some headers. You may see the example to make modifications to your own environment.
    Python version:

    -   Download address: [click here](https://gosspublic.alicdn.com/images/callback_app_server.py.zip)
    -   Running method: Extract the package and directly run `python callback_app_server.py`. You must install RSA dependencies to run this program.
    C \# version:

    -   Download address: [click here](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048926621/callback-server-dotnet.zip)
    -   Running method: Extract the package and see `README.md`.
    Go version:

    -   Download address: [click here](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048926621/callback-server-dotnet.zip)
    -   Running method: Extract the package and see `README.md`.
    Go version:

    -   Download address: [click here](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/31989/cn_zh/1501048745465/callback-server-go.zip)
    -   Running method: Extract the package and see `README.md`.
    Ruby version:

    -   Download address: [click here](https://github.com/rockuw/oss-callback-server)
    -   Running method: ruby aliyun\_oss\_callback\_server.rb

## Special instructions {#section_hq2_vkx_wdb .section}

-   If the input callback parameter or callback-var parameter is invalid, a 400 error is returned, with the error code of “InvalidArgument”. Invalid situations include the following:
    -   In the PutObject\(\) and CompleteMultipartUpload\(\) interfaces, the callback\(x-oss-callback\) or callback-var\(x-oss-callback-var\) parameters are input at the same time to the URL and header fields.
    -   The callback or callback-var parameter is too long \(over 5KB\). PostObject\(\) is not subject to this restriction because callback-var parameter is not used, and this is true for the following as well.
    -   Callback or callback-var is not Base64 encoded.
    -   After Base64 decoding, the callback or callback-var parameter is not in a valid JSON format.
    -   After callback parameter resolution, the callbackUrl field contains more than 5 URLs, or the input port in the URL is invalid,

        ```
        such as
            {"callbackUrl":"10.101.166.30:test", "callbackBody":"test"}.
        ```

    -   After callback parameter resolution, the callbackBody field is blank.
    -   After callback parameter resolution, the callbackBodyType field value is not “application/x-www-form-urlencoded” or “application/json”.
    -   After callback parameter resolution, the callbackBody field contains invalid formats of variables. The valid format is $\{var\}.
    -   After callback-var parameter resolution, the format is not the expected JSON format. The expected format is:`{"x:var1":"value1","x:var2":"value2"...}`
-   If a callback fails, the system returns a 203 error, with the error code “CallbackFailed”. A callback failure only indicates that OSS did not receive the expected callback response \(for example, the response from the application server was not in the JSON format\), not that the application server did not receive the callback request. In addition, by this time, the file has been successfully uploaded to OSS.
-   The response returned by the application server to OSS must contain the Content-Length header, and the size of the body cannot exceed 1 MB.

