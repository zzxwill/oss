# 有关Object命令 {#concept_w2w_l2c_wdb .concept}

本文主要介绍与对象（Object）相关的命令。

**说明：** 文中涉及的相关概念请参考[基本概念介绍](../../../../cn.zh-CN/开发指南/基本概念介绍.md#)。

## ls\(list\) {#section_j35_z2c_wdb .section}

命令说明：

`ls(list) oss://bucket/[prefix] [marker] [delimiter] [maxkeys]`

列出存储空间（Bucket）中的Object。填写prefix时，可以列出以指定前缀开头的所有文件。例如，prefix为abc，则列出名称以abc开头的所有Object。

使用示范：

-   `python osscmd ls oss://mybucket/folder1/folder2`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1 /`
-   `python osscmd ls oss://mybucket/`
-   `python osscmd list oss://mybucket/ "" "" 100`

命令说明：

``` {#codeblock_cpy_50a_4cm}
ls(list) oss://bucket/[prefix] --marker=xxx --delimiter=xxx --maxkeys=xxx
        --encoding_type=url
```

列出Bucket中的Object。其中encoding\_type可以指定传输中使用的编码。当指定为url编码时，支持显示含控制字符的Object。

使用示范：

-   `python osscmd ls oss://mybucket/folder1/folder2 --delimiter=/`
-   `python osscmd ls oss://mybucket/folder1/folder2 --marker=a`
-   `python osscmd ls oss://mybucket/folder1/folder2 --maxkeys=10`

## mkdir {#section_pt5_wgc_wdb .section}

命令说明：

`mkdir oss://bucket/dirname`

创建一个文件夹。

使用示范：

 `python osscmd mkdir oss://mybucket/folder`

## listallobject {#section_rt5_wgc_wdb .section}

命令说明：

`listallobject oss://bucket/[prefix]`

列出Bucket下所有的Object，可以指定prefix来显示。

使用示范：

-   `python osscmd listallobject oss://mybucket`
-   `python osscmd listallobject oss://mybucket/testfolder/`

## deleteallobject {#section_tt5_wgc_wdb .section}

命令说明：

`deleteallobject oss://bucket/[prefix]`

删除Bucket下所有的Object，可以指定prefix来删除。

使用示范：

-   `python osscmd deleteallobject oss://mybucket`
-   `python osscmd deleteallobject oss://mybucket/testfolder/`

## downloadallobject {#section_vt5_wgc_wdb .section}

命令说明：

``` {#codeblock_0zb_k3k_t8g}
downloadallobject oss://bucket/[prefix] localdir --replace=false
      --thread_num=5
```

将Bucket下的Object下载到本地目录，并且保持目录结构。可以指定prefix下载。—replace=false表示下载时不会覆盖本地的同名文件，为true时则覆盖。同时可以通过thread\_num来配置下载线程。

使用示范：

-   `python osscmd downloadallobject oss://mybucket /tmp/folder`
-   ``` {#codeblock_59y_i1y_pwe}
python osscmd downloadallobject oss://mybucket /tmp/folder
        –-replace=false
```

-   ``` {#codeblock_vqq_c9w_wfa}
python osscmd downloadallobject oss://mybucket /tmp/folder –-replace=true
          --thread_num=5
```


## downloadtodir {#section_xt5_wgc_wdb .section}

命令说明：

`downloadtodir oss://bucket/[prefix] localdir --replace=false`

将Bucket下的Object下载到本地目录，并且保持目录结构。可以指定prefix下载。—replace=false表示下载时不会覆盖本地的同名文件，为true时则覆盖。downloadtodir与downloadallobject 效果一样。

使用示范：

-   `python osscmd downloadtodir oss://mybucket /tmp/folder`
-   `python osscmd downloadtodir oss://mybucket /tmp/folder –-replace=false`
-   ``` {#codeblock_j2f_uft_bdn}
python osscmd downloadtodir oss://mybucket /tmp/folder
      –-replace=true
```


## uploadfromdir {#section_zt5_wgc_wdb .section}

命令说明：

``` {#codeblock_fa1_7j9_sz0}
uploadfromdir localdir oss://bucket/[prefix] --check_point=check_point_file --replace=false
        --check_md5=false --thread_num=5
```

将本地目录里的文件上传到Bucket中。

例如localdir为`/tmp/`，里面有a/b、a/c、a三个文件，则上传到OSS中为oss://bucket/a/b、oss://bucket/a/c、oss://bucket/a。如果指定了prefix为mytest，则上传到OSS中为oss://bucket/mytest/a/b、oss://bucket/mytest/a/c、oss://bucket/mytest/a。

`--check_point=check_point_file`用于指定文件。指定文件后，osscmd会将已经上传的本地文件以时间戳的方式存放到check\_point\_file中，uploadfromdir命令会将正在上传的文件的时间戳和check\_point\_file记录的时间戳进行比较。如果有变化则会重新上传，否则跳过。默认情况下没有check\_point\_file。--replace=false表示下载时不会覆盖本地的同名文件，为true时则覆盖。`--check_md5=false`表示上传文件时不会校验携带Content-MD5请求头，为true时则校验。

> 注意：check\_point\_file文件中记录的是上传的所有文件的。

使用示范：

-   `python osscmd uploadfromdir /mytemp/folder oss://mybucket`
-   ``` {#codeblock_ber_zlp_gad}
python osscmd uploadfromdir /mytemp/folder oss://mybucket
          --check_point_file=/tmp/mytemp_record.txt
```

-   ``` {#codeblock_3mu_42c_7l9}
python osscmd uploadfromdir C:\Documents and Settings\User\My Documents\Downloads
          oss://mybucket --check_point_file=C:\cp.txt
```


## put {#section_b55_wgc_wdb .section}

命令说明：

``` {#codeblock_l82_ptu_vfe}
put localfile oss://bucket/object --content-type=[content_type]
        --headers="key1:value1#key2:value2" --check_md5=false
```

上传一个本地的文件到Bucket中，可以指定Object的content-type，或指定自定义的headers。`--check_md5=false`表示上传文件时不会校验携带Content-MD5请求头，为true时则校验。

使用示范：

-   `python osscmd put myfile.txt oss://mybucket`
-   `python osscmd put myfile.txt oss://mybucket/myobject.txt`
-   ``` {#codeblock_b6v_s6d_0ch}
python osscmd put myfile.txt oss://mybucket/test.txt --content-type=plain/text
          --headers=“x-oss-meta-des:test#x-oss-meta-location:CN”
```

-   ``` {#codeblock_kf0_wvl_1pj}
python osscmd put myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```


## upload {#section_d55_wgc_wdb .section}

命令说明：

``` {#codeblock_rwz_1kn_rhn}
upload localfile oss://bucket/object --content-type=[content_type]
      --check_md5=false
```

将本地文件以Object group的形式上传。不推荐使用。`--check_md5=false`表示上传文件时不会校验携带Content-MD5请求头，为true时则校验。

使用示范：

``` {#codeblock_lo1_gye_87k}
python osscmd upload myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```

## get {#section_f55_wgc_wdb .section}

命令说明：

`get oss://bucket/object localfile`

将object下载到本地文件。

使用示范：

 `python osscmd get oss://mybucket/myobject /tmp/localfile`

## multiget\(multi\_get\) {#section_h55_wgc_wdb .section}

命令说明：

`multiget(multi_get) oss://bucket/object localfile --thread_num=5`

将Object以多线程的方式下载到本地文件。同时可以配置线程数。

使用示范：

-   `python osscmd multiget oss://mybucket/myobject /tmp/localfile`
-   `python osscmd multi_get oss://mybucket/myobject /tmp/localfile`

## cat {#section_j55_wgc_wdb .section}

命令说明：

`cat oss://bucket/object`

读取Object的内容，直接打印出来。在Object内容比较大的时候请不要使用。

使用示范：

 `python osscmd cat oss://mybucket/myobject`

## meta {#section_l55_wgc_wdb .section}

命令说明：

`meta oss://bucket/object`

读取Object的meta信息，打印出来。meta信息包括content-type、文件长度、自定义meta等内容。

使用示范：

 `python osscmd meta oss://mybucket/myobject`

## copy {#section_n55_wgc_wdb .section}

命令说明：

``` {#codeblock_08n_io7_8jq}
copy oss://source_bucket/source_object oss://target_bucket/target_object
        --headers="key1:value1#key2:value2"
```

将源Bucket中的源Object复制到目的Bucket中的目的Object。

使用示范：

 `python osscmd copy oss://bucket1/object1 oss://bucket2/object2`

## rm\(delete,del\) {#section_p55_wgc_wdb .section}

命令说明：

`rm(delete,del) oss://bucket/object --encoding_type=url`

删除Object。当指定encoding-type为url编码时，传入待删除的字串也需为url编码。

使用示范：

-   `python osscmd rm oss://mybucket/myobject`
-   `python osscmd delete oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/my%01object --encoding_type=url`

## signurl\(sign\) {#section_r55_wgc_wdb .section}

命令说明：

`signurl(sign) oss://bucket/object --timeout=[timeout_seconds]`

生成包含签名的URL，并指定超时时间。适用于bucket为私有时将特定的Object提供给他人访问。

使用示范：

-   `python osscmd sign oss://mybucket/myobject`
-   `python osscmd signurl oss://mybucket/myobject`

