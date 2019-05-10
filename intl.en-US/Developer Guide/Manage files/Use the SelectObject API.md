# Use the SelectObject API {#concept_ghk_xz2_4gb .concept}

The SelectObject API is commonly used to analyze logs. It can also be used together with big data products. This topic describes how to use the Python SDK and Java SDK to call the SelectObject API in the preceding scenarios.

## Introduction {#section_jwy_bbx_b2b .section}

Object Storage Service \(OSS\) is a secure and highly reliable cloud storage service based on the Apsara system. It allows the general-purpose storage of large amounts of data on the Internet at a low cost. It also supports RESTful APIs and the auto scaling of capacity and processing capability. OSS can not only store a large number of media objects, but also serve as a data warehouse to store a large number of data objects. Currently, Hadoop 3.0 has supported OSS. You can directly read and process data in OSS when you run services such as Spark, Hive, and Presto in Amazon Elastic MapReduce \(EMR\) or use Alibaba Cloud services such as MaxCompute, HybridDB, and newly published Data Lake Analytics.

However, the current GetObject API provided by OSS only allows the big data platform to download all OSS data locally for analysis and filtering. As a result, a large number of bandwidth resources and client resources are wasted in many query scenarios.

To solve this problem, OSS provides the SelectObject API. It allows OSS, but not the big data platform, to preliminarily filter data by using conditions and projections and only return useful data to the big data platform. In this way, the client can consume fewer bandwidth resources and process less data to maximize the use of CPU and memory resources. This makes OSS-based data warehousing and data analysis a highly attractive option.

The SelectObject API is currently supported by Java and Python SDKs, and will soon be supported by SDKs in other languages. The SelectObject API supports CSV objects and JSON objects that are UTF-8 encoded. The CSV objects conform to RFC 4180, including CSV-like objects such as TSV objects. In the CSV objects, row and column delimiters and quote characters can be customized. The SelectObject API supports objects of the Standard and infrequent access \(IA\) storage classes. \(Objects of the Archive storage class must be restored before use.\) The SelectObject API also supports objects encrypted by using server-side encryption fully managed by OSS \(SSE-OSS\) or server-side encryption that uses customer master keys \(CMKs\) managed by Key Management Service \(KMS\) for encryption \(SSE-KMS\).

The SelectObject API supports JSON objects in DOCUMENT and LINES formats. A JSON DOCUMENT object contains a single object. A JSON LINES object is composed of lines of objects separated by delimiters. However, the JSON object itself may not be a valid object. The SelectObject API supports typical delimiters such as \\n and \\r\\n. You do not need to specify the delimiters.

-   Supported SQL syntax
    -   SQL statements: SELECT, FROM, and WHERE
    -   Data types: string, int64, double64, decimal128, timestamp, and Boolean
    -   Operations: logical conditions \(AND, OR, and NOT\), arithmetic expressions \(+, -, ×, /, and %\), comparison operations \(\>, =, <, \>=, <=, and !=\), and string operations \(LIKE and ||\)
-   Multipart query

    The SelectObject API supports multipart query that is similar to byte-based multipart download supported by the GetObject API. Data is divided into parts by row or split. Dividing data by row is commonly used but may result in unbalanced load when sparse data is divided. Dividing data by split is more efficient because the size of each split, which includes multiple rows, is roughly the same.

-   Data type

    In OSS, data in CSV objects is of the string type by default. You can use the CAST function to convert the data type. For example, the following SQL statement converts the data in the first and second columns into the integer type and compares them.

    `Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

    In addition, the SelectObject API allows you to implicitly convert the data type in a WHERE clause. For example, the following SQL statement converts the data in the first and second columns into the integer type.

    `Select _1 from ossobject where _1 + _2 > 100`

    If you do not use the CAST function in an SQL statement, the data type of a JSON object is determined by the data type in the object. A standard JSON object can support data types such as null, Boolean, int64, double, and string.


## Python SDK {#section_usn_41f_4gb .section}

```
import os
import oss2

def select_call_back(consumed_bytes, total_bytes = None):
	print('Consumed Bytes:' + str(consumed_bytes) + '\n')
	
# Initialize OSS information such as the AccessKey ID, AccessKey Secret, and endpoint.
# Obtain the information through environment variables or replace variables such as <yourAccessKeyId> with actual values.
#
# Use China (Hangzhou) as an example to set the endpoint to one of the following:
# http://oss-cn-hangzhou.aliyuncs.com
# https://oss-cn-hangzhou.aliyuncs.com

