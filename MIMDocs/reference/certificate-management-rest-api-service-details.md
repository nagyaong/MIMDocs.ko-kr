---
title: "CM REST API 서비스 세부 정보 | Microsoft Docs"
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
ms.assetid: 530047f1-e43b-4a69-9542-75bc1da57bf7
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 3d724dea8b1536f0155f9fc287d0cd74f5f16b3c
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="cm-rest-api-service-details"></a>CM REST API 서비스 세부 정보
다음 섹션에서는 MIM(Microsoft Identity Manager) CM(인증서 관리) REST API에 대한 세부 정보를 설명합니다.

## <a name="architecture"></a>아키텍처 
MIM CM REST API 호출은 컨트롤러에서 처리합니다. 다음 표는 컨트롤러 및 컨트롤러를 사용할 수 있는 컨텍스트 샘플의 전체 목록을 보여 줍니다.

컨트롤러| 샘플 경로
----------|-------------
CertificateDataController| /api/v1.0/requests/{requestid}/certificatedata /
CertificateRequestGenerationOptionsController| /api/v1.0/requests/{requestid}/certificaterequestgenerationoptions
CertificatesController| /api/v1.0/smartcards/{smartcardid}/certificates <br/> /api/v1.0/profiles/{profileid}/certificates
OperationsController| /api/v1.0/smartcards/{smartcardid}/operations <br/> /api/v1.0/profiles/{profileid}/operations
PoliciesController| /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
ProfilesController| /api/v1.0/profiles/{id} <br/> /api/v1.0/Profiles <br/> /api/v1.0/requests/{requestid}/profiles/{id}
ProfileTemplatesController| /api/v1.0/profiletemplates/{id} <br/> /api/v1.0/profiletemplates <br/> /api/v1.0/profiletemplates/{profiletemplateid}/policies/{id}
RequestsController| /api/v1.0/requests/{id} <br/> /api/v1.0/requests
SmartcardsController| /api/v1.0/requests/{requestid}/smartcards/{id}/diversifiedkey <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/serverproposedpin <br/> /api/v1.0/requests/{requestid}/smartcards/{id}/authenticationresponse <br/> /api/v1.0/requests/{requestid}/smartcards/{id} <br/> /api/v1.0/smartcards/{id} <br/> /api/v1.0/smartcards
SmartcardsConfigurationController| /api/v1.0/profiletemplates/{profiletemplateid}/configuration/smartcards
<br/>

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


## <a name="api-versioning"></a>API 버전 관리 
CM REST API의 현재 버전은 1.0입니다. 버전은 URI에서 `/api` 세그먼트 바로 뒤의 세그먼트에 지정됩니다(예: `/api/v1.0`). API 인터페이스에 주요 변경 사항이 있는 경우 나중에 버전 번호가 변경됩니다.


## <a name="enabling-the-api"></a>API 사용 설정 
Web.config 파일의 `<ClmConfiguration>` 섹션이 새 키로 확장되었습니다.

```
<add key="Clm.WebApi.Enabled" value="false" />
```
<br/>

이 키는 CM REST API가 클라이언트에 노출되는지 여부를 결정합니다. 키가 "false"로 설정된 경우 응용 프로그램이 시작할 때 API에 대한 경로 매핑이 수행되지 않습니다. 즉, API 끝점에 대한 모든 후속 요청은 HTTP 404 오류 코드를 반환합니다. 키는 기본적으로 "disabled"으로 설정됩니다.
이 값을 "true"로 변경한 후 서버에서 **iisreset** 을 실행해야 합니다.

## <a name="enabling-tracing-and-logging"></a>추적 및 로깅 사용 설정 
MIM CM REST API는 전송된 각 HTTP 요청에 대한 추적 데이터를 내보냅니다. 다음 구성 값을 설정하여 내보낸 추적 정보의 세부 정보 표시 수준을 설정할 수 있습니다.

```
<add name="Microsoft.Clm.Web.API" value="0" />
```
<br/>

## <a name="error-handling-and-troubleshooting"></a>오류 처리 및 문제 해결 
요청을 처리하는 동안 예외가 발생하면 MIM CM REST API가 HTTP 상태 코드를 웹 클라이언트로 반환합니다. 일반적인 오류의 경우 API가 적절한 HTTP 상태 코드 및 오류 코드를 반환합니다. 

처리되지 않은 예외는 HTTP 상태 코드 500("내부 오류")과 함께 `HttpResponseException` 으로 변환되고 이벤트 로그와 MIM CM 추적 파일에서 모두 추적됩니다. 처리되지 않은 각 예외는 해당 상관 관계 ID와 함께 이벤트 로그에 기록됩니다. 또한 상관 관계 ID는 오류 메시지로 API 소비자에게도 전송됩니다. 오류를 해결하기 위해 관리자가 해당 상관 관계 ID 및 오류 세부 정보에 대해 이벤트 로그를 검색할 수 있습니다.

보안 문제로 인해 MIM CM REST API를 사용한 결과로 생성된 오류에 해당하는 스택 추적은 클라이언트로 다시 전송되지 않습니다.
