---
title: "BHOLD 액세스 관리 커넥터 설치 | Microsoft Docs"
description: "BHOLD 커넥터 모듈에서는 데이터의 초기 및 지속적인 동기화를 지원합니다."
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 09/07/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 6d7f19f470d0c0f82a68652115ab9265a13b3508
ms.sourcegitcommit: ed8dd5563e77ef4a3345b2a52a1426859c95576a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/15/2017
---
# <a name="access-management-connector-installation"></a>액세스 관리 커넥터 설치

BHOLD 제품군 액세스 관리 커넥터 모듈은 BHOLD에 대한 데이터의 초기 및 진행 중인 동기화를 모두 지원합니다. 액세스 관리 커넥터는 MIM(Microsoft Identity Manager) 동기화 서비스와 함께 작동하여 BHOLD Core 데이터베이스, FIM 2010 메타버스, 대상 응용 프로그램 및 ID 저장소 간에 데이터를 이동합니다. 액세스 관리 커넥터 모듈을 설치한 후에 BHOLD와 MIM 간의 데이터 흐름을 제어하는 FIM 관리 에이전트를 만들 수 있습니다.

## <a name="access-management-connector-software-requirements"></a>액세스 관리 커넥터 소프트웨어 요구 사항

액세스 관리 커넥터 모듈을 설치하기 전에 Microsoft .NET Framework 4를 설치해야 합니다. .NET Framework 4 및 설치 지침에 대한 자세한 내용은 [Microsoft .NET 홈페이지](http://www.microsoft.com/net)를 참조하세요.
MIM의 FIM 동기화 서비스를 실행하는 컴퓨터에 액세스 관리 커넥터를 설치해야 합니다.

## <a name="access-management-connector-setup"></a>액세스 관리 커넥터 설정

액세스 관리 제어 모듈을 설치하려면 도메인 관리자 그룹의 구성원으로 로그온하고 다음 파일을 다운로드하고 BHOLD FIM 통합 모듈을 설치하려는 서버에서 관리자 권한으로 실행합니다.

- AccessManagementConnector.msi

프로그램 파일을 관리자 권한으로 실행하려면 파일을 마우스 오른쪽 단추로 클릭하고 **관리자 권한으로 실행**을 클릭합니다.

## <a name="next-steps"></a>다음 단계

- [BHOLD FIM 통합 설치](https://technet.microsoft.com/library/jj134093(v=ws.10).aspx) 역할의 최종 사용자 셀프 서비스를 사용하기 위해 BHOLD FIM 통합 모듈을 설치할 수 있습니다
- [BHOLD 설치 가이드](bhold-installation-guide.md)
- [BHOLD 개발자 참조](../reference/mim2016-bhold-developer-reference.md)
- [BHOLD 버전 기록](../reference/version-bhold-history.md)