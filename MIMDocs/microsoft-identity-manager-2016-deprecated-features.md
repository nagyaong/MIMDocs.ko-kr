---
title: 더 이상 사용되지 않는 MIM 기능 및 향후 계획 | Microsoft Docs
description: 이 문서에서는 더 이상 사용되지 않는 MIM(Microsoft Identity Manager) 2016 SP1의 기능에 대해 설명합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/28/2018
ms.topic: reference
ms.prod: microsoft-identity-manager
ms.assetid: ''
ms.openlocfilehash: fcf9ec8387761b6f154a95d6100ef54a12d4caf8
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358442"
---
# <a name="deprecated-features"></a>더 이상 사용되지 않는 기능

이 문서에서는 더 이상 사용되지 않는 Microsoft Identity Manager 2016 SP1의 기능에 대해 설명합니다. 이러한 기능은 Microsoft Identity Manager에 여전히 있는 경우에도 계속 지원됩니다. 그러나 기능 릴리스에서 제거될 수 있으므로 새 배포에는 권장되지 않습니다.  개발자는 새 응용 프로그램이나 솔루션에서 더 이상 사용되지 않는 기능을 활용하지 않는 것이 좋습니다.

> [!NOTE]
> Microsoft Identity Manager SP1에서 제거될 기능은 **로 식별됩니다. <br>
> 지원에 대한 자세한 내용은 [Microsoft Identity Manager 수명 주기](https://support.microsoft.com/en-us/lifecycle/search?alpha=Microsoft%20Forefront%20Identity%20Manager%202010%20R2%20Service%20Pack%201,Microsoft%20Identity%20Manager%202016,Microsoft%20Forefront%20Identity%20Manager%202010)를 참조하세요.


## <a name="bhold"></a>BHOLD 

고객은 Microsoft BHOLD Suite 구성 요소의 새 배포를 시작하지 않는 것이 좋습니다. 기존의 BHOLD 배포는 계속 지원됩니다. Azure AD는 이제 BHOLD 증명 캠페인 기능 중 일부를 대체하는 [액세스 검토](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-azure-ad-controls-access-reviews-overview)를 제공합니다.

## <a name="certificate-management"></a>인증서 관리 

| **범주**                | **더 이상 사용되지 않는 기능**              | **대체 및 설명**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 관리 에이전트 | **FIM 인증서 관리 | FIM 인증서 관리 에이전트는 MIM 2016에서 제거되었습니다.                                                             |

## <a name="service-and-portal"></a>서비스 및 포털

| **범주**                | **더 이상 사용되지 않는 기능**              | **대체 및 설명**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 프로그래밍 방식 구성 | 웹 서비스 구성 인터페이스(ma-data 및 mv-data) | FIM 서비스 웹 서비스를 통해 FIM 동기화 서비스를 구성하는 기능은 다음 버전에서 제거됩니다.
|

## <a name="synchronization-service"></a>동기화 서비스 

| **범주**                | **더 이상 사용되지 않는 기능**              | **대체 및 설명**           |
|-----------------------------|-------------------------------------|----------------------------------------------|
| 프로그래밍 방식 구성 | 웹 서비스 구성 인터페이스 | FIM 서비스를 통해 FIM 동기화 서비스를 구성하는 기능은 다음 버전에서 제거됩니다.                                                          |
| 관리 에이전트           | 기본 제공 MA                        | MIM 2016에서 제거된 MA는 다음과 같습니다. </br> 1. **FIM 인증서 관리용 MA </br>2. **Lotus Notes용 MA</br> 3. **SAP R/3용 MA </br> Lotus Notes 및 SAP R/3 MA는 새 버전으로 대체되었습니다. 자세한 내용은 [최신 커넥터 버전 릴리스 내역 및 다운로드](https://docs.microsoft.com/en-us/azure/active-directory/connect/active-directory-aadconnectsync-connector-version-history)를 참조하세요.                                                                                                                                                                                                                                              |
| 관리 에이전트           | ECMA1                               | ECMA1/XMA 확장성 프레임워크는 ECMA 2.0으로 대체되었습니다. 기존 ECMA1 관리 에이전트를 ECMA2.0 커넥터로 업데이트해야 합니다.                                                                                                                                          |
| 관리 에이전트           | 외부에서 커넥터 실행      | 이 기능은 대체되지 않습니다. 동기화 서비스는 항상 동일한 프로세스에서 커넥터를 호출합니다. 커넥터에서 다른 프로세스를 시작하고 관리해야 합니다. |
| 관리 에이전트           | 파티션 표시 이름 구성    | 이 기능은 대체되지 않습니다. 이 옵션은 WMI 인터페이스에서 파티션에 대한 대체 이름을 제공하는 데만 사용되었습니다.                                                                                                                                                                       |
| 실행 프로필                | 결합된 프로필                   | 결합된 프로필 델타 가져오기/동기화, 전체 가져오기/델타 동기화 및 전체 가져오기/동기화가 제거됩니다. 대신 두 단계로 구성된 실행 프로필을 사용해야 합니다. 

> [!NOTE]
> 많은 수의 기존 단로기로 인해 성능상의 영향을 받는 환경에서만 결합된 실행 프로필을 유지해야 합니다.


| **범주**                | **더 이상 사용되지 않는 기능**              | **대체 및 설명**           |
|--------|-------|---|    
| 특성 우선 순위 | 다중 마스터/ 우선 순위                       | 같음 우선 순위가 제거됩니다. 이 기능을 대체할 수 있는 기능이 없습니다. 대신 우선 순위를 수동으로 구성해야 합니다. 환경에 FIM 서비스 관리 에이전트가 배포되어 있으면 이 기능을 계속 사용할 수 있습니다. 이 관리 에이전트는 선언적 프로비전에 대한 선행이 아닌 내보내기를 방지하기 위해 수동 우선 순위를 제공하지 않습니다. |
| 조인 규칙           | "Any" 개체 형식에서 조인                             | 이 기능은 대체되지 않습니다. 모든 조인 규칙은 조인하려는 메타버스 개체 형식을 명시적으로 정의해야 합니다.       |
| 특성 흐름      | 내보낸 값에 대한 "Null 허용" 선택 취소            | 이 기능은 대체되지 않습니다. "Null 허용"은 항상 선택됩니다. 현재 환경에서 "Null 허용"이 선택되어 있는지 확인해야 합니다.  |
| 특성 흐름      | "특성을 회수하지 않음"                            | 이 기능은 대체되지 않습니다. 특성은 항상 회수되는 것이 가장 좋습니다.  |
| 규칙 확장      | 외부에서 메타버스 및 MA 규칙 확장 실행 | 이 기능은 대체되지 않습니다. 메타버스 및 특성 흐름 규칙은 동기화 엔진과 동일한 프로세스에서 실행됩니다.       |
| 규칙 확장      | 트랜잭션 속성                                | 이 기능은 대체되지 않습니다. 이 유틸리티 클래스를 사용하여 인바운드, 프로비전 및 아웃바운드 동기화 간에 데이터를 전달하지 않도록 방지해야 합니다.  |
| 규칙 확장      | ExchangeUtils: Create55\* 메서드                     | Exchange 5.5 서버에 대한 개체를 만드는 메서드가 제거됩니다.        |
| 인터페이스            | Mms_Metaverse                                        | 모든 ClmUtils 클래스 멤버는 다음 버전에서 제거됩니다.   |

## <a name="next-steps"></a>다음 단계
다음에 대해 자세히 알아보세요.

- Microsoft Identity Manager는 여전히 이전 버전인 Forefront Identity Manager와 밀접한 관련이 있습니다. FIM을 계속 사용하거나 추가 설명서를 참조하려는 경우, [FIM 2010 R2 설명서 로드맵](https://technet.microsoft.com/library/jj133885.aspx)을 확인하세요.
- [MIM 배포를 위한 토폴로지 고려 사항](topology-considerations.md) 이 문서에서는 구현을 고려할 수 있는 다양한 배포 토폴로지를 소개합니다.
- [용량 계획 가이드](capacity-planning-guide.md) 테스트 환경과 함께 이 가이드를 사용하여 배포에 적절한 범위를 파악할 수 있습니다.
