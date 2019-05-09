# Form upload {#concept_uln_lcb_5db .concept}

By using form upload, you can use the PostObject API of OSS to upload objects with a maximum size of 5 GB.

**Note:** For more information about the PostObject API, see [PostObject](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#).

## Scenarios {#section_gdr_mcb_5db .section}

This method can be used on HTML Webpages to upload objects. A typical scenario is Web applications. For example, the following table compares the form upload process with other upload processes on a job-search website.

| |Other upload methods|Form upload|
|:-|:-------------------|:----------|
|Access process| 1.  A website user sends a request to upload a resume.
2.  The website server responds with a resume upload page.
3.  The resume is uploaded to the website server.
4.  The website server uploads the resume to OSS.

 | 1.  A website user sends a request to upload a resume.
2.  The website server responds with a resume upload page.
3.  The resume is uploaded to OSS.

 |

-   The form upload process is easier, because data is directly uploaded to OSS without being forwarded by the website server.
-   In other upload processes, objects are uploaded to the website server first. When a large number of objects are uploaded, the website server must be scaled out. In the form upload process, objects are directly uploaded from the client to OSS. When a large number of objects are uploaded, OSS bears the upload pressure and ensures the service quality.

## SDK demo {#section_nsz_hlk_lgb .section}

For more information, see [Java SDK](../../../../reseller.en-US/SDK Reference/Java/Upload objects/Form upload.md#).

## Upload limits {#section_r4t_zdb_5db .section}

-   Size: The maximum size of an object is 5 GB in this mode.
-   Naming rules:
    -   Object names must be UTF-8 encoded.
    -   Object names must be one byte to 1,023 bytes in length.
    -   Object names cannot start with a forward slash \(/\) or a backslash \(\\\).

## Process analysis {#section_bzp_22b_5db .section}

1.  Create a POST policy.

    The policy form field of a POST request is used to verify the validity of the request. For example, you can set a policy to specify the size and name of the object to be uploaded, and the URL that the client jumps to and the HTTP status code that the client receives after a successful upload. For more information, see [POST policy](../../../../reseller.en-US/API Reference/Object operations/PostObject.md#section_d5z_1ww_wdb).

    For example, in the following policy, you set the expiration time before which website users can upload data to 2115-01-27T10:56:19Z and the maximum size of the object to be uploaded to 104,857,600 bytes. \(To ensure successful testing, a long expiration period is set, which is not recommended in actual use.\)

    ```
    This example uses the Python code. The policy is a JSON-formatted string.
     policy="{\"expiration\":\"2115-01-27T10:56:19Z\",\"conditions\":[[\"content-length-range\", 0, 104857600]]}"
    ```

2.  Use Base64 to encode the policy string.
3.  Use the AccessKey Secret of OSS to add a signature to the Base64-encoded policy.
4.  Create an HTML page for upload.
5.  Open the HTML page and select the object to be uploaded.

The following example shows the complete Python sample code.

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
    #1 Create a POST policy.
    policy="{\"expiration\":\"2115-01-27T10:56:19Z\",\"conditions\":[[\"content-length-range\", 0, 1048576]]}"
    print("policy: %s" % policy)
    #2 Use Base64 to encode the policy string.
    base64policy = convert_base64(policy)
    print("base64_encode_policy: %s" % base64policy)
    #3 Use the AccessKey Secret of OSS to add a signature to the encoded policy.
    signature = get_sign_policy(access_key_secret, base64policy)
    #4 Create an HTML page for upload.
    form = '''
    <html> 
        <meta http-equiv=content-type content="text/html; charset=UTF-8"> 
        <head><title>OSS form upload (through the PostObject API)</title></head>
        <body>
            <form  action="http://%s.%s" method="post" enctype="multipart/form-data"> 
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

Save this sample code as post\_object.py and run it by using python post\_object.py.

```
Usage:
python post_object.py --bucket=Your bucket name --endpoint=Bucket endpoint --id=Your AccessKey ID --key=Your AccessKey Secret --out=Output object name
Example:
python post_object.py --bucket=oss-sample --endpoint=oss-cn-hangzhou.aliyuncs.com --id=tphpxp --key=ZQNJzf4QJRkrH4 --out=post.html
```

**Note:** 

-   In the created form, `success_action_redirect value=http://oss.aliyun.com` indicates the Webpage that appears after the upload succeeds. You can replace it with your own Webpage.
-   In the created form, `success_action_status value=201` indicates that HTTP status code 201 is returned after the upload succeeds. You can change it to another HTTP status code.
-   If the generated HTML page is post.html, open post.html and select the object to be uploaded. In this example, OSS product page \(http://oss.aliyun.com\) appears after the upload succeeds.

## Upload security and authorization {#section_arr_vbb_5db .section}

To prevent unauthorized third-party users from uploading data to your bucket, OSS provides bucket- and object-level access control. For more information, see [Access control](reseller.en-US/Developer Guide/Access and control/Overview.md#).

To authorize third-party users to upload objects, OSS also provides account authorization. For more information, see [Authorized third-party upload](reseller.en-US/Developer Guide/Upload files/Authorized third-party upload.md#).

