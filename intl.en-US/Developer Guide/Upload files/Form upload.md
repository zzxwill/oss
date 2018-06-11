# Form upload {#concept_uln_lcb_5db .concept}

## Use cases {#section_gdr_mcb_5db .section}

Form upload refers to the upload of an object by using the Post Object method in the OSS API. The object to be uploaded cannot be larger than 5 GB. This method can be used in HTML web pages to upload objects. A typical scenario is web applications. Take a job-search website as an example. The comparison between the process with and without using form upload is as follows:

| |Process without using form upload|Process using form upload|
|:-|:--------------------------------|:------------------------|
|Process comparison| 1.  A website user uploads a resume.
2.  The website server responds to the upload page.
3.  The resume is uploaded to the server.
4.  The server uploads the resume to OSS.

 | 1.  A website user uploads a resume.
2.  The website server responds to the upload page.
3.  The resume is uploaded to OSS.

 |

## Upload restrictions {#section_r4t_zdb_5db .section}

-   The maximum size of a single object is 5 GB.
-   The naming conventions of objects are as follows:
    -   Object names must use UTF-8 encoding.
    -   Object names must be at least 1 byte and no more than 1,023 bytes in length.
    -   Object names cannot start with a backslash \( \\ \) or a forward slash \( / \).

## Advantages of form upload {#section_g4q_c2b_5db .section}

-   If the form upload is not used, files are uploaded to the web server first, and then the web server forwards the files to OSS.
-   In case of huge uploads, the web server becomes the bottleneck and needs to be scaled up. If the form upload is used, files are uploaded directly from the client to OSS without the forwarding of the web server. OSS handles all upload requests and guarantees the service quality.

## Security and authorization {#section_bxs_d2b_5db .section}

