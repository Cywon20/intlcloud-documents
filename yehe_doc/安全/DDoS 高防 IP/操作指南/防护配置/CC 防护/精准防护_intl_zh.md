## 应用场景
DDoS 高防支持对已接入防护的网站业务配置精准防护策略。开启精确访问控制后，您可以对常见的 HTTP 字段（例如 URI、UA、Cookie、Referer、Accept 等）做条件组合防护策略，筛选访问请求，并对命中条件的请求设置人机校验、丢弃或放行策略动作。精准防护支持业务场景定制化的防护策略，可用于精准定制针对性的 CC 防御。

匹配条件定义了要识别的请求特征，具体指访问请求中 HTTP 字段属性特征。精确防护规则支持匹配的 HTTP 字段如下表所示。
<table>
    <tr>
        <th>匹配字段</th>
        <th>字段描述</th>
				<th>适用逻辑</th>
    </tr>
    <tr>
        <td>URI</td>
        <td>访问请求的 URI 地址</td>
				<td>等于、包含、不包含</td>
    </tr>
    <tr>
        <td>UA</td>
        <td>发起访问请求的客户端浏览器标识等相关信息</td>
				<td>等于、包含、不包含</td>
    </tr>
    <tr>
        <td>Cookie</td>
        <td>访问请求中的携带的 Cookie 信息</td>
				<td>等于、包含、不包含</td>
    </tr>
    <tr>
        <td>Referer</td>
        <td>访问请求的来源网址，即该访问请求是从哪个页面跳转产生的</td>
				<td>等于、包含、不包含</td>
    </tr>
    <tr>
        <td>Accept</td>
        <td>发起访问请求的客户端希望接受的数据类型</td>
				<td>等于、包含、不包含</td>
    <tr>
        <td>Method</td>
        <td>访问请求的方法，具体包括 HEAD、GET、POST、DELTET、PUT</td>
				<td>等于、包含、不包含</td>	    
    </tr>
</table>

## 前提条件
您需要成功 [购买 DDoS 高防 IP](https://intl.cloud.tencent.com/document/product/297/37241) ，并设置防护对象。

## 操作步骤
1. 登录 [DDoS 高防 IP（新版）管理控制台](https://console.cloud.tencent.com/ddos/antiddos-advanced/config/port) ，在左侧导航中，单击**防护配置** > **CC 防护**。
2. 在 CC 防护页面的左侧列表中，选中高防 IP 的 ID 下面的域名。
![](https://qcloudimg.tencent-cloud.cn/raw/273bc2d0fa1e5a38c0771792d2f85e07.png)
3. 在精准防护卡片中，单击**设置**，进入精准防护规则列表。
![](https://qcloudimg.tencent-cloud.cn/raw/3a344b4800103b2e69987bd5e09e45ea.png)
4. 单击**新建**，创建精准防护规则，填写相关字段，填写完成后，单击**确定**。
![](https://main.qcloudimg.com/raw/0b4604f0d97e6059c6d35c3181beee60.png)
5. 新建完成后，在精准防护列表将新增一条精准防护规则，可以在右侧操作列，单击**配置**，修改精准防护规则。
![](https://main.qcloudimg.com/raw/13ed1705034538fc6fc1b1090a65bc8e.png)
