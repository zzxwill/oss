# SelectObject {#reference_lz1_r1x_b2b .reference}

## 功能介绍 {#section_jwy_bbx_b2b .section}

对象存储（Object Storage Service，简称OSS） 是基于阿里云飞天分布式系统的海量、安全和高可靠的云存储服务，是一种面向互联网的大规模、低成本、通用存储，提供RESTful API，具备容量和处理的弹性扩展能力。OSS不仅非常适合存储海量的媒体文件，也适合作为数据仓库存储海量的数据文件。目前Hadoop 3.0已经支持OSS，在EMR上运行Spark/Hive/Presto等服务以及阿里自研的MaxCompute、HybridDB以及新上线的Data Lake Analytics都支持从OSS直接处理数据。

然而，目前OSS提供的GetObject接口决定了大数据平台只能把OSS数据全部下载到本地然后进行分析过滤，在很多查询场景下浪费了大量带宽和客户端资源。

SelectObject接口是对上述问题的解决方案。其核心思想是大数据平台将条件、Projection下推到OSS层，让OSS做基本的过滤，从而只返回有用的数据。客户端一方面可以减少网络带宽，另一方面也减少了数据的处理量，从而节省了CPU和内存用来做其他更多的事情。这使得基于OSS的数据仓库、数据分析成为一种更有吸引力的选择。

SelectObject提供了Java、Python 的SDK。目前支持RFC 4180标准的CSV（包括TSV等类CSV文件，文件的行列分隔符以及Quote字符都可自定义），且文件编码为UTF-8。支持标准存储类型和低频访问存储类型的文件（归档文件需要先取回）。支持加密文件（OSS完全托管、KMS加密-默认KMS主密钥）。

支持的SQL语法如下：

-   SQL 语句： Select From Where
-   数据类型：String, int\(64bit\), float\(64bit\), decimal\(128\) , timestamp, Boolean
-   操作： 逻辑条件（AND,OR,NOT\)， 算术表达式（+-\*/%\)， 比较操作\(\>,=, <, \>=, <=, !=\)，String 操作 \(LIKE, || \)

和GetObject提供了基于Byte的分片下载类似，SelectObject也提供了分片查询的机制，包括两种分片方式：按行分片和按Split分片。按行分片是常用的分片方式，然而对于稀疏数据来说，按行分片可能会导致分片时负载不均衡。Split是OSS用于分片的一个概念，一个Split包含多行数据，每个Split的数据大小大致相等，相对按行来，按Spit是更加高效的分片方式。尤其是对于CSV数据来说，基于Byte的分片可能会将数据破坏，因此按Spit分片更加合适。

关于数据类型，OSS中的CSV数据默认都是String类型，用户可以使用CAST函数实现数据转换，比如下面的SQL查询将\_1和\_2转换为int后进行比较。

`Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

同时，对于SelectObject支持在Where条件中进行隐式的转换，比如下面的语句中第一列和第二列将被转换成int：

`Select _1 from ossobject where _1 + _2 > 100`

## SelectObject使用说明 {#section_tp1_zcx_b2b .section}

对目标CSV文件执行SQL语句，返回执行结果。同时该命令会自动保存CSV文件的metadata信息，比如总的行数和列数等。

正确执行时，该API返回206。如果SQL语句不正确，或者和CSV文件不匹配，则会返回400错误。

**请求语法**

```
POST /object?x-oss-process=csv/select HTTP/1.1 
HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: time GMT
Content-Length: ContentLength
Content-MD5: MD5Value 
Authorization: Signature

<?xml  version="1.0"  encoding="UTF-8"?>
<SelectRequest>
	<Expression>base64 encode(Select * from OSSObject where ...)</Expression>
	<InputSerialization>
		<CompressionType>None|GZIP</CompressionType>
		<CSV>
			<FileHeaderInfo>NONE|IGNORE|USE</FileHeaderInfo>
			<RecordDelimiter>base64 encode</RecordDelimiter>
			<FieldDelimiter>base64 encode</FieldDelimiter>
			<QuoteCharacter>base64 encode</QuoteCharacter>
			<CommentCharacter>base64 encode</CommentCharacter>
			<Range>line-range=start-end|split-range=start-end</Range>
		</CSV>
  </InputSerialization>
  <OutputSerialization>
        <CSV>
			<RecordDelimiter>base64 encode</RecordDelimiter>
			<FieldDelimiter>base64 encode</FieldDelimiter>
			<KeepAllColumns>false|true</KeepAllColumns>
		</CSV>
	<OutputRawData>false|true</OutputRawData>
     	<EnablePayloadCrc>true</EnablePayloadCrc>
		<OutputHeader>false</OutputHeader>
  </OutputSerialization>
  <Options>
	<SkipPartialDataRecord>false</SkipPartialDataRecord>
  </Options>