access_key_id = os.getenv('OSS_TEST_ACCESS_KEY_ID', '<yourAccessKeyId>')
access_key_secret = os.getenv('OSS_TEST_ACCESS_KEY_SECRET', '<yourAccessKeySecret>')
bucket_name = os.getenv('OSS_TEST_BUCKET', '<yourBucket>')
endpoint = os.getenv('OSS_TEST_ENDPOINT', '<yourEndpoint>')

# Create an OSS bucket. All object-related methods must be called through the bucket.
bucket = oss2. Bucket(oss2. Auth(access_key_id, access_key_secret), endpoint, bucket_name)
key = 'python_select.csv'
content = 'Tom Hanks,USA,45\r\n'*1024
filename = 'python_select.csv'
# Upload a CSV object.
bucket.put_object(key, content)
# Set the parameters of the SelectObject API.
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
key = 'python_select.json'
content = "{\"contacts\":[{\"key1\":1,\"key2\":\"hello world1\"},{\"key1\":2,\"key2\":\"hello world2\"}]}"
filename = 'python_select.json'
# Upload a JSON DOCUMENT object.
bucket.put_object(key, content)
select_json_params = {'Json_Type': 'DOCUMENT'}
result = bucket.select_object(key, "select s.key2 from ossobject.contacts[*] s where s.key1 = 1", None, select_json_params)
select_content = result.read()
print(select_content)

result = bucket.select_object_to_file(key, filename,
"select s.key2 from ossobject.contacts[*] s where s.key1 = 1", None, select_json_params)

bucket.delete_object(key)
###JSON LINES
key = 'python_select_lines.json'
content = "{\"key1\":1,\"key2\":\"hello world1\"}\n{\"key1\":2,\"key2\":\"hello world2\"}"
filename = 'python_select.json'
# Upload a JSON LINES object.
bucket.put_object(key, content)
select_json_params = {'Json_Type': 'LINES'}
json_header = bucket.create_select_object_meta(key,select_json_params)
print(json_header.rows)
print(json_header.splits)

result = bucket.select_object(key, "select s.key2 from ossobject s where s.key1 = 1", None, select_json_params)
select_content = result.read()
print(select_content)
result = bucket.select_object_to_file(key, filename,
"select s.key2 from ossobject s where s.key1 = 1", None, select_json_params)

