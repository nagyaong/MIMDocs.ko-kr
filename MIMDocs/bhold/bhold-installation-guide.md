---
title: BHOLD SP1 설치 | Microsoft Docs
description: BHOLD SP1 설치 설명서
keywords: ''
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: ''
ms.openlocfilehash: 11cde4e3b2779f9c32d9849a47713acf5f120b3c
ms.sourcegitcommit: 35f2989dc007336422c58a6a94e304fa84d1bcb6
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36289699"
---
# <a name="microsoft-bhold-suite-sp1-60-installation-guide"></a>Microsoft BHOLD Suite SP1(6.0) 설치 가이드

Microsoft® BHOLD Suite SP1(Service Pack 1)은 응용 프로그램의 컬렉션이며 Microsoft Identity Manager 2016 SP1(MIM)에서 사용하는 경우 MIM에 대한 실제 역할 관리, 분석 및 증명을 추가합니다. Microsoft BHOLD Suite SP1은 다음 모듈로 구성됩니다.

- BHOLD Core
- 액세스 관리 커넥터
- BHOLD FIM/MIM 통합
- BHOLD 모델 생성기
- BHOLD Analytics
- BHOLD Reporting
- BHOLD Attestation


> [!NOTE]
> **적용 대상**: Microsoft Identity Manager 2016 SP1

## <a name="what-this-document-covers"></a>이 문서의 내용

이 문서에서는 비즈니스 요구를 충족하고 각 BHOLD 모듈을 설치하는 BHOLD 배포를 계획하는 방법을 설명합니다. 각 모듈, 관련 하드웨어, 인프라와 소프트웨어 요구 사항, 사전 설치 네트워크 구성, 설치 중 필요한 정보 및 사후 설치 단계가 있는 경우는 자세히 설명합니다.

## <a name="pre-requisite-knowledge"></a>필수 정보

