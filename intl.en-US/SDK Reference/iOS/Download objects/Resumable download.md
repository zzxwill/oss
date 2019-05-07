# Resumable download {#concept_llq_qpb_3gb .concept}

When a client downloads resources over a network, the download is interrupted for some reason. In this case, you can use resumable download to continue downloading the uncompleted parts. This method saves time and traffic.

Scenario: When you use a video application to download a video on a mobile phone, and the network switches from Wi-Fi to Cellular during the download, the application automatically interrupts the download. When you switch back to Wi-Fi, you can manually restart the download task to resume the download.

The following figure shows the effect of resumable download.

![](images/35699_en-US.gif)

The following figure shows the process of resumable download.

![](images/35698_en-US.png)

## Detail analysis {#section_v1x_lmc_3gb .section}

-   The Range field is added to the HTTP 1.1 header to specify the scope within which to obtain data. The formats of the Range field value are as follows:
    -   `Range: bytes=100-`: starts from the 101st Byte until the download is complete.
    -   `Range: bytes=100-200`: starts the download from the 101st Byte and stops the download at the 201st Byte. The Range field value is counted from 0 by default.`` This method is typically used for multipart transmission of large objects, such as videos.
    -   `Range: bytes =-100`: The start position is not specified. The server downloads the last 100 Bytes, instead of the 100 Bytes starting from 0.
    -   `Range: bytes=0-100, 200-300`: downloads content within multiple ranges at the same time. This is untypical.
-   During resumable download, the server requires to use the `If-Match` value in the header to verify whether the object on the server changes. The `If-Match` field value corresponds to the `ETag` value.
-   When the client initiates a request, the `Range` and `If-Match` field values are included in the request. The OSS server verifies the ETag value that corresponds to the If-Match field value. If the received ETag value does not match the ETag value saved in OSS, OSS returns the 412 precondition status code.
-   The OSS server currently supports the following fields for GetObject: `Range`, `If-Match`, `If-None-Match`, `If-Modified-Since`, `If-Unmodified-Since`. Therefore, you can use resumable download to download resources from OSS on your mobile device.

## Specific implementation {#section_lhv_nmc_3gb .section}

Example:

**Note:** The following code is for reference only. We recommend that you do not use it in production projects. To download the code, you can download it by yourself or use third-party open-source download software.

