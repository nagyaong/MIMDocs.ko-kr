---
title: PAM 환경 개요 | Microsoft 문서
description: Privileged Access Management를 배포하기 위해 필요한 가상 컴퓨터의 수와 구성 확인
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/31/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 479db14c-1bfb-4d7c-a344-cd718a01f328
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6f4b6e224b6b50bf2190688a994f35159d273713
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379501"
---
# <a name="environment-overview"></a>환경 개요

Privileged Access Management는 공유 네트워크에서 서로 연결된 별도의 드라이브가 있는 VM(가상 컴퓨터)에서 작동합니다. 이러한 가상 컴퓨터는 Windows 8.1, Windows Server 2012 R2 또는 다른 운영 체제 플랫폼에서 호스트할 수 있습니다.

![PAM 서버: 관계 및 지원되는 플랫폼 - 다이어그램](media/pam-test-lab-architecture.png)

3개 이상의 가상 컴퓨터가 필요합니다.  관리할 PAM에 대한 AD 도메인이 아직 없는 경우 CORP 도메인 컨트롤러 역할을 할 하나의 추가 VM이 필요합니다.  고가용성을 위한 PRIV 소프트웨어를 구성하려면 두 개의 추가 VM이 필요합니다.

VM 디스크 이미지를 저장할 드라이브에는 120GB 이상의 여유 디스크 공간이 필요합니다.  고가용성을 위해 배포하려는 경우 디스크 하위 시스템이 SQL 공유 스토리지에 대한 요구 사항을 충족해야 합니다.  공유 스토리지는 Windows Server 장애 조치(failover) 클러스터링 클러스터 디스크, SAN(저장 영역 네트워크)의 디스크 또는 SMB 서버의 파일 공유 형식일 수 있습니다.

> [!IMPORTANT]
> 스토리지는 배스천 환경에만 사용해야 합니다. 배스천 환경 외부의 다른 워크로드와 스토리지를 공유하는 것은 배스천 환경의 무결성을 위협할 수 있으므로 권장되지 않습니다.

## <a name="next-steps"></a>다음 단계

- [Active Directory Domain Services에 대한 Privileged Access Management](privileged-identity-management-for-active-directory-domain-services.md)는 PAM 및 작동 방법의 개요입니다.
- [PAM의 구성 요소 이해](principles-of-operation.md)는 PAM의 다양한 구성 요소에 대한 개요입니다.
