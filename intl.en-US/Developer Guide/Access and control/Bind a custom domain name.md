# Bind a custom domain name {#concept_rz2_xg5_tdb .concept}

You can bind a custom domain name to your OSS bucket in the OSS console and then add a CNAME record in the Domain Name System \(DNS\). In accordance with the requirements of China's Internet administration regulations, all users who need to be able to set up this function, must provide MIIT record number, domain name holder ID card etc. valid data, approved via Ali cloud before it can be used. After the CNAME resolution, OSS automatically processes the access requests to the custom domain name.

## Use case {#section_wwj_1gv_tdb .section}

Suppose that John has a website with the domain name \`abc.com\` in Hangzhou. The website contains a page with the link `http://img.abc.com/logo.png`. John wants the requests for \`http://img.abc.com/logo.png\` to be serviced from the OSS content. CNAME is particularly well suited for this scenario. The process is as follows: 

1.  John creates a bucket named abc-img in China East 1 \(Hangzhou\), and uploads the image logo.png.
2.  John binds the domain name \`img.abc.com\` to the bucket abc-img in the OSS console.
3.  OSS maps the domain name \`img.abc.com\` to the bucket abc-img.
4.  John adds a CNAME record in the Domain Name System \(DNS\) to map the domain name \`img.abc.com\` to the OSS domain name of the bucket abc-img, that is, \`abc-img.oss-cn-hangzhou.aliyuncs.com\`.
5.  When the request for `http://img.abc.com/logo.png` is sent, OSS resolves the mapping between img.abc.com and abc-img, and directs the request to \`http://abc-img.oss-cn-hangzhou.aliyuncs.com/logo.png\`. That is to say, the access to `http://img.abc.com/logo.png` is redirected to `http://abc-img.oss-cn-hangzhou.aliyuncs.com/logo.png`. The comparison between the process before CNAME resolution and that after CNAME resolution is as follows:

| |Before CNAME resolution|After CNAME resolution|
|:-|:----------------------|:---------------------|
|Comparison| 1.  A request to access `http://img.abc.com/logo.png` is received.
2.  DNS resolves the request to the user's server IP address.
3.  Access to logo.png on the user's server is achieved.

 | 1.  A request to access `http://img.abc.com/logo.png` is received.
2.  DNS resolves the request to \`abc-img.oss-cn-hangzhou.aliyuncs.com\`.
3.  Access to logo.png in the OSS bucket abc-img is achieved.

 |

## Reference {#section_bxj_1gv_tdb .section}

For more information about how to bind a custom domain name in the OSS console, see [Manage a domain name](../intl.en-US/Console User Guide/Manage buckets/Manage a domain.md#).

