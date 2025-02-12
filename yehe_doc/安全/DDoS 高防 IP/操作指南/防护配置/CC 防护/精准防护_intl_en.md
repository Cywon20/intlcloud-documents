## Use Cases
Anti-DDoS supports precise protection for connected web applications. With the precise protection, you can configure protection policies combining multiple conditions of common HTTP fields, such as URI, UA, Cookie, Referer, and Accept to screen access requests. For the requests matched the conditions, you can configure CAPTCHA to verify the requesters or a policy to automatically drop or allow the requests. Precise protection is available for policy customization in various use cases to precisely defend against CC attacks.

The match conditions define the request characteristics to be checked, i.e., the attribute characteristics of the HTTP field in a request. Precise protection supports checking the HTTP fields below:
<table>
    <tr>
        <th>Match Field</th>
        <th>Field Description</th>
				<th>Logic</th>
    </tr>
    <tr>
        <td>URI</td>
        <td>The URI of an access request.</td>
				<td>Equals to, includes, or does not include.</td>
    </tr>
    <tr>
        <td>UA</td>
        <td>The identifier and other information of the client browser that initiates an access request.</td>
				<td>Equals to, includes, or does not include.</td>
    </tr>
    <tr>
        <td>Cookie</td>
        <td>The cookie information in an access request.</td>
				<td>Equals to, includes, or does not include.</td>
    </tr>
    <tr>
        <td>Referer</td>
        <td>The source website of an access request, from which the access request is redirected.</td>
				<td>Equals to, includes, or does not include.</td>
    </tr>
    <tr>
        <td>Accept</td>
        <td>The data type to be received by the client that initiates the access request.</td>
				<td>Equals to, includes, or does not include.</td>
    <tr>
        <td>Method</td>
        <td>Request methods, such as HEAD, GET, POST, DELETE, and PUT.</td>
				<td>Equals to, includes, or does not include.</td>	    
    </tr>
</table>

## Prerequisites
You have purchased an [Anti-DDoS Advanced instance](https://intl.cloud.tencent.com/document/product/297/37241) and set the object to protect.

## Directions
1. Log in to the [Anti-DDoS console](https://console.cloud.tencent.com/ddos/antiddos-advanced/config/port) and select **Anti-DDoS Advanced (New)** > **Configurations** on the left sidebar. Open the **CC Protection** tab.
2. Select a domain name from the IP list on the left.
![](https://qcloudimg.tencent-cloud.cn/raw/273bc2d0fa1e5a38c0771792d2f85e07.png)
3. Click **Set** in the **Precise Protection** section to enter the rule list.
![](https://qcloudimg.tencent-cloud.cn/raw/3a344b4800103b2e69987bd5e09e45ea.png)
4. Click **Create** to create a precise protection rule. Fill in the fields and click **OK**.
![](https://main.qcloudimg.com/raw/0b4604f0d97e6059c6d35c3181beee60.png)
5. Now the new rule is added to the rule list. You can click **Configuration** on the right of the rule to modify it.
![](https://main.qcloudimg.com/raw/13ed1705034538fc6fc1b1090a65bc8e.png)
