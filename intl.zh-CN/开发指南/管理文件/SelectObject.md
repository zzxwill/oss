# SelectObject {#concept_ghk_xz2_4gb .concept}

SelectObject典型的应用场景是做日志文件分析，还可以和大数据产品结合使用。本文主要介绍如何使用Python SDK以及Java SDK实现以上应用场景。

## 功能介绍 {#section_jwy_bbx_b2b .section}

对象存储（Object Storage Service，简称OSS） 是基于阿里云飞天分布式系统的海量、安全和高可靠的云存储服务，是一种面向互联网的大规模、低成本、通用存储，提供RESTful API，具备容量和处理的弹性扩展能力。OSS不仅非常适合存储海量的媒体文件，也适合作为数据仓库存储海量的数据文件。目前Hadoop 3.0已经支持OSS，在EMR上运行Spark、Hive、Presto等服务，以及阿里自研的MaxCompute、HybridDB以及新上线的Data Lake Analytics都支持从OSS直接处理数据。

然而，目前OSS提供的GetObject接口决定了大数据平台只能把OSS数据全部下载到本地然后进行分析过滤，在很多查询场景下浪费了大量带宽和客户端资源。

SelectObject接口是对上述问题的解决方案。其核心思想是大数据平台将条件、Projection下推到OSS层，让OSS做基本的过滤，从而只返回有用的数据。客户端一方面可以减少网络带宽，另一方面也减少了数据的处理量，从而节省了CPU和内存用来做其他更多的事情。这使得基于OSS的数据仓库、数据分析成为一种更有吸引力的选择。

SelectObject提供了Java和Python 的SDK，其他语言SDK也会很快推出。目前支持RFC 4180标准的CSV（包括TSV等类CSV文件，文件的行列分隔符以及Quote字符都可自定义）和Json文件，且文件编码为UTF-8。支持标准存储类型和低频访问存储类型的文件（归档文件需要先取回）。支持加密文件（OSS完全托管加密、KMS托管主密钥）。

JSON支持DOCUMENT和LINES两种文件。DOCUMENT是指整个文件是单一的JSON对象， LINES表示整个文件由一行行的JSON对象组成，每一行是一个JSON对象（但整个文件本身并不是一个合法的JSON对象），行与行之间以换行分隔符隔开。OSS Select可以支持常见的\\n，\\r\\n等分隔符，且无需用户指定。

-   支持的SQL语法
    -   SQL 语句： Select From Where
    -   数据类型：string、int\(64bit\)、double\(64bit\), decimal\(128\) 、timestamp、bool
    -   操作： 逻辑条件（AND,OR,NOT\)， 算术表达式（+-\*/%\)， 比较操作\(\>,=, <, \>=, <=, !=\)，String 操作 \(LIKE, || \)
-   分片查询

    和GetObject提供了基于Byte的分片下载类似，SelectObject也提供了分片查询的机制，包括两种分片方式：按行分片、按Split分片。按行分片是常用的分片方式，然而对于稀疏数据来说，按行分片可能会导致分片时负载不均衡。Split是OSS用于分片的一个概念，一个Split包含多行数据，每个Split的数据大小大致相等。按Spit分片比按行分片要更加高效。

-   数据类型

    关于数据类型，OSS中的CSV数据默认都是String类型，用户可以使用CAST函数实现数据转换，比如下面的SQL查询将\_1和\_2转换为int后进行比较。

    `Select * from OSSOBject where cast (_1 as int) > cast(_2 as int)`

    同时，对于SelectObject支持在Where条件中进行隐式的转换，比如下面的语句中第一列和第二列将被转换成int：

    `Select _1 from ossobject where _1 + _2 > 100`

    对于JSON文件，如果在SQL中未指定cast函数，则其类型根据Json数据的实际类型而定，标准Json内建的数据类型包括null、bool、int64、double、string等类型。


## Python SDK 样例 {#section_usn_41f_4gb .section}

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
# 上传CSV文件
bucket.put_object(key, content)
# Select API的参数
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
# 上传JSON DOCUMENT
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
# 上传JSON LINE
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

Python Select API详解

-   select\_object 
    -   select\_object 示例：

        ```
        def select_object(self, key, sql,
                           progress_callback=None,
                           select_params=None                   ):
        ```

        上述示例的功能：对指定Key的文件运行SQL，返回查询结果。

        -   参数SQL是原始的SQL字符串，无需做base64编码。
        -   Progress\_callback是可选的用来汇报进度的回调函数。
        -   select\_params是该API的重点，它指定了select执行的各种参数以及行为。
        -   headers可以让用户指定请求中附带的header信息，其行为和get-object一致。比如，对于CSV文件，在某些情况下可以用bytes来指定SQL查询在文件中的范围。
    -   select\_params支持的参数

        |参数名称|描述|
        |----|--|
        |Json\_Type|         -   当Json\_Type没有指定时，默认为CSV文件。
        -   当Json\_Type为DOCUMENT时，该文件为JSON文件。
        -   当Json\_Type为LINES时，该文件为JSON LINE文件。
 |
        |CsvHeaderInfo|CSV的header信息，合法值为None、Ignore、 Use。        -   None：该文件没有header信息
        -   Ignore：该文件有header信息但未在SQL中使用。
        -   Use：该文件有Header信息且在sql语句中使用了Header中的列名。
