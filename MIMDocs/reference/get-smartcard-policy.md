---
title: "스마트 카드 정책 가져오기 | Microsoft Docs"
description: 
keywords: 
author: msmbaldwin
ms.author: mbaldwin
manager: mbaldwin
ms.date: 10/17/2016
ms.topic: article
ms.prod: microsoft-identity-manager
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: c015ffc7-5c94-427e-a3b3-870ec8ab92b6
audience: developer
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: 1164795e81ff0ef2f93086144b721e760f8dd028
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="get-smartcard-policy"></a>스마트 카드 정책 가져오기

지정된 워크플로의 프로필 템플릿 정책을 가져옵니다. 이 데이터는 요청을 만드는 동안 사용됩니다. 워크플로 정책은 클라이언트가 요청을 만들기 위해 필요한 데이터를 지정합니다. 이러한 데이터에는 다양한 데이터 컬렉션 항목, 요청 설명 및 일회용 암호 정책이 포함될 수 있습니다.

**참고**: 이 항목에 표시된 URL은 API 배포 중 선택한 호스트 이름과 관련이 있습니다(예: `https://api.contoso.com`.
##<a name="request"></a>요청


메서드  |요청 URL  
---------|---------
GET     |/CertificateManagement/api/v1.0/profiletemplates/{id}/policy/workflow/{형식}

###<a name="url-parameters"></a>URL 매개 변수
매개 변수| 설명
--------|-------------
ID| 필수 사항입니다. 정책을 추출하는 프로필 템플릿에 해당하는 GUID입니다.
형식| 필수 사항입니다. 요청한 정책의 형식입니다. 가능한 값은 다음과 같습니다. *Enroll*, *Duplicate*, *OfflineUnblock*, *OnlineUpdate*, *Renew*, *Recover*, *RecoverOnBehalf*, *Reinstate*, *Retire*, *Revoke*, *TemporaryEnroll*, *Unblock*.

###<a name="request-headers"></a>요청 헤더
일반적인 요청 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="request-body"></a>요청 본문
없음

##<a name="response"></a>응답
###<a name="response-codes"></a>응답 코드
코드  |설명  
---------|---------
200     | 확인
403 | 사용할 수 없음
204 | 내용 없음
500 | 내부 오류

###<a name="response-headers"></a>응답 헤더
일반적인 응답 헤더에 대한 자세한 내용은 *CM REST API 서비스 세부 정보*에서 [HTTP 요청 및 응답 헤더](certificate-management-rest-api-service-details.md#http-request-and-response-headers)를 참조하세요.
###<a name="response-body"></a>응답 본문
성공 시 [ProfileTemplatePolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx) 개체에 따라 정책 개체를 반환합니다. 정책 개체는 최소한 다음 표에 있는 속성을 포함하지만 요청된 정책에 따라 추가 속성을 포함할 수 있습니다. 예를 들어 등록 정책에 대한 요청은 [EnrollPolicy](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.enrollpolicy) 개체를 반환합니다. 자세한 내용은 요청의 {type} 매개 변수와 관련된 정책 개체에 대한 설명서를 참조하세요. 다른 형식의 정책 개체에 대한 설명서는 [Microsoft.Clm.Shared.ProfileTemplates Namespace](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates) 설명서 아래에서 찾을 수 있습니다.

속성 | 설명
---------|------------
ApprovalsNeeded | 정책의 FIM CM 요청에 필요한 승인의 수입니다.
AuthorizedApprover | 정책의 FIM CM 요청을 승인할 수 있는 권한이 있는 사용자에 대한 보안 설명자입니다.
AuthorizedEnrollmentAgent | 정책의 등록 에이전트 역할을 수행할 수 있는 사용자에 대한 보안 설명자입니다.
AuthorizedInitiator | 정책의 FIM CM 요청을 시작할 수 있는 사용자에 대한 보안 설명자입니다.
CollectComments | 정책의 FIM CM 요청에 대해 설명 컬렉션을 사용할 수 있는지 여부를 나타내는 부울 값입니다.
Collect요청Priority | 정책의 FIM CM 요청에 대해 요청 우선 순위 컬렉션을 사용할 수 있는지 여부를 나타내는 부울 값입니다.
Default요청Priority | 정책의 FIM CM 요청에 대한 기본 우선 순위입니다.
Documents | 정책에 대해 구성된 정책 문서입니다.
사용 | 정책을 사용할 수 있는지 여부를 나타내는 부울 값입니다.
EnrollAgentRequired | 정책의 FIM CM 요청에 등록 에이전트가 필요한지 여부를 나타내는 부울 값입니다.
OneTimePasswordPolicy | 정책의 FIM CM 요청에 대한 일회용 암호가 배포되는 방식을 가져옵니다.
Personalization | 정책의 스마트 카드 개인 설정 옵션입니다.
PolicyDataCollection | 정책과 관련된 데이터 컬렉션 항목입니다.
SelfService사용 | 정책의 FIM CM 요청에 대해 셀프 서비스 시작을 사용할 수 있는지 여부를 나타내는 부울 값입니다.

##<a name="example"></a>예

###<a name="request-1"></a>요청 1
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/enroll HTTP/1.1
```
###<a name="response-1"></a>응답 1
```
HTTP/1.1 200 OK

... body coming soon
```       
###<a name="request-2"></a>요청 2
```
GET /CertificateManagement/api/v1.0/profiletemplates/97CD65FA-AF4B-4587-9309-0DD6BFD8B4E1/policies/renew HTTP/1.1
```
###<a name="response-2"></a>응답 2
```
HTTP/1.1 200 OK

... body coming soon
```       
##<a name="see-also"></a>참고 항목

- [Microsoft.Clm.Shared.ProfileTemplates.ProfileTemplatePolicy 클래스](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.profiletemplatepolicy.aspx)
- [Microsoft.Clm.Shared.ProfileTemplates 네임스페이스](https://msdn.microsoft.com/library/windows/desktop/microsoft.clm.shared.profiletemplates.aspx)
