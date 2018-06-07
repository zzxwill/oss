# Bucket权限控制 {#concept_idj_zff_xdb .concept}

OSS提供ACL（Access Control List）权限控制方法，OSS ACL提供Bucket级别的权限访问控制，Bucket目前有三种访问权限：public-read-write，public-read和private，它们的含义如下：

|权限值|中文名称|权限对访问者的限制|
|:--|:---|:--------|
|public-read-write|公共读写| -   任何人（包括匿名访问）都可以对该Bucket中的Object进行读/写/删除操作。
-   所有这些操作产生的费用由该Bucket的Owner承担。

 |
|public-read|公共读，私有写| -   只有该Bucket的Owner或者授权对象可以对存放在其中的Object进行写/删除操作。
-   任何人（包括匿名访问）可以对Object进行读操作。

 |
|private|私有读写| -   只有该Bucket的Owner或者授权对象可以对存放在其中的Object进行读/写/删除操作。
-   其他人在未经授权的情况下无法访问该Bucket内的Object。

 |

**说明：** 

-   用户创建一个Bucket时，如果不指定Bucket权限，OSS会自动为该Bucket设置private权限。
-   对于一个已经存在的Bucket，只有它的创建者可以通过OSS的 Put Bucket Acl接口修改该Bucket的权限。

