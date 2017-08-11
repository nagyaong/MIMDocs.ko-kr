---
title: "PAM REST API 서비스 세부 정보 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 54c78bbd-8da1-42ff-9edc-47d913011941
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 6e248ba4a65e75b1e914c61de70072c7e094123a
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="pam-rest-api-service-details"></a>PAM REST API 서비스 세부 정보
다음 섹션에서는 MIM(Microsoft Identity Manager) PAM(권한 있는 액세스 관리) REST API에 대한 세부 정보를 설명합니다.

## <a name="http-request-and-response-headers"></a>HTTP 요청 및 응답 헤더

API에 전송된 HTTP 요청은 다음 헤더를 포함합니다(이 목록은 완전한 목록이 아님).

헤더 | 설명
-------|------------
권한 부여 | 필수 사항입니다. 인증 방법에 따라 콘텐츠가 달라집니다. WIA(Windows 통합 인증) 또는 ADFS를 기반으로 할 수 있으며 구성 가능합니다.
Content-Type | 요청에 본문이 있는 경우 필수 사항입니다. "application/json"이어야 합니다.
Content-Length | 요청에 본문이 있는 경우 필수 사항입니다. 
쿠키 | 세션 쿠키입니다. 인증 방법에 따라 필요할 수 있습니다.
<br/>
HTTP 응답은 다음 헤더를 포함합니다(이 목록은 완전한 목록이 아님).

헤더 | 설명
-------|------------
Content-Type | API는 항상 "application/json"을 반환합니다.
Content-Length | 요청 본문(있는 경우)의 길이(바이트)입니다.

## <a name="versioning"></a>버전 관리 
API의 현재 버전은 1입니다. 다음 예제와 같이 쿼리 매개 변수를 통해 요청 URL에 API 버전을 지정할 수 있습니다. `http://localhost:8086/api/pamresources/pamrequests?v=1` 요청에 버전을 지정하지 않으면 가장 최근에 릴리스된 APi 버전에 대해 요청이 실행됩니다. 

## <a name="security"></a>보안 
API에 액세스하려면 IWA(Windows 통합 인증)가 필요합니다. 이는 MIM(Microsoft Identity Manager) 설치 전에 IIS에서 수동으로 구성해야 합니다.

HTTPS(TLS)는 지원되지만 IIS에서 수동으로 구성해야 합니다. 자세한 내용은 다음을 참조하세요. **FIM 포털에 대한 SSL(Secure Sockets Layer) 구현** : FIM 2010 R2 테스트 랩 설치 가이드의 [9단계: FIM 2010 R2 설치 후 작업 수행](https://technet.microsoft.com/library/hh322875(v=ws.10%29.aspx) 

Visual Studio 명령 프롬프트에서 다음 명령을 실행하여 새 SSL 서버 인증서를 생성할 수 있습니다.
```
Makecert -r -pe -n CN="test.cwap.com" -b 05/10/2014 -e 12/22/2048 -eku 1.3.6.1.5.5.7.3.1 -ss my -sr localmachine -sky exchange -sp "Microsoft RSA SChannel Cryptographic Provider" -sy 12
```
 
이 명령은 URL이 test.cwap.com인 웹 서버에서 SSL을 사용하는 웹 응용 프로그램을 테스트하는 데 사용할 수 있는 자체 서명된 인증서를 만듭니다. -eku 옵션에 의해 정의된 OID는 인증서를 SSL 서버 인증서로 식별합니다. 인증서는 내 저장소에 저장되고 컴퓨터 수준에서 사용할 수 있으므로 mmc.exe의 인증서 스냅인에서 이 인증서를 내보낼 수 있습니다.

## <a name="cross-domain-access-cors"></a>CORS(도메인 간 액세스) 
CORS는 지원되지만 IIS에서 수동으로 구성해야 합니다. 배포된 API web.config 파일에 다음 요소를 추가하여 도메인 간 호출을 허용하도록 API를 구성합니다. 

```
<system.webServer>       
    <httpProtocol> 
        <customHeaders> 
            <add name="Access-Control-Allow-Credentials" value="true"  /> 
            <add name="Access-Control-Allow-Headers" value="content-type" /> 
            <add name="Access-Control-Allow-Origin" value="http://<hostname>:8090" /> 
        </customHeaders> 
    </httpProtocol> 
</system.webServer> 
```
<br/>

## <a name="error-handling"></a>오류 처리 
API가 오류 상태를 나타내는 HTTP 오류 응답을 반환합니다. 오류는 OData 규격입니다. 다음 표는 클라이언트에 반환될 수 있는 오류 코드입니다.

HTTP 상태 코드 | 설명
-----------------|------------
401 | 권한 없음 
403 | 사용할 수 없음 
408 | 요청 시간 초과   
500 | 내부 서버 오류 
503 | 서비스를 사용할 수 없음 
<br/>

## <a name="filtering"></a>필터링 
PAM REST API 요청은 필터를 포함하여 응답에 포함되어야 하는 속성을 지정할 수 있습니다. 필터 구문은 OData 식을 기반으로 합니다.

필터는 PAM 요청, PAM 역할 또는 보류 중인 PAM 요청의 속성을 지정할 수 있습니다. 예: PAM 요청, PAM 역할 또는 보류 중인 요청의 *ExpirationTime*, *DisplayName*또는 다른 유효한 속성

API는 필터 식에서 다음 연산자를 지원합니다. *And*, *Equal*, *NotEqual*, *GreaterThan*, *LessThan*, *GreaterThenOrEqueal*및 *LessThanOrEqual* 

다음 샘플 요청에는 필터가 포함되어 있습니다.

- 이 요청은 특정 날짜 사이의 모든 PAM 요청을 반환합니다. `http://localhost:8086/api/pamresources/pamrequests?$filter=ExpirationTime gt datetime"2015-01-09T08:26:49.721Z" and ExpirationTime lt datetime"2015-02-10T08:26:49.722Z" `
 
- 이 요청은 표시 이름 "SQL File Access"와 함께 Pam 역할을 반환합니다. `http://localhost:8086/api/pamresources/pamroles?$filter=DisplayName eq "SQL File Access" `
