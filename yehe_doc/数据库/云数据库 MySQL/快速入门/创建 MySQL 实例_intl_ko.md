
본 문서에서는 콘솔에서 TencentDB for MySQL 인스턴스를 생성하는 방법을 설명합니다.

## 전제 조건
Tencent Cloud 계정을 등록하고 본인 확인을 완료합니다.
- Tencent Cloud 계정 생성 방법:
<div style="background-color:#00A4FF; width: 270px; height: 35px; line-height:35px; text-align:center;"><a href="https://intl.cloud.tencent.com/register?s_url=https%3A%2F%2Fcloud.tencent.com%2F" target="_blank"  style="color: white; font-size:16px;" hotrep="document.guide.3128.btn1">Tencent Cloud 회원가입하기</a></div>

- 실명 인증 방법:
<div style="background-color:#00A4FF; width: 270px; height: 35px; line-height:35px; text-align:center;"><a href="https://console.cloud.tencent.com/developer" target="_blank"  style="color: white; font-size:16px;"  hotrep="document.guide.3128.btn2">실명 인증하기</a></div>

## 2노드, 3노드 인스턴스 구매
1. [TencentDB for MySQL 구매 페이지](https://buy.intl.cloud.tencent.com/cdb)에 로그인하여 다음 인스턴스 정보를 구성한 후 **즉시 구매**를 클릭합니다.
 - **과금 방식**: 정액 과금제 및 종량제가 지원됩니다.
    - 장기 수요가 안정적인 비즈니스라면 정액 과금제를 권장합니다.
    - 비즈니스의 요청량이 순간적으로 크게 변동하는 경우 종량제를 권장합니다.
 - **리전**: TencentDB for MySQL 인스턴스를 배포할 리전을 선택합니다. 연결할 CVM 인스턴스와 동일한 리전을 사용하는 것이 좋습니다. 다른 리전의 Tencent Cloud 서비스는 사설망을 통해 서로 통신할 수 없습니다. 리전은 구매 후 수정할 수 없습니다.
 - **데이터베이스 버전**: 현재 TencentDB for MySQL은 MySQL 8.0, MySQL 5.7, MySQL 5.6 및 MySQL 5.5를 지원합니다. 각 버전의 기능에 대한 자세한 내용은 [공식 문서](https://dev.mysql.com/doc/refman/5.7/en/)를 참고하십시오.
 - **엔진**: InnoDB 또는 RocksDB를 선택합니다.
    - InnoDB: 가장 일반적으로 사용되는 OLTP 스토리지 엔진으로 완전한 트랜잭션 지원과 동시 읽기/쓰기의 강력한 기능을 제공합니다.
    - RocksDB: 효율적인 쓰기 및 높은 압축 기능을 갖춘 key-value 스토리지 엔진입니다. RocksDB 엔진을 선택하면 아키텍처가 2노드가 됩니다.
  - **아키텍처**: 2노드, 3노드, 단일 노드 버전을 제공합니다. 각 아키텍처에 관한 소개는 [데이터베이스 아키텍처](https://intl.cloud.tencent.com/document/product/236/38328)를 참고하십시오.
  - **데이터 복제 모드**: 비동기, 반동기 및 강력한 동기화 복제 모드가 지원됩니다. 자세한 내용은 [데이터베이스 인스턴스 복사](https://intl.cloud.tencent.com/document/product/236/7913)를 참고하십시오.
 - **원본 가용존 및 복제본 가용존**: 다른 원본 및 복제본 가용존(즉, [멀티 가용존 배포](https://intl.cloud.tencent.com/document/product/236/8459))을 선택하여 장애 및 가용존 중단으로부터 데이터베이스를 보호합니다.
>?
>- 원본/복제본이 서로 다른 가용존에 있으면, 네트워크 동기화가 2ms - 3ms 가량 딜레이 될 수 있습니다.
>- 클라우드 서비스 구매 시 액세스 지연 시간 단축 및 다운로드 속도 향상을 위해, 가장 가까운 리전 선택을 권장합니다.
>
 - **인스턴스 유형**: 일반 또는 전용. 자세한 내용은 [격리 정책](https://intl.cloud.tencent.com/document/product/236/39794)을 참고하십시오.
 - **인스턴스 사양**: 필요에 따라 사양을 선택합니다.
 - **하드 디스크**: 디스크 공간은 MySQL 실행에 필요한 파일을 저장하는 데 사용됩니다.
 - **네트워크**: TencentDB for MySQL 인스턴스가 상주하는 네트워크를 선택합니다. 기본적으로 Default-VPC(기본값)입니다. 연결할 CVM 인스턴스와 동일한 리전에서 동일한 VPC를 선택하는 것이 좋습니다. 그렇지 않으면 MySQL 인스턴스가 사설망을 통해 CVM 인스턴스에 연결할 수 없습니다.
 - **사용자 정의 포트**: 기본적으로 3306인 데이터베이스 액세스 포트입니다.
 - **보안 그룹**: 보안 그룹의 생성 및 관리에 관한 내용은 [CDB 보안 그룹 관리](https://intl.cloud.tencent.com/document/product/236/14470)를 참고하십시오.
>?Port 3306은 보안 그룹의 인바운드 규칙을 통해 MySQL 인스턴스에 대해 열려 있어야 합니다. 인스턴스는 기본적으로 사설망 포트 3306을 사용하며 사용자 정의 포트를 지원합니다. 기본 포트가 변경되면 보안 그룹에서 새 포트를 열어야 합니다.
 - **매개변수 템플릿**: TencentDB에서 제공하는 시스템 매개변수 템플릿 외에 사용자 정의 매개변수 템플릿을 생성할 수 있습니다. 자세한 내용은 [매개변수 템플릿 사용](https://intl.cloud.tencent.com/document/product/236/31906)을 참고하십시오.
  - **문자 세트**: LATIN1, GBK, UTF8 및 UTF8MB4 문자 세트가 지원됩니다. 기본값은 UTF8입니다. 인스턴스를 구매한 후 콘솔의 인스턴스 세부 정보 페이지에서 문자 세트를 변경할 수 있습니다. 자세한 내용은 [문자 세트 설명](https://intl.cloud.tencent.com/document/product/236/7259)을 참고하십시오.
 - **테이블 이름 대소문자 구분**: 테이블 이름이 대소문자를 구분하는지 여부. 기본값은 사용입니다.
 - **비밀번호 복잡성**: 데이터베이스 보안성을 향상시키기 위한 비밀번호 복잡도 설정을 지원합니다. 기본값은 비활성화되어 있습니다. 자세한 내용은 [비밀번호 복잡성 설정](https://www.tencentcloud.com/document/product/236/49197)을 참고하십시오.
 - **root 비밀번호**: root 계정의 비밀번호를 설정합니다(새 MySQL 데이터베이스의 기본 사용자 이름은 root). **생성 후 설정**을 선택하면 인스턴스 생성 후 [비밀번호 재설정](https://intl.cloud.tencent.com/document/product/236/31901)을 할 수 있습니다.
 - **알람 정책**: Tencent Cloud 리소스 상태가 변경될 때 알람을 트리거하고 메시지를 보내는 알람 정책을 생성할 수 있습니다. 자세한 내용은 [알람 정책(Cloud Monitoring)](https://intl.cloud.tencent.com/document/product/236/8457)을 참고하십시오.
 - **프로젝트**: TencentDB 인스턴스가 속한 프로젝트를 선택합니다. 기본 프로젝트가 사용됩니다.
 - **태그**: 태그를 사용하여 리소스를 분류하고 관리합니다. 자세한 내용은 [태그 개요](https://intl.cloud.tencent.com/document/product/236/31917)를 참고하십시오.
 - **인스턴스 이름**: 지금 또는 나중에 인스턴스의 이름을 지정합니다.
 - **구매 수량**: 각 가용존에서 종량제 인스턴스를 최대 10개까지 구매할 수 있습니다.
 - **구매 기간**: 비즈니스 요구 사항에 따라 원하는 기간을 선택합니다. 과금 방식은 정액 과금제이며, 구매 기간이 길수록 할인율이 높아집니다.
 - **자동 연장**: 정액 과금제 과금 방식에서 계정 잔액이 충분한 경우 만료 시 매월 디바이스를 자동 연장합니다.
 - **서비스 약관**: 자세한 내용은 [Terms of Service](https://intl.cloud.tencent.com/document/product/236/35543)를 참고하십시오.
2. 인스턴스 구매 후 인스턴스 목록으로 돌아갑니다. 인스턴스는 **발송 중**상태가 됩니다. 인스턴스 상태가 **실행 중**으로 변경되면 약 3 - 5분 후에 인스턴스를 사용할 수 있습니다.

## 단일 노드 인스턴스 구매
>?클라우드 디스크 버전의 단일 노드 아키텍처는 현재 베타 테스트 단계입니다. 사용해 보려면 [티켓 제출](https://console.cloud.tencent.com/workorder/category)하십시오.
>
1. [TencentDB for MySQL 구매 페이지](https://buy.intl.cloud.tencent.com/cdb)에 로그인하여 다음 인스턴스 정보를 구성한 후 **즉시 구매**를 클릭합니다.
 - **과금 방식**: 정액 과금제 및 종량제가 지원됩니다.
    - 장기 수요가 안정적인 비즈니스라면 정액 과금제를 권장합니다.
    - 비즈니스의 요청량이 순간적으로 크게 변동하는 경우 종량제를 권장합니다.
 - **리전**: TencentDB for MySQL 인스턴스를 배포할 리전을 선택합니다. 이 아키텍처는 현재 상하이 리전에서 지원되며 앞으로 더 많은 리전에서 사용할 수 있습니다. 연결할 CVM 인스턴스와 동일한 리전을 사용하는 것이 좋습니다. 다른 리전의 Tencent Cloud 서비스는 사설망을 통해 서로 통신할 수 없습니다. 리전은 구매 후 수정할 수 없습니다.
 - **데이터베이스 버전**: 현재 TencentDB for MySQL은 단일 노드 아키텍처에 대해 MySQL 8.0 및 5.7을 지원합니다. 각 버전의 기능에 대한 자세한 내용은 [MySQL 5.7 레퍼런스 매뉴얼](https://dev.mysql.com/doc/refman/5.7/en/)를 참고하십시오.
 - **엔진**: 기본값은 InnoDB 엔진입니다. 가장 일반적으로 사용되는 OLTP 스토리지 엔진으로 완전한 트랜잭션 지원과 동시 읽기/쓰기의 강력한 기능을 제공합니다.
 - **아키텍처**: 단일 노드를 선택합니다.
 >!기본 버전은 장애 복구에 시간이 오래 걸리므로 프로덕션 환경에서는 2노드 또는 3노드 버전 사용을 권장합니다. 최대 99.99%의 가용성을 보장합니다.
 - **디스크 유형**: 클라우드 디스크. 디스크 유형에 대한 정보는 [Cloud Disk Types](https://intl.cloud.tencent.com/document/product/362/31636)를 참고하십시오.
 - **가용존**: 인스턴스 배포를 위한 가용존을 선택합니다. 동일한 VPC의 서로 다른 가용존에 있는 Tencent Cloud 서비스는 사설망을 통해 서로 통신할 수 있습니다. 예를 들어 상하이 2존의 CVM 인스턴스는 사설망을 통해 동일한 VPC의 상하이 3존에 있는 MySQL 인스턴스에 액세스할 수 있습니다.
 - **인스턴스 사양**: 필요에 따라 사양을 선택합니다.
 - **디스크**: 디스크 공간은 MySQL 실행에 필요한 파일을 저장하는 데 사용됩니다. SSD 클라우드 디스크 또는 인핸스드 SSD를 선택할 수 있습니다. 지원되는 디스크 용량 범위는 20GB - 32000GB입니다. 자세한 내용은 [Cloud Disk Types](https://intl.cloud.tencent.com/document/product/362/31636)를 참고하십시오.
>?다음 구성 단계는 2노드/3노드 인스턴스 구매와 동일합니다. 자세한 내용은 [구매 방식](https://intl.cloud.tencent.com/document/product/236/5160)을 참고하십시오.
2. 인스턴스 구매 후 인스턴스 목록으로 돌아갑니다. 인스턴스는 **발송 중**상태가 됩니다. 인스턴스 상태가 **실행 중**으로 변경되면 약 3min - 5min 후에 인스턴스를 사용할 수 있습니다.

## 후속 작업
Windows 또는 Linux CVM 인스턴스에서 사설망 및 공중망을 통해 TencentDB for MySQL 인스턴스에 액세스할 수 있습니다. 자세한 내용은 [MySQL 인스턴스 연결](https://intl.cloud.tencent.com/document/product/236/37788)을 참고하십시오.
