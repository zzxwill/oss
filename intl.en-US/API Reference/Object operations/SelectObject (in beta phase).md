# SelectObject \(in beta phase\) {#reference_lz1_r1x_b2b .reference}

## Introduction {#section_jwy_bbx_b2b .section}

Object Storage Service \(OSS\) built on Alibaba Cloud’s Apsara distributed system is a massive, secure, and highly reliable cloud storage solution that offers low cost storage accessible anywhere in the world. OSS possesses excellent scaling abilities for storage capacity and processes, and supports RESTful APIs.  Not only can OSS store media files, but it can also be utilized as a data warehouse for massive data file storage. OSS can seamlessly integrate with Hadoop 3.0, and services that are run on EMR \(such as Spark/Hive/Presto, MaxCompute, HybridDB and the newly-released Data Lake Analytics\) support data processing and retrieval directly from OSS.

However, the current GetObject interface provided by OSS determines that the big data platform can only download all OSS data locally and then for analysis and filter. A lot of bandwidth and client resources are wasted in querying scenarios.

To address this problem, the SelectObject interface is provided. This method allows big data platforms to access OSS to perform basic filtering on data through conditions and Projection, and return useful data only to the big data platform. In this way, the bandwidth and the amount of data processed at the client-side is greatly reduced, making OSS-based data warehousing and data analysis a highly attractive option.

SelectObject is now in beta phase, and provides Java and Python SDKs. SelectObject supports CSV files of RFC 4180 standard to be encoded as UTF-8 \(including Class CSV files such as TSV, row and column separators of the file and customizable Quote characters\). SelectObject supports files in standard and low frequency access storage types, and encrypted files, which are fully managed by OSS \(or CMK managed by KMS\).

The supported SQL syntax is as follows:

-   SQL statements: Select From Where
-   Data Type: String, Int \(64bit\), float \(64bit\), Timestamp, and Boolean
-   Operation: Logical condition \(AND, OR, NOT\), Arithmetic Expression（+-\*/%\), Comparison operation \(\>,=, <, \>=, <=, ! =\), and String operation \(LIKE, || \)

The sharding mechanism of SelectObject is similar to the shard download mechanism of GetObject, and includes two sharding methods: sharding by row and sharding by Split. Sharding by row is a common method, but it results in uneven load balancing of sparse data. Sharding by Split is more efficient than sharding by row as a Split contains multiple rows of data, and the data size of each Split is roughly equal, which enables better load balancing performance. Additionally, byte-based sharding \(provided by GetObject\) may corrupt data. Therefore, sharding by Split is recommended for CSV data.

CSV data in OSS is String type by default. Users can use CAST function to convert data. For example, the following SQL query converts \_1 and \_2 into Int and compares them.

`Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

Furthermore, SelectObject supports implicit conversion in WHERE condition, such as the first and the second columns in the following statement will be converted to Int:

`Select _1 from ossobject where _1 + _2 > 100`

## Description of RESTful API {#section_tp1_zcx_b2b .section}

Execute the SQL statement on the target CSV files and the execution results will be returned. At the same time, the command will automatically save the metadata information of the CSV files, such as the total number of rows and columns.

The API returns a 206 response when the SQL statement is executed correctly. If the SQL statement is incorrect or does not match the CSV files, a 400 error response will be returned.

**Request syntax**

```
POST /object? x-oss-process=csv/select HTTP/1.1 
HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
Date: time GMT
Content-Length: ContentLength
Content-MD5: MD5Value 
Authorization: Signature

<? xml version="1.0" encoding="UTF-8"? >
<SelectRequest>
	Base64 encode (select * From ossobject where)
	<InputSerialization>
		<CompressionType>None</CompressionType>
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
  </OutputSerialization>
