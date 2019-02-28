# SelectObject {#concept_ghk_xz2_4gb .concept}

SelectObject is commonly used in log file analysis and can be used together with Alibaba Cloud Bigdata products. This topic describes how to use the Python SDK and Java SDK of SelectObject to achieve the preceding application scenarios.

## Introduction {#section_jwy_bbx_b2b .section}

Object Storage Service \(OSS\) built on Alibaba Cloud’s Apsara distributed system is a massive, secure, and highly reliable cloud storage solution that offers low cost storage accessible anywhere in the world. OSS possesses excellent scaling abilities for storage capacity and processes, and supports RESTful APIs.  Not only can OSS store media files, but it can also be utilized as a data warehouse for massive data file storage. OSS can seamlessly integrate with Hadoop 3.0, and services that are run on EMR \(such as Spark/Hive/Presto, MaxCompute, HybridDB and the newly-released Data Lake Analytics\) support data processing and retrieval directly from OSS.

However, the current GetObject interface provided by OSS determines that the big data platform can only download all OSS data locally and then for analysis and filter. A lot of bandwidth and client resources are wasted in querying scenarios.

To address this problem, the SelectObject interface is provided. This method allows big data platforms to access OSS to perform basic filtering on data through conditions and Projection, and return useful data only to the big data platform. In this way, the bandwidth and the amount of data processed at the client-side is greatly reduced, making OSS-based data warehousing and data analysis a highly attractive option.

SelectObject provides Java and Python SDKs. SDKs in other langugae will be available soon. SelectObject supports JSON files and CSV files that conform to the RFC 4180 standard and are encoded in UTF-8 \(including Class CSV files such as TSV, in which row and column separators and quote characters can be customized\). SelectObject supports objects of the standard and IA storage class access storage types \(objects of the Archive storage class must be restored before use\). SelectObject also supports files encrypted in the following two methods: 1. Use encryption fully managed by OSS. 2. Use CMKs managed by KMS for encryption.

The following two types of JSON files are supported: DOCUMENT and LINES. A JSON file of the DOCUMENT type is a single JSON object. A JSON file is composed of lines of JSON objects separated by delimiters. However, the file itself may not be a valid JSON object. SelectObject supports common delimiters, such as "\\n" and "\\r\\n" so that you do not need to specify the delimiters.

-   Supported SQL syntax:
    -   SQL statements: Select, From, and Where
    -   Data types: string, int \(64bit\), double \(64bit\), decimal \(128\), timestamp, and bool
    -   Operations: logical conditions \(AND, OR, and NOT\), arithmetic expressions \(+, -, \*, /, and %\), comparison operations \(\>, =, <, \>=, <=, and !=\), string operations \(LIKE, and ||\)
-   Multipart query

    SelectObject provides the multipart query mechanism that is similar to the multipart download mechanism of GetObject. Data are divided into parts by rows or splits. Dividing data by rows are commonly used but may result in unbalanced workloads when dividing sparse data. Dividing data by splits is more efficient because the size of each split \(which includes multiple rows\) is roughly the same, which enables better load balancing performance.

-   Data type

    The type of CSV data is string in OSS by default. You can use the CAST function to convert the data type. For example, the following SQL query statement converts \_1 and \_2 into int and compares them.

    `Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

    Furthermore, SelectObject supports implicit type conversion in the WHERE condition. For example, the first and the second columns in the following statement are converted to int:

    `Select _1 from ossobject where _1 + _2 > 100`

    If the CAST function is not specified in the SQL statement, the type of a JSON file is determined by the data type in the file. A standard JSON file can include the following built-in data types: null, bool, int64, double, and string.


## Python SDK example {#section_usn_41f_4gb .section}

```
import os
import oss2

def  select_call_back(consumed_bytes, total_bytes =  None):
	print('Consumed Bytes:'  +  str(consumed_bytes) +  '\n')
	
# Initializes OSS information, such as AccessKeyId, AccessKeySecret, and Endpoint.
# Obtains environment variables or replace variables such as <yourAccessKeyId> with actual values.
#
# This example uses the China East 1 (Hangzhou) region, which has the following endpoints:
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com

