---
title: Microsoft Identity Manager 데이터 처리 | Microsoft Docs
description: 환경 내의 데이터를 식별하고 이에 대해 보고하려면 Microsoft Identity Manager 데이터 처리를 해석하고 운영 기능 및 요구 사항에 따라 지정된 시스템에서 작업을 수행합니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 05/22/2018
ms.topic: get-started-article
ms.prod: microsoft-identity-manager
ms.assetid: b0b39631-66df-4c5f-80c9-a1774346f816
ms.suite: ems
ms.openlocfilehash: 4102ffc450b993faaa62da66bb25f242b7e39280
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358723"
---
# <a name="microsoft-identity-manager-data-handling"></a>Microsoft Identity Manager 데이터 처리 

이 문서에서는 검색, 삭제, 업데이트 및 보고 작업을 통해 조직이 여러 연결된 데이터 원본에서 연습하거나 구현해야 하는 의사 결정을 내리는 방법에 대한 지침을 제공합니다. 삭제 또는 업데이트 방법을 결정하기 전에 ID 관리자 시스템(MIM)의 현재 디자인 및 구성을 이해하는 것이 중요합니다. 다음은 고객이 다음 질문에 대해 고려하고 답변해야 하는 몇 가지 시나리오입니다. 

- 비즈니스 프로세스에서 도움이 되는 ID 관리에 필요한 데이터는 무엇인가요?
- 현재 데이터는 MIM에서 어디에 저장되나요?
- 시스템에서 이 데이터를 어떻게 사용하나요?
- 이 데이터를 외부 파트너 데이터 원본(내보내기)과 공유하고 있나요?
- 데이터의 신뢰할 수 있는 원본 및 해당 원본의 처리란 무엇인가요?
- 데이터 보존 및 데이터 삭제 플랜은 어디에 있나요?
- 데이터를 처리하고 관리하는 데 필요한 기술을 모두 식별했나요?

