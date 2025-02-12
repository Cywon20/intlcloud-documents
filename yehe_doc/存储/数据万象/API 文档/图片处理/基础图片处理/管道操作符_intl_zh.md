## 功能概述
腾讯云数据万象的管道操作符`|`能够实现对图片按顺序进行多种处理。用户可以通过管道操作符将多个处理参数分隔开，从而实现在一次访问中按顺序对图片进行不同处理。处理图片原图大小不超过32MB、宽高不超过30000像素且总像素不超过2.5亿像素，处理结果图宽高设置不超过9999像素；针对动图，原图宽 x 高 x 帧数不超过2.5亿像素。

## 接口示例
用户在图片 URL 链接后以样式分隔符`?`与处理样式相连接，多个处理样式之间以管道操作符`|`分隔，样式按照先后顺序执行，目前最多支持10层管道。

## 实际案例


本次案例先将原图缩放为50%，然后在图的右下角添加“数据万象”的文字水印。


假设原图 URL 为：
```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg
```

原图显示如下： 
![](https://main.qcloudimg.com/raw/52e4147a6febbc589505c67973edb394.png)

#### 案例一：对图片进行缩放并添加水印

首先增加缩放操作，则 URL 为：

```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?imageMogr2/thumbnail/!50p
```

图片显示如下：
![](https://main.qcloudimg.com/raw/f6ab90d8bb2cc1faa1aa3467216c450d.jpg)

使用管道操作符再添加文字水印的样式处理，则最终 URL 为：

```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?imageMogr2/thumbnail/!50p|watermark/2/text/5pWw5o2u5LiH6LGh/fill/I0ZGRkZGRg==/fontsize/30/dx/20/dy/20
```

图片最终处理效果如下： 
![](https://main.qcloudimg.com/raw/61567af609b9f9dd93fe49b22825d3d0.jpg)

#### 案例二：使用管道操作符并携带私有文件签名

处理方式同上，仅增加签名部分，并与处理参数以“&”连接，示例如下：

```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?q-sign-algorithm=<signature>&imageMogr2/thumbnail/!50p|watermark/2/text/5pWw5o2u5LiH6LGh/fill/I0ZGRkZGRg==/fontsize/30/dx/20/dy/20
```

>? `<signature>` 为签名部分，获取方式请参考 [请求签名](https://intl.cloud.tencent.com/document/product/436/7778)。
>

## 注意事项

为了避免未授权人员通过访问不携带处理参数的链接实现访问和下载原图的情况，您可同时将处理参数签入到请求签名中，处理参数整体是参数的 key，value 为空，如下是简单的示例（仅做样式参考，可能已经过期无法直接访问），详细计算方法请参见 [请求签名](https://intl.cloud.tencent.com/document/product/436/14114)。


```plaintext
http://examples-1251000004.cos.ap-shanghai.myqcloud.com/sample.jpeg?q-sign-algorithm=sha1&q-ak=AKID********************&q-sign-time=1593342360;1593342720&q-key-time=1593342360;1593342720&q-header-list=&q-url-param-list=watermark%252f1%252fimage%252fahr0cdovl2v4yw1wbgvzlteyntewmdawmdqucgljc2gubxlxy2xvdwquy29tl3nodwl5aw4uanbn%252fgravity%252fsoutheast&q-signature=26a429871963375c88081ef60247c5746e834a98&watermark/1/image/aHR0cDovL2V4YW1wbGVzLTEyNTEwMDAwMDQucGljc2gubXlxY2xvdWQuY29tL3NodWl5aW4uanBn/gravity/southeast
```