access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<yourAccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<yourAccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<yourBucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<yourEndpoint>')

# Creates an OSS bucket. All object-related methods must be called by the bucket.
bucket = oss2.Bucket(oss2.Auth(access_key_id, access_key_secret), endpoint, bucket_name)
key =  'python_select.csv'
content =  'Tom Hanks,USA,45\r\n'*1024
filename =  'python_select.csv'
# Uploads a CSV file.
bucket.put_object(key, content)
# Configure the parameters of SelectObject.
csv_meta_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n'}
select_csv_params = {'CsvHeaderInfo': 'None',
'RecordDelimiter': '\r\n',
'LineRange': (500, 1000)}

csv_header = bucket.create_select_object_meta(key, csv_meta_params)
print(csv_header.rows)
print(csv_header.splits)
result = bucket.select_object(key, "select * from ossobject where _3 > 44", select_call_back, select_csv_params)
select_content = result.read()
print(select_content)

result = bucket.select_object_to_file(key, filename,
"select * from ossobject where _3 > 44", select_call_back, select_csv_params)
bucket.delete_object(key)

###JSON DOCUMENT
key =  'python_select.json'
content =  "{\"contacts\":[{\"key1\":1,\"key2\":\"hello world1\"},{\"key1\":2,\"key2\":\"hello world2\"}]}"
filename =  'python_select.json'
# Upload a JSON DOCUMENT file
bucket.put_object(key, content)
select_json_params = {'Json_Type': 'DOCUMENT'}
result = bucket.select_object(key, "select s.key2 from ossobject.contacts[*] s where s.key1 = 1", None, select_json_params)
select_content = result.read()
print(select_content)

result = bucket.select_object_to_file(key, filename,
"select s.key2 from ossobject.contacts[*] s where s.key1 = 1", None, select_json_params)

bucket.delete_object(key)
###JSON LINES
key =  'python_select_lines.json'
content =  "{\"key1\":1,\"key2\":\"hello world1\"}\n{\"key1\":2,\"key2\":\"hello world2\"}"
filename =  'python_select.json'
# Uploads a JSON LINE file.
bucket.put_object(key, content)
select_json_params = {'Json_Type': 'LINES'}
json_header = bucket.create_select_object_meta(key,select_json_params)
print(json_header.rows)
print(json_header.splits)

result = bucket.select_object(key, "select s.key2 from ossobject s where s.key1 = 1", None, select_json_params)
select_content =  result.read()
print(select_content)
result = bucket.select_object_to_file(key, filename,
"select s.key2 from ossobject s where s.key1 = 1", None, select_json_params)