현재 MIM 환경을 해석하는 데 도움이 되도록 다음 도구를 사용하여 MIM 환경을 문서화하거나, 구현 디자인 문서를 따를 수 있습니다.
- [MIM 구조 분석 - 현재 구성을 내보낼 수 있음](https://github.com/Microsoft/MIMConfigDocumenter)

## <a name="searching-for-and-identifying-personal-data"></a>개인 데이터에 대한 검색 및 식별
MIM에서 데이터를 검색하는 작업은 구성 및 설정에 따라 달라집니다. 대부분의 환경은 상호 연결되지만 이해를 돕기 위해 상위 수준 구성 요소로 구분했습니다.

### <a name="synchronization-service"></a>동기화 서비스

사용자와 관련된 MIM의 모든 데이터는 AD(Active Directory) 및 HR 데이터 원본에서 파생됩니다. 개인 데이터를 검색할 때 가장 먼저 고려해야 할 것은 AD 또는 연결된 데이터 원본입니다. 

MIM Synchronization Service Manager 콘솔에서 이 사용자를 추적할 수 있는 인증 원본이 확실하지 않은 경우 메타버스 검색 창을 클릭하여 데이터베이스에 저장된 식별 가능한 개인 데이터를 봅니다. 사용자는 특정 사용자 또는 특성을 검색할 수 있습니다.

- 사용자 개체 데이터의 검토 또는 검색을 수행하려면
    - 동기화 서비스 클라이언트 열기
        - 메타버스 디자이너를 사용하면 특성 흐름 가져오기 및 우선 순위를 확인할 수 있습니다.
![mim-privacy-compliance_1.PNG](media/mim-privacy-compliance/mim-privacy-compliance_1.PNG)
        - 메타버스 검색을 사용하면 데이터베이스 ![mim-privacy-compliance_2.PNG](media/mim-privacy-compliance/mim-privacy-compliance_2.PNG)에서 모든 개체 및 특성을 검색할 수 있습니다.
 
개체를 찾은 후 개체를 클릭하면 사용자 프로필 페이지가 열립니다. 개체 세부 정보는 개체에 대한 포괄적인 세부 정보, 해당 특성, 마지막으로 수정된 날짜 및 인증 원본, 아래 관리 에이전트 구성 예제에서 파생된 관련 연결된 데이터 원본을 제공합니다.

![mim-privacy-compliance.PNG](media/mim-privacy-compliance/mim-privacy-compliance.PNG)

### <a name="service-and-portal--pam"></a>서비스 및 포털/PAM
서비스 및 포털 또는 PAM의 인스턴스가 설치되어 있는 경우 사용자를 검색할 수 있는 것이 중요합니다. 

포털을 설치한 경우, UI를 사용하여 특정 사용자의 모든 특성 또는 쿼리를 검색할 수 있습니다.

서비스 서버(포털 UI 없음)만 설치된 경우에는 [여기](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)에 있는 [FIMAutomation PSSnapin] 예제를 기반으로 검색 구문을 실행할 수 있습니다.

PAM은 위의 동일한 구문을 사용하거나 [MIMPAM 모듈](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), 특히 get-pamuser cmdlet을 사용하여 PAM 환경 내에서 사용자를 검색할 수 있습니다.

사용 가능한 데이터를 검색하기 위한 기타 보고 옵션은 서비스 및 포털에 있습니다.
- [하이브리드 보고](https://docs.microsoft.com/microsoft-identity-manager/identity-manager-hybrid-reporting-azure)
- [SCSM으로 보고](https://docs.microsoft.com/previous-versions/mim/jj133853%28v%3dws.10%29)

### <a name="bhold"></a>BHOLD
Bhold 코어 서비스에는 사용자 또는 특성을 검색할 수 있는 UI가 있습니다. 

![bhold 검색](media/mim-privacy-compliance/mim-privacy-compliance-bhold.PNG)

동기화 서비스를 위해 BHOLD를 [액세스 관리 커넥터](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-access-management-connector-install)와 동기화하는 경우 연결된 사용자 개체와 BHOLD 코어로 전송하는 특성을 확인할 수 있습니다.

또한 BHOLD Reporting 모듈을 로드할 수 있습니다.

- [BHOLD Reporting](https://docs.microsoft.com/microsoft-identity-manager/bhold/bhold-concepts-guide#reporting)

### <a name="certificate-management"></a>인증서 관리
인증서 관리 서비스 검색은 UI에서 기본 제공됩니다. 관리자가 ‘사용자 찾기 및 해당 정보 보기 또는 관리’를 시작하고 선택합니다.  

![cm 검색](media/mim-privacy-compliance/mim-privacy-compliance-cm.PNG)

## <a name="exporting-personal-data"></a>개인 데이터 내보내기
MIM의 엔터티와 관련된 데이터가 여러 원본에서 파생되므로 대부분의 데이터는 동기화 서비스 데이터베이스에 저장됩니다. 따라서 MIM 동기화에서 개체 관련 데이터를 내보내야 하거나 이 데이터의 소유자를 확인할 수 있습니다.

### <a name="synchronization-service"></a>동기화 서비스
데이터를 내보내기 위한 동기화 서비스는 검색 UI에서 데이터를 선택하고 csv 또는 선호하는 형식으로 복사하여 붙여넣습니다. 이 데이터를 내보내는 또 다른 방법은 플래그 지정된 관심 있는 사용자에 대해 필요한 현재 데이터를 삭제하기 위한 파일 기반 MA를 만드는 것입니다. 파일 기반 MA 사용의 예제는 [여기](https://blogs.msdn.microsoft.com/connector_space/2016/11/17/management-agent-configuration-part-4-delimited-text-file-management-agent/)에서 찾을 수 있습니다.


### <a name="service-and-portal--pam"></a>서비스 및 포털/PAM
이 데이터를 내보낼 수 있는 PAM과 함께 서비스 및 포털은 [여기](https://social.technet.microsoft.com/wiki/contents/articles/22713.fim-portals-use-powershell-to-find-all-users-without-a-manager.aspx)에 있는 [FIMAutomation PSSnapin], 예제를 기반으로 검색 구문을 실행하고 [csv](https://docs.microsoft.com/powershell/module/microsoft.powershell.utility/export-csv?view=powershell-6)에 파이프합니다.

PAM은 위의 동일한 구문을 사용하거나 [MIMPAM 모듈](https://docs.microsoft.com/powershell/module/mimpam/get-pamuser?view=idm-ps-2016sp1), 특히 get-pamuser를 사용하여 PAM 환경 내에서 사용자를 검색하고 csv에 파이프할 수 있습니다.

- [PowerShell을 사용하여 MIM 서비스를 쿼리하는 예제](https://gallery.technet.microsoft.com/Querying-The-FIMMIM-dcb82de3)

### <a name="bhold"></a>BHOLD
Bhold 데이터는 bhold 보고 모듈을 사용하여 선호하는 형식으로 내보낼 수 있습니다.

### <a name="certificate-management"></a>인증서 관리
개인 데이터와 관련된 인증서 관리 데이터는 Active Directory에 연결됩니다. 관리자는 Active Directory Powershell을 사용하여 이 데이터를 내보낼 수 있습니다.

## <a name="updating-personal-data"></a>개인 데이터 업데이트

MIM 솔루션의 사용자 또는 개체에 대한 개인 데이터는 일반적으로 조직의 연결된 데이터 원본에 있는 사용자 개체에서 파생됩니다. HR 원본 또는 AD와 같은 다른 신뢰할 수 있는 레코드 시스템의 사용자 프로필에 대한 모든 변경 내용이 MIM 동기화 서비스에 반영되기 때문입니다.

### <a name="synchronization-service"></a>동기화 서비스

관리 작업을 수행하려면 관리자가 [여기](https://docs.microsoft.com/previous-versions/mim/jj590183(v%3dws.10))에 정의된 동기화 작업 또는 관리에 속해야 합니다.

데이터 업데이트는 인증 원본에서 규칙을 정의하여 수행됩니다. 관리 콘솔을 통해 원본에서 업데이트할 인증 원본을 식별할 수 있습니다. 다른 옵션은 동기화 규칙 또는 규칙 확장을 만들어 HR 데이터와 같은 원본을 유지해야 하는 경우 데이터 업데이트를 제어하는 것입니다. 이러한 옵션은 사용 가능한 지원 옵션입니다.

특성을 업데이트하는 다른 방법에 대한 자세한 내용은 아래를 참조하세요. 

- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)(규칙 확장 사용)
- [Understanding Data Synchronization with External Systems](https://docs.microsoft.com/previous-versions/mim/jj133850(v%3dws.10))(외부 시스템과 데이터 동기화 이해)

### <a name="service-and-portal--pam"></a>서비스 및 포털/PAM

PAM 데이터를 포함할 서비스 및 포털은 FIMAutomation 또는 PAM cmdlet을 사용하여 업데이트할 수 있습니다. 포털이 있는 경우 개체를 검색하고 수정하여 직접 업데이트할 수도 있습니다. 구성에 따라 단순히 포털에서 업데이트하면 원본이 유지되는 것은 아니라는 점에 유의해야 합니다. 인증 원본은 전체 구성에 따라 크게 달라집니다.

### <a name="bhold"></a>BHOLD

사용자는 BHOLD 코어 사용자 인터페이스 또는 액세스 관리 커넥터로 직접 업데이트할 수 있습니다.

### <a name="certificate-management"></a>인증서 관리

인증서 관리 서비스의 사용자는 모두 Active Directory의 리플렉션입니다. 업데이트하려면 Active Directory를 사용하여 개체 세부 정보를 변경합니다.

## <a name="deleting-personal-data"></a>개인 데이터 삭제

>[!Note] 
> 이 문서는 Microsoft Identity Manager에서 개인 데이터를 삭제하는 방법에 대한 지침을 제공하며 GDPR에 따른 의무를 지원하는 데 사용될 수 있습니다. GDPR에 대한 일반적인 정보를 찾는 경우 [Service Trust Portal의 GDPR 섹션](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)을 참조하세요.

MIM의 데이터가 동기화되고 연결된 데이터 원본에서 항상 업데이트됩니다. 대상에서 개체가 삭제되면 보안 조사를 위해 MIM의 개체 데이터를 유지 관리할 수 있습니다. 연결된 데이터 원본 규칙 또는 규칙 확장(코드) 및/또는 개체 삭제 규칙에 따라 개체 삭제가 구성됩니다.

### <a name="synchronization-service"></a>동기화 서비스
동기화 서비스는 비즈니스 프로세스에 따라 데이터를 처리하거나 데이터를 삭제하는 여러 가지 방법을 제공합니다. 이해를 돕기 위해 다음은 특성 삭제 및 업데이트 옵션을 이해하는 데 도움이 되는 몇 가지 문서입니다. 

- [Understanding Deprovisioning](https://social.technet.microsoft.com/wiki/contents/articles/1270.understanding-deprovisioning-in-fim.aspx)(프로비전 해제 이해)
- [Using Rules Extensions](https://msdn.microsoft.com/library/windows/desktop/ms698810(v=vs.100).aspx)(규칙 확장 사용)
- [MIM Best Practices](https://docs.microsoft.com/microsoft-identity-manager/mim-best-practices)(MIM 모범 사례)

### <a name="service-and-portal--pam"></a>서비스 및 포털/PAM

기본 30일 시스템 리소스 보존 구성을 유지하는 서비스 및 포털에 대해 권장됩니다. 이는 요청 데이터뿐 아니라 시스템에서 지워야 하는 모든 개체를 삭제할 경우, 시스템에 알립니다. 프로세스가 발생하면 이 개체에 연결된 모든 데이터가 삭제됩니다. 여기에는 모든 SSPR 등록 데이터가 포함됩니다. 이는 위의 개체 삭제 구성에 따라 결정됩니다. 개체의 GUID를 저장할 하나의 테이블이 있습니다. 빌드 4.4.1459에서 전체 테이블 크기를 줄이기 위해 FIM_DeleteExpiredSystemObjectsJob이라는 프로세스를 추가했습니다. 이 프로세스에 대한 자세한 내용은 [여기](https://support.microsoft.com/en-us/help/4012498/hotfix-rollup-package-build-4-4-1459-0-is-available-for-microsoft-iden)를 참조하세요.

![mim-privacy-compliance-srrc.PNG](media/mim-privacy-compliance/mim-privacy-compliance-srrc.PNG)


### <a name="bhold"></a>BHOLD

동기화 서비스에 연결된 대부분의 시스템처럼 Bhold는 HR 같은 원본 개체가 제거된 후 삭제되도록 구성할 수 있습니다. 이는 관리 에이전트에서 구성되고 동기화 서비스 기능에 설명된 대로 개체 삭제 규칙에 의해 제어됩니다.

다른 옵션은 BHOLD 코어 사용자 인터페이스에서 사용자 개체를 제거하는 것입니다. 설정에 따라 제대로 작동할 수 있지만 원본에서 삭제되지 않을 경우 프로비저닝 논리가 이 사용자를 다시 만들 수 있습니다.
![mim-privacy-compliance-bholdr.PNG](media/mim-privacy-compliance/mim-privacy-compliance-bholdr.PNG)


### <a name="certificate-management"></a>인증서 관리
CM에서 사용자를 제거하려면 Active Directory에서 사용자를 삭제합니다.

인증서 관리는 도메인 sAMAccountName을 사용하여 인증서 서비스의 프로파일 UID만 저장합니다. 사용자가 AD에서 삭제되면 사용자가 등록한 인증서에 대한 사용자 캐시만 존재합니다. 이로 인해 환경 운영에 전체적으로 피해를 줄 수 있으므로 데이터베이스에서 어떤 것도 삭제하지 않는 것이 좋습니다.

## <a name="opt-out-of-telemetry"></a>원격 분석 옵트아웃
이전 빌드 FIM/MIM은 각 배포에 대한 익명화된 원격 분석을 수집하고 이 데이터를 HTTPS를 통해 Microsoft 서버로 전송하는 데 사용되었습니다. 이 데이터는 이전에는 Microsoft에서 향후 버전의 FIM/MIM을 개선하는 데 사용되었습니다.

>[!Note] 
> 4.5.x.x 이상 데이터 컬렉션의 이후 릴리스는 사용할 수 없습니다.

이전 버전의 데이터 컬렉션을 사용하지 않으려면 변경 모드를 실행하고 다음 프롬프트를 선택 취소합니다.

![mim-privacy-compliance-ceip.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip.PNG)

또는 레지스트리를 편집하고 값을 0으로 설정: (구성 요소)CEIP HKLM\SOFTWARE\Microsoft\Forefront Identity Manager\2010

![mim-privacy-compliance-ceip2.PNG](media/mim-privacy-compliance/mim-privacy-compliance-ceip2.PNG)

## <a name="next-steps"></a>다음 단계 
- [SQL 관련 개인 정보 취급 방침 관련 지침](https://docs.microsoft.com/sql/relational-databases/security/microsoft-sql-and-the-gdpr-requirements?view=sql-server-2017)
- [Service Trust Portal의 GDPR 섹션](https://servicetrust.microsoft.com/ViewPage/GDPRGetStarted)
- [FIM 2010 Archive: Ramp Up - Implementing Forefront Identity Manager 2010](https://social.technet.microsoft.com/wiki/contents/articles/35789.fim-2010-archive-ramp-up-implementing-forefront-identity-manager-2010.aspx)(FIM 2010 보관: 확장 - Forefront Identity Manager 2010 구현)
