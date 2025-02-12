Storage usage refers to the used storage capacity. You are charged by the objects' size and how long you stored them. For different storage classes, the unit price, minimum object size, and minimum storage duration are different as described below. The actual costs depend on the use case and storage class of the object.

>? For more information on storage classes, see [Overview](https://intl.cloud.tencent.com/document/product/436/30925).
>



## Storage Usage Fees


### STANDARD storage usage fees     

<table>
<thead>
<tr><th style="width: 64%;">Billable Item Description</th><th style="width: 16%;">Applicable Region</th><th style="width: 20%;">Applicable Billing Mode</th></tr>
</thead>
<tbody>
<tr>
<td>STANDARD storage usage refers to the size of your data stored in the STANDARD storage class. You will be charged by the actual STANDARD storage usage and how long you store the data.</td>
<td>All regions</td>
<td>Pay-as-you-go</td>
</tr>
</tbody></table>


### STANDARD_IA storage usage fees  

<table>
<thead>
<tr><th style="width: 64%;">Billable Item Description</th><th style="width: 16%;">Applicable Region</th><th style="width: 20%;">Applicable Billing Mode</th></tr>
</thead>
<tbody>
<tr>
<td>STANDARD_IA storage usage refers to the size of your data stored in the STANDARD_IA storage class. You will be charged by the actual use case:<ul  style="margin: 0;"><li>Objects stored less than 30 days will be calculated as 30 days. A single object smaller than 64 KB will be calculated as 64 KB, while objects larger than or equal to 64 KB will be billed based on their actual sizes.</li><li>If you successfully uploaded an object to STANDARD_IA without enabling versioning, COS will delete the existing object (if any) that has the same name. In this case, storage usage fees will also be incurred for the <strong>early deletion of the object</strong>.</li></ul></td>
<td>All regions</td>
<td>Pay-as-you-go</td>
</tr>
</tbody></table>

### INTELLIGENT TIERING storage usage fees   

<table>
<thead>
<tr><th style="width: 64%;">Billable Item Description</th><th style="width: 16%;">Applicable Region</th><th style="width: 20%;">Applicable Billing Mode</th></tr>
</thead>
<tbody>
<tr>
<td>INTELLIGENT TIERING storage usage refers to the size of your data stored in the INTELLIGENT TIERING storage class. You will be charged by the actual use case:<ul  style="margin: 0;"><li>The storage usage fees vary by tier of your stored objects. If your objects are stored in the frequent access tier, you will be charged at STANDARD prices. If your objects are stored in the infrequent access tier, you will be charged at STANDARD_IA prices.</li><li>Objects smaller than 64 KB will always be stored in the frequent access tier.<br></li><li>All objects will be billed based on their actual sizes.</li></ul></td>
<td>Beijing, Nanjing, Shanghai, Guangzhou, Chengdu, Chongqing, Tokyo, and Singapore</td>
<td>Pay-as-you-go</td>
</tr>
</tbody></table>


### ARCHIVE storage usage fees       

<table>
<thead>
<tr><th style="width: 64%;">Billable Item Description</th><th style="width: 16%;">Applicable Region</th><th style="width: 20%;">Applicable Billing Mode</th></tr>
</thead>
<tbody><tr>
<td>ARCHIVE storage usage refers to the size of your data stored in the ARCHIVE storage class. You will be charged by the actual use case:<ul  style="margin: 0;"><li>Objects stored less than 90 days will be calculated as 90 days. A single object smaller than 64 KB will be calculated as 64 KB, while objects larger than or equal to 64 KB will be billed based on their actual sizes.</li><li>If you successfully uploaded an object to ARCHIVE without enabling versioning, COS will delete the existing object (if any) that has the same name. In this case, storage usage fees will be incurred for the <strong>early deletion of the object</strong>.</li></ul></td>
<td>All public cloud regions except Jakarta</td>
<td>Pay-as-you-go</td>
</tr>
</tbody></table>


### DEEP ARCHIVE storage usage fees  

<table>
<thead>
<tr><th style="width: 64%;">Billable Item Description</th><th style="width: 16%;">Applicable Region</th><th style="width: 20%;">Applicable Billing Mode</th></tr>
</thead>
<tbody><tr>
<td>DEEP ARCHIVE storage usage refers to the size of your data stored in the DEEP ARCHIVE storage class. You will be charged by the actual use case:<ul  style="margin: 0;"><li>Objects stored less than 180 days will be calculated as 180 days. A single object smaller than 64 KB will be calculated as 64 KB, while objects larger than or equal to 64 KB will be billed based on their actual sizes.</li><li>If you successfully uploaded an object to DEEP ARCHIVE without enabling versioning, COS will delete the existing object (if any) that has the same name. In this case, storage usage fees will be incurred for the <strong>early deletion of the object</strong>.</li></ul></td>
<td>Beijing, Nanjing, Shanghai, Guangzhou, Chengdu, Chongqing, Tokyo, and Singapore</td>
<td>Pay-as-you-go</td>
</tr>
</tbody></table>


## Billing Mode and Calculation Formula

| Billing Mode | Calculation Formula |
| -------- | ------------------------------------------------------------ |
|  Pay-as-you-go   | <ul  style="margin: 0;"> <li>Daily billing cycle</li><li>Storage usage fees = monthly unit price / 30 * daily storage usage * number of days</li><li>Daily storage usage = sum of "5-minute usage" / 288 (number of statistical points)</li></ul>  |

## Storage Usage Pricing


>?
>For the unit prices of storage usage, see [Pricing | Cloud Object Storage](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=).

## Billing Examples

>?
>- Prices in the following examples are for reference only. For the actual prices, see [Pricing | Cloud Object Storage](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=).
>- The storage capacity is calculated in binary, for example, 1 TB = 1024 GB.

### Example 1: STANDARD storage usage fees + STANDARD request fees

Assume that on November 1, 2020, user A uploaded 10 GB of data to a bucket residing in Guangzhou region in the STANDARD storage class, generating 100 requests. Apart from this, user A did not perform any other operations. As fees for a day were settled the next day:

- STANDARD storage usage fees: Settled daily starting from November 2, 2020.
- STANDARD request fees: Settled on November 2, 2020.

Fees analysis:
- STANDARD storage usage fees = 0.024 USD/GB/month / 30 * 10 GB * 30 days = 0.24 USD
- STANDARD request fees = 0.002 USD/10,000 requests * 100 requests / 10,000 = 0.00002 USD

In summary, the total bill for user A in November is calculated as follows: 0.24 + 0.00002 = 0.24002 USD.


### Example 2: STANDARD_IA storage usage fees + STANDARD_IA request fees

Assume that on November 1, 2020, user B uploaded 10 GB of data (including 10,000 pieces of 30 KB objects) to a bucket residing in Guangzhou region in the STANDARD_IA storage class, generating 100 requests. Apart from this, user B did not perform any other operations. As fees for a day were settled the next day:

- STANDARD_IA storage usage fees: Settled daily starting from November 2, 2020.
- STANDARD_IA request fees: Settled on November 2, 2020.

Fees analysis:

- STANDARD_IA storage usage = 10 GB + 10,000 * 30 KB = 10.286 GB
- STANDARD_IA storage usage fees = 0.018 USD/GB/month / 30 * 10.286 GB * 30 days = 0.19 USD
- STANDARD_IA request fees = 0.01 USD/10,000 requests * 100 requests / 10,000 = 0.0001 USD


In summary, the total bill for user B in November is calculated as follows: 0.19 + 0.0001 = 0.1901 USD.