</SelectRequest>
```

|Name| Type|Description|
|:---|-----|:----------|
|SelectRequest|Container| The container for storing Select requests

 Child Node: Expression, InputSerialization, OutputSerialization

 Parent node: None

 |
|Expression|String| The SQL statement encoded in Base64

 Child Nodes: None

 Parent node: SelectRequest

 |
|InputSerialization|Container| Input serialized parameters \(optional\)

 Child Node: CompressionType, CSV

 Parent node: SelectRequest

 |
|OutputSerialization|Container| Output serialized parameters \(optional\)

 Child Node: CSV, OutputRawData

 Parent node: SelectRequest

 |
|CSV（InputSerialization）|Container| Input CSV-formatted parameters \(optional\)

 Child Node: FileHeaderInfo, RecordDelimiter, FieldDelimiter, QuoteCharacter, CommentCharacter, Range

 Parent node: InputSerialization

 |
|CSV\(OutputSerialization\)|Container| Output CSV-formatted parameters \(optional\)

 Child Node: RecordDelimiter, FieldDelimiter, KeepAllColumns

 Parent node: OutputSerialization

 |
|OutputRawData|bool，default: false| Specifies output data as raw data, not Frame-based data \(optional\)

 Child Node: None

 Parent node: OutputSerialization

 |
|CompressionType|Enumeration| Specifies file compression types. It can only be None as file compression is currently not supported

 Child Node: None

 Parent node: InputSerialization

 |
|FileHeaderInfo|Enumeration| Specifies CSV files header information \(optional\)

 Value:

-   Use: The CSV file contains header information, and the CSV column name can be used as the column name in the Select.
-   Ignore: The CSV file contains header information, but the CSV column name can not be used as the column name in the Select.
-   None: The CSV file contains no header information, and the value can be default.

 Child Node: None

 Parent node: CSV\(input\)

 |
|RecordDelimiter|String| Specifies line breaks for a CSV, encoded in Base64.  The default value is\\n \(optional\). The value before decoding is at most two characters, expressed as an ANSI character. \\n used in Java indicates a line break.

 Child Node: None

 Parent node: CSV \(input, output\)

 |
|FieldDelimiter|String| Specifies the CSV column separator, encoded in Base64.  The default is ， \(optional\)

 The value before decoding must be expressed as an ANSI character ，used in Java indicates a comma.

 Child Node: None

 Parent node: CSV \(input and output\)

 |
|QuoteCharacter|String| Specifies the quote character of the CSV, encoded in Base64. The default value is \\” \(optional\). Inside the CSV quotes, the column separator is treated as a normal character. The value before encoding must be expressed as an ANSI character, such as \\”in Java indicates quotation marks.

 Child Node: None

 Parent Node: CSV \(input\)

 |
|CommentCharacter|String|Specifies the CSV comment character, encoded in Base464. The default value is \# \(optional\)|
|Range|String| Specifies the scope of the query file \(optional\). Two formats are supported:

-   Query by row: line-range=start-end
-   Query by Split: split-range=start-end

 Both start and end are inclusive. The format is the same as the range parameter in range get.

 Child Node: None

 Parent Node: CSV \(input\)

 |
|KeepAllColumns|bool| Specifies the location in the response result that contains all of the CSV columns \(optional, and default value is false\). However, only the columns in the select statement contain values, otherwise they are empty. The data of each row in the response result will be sorted in ascending order of CSV columns. Take the following statement as example:

 `select _5, _1 from ossobject.`

 If the value of KeepAllColumn is true, with six columns of data in total, the returned data is as follows:

 Value of 1st column,,,,Value of 5th column,\\n

 Child Node: None

 Parent Node: CSV\(output\)

 |

**Response results**

The request results are returned as a Frame. The format of each frame is as follows, where checksum is CRC32:

Frame-Type | Payload Length | Header Checksum | Payload | Payload Checksum

<---4 bytes--\><---4 bytes----------\><-------4 bytes-------\><variable\><----4bytes----------\>

There are three different frame types, which are as follows:

|Name|Frame-Type Value|Payload format|Description|
|:---|:---------------|:-------------|:----------|
|Data Frame| version | 8388609

 <--1 byte\><--3 bytes\>

 | scanned size  |  data

 <-8 bytes----------\><---variable-\>

 The scanned size is the size of the scanned data, and the data is the data returned from the query.

 |Data Frame is used to return the query data and report its current progress at the same time.|
|Continuous Frame| version  | 8388612

 <--1 byte\><--3 bytes-\>

 | scanned size

 <----8 bytes--\>

 |Continuous Frame is used to report current progress and maintain HTTP connections. If the query does not return data within 5s,  a Continuous Frame will be returned. |
|End Frame| version  | 8388613

 | Offset  | total scanned bytes | http status code | error message

 <--8bytes-\><--8bytes--------------\><----4 bytes--------\><-variable------\>

 Where offset is the final location offset after scanning and total scanned bytes is the total bytes of all scanned data. http status code is the final processing result. and error message is the error message itself.

 |The reason it returns the status code is that when the SelectObject is streamed, only the first block is processed when the Response Header is sent. If the first block of data and SQL match, the Status in the Response Header is a 206 response, but if the following data is illegal, the Status in the Header cannot be changed, and the final Status and Error message includes only the End Frame. Therefore, the client should treat it as the final result.|

**Example request**

```
POST /oss-select/bigcsv_normal.csv? x-oss-process=csv%2Fselect HTTP/1.1
Date: Fri, 25 May 2018 22:11:39 GMT
Content-Type:
Authorization: OSS LTAIJPXxMLocA0fD:FC/9JRbBGRw4o2QqdaL246Pxuvk=
User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
Content-Length: 748
Expect: 100-continue
Connection: keep-alive
Host: host name

