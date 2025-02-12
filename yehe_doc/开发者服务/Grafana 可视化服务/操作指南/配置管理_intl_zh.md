Grafana 可视化服务支持自定义 Grafana 配置参数、LDAP 配置以及环境变量。


## 前提条件

1. 登录 [Grafana 可视化服务控制台](https://console.cloud.tencent.com/monitor/grafana)。
2. 找到对应的实例，单击“实例 ID”。

## Grafana 配置

1. 进入实例详情页，单击左侧列表的**配置**。
2. 在配置管理页，单击左上角的**新建**。
   ![](https://qcloudimg.tencent-cloud.cn/raw/2245fd15ef317a41798ba9cafa4c8cde.png)
3. 在弹框中， 输入自定义 ini 格式的配置文件内容，单击**保存**后，实例将会进行重建，具体配置说明请参考 [Grafana-Configuration ](https://grafana.com/docs/grafana/latest/administration/configuration/#config-file-locations)。

## LDAP 配置
1. 进入实例详情页，单击左侧列表的**配置**。
2. 在配置管理页，切换到**LDAP 配置**页面，单击左上角的**修改**。
3. 参考 [Grafana-LDAP Authentication](https://grafana.com/docs/grafana/latest/auth/ldap/#grafana-ldap-configuration)，输入自定义 ini 格式的 LDAP 配置内容，单击**保存**后，实例将会进行重建。

## 环境变量
1. 进入实例详情页，单击左侧列表的**配置**。
2. 切换到**环境变量**页面，单击左上角的**修改**。
3. 按需设置环境变量，单击**保存**后，实例将会进行重建。
   ![](https://qcloudimg.tencent-cloud.cn/raw/11a2f6582147eaeebc7b021d1fd286a7.png)

## 相关说明
1. LDAP 配置暂不支持配置证书。
2. 环境变量名的格式要求：以字母或下划线开头并且只能包含字母、下划线以及数字。
