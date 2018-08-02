# OSS访问域名使用规则 {#concept_hh2_4tv_tdb .concept}

## OSS域名构成规则 {#section_xyk_h5v_tdb .section}

针对OSS的网络请求，除了GetService这个API以外，其他所有请求的域名都是带有指定Bucket信息的三级域名组成的。

访问域名规则：BucketName.Endpoint。其中Endpoint表示OSS对外服务的访问域名。OSS以HTTP RESTful API的形式对外提供服务，当访问不同的Region的时候，需要不同的访问域名。Endpoint分内网和外网访问域名。例如华东1 Region的外网Endpoint是oss-cn-hangzhou.aliyuncs.com，内网Endpoint是oss-cn-hangzhou-internal.aliyuncs.com，Region和Endpoint对照表请参考[访问域名和数据中心](intl.zh-CN/开发指南/访问域名和数据中心.md#)。

## 通过外网访问OSS服务 {#section_sgf_k5v_tdb .section}

这里的外网指的是互联网。通过外网访问产生的流入流量（写）是免费的，流出流量（读）是收费的。详情请阅读[OSS服务价格页](https://www.alibabacloud.com/product/oss#pricing)

外网访问OSS有如下两种方式：

-   访问方式1：在访问的时候以URL的形式来表示OSS的资源。OSS的URL构成如下：

    ```
    <Schema>://<Bucket>.<外网Endpoint>/<Object> 
    ```

    -   Schema：HTTP或者为HTTPS
    -   Bucket：OSS存储空间
    -   Endpoint：Bucket所在数据中心的访问域名，您需要填写外网Endpoint
    -   Object：上传到OSS上的文件
    示例：如您的Region为华东1（oss-cn-hangzhou），Bucket名称为abc，Object名称为myfile/aaa.txt，那么您的外网访问地址为：

    ```
    abc.oss-cn-hangzhou.aliyuncs.com/myfile/aaa.txt
    ```

    您还可以直接将Object的URL放入HTML中使用，如下所示：

    ```
    <img src="https://abc.oss-cn-hangzhou.aliyuncs.com/mypng/aaa.png" />
    ```

-   访问方式2： 通过OSS SDK配置外网访问域名。

    OSS SDK会对您的每一个操作拼接访问域名。但您在对不同地域的Bucket进行操作的时候需要设置不同的Endpoint。

    以Java SDK为例，对华东1的Bucket进行操作时，需要在对类实例化时设置Endpoint：

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```


## 通过内网访问OSS服务 {#section_r14_5vv_tdb .section}

内网指的是阿里云产品之间的内网通信网络，例如您通过ECS云服务器访问OSS服务。内网产生的流入和流出流量均免费。

内网访问OSS有如下两种方式：

-   访问方式1：在访问的时候以URL的形式来表示OSS的资源。OSS的URL构成如下。

    ```
    <Schema>://<Bucket>.<内网Endpoint>/<Object> 
    ```

    -   Schema：HTTP或者为HTTPS
    -   Bucket：OSS存储空间
    -   Endpoint：Bucket所在数据中心的访问域名，您需要填写内网Endpoint
    -   Object：上传到OSS上的文件
    示例：如您在Region为华东1，Bucket名称为abc ，Object名称为myfile/aaa.txt，那么您的内网访问地址为：

    ```
    abc.oss-cn-hangzhou-internal.aliyuncs.com/myfile/aaa.txt
    ```

-   访问方式2：通过ECS使用OSS SDK配置内网访问域名。

    以Java SDK为例，对华东1地域的Bucket进行操作时，需要在对类实例化时设置Endpoint：

    ```
    String accessKeyId = "<key>";
      String accessKeySecret = "<secret>";
      String endpoint = "oss-cn-hangzhou-internal.aliyuncs.com";
      OSSClient client = new OSSClient(endpoint, accessKeyId, accessKeySecret);
    ```

    **说明：** 同一个Region的ECS和OSS之间内网互通，不同Region的ECS和OSS之间内网不互通。

    例如您购买了华北2（oss-cn-beijing）的ECS，其OSS有两个Bucket：

    -   其中一个Bucket为beijingres，Region为华北2，那么在华北2的ECS中可以使用`beijingres.oss-cn-beijing-internal.aliyuncs.com`来访问 beijingres 的资源。
    -   另外一个Bucket为qingdaores，Region为华北1，那么在华北2的ECS用内网地址`qingdaores.oss-cn-qingdao-internal.aliyuncs.com`是无法访问OSS的，必须使用外网地址`qingdaores.oss-cn-qingdao.aliyuncs.com`。

