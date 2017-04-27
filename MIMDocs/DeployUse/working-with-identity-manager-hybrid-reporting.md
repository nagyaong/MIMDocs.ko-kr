---
title: "MIM 2016을 사용하여 Azure에서 하이브리드 보고 작업 | Microsoft 문서"
description: "Azure에서 온-프레미스 데이터와 클라우드 데이터를 하이브리드 보고서에 결합하는 방법 및 이러한 보고서를 보고 관리하는 방법을 알아봅니다."
keywords: 
author: kgremban
ms.author: kgremban
manager: femila
ms.date: 01/27/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.reviewer: mwahl
ms.suite: ems
translationtype: Human Translation
ms.sourcegitcommit: 3623bffb099a83d0eba47ba25e9777c3d590e529
ms.openlocfilehash: 9e64f930a8fe8422c7f6c8d98e558961ae8b88f2
ms.lasthandoff: 01/24/2017


---

# <a name="working-with-identity-manager-hybrid-reporting"></a>ID 관리자 하이브리드 보고 작업

## <a name="available-hybrid-reports"></a>사용 가능한 하이브리드 보고서
Azure AD에서 사용할 수 있는 처음 세 가지 MIM(Microsoft Identity Manager) 보고서는 **암호 재설정 작업**, **암호 재설정 등록** 및 **셀프 서비스 그룹 작업**입니다.

-   암호 재설정 작업에는 사용자가 SSPR을 사용하여 암호를 재설정했고 인증에 사용된 **방법** 또는 게이트를 제공할 때의 각 인스턴스가 표시됩니다.

    ![Azure 하이브리드 보고 - 암호 재설정 작업 이미지](media/MIM-Hybrid-passwordreset.jpg)

-   암호 재설정 등록에는 사용자가 SSPR 및 인증에 사용된 **방법** (예: 휴대폰 번호 또는 질문 및 답변)을 등록할 때의 각 시간이 표시됩니다.
    암호 재설정 등록의 경우 SMS 게이트와 MFA 게이트 간의 차이는 없습니다. 둘 다 **휴대폰**으로 간주됩니다.

-   셀프 서비스 그룹 작업에는 누군가가 스스로를 그룹에 추가하거나 그룹에서 삭제하고 그룹을 만들려고 하는 각 시도가 표시됩니다.

> [!NOTE]
> 현재 보고서에는 최대 1개월 전까지의 데이터가 있습니다.
>
> 하이브리드 보고서를 제거하려면 MIMreportingAgent.msi 에이전트를 제거합니다.

## <a name="prerequisites"></a>필수 구성 요소

1.  MIM 서비스를 포함하여 Microsoft Identity Manager 2016을 설치합니다.

2.  사용이 허가된 관리자 권한이 있는 디렉터리에 Azure AD Premium 테넌트가 있는지 확인합니다.

3.  Microsoft Identity Manager 서버에서 Azure로 나가는 인터넷 연결이 있는지 확인합니다.

## <a name="install-microsoft-identity-manager-reporting-in-azure-ad"></a>Azure AD에서 Microsoft Identity Manager 보고 설치
보고 에이전트를 설치하면 MIM에서 Microsoft Identity Manager 작업의 데이터를 Windows 이벤트 로그로 내보냅니다. MIM 보고 에이전트는 이벤트를 처리하여 Azure에 업로드합니다. Azure에서 필요한 보고서를 위해 이벤트가 구문 분석, 암호 해독 및 필터링됩니다.

1.  Microsoft Identity Manager 2016을 설치합니다.

2.  Microsoft Identity Manager 보고 에이전트를 다운로드합니다.

    1.  Azure AD 관리 포털에 로그인하고 Active Directory 아이콘을 클릭합니다.

    2.  사용자가 전역 관리자이고 사용자에게 Azure AD Premium 구독이 있는 디렉터리를 두 번 클릭합니다.

    3.  **구성** 을 클릭하고 보고 에이전트를 다운로드합니다.

3.  Microsoft Identity Manager 보고 에이전트를 설치합니다.

    1.  컴퓨터에 디렉터리를 만듭니다.

    2.  디렉터리에 `MIMHybridReportingAgent.msi` 및 `tenant.cert` 파일을 추출합니다.

    3.  에이전트 설치 관리자를 실행합니다.

    4.  MIM 보고 에이전트 서비스가 실행되고 있는지 확인합니다.

    5.  MIM 서비스를 다시 시작합니다.

4.  Microsoft Identity Manager 보고가 Azure에서 작동하는지 유효성을 검사합니다.

    사용자의 암호를 다시 설정하는 Microsoft Identity Manager 셀프 서비스 암호 다시 설정 포털을 사용하여 보고서 데이터를 만들 수 있습니다. 암호 재설정이 성공적으로 완료되었는지 확인한 다음 데이터가 Azure AD 관리 포털에 표시되는지 확인합니다.

## <a name="view-hybrid-reports-in-the-azure-classic-portal"></a>Azure 클래식 포털에서 하이브리드 보고서 보기

1.  테넌트에 대한 전역 관리자 계정을 사용하여 [Azure 클래식 포털](https://manage.windowsazure.com/)에 로그인합니다.

2.  **Active Directory** 아이콘을 클릭합니다.

3.  구독에 대해 사용 가능한 디렉터리 목록에서 테넌트 디렉터리를 선택합니다.

4.  **보고서** 를 클릭한 다음 **Password Reset Activity**(암호 재설정 작업)를 클릭합니다.

5.  원본 드롭다운 메뉴에서 **ID 관리자** 를 선택합니다.

> [!WARNING]
> Microsoft Identity Manager 데이터를 Azure AD에 표시하려면 약간의 시간이 걸릴 수 있습니다.

## <a name="stop-creating-hybrid-reports"></a>하이브리드 보고서 만들기 중지
Microsoft Identity Manager에서 Azure Active Directory로 보고 데이터 업로드를 중지하려면 하이브리드 보고 에이전트를 제거합니다. Windows **프로그램 추가/제거** 도구를 사용하여 Microsoft Identity Manager 하이브리드 보고를 제거합니다.

## <a name="windows-events-used-for-hybrid-reporting"></a>하이브리드 보고에 사용되는 Windows 이벤트
Microsoft Identity Manager가 생성한 이벤트는 Windows 이벤트 로그에 기록되고 이벤트 뷰어의 응용 프로그램 및 서비스 로그-&gt; **Identity Manager 요청 로그** 아래에 표시됩니다. 각 MIM 청은 Windows 이벤트 로그의 이벤트(JSON 구조 형식)로 내보내집니다. SIEM으로 내보낼 수 있습니다.

|이벤트 유형|ID|이벤트 세부 정보|
|--------------|------|-----------------|
|정보 산업|4121|모든 요청 데이터를 포함하는 MIM 이벤트 데이터입니다.|
|정보|4137|단일 이벤트에 대한 데이터가 너무 많은 경우에 MIM 4121 이벤트의 확장입니다. 이 이벤트 헤더의 형식은 다음과 같습니다. `"Request: <GUID> , message <xxx> out of <xxx>`|

