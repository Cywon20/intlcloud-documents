连接器用于从事件源中主动拉取事件，并将事件以**标准化的格式**推送到自定义事件集中。您无需关心底层的消费投递逻辑，通过在事件集中绑定一个或多个连接器，即可以实现自动拉取消息队列、网关的事件内容，并推送至指定的自定义事件集。



- 事件总线 EventBridge 的连接器支持以下事件源：
<table>
<thead>
<tr>
<th>事件源</th>
<th>配置方法</th>
</tr>
</thead>
<tbody><tr>
<td>消息队列 TDMQ</td>
<td><a href="https://intl.cloud.tencent.com/document/product/1108/42278">TDMQ 连接器</a></td>
</tr>
<tr>
<td>API 网关（APIGW）HTTP</td>
<td><a href="https://intl.cloud.tencent.com/document/product/1108/42279">APIGW 连接器</a></td>
</tr>
<tr>
<td>消息队列 Ckafka</td>
<td><a href="https://intl.cloud.tencent.com/document/product/1108/42280">Ckafka 连接器</a></td>
</tr>
<tr>
<td>SaaS 事件投递（企业集成服务 EIS 提供支持）</td>
<td><a href="https://intl.cloud.tencent.com/document/product/1108/42281">SaaS 连接器</a></td>
</tr>
<tr>
<td>DTS 数据订阅</td>
<td><a href="https://intl.cloud.tencent.com/document/product/1108/46991">DTS 连接器</a></td>
</tr>
</tbody></table>

- 事件总线 EventBridge 提供以下连接器管理能力：
  - 创建连接器
  - 查看连接器
  - 编辑连接器
  - 启动连接器
  - 暂停连接器
  - 删除连接器