bucket.delete_object(key)
```

SelectObject API in Python

-   select\_object
    -   The following example shows the sample code of the select\_object operation.

        ```
        def select_object(self, key, sql,
                           progress_callback=None,
                           select_params=None                   ):
        ```

        The preceding sample code is used to run an SQL statement on the object with the specified key and return query results.

        -   The SQL statement can be directly used as the value of the sql parameter and does not need to be Base64-encoded.
        -   The progress\_callback parameter is optional. It specifies a callback function used to report the query progress.
        -   The select\_params parameter is important for the API. It specifies the parameters and actions of the select\_object operation.
        -   You can use the headers parameter to specify the header information included in the request. The header information functions the same as that for the GetObject API. For example, you can set the bytes field in the request header to specify the range that an SQL statement can query in a CSV object.
    -   The following table describes the parameters supported by the select\_params parameter.

        |Name|Description|
        |----|-----------|
        |Json\_Type|         -   If this parameter is left empty, the object is a CSV object by default.
        -   If this parameter is set to DOCUMENT, the object is a JSON object.
        -   If this parameter is set to LINES, the object is a JSON LINES object.
 |
        |CsvHeaderInfo|The header information of the CSV object. Valid values: None, Ignore, and Use.        -   None: indicates that this object does not include header information.
        -   Ignore: indicates that this object includes header information, which is not used in the SQL statement.
        -   Use: indicates that this object includes header information, and the column names in the header information are used in the SQL statement.
|
        |CommentCharacter|The comment character in the CSV object. The parameter value can be only one character. Default value: None, indicating no comment character.|
        |RecordDelimiter|The row delimiter in the CSV object. The parameter value can be only one or two characters. Default value: \\n.|
        |OutputRecordDelimiter|The row delimiter in the output result of the select\_object operation. Default value: \\n.|
        |FieldDelimiter|The column delimiter in the CSV object. The parameter value can be only one character. Default value: comma \(,\).|
        |OutputFieldDelimiter|The column delimiter in the output result of the select\_object operation. Default value: comma \(,\).|
        |QuoteCharacter|The quote character for the columns in the CSV object. The parameter value can be only one character. Default value: double quotation marks \("\). Row and column delimiters enclosed in quotation marks are processed as normal characters.|
        |SplitRange|The split range in multipart queries. The parameter value is a closed interval in \(start, end\) format, indicating that splits from start\# to end\# are queried.|
        |LineRange|The row range in multipart queries. The parameter value is a closed interval in \(start, end\) format, indicating that rows from start\# to end\# are queried.|
        |CompressionType|The compression type. Default value: None. Valid value: GZIP.|
        |KeepAllColumns|If this parameter is set to true, columns that are excluded by the SELECT statement in the CSV object are left empty in the output result. However, the column positions are kept. Default value: false.Example:

A CSV object includes the columns firstname, lastname, and age. The SQL statement is `select firstname, age from ossobject`. If the KeepAllColumns parameter is set to true, the output result is firstname,,age, in which a comma \(,\) is added to indicate the position of the excluded lastname column. If the KeepAllColumns parameter is set to false, the output result is firstname,age. By using this parameter, you do not need to modify the code, but can directly use the code that is used to process the GetObject API to process the SelectObject API.

|
        |OutputRawData|         -   If this parameter is set to true, the select\_object operation directly returns the original data. If data is not returned for a long time, a timeout error may occur.
        -   If this parameter is set to false, the output data is encapsulated in frames. Default value: false.
 |
        |EnablePayloadCrc|Indicates whether a cyclic redundancy check \(CRC\) value is calculated for each frame. Default value: false.|
        |OutputHeader|The header information in the first line of the output result. This parameter applies only to CSV objects.|
        |SkipPartialDataRecord|         -   If this parameter is set to true, the current record is skipped when a column in a CSV object has no data or a key in a JSON object does not exist.
        -   If this parameter is set to false, a column without data is left empty in the output result.
 Example:

 A row includes the columns firstname, lastname, and age. The SQL statement is `select _1, _4 from ossobject`. If the SkipPartialDataRecord parameter is set to true, this row is skipped. If the SkipPartialDataRecord parameter is set to false, the following result is returned: firstname,\\n.

 |
        |MaxSkippedRecordsAllowed|The maximum number of skipped rows. Default value: 0, indicating that an error is returned if a row is skipped.|
        |ParseJsonNumberAsString|If this parameter is set to true, numbers in the JSON object are resolved as strings. If this parameter is set to false, numbers in the JSON object are resolved as integers or floating-point numbers. Default value: false.High-accuracy floating-point numbers in a JSON object suffer from loss of accuracy when they are resolved as floating-point numbers. To keep the accuracy, you can set this parameter to true and use the CAST function to convert the resolved data into the decimal type.

|

    -   Returned result of the select\_object operation: A SelectObjectResult object is returned. You can use the read\(\) function or the \_\_iter\_\_ method to obtain all results. If the output result contains large amounts of data, the read\(\) function is not the optimal method to obtain all results. This function may block the system until all results are returned and use excessive memory resources.

        We recommend that you use the \_\_iter\_\_ method \(foreach chunk in result\) to obtain all results and process each chunk in the results. This method uses fewer memory resources and allows the client to process each chunk immediately after it is processed by the OSS server. The client does not need to wait for all results to be returned.

-   select\_object\_to\_file

    ```
    def select_object_to_file(self, key, filename, sql,
                       progress_callback=None,
                       select_params=None
                       ):
    ```

    The preceding sample code is used to run an SQL statement on the object with the specified key and write the query results to another specified object.

    Other parameters are the same as those of the [select\_object](#ul_unq_lgf_4gb) operation.

-   create\_select\_object\_meta
    -   The following sample code shows the syntax of the select\_meta\_params parameter.

        ```
        def create_select_object_meta(self, key, select_meta_params=None):
        ```

        The preceding sample code is used to create Select Meta for the object with the specified key or obtain Select Meta from such an object. Select Meta includes the total number of rows, total number of columns \(for CSV objects\), and total number of splits in an object.

        If Select Meta has been created for an object, this function does not re-create Select Meta unless the value of the OverwriteIfExists parameter is set to true.

        To create Select Meta for an object, the object must be completely scanned.

    -   The following table describes the parameters supported by the select\_meta\_params parameter.

        |Name|Description|
        |----|-----------|
        |Json\_Type|If this parameter is left empty, the object is a CSV object by default. If this parameter is specified, the parameter value must be LINES, indicating that the object is a JSON LINES object. This operation does not apply to JSON DOCUMENT objects.|
        |RecordDelimiter|The row delimiter in the CSV object.|
        |FieldDelimiter|The column delimiter in the CSV object.|
        |QuoteCharacter|The quote character in the CSV object. Row and column delimiters enclosed in quotation marks are processed as normal characters.|
        |CompressionType|The compression type. If this parameter is specified, the parameter value must be None.|
        |OverwriteIfExists|Indicates whether the created Select Meta overwrites the original Select Meta. You do not need to set this parameter in common scenarios.|

    -   Returned result of the create\_select\_object\_meta operation: A GetSelectObjectMetaResult object is returned, which includes the rows and splits attributes. For a CSV object, the select\_resp object in the result includes the columns attribute, indicating the number of columns in the CSV object.

## Java SDK {#section_xlv_t1f_4gb .section}

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
import com.aliyuncs.sts.model.v20150401. AssumeRoleRequest;
import com.aliyuncs.sts.model.v20150401. AssumeRoleResponse;

import java.text.SimpleDateFormat;
import java.util.Calendar;

/**
 * Examples of the create_select_object_meta and select_object operations.
 *
 */
class MulipartSelector implements Callable<Integer> {

    private OSS client;
    private String bucket;
    private String key;
    private int start;
    private int end;
    private String sql;

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
            while ((bytesRead = ossObject.getObjectContent().read(buffer)) ! = -1) {
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
    // Obtain the current Security Token Service (STS) API version.
    public static final String STS_API_VERSION = "2015-04-01";
    public static final String serviceAccessKeyId = "<AccessKey ID that can play the assumed role>";
    public static final String serviceAccessKeySecret = "<AccessKey Secret>";

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
            // Create an AcsClient instance for sending API requests.
            if (this.client == null) {
                IClientProfile profile = DefaultProfile.getProfile(REGION_CN_HANGZHOU, accessKeyId, accessKeySecret);
                this.client = new DefaultAcsClient(profile);
            }
            // Create an AssumeRoleRequest instance and set its properties.
            final AssumeRoleRequest request = new AssumeRoleRequest();
            request.setVersion(STS_API_VERSION);
            request.setMethod(MethodType.POST);
            request.setProtocol(protocolType);
            request.setRoleArn(roleArn);
            request.setRoleSessionName(roleSessionName);
            request.setPolicy(policy);
            request.setDurationSeconds(durationSeconds);
            // Send the request and obtain the response.
            final AssumeRoleResponse response = client.getAcsResponse(request);
            return response;
        } catch (ClientException e) {
            throw e;
        }
    }


    public CredentialsProvider GetCredentialProvider()
            throws IOException {
        // Request parameters for the AssumeRole API include RoleArn, RoleSessionName, Policy, and DurationSeconds.
        // You need to obtain the value of the RoleArn parameter in the Resource Access Management (RAM) console.
        // The RoleSessionName parameter indicates the name of the session for the temporary token. You can use this parameter to identify users in audit or identify users who you want to issue tokens to.
        // However, you need to pay attention to the length and rules of the RoleSessionName parameter. It can contain only letters, numbers, hyphens (-), and underscores (_), and cannot include spaces.
        // For more information about the rules, see the format requirements in the API reference.
        SimpleDateFormat timeFormat = new SimpleDateFormat("yyyy-MM-dd");
        String roleSessionName = "AssumingRole" + timeFormat.format(Calendar.getInstance().getTime());
        // Read OSS data.
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
        // You must set the protocol type to HTTPS.
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
        if (credential ! = null && expireTime.after(Calendar.getInstance())) {
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
    private static String endpoint = "<OSS endpoint>";
    private static String bucketName = "<Bucket>";
    private static String key = "<Object>";
    private static String roleArn = "<Service role's ARN>"; // You can obtain the Alibaba Cloud Resource Name (ARN) of a RAM role in the RAM console. The RAM role must have permissions to access OSS.
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

-   SQL statement examples for CSV objects

    |Scenario|SQL statement|
    |:-------|:------------|
    |Returns the first 10 rows.|select \* from ossobject limit 10|
    |Returns integers in the first and third columns, in which the integers in the first column are larger than those in the third column.|select \_1, \_3 from ossobject where cast\(\_1 as int\) \> cast\(\_3 as int\)|
    |Returns the number of records in which the data in the first column starts with X. \(A Chinese character specified after like must be UTF-8 encoded.\)|select count\(\*\) from ossobject where \_1 like 'X%'|
    |Returns all records in which the time of the data in the second column is later than 2018-08-09 11:30:25 and the data in the third column is larger than 200.|select \* from ossobject where \_2 \> cast\('2018-08-09 11:30:25' as timestamp\) and \_3 \> 200|
    |Returns the average value, sum, maximum value, and minimum value of the floating-point numbers in the second column.| select AVG\(cast\(\_2 as double\)\), SUM\(cast\(\_2 as double\)\), MAX\(cast\(\_2 as double\)\), MIN\(cast\(\_2 as double\)\)

 |
    |Returns all records in which the strings concatenated by the data in the first and third columns start with Tom and end with Anderson.|select \* from ossobject where \(\_1 || \_3\) like 'Tom%Anderson'|
    |Returns all records in which the data in the first column is divisible by 3.|select \* from ossobject where \(\_1 % 3\) == 0|
    |Returns all records in which the data in the first column ranges from 1995 to 2012.|select \* from ossobject where \_1 between 1995 and 2012|
    |Returns all records in which the data in the fifth column is N, M, G, or L.|select \* from ossobject where \_5 in \('N', 'M', 'G', 'L'\)|
    |Returns all records in which the product of the data in the second and third columns is larger than the sum of 100 and the data in the fifth column.|select \* from ossobject where \_2 \* \_3 \> \_5 + 100|

-   SQL statement examples for JSON objects

    The following JSON object is used as an example.

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
    }, ... # Other similar nodes are omitted.
    ]}
    ```

    The following table describes the SQL statement examples.

    |Scenario|SQL statement|
    |--------|-------------|
    |Returns all records in which the value of age is 27.| select \* from ossobject.contacts\[\*\] s where s.age = 27

 |
    |Returns all home phone numbers.| select s.number from ossobject.contacts\[\*\].phoneNumbers\[\*\] s where s.type = "home"

 |
    |Returns all records in which the value of spouse is null.| select \* from ossobject s where s.spouse is null

 |
    |Returns all records in which the value of children is left empty.|select \* from ossobject s where s.children\[0\] is null**Note:** The preceding statement is used because an empty array cannot be specified in other ways.

