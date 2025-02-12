CLS supports self-service bill and usage query in the following steps:

## Viewing Bills

### Viewing Log Topic Costs

1. Log in to the **Billing Center** and enter the [Bill Details](https://console.cloud.tencent.com/expense/bill/summary?type=resource) page.
2. In the **All Products** drop-down list, select **CLS** to view CLS bills.

 - By default, the bill data is arranged by cost in descending order, and you can visually view log topics that cost the most.
 - With the bill time range selector on the top, you can switch the statistics time of the bills.
3. In the row where the log topic you need to view, click **View Bill Details**.

4. The bill details page about the log topic is displayed.

You can view the following information on the page:
 - Cost and proportion of each billable item in the bill of the log topic: When doing cost optimization, you can choose different policies based on the proportions of the traffic cost and storage cost. If the traffic cost occupies a large proportion, you can reduce the amount of logs printed on the log collection client or filter out useless logs through data processing. If the storage cost occupies a large proportion, in addition to the above two methods, you can shorten the log storage duration.
 - Monthly cost trend in the bill of the log topic: The information can be used for year-on-year analysis to determine whether there is an abnormal sudden increase in the cost.

### Classifying Services by Cost Allocation Tags

When CLS is shared by different business departments, the CLS costs need to be allocated among the business departments. In this case, you can use the [cost allocation tag](https://intl.cloud.tencent.com/document/product/555/32276) feature to associate the log topic fees with the business departments.

1. Create a tag. For details, see [Creating Tags](https://intl.cloud.tencent.com/document/product/651/41684).

 - Tag Key: for example, "service"
 - Tag Value: for example, "service A" and "service B"
2. Bind a log topic.
<dx-tabs>
::: Binding on the CLS Console
1. Log in to the CLS console, and enter the [Log Topic](https://console.cloud.tencent.com/cls/topic) page.
2. Select a log topic, and then click **Edit Tag**.

3. In the pop-up window, select the "service" tag you created, and click **OK**.
:::
::: Binding on the Tag Console
1. Log in to the tag console, and enter the [Tag List](https://console.cloud.tencent.com/tag/taglist) page.
2. Select a corresponding tag, and click **Bind Resources**.

3. In the pop-up window, select **Cloud Log Service** for **Service Type** and **Topic** for **Resource Type**, select a corresponding log topic, and click **OK**.

:::
</dx-tabs>
3. Log in to the Billing Center, and enter the [Cost Allocation Tags](https://console.cloud.tencent.com/expense/tag) page.
4. Select the "service" tag you created, and click **Set as Cost Allocation Tag**.

5. View bill statistics of different businesses by the ''service'' cost allocation tag.
<dx-tabs>
::: Viewing the Log Topic Cost of Each Business
1. Log in to the Billing Center, and enter the [Bill Details](https://console.cloud.tencent.com/expense/bill/summary?type=resource) page.
2. Select **Cloud Log Service (CLS)** from the **All products** drop-down list, and select the value of the "service" tag from the **All Tags** drop-down list.

:::
::: Viewing the Total Cost of Each Business
1. Log in to the Billing Center, and enter the [Bill Overview](https://console.cloud.tencent.com/expense/bill/overview) page.
2. Select the **By Tag** tab, and view the total cost.

:::
</dx-tabs>


### Setting an Alert for CLS Costs

>!
> - Spend data has a delay of 2 days. It can be used for cost analysis and budget planning, but not for settlement or reconciliation.
> - The spend analysis feature is currently in beta test.
>

1. Log in to the Billing Center, and enter the [Spend Analysis](https://console.cloud.tencent.com/expense/cost/analysis) page.
2. Click **Set Alert**.

3. Click **New**.

4. In the pop-up window, select **Cloud Log Service** for **Product** in the **Scope** area, and configure other parameters as needed.

Two alert types are supported: daily data alert and monthly data alert.
 - Daily data alert has three modes: day-over-day, month-over-month, and fixed value (compare your daily spend against a fixed value).
    Daily data alert formulas:
    - MoM = ABS ((Daily spend this month − Spend on the same day last month) / Spend on the same day last month × 100%)
    - DoD = ABS ((Daily spend on a day − Spend on the previous day) / Spend on the previous day × 100%)
    - Fixed value = Compare spend in the current month against a fixed value
 - Monthly data alert has two modes: month-over-month and fixed value (compare your monthly spend against a fixed value).
    Monthly data alert formulas:
    - MoM = ABS ((Spend this month - Spend last month) / Spend last month × 100%)
    For example, on August 18, spend in the current month is your total spend between August 1 and August 16, which is compared against your spend between July 1 and July 16.
    - Fixed value = Compare spend in the current month against a fixed value
 - You can configure alert notifications by product and billing mode.
    Product: All purchased products of the current user are displayed. The logic of the billing mode is similar.
5. Click **Save**.


## Querying Usage

### Viewing the usage of a single log topic

1. Log in to the CLS console, and enter the [Log Topic](https://console.cloud.tencent.com/cls/topic) page.
2. In the log topic list, click ![img](https://qcloudimg.tencent-cloud.cn/raw/9876ff99b4cb8853fe291ceec5b5b7ea.png) to view the usage of a log topic.



### Viewing the usage of multiple log topics

1. Log in to the Cloud Monitor console, and enter the [Log service monitor](https://console.cloud.tencent.com/monitor/product/cls_uin_topic) page. 
2. Select a region on the top, abd view the log topic usage in the region. (All the traffic data in the list is the traffic accumulated on the day.)

3. In the log topic list, click ![img](https://qcloudimg.tencent-cloud.cn/raw/9876ff99b4cb8853fe291ceec5b5b7ea.png) to view the usage of the log topics.


### Customizing Analysis of Log Topic Usage

1. Log in to the Cloud Monitor console, and enter the [Dashboard List](https://console.cloud.tencent.com/monitor/dashboard2/dashboards) page.
2. Click **Create Dashboard** to create a dashboard for analyzing log topic usage.

3. After completing the configuration, click **Save** in the top-right corner to save the dashboard.
For more instructions about the dashboard feature, see [Dashboard](https://intl.cloud.tencent.com/document/product/248/38461) in the operation guide of Cloud Monitor.
