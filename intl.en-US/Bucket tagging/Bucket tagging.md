# Bucket tagging {#concept_268787 .concept}

By using the bucket tagging function, you can classify your buckets to manage them. For example, you can specify that only buckets with specified tags are returned in ListBucket operations.

A tag is a key-value pair used to identify a bucket. By adding tags for buckets, you can classify and manage the buckets by their purposes.

-   Only the bucket owner and authorized RAM users can add tags for a bucket. Otherwise, the 403 Forbidden error is returned with the error code: AccessDenied.
-   You can add a maximum of 20 tags \(key-value pairs\) for a bucket.
-   The maximum size of a tag key is 64 bytes. The key of a tag cannot be null and cannot be prefixed with `http ://`, `https://`, or `Aliyun`.
-   The maximum size of a tag value is 128 bytes. The value of a tag can be null.
-   The key and value of a tag must be UTF-8 encoded.
-   When you add tags for a bucket that already has tags added, the original tags are overwritten.

## Configuration {#section_8x1_otq_tt2 .section}

OSS provides complete SDK demos for bucket tagging configurations. For the SDK demos, see:

-   [Java SDK](https://www.alibabacloud.com/help/doc-detail/119254.html)
-   [Python SDK](https://www.alibabacloud.com/help/doc-detail/119256.html)
-   [Go SDK](https://www.alibabacloud.com/help/doc-detail/119259.html)

