---
title: MIM 서비스 동적 로깅 | Microsoft 문서
description: 관리 서비스를 다시 시작하지 않고 MIM 서비스 동적 로깅 사용
keywords: ''
author: fimguy
ms.author: davidste
manager: mbaldwin
ms.date: 06/25/2018
ms.topic: article
ms.prod: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: ''
ms.openlocfilehash: ff82b2fce31abe417509347ce7b477dd1b4056f2
ms.sourcegitcommit: ace4d997c599215e46566386a1a3d335e991d821
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/15/2018
ms.locfileid: "49332340"
---
# <a name="mim-sp1-4414360--service-dynamic-logging"></a>MIM SP1(4.4.1436.0) 서비스 동적 로깅
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

기본적으로 로깅 위치는 **C:\Program Files\Microsoft Forefront Identity Manager\2010\Service에 있습니다. 동적 로그를 생성하려면 FIM 서비스 계정이 이 위치에 대한 쓰기 권한이 있어야 합니다.

![로그의 폴더 위치](media/mim-service-dynamic-logging/screen03.png)

> [!NOTE]
>  예기치 않은 오류(Microsoft.ResourceManagement.Service.exe.config 구성 파일의 구문 오류 또는 기타 오류)가 발생하는 경우 해당 오류 메시지가 %TMP%, %TEMP% 또는 %USERPROFILE% 중 첫 번째에 있는 구성 파일 Microsoft.ResourceManagement.Service.exe_Emergency.log에 기록됩니다.  
> 1. "%TMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 2. "%TEMP%\Microsoft.ResourceManagement.Service.exe_Emergency.log"
> 3. "% USERPROFILE %\Microsoft.ResourceManagement.Service.exe_Emergency.log"

추적을 보려면 [서비스 추적 뷰어 도구](https://msdn.microsoft.com//library/aa751795(v=vs.110).aspx)를 사용할 수 있습니다.

 ![서비스 추적 뷰어 스크린샷](media/mim-service-dynamic-logging/screen04.png)

# <a name="updates-build-45xx-or-greater"></a>업데이트: 빌드 4.5.x.x 이상

빌드 4.5.x.x에서는 기본 로깅 수준을 **“경고”** 로 지정하도록 로깅 기능을 수정했습니다. 서비스는 두 개의 파일(확장명 앞에 “00” 및 “01” 인덱스가 추가됨)에 메시지를 씁니다. 파일은 “C:\Program Files\Microsoft Forefront Identity Manager\2010\Service” 디렉터리에 있습니다. 파일이 최대 크기를 초과할 경우 서비스가 다른 파일에서 쓰기를 시작합니다. 다른 파일이 있으면 덮어씁니다. 파일의 기본 최대 크기는 1GB입니다. 기본 최대 크기를 변경하려면 최대 파일 크기 값(KB)과 함께 **“maxOutputFileSizeKB”** 매개 변수를 수신기에 추가(아래 예제 참조)하고 MIM 서비스를 다시 시작해야 합니다. 서비스가 시작되면 보다 최신 파일에 로그를 추가하고, 공간 제한을 초과하는 경우, 가장 오래된 파일을 덮어씁니다. 

> [!NOTE] 메시지를 쓰기 전에 서비스에서 파일 크기를 확인하면 파일 크기가 한 메시지의 최대 크기보다 클 수 있습니다. 기본적으로 로그의 크기는 약 6GB(1GB 크기의 파일 두 개가 있는 수신기 3개)일 수 있습니다.

> [!NOTE] 서비스 계정은 >“C:\Program Files\Microsoft Forefront Identity Manager\2010\Service”> 디렉터리에 쓸 수 있는 권한이 있어야 합니다. 서비스 계정에 이러한 권한이 없으면 파일이 생성되지 않습니다.

최대 파일 크기를 svclog 파일의 경우 200MB(200 * 1024KB)로 설정하고 txt 파일의 경우 100MB*(100 * 1024KB)로 설정하는 방법 예제

`<add initializeData="Microsoft.ResourceManagement.Service_tracelog.svclog" type="Microsoft.IdentityManagement.CircularTraceListener.CircularXmlTraceListener, Microsoft.IdentityManagement.CircularTraceListener, PublicKeyToken=31bf3856ad364e35" name="ServiceModelTraceListener" traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" maxOutputFileSizeKB="204800">`