이 문서에서는 서버 컴퓨터에 소프트웨어를 설치하는 방법을 기본적으로 이해한다고 가정합니다. 또한 Active Directory® Domain Services, Microsoft Identity Manager SP1(FIM) 및 Microsoft SQL Server 2008 데이터베이스 소프트웨어에 대한 기본 지식이 있다고 가정합니다. AD DS 및 FIM 등의 종속 기술을 설정하고 구성하는 방법은 이 설명서에서 다루지 않습니다. Microsoft BHOLD 모듈이 수행하는 함수에 대한 정보는 [Microsoft BHOLD Suite 개념 가이드](https://technet.microsoft.com/library/jj134102(v=ws.10).aspx)를 참조하세요.

## <a name="audience"></a>대상 그룹

이 문서는 IT 계획자, 시스템 설계자, 기술 의사 결정자, 컨설턴트, 인프라 계획자 및 Microsoft BHOLD Suite를 배포하려는 IT 담당자를 대상으로 합니다.

## <a name="bhold-infrastructure-considerations"></a>BHOLD 인프라 고려 사항

대부분의 경우 BHOLD 및 FIM은 대규모 인프라가 환경에서 사용됩니다. 특정 비즈니스 요구 사항에 맞게 FIM 및 BHOLD 아키텍처를 조정할 수 있습니다. 다음 섹션에서는 몇 가지 가능한 아키텍처 솔루션을 제공합니다. 이 개요의 모든 가능한 옵션의 포괄적인 목록이지만 네트워크에서 BHOLD를 배포할 수 있는 방법을 제안합니다.
 
이 섹션에서는 다음 항목에 대한 내용을 다룹니다.

- 단일 서버 아키텍처
- 이중 서버 아키텍처
- 2계층 아키텍처
- SQL Server 권장 사항

### <a name="single-server-architecture"></a>단일 서버 아키텍처

소규모 조직에서 배포 또는 개발 목적의 경우 다음 그림에 나와 있는 대로 SQL Server 및 AD DS와 같은 서버에서 FIM 및 BHOLD를 설치할 수 있습니다.
 
![단일 서버 아키텍처](media/bhold-installation-guide/single.png)

BHOLD Suite SP1과 FIM 포털이 단일 서버에 함께 설치되면 BHOLD 및 FIM용 DNS에서 다른 호스트 별칭(CNAME 또는 A 레코드)을 만들어야 합니다. 따라서 별도의 SPN(서비스 사용자 이름)을 BHOLD 및 FIM 서비스에 생성할 수 있습니다. 자세한 내용은 [BHOLD Core 설치](https://technet.microsoft.com/library/jj134095(v=ws.10).aspx)를 참조하세요.
단일 서버 구성에서 FIM을 설치하는 방법에 대한 지침은 Microsoft TechNet 라이브러리에서 [시작 가이드에 대한 일반적인 구성](https://technet.microsoft.com/library/ff575965.aspx)을 참조하세요.

### <a name="dual-server-architecture"></a>이중 서버 아키텍처

별도 서버에 BHOLD Core 및 FIM을 설치하면 다중 계층 아키텍처에서 제공하는 보다 복잡한 배포가 필요하지 않은 중간 규모 조직에 대한 성능 및 유연성이 향상됩니다. 다음 그림에서는 고유한 서버에 설치된 BHOLD 및 FIM을 보여줍니다. FIM 서버도 BHOLD 및 FIM에 데이터베이스 서비스를 제공하는 SQL Server를 실행하고 있습니다. FIM 서버에서 실행되는 FIM 동기화 서비스는 FIM 및 BHOLD 데이터베이스 간에 변경 내용을 동기화합니다. 최종 사용자 셀프 서비스가 필요한 경우 BHOLD FIM 통합 모듈은 FIM 서비스 및 FIM 포털과 동일한 서버에 설치되어야 합니다. BHOLD FIM 통합 모듈의 경우 FIM 서비스 및 BHOLD FIM 통합 모듈이 동일한 서버에 설치되어 있어야 합니다.

![이중 서버 아키텍처](media/bhold-installation-guide/dual.png)

> [!IMPORTANT]
> BHOLD FIM 통합 모듈의 보고 기능은 BHOLD 및 FIM 데이터베이스가 동일한 SQL Server 인스턴스에 설치되어 있어야 하고 BHOLD 서비스 계정에는 FIM 서비스 데이터베이스에 대한 액세스 권한이 있어야 합니다.

### <a name="two-tier-architecture"></a>2계층 아키텍처

특히 이 성능이 중요한 대부분의 환경에서 BHOLD Suite SP1, FIM, 및r 별도 서버에 SQL Server(2계층 아키텍처)를 실행해야 합니다. 2계층 아키텍처를 통해 메모리 및 CPU 리소스를 각 계층에 전용으로 사용합니다. 다음 그림에서는 2계층 아키텍처를 구성하는 한 가지 방법을 보여줍니다. FIM 서버에서 실행되는 FIM 동기화 서비스는 FIM 및 BHOLD 데이터베이스 간에 변경 내용을 동기화합니다. 최종 사용자 셀프 서비스가 필요한 경우 BHOLD FIM 통합 모듈은 FIM 서비스 및 포털과 동일한 서버에 설치되어야 합니다.

![2계층 아키텍처](media/bhold-installation-guide/two-tier.png)

### <a name="sql-server-recommendations"></a>SQL Server 권장 사항

대규모 조직에서 BHOLD를 배포하는 경우 Microsoft SQL Server 데이터베이스를 설정하기 위해 다음 지침을 따르는 것이 좋습니다.

- FIM 또는 BHOLD 서비스의 별도 서버에 SQL Server를 배포합니다.
- 실제 디스크 수준에서 데이터 파일로부터 로그 파일을 격리합니다.
- 저장소 중복성을 제공하기 위해 RAID를 사용하는 경우 RAID 수준 10(1+0)을 사용합니다. RAID 수준 5를 사용하지 마십시오.
- SQL Server를 실행하는 서버에 2GB 이상의 실제 메모리를 사용하는 경우 올바른 설정을 구성해야 합니다.
- 최적의 BHOLD 성능을 위해 Microsoft SQL Server 2008 R2 이상을 사용합니다.

SQL Server 모범 사례에 대한 자세한 내용은 Microsoft TechNet 라이브러리에서 [저장소 관련 상위 10가지 모범 사례](https://www.microsoft.com/technet/prodtechnol/sql/bestpractice/storage-top-10.mspx)를 참조하세요.

### <a name="trusted-certificates-list-update"></a>신뢰할 수 있는 인증서 목록 업데이트

서비스를 시작하기 전에 인증서 체인의 유효성을 검사하도록 Windows를 구성할 수 있습니다. 이러한 시스템에서 서비스가 서버의 실행 파일 코드가 서버의 TCL(신뢰할 수 있는 인증서 목록)에 없는 인증서로 서명된 경우 서비스를 시작할 수 없습니다. Microsoft BHOLD Suite SP1 소프트웨어는 Microsoft 루트 인증 기관 2010 인증서로 시작하는 코드 서명 인증서 체인을 사용하여 서명된 코드입니다.
Microsoft에서 인터넷 연결을 통해 루트 인증서를 검색하도록 Windows를 구성할 수 있습니다. 그러나 연결이 끊긴 시스템에서 Windows Server는 Windows가 출시되기 전 시점에 루트 프로그램에서 표시된 인증서만을 포함합니다. 이러한 인증서는 Windows Server 2010 이전 버전 Windows Server 릴리스에서 BHOLD Suite SP1 코드 서명 인증서 체인의 유효성을 검사하는 데 필요한 루트 인증서를 포함하지 않습니다. 최신 TCL이 설치되지 않은 시스템에서 하나 이상의 Microsoft BHOLD Suite SP1 모듈을 설치하려는 경우 BHOLD Suite SP1을 설치하기 전에 루트 업데이트 패키지를 다운로드하고 설치하거나 그룹 정책을 사용하여 루트 업데이트 패키지를 설치해야 합니다. 자세한 내용은 [Windows 루트 인증서 프로그램 멤버](http://support.microsoft.com/kb/931125)를 참조하세요.

### <a name="installing-bhold-suite-sp1-on-windows-server-20122016-required-step"></a>Windows Server 2012/2016에 BHOLD Suite SP1 설치 필수 단계 

![IIS 설치 BHOLD](media/bhold-installation-guide/iis-install-bhold.png)

Windows Server 2012 또는 2016에서 BHOLD Suite SP1을 설치하는 경우 ```C:\Windows\System32\inetsrv\config```에서 applicationHost.config 파일을 수정할 때까지 BHOLD 웹 페이지를 사용할 수 없습니다. ```<globalModules>``` 섹션에서 다음과 같이 ```<add name="SPNativeRequestModule"```로 시작하는 항목에 ```preCondition="bitness64```를 추가합니다.

```<add name="SPNativeRequestModule" image="C:\Program Files\Common Files\Microsoft Shared\Web Server Extensions\15\isapi\spnativerequestmodule.dll" preCondition="bitness64"/>```

파일을 편집하고 저장한 후에 IIS 서버를 다시 설정하는 iisreset 명령을 실행합니다.


## <a name="upgrading-bhold-suite"></a>BHOLD Suite 업그레이드

기존 BHOLD Suite 설치를 업그레이드할 수 없습니다. 대신, BHOLD 모듈을 업데이트하기 전에 기존 BHOLD Suite 설치를 제거해야 합니다. 기존 BHOLD 역할 모델이 설치된 경우 BHOLD 데이터베이스를 업그레이드하고 업데이트된 BHOLD Core 모듈을 설치할 때 사용할 수 있습니다. 자세한 내용은 [BHOLD Suite SP1로 BHOLD Suite 바꾸기](https://technet.microsoft.com/library/jj874043(v=ws.10).aspx)를 참조하세요.


## <a name="next-steps"></a>다음 단계

- [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 버전 기록](../reference/version-bhold-history.md)
