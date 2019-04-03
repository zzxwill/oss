# Sign a URL {#concept_32049_zh .concept}

SDK supports the signing of the URL with a validity period or a public resource URL so that the URL can be transferred to a third party for authorized access.

## Sign a private resource URL with a validity period {#section_ax2_dcj_lfb .section}

For a bucket or an object not assigned the public-read permission, the following interface can be called to obtain a signed URL to access the bucket or object:

```language-java
String url = oss.presignConstrainedObjectURL("<bucketName>", "<objectKey>", 30 * 60);

```

## Sign a public resource URL { .section}

For a bucket or an object with the public-read permission, the following interface can be called to obtain a public URL to access the bucket or object:

```language-java
String url = oss.presignPublicObjectURL("<bucketName>", "<objectKey>");

```

