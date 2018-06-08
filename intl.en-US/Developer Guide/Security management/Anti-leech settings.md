# Anti-leech settings {#concept_s5b_gjd_5db .concept}

OSS collects service fees based on use. To prevent users’ data on OSS from being leeched, OSS supports anti-leech based on the field referer in the HTTP header. 

You can log on to the [OSS console](https://oss.console.aliyun.com/) or use APIs to configure a referer whitelist for a bucket or whether to allow access by requests where referer is blank. 

 For example, for a bucket named oss-example, set its referer whitelist to `http://www.aliyun.com/`. Then, only requests with a referer of `http://www.aliyun.com/` can access the objects in the bucket.

## Detail analysis {#section_tnx_kjd_5db .section}

-   Anti-leech verification is performed only when users access objects through URL signatures or anonymously. When the request header contains the “Authorization” field, anti-leech verification is not performed.
-   A bucket supports multiple referer fields,  which are separated by the Enter key on the console, and by the comma “,” on API.
-   The referer field supports the wildcard “\*“ and “?”.
-   Users can set whether to allow access requests with empty referer fields.
-   When the whitelist is empty, the system checks if the referer field is null \(otherwise, all requests get rejected\).
-   When the whitelist is not empty and the rules do not allow null referer fields, only requests with referers in the whitelist are allowed. Other requests \(including null referer requests\) are then rejected.
-   If the whitelist is not empty and the rules allow empty referer fields, requests with empty referer and with the referers in the whitelist are allowed. Other requests get rejected.
-   The three bucket permissions \(private, public-read, and public-read-write\) check the referer field.

## Wildcard details: {#section_emg_mjd_5db .section}

-   Asterisk “”: The asterisk can be used to represent 0 or multiple characters.  If you are looking for an object name prefixed with AEW but have forgotten the remaining part, you can enter AEW\* to search for all types of files starting with AEW, such as AEWT.txt, AEWU.EXE and AEWI.dll.   If you want to narrow down the search scope, you can enter AEW\*.txt to search for all .txt files starting with AEW, such as AEWIP.txt and AEWDF.txt.
-   Question mark “?”: The question mark can be used to represent one character.  If you enter love?, all types of files starting with love and ending with one character get displayed, such as lovey and lovei.   If you want to narrow the search scope, you can enter love?.doc to search for all .doc files starting with love and ending with one character, such as lovey.doc and loveh.doc.

## Reference {#section_rv3_4jd_5db .section}

-   API：[Put Bucket Referer](../intl.en-US/API Reference/Bucket operations/Put Bucket Referer.md#)
-   Console: [Set Anti-leech](../intl.en-US/Console User Guide/Manage buckets/Set anti-leech.md#)

