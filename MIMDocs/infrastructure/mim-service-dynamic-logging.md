---
title: "MIM 서비스 동적 로깅 | Microsoft 문서"
description: "관리 서비스를 다시 시작하지 않고 MIM 서비스 동적 로깅 사용"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 03/24/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 1ff73d0bdfcbcb4ab79d0d81feca9abdc33f9213
ms.openlocfilehash: 1e2fb9a9ae508ab601ebad1dec7acc21dc44d13e
ms.contentlocale: ko-kr
ms.lasthandoff: 07/10/2017



---
<a id="mim-sp1-4414360--service-dynamic-logging" class="xliff"></a>
# MIM SP1(4.4.1436.0) 서비스 동적 로깅
4.4.1436.0에서는 새로운 로깅 기능을 도입했습니다. 이 기능을 사용하면 관리자와 지원 엔지니어가 관리 서비스를 다시 시작하지 않고도 로깅을 설정할 수 있습니다.

설치하면 호출되는 Microsoft.ResourceManagement.Service.exe.config에서 다음과 같은 새 줄이 표시됩니다.

*   줄 6: ``<section name="dynamicLogging" type="Microsoft.ResourceManagement.Utilities.DynamicLoggingSection, Microsoft.ResourceManagement.Service" />``
*   줄 8:  ``<dynamicLogging mode="true" loggingLevel="Verbose" />``
*   줄 266 ``</system.diagnostics> ``

![새 동적 로깅 항목을 보여 주는 강조 표시된 섹션](media/mim-service-dynamic-logging/screen01.png)

동적에 대한 로깅 수준은 [여기](https://msdn.microsoft.com/library/ms733025(v=vs.110).aspx#Anchor_3)에서 확인할 수 있습니다.

- Critical = 기본 수준 서비스에서 중요한 이벤트만 기록합니다.
- 줄 8(dynamicLogging mode="true" loggingLevel="Critical")을 기본 설정 로깅 값으로 업데이트

줄 266에 있는 동적 로깅 구성: Microsoft.ResourceManagement.Service.exe.config

![사용할 수 있는 다양한 로깅 영역이 포함된 줄을 보여 주는 강조 표시된 섹션](media/mim-service-dynamic-logging/screen02.png)

기본적으로 로깅 위치는 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service**에 있습니다. 동적 로깅을 생성하려면 FIM 서비스 계정이 이 위치에 쓸 수 있는 권한이 있어야 합니다.

![로그의 폴더 위치](media/mim-service-dynamic-logging/screen03.png)

 >[!NOTE]
 예기치 않은 오류(Microsoft.ResourceManagement.Service.exe.config 구성 파일의 구문 오류 또는 기타 오류)가 발생하는 경우 해당 오류 메시지가 %TMP%, %TEMP% 또는 %USERPROFILE% 중 첫 번째에 있는 구성 파일 Microsoft.ResourceManagement.Service.exe_Emergency.log에 기록됩니다.  
1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

추적을 보려면 [서비스 추적 뷰어 도구](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)를 사용할 수 있습니다.

 ![서비스 추적 뷰어 스크린샷](media/mim-service-dynamic-logging/screen04.png)

