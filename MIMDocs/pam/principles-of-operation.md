---
title: PAM 구성 요소 이해 | Microsoft 문서
description: Privileged Access Management는 MIM과 일부 구성 요소를 공유하며 소수의 자체 구성 요소가 있습니다. 이러한 구성 요소가 함께 어떻게 작동되는지 알아봅니다.
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/13/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 6498f68f-36d3-448c-8fe6-649ad5a7f97d
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 328005c1f55b715c81d33a84ffe1e0e41c905379
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49334298"
---
# <a name="understand-the-components-of-pam"></a>PAM의 구성 요소 이해

Privileged Access Management는 관리 액세스를 일상적인 사용자 계정과 구분합니다. 이 솔루션은 병렬 포리스트를 사용합니다.

- *CORP*: 하나 이상의 도메인을 포함하는 범용 회사 포리스트입니다. 여러 CORP 포리스트가 있을 수 있지만 단순화를 위해 이 문서의 예제에서는 단일 도메인이 포함된 단일 포리스트를 가정합니다.  
- *PRIV*: 이 PAM 시나리오를 위해 특별히 만든 전용 포리스트입니다. 이 포리스트는 하나 이상의 CORP 도메인으로부터 숨겨진 권한 있는 그룹 및 계정을 수용하는 하나의 도메인을 포함합니다.

PAM에 대해 구성된 것처럼 MIM 솔루션은 다음 구성 요소를 포함합니다.  

- **MIM 서비스**: 권한 있는 계정 관리 및 권한 상승 요청 처리를 포함하여 ID 및 액세스 관리 작업을 수행하기 위한 비즈니스 논리를 구현합니다.
- **MIM 포털**: SharePoint 2013에서 호스트하는 SharePoint 기반 포털로, 관리자 관리 및 구성 UI를 제공합니다.
- **MIM 서비스 데이터베이스**: SQL Server 2012 또는 2014에 저장되며 MIM 서비스에 필요한 ID 데이터 및 메타데이터를 저장합니다.
- **PAM 모니터링 서비스** 및 **PAM 구성 요소 서비스**: 권한 있는 계정의 수명 주기를 관리하고 그룹 구성원 자격의 수명 주기에서 PRIV AD를 지원하는 두 개의 서비스입니다.
- **PowerShell cmdlet**: 관리자 계정에 대한 권한의 JIT(Just-In-Time) 사용을 요청하는 최종 사용자와 PAM 관리자를 위해 CORP 포리스트의 사용자 및 그룹에 해당하는 사용자 및 그룹으로 PRIV AD와 MIM 서비스를 채우는 데 사용됩니다.
- **PAM REST API 및 샘플 포털**: PowerShell 또는 SOAP를 사용할 필요 없이 권한 상승을 위해 사용자 지정 클라이언트와 PAM 시나리오의 MIM을 통합하는 개발자를 위한 것입니다. REST API의 사용은 샘플 웹 응용 프로그램에서 나타납니다.

설치되고 구성된 다음 PRIV 포리스트의 마이그레이션 절차에서 만들어진 각 그룹은 원본 CORP 포리스트의 SID 그룹을 미러링하는 섀도 SIDHistory 기반 보안 그룹(또는 이후의 Windows Server vNext 업데이트에서는 외부 주 그룹)입니다. 또한 MIM 서비스가 구성원을 PRIV 포리스트의 이러한 그룹에 추가하면 해당 구성원 자격은 시간이 제한됩니다.

그 결과 사용자가 PowerShell cmdlet을 사용하여 권한 상승을 요청하고 해당 요청이 승인되면 MIM 서비스가 PRIV 포리스트의 해당 계정을 PRIV 포리스트의 그룹에 추가합니다. 사용자가 권한 있는 계정으로 로그인할 때 해당 Kerberos 토큰에 CORP 포리스트에 있는 그룹의 SID와 동일한 SID(보안 식별자)가 포함됩니다. CORP 포리스트는 PRIV 포리스트를 트러스트하도록 구성되었으므로 CORP 포리스트의 리소스에 액세스하는 데 사용되는 권한이 높은 계정은, Kerberos 그룹 구성원 자격을 확인하는 리소스에 대해 해당 리소스의 보안 그룹의 구성원이 됩니다. 이는 Kerberos 크로스 포리스트 인증을 통해 제공됩니다.

또한 이러한 구성원 자격은 미리 구성된 시간 간격 이후에는 사용자의 관리 계정이 더 이상 PRIV 포리스트의 그룹에 속해 있지 않도록 시간이 제한되어 있습니다. 그 결과 추가 리소스에 액세스하기 위해 해당 계정을 더 이상 사용할 수 없습니다.
