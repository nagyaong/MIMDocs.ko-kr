---
title: BHOLD Attestation 설치 | Microsoft Docs
description: BHOLD Attestation 모듈을 사용하면 검토자를 지정하고 검토를 수행할 수 있습니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 6838355c05f7c19436d8a83839044ea5f4e2533d
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36290342"
---
# <a name="bhold-attestation-installation"></a>BHOLD Attestation 설치

BHOLD Attestation 모듈을 사용하면 검토자를 지정하고 사용자 간에 관계 및 응용 프로그램별 사용 권한과 계정의 되풀이 검토를 수행할 수 있습니다.

## <a name="bhold-attestation-installation-requirements"></a>BHOLD Attestation 설치 요구 사항

BHOLD Attestation 모듈을 설치하기 전에 BHOLD Attestation 모듈을 설치하려는 서버에서 BHOLD Core 모듈을 설치해야 합니다. BHOLD Core 모듈을 설치하는 방법에 대한 정보는 [BHOLD Core 설치](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)를 참조하세요. BHOLD Attestation 모듈 연락처가 사용자에게 전자 메일 메시지를 전송하기 때문에 환경에는 Microsoft Exchange Server와 같은 SMTP(Simple Mail Transfer Protocol) 전자 메일 서버가 있어야 합니다.

> [!IMPORTANT]
> BHOLD Reporting 및 BHOLD Attestation을 모두 설치하는 경우 BHOLD Attestation을 설치하기 전에 BHOLD Reporting을 설치해야 합니다.

## <a name="before-you-begin"></a>시작하기 전에

BHOLD Attestation 모듈 설치를 시작하기 전에 BHOLD Attestation 설치 마법사가 설치를 완료하는 데 필요한 정보를 제공하도록 준비해야 합니다. 다음 워크시트를 사용하면 해당 정보를 기록하여 필요한 경우 제공할 수 있습니다.

| **항**                                    | **설명**                                                                                                                                                                                                           | **값**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **도메인/컴퓨터에서 보안 공급자 사용** | 이 항목을 선택하면 Active Directory Domain Services 보안이 BHOLD Core에 대한 액세스 권한을 제어하도록 지정합니다.                                                                                                                | 해당 확인란을 선택합니다. **중요:** 이 확인란을 선택하지 않으면 설치에 실패합니다.                                                                                                                                                                                                                   |
| **도메인**                                  | BHOLD Core를 설치할 때 만든 서비스 계정을 포함하는 도메인을 지정합니다. 자세한 내용은 [BHOLD Core 설치](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)를 참조하세요. | 도메인 이름은 마법사에 의해 자동으로 제공됩니다. 올바르지 않은 경우 이름을 변경합니다. **중요:** FQDN(정규화된 도메인 이름)이 아닌 NetBIOS(짧음) 이름을 사용하여 도메인 이름을 지정합니다. 예를 들어, 도메인의 FQDN이 fabrikam.com인 경우 도메인 이름을 FABRIKAM으로 지정합니다. |
| **사용자**                                    | BHOLD Core 서비스 사용자 계정의 로그 이름을 지정합니다.                                                                                                                                                          | 여기에 사용자 계정 이름을 입력합니다.                                                                                                                                                                                                                                                                                    |
| **암호**                                | 서비스 사용자 계정의 암호를 지정합니다.                                                                                                                                                                       | 여기에 암호를 입력합니다. **중요:** 숨겨진 안전한 위치에 이 암호를 보관해야 합니다.                                                                                                                                                                                                                  |

## <a name="bhold-attestation-installation"></a>BHOLD Attestation 설치

BHOLD Attestation 모듈을 설치하려면 도메인 관리자 그룹의 구성원으로 로그온하고 다음 파일을 다운로드하고 BHOLD Attestation 모듈을 설치하려는 서버에서 관리자 권한으로 실행합니다.

- BholdAttestation<em>\<Version\></em>\_Release.msi

*\<Version\>* 을 설치하는 BHOLD Attestation 릴리스의 버전 번호로 바꿉니다.

프로그램 파일을 관리자 권한으로 실행하려면 파일을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

## <a name="next-steps"></a>다음 단계

- [BHOLD 설치 가이드](bhold-installation-guide.md)
- [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 버전 기록](../reference/version-bhold-history.md)