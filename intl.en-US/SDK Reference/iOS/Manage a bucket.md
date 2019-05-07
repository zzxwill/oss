# Manage a bucket {#concept_32065_zh .concept}

A bucket serves as a container that stores objects. Objects belong to the bucket they are created in or uploaded to.

## Create a bucket {#section_lwh_f2j_lfb .section}

Use the following code to create a bucket:

```language-objc
OSSCreateBucketRequest * create = [OSSCreateBucketRequest new];
create.bucketName = @"<bucketName>";
create.xOssACL = @"public-read";
create.location = @"oss-cn-hangzhou";

OSSTask * createTask = [client createBucket:create];

[createTask continueWithBlock:^id(OSSTask *task) {
	if (! task.error) {
		NSLog(@"create bucket success!") ;
	} else {
		NSLog(@"create bucket failed, error: %@", task.error);
	}
	return nil;
}];

```

**Note:** 

-   Each user can create a maximum of 30 buckets.
-   The name of each bucket must be globally unique. In other words, a user cannot create a bucket with the same name as existing buckets. In this case, the bucket fails to be created.
-   When you create a bucket, you can configure the bucket ACL. If no ACL is configured, the bucket ACL is Private.
-   The region where the bucket belongs to is returned if the bucket is created.

## List buckets { .section}

Use the following code to list all buckets owned by the requester:

```language-objc
OSSGetServiceRequest * getService = [OSSGetServiceRequest new];
OSSTask * getServiceTask = [client getService:getService];
[getServiceTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        OSSGetServiceResult * result = task.result;
        NSLog(@"buckets: %@", result.buckets);
        NSLog(@"owner: %@, %@", result.ownerId, result.ownerDispName);
        [result.buckets enumerateObjectsUsingBlock:^(id  _Nonnull obj, NSUInteger idx, BOOL * _Nonnull stop) {
            NSDictionary * bucketInfo = obj;
            NSLog(@"BucketName: %@", [bucketInfo objectForKey:@"Name"]);
            NSLog(@"CreationDate: %@", [bucketInfo objectForKey:@"CreationDate"]);
            NSLog(@"Location: %@", [bucketInfo objectForKey:@"Location"]);
        }];
    }
    return nil;
}];

```

**Note:** Anonymous users do not have the permission to list buckets.

## List objects in a bucket { .section}

Use the following code to list objects in a bucket:

```
OSSGetBucketRequest * getBucket = [OSSGetBucketRequest new];
getBucket.bucketName = @"<bucketName>";
// getBucket.marker = @"";
// getBucket.prefix = @"";
// getBucket.delimiter = @"";
OSSTask * getBucketTask = [client getBucket:getBucket];
[getBucketTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        OSSGetBucketResult * result = task.result;
        NSLog(@"get bucket success!") ;
        for (NSDictionary * objectInfo in result.contents) {
            NSLog(@"list object: %@", objectInfo);
        }
    } else {
        NSLog(@"get bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

**Note:** 

-   To list the objects in the bucket, you must have the operation permission on the bucket.
-   You can set the prefix, marker, delimiter, and max-keys parameters to list desired results only.

## Delete a bucket { .section}

Use the following code to delete a bucket:

```
OSSDeleteBucketRequest * delete = [OSSDeleteBucketRequest new];
delete.bucketName = @"<bucketName>";
OSSTask * deleteTask = [client deleteBucket:delete];
[deleteTask continueWithBlock:^id(OSSTask *task) {
    if (! task.error) {
        NSLog(@"delete bucket success!") ;
    } else {
        NSLog(@"delete bucket failed, error: %@", task.error);
    }
    return nil;
}];
```

-   Only the bucket owner has the permission to delete the bucket.
-   To prevent unexpected data loss, OSS does not allow bucket owners to delete non-empty buckets.

