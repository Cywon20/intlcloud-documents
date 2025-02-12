## Feature Overview

The APK dynamic packaging plugin recognizes certain parameters in request URLs of end users and dynamically writes the information carried in such parameters to the APK on a CDN node for dynamic packaging. This feature requires a COS bucket as the origin.

> ? Currently, only nodes in the Chinese mainland support the APK dynamic packaging feature.

## Use Cases

- You need to release and promote an app through various channels, such as app market, ad alliance, search engine, and performance-based ad, but don't want to manually maintain an overwhelming number of channel packages.
- You want to implement hot app startup; that is, you can dynamically insert a link into the APK, so that end users will be automatically redirected to the specified page when they open the app for the first time.

## How to Use

1. Log in to the CDN console, enter the **Plugin Center**, and configure the APK dynamic packaging plugin.
2. Upload the original APK to the specified upload directory on the COS origin.
3. Wait for the CDN node to process the parent package.
4. An end user initiates a request with certain parameters.
5. The CDN node returns the APK file after dynamic packaging.

![]()

## Configuration Guide

### Step 1. Add a dynamic packaging configuration

Log in to the [CDN console](https://console.cloud.tencent.com/cdn), select **Plugin Center** on the left sidebar, and enable the APK dynamic packaging plugin feature to enter the configuration page. After enabling this feature, you can click **Basic configurations** and **Usage Statistics** at the bottom of the page to enter the configuration list page and usage statistics page respectively. 
<img src="https://qcloudimg.tencent-cloud.cn/raw/fd709b1f30cdc70c35598ad0cf4c0f87.png" width="80%">

- Bucket: Select **COS Origin**. To use this feature, you must select a COS bucket in **Beijing, Shanghai, Guangzhou, or Chengdu** region as the origin.
- Domain Name: The system will automatically select all domain names using the specified COS bucket as the origin. The domain names are used to release the APK, and you can add or remove domain names as needed.
- Upload Directory: Set the upload directory for the original APK in COS. Manually upload the original APK to this directory after completing the configuration.
- Output Directory: Set the output directory for the parent package processed by the CDN node. After you upload the original APK, a parent package with the same name is generated and uploaded to this directory. Note that in the request URL, client requests should point to this directory instead of the upload directory.
- Signature Method: Currently, you can dynamically package an APK with Android APK Signature Scheme v1 or v2. A v2 signature supports open-source multi-channel packaging solutions such as Walle and VasDolly. In addition, you can also write a comment to the specified custom block ID.
- Channel Parameter: It is fixed at `comment` and cannot be modified.
- SCF Configuration: After authorization, the system will automatically generate a function based on the configuration.

> !Do not delete this function in the SCF console.

### Step 2. Upload the original APK

Log in to the [COS console](https://console.cloud.tencent.com/cos5) and upload the original APK to the specified upload directory.

> ?The original APK can be up to 10 GB in size.

### Step 3. Wait for the parent package to be processed

On the APK dynamic packaging feature page, click **Parent Package Status** to view the progress of parent package processing by the CDN node. 
![](https://qcloudimg.tencent-cloud.cn/raw/154470582a94d93695d667a496d9726b.png)

### Step 4. Release the dynamic packaging URL

After the parent package is processed, you can release the dynamic packaging URL as follows:

```
Bucket: example.cos.ap-chengdu.myqcloud.com
Domain Name: www.example.com
Upload Directory: /src
Output Directory: /ext
Channel Parameter: comment
Channel Name: pipeline
APK Name: test.apk
```

Then, the URL to be released is `https://www.example.com/ext/test.apk?comment=pipeline`.

An end user can request this URL to get the APK containing the `pipeline` channel information from the CDN node.







