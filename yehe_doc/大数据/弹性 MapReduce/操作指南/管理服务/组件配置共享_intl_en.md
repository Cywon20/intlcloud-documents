## Description
You can share the components of an existing cluster with other clusters without deploying such components again. This makes it easier to manage multiple clusters with the same component configurations.
Currently supported components include Ranger, ZooKeeper, and Kerberos.
Currently supported versions include all EMR versions containing the above components.
- Dependent: The current cluster is using components of other existing clusters.
- Shared: One or more components of the current cluster are added to other newly created clusters.

>! Kerberos and Ranger can be shared with other EMR clusters, so they don't need to be deployed for new clusters again. This increases the Ops configuration efficiency. Dependencies cannot be canceled once configured; therefore, proceed with caution based on your business conditions.

## Use Limits
1. Your account must have the management permissions of one existing cluster.
2. When Kerberos is enabled, other clusters under the current account must have Kerberos deployed before you can enable the dependency.
3. If a dependent component involves user identity management, you need to manually sync the users in the sharing cluster to the new cluster after creating it or adding a user. Note that as the current cluster doesn't have Kerberos deployed, krb5 users cannot be created.

## Product Architecture
<img src="https://qcloudimg.tencent-cloud.cn/raw/5868e6ecbdbae430d2200892a2951724.png" style="zoom:80%;" />

## Directions
### Purchasing cluster
1. Go to the [purchase page](https://buy.cloud.tencent.com/emr) and select multiple components under Hadoop.
2. Click **Enable** and select the sharing cluster.
3. After the new cluster is successfully purchased, relevant components will automatically depend on the selected cluster.
![](https://qcloudimg.tencent-cloud.cn/raw/6ba1bb2fa17934df2638a5136dfc7200.png)

### Adding component
1. Select **Cluster Service** > **Add Component** on the cluster details page in the [console](https://console.cloud.tencent.com/emr)
2. Select the components to be added.
3. Click **Enable** and select the dependent components and cluster.
4. Click **OK** and wait for the components to be successfully added to the cluster.
![](https://qcloudimg.tencent-cloud.cn/raw/074e6625687603fd8a8c4b0768747153.png)

## Cluster Information
If a cluster has dependent or shared components, the component information is displayed in **Instance Information**.
![](https://qcloudimg.tencent-cloud.cn/raw/b5fa0fd34ef279f7b112c64b3f314c33.png)
![](https://qcloudimg.tencent-cloud.cn/raw/7a273cfad8bba3fe3aa2def72d4d5db5.png)

>! The management features of dependent components can be used only on the **Component Management** page of the depended-on cluster.

## Account Sync
User accounts need to be created in both the dependent and depended-on clusters.
1. Log in to the depended-on cluster and click **Create User** to create an account.
2. Log in to the dependent cluster and click **Create User** to create an account with the same username, user group, and password.
3. The dependent cluster will automatically sync the permission and usage information of the account.
![](https://qcloudimg.tencent-cloud.cn/raw/5aa9c5d692f6000d85b8ad32dc3b969b.png)
