---
title: Active Directory Domain Services에 대한 권한 있는 액세스 관리 | Microsoft 문서
description: Privileged Access Management란 무엇이고 Active Directory 환경 관리 및 보호에 어떤 도움이 되는지 알아봅니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: cf3796f7-bc68-4cf7-b887-c5b14e855297
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: f3c5a74966498264cd60033b2f7751622d111e2e
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49333550"
---
# <a name="privileged-access-management-for-active-directory-domain-services"></a>Active Directory 도메인 서비스에 대한 Privileged Access Management

PAM(Privileged Access Management)은 조직이 기존 Active Directory 환경 내에서 권한 있는 액세스를 제한할 수 있도록 지원하는 솔루션입니다.

Privileged Access Management는 다음 두 가지 목표를 수행합니다.

- 악의적인 공격에 영향을 받지 않는 별도의 배스천 환경을 유지 관리함으로써 손상된 Active Directory 환경에 대한 제어를 다시 설정합니다.
- 권한 있는 계정의 사용을 격리하여 해당 자격 증명의 도난 위험을 줄입니다.

> [!NOTE]
> PAM은 MIM(Microsoft Identity Manager)을 사용하여 구현된 PIM([Privileged Identity Management](https://azure.microsoft.com/documentation/articles/active-directory-privileged-identity-management-configure/))의 인스턴스입니다.

## <a name="what-problems-does-pam-help-solve"></a>PAM은 어떤 문제를 해결하는 데 도움을 주나요?

요즘 회사에서는 Active Directory 환경 내에서의 리소스 액세스가 문제가 되고 있습니다. 특히 까다로운 요인은 다음과 같습니다.

- 취약성
- 승인되지 않은 권한 상승
- [Pass-the-hash](https://technet.microsoft.com/dn785092.aspx)
- Pass-the-ticket
- 스피어 피싱
- Kerberos 타협
- 기타 공격

요즘 공격자는 너무 쉽게 Domain Admins 계정 자격 증명을 얻고 사후에 이러한 공격자를 확인하기는 너무 어렵습니다. PAM의 목표는 악의적인 사용자가 권한을 얻을 수 있는 기회를 줄이는 반면 환경에 대한 사용자의 제어 및 인식을 높이는 것입니다.

PAM을 사용하면 공격자가 네트워크에 침투하고 권한 있는 계정 액세스 권한을 얻기 어려워집니다. PAM은 다양한 도메인에 조인된 컴퓨터 및 해당 컴퓨터의 응용 프로그램에서 액세스를 제어하는 권한 있는 그룹을 추가로 보호합니다. 또한 자세한 모니터링, 자세한 표시 및 더 세분화된 컨트롤을 추가합니다. 그러면 조직에서 권한 있는 관리자 및 해당 관리자의 작업을 볼 수 있습니다. PAM은 환경에서 관리 계정이 어떻게 사용되는지 조직이 확인할 수 있습니다.

## <a name="setting-up-pam"></a>PAM 설정

PAM은 Just-in-Time 관리의 원칙을 작성하며 [JEA(Just Enough Administration)](http://channel9.msdn.com/Events/TechEd/NorthAmerica/2014/DCIM-B362)와 관련됩니다. JEA는 권한 있는 작업을 수행하기 위한 명령 집합을 정의하는 Windows PowerShell 도구 키트입니다. 관리자가 명령을 실행하는 권한 부여를 얻을 수 있는 엔드포인트입니다. JEA에서 관리자는 특정 권한이 있는 사용자가 특정 작업을 수행할 수 있도록 결정합니다. 적합한 사용자가 해당 작업을 수행해야 할 때마다 해당 사용 권한을 사용할 수 있습니다. 악의적인 사용자가 액세스 권한을 도용할 수 없도록 사용 권한은 지정된 기간 후 만료됩니다.

PAM 설정 및 작업은 네 단계가 있습니다.

![PAM 단계: 준비, 보호, 작동, 모니터링 - 다이어그램](media/MIM_PIM_SetupProcess.png)

1. **준비**: 기존 포리스트서 어떤 그룹이 상당한 권한이 있는지 식별합니다. 배스천 포리스트에서 구성원 없이 이러한 그룹을 다시 만듭니다.
2. **보호**: 사용자가 Just-In-Time 관리를 요청하는 경우 MFA(Multi-Factor Authentication)와 같은 수명 주기 및 인증 보호를 설정합니다. MFA는 프로그래밍 공격을 악성 소프트웨어 또는 다음의 자격 증명 도난에서 방지합니다.
3. **작동**: 인증 요구 사항이 충족되고 요청이 승인된 후에 사용자 계정을 배스천 포리스트의 권한 있는 그룹에 일시적으로 추가합니다. 미리 설정 시간 동안 관리자는 해당 그룹에 할당되는 모든 권한 및 액세스 권한을 갖습니다. 이 시간 후 계정이 그룹에서 제거됩니다.
4. **모니터링**: PAM은 권한 있는 액세스 요청의 감사, 경고 및 보고서를 추가합니다. 권한 있는 액세스의 기록을 검토할 수 있으며 작업을 수행한 사용자를 참조할 수 있습니다. 활동이 유효한지를 결정하고 원래 포리스트에서 권한 있는 그룹에 직접 사용자를 추가하려는 시도와 같은 권한 없는 활동을 식별할 수 있습니다. 이 단계는 "내부" 공격자를 추적할 뿐만 아니라 악성 소프트웨어를 식별하기도 합니다.

## <a name="how-does-pam-work"></a>PAM 작동 방법

PAM은 AD DS의 새로운 기능, 특히 도메인 계정 인증 및 권한, 그리고 Microsoft Identity Manager의 새로운 기능을 기반으로 합니다. PAM은 기존 Active Directory 환경에서 권한 있는 계정을 분리합니다. 권한 있는 계정을 사용해야 하는 경우 먼저 요청한 다음 승인해야 합니다. 승인 후에 권한 있는 계정은 사용자 또는 응용 프로그램의 현재 포리스트가 아닌 새 요새 포리스트에서 외래 주체 그룹을 통해 권한이 주어집니다. 방호 포리스트를 사용하면 조직은 사용자가 권한 있는 그룹의 멤버일 경우 사용자 인증을 하는 방법과 같은 제어를 강화합니다.

또한 Active Directory, MIM 서비스 및 솔루션의 다른 부분은 고가용성 구성으로 배포할 수 있습니다.

다음 예제에서는 PIM가 작동하는 방법을 자세히 보여줍니다.

![PIM 프로세스 및 참가자 - 다이어그램](media/MIM_PIM_howitworks.png)

배스천 포리스트는 시간 제한된 그룹 구성원 자격을 발급한 다음 시간 제한된 TGT(Ticket-Granting Ticket)를 생성합니다. 앱과 서비스가 배스천 포리스트를 신뢰하는 포리스트에 있는 경우 Kerberos 기반 응용 프로그램 또는 서비스는 이러한 TGT를 처리할 수 있습니다.

일상적인 사용자 계정은 새 포리스트로 이동할 필요가 없습니다. 컴퓨터, 응용 프로그램 및 해당 그룹도 마찬가지입니다. 기존 포리스트에서 현재 있는 곳을 유지합니다. 요즘 이러한 사이버 안보 문제를 우려하지만 즉시 계획이 Windows Server의 다음 버전에 서버 인프라를 업그레이드할 즉각적인 계획이 없는 조직의 예를 살펴보십시오. 해당 조직은 MIM 및 새 요새 포리스트를 사용하여 이 결합된 솔루션을 활용할 수 있고 기존 리소스에 액세스를 더 잘 제어할 수 있습니다.

PAM은 다음과 같은 이점이 있습니다.

- **권한의 격리/범위**: 사용자는 메일 확인이나 인터넷 검색 등 권한 없는 작업에도 사용되는 계정에서 권한이 없습니다. 사용자는 권한을 요청해야 합니다. 요청은 PAM 관리자가 정의한 MIM 정책을 기반으로 승인 또는 거부됩니다. 요청이 승인될 때까지 권한 있는 액세스는 사용할 수 없습니다.

- **버전 업그레이드 및 증명**: 별도 관리자 계정의 수명 주기를 관리할 수 있는 새 인증 및 권한 부여 과제입니다. 사용자는 관리 계정의 권한 상승을 요청할 수 있으며 해당 요청은 MIM 워크플로를 거칩니다.

- **추가 로깅**: 기본 제공 MIM 워크플로와 함께 요청, 요청 인증 방법 및 승인 후에 발생하는 이벤트를 식별하는 PAM에 대한 추가 로깅이 있습니다.

- **사용자 지정 가능한 워크플로**: MIM 워크플로는 다른 시나리오에서 구성할 수 있고 요청하는 사용자 또는 요청된 역할의 매개 변수를 기반으로 다양한 워크플로를 사용할 수 있습니다.

## <a name="how-do-users-request-privileged-access"></a>사용자가 권한 있는 액세스를 요청하는 방법

사용자는 다음과 같이 다양한 방법으로 요청을 제출할 수 있습니다.

- MIM 서비스 웹 서비스 API
- REST 엔드포인트
- Windows PowerShell(`New-PAMRequest`)

[권한 있는 액세스 관리 cmdlet](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam)에 대해 자세히 알아보세요.

## <a name="what-workflows-and-monitoring-options-are-available"></a>어떤 워크플로 및 모니터링 옵션을 사용할 수 있습니까?

예로써 PIM을 설정하기 전에 사용자가 관리 그룹의 멤버라고 가정해 봅니다. PIM 설치의 일부로써 사용자를 관리 그룹에서 제거하고 MIM에서 정책을 만듭니다. 정책은 해당 사용자가 관리자 권한을 요청하고 MFA로 인증되었는지 지정합니다. 요청이 승인되고 요새 포리스트의 권한 있는 그룹에 사용자를 위한 별도 계정을 추가합니다.

요청이 승인되었다고 가정하고 작업 워크플로가 직접 요새 포리스트 Active Directory와 통신하여 그룹에 사용자를 넣습니다. 예를 들어 Jen이 HR 데이터베이스 관리를 요청하면 Jen에 대한 관리 계정은 몇 초 이내 요새 포리스트에서 권한 있는 그룹에 추가됩니다. 해당 그룹에서 관리자 계정의 구성원 자격은 시간 제한이 지나면 만료됩니다. Windows Server 기술 미리 보기를 사용하여 해당 멤버 자격을 요새 포리스트의 Windows Server 2012 R2와 함께 시간 제한이 있는 Active Directory와 연결합니다. MIM로 해당 시간 제한을 적용합니다.

> [!NOTE]
> 그룹에 새 멤버를 추가하면 변경을 요새 포리스트의 다른 도메인 컨트롤러(DC)에 복제해야 합니다. 복제 대기 시간은 리소스에 액세스하는 사용자에 대한 성능에 영향을 줄 수 있습니다. 복제 지연에 대한 자세한 내용은 [Active Directory 복제 토폴로지 작동 방법](https://technet.microsoft.com/library/cc755994.aspx)을 참조하세요.
>
> 반면 SAM(보안 계정 관리자)은 만료되는 링크를 실시간으로 평가합니다. 그룹 구성원 추가는 액세스 요청을 수신하는 DC에서 복제해야 하지만 그룹 구성원 제거는 모든 DC에서 즉각적으로 평가됩니다.

이 워크플로는 특별히 이러한 관리 계정의 대상입니다. 권한 있는 액세스에 가끔식 액세스가 필요한 관리자(또는 스크립트)는 해당 액세스를 정확하게 요청할 수 있습니다. MIM은 Active Directory에서 요청 및 변경 내용을 기록하여 System Center 2012 - Operations Manager ACS(Audit Collection Services) 또는 기타 타사 도구와 같은 솔루션을 모니터링하는 회사에 데이터를 전송하거나 이벤트 뷰어에서 볼 수 있습니다.

## <a name="next-steps"></a>다음 단계

- [PtH(Pass-the-Hash) 공격 및 기타 자격 증명 도난, 버전 1 및 2 완화](https://www.microsoft.com/download/details.aspx?id=36036)
- [Privileged Access Management cmdlet](https://docs.microsoft.com/powershell/identitymanager/mimpam/vlatest/mimpam)