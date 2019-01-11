---
title: Microsoft Identity Manager 2016 모범 사례 | Microsoft Docs
description: ''
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/05/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: 8987bc53af37b32b95b00c3df67d9581d4e47120
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358859"
---
# <a name="microsoft-identity-manager-2016-best-practices"></a>Microsoft Identity Manager 2016 모범 사례

이 항목에서는 MIM(Microsoft Identity Manager) 2016을 배포 및 작동하기 위한 모범 사례를 설명합니다.

## <a name="sql-setup"></a>SQL 설정
> [!NOTE]
> SQL을 실행하는 서버를 설정하기 위한 다음 권장 사항은 FIMService 전용 SQL 인스턴스 및 FIMSynchronizationService 데이터베이스 전용 SQL 인스턴스가 있다고 가정합니다. 통합 환경에서 FIMService를 실행하는 경우 구성에 적합하게 조정해야 합니다.

SQL(구조적 쿼리 언어) 서버의 구성은 최적의 시스템 성능에 매우 중요합니다. 대규모 구현에서 최적의 MIM 성능을 달성하려면 SQL을 실행하는 서버에서 모범 사례를 적절히 적용하는 과정이 필요합니다. 자세한 내용은 SQL 모범 사례에 대한 다음 항목을 참조하세요.

