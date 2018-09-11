---
title: Azure Multi-Factor Authentication 서버 SDK를 사용하여 PAM 또는 SSPR 시나리오 활성화 | Microsoft Docs
description: 사용자가 Privileged Access Management 및 셀프 서비스 암호 재설정에서 역할을 활성화할 때 Azure Multi-Factor Authentication 서버 SDK를 두 번째 보안 계층으로 설정합니다.
keywords: ''
author: fimguy
ms.author: billmath
manager: mtillman
ms.date: 09/02/2018
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 94a74f1c-2192-4748-9a25-62a526295338
ms.openlocfilehash: 7191e445688cc9e3c5c02b9c6852c869a28a937a
ms.sourcegitcommit: ad0690bd57e3d056397108bf1c8417965d69a32c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/05/2018
ms.locfileid: "43772681"
---
# <a name="use-azure-multi-factor-authentication-server-to-activate-pam-or-sspr"></a>Azure Multi-Factor Authentication 서버를 사용하여 PAM 또는 SSPR 시나리오 활성화
다음 문서에서는 사용자가 Privileged Access Management 및 셀프 서비스 암호 재설정에서 역할을 활성화할 때 Azure MFA 서버를 두 번째 보안 계층으로 설정하는 방법을 설명합니다.

> [!IMPORTANT]
> Azure Multi-Factor Authentication 소프트웨어 개발 키트의 사용 중단 알림 때문입니다. Azure MFA SDK는 기존 고객을 위해 사용 중지 날짜인 2018년 11월 14일까지 지원됩니다. 새 고객과 현재 고객은 더 이상 Azure 클래식 포털을 통해 SDK를 다운로드할 수 없게 됩니다. 다운로드하려면 Azure 고객 지원에 문의하여 생성된 MFA 서비스 자격 증명 패키지를 받아야 합니다. <br> Microsoft 개발 팀은 Multi-Factor Authentication 서버 SDK와 통합하여 MFA에 대한 변경을 진행하고 있습니다.

아래 문서에서는 릴리스 시 Azure MFA SDK에서 Azure Multi-Factor Authentication 서버 SDK로의 간단한 전환을 설정하는 단계와 구성 업데이트에 대해 간략히 설명합니다.이 내용은 향후 핫픽스에 포함될 예정이므로 공지 사항은 [버전 기록](/reference/version-history.md)을 참조하세요. 

## <a name="prerequisites"></a>전제 조건

Azure Multi-Factor Authentication 서버와 MIM을 함께 사용하려면 다음이 필요합니다.

- Azure MFA 서비스 연결에 사용되는, PAM 및 SSPR을 제공하는 각 MIM 서비스 또는 MFA 서버에서의 인터넷 액세스
- Azure 구독
- 설치에서 Azure MFA SDK를 이미 사용 중
- 후보 사용자의 Azure Active Directory Premium 라이선스 또는 Azure MFA 라이선스 방법
- 모든 후보 사용자의 전화 번호
- MIM 핫픽스 4.5 이상. 공지 사항은 [버전 기록](/reference/version-history.md) 참조

## <a name="azure-multi-factor-authentication-server-configuration"></a>Azure Multi-Factor Authentication 서버 구성 
> [!NOTE] 
> 구성에서 SDK에 대해 유효한 SSL 인증서가 설치되어 있어야 합니다. 

