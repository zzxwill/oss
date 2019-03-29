# Manage buckets {#concept_32052_zh .concept}

A bucket serves as a container that stores objects. Objects belong to a bucket.

OSS uses buckets as the namespaces and management objects for advanced features such as billing, permission control, and logging. The bucket name is globally unique throughout OSS and cannot be changed. Every object stored on OSS must be included in a bucket.

## Create a bucket {#section_lwh_f2j_lfb .section}

The following code creates a bucket:

```
CreateBucketRequest createBucketRequest = new CreateBucketRequest("<bucketName>");
createBucketRequest.setBucketACL(CannedAccessControlList.PublicRead); // Specify the ACL permission of the bucket.
createBucketRequest.setLocationConstraint("oss-cn-hangzhou"); // Specify the data center that stores the bucket.
OSSAsyncTask createTask = oss.asyncCreateBucket(createBucketRequest, new OSSCompletedCallback<CreateBucketRequest, CreateBucketResult>() {
    @Override
    public void onSuccess(CreateBucketRequest request, CreateBucketResult result) {
        Log.d("locationConstraint", request.getLocationConstraint());
        }
    @Override
    public void onFailure(CreateBucketRequest request, ClientException clientException, ServiceException serviceException) {
        // Request exception
        if (clientException ! = null) {
            // Local exception, such as a network exception
            clientException.printStackTrace();
        }
        if (serviceException ! = null) {
            // Service exception
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

The preceding code specifies the ACL and data center for the created bucket.

-   Each user can have a maximum 30 buckets.
-   The name of each bucket is globally unique. If the name of a newly created bucket is the same as an in-service bucket that belongs to another user, the creation fails.
-   When you create a bucket, you can configure the bucket ACL. If no ACL is configured, by default the bucket permission is private.
-   The result of successful bucket creation be returned to the data center where the bucket is located.

## Get the bucket ACL permission { .section}

The following code gets the ACL of a bucket:

```
GetBucketACLRequest getBucketACLRequest = new GetBucketACLRequest("<bucketName>");
OSSAsyncTask getBucketAclTask = oss.asyncGetBucketACL(getBucketACLRequest, new OSSCompletedCallback<GetBucketACLRequest, GetBucketACLResult>() {
    @Override
    public void onSuccess(GetBucketACLRequest request, GetBucketACLResult result) {
        Log.d("BucketAcl", result.getBucketACL());
        Log.d("Owner", result.getBucketOwner());
        Log.d("ID", result.getBucketOwnerID());
    }
    @Override
    public void onFailure(GetBucketACLRequest request, ClientException clientException, ServiceException serviceException) {
        // Request exception
        if (clientException ! = null) {
            // Local exception, such as a network exception
            clientException.printStackTrace();
        }
        if (serviceException ! = null) {
            // Service exception
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
    }
});
```

When you use the preceding code to obtain the bucket ACL permission, consider the following points:

-   Currently, three access permissions are available for a bucket: public-read-write, public-read, and private.
-   Only the bucket owner can use the GetBucketACL interface.
-   The returned result contains the bucket ACL permission, ID, and name \(consistent with the ID\) of the bucket owner.

## Delete a bucket { .section}

The following code deletes a bucket:

```
DeleteBucketRequest deleteBucketRequest = new DeleteBucketRequest("<bucketName>");
OSSAsyncTask deleteBucketTask = oss.asyncDeleteBucket(deleteBucketRequest, new OSSCompletedCallback<DeleteBucketRequest, DeleteBucketResult>() {
    @Override
    public void onSuccess(DeleteBucketRequest request, DeleteBucketResult result) {
        Log.d("DeleteBucket", "Success!") ;
    }
    @Override
    public void onFailure(DeleteBucketRequest request, ClientException clientException, ServiceException serviceException) {
            // Request exception
        if (clientException ! = null) {
            // Local exception, such as a network exception
            clientException.printStackTrace();
        }
        if (serviceException ! = null) {
            // Service exception
            Log.e("ErrorCode", serviceException.getErrorCode());
            Log.e("RequestId", serviceException.getRequestId());
            Log.e("HostId", serviceException.getHostId());
            Log.e("RawMessage", serviceException.getRawMessage());
        }
});
```

**Note:** 

-   To prevent accidental deletion, OSS does not allow users to delete a non-empty bucket.
-   Only the bucket owner has the permission to delete the bucket.