```
#import "DownloadService.h"
#import "OSSTestMacros.h"

@implementation DownloadRequest

@end

@implementation Checkpoint

- (instancetype)copyWithZone:(NSZone *)zone {
    Checkpoint *other = [[[self class] allocWithZone:zone] init];
    
    other.etag = self.etag;
    other.totalExpectedLength = self.totalExpectedLength;
    
    return other;
}

@end


@interface DownloadService()<NSURLSessionTaskDelegate, NSURLSessionDataDelegate>

@property (nonatomic, strong) NSURLSession *session;         //The network session.
@property (nonatomic, strong) NSURLSessionDataTask *dataTask;   //The data request task.
@property (nonatomic, copy) DownloadFailureBlock failure;    //The request error.
@property (nonatomic, copy) DownloadSuccessBlock success;    //The request succeeds.
@property (nonatomic, copy) DownloadProgressBlock progress;  //The download progress.
@property (nonatomic, copy) Checkpoint *checkpoint;        //The checkpoint file.
@property (nonatomic, copy) NSString *requestURLString;    //The object resource URL used in a download request.
@property (nonatomic, copy) NSString *headURLString;       //The object resource URL used in a HEAD request.
@property (nonatomic, copy) NSString *targetPath;     //The path to the object.
@property (nonatomic, assign) unsigned long long totalReceivedContentLength; //The size of the downloaded content.
@property (nonatomic, strong) dispatch_semaphore_t semaphore;

@end

@implementation DownloadService

- (instancetype)init
{
    self = [super init];
    if (self) {
        NSURLSessionConfiguration *conf = [NSURLSessionConfiguration defaultSessionConfiguration];
        conf.timeoutIntervalForRequest = 15;
        
        NSOperationQueue *processQueue = [NSOperationQueue new];
        _session = [NSURLSession sessionWithConfiguration:conf delegate:self delegateQueue:processQueue];
        _semaphore = dispatch_semaphore_create(0);
        _checkpoint = [[Checkpoint alloc] init];
    }
    return self;
}

+ (instancetype)downloadServiceWithRequest:(DownloadRequest *)request {
    DownloadService *service = [[DownloadService alloc] init];
    if (service) {
        service.failure = request.failure;
        service.success = request.success;
        service.requestURLString = request.sourceURLString;
        service.headURLString = request.headURLString;
        service.targetPath = request.downloadFilePath;
        service.progress = request.downloadProgress;
        if (request.checkpoint) {
            service.checkpoint = request.checkpoint;
        }
    }
    return service;
}

/**
 * Use HeadObject to obtain Object Meta. Compare the ETag value saved in OSS with that saved in the local checkpoint file. The result is returned.
 */
- (BOOL)getFileInfo {
    __block BOOL resumable = NO;
    NSURL *url = [NSURL URLWithString:self.headURLString];
    NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:url];
    [request setHTTPMethod:@"HEAD"];
    
    NSURLSessionDataTask *task = [[NSURLSession sharedSession] dataTaskWithRequest:request completionHandler:^(NSData * _Nullable data, NSURLResponse * _Nullable response, NSError * _Nullable error) {
        if (error) {
            cNSLog(@"Failed to obtain Object Meta. error: %@", error);
        } else {
            NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *)response;
            NSString *etag = [httpResponse.allHeaderFields objectForKey:@"Etag"];
            if ([self.checkpoint.etag isEqualToString:etag]) {
                resumable = YES;
            } else {
                resumable = NO;
            }
        }
        dispatch_semaphore_signal(self.semaphore);
    }];
    [task resume];
    
    dispatch_semaphore_wait(self.semaphore, DISPATCH_TIME_FOREVER);
    return resumable;
}

/**
 * Obtain the size of the local file.
 */
- (unsigned long long)fileSizeAtPath:(NSString *)filePath {
    unsigned long long fileSize = 0;
    NSFileManager *dfm = [NSFileManager defaultManager];
    if ([dfm fileExistsAtPath:filePath]) {
        NSError *error = nil;
        NSDictionary *attributes = [dfm attributesOfItemAtPath:filePath error:&error];
        if (! error && attributes) {
            fileSize = attributes.fileSize;
        } else if (error) {
            NSLog(@"error: %@", error);
        }
    }

    return fileSize;
}

- (void)resume {
    NSURL *url = [NSURL URLWithString:self.requestURLString];
    NSMutableURLRequest *request = [[NSMutableURLRequest alloc]initWithURL:url];
    [request setHTTPMethod:@"GET"];
    
    BOOL resumable = [self getFileInfo];    // If the resumable field value is NO, resumable upload is unfeasible. Otherwise, continue the resumable upload logic.
    if (resumable) {
        self.totalReceivedContentLength = [self fileSizeAtPath:self.targetPath];
        NSString *requestRange = [NSString stringWithFormat:@"bytes=%llu-", self.totalReceivedContentLength];
        [request setValue:requestRange forHTTPHeaderField:@"Range"];
    } else {
        self.totalReceivedContentLength = 0;
    }
    
    if (self.totalReceivedContentLength == 0) {
        [[NSFileManager defaultManager] createFileAtPath:self.targetPath contents:nil attributes:nil];
    }
    
    self.dataTask = [self.session dataTaskWithRequest:request];
    [self.dataTask resume];
}

- (void)pause {
    [self.dataTask cancel];
    self.dataTask = nil;
}

- (void)cancel {
    [self.dataTask cancel];
    self.dataTask = nil;
    [self removeFileAtPath: self.targetPath];
}

- (void)removeFileAtPath:(NSString *)filePath {
    NSError *error = nil;
    [[NSFileManager defaultManager] removeItemAtPath:self.targetPath error:&error];
    if (error) {
        NSLog(@"remove file with error : %@", error);
    }
}

#pragma mark - NSURLSessionDataDelegate

- (void)URLSession:(NSURLSession *)session task:(NSURLSessionTask *)task
didCompleteWithError:(nullable NSError *)error {
    NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *)task.response;
    if ([httpResponse isKindOfClass:[NSHTTPURLResponse class]]) {
        if (httpResponse.statusCode == 200) {
            self.checkpoint.etag = [[httpResponse allHeaderFields] objectForKey:@"Etag"];
            self.checkpoint.totalExpectedLength = httpResponse.expectedContentLength;
        } else if (httpResponse.statusCode == 206) {
            self.checkpoint.etag = [[httpResponse allHeaderFields] objectForKey:@"Etag"];
            self.checkpoint.totalExpectedLength = self.totalReceivedContentLength + httpResponse.expectedContentLength;
        }
    }
    
    if (error) {
        if (self.failure) {
            NSMutableDictionary *userInfo = [NSMutableDictionary dictionaryWithDictionary:error.userInfo];
            [userInfo oss_setObject:self.checkpoint forKey:@"checkpoint"];
            
            NSError *tError = [NSError errorWithDomain:error.domain code:error.code userInfo:userInfo];
            self.failure(tError);
        }
    } else if (self.success) {
        self.success(@{@"status": @"success"});
    }
}

- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask didReceiveResponse:(NSURLResponse *)response completionHandler:(void (^)(NSURLSessionResponseDisposition))completionHandler
{
    NSHTTPURLResponse *httpResponse = (NSHTTPURLResponse *)dataTask.response;
    if ([httpResponse isKindOfClass:[NSHTTPURLResponse class]]) {
        if (httpResponse.statusCode == 200) {
            self.checkpoint.totalExpectedLength = httpResponse.expectedContentLength;
        } else if (httpResponse.statusCode == 206) {
            self.checkpoint.totalExpectedLength = self.totalReceivedContentLength +  httpResponse.expectedContentLength;
        }
    }
    
    completionHandler(NSURLSessionResponseAllow);
}

- (void)URLSession:(NSURLSession *)session dataTask:(NSURLSessionDataTask *)dataTask didReceiveData:(NSData *)data {
    
    NSFileHandle *fileHandle = [NSFileHandle fileHandleForWritingAtPath:self.targetPath];
    [fileHandle seekToEndOfFile];
    [fileHandle writeData:data];
    [fileHandle closeFile];
    
    self.totalReceivedContentLength += data.length;
    if (self.progress) {
        self.progress(data.length, self.totalReceivedContentLength, self.checkpoint.totalExpectedLength);
    }
}

@end
```

