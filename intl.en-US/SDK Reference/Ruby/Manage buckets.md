# Manage buckets {#concept_32116_zh .concept}

A bucket is a namespace on the OSS and a management entity for billing, access control, logging, and other advanced features.

## View all buckets { .section}

The `Client#list_buckets` interface is used to list all buckets of the current user. Set the `:prefix` parameter to list only the buckets with the specific prefix in the name.

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

buckets = client.list_buckets
buckets.each { |b| puts b.name }

buckets = client.list_buckets(:prefix => 'my-')
buckets.each { |b| puts b.name }

```

## Create a bucket { .section}

The following code uses the `Client#create_bucket` interface to create a bucket. Specify the name of the bucket to be created:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

client.create_bucket('my-bucket')


```

**Note:** 

-   For bucket naming conventions, see [Basic OSS Concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).
-   The bucket name has to be globally unique. Make sure your bucket name is different from other users’.

## Delete a bucket { .section}

The following code uses the `Client#delete_bucket` interface to delete a bucket. Specify the name of the bucket to be deleted:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

client.delete_bucket('my-bucket')

```

**Note:** 

-   If the bucket to be deleted has objects in it, delete the objects first.
-   If the bucket has an incomplete upload request, use `list_uploads` and `abort_upload` to cancel the request before you delete the bucket. For the usage, see [API Documentation](http://www.rubydoc.info/gems/aliyun-sdk/).

## Check whether a bucket exists { .section}

The following code uses the `Client#bucket_exists` interface to check whether a specified bucket of the current user exists:

```language-ruby
require 'aliyun/oss'

client = Aliyun::OSS::Client.new(
  endpoint: 'endpoint',
  access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')

puts client.bucket_exists?(' my-bucket')

```

## Bucket access permissions { .section}

You can set the ACL policy of a bucket to allow or prohibit anonymous reads/writes to the bucket. For more information on ACL, see [ACL](../../../../../reseller.en-US/Developer Guide/Access and control/Overview.md#).

-   Obtain the ACL of a bucket

    The following code uses `Bucket#acl` to view the ACL of a bucket:

    ```language-ruby
    require 'aliyun/oss'
    
    client = Aliyun::OSS::Client.new(
      endpoint: 'endpoint',
      access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')
    
    bucket = client.get_bucket('my-bucket')
    puts bucket.acl
    
    ```

-   Set the ACL policy of a bucket

    The following code uses `Bucket#acl=` to configure the ACL of a bucket:

    ```language-ruby
    require 'aliyun/oss'
    
    client = Aliyun::OSS::Client.new(
      endpoint: 'endpoint',
      access_key_id: 'AccessKeyId', access_key_secret: 'AccessKeySecret')
    
    bucket = client.get_bucket('my-bucket')
    bucket.acl = Aliyun::OSS::ACL::PUBLIC_READ
    puts bucket.acl
    
    ```


