# Endpoints {#concept_hh2_4tv_tdb .concept}

## Composition rules for domain names {#section_xyk_h5v_tdb .section}

In the network requests for OSS, except those for the GetService API, the domain names are the third-level domain names with specified bucket names.

The domain name contains a bucket name and an endpoint in the format of BucketName.Endpoint. An endpoint is an access domain name. OSS provides external services through HTTP RESTful APIs. Different regions use different domain names. A region has an Internet endpoint and an intranet endpoint. For example, the Internet endpoint of the region China East 1 is oss-cn-hangzhou.aliyuncs.com, and the intranet endpoint of the region China East 1 is oss-cn-hangzhou-internal.aliyuncs.com. For more information, see [Regions and endpoints](intl.en-US/Developer Guide/Regions and endpoints.md#).

## Access OSS through the Internet {#section_sgf_k5v_tdb .section}

You can always access OSS through the Internet. In the Internet, the inbound traffic \(write\) is free, and outbound traffic \(read\) is charged. For more information about outbound traffic charges, see [OSS Pricing](https://www.alibabacloud.com/product/oss#pricing).

You can access OSS through the Internet by using either of the following methods:

-   Method 1: Use the URL to access OSS resource. The URL is constructed as follows:

```
<Schema>://<Bucket>.<Internet Endpoint>/<Object>, where:
  Schema is HTTP or HTTPS.
  Bucket is your OSS storage space.
  Endpoint is the access domain name for the region of a bucket. Enter the Internet endpoint here.
  Object is a file uploaded to the OSS.
```

    For example, in the region China East 1, the object named myfile/aaa.txt is stored in the bucket abc. The Internet access address of the object is:

    ```
    abc.oss-cn-hangzhou.aliyuncs.com/myfile/aaa.txt
    ```

    You can directly use the object URL in HTML as follows:

    ```
    <img src="https://abc.oss-cn-hangzhou.aliyuncs.com/mypng/aaa.png" />
    ```

-   Method 2: Configure the Internet access domain name through OSS SDKs.

    OSS The SDK will help users access the domain name for each operation mosaic. However, you must set different endpoints when operating buckets of different regions.

    For example, before configuring buckets in the region China East 1, you must set the endpoint during class instantiation.

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```


## Access OSS through an intranet {#section_r14_5vv_tdb .section}

Intranet refers to the internal communication networks among Alibaba Cloud products. For example, you access OSS through ECS.  In an intranet, the inbound and outbound traffic is free. If the ECS instance and the OSS bucket are in the same region, we recommend that you use an intranet to access OSS.

You can access OSS through an intranet by using either of the following methods:

-   Method 1: Use the URL to access OSS resource. The URL is constructed as follows:

    ```
    <Schema>://<Bucket>.<IntranetEndpoint>/<Object>, where: 
      Schema is HTTP or HTTPS.
      Bucket is your OSS storage space.
      Endpoint is the access domain name for the region of a bucket. Enter the intranet endpoint here.
      Object is a file uploaded to the OSS.
    ```

    For example, in the region China East 1, the object named myfile/aaa.txt is stored in the bucket abc. The intranet access address of the object is:

    ```
    abc.oss-cn-hangzhou-internal.aliyuncs.com/myfile/aaa.txt 
    ```

-   Method 2: Configure the intranet access domain name using OSS SDKs on ECS.

    For example, set the intranet endpoint for the Java SDK on ECS as follows:

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou-internal.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```

    **Note:** If you want to use an intranet to access OSS, the ECS instance and the OSS bucket must be in the same region.

    For example, you have purchased ECS instances of China North 2 \(Beijing\), and you have two OSS buckets:

    -   One buckets is beijingres, and its region is China North 2 \(Beijing\). The intranet address beijingres.oss-cn-beijing-internal.aliyuncs.com can be used by `ECS instances` to access beijingres resources, and the traffic generated is free.

    -   The other bucket is qingdaores, and its region is China North 1 \(Qingdao\). The intranet address `qingdaores.oss-cn-qingdao-internal.aliyuncs.com` cannot be used by ECS instances to access qingdaores resources. The Internet address `qingdaores.oss-cn-qingdao.aliyuncs.com` must be used to access qingdaores resources, and the outbound traffic generated is charged.