|
        |CommentCharacter|CSV中的注释字符。仅支持一个字符，默认为None表示没有注释字符。|
        |RecordDelimiter|CSV中的行分隔符，仅支持一个或两个字符。默认为\\n。|
        |OutputRecordDelimiter|Select输出结果中的行分隔符，默认为\\n。|
        |FieldDelimiter|CSV的列分隔符，仅支持一个字符，默认为逗号,。|
        |OutputFieldDelimiter|Select输出结果中的列分隔符，默认为,。|
        |QuoteCharacter|CSV 列的引号字符，只支持一个字符，默认为双引号。引号内的行列分隔符被当做普通字符处理。|
        |SplitRange|使用Split做分片查询。格式为\(start, end\)，此处为闭区间，表示查询范围从Split start\#到end\#。|
        |LineRange|使用行做分片查询。格式为\(start, end\)，此处为闭区间，表示查询范围从行号start\#到end\#。|
        |CompressionType|压缩类型，默认为None，可以为GZIP。|
        |KeepAllColumns|该参数为true时表示原CSV文件中未在select列中出现的列将以空值输出（但保留列的位置）。默认为False。示例如下：

如果CSV文件中的列为 firstname, lastname, age。SQL为`select firstname, age from ossobject`。如果KeepAllColumns为true，则输出为firstname,,age（中间多了一个逗号）。如果KeepAllColumns为false，则输出为firstname,age。引入该选项的原因是让原本处理Get-Object返回数据的代码可以不用修改的情况下平移切换到用Select object。

|
        |OutputRawData|         -   该参数为True时表示输出为Select数据，没有Frame的包装，也意味着当很长时间不返回数据时会引起超时。
        -   该参数为False时表示输出为Frame包装的数据，默认是False。
 |
        |EnablePayloadCrc|为每个Frame计算CRC校验值，默认为 False。|
        |OutputHeader|仅用于CSV文件，表示输出结果中第一行是Header信息。|
        |SkipPartialDataRecord|         -   该参数为True时，当CSV中某一列值不存在或者Json中某一个Key不存在时，直接跳过整个记录。
        -   该参数为False时，则把该列当做null来处理。
 示例如下：

 假如某一行的列为firstname,lastname,age。SQL为`select _1, _4 from ossobject`。如果为True，则该行被完全跳过，如果为False，则返回firstname,\\n。

 |
        |MaxSkippedRecordsAllowed|允许跳过的行的最大值。默认为0，表示一旦有一行跳过就返回错误。|
        |ParseJsonNumberAsString|当该参数为True时表示Json文件中的数字都解析为字符串；为False时则仍按照整数或者浮点数进行解析，False为默认值。当Json文件中含有高精度的浮点数时，直接解析为浮点数会丢失精度。如果想保留原始的精度，则可以设置该参数为True，并且在SQL语句中将该列Cast为decimal类型即可。

|

    -   select\_object 返回值：返回SelectObjectResult对象，该对象支持read\(\)函数以获得所有select结果。同时也支持\_\_iter\_\_方法。如果Select结果非常大，用read\(\)函数则不是最佳的办法，因为该调用会阻塞直到select结果完全返回，同时占用过多的内存。

        推荐的做法是用\_\_iter\_\_方法（foreach chunk in result），然后对每个chunk进行处理。此做法的优势是一方面降低内存使用，另一方面OSS服务器端每处理一个请求chunk客户端就能及时处理，不必等到全部返回后才处理。