bucket.delete_object(key)
```

SelectObject API in Python

-   select\_object 
    -   select\_object example:

        ```
        def select_object(self, key, sql,
                           progress_callback=None,
                           select_params=None                   ):
        ```

        The preceding example code executes the SQL statement on the target CSV files and returns the query results.

        -   The SQL statements can be directly used as the SQL parameter and does not need to be base64-encoded.
        -   The progress\_callback parameter indicates a callback function used to report the query progress, which is optional.
        -   The select\_params parameters specifies the parameters and actions of the SelectObject operation.
        -   Headers can be used to specify the header information included in the request, which act the same role as they do in the GetObject operation. For examples, you can set the bytes header to specify the query range.
    -   Parameters supported by select\_params

        |Parameter|Description|
        |---------|-----------|
        |Json\_Type|         -   Not specified: The file is a CSV file by default.
        -   DOCUMENT: The file is a JSON file.
        -   LINES: The file is a JSON LINE file.
 |
        |CsvHeaderInfo|Specifies the header information about the CSV file. Valid values: None, Ignore, Use

        -   None: This file does not include header information.
        -   Ignore: This file includes header information but is not used in the SQL statement.
        -   Use: This file includes header information. The column nmaes are used in the SQL statement.
|
        |CommentCharacter|Specifies the comment character in the CSV file. A comment character can be only one character. The value of this parameter is None, that is, no comment character is specified.|
        |RecordDelimiter|Specifies the delimiter used to separate rows in the CSV file. The value of this parameter can be two characters in maximum and is \\n by default.|
        |OutputRecordDelimiter|Specifies the line breaks in the output CSV file. The default value of this parameter is \\n.|
        |FieldDelimiter|Specifies the delimiter used to separate columns in the CSV file. The value of this parameter can be only one character and is , by default.|
        |OutputFieldDelimiter|Specifies the delimiter used to separate columns in the output CSV file. The value of this parameter is , by default.|
        |QuoteCharacter|Specifies the quote characters used in the CSV file. The value of this parameter can be only one character and is double quotation mark by default. Delimiters enclosed in the quotation marks are processed as normal characters.|
        |SplitRange|Specifies the split range in multipart queries. The value of this parameter is in the \(start, end\) format. The range includes the start and end values, indicating that splits from the `start` to the `end` are queried.|
        |LineRange|Specifies the row range in multipart queries. The value of this parameter is in the \(start, end\) format. The range includes the start and end values, indicating that rows from the `start` to the `end` are queried.|
        |CompressionType|Specifies the compression type. The default value of this parameter is None. You can set the value to GZIP.|
        |KeepAllColumns|Indicates that all columns in the CSV file are included in the returned result. However, only columns included in the select statement have values. The default value of this parameter is false.For example:

A CSV file includes the following columns: firstname, lastname, age. And the SQL statement is `select firstname, age from ossobject`. If the value of KeepAllColumns is true, the output result is `firstname,,age`, in which a comma is added to indicate the lastname column that is not specified in the statement. If the value of KeepAllColumn is false, the output result is `firstname,age`. This paremeter is used to allow the code that processes the data returned by GetObject to process the data returned by SelectObject without being modified.

|
        |OutputRawData|         -   If the value of this parameter is true, the data returned by the SelectObject is output without being encapsulated by frames. However, if data is not returned for a long period, a timeout error may occur.
        -   If the value of this parameter is false, the output data is encapsulated by frames. The default value of this parameter is false.
 |
        |EnablePayloadCrc|Indicates that CRC values are calculated for each frame. The default value of this parameter is false.|
        |OutputHeader|Specifies that the first row in the output result is the header information. This parameter only applies to CSV files.|
        |SkipPartialDataRecord|         -   If the value of this parameter is true, the current record is skipped when the value of a column is missed on a CSV file or a key in the JSON file does not exist.
        -   If the value of this parameter is false, the value of a column without value is processed as null.
 For example:

 If a row includes the following columns: firstname, lastname, and age, and the SQL statement is`select _1, _4 from ossobject`. If the value of this parameter is true, this row is skipped. If the value of this parameter is false, the following result is returned: firstname,\\n.

 |
        |MaxSkippedRecordsAllowed|Specifies the maximum number of skipped rows. The default value of this parameter is 0, indicating that an error is returned if a row is skipped.|
        |ParseJsonNumberAsString|Numbers in the JSON file are resolved as strings if the value of this parameter is true and are resolved as integers or floats if the value is false.The accuracy of float numbers in a JSON file degrades when the numbers are resolved as float. To keep the accuracy, you can set the value of this parameter to true and convert the type of the column into decimal.

|

    -   Returned result of select\_object: A SelectObjectResult object is returned. You can use the read\(\) function or the \_\_iter\_\_\_ method to obtain all results. If the size of the result is large, the read\(\) function is not the optimal method to obtain the result because this function blocks until all results are returned and use excessive memory resources.

        We recommend that you use the \_\_iter\_\_ method to obtain results and process each chunk. This method uses less memory resources and allows you to process each chunk immediately after it is processed by the OSS server instead of waiting for all the results to be returned.

-   select\_object\_to\_file

    ```
    def select_object_to_file(self, key, filename, sql,
                       progress_callback=None,
                       select_params=None
                       ):
    ```

    The preceding code executes the SQL statement on a file that includes the specified key and writes the query result into the specified file.

    Other paraemters are the same as those of [select\_object](#ul_unq_lgf_4gb).

-   create\_select\_object\_meta
    -   select\_meta\_params

        ```
        def create_select_object_meta(self, key, select_meta_params=None):
        ```

        The preceding code create select meta for a file that includes the specified key or obtain select meta from a file that includes the specified key. Select meta includes the following information about the file: total number of rows, total number of columns \(for CSV files\), and total number of splits.

        If metadata has been created for the file, this function does not re-create the metadata unless the value of OverwriteIfExists is set to true.

        To create select meta for a file, the file must be completely scanned

    -   Parameters supported by select\_meta\_params

        |Parameter|Description|
        |---------|-----------|
        |Json\_Type|         -   Not specified: The file is a CSV file.
        -   LINES: The file is a JSON LINES file.
        -   DOCUMENTS: Not supported.
 |
        |RecordDelimiter|Specifies the delimiter used to separate rows in CSV files.|
        |FieldDelimiter|Specifies the delimiter used to separate columns in CSV files.|
        |QuoteCharacter|Specifies the quote characters used in CSV files. Delimiters enclosed in the quotation marks are processed as normal characters.|
        |CompressionType|Specifies the compression typo. Only None is supported.|
        |OverwriteIfExists|Indicates that the operation creates new select meta for the file and overwrites the original select meta. This parameter is unnecessary in common scenarios.|

    -   Returned result of create\_select\_object\_meta: A GetSelectObjectMetaResult object is returned, which includes two attributes: rows and splits. The select\_resp object in the result includes the columns attribute that indicates the number of columns in the CSV file.

## Java SDK sample {#section_xlv_t1f_4gb .section}

```
package samples;

