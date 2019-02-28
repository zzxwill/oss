# SelectObject {#reference_lz1_r1x_b2b .reference}

SelectObject is used to query an object that you have the read permission on.

## SelectObject {#section_fm3_3zq_ngb .section}

SelectObject is used to run SQL statements on the target object and return the query result.

The 206 status code is returned if the operation is successfully performed. If the SQL statements are incorrect or do not match the target object, the 400 status code is returned.

**Note:** For more information about the functions of SelectObject, see [SelectObject](../../../../../reseller.en-US/Developer Guide/Manage files/SelectObject.md#).

-   Request syntax
    -   Request syntax \(CSV\)

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
        			<FileHeaderInfo>
        				NONE|IGNORE|USE
        			</FileHeaderInfo>
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
        			
        		</CSV>
        	        <KeepAllColumns>false|true</KeepAllColumns>
        	        <OutputRawData>false|true</OutputRawData>
             	        <EnablePayloadCrc>true</EnablePayloadCrc>
        	        <OutputHeader>false</OutputHeader>
        	</OutputSerialization>
        	<Options>
        	    <SkipPartialDataRecord>false</SkipPartialDataRecord>
        	    <MaxSkippedRecordsAllowed>
        		max allowed number of records skipped
        	    <MaxSkippedRecordsAllowed>
        	</Options>
        </SelectRequest>
        ```

    -   Request syntax \(JSON\)

        ```
        POST /object?x-oss-process=json/select HTTP/1.1 
        HOST: BucketName.oss-cn-hangzhou.aliyuncs.com 
        Date: time GMT
        Content-Length: ContentLength
        Content-MD5: MD5Value 
        Authorization: Signature
        
        <?xml  version="1.0"  encoding="UTF-8"?>
        <SelectRequest>
        	<Expression>
        		Base64 encode of sql such as (select * from ossobject)
        	</Expression>
        	<InputSerialization>
        		<CompressionType>None|GZIP</CompressionType>
        		<JSON>
        			<Type>DOCUMENT|LINES</Type>
        			<Range>
        			line-range=start-end|split-range=start-end
        			</Range>
        			<ParseJsonNumberAsString> true|false
        			</ParseJsonNumberAsString>
        		</JSON>
        	</InputSerialization>
        	<OutputSerialization>
        		<JSON>
        			<RecordDelimiter>
        				Base64 of record delimiter
        			</RecordDelimiter>
        		</JSON>
        		<OutputRawData>false|true</OutputRawData>
             	        <EnablePayloadCrc>true</EnablePayloadCrc>
        	</OutputSerialization>
        	<Options>
        		<SkipPartialDataRecord>
        			false|true
        		</SkipPartialDataRecord>
        		<MaxSkippedRecordsAllowed>
        			max allowed number of records skipped
           		<MaxSkippedRecordsAllowed>
                </Options>
        </SelectRequest>
        ```

-   Request elements

    |Element|Type|Description|
    |:------|:---|:----------|
    |SelectRequest|Container| Specifies the container that saves the SelectObject request.

 Child nodes: Expression, InputSerialization, and OutputSerialization

 Parent node: None

 |
    |Expression|String| Specifies the base64-coded SQL statements.

 Child node: None

 Parent node: SelectRequest

 |
    |InputSerialization|Container| \(Optional\) Specifies the input serialization parameters.

 Child nodes: CompressionType, CSV, and JSON

 Parent node: SelectRequest

 |
    |OutputSerialization|Container| \(Optional\) Specifies the output serialization parameters.

 Child nodes: CSV, JSON, and OutputRawData

 Parent node: SelectRequest

 |
    |CSV\(InputSerialization\)|Container| \(Optional\) Specifies the format parameter for the input CSV file.

 Child nodes: FileHeaderInfo, RecordDelimiter, FieldDelimiter, QuoteCharacter, CommentCharacter, and Range

 Parent node: InputSerialization

 |
    |CSV\(OutputSerialization\)|Container| \(Optional\) Specifies the format parameter for the output CSV file.

 Child nodes: RecordDelimiter and FieldDelimiter

 Parent node: OutputSerialization

 |
    |JSON\(InputSerialization\)|Container| Specifies the format parameter for the input JSON file.

 Child node: Type

 |
    |Type|Enumeration| Specifies the type of the input JSON file: DOCUMENT | LINES

 |
    |JSON\(InputSerialization\)|Container| Specifies the format parameter for the input JSON file.

 Child node: RecordDelimiter

 |
    |OutputRawData|Bool \(false by default\)| \(Optional\) Specifies the output data as raw data, which is not the frame-based format.

 Child node: None

 Parent node: OutputSerialization

 |
    |CompressionType|Enumeration| Specifies the compression type of the object: None|GZIP

 Child node: None

 Parent node: InputSerialization

 |
    |FileHeaderInfo|Enumeration| \(Optional\) Specifies the header information about the CSV file.

 Valid values:

     -   Use: Indicates that the CSV file contains header information. You can use the column name in the CSV file as the column name in the SelectObject operation.
    -   Ignore: Indicates that the CSV file contains header information. However, you cannot use the column name in the CSV file as the column name in the SelectObject operation.
    -   None: Indicates that the CSV file does not contain header information. This is the default value.
 Child node: None

 Parent node: CSV \(input\)

 |
    |RecordDelimiter|String| \(Optional\) Specifies the delimiter, which is base64-encoded and `\n` by default. The value of this element before being encoded can be the ANSI value of two characters in maximum. For example, `\n` is used to indicate a line break in Java code.

 Child node: None

 Parent node: CSV \(input and output\) and JSON \(output\)

 |
    |FieldDelimiter|String| \(Optional\) Specifies the delimiter used to separate columns in the CSV file. The value of this element is the base64-encoded ANSI value of a character and is `,` by default. For example, `,` is used to indicate a comma in Java code.

 Child node: None

 Parent node: CSV \(input and output\)

 |
    |QuoteCharacter|String| \(Optional\) Specifies the quote characters used in the CSV file. The value of this element is base64-encoded and is `\”` by default. In a CSV file, line breaks and column delimiters are processed as normal characters. The value of this element before being encoded must be the ANSI value of a character. For example, `\”` is used to indicate a quote character in Java code.

 Child node: None

 Parent node: CSV \(input\)

 |
    |CommentCharacter|String|Specifies the comment character used in the CSV file. The value of this element is base64-encoded and is null \(no comment character\) by default.|
    |Range|String| \(Optional\) Specifies the query range. The following two query methods are supported:

     -   Query by rows: line-range=start-end
    -   Query by splits:split-range=start-end
 The start and end parameters in the preceding code are both inclusive. The format of the two parameters are the same as that of the range parameter in range get operations.

 This parameter is valid only when the document is in CSV format or the JSON Type is LINES.

 Child node: None

 Parent nodes: CSV \(input\) and JSON \(input\)

 |
    |KeepAllColumns|Bool| \(Optional\) Indicates that all columns in the CSV file are included in the returned result. However, only columns included in the select statement have values. The default value of this parameter is false. The columns in the returned result are sorted in order of the column numbers from low to high. For example:

 `select _5, _1 from ossobject.`

 If you set the value of KeepAllColumn to true and six columns are included in the CSV file, the following result is returned for the preceding select statement:

 Value of 1st column,,,,Value of 5th column,\\n

 Child node: None

 Parent node: OutputSerialization \(CSV\)

 |
    |EnablePayloadCrc|Bool| Indicates that each frame includes a 32-bit CRC32 value for verification. The client can calculate the CRC32 value of each payload and compare it with the included CRC32 value to verify data integrity.

 Child node: None

 Parent node: OutputSerialization

 |
    |Options|Container| Specifies other optional parameters.

 Child node: SkipPartialDataRecord and MaxSkippedRecordsAllowed

 Parent node: SelectRequest

 |
    |OutputHeader|Bool| Indicates that the header information about the CSV file is included in the beginning of the returned result.

 Default value: false

 Child node: None

 Parent node: OutputSerialization

 |
    |SkipPartialDataRecord|Bool| Indicates that rows without data are ignored. If the value of this parameter is false, OSS ignores rows without data \(by processing the values of the rows as null\) and does not report errors. If the value of this parameter is true, a row without data is skipped. If the number of skipped rows exceeds the maximum allowed number, OSS reports an error and stops processing the data.

 Default value: false

 Child node: None

 Parent node: Options

 |
    |MaxSkippedRecordsAllowed|Integer| Specifies the maximum allowed number of skipped rows. If a row does not match the type specified in the SQL statement, or a column or multiple columns in a row are missed and the value of SkipPartialDataRecord is True, the row is skipped. If the number of skipped rows exceeds the value of this parameter, OSS reports an error and stops processing the data.

 **Note:** If a row is not in the valid CSV format, for example, a column in the row includes continual odd numbered quote characters, OSS stops processing the data immediately and reports an error because this format error may result in incorrect resolution to the CSV file. That is, this parameter can be used to adjust the tolerance for irregular data but does not applied to invalid CSV files.

 Default value: 0

 Child node: None

 Parent node: Options

 |
    |ParseJsonNumberAsString|Bool| Indicates that the numbers \(integer and float numbers\) in a JSON file are resolved into strings. The accuracy of float numbers in a JSON file degrades when the numbers are resolved. Therefore, we recommend that you set the value of this parameter to true if you want to keep the original data. To use the numbers for calculation, you can cast them into the required format, such as int, double, or decimal, in the SQL statement.

 Default value: false

 Child node: None

 Parent node: JSON

 |

-   Response body

    If the HTTP status included in the response for a request is 4xx, it indicates that the request does not pass the SQL syntax check or an obvious error is included in the request. In this case, the body format of the returned error message is the same as that of the error message returned for a GetObject request.

    If the HTTP status code included in the response for a request is 5xx, it indicates that an error occurs in the server. In this case, the body format of the returned message is the same as that of the error message returned for a GetObject request.

    If the HTTP status code 206 is returned in the response and the value of header x-oss-select-output-raw is true, it indicates that the object data \(but not frame-based data\) is successfully returned. The client can obtain the data in the same way as that used in GetObject operations.

    If the value of x-oss-select-output-raw is false, the result is returned as frames.

    If you set a value for OutputRawData in a request, OSS returns the requested data in the format that you specified. However, we recommend that you do not set a value for OutputRawData so that OSS returns the requested data in the format automatically select by OSS.

    If you set the value of OutputRawData to true in an HTTP request, the request may be time out when no data is returned for the SQL statement for a long period.

    If you perform a SelectObject operation using a JSON file and the select statement includes repeated keys \(for example: select s.key, s.key from ossoobject s\), the value of the x-oss-select-output-json-dup-key header in the response is true.

    A returned frame is in the following format, in which the checksum is CRC32

    Version|Frame-Type | Payload Length | Header Checksum | Payload | Payload Checksum

    <1 byte\><--3 bytes--\><---4 bytes----\><-------4 bytes--\><variable\><----4bytes----------\>

    All integers in a frame are big-endian. Currently, the value of Version is 1.

    SelectObject supports three frame types, as described in the following table.

    |Frame type|Frame-Type value|Payload format|Description|
    |:---------|:---------------|:-------------|:----------|
    |Data Frame|8388609| offset  |  data

 <-8 bytes\><---variable-\>

 |A data frame includes the data returned for the SelectObject request. The offset parameter is an 8-bit integer, which indicates the current scanning location \(the offset from the file header\) and is used to report the progress of the operation.|
    |Continuous Frame|8388612| offset

 <----8 bytes--\>

 |A continuous frame is used to report the progress of an operation and keep an HTTP connection. If no data is returned for a query request within 5 seconds, a continuous frame is returned.|
    |End Frame|8388613| offset  | total scanned bytes | http status code | error message

 <--8bytes-\><--8bytes---------\><----4 bytes--------\><-variable------\>

 |An end frame is used to return the final status of an operation, including the scanned bytes and the final offset. The total scanned bytes parameter indicates the size of the scanned data, the http status code parameter indicates the final status of the operation, and the error message parameter includes error messages, including the number of each skipped row and the total number of skipped rows.SelectObject is a streamed operation so that only the first data block is processed when the response header is sent. If the first data block matches the SQL statement, the HTTP status code in the response header is 206, which indicates that the operation is successful. However, the final status code may not be 206 because the following data blocks may be valid but the status code in the response header cannot be modified in this case. Therefore the HTTP status code is included in the end frame to indicate the final status of the operation. The client should use the status code included in the end frame to determine whether the operation is successful.

|

-   Error messages

    The format of error messages included in an end frame is as follows:

    `ErrorCodes.DetailMessage`

    The ErrorCodes part includes a single ErrorCode or multiple ErrorCodes separated by commas. The ErrorCodes and DetailMessage part are sepearated by a period. For detailed error codes, see the ErrorCode list at the end of this topic.

-   Example requests
    -   Example request \(CSV\)

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
        			<CommentCharacter>Iw==</CommentCharacter/>
        		</CSV>
        	</InputSerialization>
        	<OutputSerialization>
        		<CSV>
        			<RecordDelimiter>Cg==</RecordDelimiter>
        			<FieldDelimiter>LA==</FieldDelimiter>
        			<QuoteCharacter>Ig==</QuoteCharacter>			
        		</CSV>
        		<KeepAllColumns>false</KeepAllColumns>
        	        <OutputRawData>false</OutputRawData>
        	</OutputSerialization>
        </SelectRequest>
        ```

    -   Example request \(JSON\)

        ```
        POST /oss-select/sample_json.json?x-oss-process=json%2Fselect HTTP/1.1
        Host: host name
        Accept-Encoding: identity
        User-Agent: aliyun-sdk-python/2.6.0(Darwin/16.7.0/x86_64;3.5.4)
        Accept: */*
        Connection: keep-alive
        date: Mon, 10 Dec 2018 18:28:11 GMT
        authorization: OSS AccessKeySignature
        Content-Length: 317
        
        <SelectRequest>
        	<Expression>c2VsZWN0ICogZnJvbSBvc3NvYmplY3Qub2JqZWN0c1sqXSB3aGVyZSBwYXJ0eSA9ICdEZW1vY3JhdCc=
        	</Expression>
        	<InputSerialization>
        	<JSON>
        		<Type>DOCUMENT</Type>
        	</JSON>
        	</InputSerialization>
        	<OutputSerialization>
        	<JSON>
        		<RecordDelimiter>LA==</RecordDelimiter>
        	</JSON>
        	</OutputSerialization>
        	<Options />
        </SelectRequest>
        ```

-   Regular expressions in an SQL statement

    `SELECT select-list from table where_opt limit_opt`

    SELECT, OSSOBJECT, and WHERE are keywords that cannot be modified.

    ```
    select_list: column name
    
    | column index (for example:_1, _2. column index only applies to CSV files)
    
    | json path (for example: s.contacts.firstname. json path only applies to JSON files)
    
    | function(column index | column name)
    
    | function(json_path) (only applies for JSON files)
    
    | select_list AS alias
    ```

    The following functions are supported: AVG, SUM, MAX, MIN, COUNT, and CAST \(type conversion function\). You can use only the wildcard \(\*\) after COUNT.

    table: OSSOBJECT

    | OSSOBJECT json\_path \(only supported for JSON files\)

    For a CSV file, the table must be OSSOBJECT. For a JSON file \(including DOCUMENT and LINES types\), you can specify a json\_path after OSSOBJECT.

    json\_path: \['string '\] \(The brackets can be deleted if the string does not include a space or a wildcard \(\*\), that is, 'string'.\)

    | \[n\] \(Used to indicate the nth element in an array. The value of n is counted from 0.\)

    | \[\*\] \(Used to indicate any child element in an array or object.\)

    | .'string ' \(The quotation marks around string can be deleted if the string does not include a space or a wildcard \(\*\).\)

    | json\_path jsonpath \(You can concatenate multiple elements in a json path, for example, \[n\].property1.attributes\[\*\].\)

    ```
    Where_opt:
    | WHERE expr
    expr:
    | literal value
    | column name
    | column index
    | json path (only applies to JSON files)
    | expr op expr
    | expr OR expr
    | expr AND expr
    | expr IS NULL
    | expr IS NOT NULL
    | (column name | column index | json path) IN (value1, value2,….)
    | (column name | column index | json path) NOT in (value1, value2,…)
    | (column name | column index | json path) between value1 and value2
    | NOT (expr)
    | expr op expr
    | (expr)
    | cast (column index |column name | json path | literal as INT|DOUBLE|)
    ```

    op: includes the following operators: \>, <, \>=, <=, !=, =, LIKE, +, -, \*, /, %, and ||.

    cast: You can only cast the data in a same column to one type.

    `limit_opt:`

    | limit integer

    Combination use of an aggregation function and limit

    `Select avg(cast(_1 as int)) from ossobject limit 100`

    The preceding statement calculates the average values of the first columns in the first 100 rows, which is different from the MySQL statement. It is because only one row is returned for a aggregation function in SelectObject operations so that it is unnecessary to limit its output. Therefore, limit is performed before aggregation functions in SelectObject operations.

    Limits for SQL statements

    -   Only text files encoded in UTF-8 and UTF-8 text files compressed in the GZIP format are supported. The deflate format is not supported for GZIP files.
    -   An SQL statement can query only one file. The following commands are not supported: join, order by, group by, and having.
    -   A Where statement cannot include an aggregation condition. For example, the following statement is not allowed: where max\(cast\(age as int\)\) \> 100.
    -   A maximum of 1,000 columns are supported. The maximum column number is 1024.
    -   A maximum of 5 wildcard "%" are supported in a LIKE statement. The wildcard "%" plays the same role as the wildcard "\*", which is used to indicate 0 or multiple characters. The keyword Escape is supported in a LIKE statement, which is used to escape the special characters \(such as "%", "\*", and "?"\) into normal strings.
    -   A maximum of 1,024 constants are supported in an IN statement.
    -   The Projection after Select can be a column name, a CSV column index \(such as \_1 and \_2\), an aggregation function, or a CAST function. Other expressions are not supported, for example, select \_1 + \_2 from ossobject.
    -   The maximum column size and row size for a CSV file are 256 KB.
    -   The json path after from supports a JSON node with a maximum size of 512 KB. The path can have 10 levels at most and includes a maximum of 5,000 elements in the array.
    -   In SQL statements for a JSON file, the select or where expressions cannot include the array wildcard \(\[\*\]\), which can be included only in the json path after from. For example, `select s.contacts[*] from ossobject s` is not supported but `select * from ossobject.contacts[*]` is supported.
    -   The maximum size of an SQL statement is 16 KB. A maximum of 20 expressions can be added after where. A statement supports at most 10 levels and 100 aggregation operations.
-   Data error handling
    -   Some columns are missed in some rows in a CSV file.

        If the value of SkipPartialDataRecord is not specified or is set to false, OSS calculates the expressions in the SQL statement by processing the values of the missed columns as null.

        If the value of SkipPartialDataRecord is set to true, OSS ignores the rows in which some columns are missed. In this case, if the value of MaxSkippedRecordsAllowed is not specified or is set to a value smaller than the number of skipped rows, OSS returns an error by sending the 400 HTTP status code or including the 400 status code in the end frame.

        For example, assuming that the SQL statement is `select _1, _3 from ossobject` and the data in a row of the CSV file is "John, company A".

        If the value of SkipPartialDataRecord is set to false, the returned result is "John, \\n". If the value of SkipPartialDataRecord is set to true, this row is ignored.

    -   Some keys are missed in a JSON file.

        Some objects in the JSON file may not include the keys specified in the SQL statement. In this case, if the value of SkipPartialDataRecord is set to false, OSS calculates the expressions in the SQL statement by processing the missed keys as null.

        If the value of SkipPartialDataRecord is true, OSS ignores the data in the JSON node. In this case, if the value of MaxSkippedRecordsAllowed is not specified or is set to a value smaller than the number of ignored rows, OSS returns an error by sending the 400 HTTP status code or including the 400 status code in the end frame.

        For example. assuming that the SQL statement is `select s.firstName, s.lastName , s.age from ossobject.contacts[*] s` and the value of a JSON node is \{“firstName”:”John”，”lastName”:”Smith”\}.

        If the value of SkipPartialDataRecord is not specified or be set to false, the returned result is \{“firstName”:”John”, “lastName”:”Smith”\}. If the value of SkipPartialDataRecord is set to true, this row is ignored.

    -   The data type of some columns in row in a CSV file does not match the SQL statement.

        If the data type of some rows in a CSV file does not match the type specified in the SQL statement, this row is ignored. If the number of ignored rows exceeds the value of MaxSkippedRecordsAllowed, OSS stops processing data and returns a 400 HTTP status code.

        For example, assuming that the SQL statement is `select _1, _3 from ossobject where _3 > 5`.

        If the value of a row in the CSV file is `John, Company A, To be hired`, this row is ignored because the third column in the row is not an integer.

    -   The data type of some keys in a JSON file does not match the SQL statement.

        The handling method is the same as that in a CSV file. For example, assuming that the SQL statement is `select s.name from ossobject s where s.aliren_age > 5`.

        If the value of a JSON node is \{"Name":"John", "Career age":To be hired\}, this node is ignored.

-   Keys in a returned JSON file.

    The returned result for a SelectObject operation using a JSON file is a file in the JSON LINES format, in which the keys are determined as follows:

    -   If the SQL statement is `select * from ossobject…`, if a JSON object \(\{…\}\) is returned for the wildcard \(\*\) , the object is directly returned. If the returned result is not a JSON object \(for example, a string or an array\), a DummyKey\_1 is used to indicates the returned result.

        For example, if the data is \{“Age”:5\} and the SQL statement is `select * from ossobject.Age s where s = 5`. The result returned for the wildcard \(\*\) is 5, which is not a JSON object. Therefore, the returned result for the statement is \{“\_1”:5\}. However, if the statement is `select * from ossobject s where s.Age = 5`, the result returned for the wildcard \(\*\) is the JSON object \{“Age”:5\}, so that the object is directly returned for the statement.

    -   If the SQL statement does not use `select *` but specifies a column, the format of the response is as follows: `{"{column1}":value, "{column2}":value...}`.

        In the response, the value of "n" in \{column n\} is generated as follows:

        -   If the alias of the column is specified in the SelectObject request, the value of n is set to the specified alias.
        -   If the column is a key of a JSON object, the key is used as the output key.
        -   If the column is an aggregation function or an element in a JSON array, the serial number of the column in the output result followed by a prefix \_ is used as the key of the output result.
        For example, if the data is \{”contacts”:\{“Age”:35, “Children”:\[“child1”, “child2”,”child3”\]\}\}, and the SQL statement is `select s.contacts.Age, s.contacts.Children[0] from ossobjects`, the output result is \{“Age”:35, “\_2”:”child1”\}. This result is returned because Age is a key of the input JSON object, but Children \[0\] is the first element in the array Children, which is in the second column in the output result.

        -   If the alias of the row is specified in the request, the output result for `select s.contacts.Age, s.contacts.Children[0] as firstChild from ossobject` is \{“Age”:35, “firstChild”:”child1”\}.
        -   If the SQL statement is `select max(cast(s.Age as int)) from ossobject.contacts s`, the output result is \{“\_1”:35\}, in which the serial number of the column with the prefix \_1 is used to indicate the key because this row is a aggregation function.
    **Note:** Keys in a JSON file are case-sensitive when they are used to match the keys in an SQL statement. For example, `select s_Age` and `select s_age` are different keys.


## CreateSelectObjectMeta {#section_fq4_y1r_ngb .section}

CreateSelectObjectMeta is used to obtain information about the target CSV file, such as the total number of rows, the total number of columns, and the number of Splits. If the information does not exist in the file, the whole CSV file is scanned for the preceding information. The information obtained in the first call of the API is used when the API is called again, so that you do not need to scan the whole CSV file. If the API is executed correctly, the 200 status code is returned. If the target file is not a valid CSV or JSON LINES file, or the specified delimiter does not match the target CSV file, the 400 HTTP status code is returned.

**Note:** You must have the write permission on the target object before performing a CreateSelectObjectMeta operation.

-   Request syntax
    -   Request syntax \(CSV\)

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

    -   Request syntax \(JSON\)

        ```
        POST  /samplecsv?x-oss-process=json/meta
        
        <JsonMetaRequest>
        	<InputSerialization>
        		<CompressionType>None</CompressionType>
        		<JSON>
        			<Type>LINES</Type>
        		</JSON>
        	</InputSerialization>
        	<OverwriteIfExists>false|true</OverwriteIfExists>
        </JsonMetaRequest>
        ```

-   Request elements

    |Element|Type|Description|
    |:------|----|:----------|
    |CsvMetaRequest|Container| Specifies the container that saves the Select csv Meta request.

 Child node: InputSerialization

 Parent node: None

 |
    |JsonMetaRequest|Container| Specifies the container that saves the Select json Meta request.

 Child node: InputSerialization

 Parent node: None

 |
    |InputSerialization|Container| \(Optional\) Specifies the input serialization parameters.

 Child node: CompressionType, CSV, and JSON

 Parent node: CsvMetaRequest and JsonMetaRequest

 |
    |OverwriteIfExists|Bool| \(Optional\) Recalculates the SelectMeta and overwrites the existing data. The default value is false, which means that the result is directly returned if the Select Meta already exists.

 Child node: None

 Parent node: CsvMetaRequest and JsonMetaRequest

 |
    |CompressionType|Enumeration| \(Optional\) Specifies the compression type of the object. Only None is supported currently.

 Child node: None

 Parent node: InputSerialization

 |
    |RecordDelimiter|String| \(Optional\) Specifies the delimiter, which is base64-encoded and `\n` by default. The value of this element before being encoded can be the ANSI value of two characters in maximum. For example, `\n` is used to indicate a line break in Java code.

 Child node: None

 Parent node: CSV

 |
    |FieldDelimiter|String| \(Optional\) Specifies the delimiter used to separate columns in the CSV file. The value of this element is the base64-encoded ANSI value of a character and is `,` by default. For example, `,` is used to indicate a comma in Java code.

 Child node: None

 Parent node: CSV \(input and output\)

 |
    |QuoteCharacter|String| \(Optional\) Specifies the quote characters used in the CSV file. The value of this element is base64-encoded and is `\”` by default. In a CSV file, line breaks and column delimiters are processed as normal characters. The value of this element before being encoded must be the ANSI value of a character. For example, `\”` is used to indicate a quote character in Java code.

 Child node: None

 Parent node: CSV \(input\)

 |
    |CSV|Container| Specifies the format of the input CSV file.

 Child node: RecordDelimiter, FieldDelimiter, and QuoteCharacter

 Parent node: InputSerialization

 |
    |JSON|Container| Specifies the format of the input JSON file.

 Child node: Type

 Parent node: InputSerialization

 |
    |Type|Enumeration| Specifies the type of the input JSON file.

 Valid value: LINES

 |

    Similar to SelectObject, the results for CreateSelectObjectMeta is also returned as frames, which have two types: continuous frames and end meta frames. Continuous frames used for CreateSelectObjectMeta is the same as those used for SelectObject.

    |Frame type|Frame-Type value|Payload format|Description|
    |:---------|:---------------|:-------------|:----------|
    |Meta End Frame \(CSV\)|8388614| offset | status| splits count | rows count | columns count | error message

 <-8 bytes\><--4bytes\><--4 bytes--\><--8 bytes\><--4 bytes---\><variable size\>

 | offset: A 8-bit integer that indicates the offset when the scanning is complete.

 status: A 4-bit integer that indicates the final status of the operation.

 splits\_count: A 4-bit integer that indicates the number of splits.

 rows\_count: A 8-bit integer that indicates the total number of rows.

 cols\_count: A 4-bit integer that indicates the total number of columns.

 error\_message: Includes detailed error messages. If no error occurs, the value of this parameter is null.

 Meta End Frame: Used to report the final status of a CreateSelectObjectMeta operation.

 |
    |Meta End Frame \(JSON\)|8388615| offset | status| splits count | rows count | error message

 <-8 bytes\><--4bytes\><--4 bytes--\><--8 bytes\><variable size\>

 | offset: A 8-bit integer that indicates the offset when the scanning is complete.

 status: A 4-bit integer that indicates the final status of the operation.

 splits\_count: A 4-bit integer that indicates the number of splits.

 rows\_count: A 8-bit integer that indicates the total number of rows.

 error\_message: Includes detailed error messages. If no error occurs, the value of this parameter is null.

 Meta End Frame: Used to report the final status of a CreateSelectObjectMeta operation.

 |

    Response Header: No specified header is included in the response.

-   Example requests
    -   Example request \(CSV\)

        ```
        POST /oss-select/bigcsv_normal.csv?x-oss-process=csv%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
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

    -   Example request \(JSON\)

        ```
        POST /oss-select/sample.json?x-oss-process=json%2Fmeta HTTP/1.1
        Date: Fri, 25 May 2018 23:06:41 GMT
        Content-Type:
        Authorization: OSS AccessKeySignature
        User-Agent: aliyun-sdk-dotnet/2.8.0.0(windows 16.7/16.7.0.0/x86;4.0.30319.42000)
        Content-Length: 309
        Expect: 100-continue
        Connection: keep-alive
        Host: Host
        
        <?xml version="1.0"?>
        <JsonMetaRequest>
        	<InputSerialization>
        		<JSON>
        			<Type>LINES</Type>
        		</JSON>
        	</InputSerialization>
        	<OverwriteIfExisting>false</OverwriteIfExisting>
        </JsonMetaRequest>
        ```


## Supported time format {#section_vn3_3bf_2fb .section}

You can transfer a string in the formats listed in the following table into a timestamp without specifying the time format. For example, the string cast­\('20121201' as timestamp\) is automatically resolved into a timestamp: 1st, December, 2012.

The following table describes the time formats that can be automatically recognized and transferred.

|Format|Description|
|:-----|:----------|
|YYYYMMDD|year month day|
|YYYY/MM/DD|year/month/day|
|DD/MM/YYYY/|day/month/year|
|YYYY-MM-DD|year-month-day|
|DD-MM-YY|day-month-year|
|DD.MM.YY|day.month.year|
|HH:MM:SS.mss|hour:minute:second.millisecond|
|HH:MM:SS|hour:minute:second|
|HH MM SS mss|hour minute second millisecond|
|HH.MM.SS.mss|hour.minute.second.millisecond|
|HHMM|hour second|
|HHMMSSmss|hour minute second millisecond|
|YYYYMMDD HH:MM:SS.mss|year month day hour:minute:second.millisecond|
|YYYY/MM/DD HH:MM:SS.mss|year/month/day hour:minute:second.millisecond|
|DD/MM/YYYY HH:MM:SS.mss|day/month/year hour:minute:second.millisecond|
|YYYYMMDD HH:MM:SS|year month day hour:minute:second|
|YYYY/MM/DD HH:MM:SS|year/month/day hour:minute:second|
|DD/MM/YYYY HH:MM:SS|day/month/year hour:minute:second|
|YYYY-MM-DD HH:MM:SS.mss|year-month-day hour:minute:second.millisecond|
|DD-MM-YYYY HH:MM:SS.mss|day-month-year hour:minute:second.millisecond|
|YYYY-MM-DD HH:MM:SS|year-month-day hour:minute:second|
|YYYYMMDDTHH:MM:SS|year month day T hour:minute:second|
|YYYYMMDDTHH:MM:SS.mss|year month day T hour:minute:second.millisecond|
|DD-MM-YYYYTHH:MM:SS.mss|day-month-year T hour:minute:second.millisecond|
|DD-MM-YYYYTHH:MM:SS|day-month-year T hour:minute:second|
|YYYYMMDDTHHMM|year month day T hour minute|
|YYYYMMDDTHHMMSS|year month day T hour minute second|
|YYYYMMDDTHHMMSSMSS|year month day T hour minute second millisecond|
|ISO8601-0| year-month-day T hour:minute+hour:minute, or year-month-day T hour: minute-hour:minute

 "+" indicates that the time in the current timezone is in front of standard UTC time."-" indicates that the time in the current timezone is behind the stand UTC time. In this format, ISO8601-0 can be used to indicate "+".

 |
|ISO8601-1| year-month-day T hour:minute+hour:minute, or year-month-day T hour: minute-hour:minute

 "+" indicates that the time in the current timezone is in front of standard UTC time."-" indicates that the time in the current timezone is behind the stand UTC time. In this format, ISO8601-1 can be used.

 |
|CommonLog|Such as 28/Feb/2017:12:30:51 +0700|
|RFC822|Such as Tue, 28 Feb 2017 12:30:51 GMT|
|?D/?M/YY|day/month/year, in which the day and month can be a 1-bit or 2-bit number.|
|?D/?M/YY ?H:?M|day month year hour:minute, in which the day, month, hour, and minute can be a 1-bit or 2-bit number.|
|?D/?M/YY ?H:?M:?S|day month year hour:minute:second, in which the day, month, hour, minute, and second can be a 1-bit or 2-bit number.|

The formats in the following table are ambiguous. You must specify a time format when using strings in these formats. For example, the cast\('20121201' as timestamp format 'YYYYDDMM'\) statement incorrectly resolves the string 20121201 to 12nd, January, 2012.

|Format|Description|
|:-----|:----------|
|YYYYDDMM|year day month|
|YYYY/DD/MM|year/day/month|
|MM/DD/YYYY|month/day/year|
|YYYY-DD-MM|year-day-month|
|MM-DD-YYYY|month-day-year|
|MM.DD.YYYY|month.day.year|

## ErrorCode {#section_qb_rs_ngb1 .section}

SelectObject returns Errorcodes in the following two methods:

-   Include the HTTP status code in the response headers and include error messages in the response body, which is the same as other OSS requests. An ErrorCode returned in this way indicates that an obvious input or data error \(such as an invalid SQL statement is input\) occurs.
-   Include the Error code in the end frame of the response body. An ErrorCode returned in this way indicates that the data is not correct or does not match the SQL statement. For example, a string exists in a column of which the type is specified as integer in the SQL statement. In this case, a part of data is processed and returned to the client, and the status code is 206.

Some ErrorCodes \(such as InvalidCSVLine\) can be returned as the HTTP status code in the response header or the status code included in the end frame according to the location of the error row in the CSV file.

|ErrorCode|Description|HTTP status code|Http status code in end frame|
|---------|-----------|----------------|-----------------------------|
|InvalidSqlParameter|Invalid SQL parameter.Indicates that the SQL statement in the request is null, the SQL statement size exceeds the limit, or the SQL statement is not base64-encoded.

|400|None|
|InvalidInputFieldDelimiter|Invalid column delimiter in the input CSV file.Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 1 after being decoded.

|400|None|
|InvalidInputRecordDelimiter|Invalid row delimiter in the input CSV file. Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 2 after being decoded.|400|None|
|InvalidInputQuote|Invalid quote character in the input CSV file. Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 1 after being decoded.|400|None|
|InvalidOutputFieldDelimiter|Invalid column delimiter in the output CSV file. Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 1 after being decoded.|400|None|
|InvalidOutputRecordDelimiter|Invalid row delimiter in the output CSV file. Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 2 after being decoded.|400|None|
|UnsupportedCompressionFormat|Invalid Compression parameter. Indicates that the value of the parameter is not NONE or GZIP \(case-insensitive\).|400|None|
|InvalidCommentCharacter|Invalid comment character in the CSV file. Indicates that the parameter is not base64-encoded or the size of the parameter is larger than 1 after being decoded.|400|None|
|InvalidRange|Invalid Range parameter. Indicates that the parameter is not prefixed with `line-range=` or `split-range=`, or the range value does not meet the HTTP standard for Range.|400|None|
|DecompressFailure|Indicates that the value of Compression is GZIP and the decompression fails.|400|None|
|InvalidMaxSkippedRecordsAllowed|Indicates that the value of MaxSkippedRecordsAllowed is not an integer.|400|None|
|SelectCsvMetaUnavailable|Indicates that CreateSelectObjectMeta is firstly called when the Range parameter is specified but the target object does not include CSV Meta.|400|None|
|InvalidTextEncoding|Indicates that the object is not UTF-8 encoded.|400|None|
|InvalidOSSSelectParameters|Indicates the EnablePayloadCrc and OutputRawData parameters are both set to true, which results in conflicts.|400|None|
|InternalError|Indicates that an OSS system error occurs.|500 or 206|None or 500|
|SqlSyntaxError|Indicates that the syntax of the base64-decoded SQL statement is incorrect.|400|None|
|SqlExceedsMaxInCount|Indicates that the number of values included in the IN statement exceeds 1,024.|400|None|
|SqlExceedsMaxColumnNameLength|Indicates that the size of the column name exceeds 1,024.|400|None|
|SqlInvalidColumnIndex|Indicates that the column index in the SQL statement is smaller than 1 or larger than 100.|400|None|
|SqlAggregationOnNonNumericType|Indicates that an aggregation function is used in a non-numeric column.|400|None|
|SqlInvalidAggregationOnTimestamp|Indicates that the SUM/AVG aggregation function is used in the timestamp column.|400|None|
|SqlValueTypeOfInMustBeSame|Indicates that values of different types are included in the IN statement.|400|None|
|SqlInvalidEscapeChar|Indicates that escape characters in the LIKE statement is "?", "%", or "\*".|400|None|
|SqlOnlyOneEscapeCharIsAllowed|Indicates that the size of the escape character in the LIKE statement is larger than 1.|400|None|
|SqlNoCharAfterEscapeChar|Indicates that there are no character after the escape character in the LIKE statement.|400|None|
|SqlInvalidLimitValue|Indicates that the number after the Limit statement is smaller than 1.|400|None|
|SqlExceedsMaxWildCardCount|Indicates that the number of wildcards \("\*" or "%"\) exceeds the limit in the LIKE statement.|400|None|
|SqlExceedsMaxConditionCount|Indicates that the number of conditional expressions in the Where statement exceeds the limit.|400|None|
|SqlExceedsMaxConditionDepth|Indicates that the depth of the conditional tree in the Where statement exceeds the limit.|400|None|
|SqlOneColumnCastToDifferentTypes|Indicates that a column is casted to different types in the SQL statement.|400|None|
|SqlOperationAppliedToDifferentTypes|Indicates that an operator is used for two objects of different type in the SQL statement. For example, this ErrorCode is returned if the col1 in `_col1 > 3` is a string.|400|None|
|SqlInvalidColumnName|Indicates that a column name used in the SQL statement is not included in the header of the CSV file.|400|None|
|SqlNotSupportedTimestampFormat|Indicates that the timestamp format specified in the CAST statement is not supported.|400|None|
|SqlNotMatchTimestampFormat|Indicates that the timestamp format specified in the CAST statement does not match the timestamp string.|400|None|
|SqlInvalidTimestampValue|Indicates that no timestamp format is specified in the CAST statement and the provided timestamp string cannot be casted into a timestamp.|400|None|
|SqlInvalidLikeOperand|Indicates that the left column in the LIKE statement is not column names of column indexes, the left column in the LIKE statement is not the string type, or the right column in the LIKE statement is the string type.|400|None|
|SqlInvalidMixOfAggregationAndColumn|Indicates that the SQL statement includes the column names and indexes of aggregation functions and non-aggregation functions at the same time.|400|None|
|SqlExceedsMaxAggregationCount|Indicates that the number of aggregation functions included in the SQL statement exceeds the limit.|400|None|
|SqlInvalidMixOfStarAndColumn|Indicates that the wildcard "\*", column name, and column index are included in the SQL statement at the same time.|400|None|
|SqlInvalidKeepAllColumnsWithAggregation|Indicates that the SQL statement includes aggregation functions while the KeepAllColumns parameter is set to true.|400|None|
|SqlInvalidKeepAllColumnsWithDuplicateColumn|Indicates that the SQL statement include repeated column names or indexes while the KeepAllColumns parameter is set to true.|400|None|
|SqlInvalidSqlAfterAnalysis|Indicates that the SQL statement is not supported because it is too complicated after being resolved.|400|None|
|InvalidArithmeticOperand|Indicates that arithmetical operations are performed on non-numeric constants or columns in the SQL statement.|400|None|
|SqlInvalidAndOperand|Indicates that the type of expressions connected by AND in the SQL statement is not bool.|400|None|
|SqlInvalidOrOperand|Indicates that the type of expressions connected by OR in the SQL statement is not bool.|400|None|
|SqlInvalidNotOperand|Indicates that the type of expressions connected by NOT in the SQL statement is not bool.|400|None|
|SqlInvalidIsNullOperand|Indicates that the IS NULL operation is performed on a constant in the SQL statement.|400|None|
|SqlComparerOperandTypeMismatch|Indicates that the SQL statement compares two objects of different types.|400|None|
|SqlInvalidConcatOperand|Indicates that two constants are connected by the string connect operator \(||\) in the SQL statement.|400|None|
|SqlUnsupportedSql|Indicates that the SQL statement is too complicated so that the size of the generated SQL plan exceeds the limit.|400|None|
|HeaderInfoExceedsMaxSize|Indicates that the size of header information specified in the SQL statement exceeds the limit.|400|None|
|OutputExceedsMaxSize|Indicates that a single row of output results exceeds the limit size.|400|None|
|InvalidCsvLine|Indicates that a row in the CSV file is invalid \(including that the size of the row exceeds the limit\) or the number of ignored rows exceeds the value of MaxSkippedRecordsAllowed.|206 or 400|400 or none|
|NegativeRowIndex|Indicates that the value of the array Index in the SQL statement is a negative number.|400|None|
|ExceedsMaxNestedColumnDepth|Indicates that the nested levels of the JSON file in the SQL statement exceeds the limit.|400|None|
|NestedColumnNotSupportInCsv|Indicates that the nested attributes \(including array "\[\]" or "."\) are not supported in the CSV file in the SQL statement.|400|None|
|TableRootNodeOnlySupportInJson|Indicates that the root node path can be specified after From ossobject only in JSON files.|400|None|
|JsonNodeExceedsMaxSize|Indicates that the size of the root node in the JSON file exceeds the limit.|400 or 206|None or 400|
|InvalidJsonData|Indicates that the JSON data is invalid \(incorrect format\).|400 or 206|None or 400|
|ExceedsMaxJsonArraySize|Indicates that the number of elements in an array in the root node of the JSON file exceeds the limit.|400 or 206|None or 400|
|WildCardNotAllowed|Indicates that the wildcard \(\*\) in the cannot be used in the JSON file in select or where statements. For example, the following statement is not supported: `select s.a.b[*] from ossobject where a.c[*] > 0`.|400|None|
|JsonNodeExceedsMaxDepth|Indicates that the depth of the root node of the JSON file exceeds the limit.|400 or 206|None or 400|