</SelectRequest>
```

|名称|类型|描述|
|--|--|--|
|SelectRequest|容器| 保存Select请求的容器

 子节点：Expression, InputSerialization，OutputSerialization

 父节点：None

 |
|Expression|字符串| 以Base64 编码的SQL语句

 子节点：None

 父节点：SelectRequest

 |
|InputSerialization|容器| 输入序列化参数（可选）

 子节点:CompressionType, CSV

 父节点：SelectRequest

 |
|OutputSerialization|容器| 输出序列化参数（可选）

 子节点：CSV，OutputRawData

 父节点：SelectRequest

 |
|CSV（InputSerialization）|容器| 输入CSV的格式参数（可选）

 子节点：FileHeaderInfo、RecordDelimiter、FieldDelimiter、QuoteCharacter、CommentCharacter、 Range

 父节点：InputSerialization

 |
|CSV\(OutputSerialization\)|容器| 输出CSV的格式参数（可选）

 子节点： RecordDelimiter、 FieldDelimiter、KeepAllColumns

 父节点：OutputSerialization

 |
|OutputRawData|bool，默认false| 指定输出数据为纯数据（不是下面提到的基于Frame格式）（可选）

 子节点：None

 父节点：OutputSerialization

 |
|CompressionType|枚举| 指定文件压缩类型：None|GZIP

 子节点：None

 父节点：InputSerialization

 |
|FileHeaderInfo|枚举| 指定CSV文件头信息（可选）

 取值：

 子节点：None

 父节点：CSV （输入）

 |
|RecordDelimiter|字符串| 指定CSV换行符，以Base64编码。默认值为`\n`（可选）。未编码前的值最多为两个字符，以字符的ANSI值表示，比如在Java里用`\n`表示换行。

 子节点：None

 父节点：CSV （输入、输出）

 |
|FieldDelimiter|字符串| 指定CSV列分隔符，以Base64编码。默认值为，（可选）

 未编码前的值必须为一个字符，以字符的ANSI值表示，比如Java里用，表示逗号。

 子节点：None

 父节点：CSV （输入，输出）

 |
|QuoteCharacter|字符串| 指定CSV的引号字符，以Base64编码。默认值为`\”`（可选）。在CSV中引号内的换行符，列分隔符将被视作普通字符。为编码前的值必须为一个字符，以字符的ANSI值表示，比如Java里用`\”`表示引号。

 子节点：None

 父节点：CSV （输入）

 |
|CommentCharacter|字符串|指定CSV的注释符，以Base464编码。默认值为`#`（可选）|
|Range|字符串| 指定查询文件的范围（可选）。支持两种格式：

 其中start和end均为inclusive。其格式和range get中的range参数一致。

 子节点：None

 父节点：CSV （输入）

 |
|KeepAllColumns|bool| 指定返回结果中包含CSV所有列的位置（可选，默认值为false）。但仅仅在select 语句里出现的列会有值，不出现的列则为空，返回结果中每一行的数据按照CSV列的顺序从低到高排列。比如下面语句：

 `select _5, _1 from ossobject.`

 如果KeepAllColumn = true，假设一共有6列数据，则返回的数据如下：

 Value of 1st column,,,,Value of 5th column,\\n

 子节点：None

 父节点：CSV（输出）

 |
|EnablePayloadCrc|bool| 在每个Frame中会有一个32位的crc32校验值。客户端可以计算相应payload的Crc32值进行数据完整性校验。

 子节点：None

 父节点：OutputSerialization

 |
|Options|容器| 额外的可选参数

 类型：容器

 子节点：SkipPartialDataRecord

 父节点：SelectRequest

 |
|OutputHeader|bool| 在返回结果开头输出CSV头信息。

 类型：bool，默认 false。

 子节点：None

 父节点：OutputSerialization

 |
|SkipPartialDataRecord|bool| 忽略缺失数据的行。当该参数为true时 ，OSS会忽略缺失某些列的行而不报错。当该参数为false时，CSV文件中有任何一行数据不完整时，OSS会报错并停止处理。

 类型：bool，默认 false。

 子节点:None

 父节点：Options

 |

