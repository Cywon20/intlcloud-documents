
## Connection
### Is WAF available to servers outside Tencent Cloud?

WAF can be connected with servers in data centers outside Tencent Cloud. WAF protects servers in any public networks, including but not limited to Tencent Cloud, and clouds and IDCs from other vendors. 
>! Domain names connected in the Chinese mainland must be ICP filed as required by the Ministry of Industry and Information Technology of China.

### Does WAF support HTTPS protection?

WAF fully supports HTTPS services. You just need to upload the SSL certificate and private key as instructed or select the Tencent Cloud-hosted certificate to use WAF for HTTPS traffic protection.

### How many forwarding IPs can be set for one protected domain name in WAF?
Up to 20 forwarding IPs can be set for one protected domain name in WAF.

### Does WAF support health check?
Health check is enabled for WAF by default. WAF checks the connection status of all real server IPs. For the real server IP that does not respond, WAF will not forward requests to this IP until its connection status becomes normal.

### Does WAF support session persistence?
Session persistence is supported and enabled by default in WAF.

### How long does it take for configuration changes to take effect in the WAF console?

In general, a configuration change takes effect within 10 seconds.

### Is the SSL mutual authentication supported by both the SaaS WAF and CLB WAF?
It is supported by CLB WAF but not by SaaS WAF.

## Domain Name

### How do I connect a domain name?
You can connect a domain name in the [WAF console](https://console.cloud.tencent.com/guanjia/tea-overview). For more information, see [Step 1. Add a Domain Name](https://intl.cloud.tencent.com/document/product/627/35651).

### Can I change the VIP address of my domain name?
No, you can't change it in principle. However, if an exception occurs to your WAF service, you can [submit a ticket](https://console.cloud.tencent.com/workorder/category?level1_id=141&level2_id=642&source=0&data_title=T-Sec-Web%E5%BA%94%E7%94%A8%E9%98%B2%E7%81%AB%E5%A2%99&level3_id=867&radio_title=%E6%8E%A7%E5%88%B6%E5%8F%B0%E9%97%AE%E9%A2%98&queue=15&scene_code=29995&step=2) for application, and we will switch it to another working VIP address for you.
### What options does WAF offer for domain name origin-pull?
WAF performs origin-pull based on domain name or IP. You can choose which option to configure as you need. For more information, see [Add a Domain Name](https://intl.cloud.tencent.com/document/product/627/35651).
### How do I bind a CNAME to my domain name connected to WAF?
See [CNAME Configuration](https://intl.cloud.tencent.com/document/product/228/3121) for how to bind CNAME with your DNS service provider.
### Will logging still be available once WAF is disabled for the domain name list?
Once WAF is disabled, all its protection features are unavailable, and only the traffic forwarding mode starts to run instead, with no logs recorded.
### Will the CNAME change if my domain name is deleted and added again?
No, it won't. You can go to the [WAF console](https://console.cloud.tencent.com/guanjia/tea-domain), click the domain name, and view the CNAME in **Basic settings**.
![](https://qcloudimg.tencent-cloud.cn/raw/0110a5ff713c719d286482e4954db082.png)
