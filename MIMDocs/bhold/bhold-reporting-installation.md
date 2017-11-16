---
title: "BHOLD Reporting 설치 | Microsoft Docs"
description: "BHOLD Reporting 모듈을 통해 역할 및 권한 부여 정책에 대한 보고서를 생성할 수 있습니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: aa6a263daadc4abdcad0eaaba554b6bc739fbd5f
ms.sourcegitcommit: 0d8b19c5d4bfd39d9c202a3d2f990144402ca79c
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 11/14/2017
---
# <a name="bhold-reporting-installation"></a>BHOLD Reporting 설치

BHOLD Reporting 모듈은 BHOLD에서 역할 및 다른 권한 부여 정책에 대한 보고서를 생성하는 기능을 제공합니다. 이러한 보고서는 규정 요구 사항에 대한 준수를 감사하거나 설명하는 데 유용합니다. 또한 이 모듈은 해당 역할의 멤버 자격을 분석하는 데 필요한 정보를 제공하여 조직 내에서 권한 부여를 관리하는 기능을 확장합니다. 보고서는 제한된 보기를 통해 보고서를 만드는 사용자가 확인하도록 허용된 정보만을 표시할 수 있습니다.

## <a name="bhold-reporting-installation-requirements"></a>BHOLD Reporting 설치 요구 사항

BHOLD Reporting 모듈을 설치하기 전에 BHOLD Reporting 모듈을 설치하려는 서버에서 BHOLD Core 모듈을 설치해야 합니다. BHOLD Core 모듈을 설치하는 방법에 대한 정보는 [BHOLD Core 설치](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)를 참조하세요.

>[!IMPORTANT]
BHOLD Reporting 및 BHOLD Attestation을 모두 설치하는 경우 BHOLD Attestation을 설치하기 전에 BHOLD Reporting을 설치해야 합니다.

## <a name="before-you-begin"></a>시작하기 전에

BHOLD Reporting 모듈 설치를 시작하기 전에 BHOLD Analytics 설치 마법사가 설치를 완료하는 데 필요한 Reporting 제공하도록 준비해야 합니다. 다음 워크시트를 사용하면 해당 정보를 기록하여 필요한 경우 제공할 수 있습니다.

| **항목**                                    | **설명**                                                                                                                                                                                                           | **값**                                                                                                                                                                                                                                                                                                            |
|---------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **도메인/컴퓨터에서 보안 공급자 사용** | 이 항목을 선택하면 Active Directory Domain Services 보안이 BHOLD Core에 대한 액세스 권한을 제어하도록 지정합니다.                                                                                                                | 해당 확인란을 선택합니다. </br>**중요:** 이 확인란을 선택하지 않으면 설치에 실패합니다.                                                                                                                                                                                                                   |
| **도메인**                                  | BHOLD Core를 설치할 때 만든 서비스 계정을 포함하는 도메인을 지정합니다. 자세한 내용은 [BHOLD Core 설치](https://technet.microsoft.com/en-us/library/jj134095(v=ws.10).aspx)를 참조하세요. | 도메인 이름은 마법사에 의해 자동으로 제공됩니다. 올바르지 않은 경우 이름을 변경합니다. **중요:** FQDN(정규화된 도메인 이름)이 아닌 NetBIOS(짧음) 이름을 사용하여 도메인 이름을 지정합니다. 예를 들어, 도메인의 FQDN이 fabrikam.com인 경우 도메인 이름을 FABRIKAM으로 지정합니다. |
| **사용자**                                    | BHOLD Core 서비스 사용자 계정의 로그 이름을 지정합니다.                                                                                                                                                          | 여기에 사용자 계정 이름을 입력합니다.                                                                                                                                                                                                                                                                                    |
| **암호**                                | 서비스 사용자 계정의 암호를 지정합니다.                                                                                                                                                                       | 암호를 여기에 입력합니다. </br>**중요:** 숨겨진 안전한 위치에 이 암호를 보관해야 합니다.                                                                                                                                                                                                                  |

## <a name="bhold-reporting-installation"></a>BHOLD Reporting 설치

BHOLD Reporting 모듈을 설치하려면 도메인 관리자 그룹의 구성원으로 로그온하고 다음 파일을 다운로드하고 BHOLD Reporting 모듈을 설치하려는 서버에서 관리자 권한으로 실행합니다.

- BholdReporting*\<Version\>*\_Release.msi

*\<Version\>*을 설치하는 BHOLD Reporting 릴리스의 버전 번호로 바꿉니다.

프로그램 파일을 관리자 권한으로 실행하려면 파일을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

## <a name="next-steps"></a>다음 단계

- [BHOLD 설치 가이드](bhold-installation-guide.md)
- [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 버전 기록](../reference/version-bhold-history.md)