|


## Best practices {#section_vk1_hky_b2b .section}

-   Query large objects in multipart queries.

    If columns in a CSV object do not include row delimiters, you can divide the object into parts based on bytes. This method is the simplest because you do not need to create Select Meta for the object. If columns in a CSV object include row delimiters or a JSON object is queried, follow these steps:

    1.  Call the create\_select\_object\_meta operation to obtain the total number of splits for the object. If you need to call the SelectObject API for the object, call it asynchronously before the query to shorten the scanning time.
    2.  Select the appropriate concurrency n based on resources on the client, and divide the total number of splits by the concurrency n to get the number of splits to be contained in each query.
    3.  Set parameters, such as split-range=1-20, in the request body to perform multipart queries.
    4.  Merge the results if required.
-   Use the SelectObject API for normal objects. We recommend that you do not use the SelectObject API to query multipart and appendable objects. The differences in their internal structures may deteriorate the query performance.
-   When querying a JSON object, narrow down the JSON path range in the FROM statement.

    \[DO NOT TRANSLATE\]

    The following JSON object is used as an example.

    ```
    { contacts:[
            {"firstName":"John", "lastName":"Smith", "phoneNumbers":[{"type":"home", "number":"212-555-1234"}, {"type":"office", "number":"646-555-4567"}, {"type":"mobile", "number":"123 456-7890"}], "address":{"streetAddress": "21 2nd Street", "city":"New York", "state":NY, "postalCode":"10021-3100"}
             }
    ]}
    ```

    To query all streetAddress data of records in which postalCode starts with 10021, run the following SQL statement:

    `select s.address.streetAddress from ossobject.contacts[*] s where s.address.postalCode like ‘10021%’`

    Alternatively, run the following SQL statement:

    `select s.streetAddress from ossobject.contacts[*].address s where s.postalCode like ‘10021%’`

    The query performance of the second SQL statement is better because the JSON path is more accurate.

-   Process high-accuracy floating-point numbers in JSON objects.

    If you need to calculate high-accuracy floating-point numbers in a JSON object, we recommend that you set the ParseJsonNumberAsString parameter to true, and use the CAST function to convert the resolved data into the decimal type. For example, the value of attribute a is 123456789.123456789. You can run `select s.a from ossobject s where cast(s.a as decimal) > 123456789.12345` to maintain the accuracy of attribute a.