import com.aliyun.oss.ClientBuilderConfiguration;
import com.aliyun.oss.model.*;
import com.aliyun.oss.OSS;
import com.aliyun.oss.OSSClientBuilder;


import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;
import java.util.concurrent.Future;

import com.aliyun.oss.common.auth.*;
import com.aliyuncs.DefaultAcsClient;
import com.aliyuncs.exceptions.ClientException;
import com.aliyuncs.http.MethodType;
import com.aliyuncs.http.ProtocolType;
import com.aliyuncs.profile.DefaultProfile;
import com.aliyuncs.profile.IClientProfile;
import com.aliyuncs.sts.model.v20150401.AssumeRoleRequest;
import com.aliyuncs.sts.model.v20150401.AssumeRoleResponse;

import java.text.SimpleDateFormat;
import java.util.Calendar;

/**
 * Examples of create select object metadata and select object.
 *
 */
class MulipartSelector implements Callable<Integer> {

    private  OSS client;
    private  String bucket;
    private  String key;
    private  int start;
    private  int end;
    private  String sql;

    public MulipartSelector(OSS client, String bucket, String key, int start, int end, String sql){
        this.client = client;
        this.bucket = bucket;
        this.key = key;
        this.start = start;
        this.end = end;
        this.sql = sql;
    }

    @Override
    public Integer call() throws Exception {
        SelectObjectRequest selectObjectRequest =
                new SelectObjectRequest(bucket, key)
                        .withInputSerialization(
                                new InputSerialization().withCsvInputFormat(
                                        new CSVFormat().withHeaderInfo(CSVFormat.Header.None).withRecordDelimiter("\n")
                                                .withFieldDelimiter("|")))
                        .withSplitRange(start, end)
                        .withOutputSerialization(new OutputSerialization().withCsvOutputFormat(new CSVFormat()).withCrcEnabled(true));

        selectObjectRequest.setExpression(sql);
        OSSObject ossObject = client.selectObject(selectObjectRequest);
        byte[] buffer = new byte[4096];
        int bytesRead;
        int totalSize = 0;
        try {
            while ((bytesRead = ossObject.getObjectContent().read(buffer)) != -1) {
                totalSize += bytesRead;
            }
            String result = new String(buffer, 0, totalSize - 1);
            return new Integer(Integer.parseInt(result));
        }
        catch (IOException e){
            System.out.println(e.toString());
            return new Integer(0);
        }
    }
}
class RoleCredentialProvider {
    public static final String REGION_CN_HANGZHOU = "cn-hangzhou";
    // Current STS API version
    public static final String STS_API_VERSION = "2015-04-01";
    public static final String serviceAccessKeyId = "<access Key Id that can do assume role>";
    public static final String serviceAccessKeySecret = "<access key secret>";

