---
title: Identity Manager 2016을 사용하여 Azure에서 하이브리드 보고 작업 | Microsoft Docs
description: Azure에서 온-프레미스 데이터와 클라우드 데이터를 하이브리드 보고서에 결합하는 방법 및 이러한 보고서를 보고 관리하는 방법을 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 2/20/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 68df2817-2040-407d-b6d2-f46b9a9a3dbb
ms.suite: ems
ms.openlocfilehash: 18e4127b1d854a53734142bb58442627619491ef
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358536"
---
# <a name="work-with-hybrid-reporting-in-identity-manager"></a>Identiy Manager에서 하이브리드 보고 작업

이 문서에서는 Azure에서 온-프레미스 데이터와 클라우드 데이터를 하이브리드 보고서에 결합하는 방법 및 이러한 보고서를 관리하고 보는 방법을 설명합니다.

## <a name="available-hybrid-reports"></a>사용 가능한 하이브리드 보고서
Azure AD(Azure Active Directory)에서 사용할 수 있는 처음 세 개의 Microsoft Identity Manager 보고서는 다음과 같습니다.

- **암호 재설정 활동**: 사용자가 SSPR(셀프 서비스 암호 재설정)을 사용하여 암호 재설정을 수행한 경우의 각 인스턴스가 표시되고 인증에 사용된 게이트 또는 방법을 제공합니다.

- **암호 재설정 등록**: 사용자가 SSPR 및 인증하는 데 사용된 방법을 등록한 각 시간이 표시됩니다. 방법의 예로는 휴대폰 번호 또는 질문과 대답을 들 수 있습니다.
   > [!NOTE]
   > *암호 재설정 등록* 보고서의 경우 SMS 게이트와 MFA 게이트 간의 차이는 없습니다. 둘 다 휴대폰 방법으로 간주됩니다.

- **셀프 서비스 그룹 활동**: 누군가가 자신을 그룹 및 그룹 만들기에 추가하거나 그룹 및 그룹 만들기에서 삭제하려고 한 각 시도가 표시됩니다.

    ![Azure 하이브리드 보고 - 암호 재설정 작업 이미지](media/MIM-Hybrid-passwordreset2.jpg)

> [!NOTE]
> * 현재 보고서에는 최대 1개월간의 활동에 대한 데이터가 표시됩니다.
> * 이전 하이브리드 보고 에이전트를 제거해야 합니다.
> * 하이브리드 보고서를 제거하려면 MIMreportingAgent.msi 에이전트를 제거합니다.

## <a name="prerequisites"></a>전제 조건

