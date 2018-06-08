# 有关Object命令 {#concept_w2w_l2c_wdb .concept}

## ls\(list\) {#section_j35_z2c_wdb .section}

命令说明：

`ls(list) oss://bucket/[prefix] [marker] [delimiter] [maxkeys]`

列出bucket中的object。

使用示范：

-   `python osscmd ls oss://mybucket/folder1/folder2`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1`
-   `python osscmd ls oss://mybucket/folder1/folder2 marker1 /`
-   `python osscmd ls oss://mybucket/`
-   `python osscmd list oss://mybucket/ "" "" 100`

命令说明：

```
ls(list) oss://bucket/[prefix] --marker=xxx --delimiter=xxx --maxkeys=xxx
        --encoding_type=url
```

列出bucket中的object。其中encoding\_type可以指定传输中使用的编码，当指定为url编码时，能够支持显示含控制字符的object。

使用示范：

-   `python osscmd ls oss://mybucket/folder1/folder2 --delimiter=/`
-   `python osscmd ls oss://mybucket/folder1/folder2 --marker=a`
-   `python osscmd ls oss://mybucket/folder1/folder2 --maxkeys=10`

## mkdir {#section_pt5_wgc_wdb .section}

命令说明：

`mkdir oss://bucket/dirname`

创建一个以“/”结尾的object，并且size为0。

使用示范：

-   `python osscmd mkdir oss://mybucket/folder`

## listallobject {#section_rt5_wgc_wdb .section}

命令说明：

`listallobject oss://bucket/[prefix]`

显示bucket下所有的object，可以指定prefix来显示。

使用示范：

-   `python osscmd listallobject oss://mybucket`
-   `python osscmd listallobject oss://mybucket/testfolder/`

## deleteallobject {#section_tt5_wgc_wdb .section}

命令说明：

`deleteallobject oss://bucket/[prefix]`

删除bucket下所有的object，可以指定特定的prefix来删除。

使用示范：

-   `python osscmd deleteallobject oss://mybucket`
-   `python osscmd deleteallobject oss://mybucket/testfolder/`

## downloadallobject {#section_vt5_wgc_wdb .section}

命令说明：

```
downloadallobject oss://bucket/[prefix] localdir --replace=false
      --thread_num=5
```

将bucket下的object下载到本地目录，并且保持目录结构。可以指定prefix下载。—replace=false表示如果下载时，本地已经存在同名文件，不会覆盖。true则会覆盖。同时可以通过thread\_num来配置下载线程。

使用示范：

-   `python osscmd downloadallobject oss://mybucket /tmp/folder`
-   ```
python osscmd downloadallobject oss://mybucket /tmp/folder
        –-replace=false
```

-   ```
python osscmd downloadallobject oss://mybucket /tmp/folder –-replace=true
          --thread_num=5
```


## downloadtodir {#section_xt5_wgc_wdb .section}

命令说明：

`downloadtodir oss://bucket/[prefix] localdir --replace=false`

将bucket下的object下载到本地目录，并且保持目录结构。可以指定prefix下载。—replace=false表示如果下载时，本地已经存在同名文件，不会覆盖。true则会覆盖。同downloadallobject 效果一样。

使用示范：

-   `python osscmd downloadtodir oss://mybucket /tmp/folder`
-   `python osscmd downloadtodir oss://mybucket /tmp/folder –-replace=false`
-   ```
python osscmd downloadtodir oss://mybucket /tmp/folder
      –-replace=true
```


## uploadfromdir {#section_zt5_wgc_wdb .section}

命令说明：

```
uploadfromdir localdir oss://bucket/[prefix] --check_point=check_point_file --replace=false
        --check_md5=false --thread_num=5
```

将本地目录里的文件上传到bucket中。例如localdir为`/tmp/`

里面有`a/b`，`a/c`，`a`三个文件，则上传到OSS中为`oss://bucket/a/b`，`oss://bucket/a/c`，`oss://bucket/a`。如果指定了prefix为mytest，则上传到OSS中为`oss://bucket/mytest/a/b`，`oss://bucket/mytest/a/c`，`oss://bucket/mytest/a`。

`--check_point=check_point_file`是指定文件。指定文件后，osscmd会将已经上传的本地文件以时间戳的方式放到check\_point\_file中，uploadfromdir命令会将正在上传的文件的时间戳和check\_point\_file记录的时间戳进行比较。如果有变化则会重新上传，否则跳过。默认情况下是没有check\_point\_file的。`--replace=false`表示如果下载时，本地已经存在同名文件，不会覆盖。true则会覆盖。`--check_md5=false`表示上传文件时，不会做携带Content-MD5请求头校验。true则会做校验。