-   [Storage Top 10 Best Practices](http://go.microsoft.com/fwlink/?LinkID=183663)(저장소 관련 상위 10가지 모범 사례)

-   [Tempdb 성능 최적화](http://go.microsoft.com/fwlink/?LinkID=188267)

-   [SQL Server Best Practices Article](http://go.microsoft.com/fwlink/?LinkID=188268)(SQL Server 모범 사례 문서)

-   [Reorganizing and Rebuilding Indexes](http://go.microsoft.com/fwlink/?LinkID=188269)(다시 구성 및 인덱스 다시 작성)

### <a name="presize-data-and-log-files"></a>데이터 및 로그 파일 사전 크기 조정

자동 증가에 의존하지 마세요. 대신 이러한 파일의 증가를 수동으로 관리합니다. 안전상의 이유로 자동 증가 옵션을 설정할 수 있지만 데이터 파일의 증가를 사전에 관리해야 합니다. MIM 데이터베이스의 샘플 크기에 대한 자세한 내용은 [FIM Capacity Planning Guide](http://go.microsoft.com/fwlink/?LinkID=185246)(FIM 용량 계획 가이드)를 참조하세요.

### <a name="to-presize-sql-data-and-log-files"></a>SQL 데이터 및 로그 파일 크기를 사전에 조정하려면

1.  SQL Server Management Studio를 시작합니다.

2.  FIMService 데이터베이스로 이동한 후 [FIMService]를 마우스 오른쪽 단추로 클릭하고 [속성]을 클릭합니다.

3.  [파일] 페이지에서 데이터베이스 파일을 필요한 크기로 확장합니다.

### <a name="isolate-log-from-data-files"></a>데이터 파일에서 로그 격리

SQL Server 모범 사례에 따라 데이터베이스에 대한 트랜잭션 및 데이터 로그 파일을 별도의 물리적 디스크에 격리합니다.

추가 tempdb 파일 만들기

최적의 성능을 위해서는 tempdb 파일에 CPU 코어당 하나의 데이터 파일을 만드는 것이 좋습니다.

### <a name="to-create-additional-tempdb-files"></a>추가 tempdb 파일을 만들려면

1.  SQL Server Management Studio를 시작합니다.

2.  시스템 데이터베이스의 tempdb 데이터베이스로 이동한 후 tempdb를 마우스 오른쪽 단추로 클릭한 다음 [속성]을 클릭합니다.

3.  파일 페이지에서 각 CPU 코어에 대해 하나의 데이터 파일을 만듭니다. tempdb 데이터 및 로그 파일을 다른 드라이브 및 스핀들에 분리해야 합니다.

### <a name="ensure-adequate-space-for-log-files"></a>로그 파일에 대한 적절한 공간 확인

복구 모델의 디스크 요구 사항을 이해하는 것이 중요합니다. 초기 시스템 로드 중에는 디스크 공간 사용을 제한하기 위해 단순 복구 모드가 적절할 수 있지만 가장 최근 백업 이후에 만들어진 데이터는 손실되기 쉽습니다. 전체 복구 모드를 사용할 경우 과도한 디스크 공간 사용을 방지하기 위해 트랜잭션 로그의 잦은 백업을 비롯한 백업을 통해 디스크 사용량을 관리해야 합니다. 자세한 내용은 [복구 모델 개요](http://go.microsoft.com/fwlink/?LinkID=185370)를 참조하세요.

### <a name="limit-sql-server-memory"></a>SQL Server 메모리 제한

SQL Server를 다른 서비스(즉, MIM 2016 서비스 및 MIM 2016 동기화 서비스)와 함께 공유하는 경우 SQL Server에 있는 메모리 크기에 따라, SQL의 메모리 사용량을 제한하려고 할 수 있습니다. 이렇게 하려면 다음 단계를 수행합니다.

1. SQL Server Enterprise 관리자를 시작합니다.

2. [새 쿼리]를 선택합니다.

3. 다음과 같은 쿼리를 실행합니다.

   ```SQL
   USE master

   EXEC sp_configure 'show advanced options', 1

   RECONFIGURE WITH OVERRIDE

   USE master

   EXEC sp_configure 'max server memory (MB)', 12000--- max=12G RECONFIGURE
   WITH OVERRIDE
   ```

   이 예제에서는 최대 12GB(기가바이트) 메모리를 사용하도록 SQL 서버를 다시 구성합니다.

4. 다음 쿼리를 사용하여 설정을 확인합니다.

   ```SQL
   USE master

   EXEC sp_configure 'max server memory (MB)'--- verify the setting

   USE master

   EXEC sp_configure 'show advanced options', 0

   RECONFIGURE WITH OVERRIDE
   ```

### <a name="backup-and-recovery-configuration"></a>백업 및 복구 구성

일반적으로 데이터베이스 관리자와 함께 백업 및 복구 전략을 설계해야 합니다. 일부 권장 사항은 다음과 같습니다.
- 조직의 백업 정책에 따라 데이터베이스 백업을 수행합니다. 
- 증분 로그 백업을 계획하지 않은 경우 데이터베이스를 단순 복구 모드로 설정해야 합니다. 
- 백업 전략을 구현하기 전에 다른 복구 모델의 의미를 파악해야 합니다. 이러한 모델의 디스크 공간 요구 사항을 확인하세요. 전체 복구 모델에서는 디스크 공간을 과도하게 사용하지 않도록 로그를 자주 백업해야 합니다. 

자세한 내용은 [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)(복구 모델 개요) 및 [FIM 2010 Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)(FIM 2010 백업 및 복원 가이드)를 참조하세요.

## <a name="create-a-backup-administrator-account-for-the-fim-service-after-installation"></a>설치 후에 FIM Service에 대한 백업 관리자 계정 만들기

FIMService 관리자 집합의 구성원은 MIM 배포의 작동에 중요한 고유한 사용 권한을 갖습니다. 관리자 집합의 일부로 로그온할 수 없는 경우 유일한 해결 방법은 시스템의 이전 백업으로 롤백하는 것입니다. 이 문제를 해결하려면 다른 사용자를 설치 후 구성의 일부로 FIM 관리 집합에 추가하는 것이 좋습니다.

## <a name="fim-service"></a>FIM 서비스


### <a name="configuring-fim-service-service-exchange-mailbox"></a>FIM 서비스 서비스 Exchange 사서함 구성

MIM 2016 Service 서비스 계정에 대해 Microsoft Exchange Server를 구성하기 위한 모범 사례는 다음과 같습니다.

- 내부 전자 메일 주소의 메일만 수락할 수 있도록 서비스 계정을 구성합니다. 특히, 서비스 계정 사서함은 외부 SMTP 서버의 메일을 받을 수 없어야 합니다.

#### <a name="to-configure-the-service-account"></a>서비스 계정을 구성하려면

1.  Exchange 관리 콘솔에서 **FIM 서비스 계정**을 선택합니다.

2.  [속성]을 선택하고 [메일 흐름 설정]을 선택한 다음 **메일 배달 제한**을 선택합니다.

3.  **보낸 사람 모두 인증 필요** 확인란을 선택합니다.

자세한 내용은 [메시지 배달 제한 구성](http://go.microsoft.com/fwlink/?LinkID=183625)을 참조하세요.

-   크기가 1MB보다 큰 메일은 거부하도록 서비스 계정을 구성합니다. 모범 사례를 따라 사서함 또는 메일 사용이 가능한 공용 폴더에 대해 [메시지 크기 제한을 구성](http://go.microsoft.com/fwlink/?LinkID=183626)합니다.

-   사서함 스토리지 할당량이 5GB가 되도록 서비스 계정을 구성합니다. 최적의 결과를 얻으려면 [Configure Storage Quotas for a Mailbox](http://go.microsoft.com/fwlink/?LinkID=156929)(사서함에 대한 스토리지 할당량 구성)에 나열되는 모범 사례를 따르세요.

## <a name="mim-portal"></a>MIM 포털


### <a name="disable-sharepoint-indexing"></a>SharePoint 인덱싱 사용 안 함

Microsoft Office SharePoint® 인덱싱을 사용하지 않도록 설정하는 것이 좋습니다. 인덱스화해야 할 문서가 없습니다. 인덱스를 수행하면 많은 오류 로그 항목 및 MIM의 잠재적인 성능 문제가 발생합니다. SharePoint 인덱싱을 사용하지 않으려면 아래 단계를 수행하세요.

1.  MIM 2016 포털을 호스트하는 서버에서 [시작]을 클릭합니다.

2.  [모든 프로그램]을 클릭합니다.

3.  [모든 프로그램] 목록에서 [관리 도구]를 클릭합니다.

4.  [관리 도구]에서 [SharePoint 중앙 관리]를 클릭합니다.

5.  중앙 관리 페이지에서 [작업]을 클릭합니다.

6.  [작업] 페이지의 [글로벌 구성]에서 [타이머 작업 정의]를 클릭합니다.

7.  [타이머 작업 정의] 페이지에서 [SharePoint Services Search 새로 고침]을 클릭합니다.

8.  타이머 작업 편집 페이지에서 사용 안 함을 클릭합니다.

## <a name="mim-2016-initial-data-load"></a>MIM 2016 초기 데이터 로드

이 섹션에는 외부 시스템에서 MIM으로 수행되는 초기 데이터 로드의 성능을 향상시키기 위한 일련의 단계가 나와 있습니다. 이러한 단계 여러 개는 시스템의 초기 입력 동안에만 수행된다는 것을 알아두어야 합니다. 로드 완료 시 재설정해야 합니다. 이 작업은 일회성 작업이며 연속되는 동기화가 아닙니다.

> [!NOTE]
> MIM과 AD DS(Active Directory Domain Services) 간에 사용자를 동기화하는 방법에 대한 자세한 내용은 FIM 설명서에서 [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277)(Active Directory에서 FIM으로 사용자를 동기화하는 방법)을 참조하세요.
> 
> [!IMPORTANT]
> 이 가이드의 SQL 설정 섹션에 설명되는 모범 사례를 적용했는지 확인합니다. 

### <a name="step-1-configure-the-sql-server-for-initial-data-load"></a>1단계: 초기 데이터 로드에 대해 SQL Server 구성
데이터의 초기 로드는 시간이 오래 걸릴 수 있습니다. 처음에 많은 양의 데이터를 로드하려는 경우 전체 텍스트 검색을 일시적으로 껐다가 MIM 2016 관리 에이전트(FIM MA) 내보내기를 완료한 후 다시 켜서 데이터베이스를 채우는 데 걸리는 시간을 줄일 수 있습니다.

전체 텍스트 검색을 일시적으로 끄려면

1.  SQL Server Management Studio를 시작합니다.

2.  [새 쿼리]를 선택합니다.

3.  다음 SQL 문을 실행합니다.

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = MANUAL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = MANUAL
```

> [!IMPORTANT]
> 이러한 절차를 구현하지 않으면 디스크 공간 사용량이 높아져서 디스크 공간이 부족해질 수 있습니다. [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)(복구 모델 개요)에서 이 항목에 대한 추가 정보를 찾을 수 있습니다. [The FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)(FIM 백업 및 복원 가이드)에 추가 정보가 포함되어 있습니다.

### <a name="step-2-apply-the-minimum-necessary-mim-configuration-during-the-load-process"></a>2단계: 로드 프로세스 동안 필요한 최소 MIM 구성 적용

초기 로드 프로세스 동안 MPR(관리 정책 규칙) 및 집합 정의에 대한 FIM 구성에 필요한 최소 구성만 적용해야 합니다. 데이터 로드가 완료된 후 배포에 필요한 추가 집합을 만듭니다. 해당 정책을 로드된 데이터에 소급해서 적용하려면 작업 워크플로에 대해 정책 업데이트 시 실행 설정을 사용합니다.

### <a name="step-3-configure-and-populate-the-fim-service-with-external-identity-data"></a>3단계: 외부 ID 데이터를 사용해서 FIM 서비스 구성 및 채우기

이 시점에서 Active Directory Domain Services에서 FIM으로 사용자를 동기화하는 방법 가이드에서 설명하는 절차에 따라 Active Directory의 사용자로 시스템을 구성하고 동기화하세요. 그룹 정보를 동기화해야 하는 경우 해당 프로세스에 대한 절차가 [How Do I Synchronize Groups from Active Directory Domain Services to FIM](https://technet.microsoft.com/library/ff686936(v=ws.10).aspx)(Active Directory Domain Services의 그룹을 FIM과 동기화하는 방법) 가이드에 설명되어 있습니다.

#### <a name="synchronization-and-export-sequences"></a>동기화 및 내보내기 시퀀스

성능을 최적화하기 위해 동기화를 실행한 후에 내보내기를 실행하면 커넥터 공간에 보류 중인 내보내기 작업이 많아집니다. 그런 다음 영향을 받는 커넥터 공간과 연결된 관리 에이전트에서 확인 가져오기 실행을 수행합니다. 예를 들어 초기 데이터 로드의 일환으로 일부 관리 에이전트에 대해 동기화 실행 프로필을 실행해야 할 경우 각 개별 동기화 실행 후 내보내기를 실행하고 델타 가져오기를 실행해야 합니다.
초기화 주기에 속하는 각 원본 관리 에이전트에 대해 다음 단계를 수행합니다.

1.  원본 관리 에이전트에 대한 전체 가져오기

2.  원본 관리 에이전트에 대한 전체 동기화

3.  준비된 내보내기 작업을 사용해서 영향을 받는 모든 대상 관리 에이전트에서 내보내기

4.  준비된 내보내기 작업을 사용해서 영향을 받는 모든 대상 관리 에이전트에서 델타 가져오기

### <a name="step-4-apply-your-full-mim-configuration"></a>4단계: 전체 MIM 구성 적용

초기 데이터 로드가 완료되면 배포에 전체 MIM 구성을 적용해야 합니다.

시나리오에 따라 여기에는 추가 집합, MPR 및 워크플로 만들기가 포함될 수 있습니다. 시스템의 모든 기존 개체에 소급해서 적용해야 하는 정책의 경우, 작업 워크플로에 대해 정책 업데이트 시 실행 설정을 사용하여 해당 정책을 로드된 데이터에 소급해서 적용해야 합니다.

### <a name="step-5-reconfigure-sql-to-previous-settings"></a>5단계: SQL을 이전 설정으로 다시 구성

SQL 설정을 일반 설정으로 변경해야 합니다. 여기에는 다음 항목이 포함됩니다.

-   전체 텍스트 검색 켜기

-   조직 정책에 따라 백업 정책 업데이트

초기 데이터 로드를 완료한 후 전체 텍스트 검색을 다시 켜야 합니다. 전체 텍스트 검색을 다시 켜려면 다음 SQL 문을 실행합니다.

```SQL
ALTER FULLTEXT INDEX ON [fim].[ObjectValueString] SET CHANGE_TRACKING = AUTO

ALTER FULLTEXT INDEX ON [fim].[ObjectValueXml] SET CHANGE_TRACKING = AUTO
```

단순 복구 모드로 전환해야 하는 경우 조직의 백업 정책에 따라 백업 일정을 다시 구성해야 합니다. FIM 백업 일정에 대한 사용 가능한 세부 정보는 [FIM 백업 및 복원 가이드](http://go.microsoft.com/fwlink/?LinkID=165864)에서 사용할 수 있습니다.

## <a name="configuration-migration"></a>구성 마이그레이션


### <a name="avoid-changing-display-names"></a>표시 이름 변경 안 함

MPR과 같은 많은 개체 형식에서 syncproduction.ps1 스크립트는 두 시스템 간의 유일한 앵커 특성으로 표시 이름을 사용합니다. 따라서 기존 MPR 표시 이름을 변경하면 기존 MPR이 삭제된 후 새 MPR이 생성됩니다. 이러한 결과는 마이그레이션 프로세스가 해당 조인 조건이 변경된 MPR을 조인할 수 없기 때문에 발생합니다. 이 문제를 방지하려면 사용자 지정 특성을 모든 구성 개체 형식에 바인딩하고 해당 특성을 조인 조건으로 사용할 수 있습니다. 이렇게 하면 마이그레이션 프로세스에 영향을 주지 않으면서 표시 이름을 수정할 수 있습니다.

### <a name="avoid-changing-the-content-of-intermediate-files"></a>중간 파일의 내용 변경 안 함

하위 수준 개체의 파일 형식 및 API(애플리케이션 프로그래밍 인터페이스)는 공용이며 개발자가 조작을 지원하지만, 마이그레이션 동안 중간 형식의 콘텐츠는 변경하지 않는 것이 좋습니다. 그러나 전체 ImportObjects를 changes.xml에서 제거하거나 pilot.xml에 대해 찾기 및 바꾸기 작업을 수행하여 버전 번호 또는 프로덕션 DNS 정보에 대한 파일DNS럿 DNS(Domain Name System) 정보를 바꾸어야 할 수 있습니다.

### <a name="ensure-that-the-version-number-is-correct-in-pilotxml-when-migrating-across-versions"></a>버전 간에 마이그레이션을 수행할 때 pilot.xml에서 버전 번호가 올바른지 확인

버전 번호 간 마이그레이션은 권장되거나 지원되지 않지만 pilot.xml에서 파일럿 버전 번호를 프로덕션 버전 번호와 바꾸어 이러한 마이그레이션을 수행할 수 있습니다. 특히, WorkflowDefinition 및

ActivityInformationConfiguration 개체에서는 버전 번호가 프로덕션 환경의 워크플로 활동을 정확하게 참조해야 합니다. 버전 번호를 바꾸지 못하면 Compare-FIMConfig cmdlet을 사용해서 WorkflowDefinitions의 XOML(Extensible Object Markup Language) 특성 간 차이를 식별하고 파일럿의 버전 번호를 마이그레이션할 수 있습니다. 버전 번호가 올바르지 않으면 프로덕션 FIM 서비스가 워크플로 활동을 시작하지 못할 수 있습니다.

### <a name="avoid-cyclic-references"></a>순환 참조 방지

일반적으로 순환 참조는 MIM 구성에서 권장되지 않습니다. 하지만 집합 A가 집합 B를 참조하고, 집합 B도 집합 A를 참조할 때 순환이 발생합니다. 순환 참조 문제를 방지하려면 두 집합이 서로를 참조하지 않도록 집합 A 또는 집합 B의 정의를 변경해야 합니다. 그런 후 마이그레이션 프로세스를 다시 시작합니다. 순환 참조가 있으며 Compare-FIMConfig cmdlet을 실행할 때 오류가 발생하면 수동으로 순환을 끊어야 합니다. Compare-FIMConfig cmdlet은 우선 순위에 따라 변경 내용 목록을 출력하므로 구성 개체 참조 간에 순환이 없어야 합니다.

## <a name="security"></a>보안

### <a name="mim-ma-account"></a>MIM MA 계정

MIM MA 계정은 서비스 계정으로 간주되지 않으며 일반 사용자 계정이어야 합니다. FIM Synchronization Service 서비스 계정이 가장할 수 있으려면 계정이 로컬로 로그온할 수 있어야 합니다.

MIM MA 계정이 로컬로 로그온하도록 설정하려면

1.  [시작]을 클릭하고 [관리 도구]를 클릭한 후 [로컬 보안 정책]을 클릭합니다.

2.  [로컬 정책] 노드를 열고 [사용자 권한 할당]을 클릭합니다.

3.  [로컬 로그온 허용] 정책에서 FIM MA 계정이 명시적으로 지정되어 있는지 확인하고 그렇지 않으면 이미 액세스 권한이 부여된 그룹 중 하나에 추가합니다.

### <a name="fim-synchronization-service-and-fim-services-accounts"></a>FIM Synchronization Service 및 FIM 서비스 계정

MIM 서버 구성 요소를 실행하는 서버를 안전한 방식으로 구성하려면 서비스 계정이 제한되어야 합니다. 이전 절차에 따라 MIM MA 계정을 켜는 경우 FIM Synchronization Service 및 FIM 서비스 계정에 대해 다음과 같은 제한을 설정합니다.

-   일괄 작업으로 로그온 거부

-   로컬 로그온 거부

-   네트워크에서 이 컴퓨터 액세스 거부

서비스 계정은 로컬 관리자 그룹의 구성원이 아니어야 합니다.

FIM Synchronization Service 서비스 계정은 FIM Synchronization Service에 대한 액세스를 제어하는 데 사용되는 보안 그룹(FIMSyncAdmins와 같이 FIMSync로 시작하는 그룹)의 구성원이 아니어야 합니다.

> [!IMPORTANT]
>  두 서비스 계정에 동일한 계정을 사용하는 옵션을 선택하고 FIM 서비스 및 FIM Synchronization Service를 분리하는 경우 MMS Synchronization Service 서버에 대해 네트워크에서 이 컴퓨터로의 액세스 거부를 설정할 수 없습니다. 액세스가 거부되면 FIM 서비스가 FIM Synchronization Service에 연결하여 구성 및 관리 암호를 변경하지 못하게 됩니다.

### <a name="password-reset-deployed-to-kiosk-like-computers-should-set-local-security-to-clear-virtual-memory-pagefile"></a>키오스크 유사 컴퓨터에 배포된 암호 재설정으로 가상 메모리 페이지 파일을 지우도록 로컬 보안을 설정해야 함

키오스크로 사용하려는 워크스테이션에 FIM 암호 재설정을 배포하는 경우 시스템 종료: 가상 메모리 페이지 파일 지움 로컬 보안 정책 설정을 켜서 프로세스 메모리의 중요한 정보를 권한 없는 사용자가 사용할 수 없게 하는 것이 좋습니다.

### <a name="implementing-ssl-for-the-fim-portal"></a>FIM 포털에 대한 SSL 구현

FIM 포털 서버에서 SSL(Secure Sockets Layer)을 사용하여 클라이언트와 서버 간의 트래픽을 보호하는 것이 좋습니다.

SSL을 구현하려면

1.  MIM 포털 서버에서 IIS 관리자를 엽니다.

2.  로컬 컴퓨터 이름을 클릭합니다.

3.  [서버 인증서]를 클릭합니다.

4.  [인증서 요청 만들기]를 클릭합니다.

5.  [일반 이름] 텍스트 상자에 서버의 이름을 입력합니다.

6.  [다음]을 차례로 2번 클릭합니다.

7.  파일을 아무 위치에나 저장합니다. 이후 단계에서 이 위치에 액세스해야 합니다.

8.  을 찾아봅니다 https://servername/certsrv . servername을 인증서를 발급하는 서버의 이름으로 바꿉니다.

9.  [새 인증서 요청]을 클릭합니다.

10. [고급 요청 제출]을 클릭합니다.

11. base 64로 인코딩을 사용하여 [인증서 요청 제출]을 클릭합니다.

12. 이전 단계에서 저장한 파일의 내용을 붙여 넣습니다.

13. [인증서 템플릿]에서 [웹 서버]를 선택합니다.

14. [제출]을 클릭합니다.

15. 인증서를 바탕 화면에 저장합니다.

16. IIS 관리자에서 [인증 요청 완료]를 클릭합니다.

17. IIS 관리자가 방금 바탕 화면에 저장한 인증서를 가리키도록 합니다.

18. [이름]에 서버의 이름을 입력합니다.

19. [사이트]를 클릭하고 [SharePoint-80]을 선택합니다.

20. [바인딩]을 클릭하고 [추가]를 클릭합니다.

21. https를 선택합니다.

22. 인증서에 대해 서버와 이름이 같은 인증서(방금 가져온 인증서)를 선택합니다.

23. 확인을 클릭합니다.

24. HTTP 바인딩을 제거합니다.

25. [SSL 설정]을 클릭한 다음 [SSL 필요]를 선택합니다.

26. 설정을 저장합니다.

27. [시작]을 클릭하고 [관리 도구], [SharePoint 3.0 중앙 관리]를 차례로 클릭합니다.

28. [작업을] 클릭하고 [대체 액세스 매핑]을 클릭합니다.

29. 을 클릭합니다 http://servername 

30. 변화 http://servername 을 https://servername 으로 변경한 다음, [확인]을 클릭합니다.

31. [시작]을 클릭하고, [실행]을 클릭한 후 iisreset을 입력한 후 [확인]을 클릭합니다.

## <a name="performance"></a>성능

최적의 성능 구성을 위해 다음을 수행합니다.

-   이 문서의 SQL 설정 섹션에 설명된 대로 SQL 설정 모범 사례를 적용합니다.

-   MIM 포털 사이트에서 SharePoint 인덱싱을 끕니다. 자세한 내용은 이 문서의 SharePoint 인덱싱 사용 안 함 섹션을 참조하세요.

## <a name="feature-specific-best-practices"></a>기능별 모범 사례 


### <a name="request-management"></a>요청 관리

기본적으로 MIM 2016은 만료된 시스템 개체를 제거합니다. 그러면 완료된 요청은 30일 간격으로 연결된 승인, 승인 응답 및 워크플로 인스턴스에 포함됩니다. 조직에 더 긴 요청 기록이 필요한 경우 MIM에서 요청을 내보낸 후 보조 데이터베이스에 저장하여 30일 기간 이상 보존할 수 있습니다. 30일 요청 삭제 기간을 구성할 수 있지만 이 기간을 연장하면 시스템의 추가 개체 때문에 성능이 저하될 수 있습니다.

### <a name="management-policy-rules"></a>관리 정책 규칙

#### <a name="use-the-appropriate-mpr-type"></a>적절한 MPR 형식 사용

MIM은 2가지 MPR 형식인 요청 및 전환 설정을 제공합니다.

- RMPR(요청 MPR)

  - 리소스에 대한 CRUD(만들기, 읽기, 업데이트 또는 삭제) 작업에 대해 액세스 제어 정책(인증, 권한 부여 및 작업)을 정의하는 데 사용됩니다.
  - MIM의 대상 리소스에 대해 CRUD 작업이 실행될 때 적용됩니다.
  - 규칙에 정의된 일치 조건, 즉 규칙이 적용되는 CRUD 요청에 따라 범위가 지정됩니다.

- 집합 TMPR(전환 MPR)
  - 개체가 전환 집합으로 표시되는 현재 상태로 어떻게 전환되었는지에 관계없이 정책을 정의하는 데 사용됩니다. 자격 정책을 모델링하려면 TMPR을 사용합니다.
  - 리소스가 연결된 집합에 들어가거나 이러한 집합에서 나올 때 적용됩니다.
  - 집합의 구성원으로 범위가 지정됩니다.

>[참고] 자세한 내용은 [Designing Business Policy Rules](http://go.microsoft.com/fwlink/?LinkID=183691)(비즈니스 정책 규칙 디자인)을 참조하세요.

#### <a name="only-enable-mprs-as-necessary"></a>필요할 때만 MPR 사용

구성을 적용할 때 최소 권한 원칙을 사용합니다. MPR은 MIM 배포에 대한 액세스 정책을 제어합니다. 대부분의 사용자가 사용하는 기능만 사용하도록 설정합니다. 예를 들어 모든 사용자가 그룹 관리에 MIM을 사용하지는 않으므로 연결된 그룹 관리 MPR을 사용하지 않도록 설정하는 것이 좋습니다. 기본적으로 MIM에는 대부분의 비관리자 권한이 사용되지 않도록 설정되어 있습니다.

#### <a name="duplicate-built-in-mprs-instead-of-directly-modifying"></a>직접 수정 대신 기본 제공 MPR 복제
기본 제공 MPR을 수정해야 할 경우 필요한 구성으로 새 MPR을 만든 후 기본 제공 MPR을 해제해야 합니다. 이렇게 하면 업그레이드 프로세스를 통해 도입된 기본 제공 MPR에 대한 후속 변경 내용이 시스템 구성에 부정적인 영향을 주지 않습니다.

#### <a name="end-user-permissions-should-use-explicit-attribute-lists-scoped-to-users-business-needs"></a>최종 사용자 권한은 사용자 비즈니스 요구로 국한된 명시적 특성 목록을 사용해야 함
명시적 특성 목록을 사용하면 특성이 개체에 추가될 때 권한 없는 사용자에게 실수로 권한이 부여되는 경우를 방지할 수 있습니다. 관리자는 액세스 권한을 제거하는 대신, 새 특성에 대한 액세스 권한을 명시적으로 부여해야 합니다.

데이터에 대한 액세스는 사용자의 비즈니스 요구로 국한되어야 합니다. 예를 들어 그룹 구성원은 구성원으로 속해 있는 그룹의 필터 특성에 액세스할 수 없어야 합니다. 필터는 사용자에게 일반적으로 액세스 권한이 없는 조직 데이터를 실수로 노출할 수 있습니다.

#### <a name="mprs-should-reflect-effective-permissions-in-the-system"></a>MPR은 시스템의 유효 사용 권한을 반영해야 함
사용자가 절대 사용할 수 없는 특성에 대해 사용 권한을 부여하지 않도록 합니다. 예를 들어 objectType 등의 핵심 리소스 특성을 수정할 수 있는 권한을 부여하지 않아야 합니다. MPR 기능이 있더라도, 리소스가 생성된 후에 형식을 수정하려고 하면 시스템에서 거부됩니다.

#### <a name="read-permissions-should-be-separate-from-modify-and-create-permissions-when-using-explicit-attributes-in-mprs"></a>MPR에서 명시적 특성을 사용하는 경우 수정 및 만들기 권한에서 읽기 권한을 분리해야 합니다.

MPR에서 특성을 명시적으로 나열할 때 만들기 및 수정에 필요한 특성은 읽기에 사용할 수 있는 특성과 다른 것이 일반적입니다. 예를 들어 읽기는 Creator 또는 objectId 등과 같은 시스템 특성에 대해 부여할 수 있지만 만들기 또는 수정은 시스템 특성에 대해 지정할 수 없습니다.

#### <a name="create-permissions-should-be-separate-from-modify-permissions-when-using-explicit-attributes-in-rules"></a>규칙에서 명시적 특성을 사용하는 경우 수정 권한에서 만들기 권한을 분리해야 합니다.

만들기 작업에서는 작업의 일부로 objectType을 선택해야 합니다. 이것은 만들기 작업을 수행한 후 수정할 수 없는 핵심 시스템 특성입니다.

#### <a name="use-one-request-mpr-for-all-attributes-with-the-same-access-requirements"></a>동일한 액세스 요구 사항을 갖는 모든 특성에 대해 하나의 요청 MPR 사용

변경되지 않을 동일한 액세스 요구 사항을 갖는 특성의 경우 효율성을 위해 이러한 특성을 단일 요청 MPR에 결합할 수 있습니다.

#### <a name="avoid-giving-unrestricted-access-even-to-selected-principal-groups"></a>선택한 사용자 그룹이더라도 무제한 액세스 권한을 부여하지 않음

MIM에서 사용 권한은 긍정 어설션으로 정의됩니다. MIM은 거부 권한을 지원하지 않으므로 리소스에 대해 무제한 액세스 권한을 부여하면 사용 권한에 예외 항목을 지정해야 하므로 작업이 복잡해집니다. 가장 좋은 방법은 필요한 사용 권한만 부여하는 것입니다.

#### <a name="use-tmprs-to-define-custom-entitlements"></a>TMPR을 사용하여 사용자 지정 자격 정의

RMPR 대신 집합 TMPR(전환 MPR)을 사용하여 사용자 지정 자격을 정의합니다. TMPR은 전환 집합 또는 역할의 멤버 자격 및 함께 제공되는 워크플로 활동에 따라 자격을 할당하거나 제거하기 위한 상태 기반 모델을 제공합니다. TMPR은 리소스 전환 시작 및 리소스 전환 종료로 이루어진 쌍으로 정의되어야 합니다. 또한 각 전환 MPR은 프로비전 및 프로비전 해제 활동을 위한 별도 워크플로를 포함해야 합니다.

> [!NOTE]
> 프로비전 해제 워크플로는 [정책 업데이트 시 실행] 특성이 true로 설정되도록 합니다.

#### <a name="enable-the-set-transition-in-mpr-last"></a>[Set Transition In MPR last]\(마지막에 집합 전환 시작 MPR)을 사용하도록 설정

TMPR 쌍을 만들 때 [Set Transition In MPR last]\(마지막에 집합 전환 시작 MPR)을 켭니다. 이 명령을 사용하면 In MPR이 켜져 있지만 Out MPR이 켜지기 전에 집합에 리소스를 추가하거나 제거하는 경우 해당 자격에 리소스가 남아 있지 않게 됩니다.

#### <a name="workflows-in-tmpr-should-check-target-resource-state-first"></a>TMPR의 워크플로는 먼저 대상 리소스 상태를 확인해야 함

프로비전 워크플로는 먼저 대상 리소스가 자격에 맞게 프로비전되었는지를 확인해야 합니다. 제대로 프로비전되어 있으면 아무 작업도 수행하지 않습니다.

프로비전 해제 워크플로는 먼저 대상 리소스가 프로비전되었는지를 확인해야 합니다. 제대로 프로비전되어 있으면 대상 리소스의 프로비전을 해제합니다. 그렇지 않으면 아무 작업도 수행하지 않습니다.

#### <a name="select-run-on-policy-update-for-tmprs"></a>TMPR에 대해 정책 업데이트 시 실행 선택

이렇게 하면 정책 업데이트가 구현되고, TMPR과 연결된 작업 워크플로에 [정책 업데이트 시 실행] 플래그를 사용할 때 올바른 프로비전 동작이 적용됩니다. 이를 통해 정책 정의가 변경되면 전환 집합의 새 구성원에 작업 워크플로가 적용됩니다.

#### <a name="avoid-associating-the-same-entitlement-with-two-different-transition-sets"></a>2개의 다른 전환 집합에 동일한 자격 연결 안 함

2개의 다른 전환 집합에 동일한 자격을 연결하면 리소스가 한 집합에서 다른 집합으로 이동될 때 불필요하게 자격이 해지되었다가 다시 부여될 수 있습니다. 모범 사례로, 연결된 자격이 필요한 모든 리소스를 하나의 집합에 포함할 수 있습니다. 이렇게 하면 전환 집합 및 워크플로에 부여하는 자격 간에 일대일 관계가 설정됩니다.

#### <a name="use-an-appropriate-sequence-of-operations-when-removing-entitlements-in-the-system"></a>시스템에서 자격을 제거할 때 작업의 적절한 순서 사용

시스템에서 자격을 제거할 때 수행되는 단계의 순서는 2가지 다른 작업 결과를 가져올 수 있습니다. 어떤 순서를 사용해야 원하는 결과를 얻을 수 있는지 이해해야 합니다.

시스템에서 자격을 제거하려면(현재 자격을 보유하는 모든 구성원에서 자격 박탈):

1.  T-In MPR을 사용하지 않도록 설정합니다. 이렇게 하면 새 권한 부여가 방지됩니다.

2.  T-Set 필터를 삭제하거나 집합이 빈 상태가 되도록 변경합니다. 이렇게 하면 모든 기존 구성원의 전환이 종료되고, 자격과 연결된 구성된 프로비전 해제 워크플로를 비롯한 전환 종료 정책이 적용됩니다.

3.  T-Out MPR을 사용하지 않도록 설정합니다.

자격은 제거하지만 현재 구성원은 그대로 두려면(예: MIM을 사용한 자격 관리 중지)

1.  T-In MPR을 사용하지 않도록 설정합니다. 이렇게 하면 새 권한 부여가 방지됩니다.

2.  T-Out MPR을 사용하지 않도록 설정합니다.

3.  T-Set 필터를 삭제하거나 집합이 빈 상태가 되도록 변경합니다. 집합이 더 이상 TMPR에 연결되지 않으므로 프로비전 해제 워크플로가 적용됩니다.

### <a name="sets"></a>설정

집합에 대해 모범 사례를 적용할 때 최적화가 관리 효율성 및 향후 관리 편의성에 미치는 영향을 고려해야 합니다. 이러한 권장 사항을 적용하기 전에 성능과 관리 효율성 간의 적절한 균형 수준을 알아내기 위해 예상되는 프로덕션 규모에서 적절한 테스트를 수행해야 합니다.

>[!NOTE]
> 다음에 나오는 모든 지침은 동적 집합 및 동적 그룹에 적용됩니다.


#### <a name="minimize-the-use-of-dynamic-nesting"></a>동적 중첩 사용 최소화

이것은 다른 집합의 ComputedMember 특성을 참조하는 집합의 필터를 나타냅니다. 집합을 중첩하는 일반적인 이유는 많은 집합 간에 멤버 자격 조건이 중복되는 것을 피하기 위해서입니다. 이 방법을 사용하면 집합의 관리 효율을 높일 수 있지만 성능이 저하될 수 있습니다. 집합 자체를 중첩하는 대신, 중첩된 집합의 멤버 자격 조건을 복제하여 성능을 최적화할 수 있습니다.

기능적 요구를 충족하기 위해 집합 중첩을 피하기 어려운 경우도 있을 수 있습니다. 다음은 집합을 중첩해야 하는 기본적인 상황입니다. 예를 들어 Full-Time Employee 소유자가 없는 모든 그룹의 집합을 정의하려면 다음과 같이 집합의 중첩을 사용해야 합니다. `/Group[not(Owner = /Set[ObjectID = ‘X’]/ComputedMember]` 여기서 ‘X’는 All Full Time Employees 집합의 ObjectID입니다.

#### <a name="minimize-the-use-of-negative-conditions"></a>부정 조건의 사용 최소화

부정 조건은 `!=`, `not()`, `\<` , `\<=` 등의 연산자 또는 함수를 사용하는 멤버 자격 조건입니다. 성능 최적화를 위해 가능한 경우 부정 조건 하나보다는 여러 긍정 조건을 사용해서 원하는 조건을 나타냅니다.

#### <a name="minimize-the-use-of-membership-conditions-based-on-multivalued-reference-attributes"></a>다중값 참조 특성에 따라 멤버 자격 조건의 사용 최소화

이러한 많은 수의 집합은 멤버 자격 조건에 사용되는 특성에 대한 작업의 성능에 영향을 줄 수 있으므로 다중값 참조 특성에 따른 조건의 사용을 최소화해야 합니다.

### <a name="password-reset"></a>암호 다시 설정

#### <a name="kiosk-like-computers-that-are-used-for-password-reset-should-set-local-security-to-clear-the-virtual-memory-pagefile"></a>암호 재설정에 사용되는 키오스크 유사 컴퓨터는 가상 메모리 페이지 파일을 지우기 위해 로컬 보안을 설정해야 함

키오스크로 사용하려는 워크스테이션에 MIM 암호 재설정을 배포하는 경우 시스템 종료: 가상 메모리 페이지 파일 지움 로컬 보안 정책 설정을 켜서 프로세스 메모리의 중요한 정보를 권한 없는 사용자가 사용할 수 없게 하는 것이 좋습니다.

#### <a name="users-should-always-register-for-a-password-reset-on-a-computer-that-they-are-logged-on-to"></a>사용자가 로그온된 컴퓨터에서 항상 암호 재설정을 등록해야 함

사용자가 웹 포털을 통해 암호 재설정에 등록하려고 하면 MIM은 웹 사이트에 로그온한 사용자가 누군지 관계없이, 로그온한 사용자 대신 등록을 시작합니다. 사용자는 로그온된 컴퓨터에서 항상 암호 재설정을 등록해야 합니다.

#### <a name="do-not-set-the-avoidpdconwan-registry-key-to-true"></a>AvoidPdcOnWan 레지스트리 키를 true로 설정하지 말 것

MIM 2016 암호 재설정을 사용할 경우에는 AvoidPdcOnWan 레지스트리 키를 true로 설정하지 않도록 합니다.

이 레지스트리 키를 true로 설정하면 사용자가 암호 게이트를 통과하고, PDC(주 도메인 컨트롤러)에서 암호를 재설정하고, 로그온을 시도할 가능성이 높아집니다. 이 레지스트리 키로 인해 로컬 도메인 컨트롤러는 PDC와의 보조 유효성 검사를 수행하지 않으므로 로그온 요청을 거부합니다. 사용자가 충분한 횟수만큼 거부되면 도메인에서 잠겨지고 지원 센터에 문의해야 합니다.

#### <a name="do-not-turn-on-logging-of-clear-text-passwords"></a>일반 텍스트 암호의 로깅을 켜지 말 것

WCF(Windows Communication Foundation)에서 진단 서비스 수준 추적을 켤 경우

일반 텍스트 암호를 로깅할 수 있음 이 옵션은 기본적으로 켜져 있지 않으며 프로덕션 환경에서는 켜지 않는 것이 좋습니다. 이러한 암호는 사용자가 암호 재설정에 등록할 경우 암호화된 SOAP(Simple Object Access Protocol) 메시지에서 단순 개체 요소로 표시될 수 있습니다. 자세한 내용은 [메시지 로깅 구성](http://go.microsoft.com/fwlink/?LinkID=168572)을 참조하세요.

#### <a name="do-not-map-an-authorization-workflow-to-the-password-reset-process"></a>권한 부여 워크플로를 암호 재설정 프로세스에 매핑하지 않음

권한 부여 워크플로 암호 재설정 작업에 연결하지 않아야 합니다. 암호 재설정에는 동기 응답이 필요하며 승인 활동 등의 활동을 포함하는 권한 부여 워크플로는 비동기적입니다.

#### <a name="do-not-map-multiple-action-activities-to-password-reset"></a>여러 작업 활동을 암호 재설정에 매핑하지 않음

둘 이상의 작업 활동을 포함하는 워크플로를 암호 재설정 작업에 연결하지 않도록 합니다. 또 다른 AD DS 암호 재설정 활동을 암호 재설정 MPR에 추가하는 경우를 예로 들 수 있습니다. 이 시나리오는 지원되지 않습니다.

#### <a name="require-reregistration-when-adding-removing-or-changing-the-order-of-activities-in-an-existing-workflow"></a>기존 워크플로에서 활동을 추가 또는 제거하거나 활동 순서를 변경할 때 등록 필요

기존 워크플로에서 인증 활동을 추가, 제거하거나 인증 활동 순서를 변경할 때는 항상 재등록을 요구하는 옵션을 선택합니다. 워크플로에서 활동이 추가되거나 제거된 후, 사용자가 아직 재등록하기 전에 암호 재설정을 위해 인증을 시도하면 원치 않는 결과가 발생할 수 있습니다.

### <a name="portal-configuration-and-resource-control-display-configuration"></a>포털 구성 및 리소스 컨트롤 표시 구성

#### <a name="consider-adding-a-privacy-disclaimer-to-the-user-profile-page"></a>사용자 프로필 페이지에 개인 정보 취급 방침 고지 사항 추가 고려

MIM에서 기본적으로 일부 사용자 프로필 정보가 다른 사용자에게 표시될 수 있습니다. 사용자의 편의를 위해, 관리자는 사용자 프로필 페이지에 회사 정책에 부합되는 사용자 지정 텍스트를 추가하는 것을 고려해야 합니다. MIM 포털 페이지에 사용자 지정 텍스트를 추가하는 방법에 대한 자세한 내용은 [Introduction to Configuring and Customizing the FIM Portal](http://go.microsoft.com/fwlink/?LinkID=165848)(FIM 포털 구성 및 사용자 지정 소개)을 참조하세요.

### <a name="schema"></a>스키마

#### <a name="do-not-delete-person-or-group-resource-types"></a>개인 또는 그룹 리소스 유형은 삭제 안 함

개인 및 그룹 리소스 유형은 핵심 리소스 유형으로 표시되지 않지만 리소스 자체 및 할당된 특성을 삭제하면 안 됩니다. MIM 포털의 UI(사용자 인터페이스)를 사용하려면 개인 및 그룹 리소스 유형과 해당 특성이 있어야 합니다.

#### <a name="do-not-modify-the-core-attributes"></a>핵심 특성 수정 안 함

모든 리소스 유형에 할당된 13가지 핵심 특성이 있습니다. 어떤 경우에도 리소스 유형에 대한 관계를 수정하지 않아야 합니다. 13개의 핵심 특성은 다음과 같습니다.

-   CreatedTime

-   Creator

-   DeletedTime

-   설명

-   DetectedRulesList • DisplayName

-   ExpectedRulesList

-   ExpirationTime

-   로캘

-   MVObjectID

-   ObjectID

-   ObjectType

-   ResourceTime

감사에 대한 종속성 요구가 있는 스키마 리소스 삭제 안 함

이러한 리소스에 대한 감사 요구가 있을 때는 스키마 리소스를 삭제하면 안 됩니다.

#### <a name="making-regular-expressions-case-insensitive"></a>정규식이 대/소문자를 구분하지 않도록 지정

MIM에서 일부 정규식을 대/소문자를 구분하지 않게 설정하면 도움이 될 수 있습니다. ?!:을 사용하여 그룹 내에서 대/소문자를 무시할 수 있습니다. 예를 들어 Employee 유형에 대해 다음을 사용합니다.

`\^(?!:contractor\|full time employee)%.`

#### <a name="calculation-of-the-member-attribute"></a>멤버 특성 계산

동기화 엔진에 노출된 Member 특성은 실제로 ComputedMembers에 매핑됩니다. 조건 기준 멤버와 수동으로 선택한 멤버가 조합된 것입니다. 세 가지 특성(Filter, ExplicitMembers 및 ComputedMembers)을 모두 추가하더라도, 그룹 및 집합 이외의 리소스 유형에서는 멤버 특성의 동적 계산이 발생하지 않습니다.

#### <a name="leading-and-trailing-spaces-in-strings-are-ignored"></a>문자열의 선행 및 후행 공백이 무시됨

MIM에서 선행 및 후행 공백이 있는 문자열을 입력할 수 있지만 MIM 시스템은 이러한 공백을 무시합니다. 선행 및 후행 공백이 있는 문자열을 제출하면 동기화 엔진 및 웹 서비스가 이러한 공백을 무시합니다.

#### <a name="empty-strings-do-not-equal-null"></a>빈 문자열이 null이 아님

이번 MIM 릴리스에서는 빈 문자열이 null이 아닙니다. 빈 문자열 입력이 유효한 값으로 간주됩니다. 없음이 null로 간주됩니다.

### <a name="workflow-and-request-processing"></a>워크플로 및 요청 처리

#### <a name="do-not-delete-default-workflows-that-are-shipped-with-mim-2016"></a>MIM 2016에 포함된 기본 워크플로는 삭제하지 말 것

다음과 같은 워크플로가 MIM에 포함되어 있으며 삭제하지 않아야 합니다.

-   만료 워크플로

-   관리자를 위한 필터 유효성 검사 워크플로

-   비관리자를 위한 필터 유효성 검사 워크플로

-   그룹 만료 알림 워크플로

-   그룹 유효성 검사 워크플로

-   소유자 승인 워크플로

-   암호 재설정 작업 워크플로

-   암호 재설정 인증 워크플로

-   소유자 권한 부여를 사용한 요청자 유효성 검사

-   소유자 권한 부여를 사용하지 않는 요청자 유효성 검사

-   등록에 필요한 시스템 워크플로

#### <a name="do-not-run-two-or-more-approvalactivities-in-parallel"></a>두 개 이상의 ApprovalActivity를 동시에 실행하지 말 것

두 개 이상의 ApprovalActivity를 동시에 실행하지 않아야 합니다. 이렇게 하면 요청의 권한 부여 단계가 중단될 수 있습니다. 여러 승인의 경우, 더 큰 승인자 목록을 승인에 포함하거나 두 작업을 연속해서 배치합니다.

#### <a name="authorization-activities-should-not-modify-mim-resources-data"></a>권한 부여 작업이 MIM 리소스 데이터를 수정하지 않아야 함

함수 평가기 작업과 같이 권한 부여 워크플로의 워크플로에 속하는 MIM 리소스를 수정하는 작업은 사용하지 마세요. 처리의 권한 부여 시점에서 요청이 커밋되지 않았으므로 요청이 거부될 수 있더라도 ID 정보에 대한 수정 사항이 적용될 수 있습니다.

### <a name="understanding-fim-service-partitions"></a>FIM 서비스 파티션의 이해

MIM의 목적은 구성한 비즈니스 정책에 따라 FIM Synchronization Service 및 셀프 서비스 구성 요소와 같은 다양한 MIM 클라이언트가 시작할 수 있는 요청을 처리하는 것입니다. 기본적으로 각 FIM 서비스 인스턴스는 하나 이상의 FIM 서비스 인스턴스로 구성되는 논리적 그룹(FIM 서비스 파티션이라고도 함)에 속합니다. 모든 요청을 처리하기 위해 FIM 서비스 인스턴스를 하나만 배포하는 경우 처리 대기 시간이 발생할 수 있습니다. 일부 작업은 셀프 서비스 작업에 적절한 기본 시간 제한 값을 초과할 수도 있습니다. FIM 서비스 파티션은 이 문제를 해결하는 데 도움이 될 수 있습니다.

자세한 내용은 [Understanding FIM Service Partitions](https://social.technet.microsoft.com/wiki/contents/articles/2363.understanding-fim-service-partitions.aspx)(FIM Service 파티션 이해)를 참조하세요.

## <a name="next-steps"></a>다음 단계
- [FIM Backup and Restore Guide](http://go.microsoft.com/fwlink/?LinkID=165864)(FIM 백업 및 복원 가이드)
- [How do I Synchronize Users from Active Directory to FIM](http://go.microsoft.com/fwlink/?LinkID=188277)(Active Directory에서 FIM으로 사용자를 동기화하는 방법) 
- [Recovery Model Overview](http://go.microsoft.com/fwlink/?LinkID=185370)(복구 모델 개요)