<? xml version="1.0"? >
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

**SQL statement regex**

`SELECT select-list from OSSObject where_opt limit_opt`

The keywords SELECT, OSSOBJECT and WHERE cannot be changed.

```
select_list: column name

| column index (for example, _1, _2)

| function(column index | column name)

| select_list AS alias
```

The supported functions are AVG, SUM, MAX, MIN, COUNT, and CAST \(type conversion function\). Only \* can be used after COUNT.

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

op: includes \> < \>= <= ! = =, LIKE，+-\*/%, and connection string ||.

cast: Cast can only be one type for the same column.

`limit_opt:`

| limit Integer

Mixing of aggregations and limit

`Select avg(cast(_1 as int)) from ossobject limit 100`

In the preceding statement, the AVG value of the first column in the first 100 rows is calculated. This statement is different from what MYSQL outputs, as aggregation in SelectObject always returns only one row of data, so there is no need to limit its output volumes. Therefore, the limit in SelectObject will be executed before the Aggregate function.

SQL statement restrictions are as follows:

-   Only UTF-8 encoded text files are supported. Uncompressed GZIP text files can be processed. Support for processing compressed files is coming soon.
-   Only single file queries are supported, not JOIN, ORDER BY, GROUP BY, and HAVING
-    Not contains aggregation conditions in WHERE statement. For example, where max\(cast\(age as int\)\) \> 100 is not allowed.
-   Up to 1000 columns are supported and the maximum column name is 1024.
-    Up to 5% wildcards are supported in the LIKE statement. \* and % are equivalent, representing 0 or multiple arbitrary characters.
-    Up to 1024 constant items are supported in the IN statement.
-   The Projection after Select can be a column name, a column index \(\_1, \_2, etc.\), an aggregate function, or a CAST function. Other expressions are not supported Like select \_ 1 + \_2 from ossobject is not allowed.
-   The length of maximum row and column are both 256 KB.

## CreateSelectObjectMeta {#section_pbq_vfy_b2b .section}

CreateSelectObjectMeta API is used to obtain information about the target CSV file, such as the total number of rows, the total number of columns, and the number of Splits. If the information does not exist in the file, the whole CSV file is scanned for the preceding information. If the API executes correctly, a 200 response is returned. If the target CSV file is illegal or the specified delimiter does not match the target CSV file, a 400 error response is returned.

**Request elements**

|Name| Type|Description|
|:---|-----|:----------|
|CsvMetaRequest|Container| Saves the container that created Select Meta requests

 Child Node: Expression, InputSerialization, OutputSerialization

 Parent node: None

 |
|InputSerialization|Container| Inputs serialized parameters \(optional\)

 Child Node: CompressionType, CSV

 Parent Node: CsvMetaRequest

 |
|OverwriteIfExists|bool| Recalculates SelectMeta to overwrite existing data \( optional, the default value is false. If Select Meta already exists, then Select Meta is returned.\)

 Child Node: None

 Parent Node: CsvMetaRequest

 |
|CompressionType|Enumeration| Specifies file compression types. It can only be None as file compression is currently not supported.

 Child Node: None

 Parent node: InputSerialization

 |
|RecordDelimiter|String| Specifies line breaks for a CSV, encoded in Base64.  The default value is \\n \(optional\).  The value before decoding is at most two characters, expressed as an ANSI character. \\n used in Java indicates a line break.

 Child Node: None

 Parent node: CSV

 |
