This document describes how to access and debug GME client APIs for the voice message and speech-to-text services for Unity.

## Key Considerations for Using GME

GME provides the real-time voice, voice message, and speech-to-text services, which all depend on core APIs such as `Init` and `Poll`.

#### Key notes

- You have created a GME application and obtained the `AppID` and `Key` of the SDK as instructed in [Activating Services](https://intl.cloud.tencent.com/document/product/607/10782).
- You have **activated the real-time voice, voice message, and speech-to-text services of GME** as instructed in [Activating Services](https://intl.cloud.tencent.com/document/product/607/10782).
- Configure your project before using GME; otherwise, the SDK will not take effect.
- After a GME API is called successfully, `QAVError.OK` will be returned with the value being 0.
- GME APIs should be called in the same thread.
- The `Poll` API should be called periodically for GME to trigger event callbacks.
- For detailed error code, please see <dx-tag-link link="https://intl.cloud.tencent.com/document/product/607/33223" tag="ErrorCode">Error Codes</dx-tag-link>.

## Connecting to the SDK

### Directions

Key processes involved in SDK connection are as follows:

<img src="https://qcloudimg.tencent-cloud.cn/raw/c8758a24fe68fc084b8d12b09de5e27a.jpg"  width="70%" /></img>

<dx-steps>
-<dx-tag-link link="#Init" tag="API: Init">Initializing GME</dx-tag-link>
-<dx-tag-link link="#Poll" tag="API: Poll">Calling Poll periodically to trigger event callbacks</dx-tag-link>
-<dx-tag-link link="#ApplyPtt" tag="API: ApplyPTTAuthbuffer">Initializing authentication</dx-tag-link>
-<dx-tag-link link="#StartRWSR" tag="API: StartRecordingWithStreamingRecognition">Starting streaming speech recognition</dx-tag-link>
-<dx-tag-link link="#Stop" tag="API: StopRecording">Stop recording</dx-tag-link>
-<dx-tag-link link="#UnInit" tag="API: UnInit">Uninitializing GME</dx-tag-link>
</dx-steps>

### C# classes

| Class | Description |
| ----------- | :----------------------: |
| ITMGContext | Key APIs |
| ITMGPTT     | Voice message and speech-to-text conversion APIs |

## Key APIs

| API | Description |
| ------ | :----------: |
| Init | Initializes GME |
| Poll | Triggers event callback |
| Pause | Pauses the system |
| Resume | Resumes the system |
| Uninit | Uninitializes GME |

### Importing the header file

```
using TencentMobileGaming;
```

### Getting an instance

Get the `Context` instance by using the `ITMGContext` method instead of `QAVContext.GetInstance()`.

[](id:Init)
### Initializing the SDK

**You need to initialize the SDK through the `Init` API** before you can use the real-time voice, voice message, and speech-to-text services. The `Init` API must be called in the same thread as other APIs. We recommend you call all APIs in the main thread.

#### API prototype

```
//class ITMGContext
public abstract int Init(string sdkAppID, string openID);
```

| Parameter     |  Type  | Description                                                         |
| -------- | :----: | ------------------------------------------------------------ |
| sdkAppId | string | `AppID` provided in the [GME console](https://console.cloud.tencent.com/gamegme), which can be obtained as instructed in [Activating Services](https://intl.cloud.tencent.com/document/product/607/10782#.E9.87.8D.E7.82.B9.E5.8F.82.E6.95.B0). |
| openID   | string | `openID` can only be in `Int64` type, which is passed in after being converted to a string. You can customize its rules, and it must be unique in the application. To pass in `openID` as a string, [submit a ticket](https://console.cloud.tencent.com/workorder/category?level1_id=438&level2_id=445&source=0&data_title=%E6%B8%B8%E6%88%8F%E5%A4%9A%E5%AA%92%E4%BD%93%E5%BC%95%E6%93%8EGME&step=1) for application. |

#### Returned values

| Returned Value | Description |
| ------------------------------- | --------------------------------------------- |
| QAVError.OK= 0 | Initialized SDK successfully |
| AV_ERR_SDK_NOT_FULL_UPDATE=7015 | Checks whether the SDK file is complete. It is recommended to delete it and then import the SDK again. |

<dx-alert infotype="notice" title="Notes on 7015 error code">

- The 7015 error code is judged by md5. If this error is reported during integration, please check the integrity and version of the SDK file as prompted.
- The returned value `AV_ERR_SDK_NOT_FULL_UPDATE` is **only a reminder** but will not cause an initialization failure.
- Due to the third-party reinforcement, Unity packaging mechanism and other factors, the md5 of the library file will be affected, resulting in misjudgment. **Please ignore this error in the logic for official release**, and try to avoid displaying it in the UI.
  </dx-alert>

#### Sample code 

```
int ret = ITMGContext.GetInstance().Init(sdkAppId, openID);
// Determine whether the initialization is successful by the returned value
if (ret != QAVError.OK)
    {
        Debug.Log("SDK initialization failed:"+ret);
        return;
    }
```

[](id:Poll)
### Triggering an event callback

Event callbacks can be triggered by periodically calling the `Poll` API in `update`. The `Poll` API is GME's message pump and should be called periodically for GME to trigger event callbacks; otherwise, the entire SDK service will run abnormally. For more information, see the `EnginePollHelper` file in [SDK Download Guide](https://intl.cloud.tencent.com/document/product/607/18521).

<dx-alert infotype="alarm" title="Calling the `Poll` API periodically">
The `Poll` API must be called periodically and in the main thread to avoid abnormal API callbacks.
</dx-alert>

#### API prototype

```
ITMGContext public abstract int Poll();
```

#### Sample code

```
public void Update()
    {
        ITMGContext.GetInstance().Poll();
    }
```

### Pausing the system

When a `Pause` event occurs in the system, the engine should also be notified for pause. For example, when the application switches to the background (OnApplicationPause, isPause=True), and you do not need the background to play back the audio in the room, please call `Pause` API to pause the GME service.

#### API prototype

```
ITMGContext public abstract int Pause()
```

### Resuming the system

When a `Resume` event occurs in the system, the engine should also be notified for resumption. The `Resume` API only supports resuming voice chat.

#### API prototype

```
ITMGContext  public abstract int Resume()
```

[](id:UnInit)
### Uninitializing the SDK

This API is used to uninitialize the SDK to make it uninitialized. **If the game business account is bound to `openid`, switching game account requires uninitializing GME and then using the new `openid` to initialize again.**

#### API prototype

```
ITMGContext public abstract int Uninit()
```

## Voice Message and Speech-to-Text Services

<dx-alert infotype="explain" title="">

- The speech-to-text service consists of fast recording-to-text conversion and streaming speech-to-text conversion.
- You do not need to enter a voice chat room when using the voice message service.
- The maximum recording duration of a voice message is 58 seconds by default, and the minimum recording duration cannot be less than one second. If you want to customize the recording duration, for example, to change the maximum recording duration to ten seconds, call the `SetMaxMessageLength` API to set it after initialization.
  </dx-alert>

### Flowchart for using the voice message service

<img src="https://qcloudimg.tencent-cloud.cn/raw/24c16c71cb2fcc6cf170a6b481067564.jpg" width="80%">

### Flowchart for using the speech-to-text service

<img src="https://qcloudimg.tencent-cloud.cn/raw/8269f413756379d574a2969ac8da0158.jpg" width="80%">

| API | Description |
| ------------------- | :------------------: |
|GenAuthBuffer| Generates the local authentication key |
| ApplyPTTAuthbuffer  | Initializes authentication |
| SetMaxMessageLength | Specifies the maximum length of voice message |




### Generating the local authentication key

Generate `AuthBuffer` for encryption and authentication of relevant features. For release in the production environment, please use the backend deployment key as detailed in [Authentication Key](https://intl.cloud.tencent.com/document/product/607/12218).    

#### API prototype

```
QAVAuthBuffer GenAuthBuffer(int appId, string roomId, string openId, string key)
```

| Parameter   |  Type  | Description                                                         |
| ------ | :----: | ------------------------------------------------------------ |
| appId | int | `AppId` from the Tencent Cloud console.|
| roomId | string | Enter `null` or an empty string                                      |
| openId | string | User ID, which is the same as `OpenId` during initialization. |
| key | string | Permission key from the Tencent Cloud [console](https://console.cloud.tencent.com/gamegme). |

### Application authentication

After the authentication information is generated, the authentication is assigned to the SDK.  

#### API prototype  

```
ITMGPTT int ApplyPTTAuthbuffer (byte[] authBuffer)
```

| Parameter       |  Type  | Description                    |
| ---------- | :----: | ---- |
| authBuffer | byte[] | Authentication |

#### Sample code

```
UserConfig.SetAppID(transform.Find ("appId").GetComponent<InputField> ().text);
UserConfig.SetUserID(transform.Find ("userId").GetComponent<InputField> ().text);
UserConfig.SetAuthKey(transform.Find("authKey").GetComponent<InputField>().text);
byte[] authBuffer = UserConfig.GetAuthBuffer(UserConfig.GetAppID(), UserConfig.GetUserID(), null,UserConfig.GetAuthKey());
ITMGContext.GetInstance ().GetPttCtrl ().ApplyPTTAuthbuffer(authBuffer);
```

### Specifying the maximum duration of voice message

This API is used to specify the maximum duration of a voice message, which can be up to 58 seconds.

#### API prototype

```
ITMGPTT int SetMaxMessageLength(int msTime)
```

| Parameter   | Type | Description                                            |
| ------ | :--: | ----------------------------------------------- |
| msTime | int  | Audio duration in ms. Value range: 1000 < msTime <= 58000 |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().SetMaxMessageLength(58000); 
```

## Streaming Speech Recognition



### Voice message and speech-to-text APIs

| API | Description |
| -------------------------------------- | :----------: |
| StartRecordingWithStreamingRecognition | Starts streaming recording |
| StopRecording                          |   Stops recording   |


[](id:StartRWSR)
### Starting streaming speech recognition

This API is used to start streaming speech recognition. Text obtained from speech-to-text conversion will be returned in real time in its callback. It can specify a language for recognition or translate the information recognized in speech into a specified language and return the translation. **To stop recording, call <dx-tag-link link="#Stop" tag="API: StopRecording"> Stop recording</dx-tag-link>**.

#### API prototype  

```
ITMGPTT int StartRecordingWithStreamingRecognition(string filePath)
ITMGPTT int StartRecordingWithStreamingRecognition(string filePath, string speechLanguage,string translateLanguage) 
```

| Parameter | Type | Description |
| ----------------- | :----: | ------------------------------------------------------------ |
| filePath | String | Path of stored audio file |
| speechLanguage    | String | The language in which the audio file is to be converted to text. For parameters, see [Language Parameter Reference List](https://intl.cloud.tencent.com/document/product/607/30260). |
| translateLanguage | String | The language into which the audio file is to be translated. For parameters, see [Language Parameter Reference List](https://intl.cloud.tencent.com/document/product/607/30260). |

#### Sample code  

```
string recordPath = Application.persistentDataPath + string.Format("/{0}.silk", sUid++);
int ret = ITMGContext.GetInstance().GetPttCtrl().StartRecordingWithStreamingRecognition(recordPath, "cmn-Hans-CN","cmn-Hans-CN");
```

> ! Translation incurs additional fees. For more information, see [Purchase Guide](https://intl.cloud.tencent.com/document/product/607/50009).

### Callback for streaming speech recognition

After streaming speech recognition is started, you need to listen on callback messages in the `OnStreamingSpeechComplete` or `OnStreamingSpeechisRunning` notification, which is as detailed below:

- `OnStreamingSpeechComplete` returns text after the recording is stopped and the recognition is completed, which is equivalent to returning the recognized text after a paragraph of speech.
- `OnStreamingSpeechisRunning` returns the recognized text in real time during the recording, which is equivalent to returning the recognized text while speaking.

The event message will be identified in the `OnEvent` notification as needed and contains the following four parameters:

| Parameter | Description |
| --------- | :---------------------------------------------: |
| result | Return code used to determine whether the streaming speech recognition is successful |
| text      |            Text converted from speech             |
| file_path |               The local path of the stored recording file                |
| file_id | Backend URL address of recording file, which will be retained for 90 days |



<dx-alert infotype="notice" title="">
The file_id is empty when the 'ITMG_MAIN_EVNET_TYPE_PTT_STREAMINGRecognition_IS_RUNNING' message is listened.
</dx-alert>



#### Error codes

| Error Code | Description | Suggested Solution |
| ------ | :--: | :------: |
| 32775 | Streaming speech-to-text conversion failed, but recording succeeded.| Call the `UploadRecordedFile` API to upload the recording file and then call the `SpeechToText` API to perform speech-to-text conversion. |
| 32777 | Streaming speech-to-text conversion failed, but recording and upload succeeded.| The message returned contains a backend URL after successful upload. Call the `SpeechToText` API to perform speech-to-text conversion. |
| 32786 | Streaming speech-to-text conversion failed. | During streaming recording, wait for the execution result of the streaming recording API to return. |
| 32787     | Speech-to-text conversion succeeded, but the text translation service was not activated.         | Activate the text translation service in the console.                                 |
| 32788     | Speech-to-text conversion succeeded, but the language parameter of the text translation service was invalid.         | Check the parameter passed in.                                 |

If the error code 4098 is reported, see [Speech-to-text Conversion](https://intl.cloud.tencent.com/document/product/607/39716) for solutions.

#### Sample code  

```
			// Listen on an event:
			ITMGContext.GetInstance().GetPttCtrl().OnStreamingSpeechComplete +=new QAVStreamingRecognitionCallback (OnStreamingSpeechComplete);
			ITMGContext.GetInstance().GetPttCtrl().OnStreamingSpeechisRunning += new QAVStreamingRecognitionCallback (OnStreamingRecisRunning);
			// Process the event listened on:
			void OnStreamingSpeechComplete(int code, string fileid, string filepath, string result){
					// Callback for streaming speech recognition
			}

			void OnStreamingRecisRunning(int code, string fileid, string filePath, string result){
					if (code == 0)
					{
						setBtnText(mStreamBtn, "Streaming");
						InputField field = transform.Find("recordFilePath").GetComponent<InputField>();
						field.text = filePath;

						field = transform.Find("downloadUrl").GetComponent<InputField>();
						field.text = "Stream is Running";

						field = transform.Find("convertTextResult").GetComponent<InputField>();
						field.text = result;
						showWarningText("Recording");
					}	
}
```



## Voice Message Recording

**The recording process is as follows: Start recording > stop recording > return the recording callback > start the next recording.**



### Voice message and speech-to-text APIs

| API | Description |
| --------------- | :------: |
| StartRecording  | Starts recording |
| PauseRecording  | Pauses recording |
| ResumeRecording | Resumes recording |
| StopRecording   | Stops recording |
| CancelRecording | Cancels recording |



### Starting recording

This API is used to start recording.

#### API prototype  

```
ITMGPTT int StartRecording(string fileDir)
```

| Parameter | Type | Description |
| ------- | :----: | -------------- |
| fileDir | string | Path of stored audio file |

#### Sample code  

```
string recordPath = Application.persistentDataPath + string.Format ("/{0}.silk", sUid++);
int ret = ITMGContext.GetInstance().GetPttCtrl().StartRecording(recordPath);
```

[](id:Stop)
### Stopping recording

This API is used to stop recording. It is async, and a callback for recording completion will be returned after recording stops. A recording file will be available only after recording succeeds.

#### API prototype  

```
ITMGPTT int StopRecording()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().StopRecording();
```

### Callback for recording start

A callback will be executed through a delegate function to pass a message when recording is completed.

**To stop recording, call `StopRecording`**. The callback for recording start will be returned after the recording is stopped.

#### API prototype  

```
public delegate void QAVRecordFileCompleteCallback(int code, string filepath); 
public abstract event QAVRecordFileCompleteCallback OnRecordFileComplete;
```

| Parameter     |  Type  | Description                                                         |
| -------- | :----: | ------------------------------------------------------------ |
| code | string | 0: Recording is completed |
| filepath | string | Path of stored recording file, which must be accessible and cannot be the `fileid` |

#### Error codes

| Error Code Value | Cause | Suggested Solution |
| -------- | ------------------------ | ------------------------------------------------------------ |
| 4097 | Parameter is empty. | Check whether the API parameters in the code are correct. |
| 4098 | Initialization error. | Check whether the device is being used, whether the permissions are normal, and whether the initialization is normal. |
| 4099 | Recording is in progress. | Ensure that the SDK recording feature is used at the right time. |
| 4100 | Audio data is not captured. | Check whether the mic is working properly. |
| 4101 | An error occurred while accessing the file during recording. | Ensure the existence of the file and the validity of the file path. |
| 4102 | The mic is not authorized. | Mic permission is required for using the SDK. To add the permission, please see the SDK project configuration document for the corresponding engine or platform. |
| 4103 | The recording duration is too short. | The recording duration should be in ms and longer than 1,000 ms. |
| 4104 | No recording operation is started. | Check whether the recording starting API has been called. |

#### Sample code  

```
// Listen on an event
ITMGContext.GetInstance().GetPttCtrl().OnRecordFileComplete +=  new QAVRecordFileCompleteCallback (OnRecordFileComplete);
// Process the event listened on
void OnRecordFileComplete(int code, string filepath){
    // Callback for recording start
}
```

### Pausing recording

This API is used to pause recording. If you want to resume recording, please call the `ResumeRecording` API.

#### API prototype  

```
ITMGPTT int PauseRecording()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().PauseRecording();
```

### Resuming recording

This API is used to resume recording.

#### API prototype  

```
ITMGPTT int ResumeRecording()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().ResumeRecording();
```



### Canceling recording

This API is used to cancel recording. **There is no callback after cancellation**.

#### API prototype  

```
ITMGPTT int CancelRecording()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().CancelRecording();
```





## Voice Message Upload, Download, and Playback

| API | Description |
| -------------------- | :------------: |
| UploadRecordedFile   |  Uploads an audio file  |
| DownloadRecordedFile |  Downloads an audio file  |
| PlayRecordedFile     |    Plays back an audio file    |
| StopPlayFile         |  Stops playing back an audio file  |
| GetFileSize          | Gets the audio file size |
| GetVoiceFileDuration | Gets the audio file duration |

### Uploading an audio file

This API is used to upload an audio file.

#### API prototype  

```
ITMGPTT int UploadRecordedFile (string filePath)
```

| Parameter | Type | Description |
| -------- | :----: | -------------------------------- |
| filePath | String | Path of uploaded audio file, which is a local path. |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().UploadRecordedFile(filePath);
```

### Callback for audio file upload completion

A callback will be executed through a delegate function to pass a message when the upload of audio file is completed.

#### API prototype

```
public delegate void QAVUploadFileCompleteCallback(int code, string filepath, string fileid);
public abstract event QAVUploadFileCompleteCallback OnUploadFileComplete; 
```

| Parameter | Type | Description |
| -------- | :----: | ----------------------- |
| code | int | 0: recording is completed |
| filepath | string | Path of stored recording file |
| fileid | string | File URL path |

#### Error codes

| Error Code Value | Cause | Suggested Solution |
| -------- | ------------------------------ | ------------------------------------------------------ |
| 8193 | An error occurred while accessing the file during upload. | Ensure the existence of the file and the validity of the file path. |
| 8194 | Signature verification failed. | Check whether the authentication key is correct and whether the voice message and speech-to-text feature is initialized. |
| 8195 | A network error occurred. | Check whether the device can access the internet. |
| 8196 | The network failed while getting the upload parameters. | Check whether the authentication is correct and whether the device can access the internet. |
| 8197 | The packet returned during the process of getting the upload parameters is empty. | Check whether the authentication is correct and whether the device network can normally access the internet. |
| 8198 | Failed to decode the packet returned during the process of getting the upload parameters. | Check whether the authentication is correct and whether the device can access the internet. |
| 8200 | No `appinfo` is set. | Check whether the `apply` API is called or whether the input parameters are empty. |

#### Sample code

```
// Listen on an event
ITMGContext.GetInstance().GetPttCtrl().OnUploadFileComplete +=new QAVUploadFileCompleteCallback (OnUploadFileComplete);
// Process the event listened on
void OnUploadFileComplete(int code, string filepath, string fileid){
    // Callback for audio file upload completion
}
```

### Downloading the audio file

This API is used to download an audio file.

#### API prototype  

```
ITMGPTT DownloadRecordedFile (string fileID, string downloadFilePath)
```

| Parameter | Type | Description |
| ---------------- | :----: | ------------------------------------------------------------ |
| fileID           | String | File URL |
| downloadFilePath | String | Local path of saved file, which must be accessible and cannot be the `fileid`                    |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().DownloadRecordedFile(fileId, filePath);
```

### Callback for audio file download completion

A callback will be executed through a delegate function to pass a message when the download of audio file is completed.

#### API prototype  

```
public delegate void QAVDownloadFileCompleteCallback(int code, string filepath, string fileid);
public abstract event QAVDownloadFileCompleteCallback OnDownloadFileComplete;
```

| Parameter     |  Type  | Description                                    |
| -------- | :----: | --------------------------------------- |
| code | int | 0: recording is completed |
| filepath | string | Path of stored recording file |
| fileid | string | URL path of file, which will be retained on the server for 90 days |

#### Error codes

| Error Code Value | Cause | Suggested Solution |
| -------- | --------------------------------- | ------------------------------------------------------------ |
| 12289 | An error occurred while accessing the file during download. | Check whether the file path is valid. |
| 12290 | Signature verification failed. | Check whether the authentication key is correct and whether the voice message and speech-to-text feature is initialized. |
| 12291    | Network storage system exception. | The server failed to get the audio file. Check whether the API parameter `fileid` is correct, whether the network is normal, and whether the file exists in COS. |
| 12292 | Server file system error. | Check whether the device can access the internet and whether the file exists on the server. |
| 12293 | The HTTP network failed during the process of getting the download parameters. | Check whether the device can access the internet. |
| 12294 | The packet returned during the process of getting the download parameters is empty. | Check whether the device can access the internet. |
| 12295 | Failed to decode the packet returned during the process of getting the download parameters. | Check whether the device can access the internet. |
| 12297 | No `appinfo` is set. | Check whether the authentication key is correct and whether the voice message and speech-to-text feature is initialized. |

#### Sample code

```
// Listen on an event
ITMGContext.GetInstance().GetPttCtrl().OnDownloadFileComplete +=new QAVDownloadFileCompleteCallback(OnDownloadFileComplete);
// Process the event listened on
void OnDownloadFileComplete(int code, string filepath, string fileid){
    // Callback for audio file download completion
}
```

### Playing back audio

This API is used to play back audio.

#### API prototype  

```
ITMGPTT PlayRecordedFile(string filePath)
ITMGPTT PlayRecordedFile(string filePath,int voiceType);
```

| Parameter      |  Type  | Description                                                         |
| --------- | :----: | ------------------------------------------------------------ |
| filePath | string | Local audio file path |
| voicetype |  int   | Voice changing type, please see [Voice Changing Effects](https://intl.cloud.tencent.com/document/product/607/44995) |

#### Error codes

| Error Code Value | Cause | Suggested Solution |
| -------- | ---------- | ------------------------------ |
| 20485 | Playback is not started. | Ensure the existence of the file and the validity of the file path. |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().PlayRecordedFile(filePath); 
```

### Callback for audio playback

A callback will be executed through a delegate function to pass a message when an audio file is played back.

#### API prototype  

```
public delegate void QAVPlayFileCompleteCallback(int code, string filepath);
public abstract event QAVPlayFileCompleteCallback OnPlayFileComplete;
```

| Parameter | Type | Description |
| -------- | :----: | ----------------------- |
| code | int | 0: playback is completed |
| filepath | string | Path of stored recording file |

#### Error codes

| Error Code Value | Cause | Suggested Solution |
| -------- | ---------------------------------------------------------- | ------------------------------------------------------------ |
| 20481 | Initialization error. | Check whether the device is being used, whether the permissions are normal, and whether the initialization is normal. |
| 20482 | During playback, the client tried to interrupt and play back the next one but failed (which should succeed normally). | Check whether the code logic is correct. |
| 20483 | Parameter is empty. | Check whether the API parameters in the code are correct. |
| 20484 | Internal error. | An error occurred while initializing the player. This error code is generally caused by failure in decoding, and the error should be located with the aid of logs. |

#### Sample code

```
// Listen on an event:
ITMGContext.GetInstance().GetPttCtrl().OnPlayFileComplete +=new  QAVPlayFileCompleteCallback(OnPlayFileComplete);
// Process the event listened on:
void OnPlayFileComplete(int code, string filepath){
    // Callback for audio playback
}
```



### Stopping audio playback

This API is used to stop audio playback. There will be a callback for playback completion when the playback stops.

#### API prototype  

```
ITMGPTT int StopPlayFile()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().StopPlayFile();
```


### Getting the audio file size

This API is used to get the size of an audio file.

#### API prototype  

```
ITMGPTT GetFileSize(string filePath) 
```

| Parameter | Type | Description |
| -------- | :----: | -------------------------------- |
| filePath | string | Path of audio file, which is a local path. |

#### Sample code  

```
int fileSize = ITMGContext.GetInstance().GetPttCtrl().GetFileSize(filepath);
```

### Getting the audio file duration

This API is used to get the duration of an audio file in milliseconds.

#### API prototype  

```
ITMGPTT int GetVoiceFileDuration(string filePath)
```

| Parameter | Type | Description |
| -------- | :----: | -------------------------------- |
| filePath | string | Path of audio file, which is a local path. |

#### Sample code  

```
int fileDuration = ITMGContext.GetInstance().GetPttCtrl().GetVoiceFileDuration(filepath);
```





## Fast Recording-to-Text Conversion

| API | Description |
| ------------ | :------------: |
| SpeechToText | Converts speech to text |

### Converting an audio file to text

This API is used to convert a specified audio file to text.

#### API prototype  

```
ITMGPTT int SpeechToText(String fileID)
```

| Parameter | Type | Description |
| ------ | :----: | ------------ |
| fileID | String | Audio file URL |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().SpeechToText(fileID);
```



### Translating an audio file into text in a specified language

This API can specify a language for recognition or translate the information recognized in speech into a specified language and return the translation.

>!Translation incurs additional fees. For more information, see [Purchase Guide](https://intl.cloud.tencent.com/document/product/607/50009).

#### API prototype  

```
ITMGPTT int SpeechToText(String fileID,String speechLanguage)
ITMGPTT int SpeechToText(String fileID,String speechLanguage,String translatelanguage)
```

| Parameter | Type | Description |
| ----------------- | :----: | ------------------------------------------------------------ |
| fileID | String | URL of audio file, which will be retained on the server for 90 days |
| speechLanguage    | String | The language in which the audio file is to be converted to text. For parameters, see [Language Parameter Reference List](https://intl.cloud.tencent.com/document/product/607/30260). |
| translatelanguage | String | The language into which the audio file is to be translated. For language parameters for translation, see [Language Parameter Reference List](https://intl.cloud.tencent.com/document/product/607/30260). |

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().SpeechToText(fileID,"cmn-Hans-CN","cmn-Hans-CN");
```



### Callback for recognition

A callback will be executed through a delegate function to pass a message when a specified audio file is recognized and converted to text.

#### API prototype  

```
public delegate void QAVSpeechToTextCallback(int code, string fileid, string result);
public abstract event QAVSpeechToTextCallback OnSpeechToTextComplete;
```

| Parameter | Type | Description |
| ------ | :----: | ------------------------------------ |
| code | int | 0: recording is completed |
| fileid | string | URL of recording file, which will be retained on the server for 90 days |
| result | string | Converted text |

#### Error codes

| Error Code | Cause | Suggested Solution |
| -------- | ---------------------------------- | ------------------------------------------------------------ |
| 32769 | An internal error occurred. | Analyze logs, get the actual error code returned from the backend to the client, and ask backend personnel for assistance. |
| 32770  | Network failed. | Check whether the device can access the internet. |
| 32772 | Failed to decode the returned packet. | Analyze logs, get the actual error code returned from the backend to the client, and ask backend personnel for assistance. |
| 32774 | No `appinfo` was set. | Check whether the authentication key is correct and whether the voice message and speech-to-text feature is initialized. |
| 32776 | `authbuffer` check failed. | Check whether `authbuffer` is correct. |
| 32784  | Incorrect speech-to-text conversion parameter. | Check whether the API parameter `fileid` in the code is empty. |
| 32785 | Speech-to-text translation returned an error. | Error with the backend of the voice message and speech-to-text feature. Analyze logs, get the actual error code returned from the backend to the client, and ask backend personnel for assistance. |
| 32787     | Speech-to-text conversion succeeded, but the text translation service was not activated.         | Activate the text translation service in the console.                                 |
| 32788     | Speech-to-text conversion succeeded, but the language parameter of the text translation service was invalid.         | Check the parameter passed in.                                 |

#### Sample code

```
// Listen on an event
ITMGContext.GetInstance().GetPttCtrl().OnSpeechToTextComplete += new QAVSpeechToTextCallback(OnSpeechToTextComplete);
// Process the event listened on
void OnSpeechToTextComplete(int code, string fileid, string result){
    // Callback for recognition
}
```

## Voice Message Volume Level APIs

| API | Description |
| ---------------- | :----------------: |
| GetMicLevel      | Gets the real-time mic volume level |
| SetMicVolume     |    Sets the recording volume level    |
| GetMicVolume     |    Gets the recording volume level    |
| GetSpeakerLevel  | Gets the real-time speaker volume level |
| SetSpeakerVolume | Sets playback volume level |
| GetSpeakerVolume | Gets playback volume level |

### Getting the real-time mic volume of a voice message

This API is used to get the real-time mic volume. An int-type value will be returned. Value range: 0-200.

#### API prototype  

```
ITMGPTT int GetMicLevel()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().GetMicLevel();
```

### Setting the recording volume of a voice message

This API is used to set the recording volume of voice message. Value range: 0-200.

#### API prototype  

```
ITMGPTT int SetMicVolume(int vol)
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().SetMicVolume(100);
```

### Getting the recording volume of a voice message

This API is used to get the recording volume of voice message. An int-type value will be returned. Value range: 0-200.

#### API prototype  

```
ITMGPTT int GetMicVolume()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().GetMicVolume();
```

### Getting the real-time speaker volume of a voice message

This API is used to get the real-time speaker volume. An int-type value will be returned. Value range: 0-200.

#### API prototype  

```
ITMGPTT int GetSpeakerLevel()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().GetSpeakerLevel();
```

### Setting the playback volume of a voice message

This API is used to set the playback volume of voice message. Value range: 0-200.

#### API prototype  

```
ITMGPTT int SetSpeakerVolume(int vol)
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().SetSpeakerVolume(100);
```

### Getting the playback volume of a voice message

This API is used to get the playback volume of voice message. An int-type value will be returned. Value range: 0-200.

#### API prototype  

```
ITMGPTT int GetSpeakerVolume()
```

#### Sample code  

```
ITMGContext.GetInstance().GetPttCtrl().GetSpeakerVolume();
```

## Advanced APIs

### Getting the version number

This API is used to get the SDK version number for analysis.

#### API prototype

```
ITMGContext  abstract string GetSDKVersion()
```

#### Sample code  

```
ITMGContext.GetInstance().GetSDKVersion();
```


### Setting the log printing level

This API is used to set the level of logs to be printed, and needs to be called before the initialization. It is recommended to keep the default level.

#### API prototype

```
ITMGContext  SetLogLevel(ITMG_LOG_LEVEL levelWrite, ITMG_LOG_LEVEL levelPrint)
```

#### Parameter description

| Parameter | Type | Description |
| ---------- | -------------- | ------------------------------------------------------------ |
| levelWrite | ITMG_LOG_LEVEL | Sets the level of logs to be written. `TMG_LOG_LEVEL_NONE` indicates not to write. Default value: TMG_LOG_LEVEL_INFO |
| levelPrint | ITMG_LOG_LEVEL | Sets the level of logs to be printed. `TMG_LOG_LEVEL_NONE` indicates not to print. Default value: TMG_LOG_LEVEL_ERROR |

`ITMG_LOG_LEVEL` is as detailed below:

| ITMG_LOG_LEVEL | Description |
| --------------------- | -------------------- |
| TMG_LOG_LEVEL_NONE | Does not print logs |
| TMG_LOG_LEVEL_ERROR | Prints error logs (default) |
| TMG_LOG_LEVEL_INFO | Prints info logs |
| TMG_LOG_LEVEL_DEBUG | Prints debug logs |
| TMG_LOG_LEVEL_VERBOSE | Prints verbose logs |

#### Sample code  

```
ITMGContext.GetInstance().SetLogLevel(TMG_LOG_LEVEL_INFO,TMG_LOG_LEVEL_INFO);
```



### Setting the log printing path

This API is used to set the log printing path. The default path is as follows. It needs to be called before Init.

| OS | Path |
| ------- | ------------------------------------------------------------ |
| Windows | %appdata%\Tencent\GME\ProcessName |
| iOS | Application/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/Documents |
| Android | /sdcard/Android/data/xxx.xxx.xxx/files |
| Mac | /Users/username/Library/Containers/xxx.xxx.xxx/Data/Documents |

#### API prototype

```
ITMGContext  SetLogPath(string logDir)
```

| Parameter | Type | Description |
| ------ | :----: | ---- |
| logDir | String | Path |

#### Sample code  

```
ITMGContext.GetInstance().SetLogPath(path);
```

