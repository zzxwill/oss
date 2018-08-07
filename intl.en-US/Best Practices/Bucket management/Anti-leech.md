# Anti-leech {#concept_n5g_qd2_vdb .concept}

## Background {#section_zkn_rd2_vdb .section}

For example, A is the webmaster of a website. Webpages on the website contain links to images and audio/video files. These static resources are stored on [Alibaba Cloud OSS](https://www.alibabacloud.com/product/oss). For example, A may save an image file on OSS with the URL `http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png`. 

For OSS external resource url, see [OSS address](../../../../intl.en-US/Developer Guide/Access and control/ACL verification.md#) such a URL \(without signing\) requires the user's bucket permission to read publicly.

B is the webmaster of another website, B use the image resources of the website without permission, use this method to steal space and traffic by placing it in a web page on your website. In this case, the third-party web site user sees the B web site, but it's not clear the source of the pictures on the website. Since OSS charges by usage, so that user A does not get any benefit, instead, the cost of resource use is borne.

This article applies to users who use OSS resources as outer chains in a Web page, it also introduces a-like users who have stored their resources on OSS, how to avoid the use of unnecessary resources by setting up anti-theft chains.

## Implementation method {#section_j1t_sd2_vdb .section}

At present, the methods of anti-theft chain provided by OSS mainly include the following two types:

-   Set Referer : The operation is available through the console and the SDK, and the user can choose according to their needs.
-   Use signature URL: This is suitable for users who are used to developing.

The following two examples are provided in this article:

-   Set the Referer anti-theft chain through the console
-   Dynamic generation of signed URL anti-theft chains based on PHP SDK

Set Referer

This section focuses on what Referer is and how OSS uses Referer for anti-theft chains.

-   What is Referer?

    Referer is HTTP Part of the header that usually comes with a referer when the browser sends a request to the web server, tell the server the source of the link for this request. In the example above, if the web site for user B is userd omain-steal, want to steal a picture link `http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png`. A's website domain name is `userdomain`.

    Suppose the web page of the chain website user domain-steal is as follows:

    ```
    <html>
        <p>This is a test</p>
        <img src="http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png" />
    </html>
    ```

    Assume the web page with the source station user domain is as follows:

    ```
    <html>
        <p>This is my test link from OSS URL</p>
        <img src="http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png" />
    </html>
    ```

    -   When an Internet user uses a browser to access the Web page of B's website `http://userdomain-steal/index.html`, the link in the web page is a picture of the site A. Because a request from one domain name \(user domain-steal\) jumped to another domain name \(maid \), the browser takes the Referer with it in the header of the HTTP request, as shown:

        You can see that the browser Referer in the HTTP request is`http://userdomain-steal/index.html`. This article mainly uses Chrome's developer mode to view web page requests, as follows:

    -   The same browser visits `http://userdomain/error.html`, and you can also see that the browser's Referer is `http://userdomain/error.html`.
    -   If the browser enters the address directly, you can see that Referer is empty in the request.

        If a does not have any Referer-related settings on the OSS, all three cases have access to the picture link for user.

-   The principle of OSS through Referer anti-theft chain

    Thus, when the browser requests the OSS resource, if a page Jump occurs, the browser takes the Referer in the request, and the Referer's value is the URL on the previous page, sometimes Referer is empty.

    For both cases, the OSS Referer feature offers two options:

    -   Sets whether empty Referer access is allowed. It cannot be set separately and needs to be used in conjunction with the Referer whitelist.
    -   Sets the Referer white list.
    The details are analyzed as follows:

    -   Anti-theft chain authentication is performed only if the user is accessing the object through a signed URL or an anonymous access. If the requested header has an "Authorization" field, it does not do anti-theft chain validation.
    -   A bucket can support multiple Referer parameters.
    -   The Referer parameter supports wildcard characters '\*' and '?'.
    -   Users can set up to allow request access for empty referer.
    -   When the White List is empty, the Referer field is not checked for empty \(otherwise all requests will be rejected, because empty Referer will be rejected, for non-empty Referer OSS is also not found on the Referer White List \).
    -   The White List is not empty, and a rule is set that does not allow Referer fields to be empty. Only Referer's white list of requests is allowed, other requests, including those whose Referer is empty, are rejected.
    -   The White List is not empty, but the rule "allow Referer field to be empty" is set. An empty request with Referer and a whitelist-compliant request are permitted, other requests are rejected.
    -   Three permissions of bucket \(private, public-read, public-read-write\) the Referer field is checked.
    Wildcard character explanation:

    -   Asterisks '\*': You can use an asterisks instead of 0 or more characters. If you are looking for a file name that starts with “AEW”, you can enter AEW to search for all types of files with the names starting with “AEW”, for example, AEWT.txt, AEWU.EXE, and AEWI.dll.  If you want to narrow down the search scope, you can enter AEW.txt to search for all .txt files with names starting with AEW, such as AEWIP.txt and AEWDF.txt.
    -   Question mark \(?\): represents one character.  If you enter love?, all types of files with names starting with “love” and ending with a character are displayed, such as lovey and lovei. If you want to narrow the search scope, you can enter love?.doc to search for all .doc files with names starting with “love” and ending with a character, such as lovey.doc and lovei.doc.
-   Anti-leech effects of different Referer settings

    The following describes the effects of Referer settings:

    -   Disable Allow Empty Referer, as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4411/15336322051701_en-US.png)

        Direct access: The resources are accessible even when anti-leech protection takes effect. The reason is, if the whitelist is blank, the system does not check whether the Referer field is blank. The Referer setting does not take effect when the whitelist is blank. Therefore, the Referer whitelist must be configured.

    -   Disable Allow Empty Referer and configure a Referer whitelist.

        As shown in the preceding example, the Referer in the browser request is the URL of the current webpage. Therefore, it is necessary to know from which URL the request jumps and then specify the URL.

        Referer whitelist setting rules:

        -   In the example, the Referer is `http://userdomain/error.html`. Therefore, the Referer whitelist can be set to `http://userdomain/error.html`. As the Referer check performed by OSS is based on prefix matching, access to other webpages such as `http://userdomain/index.html` fails. To avoid this problem, you can set the Referer whitelist set to `http://userdomain/`.
        -   To allow access to other domain names such as `http://img.userdomain/index.html`, add `http://*.userdomain/` to the Referer whitelist.
        Both entries are configured as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4411/15336322051702_en-US.png)

        After testing, the following results are obtained:

        |Browser input| Expectation|Result|
        |:------------|:-----------|:-----|
        |[http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png](http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png)|Expectation for direct access with a blank Referer: Blank Referers are not allowed and OSS returns 403.|As expected|
        |[http://userdomain/error.html](http://userdomain/error.html)|Expectation for a request from the origin site: successful access.|As expected|
        |[http://userdomain-steal/index.html](http://userdomain-steal/index.html)|Expectation for a request from a leeching site: OSS returns 403. Anti-leech protection is successful.|As expected|
        |[http://img.userdomain/error.html](http://img.userdomain/error.html)|Expectation for a request from a third-level domain of the origin site: successful access.|As expected|

        **Note:** 

        -   In this test, the domain names only serve as examples, and are not the same as the actual domain names you use. Be sure to differentiate them.
        -   If the Referer whitelist only contains `http://userdomain/`, and the browser attempts to access the resources through the simulated third-level domain name `http://img.userdomain/error.html`, the third-level domain name fails to match any of the entries in the Referer whitelist, and OSS returns 403.
    -   Enable Allow Empty Referer and configure a Referer whitelist.

        The Referer whitelist contains `http://*.userdomain/` and `http://userdomain`, as shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4411/15336322051709_en-US.png)

        After testing, the following results are obtained:

        |Browser input| Expectation|Result|
        |:------------|:-----------|:-----|
        |[http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png](http://referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png)|Expectation for direct access with a blank Referer: successful access|As expected|
        |[http://userdomain/error.html](http://userdomain/error.html)|Expectation for a request from the origin site: successful access|As expected|
        |[http://userdomain-steal/index.html](http://userdomain-steal/index.html)| Expectation for a request from a leeching site: OSS returns 403. Anti-leech protection is successful.|As expected|
        |[http://img.userdomain/error.html](http://img.userdomain/error.html)|Expectation for a request from a third-level domain of the origin site: successful access|As expected|

-   How to configure Referer on OSS

    Functional use reference:

    -   API: [Put Bucket Referer](../../../../intl.en-US/API Reference/Bucket operations/Put Bucket Referer.md#)
    -   Console: [Anti-leech settings](../../../../intl.en-US/Console User Guide/Manage buckets/Set anti-leech.md#)
-   Pros and cons of Referer anti-leech protection

    Referer anti-leech protection can be easily configured on the console. The main drawback of the Referer anti-leech protection is that it cannot prevent access attempts by the malicious spoofing Referers. If a leecher uses an application to simulate HTTP requests with a spoofing Referer, the Referer can bypass anti-leech protection settings. If you have higher anti-leech protection requirements, consider using signed URL anti-leech protection.


Signed URLs

For the principles and implementation methods for signed URLs, see [Authorizing third-Party download](../../../../intl.en-US/Developer Guide/Download files/Authorized third-party download.md#). A signed URL is implemented as follows:

1.  Set the bucket permission to private-read.
2.  Generate a signature based on the expected expiration time \(the time when the signed URL expires\).

Specific implementation

1.  Install the latest PHP code by referring to the [PHP SDK documentation](https://www.alibabacloud.com/help/doc-detail/32099.htm).
2.  Generate a signed URL and add it to the webpage as an external link, for example:

    ```
    <? php
     require 'vendor/autoload.php';
     #Indicates the automatic loading function provided by the latest PHP.
     use OSS\OssClient;
     #Indicates the namespace used.
     $accessKeyId="a5etodit71tlznjt3pdx7lch";
     #Indicates the AccessKeyId, which must be replaced by the one you use.
     $accessKeySecret="secret_key";
     #Indicates the AccessKeySecret, which must be replaced by the one you use.
     $endpoint="oss-cn-hangzhou.aliyuncs.com";
     #Indicates the Endpoint, selected based on the region created by the bucket. In the example, the endpoint is Hangzhou.
     $bucket = 'referer-test';
     #Indicates the bucket, which must be replaced by the one you use.
     $ossClient = new OssClient($accessKeyId, $accessKeySecret, $endpoint);
     $object = "aliyun-logo.png";
     #Indicates the object to be signed.
     $timeout = 300;
     #Indicates the expected link expiration time. The value indicates that the link is valid for 300 seconds from when this line of code starts running.
     $signedUrl = $ossClient->signUrl($bucket, $object, $timeout); #Indicates the function used to implement the signed URL.
     $img= $signedUrl;
     #Indicates dynamically placing the signed URL in image resources and printing it out.
     $my_html = "<html>";
     $my_html .= "<img src=\"".$img. "\" />";
     $my_html .= "<p>".$img."</p>";
     $my_html .= "</html>";
     echo $my_html;
     ? >
    ```

3.  If the browser requests the resource multiple times, different signed URLs may be displayed. This is a normal phenomenon because the signed URL changes once it expires. After expiration time the link is no longer valid. It is displayed in Unix time format,  for example, Expires=1448991693. The time can be converted to the local time. In Linux, the command for converting the time is `date -d@1448991693`. You can also find a conversion tool on the Internet.

Special instructions

Signed URLs can be used with the Referer whitelist function.

If the expiration time of signed URLs is limited to minutes, even when a leecher spoofs a Referer, the leecher needs to obtain the signed URL and complete leeching before the signed URL expires. Compared with the Referer method, this makes leeching more difficult. Using signed URLs with the Referer whitelist function provides enhanced anti-leech protection results.

## Conclusion {#section_bgc_3l2_vdb .section}

Best practices of OSS-based anti-leech protection:

-   Use third-level domain name URLs, such as `referer-test.oss-cn-hangzhou.aliyuncs.com/aliyun-logo.png`, as they are more secure than bound second-level domain names. The third-level domain name access method provides bucket-level cleaning and isolation, enabling you to respond to a burst in leeching traffic while preventing different buckets from affecting each other, thereby increasing service availability.
-   If you use custom domain names as links, bind the CNAME to a third-level domain name, with the rule bucket + endpoint. For example, your bucket is named “test” and the third-level domain name is `test.oss-cn-hangzhou.aliyuncs.com`.
-   Set the strictest possible permission for the bucket. For example, set a bucket that provides Internet services to public-read or private. Do not set it to public-read-write. For bucket permission information, see [Access control](../../../../intl.en-US/Developer Guide/Access and control/Access control.md#).
-   Verify access sources and set a Referer whitelist based on your requirement.
-   If you need a more rigorous anti-leeching solution, consider using signed URLs.
-   Record access logs of the bucket, so that you can promptly discover leeching and verify the effectiveness of your anti-leeching solution. For access log information, see [Access logging configuration](../../../../intl.en-US/Developer Guide/Access and control/Access control.md#).

## FAQ {#section_htv_jl2_vdb .section}

-   I have configured anti-leech protection on the OSS Console, but the configuration does not take effect. Access to webpages is blocked, whereas access to players is not. Why? How can this problem be fixed?

    Currently, anti-leech protection fails to take effect for audio and video files. When a media player, such as Windows Media Player or Flash Player, is used to request OSS resources, a blank Referer request is sent. This causes anti-leech protection ineffective. To resolve this issue, you can see the preceding signed URL anti-leech protection method.

-   What is a Referer?  How is it sent? How to deal with HTTPS websites? Does anything else need to be added, like commas?

    A Referer is a request header in the HTTP protocol.  It is attached to a request that involves a page jump. You must check whether the Referer in the request sent by your browser is `http://` or `https://`. In normal cases, the Referer is `http://`.

-   How are signed URLs generated?  Is storing the AccessKeySecret on the client secure?

    See the individual SDK documentation for the method of signing the URL. It is not recommended that the AccessKeySecret be directly stored on the client. RAM provides the [STS service](../../../../intl.en-US/Developer Guide/Access and control/Access control.md#section_mjv_skv_tdb) to solve this problem. Also, see [RAM and STS Guide](intl.en-US/Best Practices/Access control/Overview.md#).

-   How do I use wildcard characters \(\*, ?\) to write `a.baidu.com` and `b.baidu.com` ?

    You can use `http://*.baidu.com`. If the wildcard character represents a single character only, you can also use `http://?.baidu.com`.

-   \*.domain.com can match a second-level domain name, but does not match domain.com. Only adding a second entry of domain.com does not work either. What settings must be configured?

    Note that a Referer generally includes a parameter such as http. You can view the request Referer in Chrome’s developer mode and then specify the corresponding Referer. As in this case, you may have forgotten to include `http://`, which is required to be `http://domain.com`.

-   What must I do if anti-leech protection does not take effect?

    We recommend that you use Chrome to solve the problem. Open developer mode and click on the Web page to view the `Referer` specific values in the HTTP request. Check whether the `Referer` value matches the Referer value configured on OSS. If they do not match, set the Referer value configured on OSS to the Referer value in the HTTP request. If the problem persists, open a ticket.


