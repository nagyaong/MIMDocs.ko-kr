---
title: PAM 환경 계층 모델 | Microsoft 문서
description: 위험에 대한 취약성을 기준으로 시스템을 분리하는 계층 모델에 대해 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 08/30/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: c6e3cd02-1e32-4194-a8ed-3a0b3d022a43
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 8e7b7217714f0ef74c1d959eb51dac07018d6e77
ms.sourcegitcommit: 44a2293ff17c50381a59053303311d7db8b25249
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/31/2018
ms.locfileid: "50379726"
---
# <a name="tier-model-for-partitioning-administrative-privileges"></a>관리자 권한 분할에 대한 계층 모델

이 문서에서는 위험도 높은 영역에서 높은 권한 활동을 분리하여 권한 상승을 방지하도록 고안된 보안 모델을 설명합니다. 이 모델은 모범 사례 및 보안 원칙을 따르면서 효율적인 사용자 환경을 제공합니다.

## <a name="elevation-of-privilege-in-active-directory-forests"></a>Active Directory 포리스트에서의 권한 상승

사용자, 서비스 또는 애플리케이션 계정이 Windows Server AD(Active Directory) 포리스트에 대해 영구 관리자 권한을 부여받으면 조직의 업무 및 비즈니스의 위험 요소가 크게 증가합니다. 이러한 계정은 종종 공격자의 목표가 되며, 손상된 경우 공격자는 도메인에 있는 다른 서버 또는 애플리케이션에 연결할 권한을 갖습니다.

계층 모델은 관리하는 리소스를 기반으로 관리자 간에 구역을 만듭니다. 사용자 워크스테이션에 대해 제어 권한이 있는 관리자는 애플리케이션을 제어하거나 엔터프라이즈 ID를 관리하는 관리자와 구분됩니다. [권한 있는 액세스 보안 참조 자료](http://aka.ms/tiermodel)에서 이 모델에 대해 알아봅니다.

## <a name="restricting-credential-exposure-with-logon-restrictions"></a>로그온 제한으로 자격 증명 노출 제한

일반적으로 관리자 계정에 대한 자격 증명 도난 위험을 줄이려면 관리 방식을 개선하여 공격자에 대한 노출을 제한해야 합니다. 첫 번째 단계로, 조직은 다음을 수행하는 것이 좋습니다.

- 관리자 자격 증명이 노출되는 호스트의 수를 제한합니다.
- 역할 권한을 필요한 최소 수준으로 제한합니다.
- 표준 사용자 활동(예: 메일 및 웹 검색)에 사용되는 호스트에서 관리 작업을 수행되지 않도록 합니다.

다음 단계는 로그온 제한을 구현하고 계층 모델 요구 사항에 맞게 프로세스 및 방식을 설정하는 것입니다. 이상적으로는 자격 증명 노출 또한 각 계층 내의 역할에 필요한 최소 권한 수준으로 줄여야 합니다.

높은 권한이 있는 계정은 덜 안전한 리소스에 액세스할 수 없도록 로그온 제한을 적용해야 합니다. 예를 들면 다음과 같습니다.

- 도메인 관리자(계층 0)는 엔터프라이즈 서버(계층 1) 및 표준 사용자 워크스테이션(계층 2)에 로그온할 수 없습니다.
- 서버 관리자(계층 1)는 표준 사용자 워크스테이션(계층 2)에 로그온할 수 없습니다.

>[!NOTE]
> 서버 관리자는 도메인 관리자 그룹에 속하지 않아야 합니다. 도메인 컨트롤러와 엔터프라이즈 서버를 둘 다 관리하는 직원에게는 별도 계정을 제공해야 합니다.

다음을 사용하여 로그온 제한을 적용할 수 있습니다.

- 그룹 정책 로그온 권한 제한은 다음과 같습니다.
    - 네트워크에서 이 컴퓨터 액세스 거부
    - 일괄 작업으로 로그온 거부
    - 서비스로 로그온 거부
    - 로컬 로그온 거부
    - 원격 데스크톱 설정을 통한 로그온 거부  
- 인증 정책 및 사일로(Windows Server 2012 이상을 사용하는 경우)
- 계정이 전용 관리자 포리스트에 있는 경우 선택적 인증

## <a name="next-steps"></a>다음 단계

- 다음 문서 [배스천 환경 계획](planning-bastion-environment.md)에서는 Microsoft Identity Manager에 대한 전용 관리 포리스트를 추가하여 관리자 계정을 설정하는 방법을 설명합니다.
- [Privileged Access Workstation](https://docs.microsoft.com/windows-server/identity/securing-privileged-access/privileged-access-workstations)은 인터넷 공격 및 위협 벡터로부터 보호되는 중요한 작업에 대한 전용 운영 체제를 제공합니다.
