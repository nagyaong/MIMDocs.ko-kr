---
title: "MIM 2016을 사용하여 Azure에서 하이브리드 보고 작업 | Microsoft 문서"
description: "Azure에서 온-프레미스 데이터와 클라우드 데이터를 하이브리드 보고서에 결합하는 방법 및 이러한 보고서를 보고 관리하는 방법을 알아봅니다."
keywords: 
author: fimguy
ms.author: barclayn
manager: mbaldwin
ms.date: 10/12/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 17745bfdba831364d32bc2786cc2a38191fe6cc7
ms.sourcegitcommit: e52bab207117390997c6fa8450de24335b502673
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 12/20/2017
---
# <a name="working-with-identity-manager-hybrid-reporting---public-preview-refresh"></a>ID 관리자 하이브리드 보고 작업 - 공개 미리 보기(새로 고침)

## <a name="available-hybrid-reports"></a>사용 가능한 하이브리드 보고서
Azure AD에서 사용할 수 있는 처음 세 가지 MIM(Microsoft Identity Manager) 보고서는 **암호 재설정 작업**, **암호 재설정 등록** 및 **셀프 서비스 그룹 작업**입니다.

-   암호 재설정 작업에는 사용자가 SSPR을 사용하여 암호를 재설정했고 인증에 사용된 **방법** 또는 게이트를 제공할 때의 각 인스턴스가 표시됩니다.

-   암호 재설정 등록에는 사용자가 SSPR 및 인증에 사용된 **방법** (예: 휴대폰 번호 또는 질문 및 답변)을 등록할 때의 각 시간이 표시됩니다.
    암호 재설정 등록의 경우 SMS 게이트와 MFA 게이트 간의 차이는 없습니다. 둘 다 **휴대폰**으로 간주됩니다.

