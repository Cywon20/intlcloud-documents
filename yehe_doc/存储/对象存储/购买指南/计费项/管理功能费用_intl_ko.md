
관리 기능 비용이란 사용자가 관리 기능(리스트 또는 인덱스 기능)을 활성화 및 사용하여 발생한 요금입니다.

>?스토리지 유형에 대한 더 자세한 소개는 [스토리지 유형 개요](https://intl.cloud.tencent.com/document/product/436/30925)를 참고하십시오.
> 

<table>
<thead>
<tr>
<th>과금 항목</th>
<th>적용 스토리지 유형</th>
<th>설명</th>
<th>과금 방식</th>
</tr>
</thead>
<tbody><tr>
<td>인벤토리 기능 요금</td>
<td nowrap="nowrap">스토리지 유형과 별개</td>
<td>인벤토리 기능이 활성화된 후 버킷 객체를 나열할 때 발생하는 요금</td>
<td>종량제</li></td>
</tr>
<tr>
<td >인덱스 기능 비용</td>
<td>STANDARD<br>STANDARD_IA</td>
<td>인덱스 기능이 활성화된 경우 객체 인덱스로 인해 발생하는 요금</td>
<td>종량제</td>
</tr>
<tr>
<td>일괄 프로세스 요금</td>
<td nowrap="nowrap">스토리지 유형과 별개</td>
<td>일괄 프로세스 기능을 활성화하면 COS에서 생성된 작업 및 처리된 객체 수를 기준으로 과금</td>
<td>종량제</td>
</tr>
<tr>
<td>객체 태그 요금</td>
<td nowrap="nowrap">스토리지 유형과 별개</td>
<td>객체 태그 기능을 활성화하면 COS는 설정된 객체 태그 수를 기준으로 과금</td>
<td>종량제</td>
</tr>
</tbody></table>



## 관리 기능 요금의 과금 방식 및 계산 방법

<table>
   <tr>
      <th>과금 방식</th>
      <th>과금 항목</th>
      <th>계산 방법</th>
   </tr>
   <tr>
      <td rowspan=4>종량제</td>
      <td>인벤토리 기능 요금</td>
      <td nowrap="nowrap"><ul  style="margin: 0;"><li>일 결산 주기</li><li>인벤토리 요금 = 나열된 객체 수 / 100만 x 단가</li></ul></td>
   </tr>
   <tr>
      <td>인덱스 기능 요금</td>
      <td nowrap="nowrap"><ul  style="margin: 0;"><li>일 결산 주기</li><li>인덱스 요금 = GB당 단가 x 1일 인덱스 데이터 누적량</li></ul></td>
   </tr>
   <tr>
      <td>일괄 프로세스 요금</td>
			<td nowrap="nowrap">일괄 프로세스 요금에는 작업 요금과 객체 처리 요금이 포함됩니다. <ul  style="margin: 0;"><li>일 결산 주기<br><li>작업 요금 = 생성된 작업 수 x 단가</li><li>객체 처리 요금 = 처리된 객체 1만 개당 x 단가</li></ul></td>
   </tr>
   <tr>
      <td>객체 태그 요금</td>
			<td nowrap="nowrap"><ul  style="margin: 0;"><li>일 결산 주기</li><li>객체 태그 요금 = 태그 1만 개당 x 단가</li></ul></td>
   </tr>
</table>


## 관리 기능 가격

관리 기능 단가는 [가격](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)을 참고하십시오.

>!2021년 9월 30일부터 객체 태그 기능의 정가가 인하되었습니다.
>1. 퍼블릭 클라우드 중국 본토 리전 가격은 0.00025817 USD/10만 태그/일, 중국홍콩 및 해외 리전은 0.0003098 USD/1만 태그/일로 인하되었습니다.
>2. 이 가격은 2021년 10월 1일부터 생성된 청구서(즉, 2021년 9월 30일 이후에 발생한 요금)에 적용됩니다.
>




## 과금 사례

>? 다음 사례의 가격은 참고용이며, 실제 가격은 COS [가격](https://buy.intl.cloud.tencent.com/price/cos?lang=en&pg=)을 참고하십시오.
>


### 사례: STANDARD 스토리지 사용 요금 + 객체 태그 요금 + 요청 요금

2020년 11월 1일에 사용자 A가 STANDARD 스토리지 클래스의 광저우 리전에 있는 버킷에 10GB의 데이터를 업로드하고 같은 날 10만개의 객체에 대한 태그를 추가하여 10만회 요청을 생성하였으며, 이러한 작업 외에 사용자 A는 다른 작업을 수행하지 않았다고 가정할 때, 다음과 같이 전일 요금이 다음날에 정산됩니다.

- STANDARD 스토리지 사용 요금: 2020년 11월 2일에 정산됩니다.
- STANDARD 요청 요금: 2020년 11월 2일에 정산됩니다.
- 객체 태그 요금: 2020년 11월 2일에 정산됩니다.

요금 분석:


- STANDARD 스토리지 사용 요금 = 0.024 USD/GB/월 / 30 x 10GB x 30일 = 0.24 USD.
- STANDARD 요청 요금 = 0.002 USD/1만 회 x 10만 회 = 0.02 USD
- 객체 태그 요금 = 0.00025817 USD/태그 1만 개당/일 x 30 x 객체 10만 개 = 0.077451 USD

요약하면 11월 사용자 A의 총 요금은 0.24 + 0.02 + 0.077451 = 0.337451 USD로 계산됩니다.

