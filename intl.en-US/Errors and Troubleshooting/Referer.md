# Referer {#concept_kh3_c3j_wdb .concept}

## Introduction {#section_q1c_d3j_wdb .section}

`Referer` is part of an HTTP header. When your browser sends a request to a web server, the request normally carries a referer that notifies the server of the source page for the current request. The correct spelling of \`Referer\` must be the word \`Referrer\`. However, developers repeat the typo considering its massive usage.

## Referer functions {#section_bct_23j_wdb .section}

-   Anti-leech. For example, when a website accesses its own image server, the image server obtains the referer to determine if the requesting domain name falls within its own domain names. If it is true, access is permitted; if it is false, access is denied.
-   Data statistics. For example, the referer collects the statistics on source links of user accesses.

## Blank referer {#section_jvc_g3j_wdb .section}

A blank referer means that the content of the referer header in an HTTP request is empty, or the HTTP request does not contain the referer header.

The referer is blank in the following two scenarios:

-   The request is not triggered by a link. For example, open a page by directly entering an address in the address bar.
-   The referer won't be detected on an HTTP page if you access a non-encrypted HTTP page through the link on the HTTPS page.

What is the difference between allowing and disallowing blank referers in anti-leech settings?

In the whitelist of anti-leech settings, specifying that an item in the list can contain blank referers allows you to directly access this resource URL on the address bar of a browser. In other words, failing to do so disallows you to directly access it in a browser.

## OSS anti-leech {#section_sb2_h3j_wdb .section}

OSS anti-leech is implemented through `Referer`. Therefore, this function is also referred to as OSS `Refer` or `refer` for short. For more information, see [OSS anti-leech](../intl.en-US/Best Practice Guide/Bucket management/Anti-leech.md#).

-   OSS anti-leech configuration

    OSS anti-leech includes:

    -   Permit for access requests with blank referer fields
    -   Whitelist of referer fields
    OSS refer is configured by setting the \`bucket\` property on the OSS console or in the [SDK](https://www.alibabacloud.com/help/doc-detail/32021.htm).

-   OSS anti-leech precautions

    Note the following precautions for OSS refer configuration:

    -   Anti-leech verification is performed only when users access objects through URL signatures or anonymously. If the request header contains the `Authorization` field, anti-leech verification is skipped.
    -   A bucket supports multiple referer parameters which are separated by commas \(`,`\).
    -   Referer parameters support wildcard characters `*` and `?`. For more information, see the description for the following wildcard characters.
    -   Users can set `whether access requests with blank referer fields are permitted`.
    -   When the whitelist is empty, no check is performed for blank referer fields \(otherwise all requests would be rejected\).
    -   When the whitelist is not empty and is set with rules for disallowing blank referer fields, only the requests with referers defined in the whitelist are permitted while any other requests \(including the requests with blank referers\) are rejected.
    -   If the whitelist is not empty and is set with rules for allowing blank referer fields, requests with blank referers or which meet the whitelist are permitted while any other requests are rejected.
    -   All the three bucket permissions \(private, public-read, and public-read-write\) check referer fields.
    Description for wildcard characters:

    -   Asterisk \(`*`\): represents zero or multiple characters. If you are looking for a file that starts with AEW in the name but have forgotten the rest part, you can enter AEW\* to search for all types of files starting with AEW in the name, such as AEWT.txt, AEWU.EXE, and AEWI.dll. To narrow down the search scope, you can enter AEW\*.txt to search for all .txt files whose names start with AEW, such as AEWIP.txt and AEWDF.txt.
    -   Question mark \(`?`\): represents a single character. By entering love?, you can search for all types of files whose names are ‘love’ followed by a single ending character, such as lovey and lovei. To narrow down the search scope, you can enter love?.doc to search for all .doc files whose names are ‘love’ followed by a single ending character, such as lovey.doc and lovei.doc.
-   Typical configuration
    -   Permit accesses by all requests

        -   Blank referer: allow blank referers
        -   Referer list: empty
    -   Only permit accesses by requests with specified referers

        -   Blank referer: disallow blank referers
        -   Refer list: `http://*.oss-cn-beijing.aliyuncs.com`, `http://*.aliyun.com`
    -   Permit accesses by requests with specified referers and without referers

        -   Blank referer: allow blank referers
        -   Refer list: `http://*.oss-cn-beijing.aliyuncs.com`, `http://*.aliyun.com`

## Common errors and troubleshooting {#section_fxn_n3j_wdb .section}

When a referer is misconfigured, the HTTP status code \(\`http code\`\) is 403 and OSS returns the following error:

```
<Code>AccessDenied</Code>
<Message>You are denied by bucket referer policy.</Message>
```

**Note:** 

-   Normally, referer error reporting applies to site applications, and you can view the referer of a header in browsers. For example, in Google Chrome, press `F12` to open `Developer Tools` and view the header of an element in `Network`.
-   Errors returned by OSS can be obtained by capturing packets. For example, in Wireshark, you can specify the filter as `host bucket-name.oss-cn-beijing.aliyuncs.com` .

Possible causes:

-   The referer is blank and the request header contains no or blank referer fields.
-   The referer is out of the specified referer range.  Note the following items:
    -   Determine whether the refer configuration is prefixed with `http://` or `https://` during configuration.
    -    a.aliyun.com and b.aliyun.com match with `http://*.aliyun.com` or `http://?.aliyun.com`.
    -   domain.com matches with `http://domain.com` instead of `http://*.domain.com`.
-   Check that the refer configuration is always prefixed with `http://` or `https://`; otherwise, the referer format is incorrect and the refer configuration is invalid. For example, b.aliyun.com is an invalid configuration.

**Note:** 

-   Configure referers in **OSS Console \>** \> **Bucket \>** \> **Bucket Properties** \> ** \>** Anti-Leech.
-   Clear browser cache for debugging.
-   OSS refer supports only the whitelist but not the blacklist temporarily.

For troubleshooting other errors, see [OSS 403 Errors and Troubleshooting](intl.en-US/Errors and Troubleshooting/OSS 403.md#)

## Other problems {#section_y3c_cjj_wdb .section}

Why can videos still be captured with curl when anti-leech is active?

Check if CDN is enabled, refer settings for CDN are nonempty and the anti-leech list for CDN is consistent with that for OSS. For anti-leech settings for CDN, see Anti-Leech in the CDN User Guide. When debugging OSS referers, first eliminate the impact caused by CDN. Adjust OSS referers and then CDN referers.

For more information about referers and their configuration, see [Anti-Leech](../intl.en-US/Best Practice Guide/Bucket management/Anti-leech.md#).