    public static final long DurationSeconds = 15 * 60;

    private Credentials credential;
    private Calendar expireTime;

    private String roleArn;
    private DefaultAcsClient client;

    public RoleCredentialProvider(String roleArn) throws InvalidCredentialsException {
        this.roleArn = roleArn;
    }

    private AssumeRoleResponse assumeRole (String accessKeyId, String accessKeySecret, String roleArn, String roleSessionName, String policy, ProtocolType protocolType, long durationSeconds) throws ClientException {
        try {
            // create Aliyun Acs Client for sending OpenAPI requests
            if (this.client == null) {
                IClientProfile profile = DefaultProfile.getProfile(REGION_CN_HANGZHOU, accessKeyId, accessKeySecret);
                this.client = new DefaultAcsClient(profile);
            }
            // create an instance of AssumeRoleRequest and set properties
            final AssumeRoleRequest request = new AssumeRoleRequest();
            request.setVersion(STS_API_VERSION);
            request.setMethod(MethodType.POST);
            request.setProtocol(protocolType);
            request.setRoleArn(roleArn);
            request.setRoleSessionName(roleSessionName);
            request.setPolicy(policy);
            request.setDurationSeconds(durationSeconds);
            // send request and get the response
            final AssumeRoleResponse response = client.getAcsResponse(request);
            return response;
        } catch (ClientException e) {
            throw e;
        }
    }


    public CredentialsProvider GetCredentialProvider()
            throws IOException {
        // Parameters in AssumeRole requests: RoleArn, RoleSessionName, Policy, and DurationSeconds
        // RoleArn can be obtained on the RAM console.
        // RoleSessionName is the name of the session for the temporary token. It is used to identify users in audit or identify users that you want to issue tokens to.
        // RoleSessionName can only include letters, numbers,'-' and  '_' and cannot include spaces.
        // For more information about the format, see the API documentation.
        SimpleDateFormat timeFormat = new SimpleDateFormat("yyyy-MM-dd");
        String roleSessionName = "AssumingRole" + timeFormat.format(Calendar.getInstance().getTime());
        // read OSS data
        String policy = "{\n" +
                "    \"Version\": \"1\", \n" +
                "    \"Statement\": [\n" +
                "        {\n" +
                "            \"Action\": \"oss:*\", \n" +
                "            \"Resource\": [\n" +
                "                \"acs:oss:*:*:*\"\n" +
                "            ], \n" +
                "            \"Effect\": \"Allow\"\n" +
                "        }\n" +
                "    ]\n" +
                "}";
        // The protocal type must be set to HTTPS.
        ProtocolType protocolType = ProtocolType.HTTPS;
        try {
            final AssumeRoleResponse response = assumeRole(serviceAccessKeyId, serviceAccessKeySecret,
                    roleArn, roleSessionName, policy, protocolType, DurationSeconds);
            String ossAccessId = response.getCredentials().getAccessKeyId();
            String ossAccessKey = response.getCredentials().getAccessKeySecret();
            String ossSts = response.getCredentials().getSecurityToken();

            return new DefaultCredentialProvider(ossAccessId, ossAccessKey, ossSts);

        } catch (ClientException e) {
            throw new InvalidCredentialsException("Unable tp get the temporary AK:" + e);
        }
    }

    public void setClient(DefaultAcsClient client) {
        this.client = client;
    }

    public void setCredentials(Credentials creds) {
        this.credential = creds;
    }

    public Credentials getCredentials() {
        if (credential != null && expireTime.after(Calendar.getInstance())) {
            return credential;
        }

        try {
            CredentialsProvider provider = GetCredentialProvider();
            credential = provider.getCredentials();
            expireTime = Calendar.getInstance();
            expireTime.add(Calendar.SECOND, (int) DurationSeconds - 60);
            return credential;
        } catch (IOException e) {
            throw new InvalidCredentialsException("Unable tp get the temporary AK:" + e);
        }
    }
}
public class SelectObjectSample {
    private static String endpoint = "<oss endpoint>";
private static String bucketName = "<bucket>”;
private static String key = "<object>";
private static String roleArn = "<service role’s ARN>";// You can obtain the ARN of a RAM use on the RAM console. The RAM user must have the permission to access OSS.
private static String recordDelimiter = "\n";
private static int threadCount = 10;

