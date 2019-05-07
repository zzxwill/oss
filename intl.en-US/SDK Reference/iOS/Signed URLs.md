# Signed URLs {#concept_32062_zh .concept}

The SDK supports the signed URLs with a validity period or public URLs so that the URL can be forwarded to a third party for authorized access.

## Generated a signed private resource URL with a validity period {#section_ax2_dcj_lfb .section}

If the bucket or object ACL is Private, you must call the following API to obtain the signed URL:

```
NSString * constrainURL = nil;

// Generate the signed URL.
OSSTask * task = [client presignConstrainURLWithBucketName:@"<bucket name>"
                                             withObjectKey:@"<object key>"
                                    withExpirationInterval: 30 * 60];
if (! task.error) {
    constrainURL = task.result;
} else {
    NSLog(@"error: %@", task.error);
}
```

## Generated a signed public URL { .section}

If the bucket or object ACL is Public Read, you must call the following API to obtain the URL of the publicly accessible object:

```language-objc
NSString * publicURL = nil;

// Generate the signed URL.
task = [client presignPublicURLWithBucketName:@"<bucket name>"
								withObjectKey:@"<object key>"];
if (! task.error) {
	publicURL = task.result;
} else {
	NSLog(@"sign url error: %@", task.error);
}

```