The preceding sample code shows the process logic when data is received over the network. The fields in the code are described as follows:

-   DownloadService: the core of the download logic.
-   URLSession:dataTask:didReceiveData: The received network data is appended to the object. The download progress record is updated.
-   URLSession:task:didCompleteWithError: determines whether the download task is complete, and then returns the result to the upper layer business.
-   URLSession:dataTask:didReceiveResponse:completionHandler: processes object-relevant information, such as the ETag value for the precheck of resumable download and the content-length field value to calculate the download progress.

DownloadRequest is defined as follows:

```
#import <Foundation/Foundation.h>

typedef void(^DownloadProgressBlock)(int64_t bytesReceived, int64_t totalBytesReceived, int64_t totalBytesExpectToReceived);
typedef void(^DownloadFailureBlock)(NSError *error);
typedef void(^DownloadSuccessBlock)(NSDictionary *result);

@interface Checkpoint : NSObject<NSCopying>

@property (nonatomic, copy) NSString *etag;     // The ETag value of the resource.
@property (nonatomic, assign) unsigned long long totalExpectedLength;    //The total size of the object.

@end

@interface DownloadRequest : NSObject

@property (nonatomic, copy) NSString *sourceURLString;      // The URL for the download.

@property (nonatomic, copy) NSString *headURLString;        // The URL used to obtain the original information of the object.

@property (nonatomic, copy) NSString *downloadFilePath;     // The local path to the object.

@property (nonatomic, copy) DownloadProgressBlock downloadProgress; // The download progress.

@property (nonatomic, copy) DownloadFailureBlock failure;   // The callback that is sent after the download fails.

@property (nonatomic, copy) DownloadSuccessBlock success;   // The callback that is sent after the download succeeds.

@property (nonatomic, copy) Checkpoint *checkpoint;         // The checkpoint file that stores the ETag value of the object.

@end


@interface DownloadService : NSObject

+ (instancetype)downloadServiceWithRequest:(DownloadRequest *)request;

/**
 * Start the download.
 */
- (void)resume;

/**
 * Pause the download.
 */
- (void)pause;

/**
 * Cancel the download.
 */
- (void)cancel;

@end
```