-   select\_object\_to\_file

    ```
    def select_object_to_file(self, key, filename, sql,
                       progress_callback=None,
                       select_params=None
                       ):
    ```

    上述示例的功能：对指定Key的文件运行SQL，将查询结果写入指定的文件。

    其他参数同[select\_object](#ul_unq_lgf_4gb)相同。

-   create\_select\_object\_meta
    -   select\_meta\_params 的语法结构

        ```
        def create_select_object_meta(self, key, select_meta_params=None):
        ```

        上述示例的功能：对指定Key的文件进行创建或者获取select meta。Select meta是指该文件的总行数，总列数（CSV文件）、总的Split数。

        如果该文件已经创建过meta，则调用该函数不会重新创建，除非在参数中指定OverwriteIfExists为true。

        创建select meta需要扫描整个文件。

    -   select\_meta\_params 中支持的参数

        |参数名称|描述|
        |----|--|
        |Json\_Type|当Json\_Type没有指定时，默认为CSV文件。若指定，必须为LINES，表示文件是Json LINES。JSON DOCUMENT不支持该操作。|
        |RecordDelimiter|CSV文件换行符。|
        |FieldDelimiter|CSV文件列分隔符。|
        |QuoteCharacter|CSV文件列引号符。引号符内的行列分隔符按普通字符处理。|
        |CompressionType|压缩类型，若指定，只能为None。|
        |OverwriteIfExists|覆盖原有的Select Meta。正常情况无需使用该选项。|

    -   create\_select\_object\_meta 返回值： GetSelectObjectMetaResult对象，它包括rows、 splits 两个属性。对于CSV文件，其内部的select\_resp对象还包括columns值，表示CSV文件的列数。

## Java SDK 样例 {#section_xlv_t1f_4gb .section}

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
    // 当前 STS API 版本
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
        // AssumeRole API 请求参数: RoleArn, RoleSessionName, Policy, and DurationSeconds
        // RoleArn 需要在 RAM 控制台上获取
        // RoleSessionName 是临时Token的会话名称，自己指定用于标识你的用户，主要用于审计，或者用于区分Token颁发给谁
        // 但是注意RoleSessionName的长度和规则，不要有空格，只能有'-' '_' 字母和数字等字符
        // 具体规则请参考API文档中的格式要求
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
        // 此处必须为 HTTPS
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
    private static String roleArn = "<service role’s ARN>";// 访问控制台中可以获得一个RAM角色的ARN，该角色必须有访问OSS的权限
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

## 常见的SQL用例 {#section_c4k_hrs_ngb .section}

-   常见的SQL用例（Csv）

    |应用场景|SQL语句|
    |:---|:----|
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

-   常见的SQL用例（Json）

    假如我们有如下Json文档。

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
    }，…… #此处省略其他类似的节点
    ]}
    ```

    SQL用例如下：

    |应用场景|SQL语句|
    |----|-----|
    |返回所有age是27的记录| select \* from ossobject.contacts\[\*\] s where s.age = 27

 |
    |返回所有的家庭电话| select s.number from ossobject.contacts\[\*\].phoneNumbers\[\*\] s where s.type = “home”

 |
    |返回所有单身的记录| select \* from ossobject s where s.spouse is null

 |
    |返回所有没有孩子的记录|select \* from ossobject s where s.children\[0\] is null**说明：** 目前没有专用的空数组的表示方法，用以上语句代替。

|


## 最佳实践 {#section_vk1_hky_b2b .section}

-   大文件分片查询

    如果确定该CSV文件列中不含有换行符，则基于Bytes的分片由于不需要创建Meta，其使用最为简便。如果列中包含换行符或者是Json文件时，则使用以下步骤：

    1.  调用Create Select Object Meta API获得该文件的总的Split数。理想情况下如果该文件需要用SelectObject，则该API最好在查询前进行异步调用，这样可以节省扫描时间。
    2.  根据客户端资源情况选择合适的并发度n，用总的Split数除以并发度n得到每个分片查询应该包含的Split个数。
    3.  在请求Body中用诸如split-range=1-20的形式进行分片查询。
    4.  如果需要最后可以合并结果。
-   SelectObject和Normal类型文件配合性能更佳。Multipart 以及Appendable类型的文件由于其内部结构差异导致性能较差。
-   查询Json文件时，在SQL的From 语句中尽可能的缩小From后的Json Path 范围。

    示例：有如下Json文件。

    ```
    { contacts:[
            {“firstName”:”John”, “lastName”:”Smith”, “phoneNumbers”:[{“type”:”home”, “number”:”212-555-1234”}, {“type”:”office”, “number”:”646-555-4567”}, {“type”:”mobile”, “number”:”123 456-7890”}], ”address”:{“streetAddress”: “21 2nd Street”, “city”:”New York”, “state”:NY, “postalCode”:”10021-3100”}
             }
    ]}
    ```

    如果要查找所有postalCode为 10021 开头的streetAddress，可以这样写SQL：

    `select s.address.streetAddress from ossobject.contacts[*] s where s.address.postalCode like ‘10021%’`

    也可以这样写SQL：

    `select s.streetAddress from ossobject.contacts[*].address s where s.postalCode like ‘10021%’`

    第二个SQL由于Json Path更加精确，所以性能会更好。

-   在Json文件中处理高精度浮点数

    在Json文件中需要进行高精度浮点数的数值计算时，推荐的做法是使用ParseJsonNumberAsString选项为true, 同时将该值cast成Decimal。比如一个属性a值为123456789.123456789，用`select s.a from ossobject s where cast(s.a as decimal) > 123456789.12345`就可以保持原始数据的精度不丢失。