-   To grant upload permissions to a third party, you can use the PostObject interface. For more information, see [PostObject](../intl.en-US/API Reference/Object operations/PostObject.md#).


## Procedures for form upload {#section_bzp_22b_5db .section}

1.  Construct a Post policy.

    The policy form field of the Post request is used to verify the validity of the request. For example, the policy can specify the size and name of objects to be uploaded, the redirect URL of the client, and the status code the client receives after a successful upload.  For more information, see [Post Policy](../intl.en-US/API Reference/Object operations/PostObject.md#section_d5z_1ww_wdb).

    In the following example of policy, the expiration time for uploads by website users is 2115-01-27T10:56:19Z \(a long expiration period is set for tests only and is not recommended in actual use\) and the maximum file size is 104857600 bytes.

    ```
    This example uses the Python code and the policy is a string in JSON format.
     policy="{\"expiration\":\"2115-01-27T10:56:19Z\",\"conditions\":[[\"content-length-range\", 0, 104857600]]}"
    ```

2.  Encode the policy string using Base64.
3.  Use the OSS AccessKeySecret to sign the Base64-encoded policy.
4.  Construct an HTML page for uploads.
5.  Open the HTML page and select and upload files.

A complete Python code example is as follows:

```
#coding=utf8
import md5
import hashlib
import base64
import hmac
from optparse import OptionParser
def convert_base64(input):
    return base64.b64encode(input)
def get_sign_policy(key, policy):
    return base64.b64encode(hmac.new(key, policy, hashlib.sha1).digest())
def get_form(bucket, endpoint, access_key_id, access_key_secret, out):
    #1. Construct a Post policy
    policy="{\"expiration\":\"2115-01-27T10:56:19Z\",\"conditions\":[[\"content-length-range\", 0, 1048576]]}"
    print("policy: %s" % policy)
    #2. Encode the policy string using Base64
    base64policy = convert_base64(policy)
    print("base64_encode_policy: %s" % base64policy)
    #3. Use the OSS AccessKeySecret to sign the Base64-encoded policy
    signature = get_sign_policy(access_key_secret, base64policy)
    #4. Construct an HTML page for uploads
    form = '''
    <html>
        <meta http-equiv=content-type content="text/html; charset=UTF-8">
        <head><title>OSS form upload (PostObject)</title></head>
        <body>
            <form action="http://%s.%s" method="post" enctype="multipart/form-data">
                <input type="text" name="OSSAccessKeyId" value="%s">
                <input type="text" name="policy" value="%s">
                <input type="text" name="Signature" value="%s">
                <input type="text" name="key" value="upload/${filename}">
                <input type="text" name="success_action_redirect" value="http://oss.aliyun.com">
                <input type="text" name="success_action_status" value="201">
                <input name="file" type="file" id="file">
                <input name="submit" value="Upload" type="submit">
            </form>
        </body>
    </html>
    ''' % (bucket, endpoint, access_key_id, base64policy, signature)
    f = open(out, "wb")
    f.write(form)
    f.close()
    print("form is saved into %s" % out)
if __name__ == '__main__':
    parser = OptionParser()
    parser.add_option("", "--bucket", dest="bucket", help="specify ")
    parser.add_option("", "--endpoint", dest="endpoint", help="specify")
    parser.add_option("", "--id", dest="id", help="access_key_id")
    parser.add_option("", "--key", dest="key", help="access_key_secret")
    parser.add_option("", "--out", dest="out", help="out put form")
    (opts, args) = parser.parse_args()
    if opts.bucket and opts.endpoint and opts.id and opts.key and opts.out:
        get_form(opts.bucket, opts.endpoint, opts.id, opts.key, opts.out)
    else:
        print "python %s --bucket=your-bucket --endpoint=oss-cn-hangzhou.aliyuncs.com --id=your-access-key-id --key=your-access-key-secret --out=out-put-form-name" % __file__
```

Save this code example as post\_object.py and run it by using python post\_object.py.

```
Usage:
python post_object.py --bucket=Your bucket --endpoint=The bucket's OSS domain name --id=Your AccessKeyId --key=Your AccessKeySecret --out=Output file name
Example:
python post_object.py --bucket=oss-sample --endpoint=oss-cn-hangzhou.aliyuncs.com --id=tphpxp --key=ZQNJzf4QJRkrH4 --out=post.html
```

**Note:** 

-   In the constructed form, `success_action_redirect value=http://oss.aliyun.com` indicates the redirect URL after a successful upload.  You can replace it with your own page.
-   `success_action_status value=201` indicates that Status Code 201 is returned after a successful upload. This value can be replaced.
-   If the specified HTML file is post.html, open post.html and select the file to be uploaded. In this example, the client redirects to the OSS homepage \`http://oss.aliyun.com\` after a successful upload.

## Usage {#section_txy_hfb_5db .section}

-   API: [PostObject](../intl.en-US/API Reference/Object operations/PostObject.md#)

## Best practices {#section_wgy_3fb_5db .section}

-   [Web client direct upload](../intl.en-US/Best Practices/Direct upload to OSS from Web/Overview of direct transfer on Web client.md#)
-   [Cross-origin Resource Sharing \(CORS\)](../intl.en-US/Best Practices/Bucket management/Cross-origin resource sharing (CORS).md#)

## Related links {#section_j4d_kfb_5db .section}

-   [Upload callback](intl.en-US/Developer Guide/Upload files/Upload callback.md#)
-   [OSS-based app development](intl.en-US/Developer Guide/Access OSS/OSS-based app development.md#)
-   [Simple download](intl.en-US/Developer Guide/Download Files/Simple download.md#)
-   [Image processing](intl.en-US/Developer Guide/Image Processing.md#)
-   [Cloud data processing](intl.en-US/Developer Guide/Cloud data processing.md#)
-   [Access Control](intl.en-US/Developer Guide/Access and control/Access control.md#)
-   [Authorized third-party upload](intl.en-US/Developer Guide/Upload files/Authorized third-party upload.md#)
-   [Object Meta](intl.en-US/Developer Guide/Managing Objects/Object Meta.md#)