**返回结果**

请求结果以一个个Frame形式返回。每个Frame的格式如下,其中checksum均为CRC32：

Version|Frame-Type | Payload Length | Header Checksum | Payload | Payload Checksum

<=1 byte\><--3 bytes--\><---4 bytes----\><-------4 bytes--\><variable\><----4bytes----------\>

Frame里所有的整数均以大端编码\(big endian\)。 Version目前为1。

对于SelectObject这个API一共有三种不同的Frame Type， 列举如下：

|名称|Frame-Type值|Payload格式|描述|
|:-|:----------|:--------|:-|
|Data Frame|8388609| offset  |  data

 <-8 bytes\><---variable-\>

 |DataFrame包含Select请求返回的数据，并用offset来汇报进展，offset为当前扫描位置（从文件头开始的偏移），是8位整数。|
|Continuous Frame|8388612| offset

 <----8 bytes--\>

 |Continuous Frame用以汇报当前进展以及维持http连接。如果该查询在5s内未返回数据则会返回一个Continuous Frame。|
|End Frame|8388613| offset  | total scanned bytes | http status code | error message

 <--8bytes-\><--8bytes---------\><----4 bytes--------\><-variable------\>

 |其中offset为扫描后最终的位置偏移，total scanned bytes为最终扫描过的数据大小。http status code为最终的处理结果，error message为错误信息。这里返回status code的原因在于SelectObject为流式处理，因而在发送Response Header的时候仅仅处理了第一个Block。如果第一个Block数据和SQL是匹配的，则在Response Header中的Status为206，但如果下面的数据非法，我们已无法更改Header中的Status，只能在End Frame里包含最终的Status及其出错信息。因此客户端应该视其为最终状态。|

**样例请求**

```
POST /oss-select/bigcsv_normal.csv?x-oss-process=csv%2Fselect HTTP/1.1
Date: Fri, 25 May 2018 22:11:39 GMT
Content-Type:
Authorization: OSS LTAIJPXxMLocA0fD:FC/9JRbBGRw4o2QqdaL246Pxuvk=
User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
Content-Length: 748
Expect: 100-continue
Connection: keep-alive
Host: host name

<?xml version="1.0"?>
<SelectRequest>
	<Expression>c2VsZWN0IGNvdW50KCopIGZyb20gb3Nzb2JqZWN0IHdoZXJlIF80ID4gNDU=
	</Expression>
	<InputSerialization>
		<Compression>None</Compression>
		<CSV>
			<FileHeaderInfo>Ignore</FileHeaderInfo>
			<RecordDelimiter>Cg==</RecordDelimiter>
			<FieldDelimiter>LA==</FieldDelimiter>
			<QuoteCharacter>Ig==</QuoteCharacter>
			<Comments>Iw==</Comments>
		</CSV>
	</InputSerialization>
	<OutputSerialization>
		<CSV>
			<RecordDelimiter>Cg==</RecordDelimiter>
			<FieldDelimiter>LA==</FieldDelimiter>
			<QuoteCharacter>Ig==</QuoteCharacter>
			<KeepAllColumns>false</KeepAllColumns>
	</CSV>
	<OutputRawData>false</OutputRawData>
 </OutputSerialization>
</SelectRequest>

```

**SQL语句正则表达式**

`SELECT select-list from OSSObject where_opt limit_opt`

其中SELECT, OSSOBJECT以及 WHERE为关键字不得更改。

```
select_list: column name

| column index (比如_1, _2)

| function(column index | column name)

| select_list AS alias
```

支持的function为AVG,SUM,MAX,MIN,COUNT, CAST\(类型转换函数\)。其中COUNT后只能用\*。

```
Where_opt:
| WHERE expr
expr:
| literal value
| column name
| column index
| expr op expr
| expr OR expr
| expr AND expr
| expr IS NULL
| expr IS NOT NULL
| expr IN (value1, value2,….)
| expr NOT in (value1, value2,…)
| expr between value1 and value2
| NOT (expr)
| expr op expr
| (expr)
| cast (column index or column name or literal as INT|DOUBLE|DATETIME)
```

op：包括 \> < \>= <= != =, LIKE，+-\*/%以及字符串连接||。

cast: 对于同一个column，只能cast成一种类型。

