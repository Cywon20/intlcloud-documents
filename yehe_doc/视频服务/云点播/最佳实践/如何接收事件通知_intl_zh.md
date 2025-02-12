## 使用须知

### Demo 功能介绍

本文以一个视频的上传、转码流程为例，向开发者展示云点播（VOD）[事件通知机制](https://intl.cloud.tencent.com/document/product/266/33948)  的使用方法。

### 架构和流程

Demo 基于云函数（SCF） 搭建了一个 HTTP 服务，用于接收来自 VOD 的事件通知请求。该服务通过对 NewFileUpload（[视频上传完成事件通知](https://intl.cloud.tencent.com/document/product/266/33950)）和 ProcedureStateChanged（[任务流状态变更](https://intl.cloud.tencent.com/document/product/266/33953)）的处理，实现发起视频转码和获取转码结果。

系统主要涉及四个组成部分：控制台、API 网关、云函数和云点播，其中 API 网关和云函数即是本 Demo 的部署对象，如下图所示：
<img src="https://qcloudimg.tencent-cloud.cn/raw/ec5a27df9988417547c884df4ac28816.png" width="550">

具体业务流程为：

1. 在控制台上传一个视频到 VOD。
2. VOD 后台发起 NewFileUpload 事件通知请求给 Demo。
3. Demo 解析事件通知内容，调用 VOD 的 [ProcessMedia](https://intl.cloud.tencent.com/document/product/266/34125) 接口对刚上传的视频发起转码，使用的转码模板为 [系统预置模板](https://intl.cloud.tencent.com/document/product/266/33932) 100010和100020。
4. VOD 完成转码任务后，发起 ProcedureStateChanged 事件通知请求给 Demo。
5. Demo 解析事件通知内容，将转码输出文件的 URL 打印到 SCF 日志中。

> ?Demo 中的 SCF 代码使用 Python3.6 进行开发，此外 SCF 还支持 Python2.7、Node.js、Golang、PHP 和 Java 等多种编程语言，开发者可以根据情况自由选择，具体请参考 [SCF 开发指南](https://intl.cloud.tencent.com/document/product/583/11061)。

### 费用

本文提供的云点播事件通知接收服务 Demo 是免费开源的，但在搭建和使用的过程中可能会产生以下费用：

- 购买腾讯云云服务器（CVM）用于执行服务部署脚本，详见 [CVM 计费](https://intl.cloud.tencent.com/document/product/213/2180)。
- 使用 SCF 提供签名派发服务，详见 [SCF 计费](https://intl.cloud.tencent.com/document/product/583/12284) 和 [SCF 免费额度](https://intl.cloud.tencent.com/document/product/583/12282)。
- 使用腾讯云 API 网关为 SCF 提供外网接口，详见 [API 网关计费](https://intl.cloud.tencent.com/document/product/628/11771)。
- 消耗 VOD 存储用于存储上传的视频，详见 [存储计费](https://intl.cloud.tencent.com/document/product/266/14666#.E5.AA.92.E8.B5.84.E5.AD.98.E5.82.A8.3Cspan-id.3D.22media_storage.22.3E.3C.2Fspan.3E)。
- 消耗 VOD 转码时长用于对视频进行转码，详见 [转码计费](https://intl.cloud.tencent.com/document/product/266/14666#.E5.AA.92.E8.B5.84.E5.A4.84.E7.90.86.3Cspan-id.3D.22media_edit.22.3E.3C.2Fspan.3E)。

<span id="p0"></span>
### 避免影响生产环境

事件通知接收服务 Demo 的业务逻辑使用到 VOD 事件通知机制，因此部署过程中需要开发者配置事件通知地址。如果该账号已有基于 VOD 的生产环境，那么变更事件通知地址可能造成业务异常。**操作前请务必确认不会影响生产环境，如果您无法确定，请更换一个全新账号来部署 Demo**。

## 快速部署事件通知接收服务
<span id="p1"></span>
### 步骤1：准备腾讯云 CVM

部署脚本需要运行在一台腾讯云 CVM 上，要求如下：

- 地域：任意。
- 机型：官网最低配置（1核1GB）即可。
- 公网：需要拥有公网 IP，带宽1Mbps或以上。
- 操作系统：官网公共镜像`Ubuntu Server 16.04.1 LTS 64位`或`Ubuntu Server 18.04.1 LTS 64位`。

购买 CVM 的方法请参见 [操作指南 - 创建实例](https://intl.cloud.tencent.com/document/product/213/4855)。重装系统的方法请参见 [操作指南 - 重装系统](https://intl.cloud.tencent.com/document/product/213/4933)。

>!
>- 事件通知接收服务 Demo 本身并不依赖于 CVM，仅使用 CVM 来执行部署脚本。
>- 如果您没有符合上述条件的腾讯云 CVM，也可以在其它带外网的 Linux（如 CentOS、Debian 等）或 Mac 机器上执行部署脚本，但需根据操作系统的区别修改脚本中的个别命令，具体修改方式请开发者自行搜索。
<span id="p2"></span>
### 步骤2：开通云点播

请参考 [快速入门 - 步骤1](https://intl.cloud.tencent.com/document/product/266/8757) 开通云点播服务。
<span id="p3"></span>
### 步骤3：获取 API 密钥和 APPID

事件通知接收服务 Demo 的部署和运行过程需要使用到开发者的 API 密钥（即 SecretId 和 SecretKey）和 APPID。

- 如果还未创建过密钥，请参见 [创建密钥文档](https://intl.cloud.tencent.com/document/product/598/34228) 生成新的 API 密钥；如果已创建过密钥，请参见 [查看密钥文档](https://intl.cloud.tencent.com/document/product/598/34228) 获取 API 密钥。
- 在控制台 [账号信息](https://console.cloud.tencent.com/developer) 页面可以查看 APPID，如下图所示：
  ![](https://main.qcloudimg.com/raw/6c9fe4238232392c8d914f9ebf0f53aa.png)
<span id="p4"></span>
### 步骤4：部署事件通知接收服务

登录 [步骤1准备的 CVM](#p1)（登录方法详见 [操作指南 - 登录 Linux](https://intl.cloud.tencent.com/document/product/213/5436)），在远程终端输入以下命令并运行：

```
ubuntu@VM-69-2-ubuntu:~$ export SECRET_ID=AKxxxxxxxxxxxxxxxxxxxxxxx; export SECRET_KEY=xxxxxxxxxxxxxxxxxxxxx;export APPID=125xxxxxxx;git clone https://github.com/tencentyun/vod-server-demo.git ~/vod-server-demo; bash ~/vod-server-demo/installer/callback_scf.sh
```

>?请将命令中的 SECRET_ID、SECRET_KEY 和 APPID 赋值为 [步骤3](#p3) 中获取到的内容。

该命令将从 Github 下载 Demo 源码并自动执行安装脚本。安装过程需几分钟（具体取决于 CVM 网络状况），期间远程终端会打印如下示例的信息：

```
[2020-06-05 17:16:08]开始检查npm。
[2020-06-05 17:16:12]npm 安装成功。
[2020-06-05 17:16:12]开始安装 ServerLess。
[2020-06-05 17:16:13]serverless 安装成功。
[2020-06-05 17:16:14]开始部署云点播事件通知接收服务。
[2020-06-05 17:16:24]云点播事件通知接收服务部署完成。
[2020-06-05 17:16:26]服务地址：https://service-xxxxxxxx-125xxxxxxx.gz.apigw.tencentcs.com/release/callback
```

复制输出日志中的事件通知接收服务地址（示例中的`https://service-xxxxxxxx-125xxxxxxx.gz.apigw.tencentcs.com/release/callback`）。

> !如果输出日志中出现如下所示的警告，一般是由于 CVM 无法立即解析刚部署好的服务域名，可尝试忽略该警告。
> ```
> [2020-04-25 17:18:44]警告：事件通知接收服务测试不通过。
> ```

### 步骤5：配置事件通知地址

如 [避免影响生产环境](#p0) 一节所述，操作之前请先确认您的线上业务不依赖于 VOD 事件通知。

登录 [云点播控制台](https://console.cloud.tencent.com/vod/callback)，单击【设置】，回调模式选择【普通回调】，回调 URL 填写 [步骤4](#p4) 中获得的事件通知接收服务地址，回调事件全部勾选，然后单击【确定】。如下图所示：
![](https://qcloudimg.tencent-cloud.cn/raw/0e5631bc9013b6e30283a7ac5411e332.png)

>!如果您在控制台同时看到两个回调 URL 设置（2.0版本格式和3.0版本格式），请填写3.0版本。

### 步骤6：测试 Demo

按照 [上传视频 - 本地上传步骤](https://intl.cloud.tencent.com/document/product/266/33890) 的说明，上传一个测试视频到云点播，注意上传过程选择默认的【只上传，暂不进行视频处理】。上传完成后，在“已上传”标签页可以看到该视频的状态为“处理中”，说明 Demo 接收到了 NewFileUpload 事件通知并发起了转码请求。


等待视频处理完成（状态变为“正常”）后，单击【快捷查看】，在页面右侧可以看到该视频有两个转码视频。

登录 [SCF 控制台日志页面](https://console.cloud.tencent.com/scf/list-detail?rid=1&ns=vod_demo&id=callback&menu=log) 查看 SCF 日志记录，在最新的一条日志中，可以看到两个转码文件的 URL 已经打印出来，在实际应用场景中，开发者可以通过 SCF 将 URL 记录在自己的数据库，或者通过其它渠道发布给观众。
>?SCF 日志可能会有些许延迟，如果在页面上没有看到日志，请耐心等待一两分钟，然后单击【重置】刷新。
<span id="design"></span>
## 系统设计说明

### 接口协议

事件通知接收云函数通过 API 网关对外提供接口，具体接口协议请参考文档 [视频上传完成事件通知](https://intl.cloud.tencent.com/document/product/266/33950) 和 [任务流状态变更](https://intl.cloud.tencent.com/document/product/266/33953)。

### 事件通知接收服务代码解读

1. `main_handler()`为入口函数。
2. 调用`parse_conf_file()`，从`config.json`文件中读取配置信息。配置项说明如下：
<table>
<thead>
<tr>
<th>字段</th>
<th>数据类型</th>
<th>功能</th>
</tr>
</thead>
<tbody><tr>
<td>secret_id</td>
<td>String</td>
<td>API 密钥</td>
</tr>
<tr>
<td>secret_key</td>
<td>String</td>
<td>API 密钥</td>
</tr>
<tr>
<td>region</td>
<td>String</td>
<td>云 API 请求地域，对于 VOD 可随意填写</td>
</tr>
<tr>
<td>definitions</td>
<td>Array of Integer</td>
<td>转码模板</td>
</tr>
<tr>
<td>subappid</td>
<td>Integer</td>
<td>事件通知是否来自 <a href="https://intl.cloud.tencent.com/document/product/266/33987" target="_blank">云点播子应用</a></td>
</tr>
</tbody></table>
3. 针对 NewFileUpload 类型的事件通知，调用`deal_new_file_event()`解析请求，从中取出新上传视频的 FileId：
```
           if event_type == "NewFileUpload":
               fileid = deal_new_file_event(body)
               if fileid is None:
                   return ERR_RETURN
```
4. 调用`trans_media()`发起转码，输出云 API 的回包到 SCF 日志，并回包给 VOD 的事件通知服务：
```
               rsp = trans_media(configuration, fileid)
               if rsp is None:
                   return ERR_RETURN
               print(rsp)
```
5. 在`trans_media()`中，调用云 API SDK 发起 `ProcessMedia` 请求：
```
        cred = credential.Credential(conf["secret_id"], conf["secret_key"])
        client = vod_client.VodClient(cred, conf["region"])

        method = getattr(models, API_NAME + "Request")
        req = method()
        req.from_json_string(json.dumps(params))

        method = getattr(client, API_NAME)
        rsp = method(req)
        return rsp
```
6. 针对 ProcedureStateChanged 类型的事件通知，调用`deal_procedure_event()`解析请求，从中取出转码输出视频的 URL 并打印到 SCF 日志：
```
           elif event_type == "ProcedureStateChanged":
               rsp = deal_procedure_event(body)
               if rsp is None:
                   return ERR_RETURN
```

   
