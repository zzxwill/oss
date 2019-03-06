# Set ACLs {#concept_32123_zh .concept}

OSS allows you to set ACLs for buckets and objects respectively. This helps you to conveniently control external access to your resources.

You can set three ACLs for a bucket:

-   public-read-write: Anonymous users are allowed to create/retrieve/delete objects in the bucket.
-   public-read: Anonymous users are allowed to retrieve objects in the bucket.
-   private: Anonymous users are not allowed to access objects in the bucket. Signature is required for all accesses.

When a bucket is created, the private permission applies by default. You can use `bucket.acl=` to set the ACL of the bucket.

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')
puts bucket.acl

```

You can set four ACLs for an object.

-   default: The object inherits the ACL for the bucket it belongs to, that is, the ACL for the object is same as that for the bucket where the object is stored.
-   public-read-write: Anonymous users are allowed to read/write the object.
-   public-read: Anonymous users are allowed to read the object.
-   private: Anonymous users are not allowed to access objects in the bucket. Signature is required for all accesses.

When an object is created, the default permission applies by default. You can use `bucket.set_object_acl` to configure the ACL of the object.

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

bucket = client.get_bucket('my-bucket')

acl = bucket.get_object_acl('my-object')
puts acl # default
bucket.set_object_acl('my-object', Aliyun::OSS::ACL::PUBLIC_READ)
acl = bucket.get_object_acl('my-object')
puts acl # public-read

```

**Note:** 

-   If an object is configured with an ACL policy \(not default\), the object ACL takes priority during permission authentication when the object is accessed. The bucket ACL is ignored.
-   If anonymous access is allowed \(public-read or public-read-write is configured for the object\), you can directly access the object using a browser.

    ```
      For example: http://bucket-name.oss-cn-hangzhou.aliyuncs.com/object.jpg
    
    ```

-   A bucket or an object with the public ACL can be accessed by an anonymous client:

    ```language-ruby
    	require 'aliyun/oss'
    
    	# If access_key_id and access_key_secret are not specified, an anonymous client will be created. The client can access only
    	# the buckets and objects with the public ACL.
    	client = Aliyun::OSS::Client.new(endpoint: 'endpoint')
    	bucket = client.get_bucket('my-bucket')
    
    	bucket.get_object('my-object', :file => 'local_file')
    
    ```

-   For more information about ACL, see [Access Control](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).