`limit_opt:`

| limit 整数

聚合和Limit的混用

`Select avg(cast(_1 as int)) from ossobject limit 100`

对于上面的语句，其含义是指在前100行中计算第一列的AVG值。这个行为和MY SQL不同，原因是在 SelectObject中聚合永远只返回一行数据，因而对聚合来说限制其输出规模是多余的。因此SelectObject里limit 将先于聚合函数执行。

SQL语句限制

-   目前仅仅支持UTF-8编码的文本文件以及GZIP压缩过的UTF-8文本。GZIP文件不支持deflate格式。
-   仅支持单文件查询，不支持join, order by, group by, having
-   Where语句里不能包含聚合条件\(e.g. where max\(cast\(age as int\)\) \> 100这个是不允许的\)。
-   支持的最大的列数是1000，SQL中最大的列名称为1024。
-   在LIKE语句中，支持最多5个%通配符。\*和%是等价的，表示0或多个任意字符。
-   在IN语句中，最多支持1024个常量项。
-   Select后的Projection可以是列名，列索引\(\_1, \_2等\)，或者是聚合函数，或者是CAST函数；不支持其他表达式。 比如select \_1 + \_2 from ossobject是不允许的。
-   支持的最大行及最大列长度是都是256K。
-   SQL最大长度16K，where后面表达式个数最多20个，表达式深度最多10层，聚合操作最多100个。

## CREATE SELECT OBJECT META {#section_pbq_vfy_b2b .section}

Create Select Object Meta API作用获得目标CSV文件的总的行数，总的列个数，以及Splits个数。如果该信息不存在，则会扫描整个文件分析并记录下CSV文件的上述信息。如果该API执行正确，返回200。否则如果目标CSV文件为非法、或者指定的分隔符和目标CSV不匹配，则返回400。

**请求语法**

```
POST  /samplecsv?x-oss-process=csv/meta


<CsvMetaRequest>
	<InputSerialization>
		<CompressionType>None</CompressionType>
		<CSV>
			<RecordDelimiter>base64 encode</RecordDelimiter>
			<FieldDelimiter>base64 encode</FieldDelimiter>
			<QuoteCharacter>base64 encode</QuoteCharacter>
		</CSV>
	</InputSerialization>
	<OverwriteIfExists>false|true</OverwriteIfExists>
</CsvMetaRequest>
```

**请求元素**

|名称|类型|描述|
|:-|--|:-|
|CsvMetaRequest|容器| 保存创建Select Meta请求的容器。

 子节点：Expression、 InputSerialization、 OutputSerialization

 父节点：None

 |
|InputSerialization|容器| 输入序列化参数（可选）

 子节点：CompressionType、 CSV

 父节点：CsvMetaRequest

 |
|OverwriteIfExists|bool| 重新计算SelectMeta，覆盖已有数据。（可选，默认是false，即如果Select Meta已存在则直接返回）

 子节点：None

 父节点：CsvMetaRequest

 |
|CompressionType|枚举| 指定文件压缩类型。目前不支持任何压缩，故只能为None

 子节点： None

 父节点：InputSerialization

 |
|RecordDelimiter|字符串| 指定CSV换行符，以Base64编码。默认值为’\\n’（可选）。未编码前的值最多为两个字符，以字符的ANSI值表示，比如在Java里用‘\\n’表示换行。

 子节点：None

 父节点：CSV

 |
|FieldDelimiter|字符串| 指定CSV列分隔符，以Base64编码。默认值为，（可选）

 未编码前的值必须为一个字符，以字符的ANSI值表示，比如Java里用，表示逗号。

 子节点：None

 父节点：CSV （输入，输出）

 |
|QuoteCharacter|字符串| 指定CSV的引号字符，以Base64编码。默认值为\\”（可选）。在CSV中引号内的换行符，列分隔符将被视作普通字符。为编码前的值必须为一个字符，以字符的ANSI值表示，比如Java里用\\”表示引号。

 子节点：None

 父节点：CSV （输入）

 |
|CSV|容器| 指定CSV输入格式

 子节点：RecordDelimiter，FieldDelimiter，QuoteCharacter

 父节点：InputSerialization

 |

Response Body：和SelectObject 类似，Create Meta API也以Frame的形式返回。有两种类型的Type：Continuous Frame以及End Meta Frame。Continuous Frame和SelectObject API完全一致。

