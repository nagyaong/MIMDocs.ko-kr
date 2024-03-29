---
title: PAM 재해 복구 | Microsoft Docs
description: 고가용성 및 재해 복구를 위해 Privileged Access Management를 구성하는 방법을 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 03e521cd-cbf0-49f8-9797-dbc284c63018
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 0d0d55d4007ab88df4c2f3b5a30ca0fdedea9fe2
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379976"
---
# <a name="high-availability-and-disaster-recovery-considerations-for-the-bastion-environment"></a>배스천 환경에 대한 고가용성 및 재해 복구 고려 사항

이 문서에서는 PAM(Privileged Access Management)을 위해 AD DS(Active Directory 도메인 서비스) 및 MIM(Microsoft Identity Manager) 2016을 배포하는 경우 고가용성 및 재해 복구에 대한 고려 사항을 설명합니다.

기업에서는 Windows Server, SQL Server 및 Active Directory의 워크로드에 대한 고가용성 및 재해 복구에 집중합니다. 하지만 Privileged Access Management를 위한 배스천 환경의 신뢰할 수 있는 가용성 또한 중요합니다. 배스천 환경은 사용자가 관리 역할을 수행하기 위해 해당 구성 요소와 상호 작용하므로 조직의 IT 인프라에 중요한 부분입니다. 백서 [Microsoft 고가용성 개요](http://download.microsoft.com/download/3/B/5/3B51A025-7522-4686-AA16-8AE2E536034D/Microsoft%20High%20Availability%20Strategy%20White%20Paper.doc)를 다운로드하면 일반적인 고가용성에 대한 자세한 내용을 확인할 수 있습니다.

## <a name="high-availability-and-disaster-recovery-scenarios"></a>고가용성 및 재해 복구 시나리오

고가용성 및 재해 복구를 계획할 때 다음 사항을 고려해야 합니다.

- 중단으로 영향을 받을 수 있는 기능
- 업무 핵심인 기능 및/또는 IT 운영에 중요한 기능
- 이러한 시스템이 중단될 수 있는 위험 요인

이러한 고려 사항의 범위는 배포 및 운영의 총 비용에 영향을 주기 때문에 조직은 특정 기능에 다른 기능보다 높은 우선 순위를 지정하고 낮은 우선 순위 기능을 임시 중단하는 위험을 감수하기도 합니다. 다음 표에서는 조직에서 사용할 수 있는 잠재적 우선 순위 지정에 대해 설명합니다.

| **배스천 포리스트 기능** | **복구 중 상대적 우선 순위** | **기능을 사용할 수 없는 경우 완화 방법** |
| --------------------------- | --------------------- | -------------- |
| 트러스트 설정         | 낮음 | 배스천 환경이 복원될 때까지 대기 |
| 사용자 및 그룹 완화   | 낮음 | 배스천 환경이 복원될 때까지 대기 |
| MIM 관리          | 낮음 | 배스천 환경이 복원될 때까지 대기 |
| 권한 있는 역할 활성화  | 중형 | 수동으로 관리 그룹에 사용자를 추가하기 위한 전용 스마트 카드 지원 계정 |
| 리소스 관리         | 높은 | 수동으로 관리 그룹에 사용자를 추가하기 위한 전용 스마트 카드 지원 계정 |
| 기존 포리스트의 사용자 및 그룹 모니터링 | 낮음 | 배스천 환경이 복원될 때까지 대기 |

이제 이러한 각 배스천 포리스트 기능에 대해 차례대로 살펴보겠습니다.

### <a name="trust-establishment"></a>트러스트 설정

기존 포리스트의 도메인과 배스천 환경의 포리스트 간에 포리스트 트러스트가 필요합니다. 그래야 배스천 환경에 인증하는 사용자가 기존 포리스트의 리소스를 관리할 수 있습니다. 예를 들어 이전 버전의 Windows Server의 기존 도메인에서 사용자의 마이그레이션을 허용하려면 추가 구성이 필요할 수 있습니다.

트러스트 설정을 위해 기존 포리스트 도메인 컨트롤러와 배스천 환경의 MIM 및 AD 구성 요소가 온라인 상태여야 합니다.  트러스트 설정 중에 이들 중 하나라도 중단되는 경우 중단을 해결하면 관리자가 다시 시도할 수 있습니다.  기존 포리스트 도메인 컨트롤러 또는 배스천 환경이 다음 중단을 복구한 경우, MIM은 트러스트가 계속 설정되어 있는지 확인하는 데 사용할 수 있는 PowerShell cmdlet `Test-PAMTrust` 및 `Test-PAMDomainConfiguration`을 포함합니다.

### <a name="user-and-group-migration"></a>사용자 및 그룹 마이그레이션

트러스트가 설정되면 해당 그룹의 구성원 및 승인자에 대한 사용자 계정뿐만 아니라 배스천 환경에서 섀도 그룹을 만들 수 있습니다. 그러면 해당 사용자가 권한 있는 역할을 활성화하고 효과적인 그룹 구성원 자격을 다시 얻을 수 있습니다.

사용자 및 그룹 마이그레이션을 위해 기존 포리스트 도메인 컨트롤러와 배스천 환경의 MIM 및 AD 구성 요소가 온라인 상태여야 합니다.   기존 포리스트 도메인 컨트롤러에 연결할 수 없는 경우 배스천 환경에 또 다른 사용자 및 그룹을 추가할 수 없지만 기존 사용자 및 그룹은 영향을 받지 않습니다. 마이그레이션하는 동안 구성 요소 중에서 중단이 발생하는 경우 중단이 해결되면 관리자가 다시 시도할 수 있습니다.

### <a name="mim-administration"></a>MIM 관리

사용자 및 그룹을 마이그레이션하면 관리자가 MIM에서 활성화할 후보인 사용자를 역할에 연결하는 역할 할당을 추가로 구성할 수 있습니다.  승인 및 Azure MFA에 대한 MIM 정책을 구성할 수도 있습니다.  

MIM 관리를 위해 배스천 환경의 MIM 및 AD 구성 요소가 온라인 상태여야 합니다.

### <a name="privileged-role-activation"></a>권한 있는 역할 활성화

사용자가 권한 있는 역할을 활성화하려는 경우 배스천 환경 도메인에 인증하고 MIM에 요청을 제출해야 합니다.  MIM에는 PowerShell 및 웹 페이지의 사용자 인터페이스뿐만 아니라 SOAP 및 REST API가 포함되어 있습니다.

권한 있는 역할 활성화를 위해 배스천 환경의 MIM 및 AD 구성 요소가 온라인 상태여야 합니다.  또한 MIM이 선택한 역할의 [활성화를 위해 Azure MFA](use-azure-mfa-for-activation.md)를 사용하도록 구성된 경우 Azure MFA에 연결하기 위해 인터넷 액세스가 필요합니다.

### <a name="resource-management"></a>리소스 관리

사용자가 역할에 대해 성공적으로 활성화되면 도메인 컨트롤러가 기존 도메인의 도메인 컨트롤러에서 사용할 수 있는 Kerberos 티켓을 생성할 수 있고 사용자의 새 임시 그룹 구성원 자격을 인식하게 됩니다.

리소스 관리를 위해 리소스 도메인의 도메인 컨트롤러와 배스천 환경의 도메인 컨트롤러가 온라인 상태여야 합니다.  사용자가 활성화되면 해당 Kerberos 티켓을 발급하는 데 배스천 환경에서 MIM 또는 SQL이 온라인 상태가 아니어도 됩니다.  (배스천 환경에 대한 기능 수준으로 Windows Server 2012 R2를 사용하면 임시 그룹 구성원 자격을 종료하는 데 MIM이 온라인 상태여야 합니다.)

### <a name="monitoring-of-users-and-groups-in-the-existing-forest"></a>기존 포리스트의 사용자 및 그룹 모니터링

MIM에는 기존 도메인의 사용자 및 그룹을 정기적으로 확인하고 MIM 데이터베이스와 AD를 적절하게 업데이트하는 PAM 모니터링 서비스도 포함되어 있습니다.  이 서비스는 역할 활성화를 위해 또는 리소스를 관리하는 동안 온라인 상태가 아니어도 됩니다.

모니터링을 위해 기존 포리스트 도메인 컨트롤러와 배스천 환경의 MIM 및 AD 구성 요소가 온라인 상태여야 합니다.  

## <a name="deployment-options"></a>배포 옵션

[환경 개요](environment-overview.md)는 고가용성을 위해 디자인되지 않은 기술 학습에 적합한 기본 토폴로지를 보여 줍니다. 이 섹션에서는 단일 사이트뿐만 아니라 여러 개의 기존 사이트가 있는 조직에서 고가용성을 제공하도록 토폴로지를 확장하는 방법을 설명합니다.

### <a name="networking"></a>네트워킹

배스천 환경에서 컴퓨터 간의 네트워크 트래픽은 기존 네트워크(예: 서로 다른 실제 또는 가상 네트워크 사용)로부터 격리되어야 합니다.  배스천 환경에 대한 위험에 따라 컴퓨터 간에 독립적인 물리적 상호 연결이 필요할 수도 있습니다.  특정 장애 조치(failover) 클러스터 기술에는 네트워크 인터페이스에 대한 추가 요구 사항이 있습니다.

배스천 환경에서 Active Directory 도메인 서비스 및 MIM 서비스를 호스트하는 컴퓨터는 다음의 경우 기존 포리스트에 있는 리소스에 대해 양방향 연결이 필요합니다.

- 사용자가 PRIV 포리스트의 도메인 컨트롤러에 의해 인증되는 경우
- 사용자가 활성화를 요청하는 경우
- 사용자가 기존 포리스트의 리소스로 Kerberos 티켓을 사용할 수 있는 경우
- MIM이 기존 포리스트 도메인을 모니터링하는 경우
- MIM이 기존 포리스트에 있는 메일 서버를 통해 메일을 보내는 경우

### <a name="minimal-high-availability-topologies"></a>최소한의 고가용성 토폴로지

조직은 해당 배스천 환경에서 어떤 기능에 고가용성이 필요한지 선택할 수 있습니다. 다음 제약 조건이 적용됩니다.

- 배스천 환경에서 제공하는 임의 기능에 대해 고가용성을 사용하려면 두 개 이상의 도메인 컨트롤러가 필요합니다.  
- 활성화 요청에 대한 고가용성에는 MIM 서비스를 호스트하는 두 대 이상의 컴퓨터가 필요하며 SQL Server에 대한 고가용성도 필요합니다.
- 장애 조치(failover) 클러스터에서 SQL Server 고가용성을 사용하려면 SQL Server를 제공하는 두 개 이상의 서버가 필요한데 도메인 컨트롤러와 같을 수 없습니다.
- 각 서버의 공격 노출 영역을 최소화하기 위해 MIM 서비스를 도메인 컨트롤러에 설치하지 않아야 합니다.

배스천 환경에서 모든 기능에 대한 가장 작은 고가용성 토폴로지는 4개 이상의 서버 및 공유 스토리지로 구성됩니다. 두 서버는 Active Directory 도메인 서비스를 제공하는 도메인 컨트롤러로 구성해야 합니다. 다른 두 서버는 SQL Server를 제공하는 장애 조치(failover) 클러스터로 구성될 수 있으며 MIM 서비스를 제공합니다.

또한 배스천 환경의 일반 배포에는 이러한 서버 관리에 대해 권한 있는 관리 워크스테이션뿐만 아니라 모니터링 구성 요소도 포함됩니다.

다음 다이어그램에서는 하나의 가능한 아키텍처를 보여 줍니다.

![배스천 토폴로지 - 다이어그램](media/bastion1.png)

아래 설명된 대로 부하 조건에서 또는 지리적 중복성을 위해 더 높은 성능을 제공하려면 이러한 각 기능에 대해 추가 서버를 구성할 수 있습니다.

### <a name="deployments-supporting-multiple-sites"></a>여러 사이트를 지원하는 배포

여러 사이트에 걸쳐 배포되는 리소스에 대한 올바른 배포 토폴로지 선택은 다음 세 가지 요인에 따라 달라집니다.

- 고가용성 및 재해 복구 목표 및 위험 사항
- 배스천 환경을 호스트하는 하드웨어 용량
- 각 사이트에 대한 관리 작업 모델

특정 사이트에서 배스천 환경을 호스트하는 것이 가장 간단한 방법 중 하나입니다.  정상 조건에서는 사용자가 해당 사이트의 배스천 환경에서 MIM 배포에 연결하고 활성화를 요청하며 이 활성화는 각 사이트의 리소스 전체에 영향을 줍니다.  네트워크 연결이 끊어지거나 배스천 환경을 호스트하는 사이트를 사용할 수 없는 경우 네트워크에 다시 연결될 때까지 임시 관리를 수행하기 위해 다른 사이트에서 오프라인 자격 증명에 액세스할 수 있습니다.  이 방법은 지사와 같은 특정 사이트의 로컬 관리 상황에 적합할 수 있지만 드물게 발생하며 조직 네트워크의 나머지 부분에 해당 사이트를 다시 연결하는 것으로 제한됩니다.

![다중 사이트에 대한 단일 배스천 토폴로지 - 다이어그램](media/bastion2.png)

사이트의 고가용성 및 재해 복구를 위해 각 사이트에서 배스천 환경의 구성 요소를 배포하여 일반 PRIV 디렉터리 및 일반 SQL 데이터베이스를 공유하는 것도 가능합니다.  이 토폴로지에서는 네트워크 연결이 끊어진 경우 각 사이트에 있는 사용자는 독립적으로 계속 작동합니다.

![다중 사이트에 대한 다중 배스천 토폴로지 - 다이어그램](media/bastion3.png)

이 배포 방법에서 한 가지 제약은 SQL Server에 두 사이트에 걸쳐 있는 클러스터가 필요해서 배포가 복잡할 수 있다는 점입니다. 이런 상황에서는 대신 배스천 환경의 Active Directory(PRIV 포리스트) 복제만 고려합니다.  사이트 간 네트워크 중단이 발생하는 경우 권한 있는 역할을 이전에 이미 활성화한 사이트 B의 사용자는 사이트 B의 리소스를 관리하기 위한 작업을 계속 수행할 수 있습니다.

![다중 사이트에 대한 복제된 배스천 토폴로지 - 다이어그램](media/bastion4.png)

각 사이트가 별도 관리 경계를 나타내는 경우 여러 독립적인 배스천 환경을 배포하는 것도 가능합니다.  각 배스천 환경에 같은 소프트웨어가 있을 수 있지만, 각 도메인 이름은 서로 다르며 각 배스천 환경의 디렉터리와 데이터베이스 간의 공통점은 없습니다. 특정 사이트의 리소스를 관리하려는 사용자는 해당 사이트의 배스천 환경에서 사용자 계정을 활성화합니다.

![다중 사이트에 대한 독립적인 배스천 토폴로지 - 다이어그램](media/bastion5.png)

마지막으로, 여러 배스천 환경을 특정 도메인의 리소스를 관리하도록 독립적으로 구성할 수 있으므로 보다 복잡한 배포가 가능합니다.

![다중 사이트에 대한 복잡한 배스천 토폴로지 - 다이어그램](media/bastion6.png)

### <a name="hosted-bastion-environment"></a>호스트된 배스천 환경

일부 조직에서는 해당 기존 사이트와는 별도로 배스천 환경을 설정하는 것도 고려합니다. 배스천 환경 소프트웨어는 조직의 네트워크 내 또는 외부 호스팅 공급자의 가상화 플랫폼에서 호스트될 수 있습니다.  이 방법을 평가할 때는 다음에 유의하세요.

- 기존 도메인에서 발생하는 공격을 방지하기 위해 배스천 환경의 관리는 기존 도메인의 관리자 계정에서 격리되어야 합니다.
- 배스천 환경에는 기존 도메인의 도메인 컨트롤러에 대한 TCP/IP 연결이 필요합니다.  포트 목록은 [도메인 및 트러스트를 위한 방화벽을 구성하는 방법](https://support.microsoft.com/kb/179442)에서 찾을 수 있습니다.
- Active Directory 도메인 서비스의 가상화된 배포에는 [가상화된 도메인 컨트롤러 배포 및 구성](https://technet.microsoft.com/library/jj574223.aspx)에 설명된 가상화 플랫폼의 특정 기능이 필요합니다.
- MIM 서비스에 대한 SQL Server의 고가용성 배포에는 아래 [SQL Server 데이터베이스 스토리지](#sql-server-database-storage) 섹션에 설명된 특수 스토리지 구성이 필요합니다.  현재 모든 호스팅 공급자가 SQL Server 장애 조치(failover) 클러스터에 적합한 디스크 구성의 Windows Server 호스팅을 제공하는 것은 아닙니다.

## <a name="deployment-preparation-and-recovery-procedures"></a>배포 준비 및 복구 절차

배스천 환경의 고가용성 또는 재해 복구 준비 배포를 준비할 때는 Windows Server Active Directory, SQL Server와 공유 스토리지의 해당 데이터베이스 및 MIM 서비스와 해당 PAM 구성 요소를 설치하는 방법을 고려해야 합니다.

### <a name="windows-server"></a>Windows Server

Windows Server에는 장애 조치(failover) 클러스터로 여러 컴퓨터가 함께 작동하도록 하는 고가용성을 위한 기본 제공 기능이 포함됩니다. 클러스터된 서버는 실제 케이블 및 소프트웨어로 연결됩니다. 클러스터 노드 중 하나 이상에 장애가 발생하면 다른 노드에서 서비스를 제공하기 시작합니다. 이 프로세스를 장애 조치(failover)라고 합니다.   자세한 내용은 [장애 조치(failover) 클러스터링 개요](https://technet.microsoft.com/library/hh831579.aspx)를 참조하세요.

배스천 환경에서 애플리케이션 및 운영 체제가 보안 문제에 대한 업데이트를 수신하는지 확인합니다. 이러한 업데이트 중 일부는 서버를 다시 시작할 수 있습니다. 따라서 중단 시간이 길어지는 것을 방지하기 위해 업데이트가 서버에 적용되는 시간을 조정합니다. 한 가지 방법은 Windows Server 장애 조치(failover) 클러스터의 서버에 대해 [클러스터 인식 업데이트](https://technet.microsoft.com/library/hh831694.aspx)를 사용하는 것입니다.

배스천 환경에서 서버는 도메인에 가입되고 도메인 서비스에 종속됩니다. 실수로 DNS와 같은 서비스에 대한 특정 도메인 컨트롤러에 대한 종속성을 갖도록 구성되지 않았는지 확인합니다.

### <a name="bastion-environment-active-directory"></a>배스천 환경 Active Directory

Windows Server Active Directory 도메인 서비스에는 기본적으로 고가용성 및 재해 복구에 대한 지원이 포함됩니다.

#### <a name="preparation"></a>준비

Privileged Access Management의 일반적인 프로덕션 배포에는 배스천 환경에서 두 개 이상의 도메인 컨트롤러가 포함됩니다. 배스천 환경의 첫 번째 도메인 컨트롤러 설정 지침은 배포 문서의 2단계, [PRIV 도메인 컨트롤러 준비](step-2-prepare-priv-domain-controller.md)에 나와 있습니다.

또 다른 도메인 컨트롤러를 추가하는 절차는 [기존 도메인에 복제 Windows Server 2012 도메인 컨트롤러 설치(수준 200)](https://technet.microsoft.com/library/jj574134.aspx)를 참조하세요.  

>[!NOTE]
> 도메인 컨트롤러가 Hyper-V와 같은 가상화 플랫폼에서 호스트될 경우 [가상화된 도메인 컨트롤러 배포 및 구성](https://technet.microsoft.com/library/jj574223.aspx)의 주의 사항을 검토하세요.

#### <a name="recovery"></a>복구

중단 후 다른 서버를 다시 시작하기 전에 하나 이상의 도메인 컨트롤러가 배스천 환경에서 사용 가능한지 확인합니다.

도메인 내의 Active Directory는 [작업 마스터 작동 방법](https://technet.microsoft.com/library/cc780487.aspx)에 설명된 대로 도메인 컨트롤러에서 FSMO(Flexible Single Master Operation) 역할을 배포합니다.  도메인 컨트롤러가 실패한 경우 해당 도메인 컨트롤러에 할당된 하나 이상의 [도메인 컨트롤러 역할](https://technet.microsoft.com/library/cc786438.aspx)을 전송해야 할 수 있습니다.

해당 도메인 컨트롤러가 프로덕션에 반환되지 않도록 지정한 후 해당 도메인 컨트롤러에 할당된 역할이 있는지 확인하고 필요에 따라 다시 할당합니다. 지침은 [현재 작업 마스터 역할 소유자 보기](https://technet.microsoft.com/library/cc816893.aspx) 및 관련 문서에서 찾아볼 수 있습니다.

또한 해당 도메인 컨트롤러와 트러스트 관계가 있는 CORP 도메인의 도메인 컨트롤러뿐만 아니라 배스천 환경에 가입된 컴퓨터의 DNS 설정을 확인하여 해당 도메인 컨트롤러 컴퓨터의 IP 주소에 대한 종속성으로 하드 코드되지 않았는지 확인하는 것이 좋습니다.

### <a name="sql-server-database-storage"></a>SQL Server 데이터베이스 스토리지

고가용성 배포에는 SQL Server 장애 조치(failover) 클러스터가 필요하고 SQL Server 장애 조치(failover) 클러스터 인스턴스는 로그 스토리지와 데이터베이스의 모든 노드 간 공유 스토리지를 사용합니다. 공유 스토리지는 Windows Server 장애 조치(failover) 클러스터링 클러스터 디스크, SAN(저장 영역 네트워크)의 디스크 또는 SMB 서버의 파일 공유 형식일 수 있습니다.  공유 스토리지는 배스천 환경에만 사용해야 합니다. 배스천 환경 외부의 다른 워크로드와 스토리지를 공유하는 것은 배스천 환경의 무결성을 위협할 수 있으므로 권장되지 않습니다.

### <a name="sql-server"></a>SQL  Server

MIM 서비스를 사용하려면 배스천 환경에 SQL Server를 배포해야 합니다.   고가용성을 위해 FCI(장애 조치(failover) 클러스터 인스턴스)를 사용하여 SQL을 배포할 수 있습니다. 독립 실행형 인스턴스에서와 달리 FCI에서 SQL Server의 고가용성은 FCI에 있는 중복 노드를 통해 보호됩니다. 오류가 발생하거나 계획된 업그레이드의 경우 리소스 그룹 소유권이 다른 Windows Server 장애 조치(failover) 클러스터 노드로 이동합니다.

고가용성이 아닌 재해 복구 지원만 필요한 경우에는 장애 조치(failover) 클러스터링 대신 로그 전달, 트랜잭션 복제, 스냅샷 복제 또는 데이터베이스 미러링을 사용할 수 있습니다.   

#### <a name="preparation"></a>준비

배스천 환경에서 SQL Server를 설치하는 경우 이미 CORP 포리스트에 있는 모든 SQL Server와 독립적이어야 합니다.  또한 도메인 컨트롤러의 서버와는 다른 전용 서버에 SQL Server를 배포하는 것이 좋습니다.
자세한 내용은 [AlwaysOn 장애 조치(failover) 클러스터 인스턴스](https://msdn.microsoft.com/library/ms189134.aspx)에 대한 SQL Server 가이드에 설명되어 있습니다.

#### <a name="recovery"></a>복구

SQL Server가 재해 복구에 로그 전달을 사용하도록 구성된 경우 복구하는 동안 SQL Server 업데이트 작업을 수행해야 합니다.  또한 각 MIM 서비스 인스턴스를 다시 시작해야 합니다.

SQL Server가 실패하거나 SQL Server와 MIM 서비스 간의 연결이 끊긴 경우 SQL Server를 복원한 후 각 MIM 서비스를 다시 시작하는 것이 좋습니다.  이렇게 하면 MIM 서비스가 SQL Server와의 연결을 다시 설정합니다.

### <a name="mim-service"></a>MIM 서비스
활성화 요청을 처리하려면 MIM 서비스가 필요합니다.  활성화 요청을 계속 수신하는 동안 유지 관리를 위해 MIM 서비스를 호스트하는 컴퓨터를 중지할 수 있도록 여러 MIM 서비스 컴퓨터를 배포할 수 있습니다.  사용자가 그룹에 추가되면 MIM 서비스는 Kerberos 작업에 사용되지 않습니다.  

#### <a name="preparation"></a>준비
PRIV 도메인에 가입된 여러 서버에 MIM 서비스를 배포하는 것이 좋습니다.
고가용성에 대한 자세한 내용은 Windows Server 문서, [장애 조치(failover) 클러스터링 하드웨어 요구 사항 및 스토리지 옵션](https://technet.microsoft.com/library/jj612869.aspx) 및 [Windows Server 2012 장애 조치(failover) 클러스터 만들기](http://blogs.msdn.com/b/clustering/archive/2012/05/01/10299698.aspx)를 참조하세요.

여러 서버에서 프로덕션 배포의 경우 NLB(네트워크 부하 분산)를 사용하여 처리 부하를 분산할 수 있습니다.  하나의 공통 이름을 사용자에게 노출하도록 별칭(예: A 또는 CNAME 레코드)이 있어야 합니다.

>[!IMPORTANT]
> Windows Server 2012 R2에서 NLB 기능 이외의 부하 분산 기술을 사용하면 솔루션이 한 세션을 임의의 서버가 아닌 동일한 서버로 리디렉션합니다.

다중 서버 MIM 배포에서 각 MIM 서비스에는 외부 호스트 이름, 서비스 이름 및 서비스 파티션 이름이 있습니다.  서비스 이름의 기본값은 컴퓨터의 이름이며 외부 호스트 이름 및 서비스 파티션 이름의 기본값은 MIM 서비스를 설치하는 동안 MIM 서비스 서버 주소를 요청하는 화면에서 구성합니다. 이러한 세 가지 이름은 `resourceManagementService` 구성 노드의 특성 `externalHostName`, `serviceName` 및 `servicePartitionName`으로 %ProgramFiles%\Microsoft Forefront Identity Manager\Service\Microsoft.ResourceManagementService.exe.config 파일에 저장됩니다.  

MIM 서비스가 요청을 받으면 서비스 파티션 이름이 해당 요청에 대한 특성으로 저장됩니다.   나중에 동일한 서비스 파티션 이름을 가진 다른 MIM 서비스 설치만 해당 요청과 상호 작용하도록 허용됩니다.  결과적으로 PAM 시나리오에 수동 승인 또는 기타 수명이 긴 요청 처리가 포함되는 경우 각 MIM 서비스가 해당 구성 파일에서 같은 `servicePartitionName` 특성을 사용하는지 확인합니다.

#### <a name="recovery"></a>복구

중단 후 MIM 서비스를 다시 시작하기 전에 하나 이상의 Active Directory 도메인 컨트롤러와 SQL Server를 배스천 환경에서 사용할 수 있는지 확인합니다.  

워크플로 인스턴스는 인스턴스를 시작한 MIM 서비스 서버와 동일한 서비스 파티션 이름 및 서비스 이름을 사용하는 MIM 서비스 서버로만 완료할 수 있습니다.  특정 컴퓨터가 요청을 처리하는 MIM 서비스를 호스트하는 동안 오류가 발생하고 해당 컴퓨터가 서비스 상태로 돌아오지 않으면 새 컴퓨터에 MIM 서비스를 설치해야 합니다. 설치 후 새 MIM 서비스에서 *resourcemanagementservice.exe.config* 파일을 편집하고 새 MIM 배포의 `serviceName` 및 `servicePartitionName` 특성을 실패한 컴퓨터의 호스트 이름 및 서비스 파티션 이름과 동일하게 설정합니다.

### <a name="mim-pam-components"></a>MIM PAM 구성 요소

MIM 서비스 및 포털 설치 관리자는 PowerShell 모듈 및 두 가지 서비스를 포함하여 추가 PAM 구성 요소도 통합합니다.

#### <a name="preparation"></a>준비

MIM 서비스를 설치한 배스천 환경의 각 컴퓨터에 Privileged Access Management 구성 요소를 설치해야 합니다.  나중에 추가할 수 없습니다.

#### <a name="recovery"></a>복구

중단 복구 후 MIM 서비스가 하나 이상의 서버에서 실행되고 있는지 확인합니다.  그런 다음 `net start "PAM Monitoring service"`를 사용하여 해당 서버에서 MIM PAM 모니터링 서비스도 실행되도록 합니다.

배스천 환경 포리스트 기능 수준이 Windows Server 2012 R2인 경우 `net start "PAM Component service"` 명령을 사용하여 해당 서버에서 MIM PAM 구성 요소 서비스가 실행되도록 합니다.
