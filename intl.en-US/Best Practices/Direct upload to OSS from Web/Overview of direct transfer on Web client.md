# Overview of direct transfer on Web client {#concept_iyn_vfy_5db .concept}

## Purpose {#section_cym_wfy_5db .section}

This document with the help of two examples, elaborates how to transfer a file in HTML form directly to OSS.

-   Example 1: Describes how to add a signature on a server \(PHP\) and then upload the file directly to OSS using a form.
-   Example 2: Describes how to add a signature on the server \(PHP\), and set the callback upon uploading on the server. Then, upload the form directly to OSS. After that, OSS calls back the application server and returns the result to the user.

## Background {#section_m3m_xfy_5db .section}

Every OSS user may use the upload service. This is because, the data is uploaded using Web pages and it includes some HTML5 pages in some apps. The demand to upload these services is strong. Many users choose to upload files to the application servers through browsers/apps, and then the application server uploads the files to OSS.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/4403/1459_en-US.png)

However, the preceding method has following limitations:

-   Low uploading speed: Intially, files are uploaded to the application server, and then to OSS. Therefore, the workload of transmission over the Internet is doubled. If the data is transferred directly to OSS without passing through the application server, the speed increases significantly. Moreover, OSS uses BGP bandwidth, thus ensuring a high speed for operators in different places.
-   Poor scalability: As the number of users increases in future, the application server may constitute a bottleneck.
-   High cost: The traffic consumed for uploading files directly to OSS is free of charge. If data is uploaded directly to OSS without passing through the application server, the costs of several application servers can be saved.

## Basic {#section_rpn_hgy_5db .section}

[The application server uses PHP script language to return the signature. Click here for the example.](intl.en-US/Best Practices/Direct upload to OSS from Web/Direct transfer after adding a signature on the server.md#)

## Advanced {#section_p4n_3gy_5db .section}

[The application server returns the signature using the PHP script language and implements uploading callback. Click here for the example.](intl.en-US/Best Practices/Direct upload to OSS from Web/Directly add a signature on the server, transfer the file, and set upload callback.md#)