-   셀프 서비스 그룹 작업에는 누군가가 스스로를 그룹에 추가하거나 그룹에서 삭제하고 그룹을 만들려고 하는 각 시도가 표시됩니다.

    ![Azure 하이브리드 보고 - 암호 재설정 작업 이미지](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> 현재 보고서에는 최대 1개월 전까지의 데이터가 있습니다.</br>
> 이전 하이브리드 에이전트를 제거해야 합니다.</br>
> 하이브리드 보고서를 제거하려면 MIMreportingAgent.msi 에이전트를 제거합니다.

## <a name="prerequisites"></a>전제 조건

1.  Microsoft Identity Manager 2016 RTM 또는 SP1 MIM 서비스를 설치합니다.

2.  사용이 허가된 관리자 권한이 있는 디렉터리에 Azure AD Premium 테넌트가 있는지 확인합니다.

3.  Microsoft Identity Manager 서버에서 Azure로 나가는 인터넷 연결이 있는지 확인합니다.

## <a name="requirements"></a>요구 사항
다음 표는 Microsoft Identity Manager 하이브리드 보고를 사용하기 위한 요구 사항 목록입니다.

| 요구 사항 | 설명 |
| --- | --- |
| Azure AD Premium | 하이브리드 보고는 Azure AD Premium 기능으로, Azure AD Premium이 있어야 사용할 수 있습니다. </br></br>자세한 내용은 [Azure AD Premium 시작](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)을 참조하세요. </br>30일 평가판을 시작하려면 [평가판 시작](https://azure.microsoft.com/trial/get-started-active-directory/)을 참조하세요. |
| 시작하려면 Azure AD의 전역 관리자여야 함 |기본적으로 전역 관리자만 시작할 에이전트를 설치 및 구성하고, 포털에 액세스하고, Azure 내에서 작업을 수행할 수 있습니다. </br></br>**중요:** 에이전트를 설치할 때는 회사 또는 학교 계정을 사용해야 합니다. Microsoft 계정은 사용할 수 없습니다. 자세한 내용은 [조직으로 Azure 등록](https://docs.microsoft.com/azure/active-directory/sign-up-organization)을 참조하세요. |
| Microsoft Identity Manager 하이브리드 에이전트가 각 대상 MIM 서비스 서버에 설치되어 있어야 함 | 하이브리드 보고를 사용하려면 대상 서버에서 데이터를 수신하고 모니터링 및 분석 기능을 제공할 에이전트를 설치 및 구성해야 합니다. </br>|
| Azure 서비스 끝점에 대한 아웃바운드 연결 | 설치 중과 런타임에 에이전트는 Azure 서비스 끝점에 연결해야 합니다. 방화벽을 사용하여 아웃바운드 연결을 차단한 경우 다음 끝점을 허용 목록에 추가해야 합니다. </br></br><li>&#42;.blob.core.windows.net </li><li>&#42;.servicebus.windows.net - Port: 5671 </li><li>&#42;.adhybridhealth.azure.com/</li><li>https://management.azure.com </li><li>https://policykeyservice.dc.ad.msft.net/</li><li>https://login.windows.net</li><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li> |
|IP 주소 기반 아웃바운드 연결 | 방화벽의 IP 주소 기반 필터링에 대해서는 [Azure IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653)(Azure IP 범위)를 참조하세요.|
| 아웃바운드 트래픽에 대한 SSL 검사를 필터링하거나 사용하지 않도록 설정 | 네트워크 계층에서 아웃바운드 트래픽에 대한 SSL 검사 또는 종료가 설정되어 있으면 에이전트 등록 단계나 데이터 업로드 작업이 실패할 수 있습니다. |
| 에이전트를 실행하는 서버의 방화벽 포트 |다음과 같은 방화벽 포트를 열어 두어야만 에이전트가 Azure 서비스 끝점과 통신할 수 있습니다.</br></br><li>TCP 포트 443</li><li>TCP 포트 5671</li> |
| IE 보안 강화가 사용되는 경우 다음 웹 사이트 허용 |[IE 보안 강화]를 사용하는 경우 에이전트를 설치할 서버에서 다음 웹 사이트를 허용해야 합니다.</br></br><li>https://login.microsoftonline.com</li><li>https://secure.aadcdn.microsoftonline-p.com</li><li>https://login.windows.net</li><li>Azure Active Directory에서 신뢰하는 조직의 페더레이션 서버. 예: https://sts.contoso.com</li> |
</BR>

## <a name="install-microsoft-identity-manager-reporting-agent-in-azure-ad"></a>Azure AD에서 Microsoft Identity Manager 보고 에이전트 설치
보고 에이전트를 설치하면 MIM에서 Microsoft Identity Manager 작업의 데이터를 Windows 이벤트 로그로 내보냅니다. MIM 보고 에이전트는 이벤트를 처리하여 Azure에 업로드합니다. Azure에서 필요한 보고서를 위해 이벤트가 구문 분석, 암호 해독 및 필터링됩니다.

1.  Microsoft Identity Manager 2016을 설치합니다.

2.  Microsoft Identity Manager 보고 에이전트를 다운로드합니다.

    1.  Azure AD 관리 포털에 로그인하고 Active Directory 아이콘을 클릭합니다.

    2.  사용자가 전역 관리자이고 사용자에게 Azure AD Premium 구독이 있는 디렉터리를 두 번 클릭합니다.

    3.  **구성** 을 클릭하고 보고 에이전트를 다운로드합니다.

3.  Microsoft Identity Manager 보고 에이전트를 설치합니다.

    1.  [MIMHReportingAgentSetup.exe](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe)를 Microsoft Identity Manager 서비스 서버로 다운로드합니다.
    2.  `MIMHReportingAgentSetup.exe`을 실행합니다. 
    3.  에이전트 설치 관리자를 실행합니다.

    4.  MIM 보고 에이전트 서비스가 실행되고 있는지 확인합니다.

    5.  MIM 서비스를 다시 시작합니다.

4.  Microsoft Identity Manager 보고가 Azure에서 작동하는지 유효성을 검사합니다.

    사용자의 암호를 다시 설정하는 Microsoft Identity Manager 셀프 서비스 암호 다시 설정 포털을 사용하여 보고서 데이터를 만들 수 있습니다. 암호 재설정이 성공적으로 완료되었는지 확인한 다음 데이터가 Azure AD 관리 포털에 표시되는지 확인합니다.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Azure Portal에서 하이브리드 보고서 보기

1.  테넌트에 대한 전역 관리자 계정을 사용하여 [Azure Portal](https://portal.azure.com/)에 로그인합니다.

2.  **Azure Active Directory** 아이콘을 클릭합니다.

3.  구독에 대해 사용 가능한 디렉터리 목록에서 테넌트 디렉터리를 선택합니다.

4.  **감사 로그**를 클릭합니다.

5.  [범주] 드롭다운 메뉴에서 **MIM 서비스**를 선택해야 합니다.

> [!WARNING]
> Microsoft Identity Manager 감사 데이터를 Azure Portal에 표시하려면 약간의 시간이 걸릴 수 있습니다.

## <a name="stop-creating-hybrid-reports"></a>하이브리드 보고서 만들기 중지
Microsoft Identity Manager에서 Azure Active Directory로 보고 감사 데이터 업로드를 중지하려면 하이브리드 보고 에이전트를 제거합니다. Windows **프로그램 추가/제거** 도구를 사용하여 Microsoft Identity Manager 하이브리드 보고를 제거합니다.

## <a name="windows-events-used-for-hybrid-reporting"></a>하이브리드 보고에 사용되는 Windows 이벤트
Microsoft Identity Manager가 생성한 이벤트는 Windows 이벤트 로그에 기록되고 이벤트 뷰어의 응용 프로그램 및 서비스 로그-&gt; **Identity Manager 요청 로그** 아래에 표시됩니다. 각 MIM 청은 Windows 이벤트 로그의 이벤트(JSON 구조 형식)로 내보내집니다. SIEM으로 내보낼 수 있습니다.

|이벤트 유형|ID|이벤트 세부 정보|
|--------------|------|-----------------|
|정보 산업|4121|모든 요청 데이터를 포함하는 MIM 이벤트 데이터입니다.|
|정보 산업|4137|단일 이벤트에 대한 데이터가 너무 많은 경우에 MIM 4121 이벤트의 확장입니다. 이 이벤트 헤더의 형식은 다음과 같습니다. `"Request: <GUID> , message <xxx> out of <xxx>`|