    public static void main(String[] args) throws Exception {
        ClientBuilderConfiguration config = new ClientBuilderConfiguration();
        RoleCredentialProvider provider = new RoleCredentialProvider(roleArn);
        Credentials credentials = provider.getCredentials();
        //OSS client = new OSSClientBuilder().build(endpoint, accessKeyId, accessKeySecret, config);
        System.out.println("Id " + credentials.getAccessKeyId());
        System.out.println("Key " + credentials.getSecretAccessKey());
        System.out.println("Token " + credentials.getSecurityToken());
        OSS client = new OSSClientBuilder().build(endpoint, credentials.getAccessKeyId(), credentials.getSecretAccessKey(), credentials.getSecurityToken(), config);
        int totalSplits = 1;
        try {
            SelectObjectMetadata selectObjectMetadata = client.createSelectObjectMetadata(
                    new CreateSelectObjectMetadataRequest(bucketName, key)
                            .withInputSerialization(
                                    new InputSerialization().withCsvInputFormat(
                                            new CSVFormat().withHeaderInfo(CSVFormat.Header.None).withRecordDelimiter(recordDelimiter))));

            totalSplits = selectObjectMetadata.getCsvObjectMetadata().getSplits();
            System.out.println(selectObjectMetadata.getCsvObjectMetadata().getTotalLines());
            System.out.println(totalSplits);

        }
        catch (Exception e)
        {
		e.printStackTrace();
	 }

        String sql = "select count(*) from ossobject";

        ExecutorService executor = Executors.newFixedThreadPool(threadCount);
        long startTime = System.currentTimeMillis();
        List<Future<Integer>> list = new ArrayList<Future<Integer>>();
        int n = threadCount < totalSplits ? threadCount: totalSplits;
        for(int i = 0; i < n; i++) {
            int start = i * totalSplits/n;
            int end = i == n-1 ? totalSplits - 1 : (i+1)* totalSplits /n - 1;
            System.out.println("start:" + start + " end:" + end);
            Callable<Integer> task = new MulipartSelector(client, bucketName, key, start, end, sql);
            Future<Integer> future = executor.submit(task);
            list.add(future);
        }

        long totalLines = 0;
        for(Future<Integer> task : list){
            totalLines += task.get().longValue();
        }
        long endTime = System.currentTimeMillis();
        System.out.println("total lines:" + totalLines);
        System.out.printf("Total time %dms\n" , (endTime - startTime));

    }
}
```

## SQL statement examples {#section_c4k_hrs_ngb .section}

-   SQL statement examples \(for CSV files\)

    |Scenario|SQL statement|
    |:-------|:------------|
    |Return the first 10 rows.|select \* from ossobject limit 10|
    |Return all integers in column 1 and column 3 in which the integers in column 1 are larger than those in column 3.|select \_1, \_3 from ossobject where cast\(\_1 as int\) \> cast\(\_3 as int\)|
    |Return the number of records in which the first column starts with 'John'|select count\(\*\) from ossobject where \_1 like 'John%'|
    |Return all records in which the time in column 2 is later than 2018-08-09 11:30:25 and the value in column 3 is larger than 200.|select \* from ossobject where \_2 \> cast\('2018-08-09 11:30:25' as timestamp\) and \_3 \> 200|
    |Return the average value, sum, maximum value, and minimum value of the floats in column 2.| select AVG\(cast\(\_2 as double\)\), SUM\(cast\(\_2 as double\)\), MAX\(cast\(\_2 as double\)\), MIN\(cast\(\_2 as double\)\)

 |
    |Return all records in which the strings concatenated by the values in column 1 and column 3 starts with 'Tom' and ends with 'Anderson'.|select \* from ossobject where \(\_1 || \_3\) like 'Tom%Anderson'|
    |Return all records in which the values in column 1 are divisible by 3.|select \* from ossobject where \(\_1 % 3\) == 0|
    |Return all records in which the values in column 1 is in the range from 1995 to 2012.|select \* from ossobject where \_1 between 1995 and 2012|
    |Return all records in which the values in column 5 is N, M, G, or L.|select \* from ossobject where \_5 in \('N', 'M', 'G', 'L'\)|
    |Return all records in which the product of the values in column 2 and column 3 is larger than the sum of 100 and the value in column 5.|select \* from ossobject where \_2 \* \_3 \> \_5 + 100|

-   SQL statement examples \(for JSON files\)

    Assuming that we have the following JSON file:

    ```
    {
      "contacts":[
    {
      "firstName": "John",
      "lastName": "Smith",
      "isAlive": true,
      "age": 27,
      "address": {
        "streetAddress": "21 2nd Street",
        "city": "New York",
        "state": "NY",
        "postalCode": "10021-3100"
      },
      "phoneNumbers": [
        {
          "type": "home",
          "number": "212 555-1234"
        },
        {
          "type": "office",
          "number": "646 555-4567"
        },
        {
          "type": "mobile",
          "number": "123 456-7890"
        }
      ],
      "children": [],
      "spouse": null
    }，…… # Similar nodes are omitted.
    ]}
    ```

    The following table describes SQL statement examples for the preceding JSON file.

    |Scenario|SQL statement|
    |--------|-------------|
    |Return all records in which the value of age is 27.| select \* from ossobject.contacts\[\*\] s where s.age = 27

 |
    |Return all home phone numbers.| select s.number from ossobject.contacts\[\*\].phoneNumbers\[\*\] s where s.type = “home”

 |
    |Return all records in which the value of spouse is null.| select \* from ossobject s where s.spouse is null

 |
    |Return all records in which the value of children is null.|select \* from ossobject s where s.children\[0\] is null**说明：** The preceding statement is used because an empty arry cannot be specified in other ways.

|


## Best practice {#section_vk1_hky_b2b .section}

-   Multipart query for large files

    If columns in a CSV file do not include row delimiters, it is the most simple way to divide the file into parts based on bytes because you do not need to create metadata for the file. If columns in a CSV file includes row delimiters or a JSON file is queried, follow these step:

    1.  Call CreateSelectObjectMeta to get the total number of splits for the file. If you need to call SelectObject for the file, call it asynchronously before the query, which reduces the scan time.
    2.  Select the appropriate concurrency n based on client-side resources, and divide the total number of splits by the concurrency n to get the number of splits that each query needs to contain.
    3.  Perform multipart queries by adding parameters, such as `split-range=1-20`, in the request body.
    4.  Merge the results if required.
-   Use SelectObject for objects of the normal type. We recommend that you do not use SelectObject to query objects of the multipart and appendable types because of their poor performance caused by differences in their internal structure.
-   Narrow the json path in the From statement used to query a JSON file.

    For example:

    Assuming that the following JSON file is queried:

    ```
    { contacts:[
            {“firstName”:”John”, “lastName”:”Smith”, “phoneNumbers”:[{“type”:”home”, “number”:”212-555-1234”}, {“type”:”office”, “number”:”646-555-4567”}, {“type”:”mobile”, “number”:”123 456-7890”}], ”address”:{“streetAddress”: “21 2nd Street”, “city”:”New York”, “state”:NY, “postalCode”:”10021-3100”}
             }
    ]}
    ```

    You can use the following SQL statemtn to query all streetAddress in which the postalCode starts with 10021.

    `select s.address.streetAddress from ossobject.contacts[*] s where s.address.postalCode like ‘10021%’`

    You can also use the following SQL statement:

    `select s.streetAddress from ossobject.contacts[*].address s where s.postalCode like ‘10021%’`

    The performance of the second statement is better because the json path in the statement is more accurate.

-   Process high-accuracy float points in JSON files.

    When you need to calculate high-accuracy float points in a JSON file, we recommend you set the value of ParseJsonNumberAsString to true, and use the CAST function to convert the type of float points to Decimal. For example, you can use the following statement to query for an attribute a whose value is 123456789.123456789: `select s.a from ossobject s where cast(s.a as decimal) > 123456789.12345`.


