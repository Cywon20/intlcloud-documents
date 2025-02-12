## 소개

인텔리전트 티어링 유형은 데이터에 콜드/핫 레이어 메커니즘을 구분해 사용자 데이터의 액세스 모드에 따라 자동으로 데이터의 콜드/핫 레이어를 전환함으로써 사용자 데이터의 스토리지 비용을 줄일 수 있습니다.

인텔리전트 티어링 스토리지 유형은 액세스 모드가 가변적이거나 데이터의 액세스 모드를 예측할 수 없는 경우에 활용할 수 있습니다. 스탠다드 스토리지와 동일한 수준의 낮은 딜레이와 높은 처리량의 성능을 제공하지만, 요금은 스탠다드 스토리지보다 저렴합니다. 서비스 요구사항에 맞춰 액세스 모드가 가변적인 데이터를 스탠다드 스토리지 유형에서 인텔리전트 티어링 스토리지 유형으로 전환하여 클라우드 내 스토리지 비용을 절약할 수 있습니다.

>!
> - 현재 인텔리전트 티어링은 베이징, 난징, 상하이, 광저우, 청두, 충칭, 도쿄, 싱가포르 리전에서만 사용할 수 있습니다.
> - 인텔리전트 티어링 유형은 독립적인 스토리지 유형입니다. 사용 시 인텔리전트 티어링 용량 비용이 발생합니다. 자세한 과금 정보는 [제품 가격](https://intl.cloud.tencent.com/pricing/cos)을 참고하십시오.
> 

## 강점

사용자가 데이터를 업로드할 때 인텔리전트 티어링 유형을 선택해 COS에 보관할 경우, COS는 주기적으로 데이터 액세스 횟수를 모니터링하며 데이터 액세스가 일정 기간 지속되지 않을 경우 데이터를 스토리지 비용이 더 저렴한 액세스 레이어로 이동합니다. 데이터가 다시 액세스되면, 다시 고빈도 액세스 레이어로 이동해 데이터 읽기 성능을 보장합니다. 데이터 핫/콜드 티어링 스토리지를 통해 사용자가 스토리지 비용과 읽기 성능 사이에서 균형점을 찾을 수 있게 도와줍니다. 인텔리전트 티어링은 다음과 같은 장점이 있습니다.

- **비용 집약**: 데이터를 지속적으로 인텔리전트 티어링 유형으로 저장할 경우, 저장 시간이 길어질수록 스탠다드 스토리지 비용은 상대적으로 낮아져 최대 20% 정도의 스토리지 비용을 절약할 수 있습니다. 인텔리전트 티어링 유형은 객체 스토리지 라이프사이클 프로세스에 관여하여, 사용자는 필요에 따라 인텔리전트 티어링을 아카이브로 전환해 데이터 저장 비용을 더 낮출 수 있습니다.
- **안정성과 지속성**: 인텔리전트 티어링은 스탠다드 스토리지와 동일한 낮은 딜레이 시간, 높은 처리량의 체험을 제공합니다. 또한 인텔리전트 티어링은 이레이저 코딩의 중복 스토리지 방식을 택하여 99.999999999%(11개 중 9)의 데이터 신뢰성을 제공하며, 데이터 블록 스토리지, 동시 읽기/쓰기 등 99.95%의 비즈니스 가용성을 제공합니다.
- **편리성**: 데이터에 객체 스토리지 유형만 지정하면 인텔리전트 티어링의 특징을 적용할 수 있습니다. 인텔리전트 티어링 스토리지 유형은 COS의 API, SDK, 툴 및 생태 애플리케이션에 자연스럽게 융합되어, 사용자가 필요에 따라 클라우드에 저장된 데이터를 관리할 수 있게 합니다.

## 사용 방법

데이터를 인텔리전트 티어링 유형으로 COS에 저장하려면 먼저 버킷의 인텔리전트 티어링 설정을 활성화해야 합니다. 활성화 후, 사용자가 객체를 업로드할 때 **스토리지 유형**을 인텔리전트 티어링 유형으로 지정하면 됩니다.

### COS 콘솔 사용

#### 객체 업로드 시 인텔리전트 티어링으로 설정

다음 단계를 참고하여 객체를 인텔리전트 티어링 유형으로 저장할 수 있습니다.

1. 버킷 설정 페이지에서 인텔리전트 티어링 설정을 활성화합니다. 자세한 내용은 [INTELLIGENT TIERING 설정](https://intl.cloud.tencent.com/document/product/436/38306) 문서를 참고하십시오.
2. 파일 업로드 시 파일 스토리지 유형을 지정합니다. 파일의 업로드 가이드는 [객체 업로드](https://intl.cloud.tencent.com/document/product/436/13321) 문서를 참고하십시오.

>! 버킷의 인텔리전트 티어링 구성을 활성화하면 비활성화할 수 없으니 신중하게 설정하십시오.

#### 클라우드 상의 데이터를 인텔리전트 티어링으로 전환

다음 단계를 참고하여 업로드된 인벤토리 데이터를 인텔리전트 티어링 유형으로 전환할 수 있습니다.

1. 버킷 설정 페이지에서 라이프사이클 규칙을 생성합니다. 자세한 내용은 [라이프사이클 설정](https://intl.cloud.tencent.com/document/product/436/14605) 문서를 참고하십시오.
2. 지정된 규칙의 응용 범위를 설정하고 데이터를 인텔리전트 티어링으로 전환합니다.



### REST API 사용

다음 API를 통해 인텔리전트 티어링을 직접 설정할 수 있습니다.

1. 먼저 REST API를 사용해 버킷의 인텔리전트 티어링을 활성화합니다. 다음 API 문서를 참고하십시오.
	- [PUT Bucket IntelligentTiering](https://intl.cloud.tencent.com/document/product/436/38314)
	- [GET Bucket IntelligentTiering](https://intl.cloud.tencent.com/document/product/436/38315)
2. 버킷에 인텔리전트 티어링이 활성화되면, 다음 API 문서를 참고하여 객체를 인텔리전트 티어링 유형으로 업로드할 수 있습니다.
	- [PUT Object](https://intl.cloud.tencent.com/document/product/436/7749)
	- [PUT Object - Copy](https://intl.cloud.tencent.com/document/product/436/10881)
	- [POST Object](https://intl.cloud.tencent.com/document/product/436/14690)
	- [Initiate Multipart Upload](https://intl.cloud.tencent.com/document/product/436/7746)
3. 객체의 스토리지 유형 및 속한 스토리지 레이어를 조회해야 할 경우, 다음 API 문서를 참고하십시오.
	- [GET Object](https://intl.cloud.tencent.com/document/product/436/7753)
	- [HEAD Object](https://intl.cloud.tencent.com/document/product/436/7745)
4. REST API를 직접 사용하여 인텔리전트 티어링 유형의 객체를 삭제할 수 있습니다. 다음 API 문서를 참고하십시오.
	- [DELETE Object](https://intl.cloud.tencent.com/document/product/436/7743)
	- [DELETE Multiple Objects](https://intl.cloud.tencent.com/document/product/436/8289)

### SDK 사용

현재 COS에서 배포하는 SDK는 모두 인텔리전트 티어링 유형을 지원합니다. 구체적인 방법은 파일 업로드 시, StorageClass 매개변수를 INTELLIGENT_TIERING으로 설정하여 다이렉트 인텔리전트 티어링을 구현하는 것입니다. 자세한 내용은 [SDK 개요](https://intl.cloud.tencent.com/document/product/436/6474)를 참고하십시오.



## 사용 제한

인텔리전트 티어링 사용에는 다음과 같은 제한이 있습니다.

- **설정 제한**: 초기 설정 후 변경이 불가능합니다. 변경이 필요한 경우 [문의](https://intl.cloud.tencent.com/contact-sales)하십시오.
- **초기 스토리지 레이어 제한**: 인텔리전트 티어링 유형의 신규 객체는 기본적으로 고빈도 액세스 레이어에 저장됩니다. 일정 기간 동안 액세스가 없는 상태가 지속되어야만 저빈도 액세스 레이어로 전환됩니다.
- **최소 스토리지 단위 제한**: 64KB 이하 객체는 고빈도 액세스 레이어에 영구 저장되며, 고빈도 액세스 레이어와 저빈도 액세스 레이어 간 전환은 불가합니다. 단일 스토리지 파일의 크기에 관계없이 실제 데이터 크기에 따라 과금됩니다.
- **작업 제한**: 추가 업로드 인터페이스 통해 객체를 인텔리전트 티어링 유형으로 업로드하는 기능은 지원하지 않습니다.
- **라이프사이클 제한**: 인텔리전트 티어링 유형은 아카이브 또는 딥 아카이브 유형으로만 전환될 수 있습니다. 스탠다드 스토리지 유형이 인텔리전트 티어링 유형으로 전환될 때는 고빈도 액세스 레이어에 저장되며, 스탠다드IA 스토리지 유형이 인텔리전트 티어링 유형으로 전환될 때는 스탠다드IA 스토리지 액세스 레이어에 저장됩니다.
- **버킷 복사 제한**: 버킷 복사 시, 타깃 버킷의 인텔리전트 티어링 설정이 활성화되지 않았을 경우 객체를 인텔리전트 티어링 유형으로 복사할 수 없습니다.

## FAQ
### 인텔리전트 티어링은 어떻게 과금되나요?

인텔리전트 티어링에는 **인텔리전트 티어링 용량 요금** 및 **인텔리전트 티어링 객체 모니터링 요금**이 포함됩니다. 그 중:
1. 인텔리전트 티어링 용량 요금은 파일이 위치한 스토리지 레이어에 따라 다른 스토리지 요금을 부과합니다.
 - 파일이 고빈도 레이어에 있을 경우 스탠다드 스토리지 요금에 따라 과금됩니다.
 - 파일이 스탠다드IA 레이어에 있을 경우 스탠다드IA 스토리지 용량 요금에 따라 과금됩니다.
>?
>- 스탠다드 스토리지 및 스탠다드IA 스토리지 용량 요금은 퍼블릭 클라우드 리전에 따라 가격이 상이하며, 구체적인 가격은 [제품 가격](https://intl.cloud.tencent.com/pricing/cos)을 참고하십시오.
>- 파일 업로드 및 다운로드 과정에서 요청 요금 및 트래픽 요금도 발생하며, 이러한 요금 계산 예시는 [트래픽 요금 과금 사례](https://intl.cloud.tencent.com/document/product/436/33776) 및 [요청 요금 과금 사례](https://intl.cloud.tencent.com/document/product/436/40100)를 참고하십시오.
>
2. 인텔리전트 티어링 객체 모니터링 요금은 저장된 파일 수량을 기준으로 계산되며 모니터링 객체 10,000개마다 월 0.025USD가 청구됩니다.

**예시**

회사에 10MB 크기의 파일 총 100,000개로 구성된 1TB의 파일이 있고, 데이터는 인텔리전트 티어링 형태의 스토리지로 베이징 지역에 저장되어 있다고 가정합니다. 또한, 매월 20%의 파일(20,000개 파일)이 저빈도 레이어로 전환된다고 가정하면 월별 객체 모니터링 및 스토리지 요금은 다음 표와 같습니다.

|스토리지 월|객체 모니터링 요금(USD)|인텔리전트 티어링 요금(USD)|스탠다드 스토리지 요금(USD)|
|----|----|----|----|
|1| 0.25|1024 x 0.024 = 24.58  |   1024 x 0.024 = 24.58  |
|2|0.25|819.2 x 0.024 + 204.8 x 0.018 = 23.35| 1024 x 0.024 = 24.58  |
|3|0.25|655.36 x 0.024 + 368.64 x 0.018 = 22.36 |  1024 x 0.024 = 24.58  |
|4|0.25|524.288 x 0.024 + 499.712 x 0.018 = 21.58 |  1024 x 0.024 = 24.58 |
|5|0.25|419.4304 x 0.024 + 604.5696 x 0.018 = 20.95  |  1024 x 0.024 = 24.58  |
|6|0.25|335.54432 x 0.024 + 688.45568 x 0.018 = 20.45  | 1024 x 0.024 = 24.58 |

스토리지 기간이 길어질수록 매월 소액의 모니터링 비용만 지불하면 되므로 상당한 비용 절감 효과를 얻을 수 있음을 알 수 있습니다.

### 인텔리전트 티어링은 어떤 유형의 파일에 적용됩니까?

인텔리전트 티어링은 오디오/비디오, 로그와 같이 파일 크기가 평균적으로 큰 파일에 적합하며 액세스 모드는 고정되어 있지 않습니다. 평균 파일 용량이 클수록 각 파일의 GB당 지불해야 하는 모니터링 비용이 줄어듭니다. 비즈니스 액세스 모드가 상대적으로 고정되어 있으면 인텔리전트 티어링을 사용할 필요 없이 라이프사이클을 통해 지정된 시간을 설정하여 스탠다드IA 스토리지로 전환할 수 있습니다.

### 인텔리전트 티어링으로 파일을 저장하는 방법은 무엇입니까?
다음 두 가지 방법으로 파일을 인텔리전트 티어링으로 저장할 수 있습니다.
- 추가 파일: 업로드 시 스토리지 유형을 인텔리전트 티어링으로 지정하기만 하면 파일을 인텔리전트 티어링으로 저장할 수 있습니다.
- 기존 파일: COPY 인터페이스를 통해 파일 스토리지 유형을 인텔리전트 티어링 유형으로 수정하거나 라이프사이클 기능을 사용하여 스탠다드 스토리지 및 스탠다드IA 스토리지 유형을 인텔리전트 티어링 유형으로 전환할 수 있습니다.

>! 인텔리전트 티어링 객체가 64KB보다 작으면 항상 스탠다드로 저장됩니다. 따라서 64KB 미만의 파일은 필요에 따라 스탠다드, 스탠다드_IA, 아카이브 또는 딥 아카이브 등 스토리지 유형으로 직접 업로드하는 것이 좋습니다. 이렇게 하면 비용을 절감할 수 있습니다.
>

### 인텔리전트 티어링 설정을 끄는 방법은 무엇입니까?

인텔리전트 티어링은 활성화 후 **비활성화할 수 없습니다**. 파일을 인텔리전트 티어링으로 저장할 필요가 없는 경우 파일을 업로드할 때 파일 스토리지 유형을 스탠다드 스토리지, 스탠다드IA 스토리지, 아카이브 또는 딥 아카이브로 지정하기만 하면 됩니다.