|名称|Frame-Type值|Payload格式|描述|
|--|-----------|---------|--|
|Meta End Frame|8388614| offset | status| splits count | rows count | columns count | error message

 <-8 bytes\><--4bytes\><--4 bytes--\><--8 bytes\><--4 bytes---\><variable size\>

 | offset：8位整数，扫描结束时的文件偏移。

 status：4位整数，最终的status

 splits\_count：4位整数，总split个数。

 rows\_count：8位整数，总行数。

 cols\_count：4位整数，总列数。

 error\_message：详细的错误信息，若没有错误则为空。

 Meta End Frame用来汇报Create Select Meta API最终的状态。

 |

Response Header：无专门header。

**样例请求**

```
POST /oss-select/bigcsv_normal.csv?x-oss-process=csv%2Fmeta HTTP/1.1
Date: Fri, 25 May 2018 23:06:41 GMT
Content-Type:
Authorization: OSS LTAIJPXxMLocA0fD:2WF2l6zozf+hzTj9OSXPDklQCvE=
User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
Content-Length: 309
Expect: 100-continue
Connection: keep-alive
Host: Host

<?xml version="1.0"?>
<CsvMetaRequest>
	<InputSerialization>
		<CSV>
			<RecordDelimiter>Cg==</RecordDelimiter>
			<FieldDelimiter>LA==</FieldDelimiter>
			<QuoteCharacter>Ig==</QuoteCharacter>
		</CSV>
	</InputSerialization>
	<OverwriteIfExisting>false</OverwriteIfExisting>
</CsvMetaRequest>

```

**返回响应**

```
HTTP/1.1 200 OK
Server: AliyunOSS
Date: Fri, 25 May 2018 23:06:42 GMT
Content-Type: application/vnd.ms-excel
Content-Length: 0
Connection: close
x-oss-request-id: 5B089702461FB4C07B000C75
x-oss-location: oss-cn-hangzhou-a
x-oss-access-id: LTAIJPXxMLocA0fD
x-oss-sign-type: NormalSign
x-oss-object-name: bigcsv_normal.csv
Accept-Ranges: bytes
ETag: "3E1372A912B4BC86E8A51234AEC0CA0C-400"
Last-Modified: Wed, 09 May 2018 00:22:32 GMT
x-oss-object-type: Multipart
x-oss-bucket-storage-type: standard
x-oss-hash-crc64ecma: 741622077104416154
x-oss-storage-class: Standard
**x-oss-select-csv-rows: 54000049**
**x-oss-select-csv-columns: 4**
**x-oss-select-csv-splits: 960**
```

## Python SDK 样例 {#section_hd3_1ky_b2b .section}

```
import os
import oss2

def  select_call_back(consumed_bytes, total_bytes =  None):
	print('Consumed Bytes:'  +  str(consumed_bytes) +  '\n')
	
# 首先初始化AccessKeyId、AccessKeySecret、Endpoint等信息。
# 通过环境变量获取，或者把诸如“<yourAccessKeyId>”替换成真实的AccessKeyId等。
#
# 以杭州区域为例，Endpoint可以是：
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com

access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<yourAccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<yourAccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<yourBucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<yourEndpoint>')

# 创建存储空间实例，所有文件相关的方法都需要通过存储空间实例来调用。
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
key =  'python_select.csv'
content =  'Tom Hanks,USA,45\r\n'*1024
filename =  'python_select.csv'
# 上传文件
bucket.put_object(key, content)
csv_meta_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n'}
select_csv_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n',
'LineRange': (500, 1000)}

csv_header = bucket.create_select_object_meta(key, csv_meta_params)
print(csv_header.csv_rows)
print(csv_header.csv_splits)
result = bucket.select_object(key, "select * from ossobject where _3 > 44 limit 100000", select_call_back, select_csv_params)
content_got =  b''
for chunk in result:
	content_got += chunk
print(content_got)

result = bucket.select_object_to_file(key, filename,
"select * from ossobject where _3 > 44 limit 100000", select_call_back, select_csv_params)

bucket.delete_object(key)

```

## Java SDK 样例 {#section_m3y_dky_b2b .section}

