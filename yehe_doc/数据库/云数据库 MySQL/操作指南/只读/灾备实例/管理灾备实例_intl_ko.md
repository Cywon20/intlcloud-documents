## 작업 시나리오
서비스 연속성, 데이터 안정성 및 규정 준수에 대한 요구 사항이 높은 애플리케이션의 경우 TencentDB for MySQL은 리전 간 재해 복구 인스턴스를 제공하여 낮은 비용으로 지속적인 서비스를 제공하고 데이터 안정성을 개선할 수 있는 기능을 향상하는 데 도움이 됩니다.
>?재해 복구 인스턴스 요금과 원본 인스턴스에 대한 내용은 [제품 가격](https://buy.intl.cloud.tencent.com/price/cdb)을 참고하십시오.

#### 기능 특징
- 독립된 데이터베이스 연결 주소를 제공합니다. 재해 복구 인스턴스는 읽기 액세스 기능을 제공할 수 있으며 근거리 액세스, 데이터 분석 등 시나리오에 사용되어 디바이스 이중화 비용을 절감할 수 있습니다.
- 가용성 높은 원본/복제본 아키텍처로 데이터베이스 단일 노드 구성으로 인한 리스크를 예방할 수 있습니다.
- 재해 복구 인스턴스는 내부 네트워크 전용 회선을 통해 동기화하여 동기화 딜레이 시간을 줄이고 안정성을 높일 수 있으며 외부 네트워크보다 우수한 품질의 동기화 링크를 제공합니다. 
- 현재는 프로모션 기간으로 전용 회선 트래픽을 무상으로 이용할 수 있습니다. 상용화 후 요금 부과 시 별도로 공지합니다. 

#### 작동 원리
- Tencent CDB를 재해 복구 데이터베이스로 사용하는 시나리오에서 재해 복구 인스턴스는 원본 인스턴스 데이터베이스를 복사한 백업입니다.
- 원본 인스턴스에 변화가 발생하면 기록이 수정된 Log 정보는 재해 복구 인스턴스에 복사된 후 로그 리플레이를 통해 데이터가 동기화됩니다.
- 원본 인스턴스에 장애가 발생하면 몇 초 내에 재해 복구 인스턴스가 활성화되며 읽기/쓰기 기능이 완전히 복구됩니다.

## 기능 제한
- 클라우드 디스크 버전의 단일 노드 인스턴스에 대해 재해 복구 인스턴스를 생성할 수 없습니다.
- 1GB 메모리, 50GB 디스크 및 그 이상의 사양과 더불어 MySQL 5.6 이상 버전 및 InnoDB 엔진 고가용성 버전의 원본 인스턴스만 재해 복구 인스턴스를 구매할 수 있습니다. 원본 인스턴스의 사양이 낮을 경우, 먼저 사양을 업그레이드하고 GTID 기능을 사전에 활성화하십시오. 
>?GTID를 활성화하지 않은 경우, [콘솔](https://console.cloud.tencent.com/cdb/) 인스턴스 상세 페이지에서 활성화할 수 있습니다. GTID 활성화는 시간이 많이 소요되며 인스턴스 연결이 몇 초 정도 끊길 수 있습니다. 피크 시간대를 피해 작업하시고 데이터베이스 액세스의 재연결 메커니즘을 확보할 것을 권장합니다.
- 재해 복구 인스턴스의 최저 사양은 1GB 메모리, 50GB 디스크이며, 원본 인스턴스에 사용된 스토리지 사양의 1.1배 이상이어야 합니다.
- 한 개의 원본 인스턴스는 최대 한 개의 재해 복구 인스턴스를 생성할 수 있습니다. 격리 상태 중에는 재해 복구 인스턴스를 재생성할 수 없습니다.
- 현재 재해 복구 인스턴스의 미지원 기능: 마이그레이션, 롤백, SQL 작업, 문자 세트 변경, 계정 관리, 포트 변경, 데이터 가져오기, 로그 롤백, 읽기 전용 인스턴스 기능.

## 재해 복구 인스턴스 생성
#### 1단계: 재해 복구 인스턴스 생성
1. [TencentDB for MySQL 콘솔](https://console.cloud.tencent.com/cdb/)에 로그인한 후, 인스턴스 리스트에서 인스턴스 ID 또는 **작업** 열의 **관리**를 클릭하여 세부 정보 페이지로 이동합니다.
2. 인스턴스 상세 페이지에서 인스턴스의 기본 정보를 확인하여 GTID 기능이 활성화되어 있는지 확인합니다. 인스턴스 아키텍처 다이어그램에서 **재해 복구 인스턴스 추가**를 클릭하여 재해 복구 인스턴스 구매 페이지로 이동합니다.
![](https://qcloudimg.tencent-cloud.cn/raw/3535c0274c3dfa18134ccc735edd0f46.png)
3. 구매 페이지에서 재해 복구 인스턴스의 **과금 방식**, **리전**, **동기화 정책** 등 기본 정보를 설정합니다.
 - 동기화 정책이 **즉시 동기화**인 경우 재해 복구 인스턴스가 생성되는 즉시 데이터가 동기화됩니다.
 - 동기화 정책이 **생성 후 동기화**인 경우 인스턴스가 성공적으로 생성된 후 재해 복구 동기화 링크를 구성해야 합니다. 자세한 사용법은 아래의 [동기화 링크 생성](#cjtblj)을 참고하십시오.
>?
>- 생성을 완료하는 데 필요한 시간은 데이터 양에 따라 다르며 생성 중에는 콘솔의 원본 인스턴스에 대해 어떤 작업도 수행할 수 없습니다. 적절한 시기에 하는 것이 좋습니다.
>- 현재 전체 인스턴스 데이터 동기화만 지원합니다. 디스크 용량이 충분한지 확인하십시오.
>- 원본 인스턴스가 실행 상태이고 구성 조정 작업, 다시 시작 작업 및 기타 수정 작업이 실행되지 않았는지 확인해야 합니다. 그렇지 않으면 동기화 작업이 실패할 수 있습니다.  
4. 모든 항목이 올바른지 확인한 후 **즉시 구매**를 클릭하고 재해 복구 인스턴스가 전달될 때까지 기다립니다.
5. 인스턴스 리스트로 되돌아갑니다. 인스턴스 상태가 **실행 중**으로 변경되면, 정상적으로 사용할 수 있습니다.

## 재해 복구 인스턴스 관리
- **재해 복구 인스턴스 조회**
재해 복구 인스턴스는 소속 리전에서 조회할 수 있으며 인스턴스 리스트에서 해당 리전의 전체 재해 복구 인스턴스를 필터링할 수 있습니다.
![](https://qcloudimg.tencent-cloud.cn/raw/096486c7562d5385bfd50a187f85b307.png)
- **원본 인스턴스와 재해 복구 읽기 전용 인스턴스 간의 관계 보기**
각 재해 복구 인스턴스 또는 원본 인스턴스 우측의 이미지를 클릭하면 종속 관계를 확인할 수 있습니다.
![](https://qcloudimg.tencent-cloud.cn/raw/89d342733d4c2264dd5dc00a1c7d4143.png)
- **동기화 딜레이 조회**
재해 복구 인스턴스의 인스턴스 상세 페이지 상단에서 원본 인스턴스와 재해 복구 인스턴스 간의 동기화 딜레이를 확인할 수 있습니다.
![](https://qcloudimg.tencent-cloud.cn/raw/053c38735dd3dd5b7e46d5c82687dd09.png)
- **재해 복구 인스턴스 기능**
재해 복구 인스턴스에는 인스턴스 세부 정보 보기, 인스턴스 모니터링, 데이터베이스 관리, 보안 그룹, 백업 복구와 같은 다양한 기능이 제공됩니다.

## 재해 복구 인스턴스를 소스 인스턴스로 업그레이드
재해 복구 인스턴스를 원본 인스턴스로 전환해야 할 경우 콘솔에서 자체적으로 전환할 수 있습니다. 
>!
>- 재해 복구 인스턴스가 원본 인스턴스로 업그레이드된 후 기존 인스턴스가 새 원본 인스턴스로 대체되고 액세스 주소가 변경됩니다. 비즈니스측에서 새 주소로 업데이트해야 합니다.
>- 재해 복구 인스턴스를 원본 인스턴스로 전환한 후, 기존 원본 인스턴스에 대한 동기화 링크가 끊어지고 다시 연결할 수 없습니다. 유의하시기 바랍니다.
>
1. [인스턴스 리스트](https://console.cloud.tencent.com/cdb)에서 원본 인스턴스로 업그레이드할 재해 복구 인스턴스를 선택한 후, 인스턴스 ID를 클릭하면 인스턴스 관리 페이지로 이동합니다.
2. 인스턴스 관리 페이지에서 우측 상단의 **원본 인스턴스로 전환**을 클릭하면 재해 복구 인스턴스가 원본 인스턴스로 업그레이드됩니다. 전환 후에는 기존 원본 인스턴스와의 동기화 연결이 끊어지고 인스턴스 데이터베이스의 데이터 입력 기능과 MySQL 기능이 복구됩니다.
![](https://qcloudimg.tencent-cloud.cn/raw/5eb3f1f08b46d4c20ab3b4038309ab03.png)



