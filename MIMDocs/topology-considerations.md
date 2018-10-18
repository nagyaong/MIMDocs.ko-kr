---
title: 배포에 대한 토폴로지 가이드 | Microsoft 문서
description: MIM 2016 구성 요소를 이해하고 이를 사용자 환경에 배포하는 방법에 대한 제안 사항을 알아봅니다.
keywords: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/12/2017
ms.topic: article
ms.prod: microsoft-identity-manager
ms.assetid: 735dc357-dfba-4f68-a5b3-d66d6c018803
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 42562e92b3fe0daa63110d33d8952a3a1fc3de17
ms.sourcegitcommit: 7de35aaca3a21192e4696fdfd57d4dac2a7b9f90
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/16/2018
ms.locfileid: "49358083"
---
# <a name="topology-considerations"></a>토폴로지 고려 사항
동일한 서버 또는 서로 다른 구성의 여러 서버에서 MIM(Microsoft Identity Manager) 구성 요소를 배포할 수 있습니다. 배포를 위한 선택한 토폴로지는 MIM에서 얻을 수 있는 성능에 영향을 줍니다. 이 문서에서는 구현을 고려할 수 있는 다양한 배포 토폴로지를 소개합니다.


> [!NOTE]
> 이러한 옵션은 ID 관리를 위해 MIM 동기화, MIM 서비스 및 MIM 포털만을 사용하는 배포에 적용할 수 있습니다.  MIM CM, MIM BHOLD Suite를 사용하거나 권한 있는 액세스 관리를 사용하는 배포에는 서로 다른 배포 옵션이 있습니다.


## <a name="mim-components"></a>MIM 구성 요소
배포 토폴로지를 설계할 때 각 구성 요소가 무엇이고 어떻게 상호 작용하는지 알아야 합니다.

- <a name="mim-portal---an-interface-for-password-resets-group-management-and-administrative-operations"></a>**MIM 포털** - 암호 재설정, 그룹 관리 및 관리 작업용 인터페이스입니다.
    -
- **MIM 서비스** - MIM 2016 ID 관리 기능을 구현하는 웹 서비스입니다.
- **MIM 동기화 서비스** - 다른 ID 시스템과 데이터를 동기화합니다.
- **Microsoft SQL Server** - MIM 서비스와 MIM 동기화 서비스 모두 해당 데이터를 SQL 데이터베이스에 저장합니다.

다음 표에는 각 MIM 구성 요소를 호스트하는 옵션이 나와 있습니다. MIM 구성 요소는 동일한 컴퓨터에서 호스트되거나 여러 서버 및 클러스터 간에 분산될 수 있습니다.

| | MIM 포털 | MIM 서비스 | MIM 동기화 서비스 | SQL  Server |
| --- | --- | --- | --- | --- |
| 동일한 컴퓨터 | 예 | 예 | 예 | 예 |
| 별도 서버 | 예 | 예 | 예 | 예 |
| 네트워크 부하 분산 클러스터 | 예 | 예 | | |
| 서버 클러스터 | | | | 예 |


## <a name="multitier-topology"></a>다중 계층 토폴로지
다중 계층 토폴로지는 가장 일반적으로 사용되는 토폴로지로서, 뛰어난 유연성을 제공합니다. MIM 포털, MIM 서비스 및 데이터베이스가 계층으로 분리되고 여러 컴퓨터에 배포됩니다. 이 토폴로지는 다양한 MIM 구성 요소의 크기 조정 시 유연성을 더해 줍니다. 예를 들어 NLB(네트워크 부하 분산) 클러스터에서 서버를 추가하여 MIM 포털을 수평 확장할 수 있습니다. 마찬가지로 NLB 클러스터를 사용하고 필요에 따라 클러스터의 컴퓨터(노드) 수를 늘려 MIM 서비스를 확장할 수 있습니다.

다중 계층 토폴로지에서는 각 SQL 데이터베이스를 호스트하는 전용 컴퓨터(MIM 서비스용 컴퓨터와 MIM 동기화 서비스용 컴퓨터)가 할당됩니다. 하드웨어를 추가하거나 업그레이드하여 SQL 데이터베이스를 호스트하는 컴퓨터의 성능 확장성을 증가시킬 수 있습니다. 예를 들어 CPU를 업그레이드하거나, CPU를 추가하거나, RAM(Random Access Memory)을 늘리거나, RAM을 업그레이드하거나, 하드 드라이브 구성을 업그레이드하여 읽기 및 쓰기 액세스를 늘리고 대기 시간을 단축할 수 있습니다.

![MIM 다중 계층 토폴로지 다이어그램](media/MIM-topo-multitier.png)

이 구성에서는 MIM 동기화 서비스 및 해당 데이터베이스가 동일한 컴퓨터에서 호스트됩니다. 그러나 MIM 동기화 서비스와 해당 데이터베이스가 별도 컴퓨터에서 호스트되는 경우 둘 간에 1기가비트의 전용 네트워크 연결이 있다면 유사한 성능을 얻을 수 있습니다.


## <a name="multitier-topology-with-multiple-mim-services"></a>여러 MIM 서비스를 사용하는 다중 계층 토폴로지
외부 시스템과의 데이터 동기화에는 시간이 오래 걸릴 수 있으며 해당 기간 동안 시스템에 상당한 부하가 추가될 수 있습니다. 동기화 구성으로 인해 워크플로 관련 정책이 트리거되는 경우 이러한 정책은 최종 사용자 워크플로와 리소스를 경합합니다. 이러한 문제는 실시간으로 수행되고 최종 사용자가 프로세스가 완료되기를 기다리는 암호 재설정과 같은 인증 워크플로에서 명확히 나타날 수 있습니다. 최종 사용자 작업용 MIM 서비스 인스턴스 하나와 관리 데이터 동기화용 별도 포털을 제공하면 최종 사용자 작업에 대한 향상된 응답성을 제공할 수 있습니다.

![MIM 다중 계층 토폴로지 다이어그램](media/MIM-topo-multitier-multiservice.png)

표준 다중 계층 토폴로지와 마찬가지로 NLB 클러스터를 사용하거나 필요에 따라 클러스터의 노드 수를 늘려 MIM 포털 성능을 개선할 수 있습니다.

MIM 동기화 서비스 및 MIM 서비스 데이터베이스를 호스트하는 SQL Server를 실행하는 컴퓨터는 MIM 배포의 전반적인 성능에 큰 영향을 미칩니다. 따라서 데이터베이스 성능을 최적화하기 위한 SQL Server 설명서의 권장 사항을 따릅니다. 자세한 내용은 다음 문서를 참조하세요.

## <a name="see-also"></a>참고 항목
- 다운로드 가능한 [FIM(Forefront Identity Manager) 2010 용량 계획 가이드](http://go.microsoft.com/fwlink/?LinkId=200180)에서는 테스트 빌드 및 성능 테스트 결과에 대해 자세히 알아봅니다.