```
package samples;

import com.aliyun.oss.event.ProgressEvent;
import com.aliyun.oss.event.ProgressListener;
import com.aliyun.oss.model.*;
import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;

import java.io.BufferedOutputStream;
import java.io.ByteArrayInputStream;
import java.io.FileOutputStream;

/**
 * Examples of create select object metadata and select object.
 *
 */
public class SelectObjectSample {
    private static String endpoint = "<endpoint, http://oss-cn-hangzhou.aliyuncs.com>";
    private static String accessKeyId = "<accessKeyId>";
    private static String accessKeySecret = "<accessKeySecret>";
    private static String bucketName = "<bucketName>";
    private static String key = "<objectKey>";

    public static void main(String[] args) throws Exception {
        OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret);
        String content = "name,school,company,age\r\n" +
                "Lora Francis,School A,Staples Inc,27\r\n" +
                "Eleanor Little,School B,\"Conectiv, Inc\",43\r\n" +
                "Rosie Hughes,School C,Western Gas Resources Inc,44\r\n" +
                "Lawrence Ross,School D,MetLife Inc.,24";

        client.putObject(bucketName, key, new ByteArrayInputStream(content.getBytes()));

        SelectObjectMetadata selectObjectMetadata = client.createSelectObjectMetadata(
                new CreateSelectObjectMetadataRequest(bucketName, key)
                        .withInputSerialization(
                                new InputSerialization().withCsvInputFormat(
                                        new CSVFormat().withHeaderInfo(CSVFormat.Header.Use).withRecordDelimiter("\r\n"))));
        System.out.println(selectObjectMetadata.getCsvObjectMetadata().getTotalLines());
        System.out.println(selectObjectMetadata.getCsvObjectMetadata().getSplits());

        SelectObjectRequest selectObjectRequest =
                new SelectObjectRequest(bucketName, key)
                        .withInputSerialization(
                                new InputSerialization().withCsvInputFormat(
                                        new CSVFormat().withHeaderInfo(CSVFormat.Header.Use).withRecordDelimiter("\r\n")))
                        .withOutputSerialization(new OutputSerialization().withCsvOutputFormat(new CSVFormat()));
        selectObjectRequest.setExpression("select * from ossobject where _4 > 40");
        OSSObject ossObject = client.selectObject(selectObjectRequest);
        // read object content from ossObject
        BufferedOutputStream outputStream = new BufferedOutputStream(new FileOutputStream("result.data"));
        byte[] buffer = new byte[1024];
        int bytesRead;
        while ((bytesRead = ossObject.getObjectContent().read(buffer)) != -1) {
            outputStream.write(buffer, 0, bytesRead);
        }
        outputStream.close();
    }
}

```

**常见的SQL用例**

|应用场景|SQL语句|
|----|-----|
|返回前10行数据|select \* from ossobject limit 10|
|返回第1列和第3列的整数，并且第1列大于第3列|select \_1, \_3 from ossobject where cast\(\_1 as int\) \> cast\(\_3 as int\)|
|返回第1列以'陈'开头的记录的个数\(注：此处like后的中文需要用UTF-8编码\)|select count\(\*\) from ossobject where \_1 like '陈%'|
|返回所有第2列时间大于2018-08-09 11:30:25且第3列大于200的记录|select \* from ossobject where \_2 \> cast\('2018-08-09 11:30:25' as timestamp\) and \_3 \> 200|
|返回第2列浮点数的平均值，总和，最大值，最小值| select AVG\(cast\(\_2 as double\)\), SUM\(cast\(\_2 as double\)\), MAX\(cast\(\_2 as double\)\), MIN\(cast\(\_2 as double\)\)

 |
|返回第1列和第3列连接的字符串中以'Tom'为开头以’Anderson‘结尾的所有记录|select \* from ossobject where \(\_1 || \_3\) like 'Tom%Anderson'|
|返回第1列能被3整除的所有记录|select \* from ossobject where \(\_1 % 3\) == 0|
|返回第1列大小在1995到2012之间的所有记录|select \* from ossobject where \_1 between 1995 and 2012|
|返回第5列值为N,M,G,L的所有记录|select \* from ossobject where \_5 in \('N', 'M', 'G', 'L'\)|
|返回第2列乘以第3列比第5列大100以上的所有记录|select \* from ossobject where \_2 \* \_3 \> \_5 + 100|

## 支持的时间格式 {#section_vn3_3bf_2fb .section}

您无需指定日期格式，SelectObject可以将以下格式的字符串自动转换成timestamp类型。比如cast­\('20121201' as timestamp\)会被解析成2012年12月1日。