### <a name="step-1-download-azure-multi-factor-authentication-server-from-the-azure-portal"></a>1단계: Azure Portal에서 Azure Multi-Factor Authentication 서버 다운로드 
[Azure Portal](https://portal.azure.com/)에 로그인하여 Azure MFA 서버를 다운로드합니다.
![working-with-mfaserver-for-mim_downloadmfa](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_downloadmfa.PNG)

### <a name="step-2-generate-activation-credentials"></a>2단계: 활성화 자격 증명 생성
**사용을 시작할 활성화 자격 증명 생성** 링크를 사용하여 활성화 자격 증명을 생성합니다. 생성되었으면 나중에 사용하기 위해 저장합니다.

### <a name="step-3-install-the-azure-multi-factor-authentication-server"></a>3단계: Azure Multi-Factor Authentication 서버 설치
서버를 다운로드한 후 [설치](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy#install-and-configure-the-mfa-server)합니다.  활성화 자격 증명이 필요합니다. 

### <a name="step-4-create-your-iis-web-application-that-will-host-the-sdk"></a>4단계: SDK를 호스트하는 IIS 웹 응용 프로그램 만들기
1. IIS 관리자를 엽니다. ![working-with-mfaserver-for-mim_iis.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iis.PNG)
2.  새 웹 사이트 호출 “MIM MFASDK”를 만들어 빈 디렉터리에 연결합니다. ![working-with-mfaserver-for-mim_sdkweb.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkweb.PNG)
3. Multi-Factor Authentication 콘솔을 열고 웹 서비스 SDK를 클릭합니다. ![working-with-mfaserver-for-mim_sdkinstall.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_sdkinstall.PNG)
4. 마법사에서 클릭하여 구성에 도달하면 “MIM MFASDK” 및 앱 풀을 선택합니다.

> [!NOTE] 
> 마법사에서는 관리 그룹을 만들어야 합니다. 자세한 내용은 Azure > > MFA Azure Multi-Factor Authentication 서버 설명서에서 확인할 수 있습니다.

5. 다음으로 MIM 서비스 계정을 가져와야 합니다. Multi-Factor Authentication 콘솔을 열고 [사용자]를 선택합니다. a. [Active Directory에서 가져오기]를 클릭합니다. b. 서비스 계정 즉, “contoso\mimservice”로 이동합니다. c. [가져오기] 및 [닫기]를 클릭합니다. ![working-with-mfaserver-for-mim_importmimserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_importmimserviceaccount.PNG) 
6. Multi-Factor Authentication 콘솔에서 사용하도록 MIM 서비스 계정을 편집합니다. ![working-with-mfaserver-for-mim_enableserviceaccount.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_enableserviceaccount.PNG)
7. “MIM MFASDK” 웹 사이트에서 IIS 인증을 업데이트합니다. 먼저, [익명 인증]을 사용하지 않도록 설정한 다음, [Windows 인증]을 사용하도록 설정합니다. ![working-with-mfaserver-for-mim_iisconfig.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_iisconfig.PNG)
8. 최종 단계: “PhoneFactor Admins”에 MIM 서비스 계정을 추가합니다. ![working-with-mfaserver-for-mim_addservicetomfaadmin.PNG](media/working-with-mfaserver-for-mim/working-with-mfaserver-for-mim_addservicetomfaadmin.PNG)

## <a name="configuring-the-mim-service-for-azure-multi-factor-authentication-server"></a>Azure Multi-Factor Authentication 서버에 대한 MIM 서비스 구성 

### <a name="step-1-patch-server-to-452020"></a>1단계: 4.5.202.0로 서버 패치
 
### <a name="step-2-backup-and-open-the-mfasettingsxml-located-in-the-cprogram-filesmicrosoft-forefront-identity-manager2010service"></a>2단계: “C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”에 있는 MfaSettings.xml을 백업하여 엽니다.

### <a name="step-3-update-the-following-lines"></a>3단계: 다음 줄을 업데이트합니다.
1. 다음 구성 항목 줄을 제거하거나 지웁니다. <br>
<LICENSE_KEY></LICENSE_KEY><br>
<GROUP_KEY></GROUP_KEY><br>
<CERT_PASSWORD></CERT_PASSWORD><br>
<CertFilePath></CertFilePath><br>

2. 다음 줄을 업데이트하거나 MfaSettings.xml에 추가합니다. <br>
`<Username>mimservice@contoso.com</Username>` <br>
`<LOCMFA>true</LOCMFA>`<br>
`<LOCMFASRV>https://CORPSERVICE.contoso.com:9999/MultiFactorAuthWebServiceSdk/PfWsSdk.asmx</LOCMFASRV>`

3. MIM 서비스를 다시 시작하고 Azure Multi-Factor Authentication 서버로 기능을 테스트합니다.

> [!NOTE] 
> 설정을 되돌리려면 MfaSettings.xml을 2단계의 백업 파일로 바꿉니다.


## <a name="next-steps"></a>다음 단계

-    [Azure Multi-Factor Authentication 서버 시작](https://docs.microsoft.com/en-us/azure/active-directory/authentication/howto-mfaserver-deploy)
- [Azure Multi-Factor Authentication이란](https://docs.microsoft.com/azure/multi-factor-authentication/multi-factor-authentication)
- [사용자 지정 Multi-Factor Authentication API를 사용하여 PAM 또는 SSPR 활성화](Working-with-custommfaserver-for-mim.md)
- [MIM 버전 릴리스 기록](./reference/version-history.md)