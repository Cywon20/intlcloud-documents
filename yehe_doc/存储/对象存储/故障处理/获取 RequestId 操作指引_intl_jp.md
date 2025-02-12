## 概要

Cloud Object Storage（COS）サービスにおいて、毎回のリクエスト送信時に、COSサーバーはリクエストに対し1つのID、すなわちRequestIdを生成します。ここでは、様々なケースにおけるRequestIdの取得方法についてご説明します。

## コンソール上でブラウザを通じて取得

1. [COSコンソール](https://console.cloud.tencent.com/cos5)にログインし、左側ナビゲーションバーの【バケットリスト】をクリックし、バケットリストのページに進みます。
2. アクセスしたいバケットをクリックして進みます。
3. `F12`キーを押し、ブラウザの開発者ツールページに進みます。
4. 開発者ツールの上方の【Network】をクリックします。
![](https://main.qcloudimg.com/raw/0a201a890f54bfabc4267e9c86c89338.png)
5. ダウンロードしたいファイル名の右側の【ダウンロード】をクリックし、開発者ツールページでダウンロードしたいファイル名を入力してフィルタリングを行います。ファイルを選択し、【Headers】をクリックし、**Response Headers**エリアでRequestId情報を取得します。
![](https://main.qcloudimg.com/raw/9da8afe99d7e6c7c5f0b77f3c1c48861.png)

## ファイルアクセス失敗時の取得

ファイルへのアクセスが失敗した際、ページに表示される、返されたXML情報の中から**RequestIdノード情報**を取得できます。
![](https://main.qcloudimg.com/raw/e0d4149121fb0022640465ff690810e1.png)

次の操作を行って取得することもできます。
1. `F12`キーを押し、ブラウザの開発者ツールページに進みます。
2. ページ上方の【Network】をクリックし、Allタイプを選択すると、Response Headersの中からRequestIdフィールド情報を見つけることができます。
![](https://main.qcloudimg.com/raw/ac6902c6ac615a9ec2978a5999a49073.png)

## SDKによる取得

SDKに含まれるインターフェースは非常に多く、すべてのインターフェースの例を挙げることができないため、すべてのSDKについて**ファイルのアップロード**を例にとり、現在の操作のRequestIdの取得方法を示します。

## .NET SDKによる取得

```
try
{
 string bucket = "examplebucket-1250000000"; //バケット、形式：BucketName-APPID
 string cosPath = "test.cs"; // オブジェクトキー
 byte[] data = System.Text.Encoding.Default.GetBytes("Hello COS"); // バイナリーデータ
 PutObjectRequest putObjectRequest = new PutObjectRequest(bucket, cosPath, data);
 
 PutObjectResult result = cosXml.PutObject(putObjectRequest);
 string requestId = result.responseHeaders.GetValueOrDefault("x-cos-request-id")[0];
 Console.WriteLine(requestId);
}
catch (COSXML.CosException.CosClientException clientEx)
{
 //リクエストに失敗しました
 Console.WriteLine("CosClientException: " + clientEx);
}
catch (COSXML.CosException.CosServerException serverEx)
{
 //リクエストに失敗しました
 Console.WriteLine("CosServerException: " + serverEx.GetInfo());
}
```

 

### Go SDKによる取得

```
package main
 
import (
   "context"
   "net/http"
   "net/url"
   "os"
   "strings"
   "github.com/tencentyun/cos-go-sdk-v5"
)
 
func main() {
   // examplebucket-1250000000およびCOS_REGIONを真の情報に変更します
   u, _ := url.Parse("https://examplebucket-1250000000.cos.COS_REGION.myqcloud.com")
   b := &cos.BaseURL{BucketURL: u}
   c := cos.NewClient(b, &http.Client{
       Transport: &cos.AuthorizationTransport{
           SecretID:  "SECRETID",
           SecretKey: "SECRETKEY",
       },
   })
   // オブジェクトキー（Key）はオブジェクトのバケット内での固有識別子です。
   // 例えば、オブジェクトのアクセスドメイン名`examplebucket-1250000000.cos.COS_REGION.myqcloud.com/test.go`の中で、オブジェクトキーはtest.goとなります
   name := "test.go"
   // 1.文字列によってオブジェクトをアップロードします
   f := strings.NewReader("Hello COS")
 
   _, err := c.Object.Put(context.Background(), name, f, nil)
   if err != nil {
       // error情報にはRequestIdフィールドが直接含まれます
       panic(err)
   }
   requestId := response.Header.Get("X-Cos-Request-Id")
   fmt.Println(requestId)
}
```

### Java SDKによる取得

```
// 1 ユーザーID情報（secretId、secretKey）を初期化します。
String secretId = "SECRETID";
String secretKey = "SECRETKEY";
COSCredentials cred = new BasicCOSCredentials(secretId, secretKey);
// 2 bucketのリージョンを設定します。COSリージョンの略称についてはhttps://cloud.tencent.com/document/product/436/6224をご参照ください
// clientConfigにはregion、https(デフォルトではhttp)、タイムアウト、プロキシなどを設定するsetメソッドが含まれます。使用にあたってはソースコードまたはよくあるご質問のJava SDKのパートを参照できます。
Region region = new Region("COS_REGION");
ClientConfig clientConfig = new ClientConfig(region);
// ここではhttpsプロトコルの設定と使用を推奨します
clientConfig.setHttpProtocol(HttpProtocol.https);
// 3 cosクライアントを生成します。
COSClient cosClient = new COSClient(cred, clientConfig);
// Bucketの命名形式はBucketName-APPIDです。ここに入力するバケット名は、この形式でなければなりません
String bucketName = "examplebucket-1250000000";
 
String content = "Hello COS";
String key = "test.java";
PutObjectResult putObjectResult = cosClient.putObject(bucketName, key, content);
String requestId = putObjectResult.getRequestId();
System.out.println(requestId);
```

 

### JavaScript SDKによる取得

```
cos.putObject({
    Bucket: 'examplebucket-1250000000', /* 必須 */
    Region: 'COS_REGION',    /* 必須 */
    Key: 'test.js',              /* 必須 */
    StorageClass: 'STANDARD',
    Body: 'Hello COS',
    onProgress: function(progressData) {
        console.log(JSON.stringify(progressData));
    }
}, function(err, data) {
    var requestId = (err || data).headers['x-cos-request-id'];
    console.log(requestId );
});
```

 

### Node.js SDKによる取得

```
var COS = require('cos-nodejs-sdk-v5');
var cos = new COS({
    SecretId: 'SECRETID',
    SecretKey: 'SECRETKEY'
});
 
cos.putObject({
    Bucket: 'examplebucket-1250000000', /* 必須 */
    Region: 'COS_REGION',    /* 必須 */
    Key: 'test.nodejs',              /* 必須 */
    StorageClass: 'STANDARD',
    Body: Buffer.from('Hello COS'),
    onProgress: function(progressData) {
        console.log(JSON.stringify(progressData));
    }
}, function(err, data) {
    var requestId = (err || data).headers['x-cos-request-id'];
    console.log(requestId );
});
```

### WeixinアプレットSDKによる取得

```
var COS = require('cos-wx-sdk-v5');
var cos = new COS({
    SecretId: 'SECRETID',
    SecretKey: 'SECRETKEY'
});
 
cos.putObject({
    Bucket: 'examplebucket-1250000000', /* 必須 */
    Region: 'COS_REGION',    /* 必須 */
    Key: 'test.js',              /* 必須 */
    StorageClass: 'STANDARD',
    Body: 'Hello COS',
    onProgress: function(progressData) {
        console.log(JSON.stringify(progressData));
    }
}, function(err, data) {
    var requestId = (err || data).headers['x-cos-request-id'];
    console.log(requestId );
});
```

 

### PHP SDKによる取得

```
$secretId = "SECRETID"; //"Tencent Cloud APIキー SecretId";
$secretKey = "SECRETKEY"; //"Tencent Cloud APIキー SecretKey";
$region = "COS_REGION"; //デフォルトのバケットリージョンを設定
$cosClient = new Qcloud\Cos\Client(
   array(
       'region' => $region,
       'schema' => 'https', //プロトコルヘッダー。デフォルトではhttp
       'credentials'=> array(
           'secretId'  => $secretId ,
           'secretKey' => $secretKey)));
# ファイルのアップロード
## putObject(アップロードインターフェース。最大5Gまでのファイルのアップロードをサポート)
### アップロードメモリ内の文字列
try {
   $bucket = "examplebucket-1250000000"; //バケット名 形式：BucketName-APPID
   $key = "test.php"; //ここでのkeyはオブジェクトキーです。オブジェクトキーはオブジェクトのバケット内での固有識別子です
   $result = $cosClient->putObject(array(
       'Bucket' => $bucket,
       'Key' => $key,
       'Body' => 'Hello COS'));
   $requestId = $result['RequestId'];
   print_r($requestId);
} catch (\Exception $e) {
   echo "$e\n";
}
```


### iOS SDKによる取得

```
QCloudCOSXMLUploadObjectRequest* put = [QCloudCOSXMLUploadObjectRequest new];
/** ローカルファイルパスは、URLを必ずfile://で始めてください。形式は次のとおりです。
1. [NSURL URLWithString:@"file:////var/mobile/Containers/Data/Application/DBPF7490-D5U8-4ABF-A0AF-CC49D6A60AEB/Documents/exampleobject"]
2. [NSURL fileURLWithPath:@"/var/mobile/Containers/Data/Application/DBPF7490-D5U8-4ABF-A0AF-CC49D6A60AEB/Documents/exampleobject"]
*/
NSURL* url = [NSURL fileURLWithPath:@"ファイルのURL"];
// バケット名はBucketName-Appidで構成され、COSコンソールで確認できます。 https://console.cloud.tencent.com/cos5/bucket
put.bucket = @"examplebucket-1250000000";
// オブジェクトキーは、オブジェクトのCOS上での完全パスです。ディレクトリがある場合は、形式は「video/xxx/movie.mp4」です
put.object = @"exampleobject";
// アップロードする必要があるオブジェクトの内容です。NSData*またはNSURL*タイプの変数を渡すことができます
put.body =  url;
// アップロードの進捗を監視します
[put setSendProcessBlock:^(int64_t bytesSent,
                           int64_t totalBytesSent,
                           int64_t totalBytesExpectedToSend) {
    //      bytesSent                 今回送信する必要があるバイト数（大容量ファイルは数回に分けての送信が必要な可能性があります）
    //      totalBytesSent            送信済みバイト数
    //      totalBytesExpectedToSend  今回のアップロードで送信する必要がある総バイト数（すなわち1ファイルのサイズ）
}];
// アップロードの結果を監視します
[put setFinishBlock:^(QCloudUploadObjectResult *result, NSError *error) {
    // requestidを取得します
   [result.__originHTTPURLResponse__.allHeaderFields objectForKey:@"x-cos-request-id"]
}];
[put setInitMultipleUploadFinishBlock:^(QCloudInitiateMultipartUploadResult *
                                        multipleUploadInitResult,
                                        QCloudCOSXMLUploadObjectResumeData resumeData) {
    // マルチパートアップロードの初期化が完了すると、このblockがコールバックされ、ここでresumeData、uploadidが取得できるようになります
    NSString* uploadId = multipleUploadInitResult.uploadId;
}];
[[QCloudCOSTransferMangerService defaultCOSTransferManager] UploadObject:put];
```