|格式|说明|
|--|--|
|YYYYMMDD|年月日|
|YYYY/MM/DD|年/月/日|
|DD/MM/YYYY/|日/月/年|
|YYYY-MM-DD|年-月-日|
|DD-MM-YY|日-月-年|
|DD.MM.YY|日.月.年|
|HH:MM:SS.mss|小时:分钟:秒.毫秒|
|HH:MM:SS|小时:分钟:秒|
|HH MM SS mss|小时 分钟 秒 毫秒|
|HH.MM.SS.mss|小时.分钟.秒.毫秒|
|HHMM|小时分钟|
|HHMMSSmss|小时分钟秒毫秒|
|YYYYMMDD HH:MM:SS.mss|年月日 小时:分钟:秒.毫秒|
|YYYY/MM/DD HH:MM:SS.mss|年/月/日 小时:分钟:秒.毫秒|
|DD/MM/YYYY HH:MM:SS.mss|日/月/年 小时:分钟:秒.毫秒|
|YYYYMMDD HH:MM:SS|年月日 小时:分钟:秒|
|YYYY/MM/DD HH:MM:SS|年/月/日 小时:分钟:秒|
|DD/MM/YYYY HH:MM:SS|日/月/年 小时:分钟:秒|
|YYYY-MM-DD HH:MM:SS.mss|年-月-日 小时:分钟:秒.毫秒|
|DD-MM-YYYY HH:MM:SS.mss|日-月-年 小时:分钟:秒.毫秒|
|YYYY-MM-DD HH:MM:SS|年-月-日 小时:分钟:秒|
|YYYYMMDDTHH:MM:SS|年月日T小时:分钟:秒|
|YYYYMMDDTHH:MM:SS.mss|年月日T小时:分钟:秒.毫秒|
|DD-MM-YYYYTHH:MM:SS.mss|日-月-年T小时:分钟:秒.毫秒|
|DD-MM-YYYYTHH:MM:SS|日-月-年T小时:分钟:秒|
|YYYYMMDDTHHMM|年月日T小时分钟|
|YYYYMMDDTHHMMSS|年月日T小时分钟秒|
|YYYYMMDDTHHMMSSMSS|年月日T小时分钟秒毫秒|
|ISO8601-0| 年-月-日T小时:分钟+小时:分钟，或者年-月-日T小时:分钟-小时:分钟

 这里+表示时区在标准时间UTC前面，-表示在后面。注意这个格式可用ISO8601-0来表示

 |
|ISO8601-1| 年-月-日T小时:分钟:秒+小时:分钟，或者年-月-日T小时:分钟-小时:分钟

 这里+表示时区在标准时间UTC前面，-表示在后面. 这个格式可用ISO8601-1来表示

 |
|CommonLog|比如28/Feb/2017:12:30:51 +0700|
|RFC822|比如 Tue, 28 Feb 2017 12:30:51 GMT|
|?D/?M/YY|日/月/年 此处日月均可用1位或者2位表示|
|?D/?M/YY ?H:?M|日月年 小时:分钟, 此处日月分钟小时均可用1位或者2位表示|
|?D/?M/YY ?H:?M:?S|日月年 小时:分钟:秒, 此处日月秒分钟小时均可用1位或者2位表示|

由于以下格式会引起歧义，故用户需要指定格式，比如cast\('20121201' as timestamp format 'YYYYDDMM'\)会将20121201解析成2012年1月12日。

|格式|说明|
|--|--|
|YYYYDDMM|年日月|
|YYYY/DD/MM|年日月|
|MM/DD/YYYY|月/日/年|
|YYYY-DD-MM|年-日-月|
|MM-DD-YYYY|月-日-年|
|MM.DD.YYYY|月.日.年|

## 最佳实践 {#section_vk1_hky_b2b .section}

当一个文件很大时，要有效实现分片查询，推荐的流程如下：

1.  调用Create Select Object Meta API获得该文件的总的Split数。理想情况下如果该文件需要用SelectObject，则该API最好在查询前进行异步调用，这样可以节省扫描时间。
2.  根据客户端资源情况选择合适的并发度n，用总的Split数除以并发度n得到每个分片查询应该包含的Split个数。
3.  在请求Body中用诸如split-range=1-20的形式进行分片查询。
4.  如果需要最后可以合并结果。

SelectObject和Normal类型文件配合性能更佳。Multipart 以及Appendable类型的文件由于其内部结构差异导致性能较差。

