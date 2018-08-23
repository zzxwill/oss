# Set back-to-origin rules {#concept_ukn_3tf_vdb .concept}

You can set back-to-origin rules to define whether to retrieve origin data by mirroring or redirection. Back-to-origin rules are usually used for hot migration of data and redirection of specific requests. You can configure up to five back-to-source rules, which are executed in sequence.

 

**Note:** Back-to-origin does not support intranet endpoint. 

## Procedure {#section_edr_sfg_vdb .section}

1.  Log on to the [OSS console](https://oss.console.aliyun.com/) .
2.  Click one of the bucket names on the left.
3.  Click **Basic Settings**, locate **Back-to-Origin** area, and click **Edit**.
4.  Click **Create Rule**. 
5.  Select **Mirroring** or **Redirection**.
    -   If you choose **Mirroring** and a requested file cannot be found on OSS, OSS will automatically fetch the file from the origin, save it locally, and return the content to the requester.
    -   If you choose **Redirection**, OSS redirects requests that meet the prerequisites to the origin URL over HTTP, and then a browser or client returns the content from the origin to the requester.
6.  Set **Prerequisite** and **Origin URL**. In the Mirroring mode, you can choose to enable **Transfer queryString**or not. In the Redirection mode, you can set **Redirection Code**.
7.  Set transmission rule of HTTP header.

    The configuration example is as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4750/15350165669983_en-US.png)

    If the HTTP header in a request that sent to OSS is as follows:

    ```
    GET /object
    host : bucket.oss-cn-hangzhou.aliyuncs.com
    aaa-header : aaa
    bbb-header : bbb
    ccc-header : 111
    ```

    After the back-to-origin is triggered, the request that OSS sends to the origin is as follows:

    ```
    GET /object
    host : source.com
    aaa-header : aaa
    ccc-header : ccc
    
    ```

8.  Click **OK**.

**Note:** After the rule is saved, you can view the configured rule in the rule list and perform corresponding Edit or Clear operations.

