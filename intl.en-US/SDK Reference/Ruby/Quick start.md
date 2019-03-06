# Quick start {#concept_32115_zh .concept}

This topic describes how to use the OSS Ruby SDK to access OSS services to view the bucket list and object list, to upload, download, and delete the objects.

For usage convenience, the following operations are performed in `irb` which is Ruby’s interactive command-line interface \(CLI\).

## Initialize the client {#section_o5l_zfn_lfb .section}

Input the following content on the CLI and press the Enter:

```language-ruby
irb

```

Enter the Ruby interactive CLI mode. Introduce the SDK package with `require`:

```language-ruby
> require 'aliyun/oss'
=> true

```

**Note:** In the following demonstration, the `>` symbol introduces the command that the user enters, and the `=>` symbol introduces the content returned by a program.

Run the following command to create an OSSClient:

```language-ruby
> client = Aliyun::OSS::Client.new(
>   endpoint: 'endpoint',
>   access_key_id: 'AccessKeyId',
>   access_key_secret: 'AccessKeySecret')
=> #<Aliyun::OSS::Client...

```

Replace the parameters with the actual endpoint, AccessKeyID, and AccessKeySecret.

## View the bucket list { .section}

Run the following command to view the bucket list:

```language-ruby
> buckets = client.list_buckets
=> #<Enumerator...
> buckets.each { |b| puts b.name }
=> bucket-1
=> bucket-2
=> ...

```

If the bucket list is empty, run the following command to create a bucket:

```language-ruby
> client.create_bucket('my-bucket')
=> true

```

**Note:** 

-   For bucket naming conventions, see [Basic OSS Concepts](../../../../../reseller.en-US/Developer Guide/Basic concepts.md#).
-   The bucket name cannot be the same with existing bucket names of other users in OSS. To create a bucket successfully, select a unique bucket name.

## View the object list { .section}

You can run the following command to view the objects in a bucket:

```language-ruby
> bucket = client.get_bucket('my-bucket')
=> #<Aliyun::OSS::Bucket...
> objects = bucket.list_objects
=> #<Enumerator...
> objects.each { |obj| puts obj.key }
=> object-1
=> object-2
=> ...

```

## Upload an object { .section}

You can run the following command to upload a file to a bucket:

```language-ruby
> bucket.put_object('my-object', :file => 'local-file')
=> true

```

Specifically, `local-file` indicates the path to the local file to be uploaded. When the file is uploaded, you can view it using `list_objects`:

```language-ruby
> objects = bucket.list_objects
=> #<Enumerator...
> objects.each { |obj| puts obj.key }
=> my-object
=> ...

```

## Download an object { .section}

You can run the following command to download an object from a bucket:

```language-ruby
> bucket.get_object('my-object', :file => 'local-file')
=> #<Aliyun::OSS::Object...

```

Specifically, `local-file` is the path to the downloaded object. After the object is downloaded, you can open the object to view its content.

## Delete an object { .section}

You can run the following command to delete an object from a bucket:

```language-ruby
> bucket.delete_object('my-object')
=> true

```

After deletion, you can run `list_objects` to check whether the object has been successfully deleted from the bucket.

```language-ruby
> objects = bucket.list_objects
=> #<Enumerator...
> objects.each { |obj| puts obj.key }
=> object-1
=> ...

```

## Learn more { .section}

-    [Manage a bucket](reseller.en-US/SDK Reference/Ruby/Manage buckets.md#) 
-    [Upload objects](reseller.en-US/SDK Reference/Ruby/Upload objects.md#) 
-    [Download objects](reseller.en-US/SDK Reference/Ruby/Download objects.md#) 
-    [Manage objects](reseller.en-US/SDK Reference/Ruby/Manage objects.md#)
-    [Integration with Rails](reseller.en-US/SDK Reference/Ruby/Application of Rails.md#) 
-    [CNAME](reseller.en-US/SDK Reference/Ruby/CNAME.md#) 
-    [Use STS to access OSS](reseller.en-US/SDK Reference/Ruby/Use STS to access OSS.md#) 
-    [Set access permissions](reseller.en-US/SDK Reference/Ruby/Set ACLs.md#) 
-    [Manage lifecycle rules](reseller.en-US/SDK Reference/Ruby/Manage lifecycle rules.md#) 
-    [Set logging](reseller.en-US/SDK Reference/Ruby/Set logging.md#)
-    [Static website hosting](reseller.en-US/SDK Reference/Ruby/Static website hosting.md#)
-    [Anti-leech](reseller.en-US/SDK Reference/Ruby/Anti-leech.md#) 
-    [CORS](reseller.en-US/SDK Reference/Ruby/CORS.md#) 
-    [Exception handling](reseller.en-US/SDK Reference/Ruby/Exception handling.md#) 