> 注意：由于check\_point\_file文件中记录的是上传的所有文件的。所以当上传文件特别多的时候，check\_point\_file会特别巨大。

使用示范：

-   `python osscmd uploadfromdir /mytemp/folder oss://mybucket`
-   ```
python osscmd uploadfromdir /mytemp/folder oss://mybucket
          --check_point_file=/tmp/mytemp_record.txt
```

-   ```
python osscmd uploadfromdir C:\Documents and Settings\User\My Documents\Downloads
          oss://mybucket --check_point_file=C:\cp.txt
```


## put {#section_b55_wgc_wdb .section}

命令说明：

```
put localfile oss://bucket/object --content-type=[content_type]
        --headers="key1:value1#key2:value2" --check_md5=false
```

上传一个本地的文件到bucket中，可以指定object的content-type，或则指定自定义的headers。`--check_md5=false`表示上传文件时，不会做携带Content-MD5请求头校验。true则会做校验。

使用示范：

-   `python osscmd put myfile.txt oss://mybucket`
-   `python osscmd put myfile.txt oss://mybucket/myobject.txt`
-   ```
python osscmd put myfile.txt oss://mybucket/test.txt --content-type=plain/text
          --headers=“x-oss-meta-des:test#x-oss-meta-location:CN”
```

-   ```
python osscmd put myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```


## upload {#section_d55_wgc_wdb .section}

命令说明：

```
upload localfile oss://bucket/object --content-type=[content_type]
      --check_md5=false
```

将本地文件以object group的形式上传。不推荐使用。`--check_md5=false`表示上传文件时，不会做携带Content-MD5请求头校验。true则会做校验。

使用示范：

-   ```
python osscmd upload myfile.txt oss://mybucket/test.txt
        --content-type=plain/text
```


## get {#section_f55_wgc_wdb .section}

命令说明：

`get oss://bucket/object localfile`

将object下载到本地文件。

使用示范：

-   `python osscmd get oss://mybucket/myobject /tmp/localfile`

## multiget\(multi\_get\) {#section_h55_wgc_wdb .section}

命令说明：

`multiget(multi_get) oss://bucket/object localfile --thread_num=5`

将object以多线程的方式下载到本地文件。同时可以配置线程数。

使用示范：

-   `python osscmd multiget oss://mybucket/myobject /tmp/localfile`
-   `python osscmd multi_get oss://mybucket/myobject /tmp/localfile`

## cat {#section_j55_wgc_wdb .section}

命令说明：

`cat oss://bucket/object`

读取object的内容，直接打印出来。在object内容比较大的时候请不要使用。

使用示范：

-   `python osscmd cat oss://mybucket/myobject`

## meta {#section_l55_wgc_wdb .section}

命令说明：

`meta oss://bucket/object`

读取object的meta信息，打印出来。meta信息包括content-type，文件长度，自定义meta等内容。

使用示范：

-   `python osscmd meta oss://mybucket/myobject`

## copy {#section_n55_wgc_wdb .section}

命令说明：

```
copy oss://source_bucket/source_object oss://target_bucket/target_object
        --headers="key1:value1#key2:value2"
```

将源bucket的源object 复制到目的bucket中的目的object。

使用示范：

-   `python osscmd copy oss://bucket1/object1 oss://bucket2/object2`

## rm\(delete,del\) {#section_p55_wgc_wdb .section}

命令说明：

`rm(delete,del) oss://bucket/object --encoding_type=url`

删除object。当指定encoding-type为url编码时，传入待删除的字串也需为url编码。

使用示范：

-   `python osscmd rm oss://mybucket/myobject`
-   `python osscmd delete oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/myobject`
-   `python osscmd del oss://mybucket/my%01object --encoding_type=url`

## signurl\(sign\) {#section_r55_wgc_wdb .section}

命令说明：

`signurl(sign) oss://bucket/object --timeout=[timeout_seconds]`

生成一个包含签名的URL，并指定超时的时间。适用于bucket为私有时将特定的object提供给他人访问。

使用示范：

-   `python osscmd sign oss://mybucket/myobject`
-   `python osscmd signurl oss://mybucket/myobject`