|FieldDelimiter|String| Specifies the CSV column separator, encoded in Base64.  The default value is ， \(optional\).

 The value before decoding must be expressed as an ANSI character. ， used in Java indicates a comma.

 Child Node: None

 Parent Node: CSV \(input and output\)

 |
|QuoteCharacter|String| Specifies the CSV quote character, encoded in Base64. The default value is \\” \(optional\).  Line breaks in quotation marks in CSV, column separators will be treated as normal characters. The value before decoding must be expressed as an ANSI character. \\” used in Java indicates a comma.

 Child Node: None

 Parent Node: CSV \(input\)

 |
|CSV|Container| Specifies CSV input format

 Child Node: RecordDelimiter，FieldDelimiter，QuoteCharacter

 Parent Node: InputSerialization

 |

Response Body: empty

Response Header：

-   x-oss-select-csv-lines: total number of rows
-   x-oss-select-csv-columns: total number of columns
-   x-oss-select-csv-splits: total number of Splits
-   content-length: file content length

**Note:** X-OSS-select-CSV-columns refers to the number of columns in the first row, assuming that the data in the first row  is correct.

**Example request**

```
POST /oss-select/bigcsv_normal.csv? x-oss-process=csv%2Fmeta HTTP/1.1
Date: Fri, 25 May 2018 23:06:41 GMT
Content-Type:
Authorization: OSS LTAIJPXxMLocA0fD:2WF2l6zozf+hzTj9OSXPDklQCvE=
User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
Content-Length: 309
Expect: 100-continue
Connection: keep-alive
Host: Host

<? xml version="1.0"? >
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

**Response code**

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

## Python SDK {#section_hd3_1ky_b2b .section}

```
import os
import oss2

def select_call_back(consumed_bytes, total_bytes = None):
	print('Consumed Bytes:' + str(consumed_bytes) + '\n')
	
# First, initialize the information such as AccessKeyId, AccessKeySecret, and Endpoint.
# Obtain the information through environment variables or replace the information such as“<yourAccessKeyId>” with the real AccessKeyId, and so on.
#
# Use Hangzhou region as an example. Endpoint can be:
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com

access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<yourAccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<yourAccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<yourBucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<yourEndpoint>')

# Create a bucket instance, all object-related methods need to be called through the bucket instance.
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
key = 'python_select.csv'
content = 'Tom Hanks,USA,45\r\n'*1024
filename = 'python_select.csv'
# Upload files
bucket.put_object(key, content)
csv_meta_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n'}
select_csv_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n',
'LineRange': (500, 1000)}

csv_header = bucket.create_select_object_meta(key, csv_meta_params)
print(csv_header.csv_rows)
Print(csv_header.csv _ splits)
result = bucket.select_object(key, "select * from ossobject where _3 > 44 limit 100000", select_call_back, select_csv_params)
content_got = b''
for chunk in result:
	content_got += chunk
print(content_got)

result = bucket.select_object_to_file(key, filename,
"select * from ossobject where _3 > 44 limit 100000", select_call_back, select_csv_params)

bucket.delete_object(key)

```

## Java SDK {#section_m3y_dky_b2b .section}

```
package samples;

import com.aliyun.oss.event.ProgressEvent;
import com.aliyun.oss.event.ProgressListener;
import com.aliyun.oss.model.*;
import com.aliyun.oss.OSS;
Import com. aliyun. OSS;

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
        System. Out. println (selectobjectmetadata. getcsvobjectmetadata (). getshares ());

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
        while ((bytesRead = ossObject.getObjectContent().read(buffer)) ! = -1) {
            outputStream.write(buffer, 0, bytesRead);
        }
        outputStream.close();
    }
}

```

## Best practices {#section_vk1_hky_b2b .section}

If you want to perform Shard-Query on a massive file, we recommend that you:

1.  Call the Create Select Object Meta API to get the total number of Splits for the file. If the file needs to call the SelectObject API, this API should make asynchronous calls before the query, which reduces scan time.
2.  Select the appropriate concurrency n based on client-side resources, and divide the total number of Splits by the concurrency n to get the number of Splits that each shard query should contain.
3.  Perform the Shard-Query in a form of split-range=1-20 in request body.
4.  Merge the results if required.

Use SelectObject with Normal type files. Files of Multipart and Appendable types are not recommended due to poor performance caused by differences in their internal structure.