The invocation code for the upper-layer business is as follows:

```
- (void)initDownloadURLs {
    OSSPlainTextAKSKPairCredentialProvider *pCredential = [[OSSPlainTextAKSKPairCredentialProvider alloc] initWithPlainTextAccessKey:OSS_ACCESSKEY_ID secretKey:OSS_SECRETKEY_ID];
    _mClient = [[OSSClient alloc] initWithEndpoint:OSS_ENDPOINT credentialProvider:pCredential];
    
    // Generate the signed URL for the GET request.
    OSSTask *downloadURLTask = [_mClient presignConstrainURLWithBucketName:@"aliyun-dhc-shanghai" withObjectKey:OSS_DOWNLOAD_FILE_NAME withExpirationInterval:1800];
    [downloadURLTask waitUntilFinished];
    _downloadURLString = downloadURLTask.result;
    
    // Generate the signed URL for the HEAD request.
    OSSTask *headURLTask = [_mClient presignConstrainURLWithBucketName:@"aliyun-dhc-shanghai" withObjectKey:OSS_DOWNLOAD_FILE_NAME httpMethod:@"HEAD" withExpirationInterval:1800 withParameters:nil];
    [headURLTask waitUntilFinished];
    
    _headURLString = headURLTask.result;
}

- (IBAction)resumeDownloadClicked:(id)sender {
    _downloadRequest = [DownloadRequest new];
    _downloadRequest.sourceURLString = _downloadURLString;       // Set the resource URL.
    _downloadRequest.headURLString = _headURLString;
    NSString *documentPath = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES).firstObject;
    _downloadRequest.downloadFilePath = [documentPath stringByAppendingPathComponent:OSS_DOWNLOAD_FILE_NAME];   //Set the local path to the object you want to download.
    
    __weak typeof(self) wSelf = self;
    _downloadRequest.downloadProgress = ^(int64_t bytesReceived, int64_t totalBytesReceived, int64_t totalBytesExpectToReceived) {
        // totalBytesReceived is the number of Bytes already cached by the client. totalBytesExpectToReceived is the total number of Bytes that need to be downloaded.
        dispatch_async(dispatch_get_main_queue(), ^{
            __strong typeof(self) sSelf = wSelf;
            CGFloat fProgress = totalBytesReceived * 1.f / totalBytesExpectToReceived;
            sSelf.progressLab.text = [NSString stringWithFormat:@"%. 2f%%", fProgress * 100];
            sSelf.progressBar.progress = fProgress;
        });
    };
    _downloadRequest.failure = ^(NSError *error) {
        __strong typeof(self) sSelf = wSelf;
        sSelf.checkpoint = error.userInfo[@"checkpoint"];
    };
    _downloadRequest.success = ^(NSDictionary *result) {
        NSLog(@"The download succeeds.");
    };
    _downloadRequest.checkpoint = self.checkpoint;
    
    NSString *titleText = [[_downloadButton titleLabel] text];
    if ([titleText isEqualToString:@"download"]) {
        [_downloadButton setTitle:@"pause" forState: UIControlStateNormal];
        _downloadService = [DownloadService downloadServiceWithRequest:_downloadRequest];
        [_downloadService resume];
    } else {
        [_downloadButton setTitle:@"download" forState: UIControlStateNormal];
        [_downloadService pause];
    }
}

- (IBAction)cancelDownloadClicked:(id)sender {
    [_downloadButton setTitle:@"download" forState: UIControlStateNormal];
    [_downloadService cancel];
}
```

**Note:** The checkpoint file can be obtained from the failure callback when the download is paused or cancelled. When you restart the download, you can upload the checkpoint file to DownloadRequest. Then, DownloadService uses the checkpoint file to verify the consistency.