* Identity Manager 2016 SP1 Identity Manager 서비스, 권장 빌드 [4.4.1749.0](https://support.microsoft.com/en-us/help/4050936/hotfix-rollup-package-build-4-4-1749-0-for-microsoft-identity-manager)

* 디렉터리의 사용 허가를 받은 관리자가 있는 Azure AD Premium 테넌트

* Identity Manager 서버에서 Azure로 나가는 인터넷 연결

## <a name="requirements"></a>요구 사항
Identity Manager 하이브리드 보고를 사용하기 위한 요구 사항은 다음 표에 나열되어 있습니다.


|                                         요구 사항                                         |                                                                                                                                                                                                                                                                                    설명                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                      Azure AD Premium                                       |                                                                                                        하이브리드 보고는 Azure AD Premium 기능으로, Azure AD Premium이 있어야 사용할 수 있습니다. </br>자세한 내용은 [Azure AD Premium 시작](https://docs.microsoft.com/azure/active-directory/active-directory-get-started-premium)을 참조하세요. </br>[Azure AD Premium 30일 평가판](https://azure.microsoft.com/trial/get-started-active-directory/)을 받으세요.                                                                                                         |
|                     Azure AD의 전역 관리자여야 함                     |                                                                   기본적으로 전역 관리자만 시작할 에이전트를 설치 및 구성하고, 포털에 액세스하고, Azure 내에서 작업을 수행할 수 있습니다. </br>**중요**: 에이전트를 설치할 때 사용하는 계정은 회사 또는 학교 계정이어야 합니다. Microsoft 계정은 사용할 수 없습니다. 자세한 내용은 [조직으로 Azure 등록](https://docs.microsoft.com/azure/active-directory/sign-up-organization)을 참조하세요.                                                                   |
| Identity Manager 하이브리드 에이전트가 각 대상 Identity Manager 서비스 서버에 설치되어 있어야 함 |                                                                                                                                                                                                       데이터를 수신하고 모니터링 및 분석 기능을 제공하려는 경우 하이브리드 보고를 사용하려면 대상 서버에서 에이전트를 설치 및 구성해야 합니다.  </br>                                                                                                                                                                                                       |
|                    Azure 서비스 엔드포인트에 대한 아웃바운드 연결                     | 설치 중과 런타임에 에이전트는 Azure 서비스 엔드포인트에 연결해야 합니다. 아웃바운드 연결이 방화벽에 의해 차단한 경우 다음 엔드포인트를 허용 목록에 추가해야 합니다.<ul><li>\*.blob.core.windows.net </li><li>\*.servicebus.windows.net - Port: 5671 </li><li>\*.adhybridhealth.azure.com/</li><li><https://management.azure.com> </li><li><https://policykeyservice.dc.ad.msft.net/></li><li><https://login.windows.net></li><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li></ul> |
|                         IP 주소 기반 아웃바운드 연결                         |                                                                                                                                                                                                                      방화벽의 IP 주소 기반 필터링에 대해서는 [Azure IP Ranges](https://www.microsoft.com/download/details.aspx?id=41653)(Azure IP 범위)를 참조하세요.                                                                                                                                                                                                                      |
|                 아웃바운드 트래픽에 대한 SSL 검사를 필터링하거나 사용하지 않도록 설정                 |                                                                                                                                                                                                               네트워크 계층에서 아웃바운드 트래픽에 대한 SSL 검사 또는 종료가 설정되어 있으면 에이전트 등록 단계나 데이터 업로드 작업이 실패할 수 있습니다.                                                                                                                                                                                                                |
|                      에이전트를 실행하는 서버의 방화벽 포트                       |                                                                                                                                                                                                          에이전트가 Azure 서비스 엔드포인트와 통신하려면 다음과 같은 방화벽 포트를 열어야 합니다.<ul><li>TCP 포트 443</li><li>TCP 포트 5671</li></ul>                                                                                                                                                                                                          |
|          Internet Explorer 보안 강화가 사용하도록 설정된 경우 특정 웹 사이트 허용           |                                                                                Internet Explorer 보안 강화가 사용하도록 설정된 경우 에이전트가 설치된 서버에서 다음 웹 사이트를 허용해야 합니다.<ul><li><https://login.microsoftonline.com></li><li><https://secure.aadcdn.microsoftonline-p.com></li><li><https://login.windows.net></li><li>Azure Active Directory에서 신뢰하는 조직의 페더레이션 서버(예: <https://sts.contoso.com>)</li></ul>                                                                                |

</BR>

## <a name="install-identity-manager-reporting-agent-in-azure-ad"></a>Azure AD에서 Identity Manager 보고 에이전트 설치
보고 에이전트가 설치된 후 Identity Manager에서 Identity Manager 활동의 데이터를 Windows 이벤트 로그로 내보냅니다. Identity Manager 보고 에이전트는 이벤트를 처리한 다음 Azure에 업로드합니다. Azure에서 필요한 보고서를 위해 이벤트가 구문 분석, 암호 해독 및 필터링됩니다.

1.  Identity Manager 2016을 설치합니다.

2.  Identity Manager 보고 에이전트를 다운로드한 후 다음을 수행합니다.

    a. Azure AD 관리 포털에 로그인한 다음 **Active Directory**를 선택합니다.

    b. 사용자가 전역 관리자이고 사용자에게 Azure AD Premium 구독이 있는 디렉터리를 두 번 클릭합니다.

    c. **구성**을 선택한 다음 보고 에이전트를 다운로드합니다.

3.  다음을 수행하여 보고 에이전트를 설치합니다.

    a.  Identity Manager 서비스 서버용 [MIMHReportingAgentSetup.exe 파일](http://download.microsoft.com/download/7/3/1/731D81E1-8C1D-4382-B8EB-E7E7367C0BF2/MIMHReportingAgentSetup.exe)을 다운로드합니다.

    b.  `MIMHReportingAgentSetup.exe`을 실행합니다. 

    c.  에이전트 설치 관리자를 실행합니다.

    d.  Identity Manager 보고 에이전트 서비스가 실행되고 있는지 확인합니다.

    e.  Identity Manager 서비스를 다시 시작합니다.

4.  Azure에서 Identity Manager 보고 에이전트가 작동하고 있는지 확인합니다.

    사용자의 암호를 재설정하는 Identity Manager 셀프 서비스 암호 재설정 포털을 사용하여 보고서 데이터를 만들 수 있습니다. 암호 재설정이 성공적으로 완료되었는지 확인한 다음 데이터가 Azure AD 관리 포털에 표시되는지 확인합니다.

## <a name="view-hybrid-reports-in-the-azure-portal"></a>Azure Portal에서 하이브리드 보고서 보기

1.  테넌트의 전역 관리자 계정으로 [Azure Portal](https://portal.azure.com/)에 로그인합니다.

2.  **Azure Active Directory**를 선택합니다.

3.  구독에 대해 사용 가능한 디렉터리 목록에서 테넌트 디렉터리를 선택합니다.

4.  **감사 로그**를 선택합니다.

5.  **범주** 드롭다운 목록에서 **MIM 서비스**가 선택되어 있는지 확인합니다.

> [!IMPORTANT]
> Azure Portal에 Identity Manager 감사 데이터가 나타날 때까지 약간의 시간이 걸릴 수 있습니다.

## <a name="stop-creating-hybrid-reports"></a>하이브리드 보고서 만들기 중지
Identity Manager에서 Azure AD로 보고 감사 데이터 업로드를 중지하려면 하이브리드 보고 에이전트를 제거합니다. Windows 프로그램 추가/제거 도구를 사용하여 Identity Manager 하이브리드 보고를 제거합니다.

## <a name="windows-events-used-for-hybrid-reporting"></a>하이브리드 보고에 사용되는 Windows 이벤트
Identity Manager에 의해 생성된 이벤트는 Windows 이벤트 로그에 저장됩니다. **응용 프로그램 및 서비스 로그** > **Identity Manager 요청 로그**를 선택하여 **이벤트 뷰어**에서 이벤트를 볼 수 있습니다. 각 Identity Manager 요청은 Windows 이벤트 로그의 이벤트(JSON 구조 형식)로 내보내집니다. 결과를 SIEM(보안 정보 및 이벤트 관리) 시스템으로 내보낼 수 있습니다.

|이벤트 유형|ID|이벤트 세부 정보|
|--------------|------|-----------------|
|정보 산업|4121|모든 요청 데이터를 포함하는 Identity Manager 이벤트 데이터입니다.|
|정보 산업|4137|Identity Manager 이벤트 4121 확장입니다(단일 이벤트에 대한 데이터가 너무 많은 경우). 이 이벤트의 헤더는 `"Request: <GUID> , message <xxx> out of <xxx>` 형식으로 표시됩니다.|
