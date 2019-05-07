# Determine whether the specified object exists {#concept_osh_q3g_4fb .concept}

The OSS iOS SDK provides a convenient synchronous API to check whether the specified object exists in the bucket.

Use the following code to determine whether an object exists:

```language-objc
NSError * error = nil;
BOOL isExist = [client doesObjectExistInBucket:TEST_BUCKET withObjectKey:@"file1m" withError:&error];
if (! error) {
    if(isExist) {
        NSLog(@"File exists.");
    } else {
        NSLog(@"File not exists.");
    }
} else {
    NSLog(@"Error!") ;
}

```

