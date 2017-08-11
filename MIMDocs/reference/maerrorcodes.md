---
title: "관리 에이전트 실행 오류 코드 | Microsoft Docs"
description: 
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 08/01/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: 511bd9ae4e303a2142c3f101c3880dc8cb46be3d
ms.sourcegitcommit: 5ba5d916c0ca1e5aa501592af0cef714bfdc8afe
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/02/2017
---
# <a name="management-agent-run-error-codes"></a>관리 에이전트 실행 오류 코드

다음 표에는 MIM(Microsoft Identity Manager) 2016의 Synchronization Service Manager 사용자 인터페이스에 표시될 수 있는 오류 코드와 각 오류에 대한 설명이 포함되어 있습니다.

## <a name="connection-errors"></a>연결 오류

| 오류                  | 설명                                    |
|--------------------------|--------------------------------------------|
| failed=connection         | 연결된 디렉터리에 대한 연결이 인증 외의 다른 이유로 실패했습니다. 예를 들어 네트워크를 사용할 수 없거나 대상 서버가 오프라인 상태입니다.                                                                                                                                                                                                                                                                                                                      |
| dropped-connection        | 관리 에이전트와 연결된 디렉터리 간 연결이 더 이상 존재하지 않습니다. 관리 에이전트는 대부분의 경우 연결된 디렉터리에 다시 연결하려고 합니다.                                                                                                                                                                                                                                                                                                         |
| failed-authentication     | 제공된 자격 증명을 사용하여 인증할 수 없습니다.                                                                                                                                                                                                                                                                                                                                                                                                                          |
| failed-permission         | 연결된 디렉터리의 컨테이너에 액세스할 수 있는 충분한 권한이 없습니다. 이 오류는 연결된 다른 디렉터리 컨테이너를 검색하는 LDAP(Lightweight Directory Access Protocol) 관리 에이전트에서만 발생합니다.                                                                                                                                                                                                                                                              |
| failed-search            | 예기치 않은 오류를 발생하면 컨테이너 또는 테이블 검색이 실패했습니다.                                                                                                                                                                                                                                                                                                                                                                                                                            |
| warning-no-watermark      | 관리 에이전트에서 전체 가져오기를 수행할 때 워터마크를 읽을 수 없습니다. 초기 관리 에이전트 구성이 완료되고 연결된 디렉터리에 변경 로그가 사용되도록 설정되면 Sun ONE Directory Server 5.1(이전의 iPlanet Directory Server)용 관리 에이전트에서만 이 오류가 발생합니다. 나중에 연결된 디렉터리 변경 로그가 해제되고 관리 에이전트 구성이 업데이트되지 않은 경우 전체 가져오기가 완료될 때 이 경고가 발생합니다. |
| no-start-partition-delete | 이 관리 에이전트에 대해 처음에 구성된 LDAP 파티션이 더 이상 없습니다. 파티션을 삭제되었거나, 파티션이 삭제됨과 동시에 다시 만들어지면 이 오류가 반환됩니다. 후자의 경우 이름이 동일하더라도 오류 파티션이 GUID는 달라진 것입니다.                                                                                                                                                                           |

## <a name="discovery-errors"></a>검색 오류

| 오류                   | 설명                                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|-------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| missing-change-type      | 이 오류는 변경 형식 열 값(add, modify, delete)이 없을 때 파일 기반 및 데이터베이스 관리 에이전트는 물론, Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 의한 델타 가져오기 실행 동안 반환됩니다.                                                                                                                                                                                                                                          |
| invalid-change-type      | 이 오류는 변경 형식 열 값이 유효한 변경 형식 목록과 일치하지 않을 때 델타 가져오기 파일 기반 및 데이터베이스 관리 에이전트는 물론, Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 의한 델타 가져오기 실행 동안 반환됩니다. 변경 형식 필드가 있고 add 이외의 값을 가질 경우 LDIF(LDAP 데이터 교환 형식) 전체 가져오기에서도 이 오류가 반환됩니다.                                                                                  |
| multi-valued-change-type | 이 오류는 변경 형식에 대해 2개 이상의 값이 있을 때 파일 기반, Sun 및 Netscape 디렉터리 서버 관리 에이전트에 의한 델타 가져오기 실행 동안 반환됩니다.                                                                                                                                                                                                                                                                                                        |
| need-full-object        | 이 오류는 파일 기반 관리 에이전트의 델타 가져오기 실행 동안 또는 파일 기반 관리 에이전트에서 다시 실행될 때 반환됩니다. 관리 에이전트가 커넥터 공간에 있을 수 없는 개체에 대한 수정 내용을 제출했음을 나타냅니다. 동기화 엔진은 개체의 모든 특성에 대한 현재 값을 요청하고 있습니다. 파일에서 가져오기이므로 해당 정보를 사용할 수 없습니다. 전체 가져오기로 이 문제를 해결해야 합니다. |
| missing-dn              | 이 오류는 도메인 이름 값이 없을 때 파일 기반 관리 에이전트(즉, 도메인 이름 값이 구성되어 있는 LDIF, DSML 또는 플랫 파일용 관리 에이전트)에 대해 반환됩니다. 도메인 이름 특성이 없는 경우 손상된 Sun ONE Directory Server 변경 로그에서도 이 오류가 반환됩니다. 관리 에이전트가 요소를 읽고 구문 분석할 수 있지만 개체에 대한 도메인 이름 값이 없음을 나타냅니다.                           |
| dn-not-ldap-conformant   | 이 오류는 도메인 이름 특성이 구성된 LDAP, LDIF, DSML 또는 플랫 파일용 관리 에이전트가 LDAP 사양을 준수하지 않는 도메인 이름 값을 보고할 때 반환됩니다.                                                                                                                                                                                                                                                                                |
| invalid-dn              | 이 오류는 관리 에이전트가 도메인 이름이 다음을 비롯한 FIM 제약 조건을 충족하지 않음을 보고할 때 반환됩니다.   <br/>&#176; FIM에서 허용되지 않는 하나 이상의 문자 <br/>&#176; 빈 RDN(상대 고유 이름) <br/>&#176; FIM에 대한 최대값을 초과하는 상대 고유 이름 <br/>&#176; 도메인 이름의 계층 수준 수가 FIM에 대한 최대값을 초과합니다.                                                                                                                                                                                                                                                                                                                                                            |
| missing-anchor-component | 이 오류는 하나 이상의 앵커 생성 규칙 특성에 값이 없으므로 앵커를 생성할 수 없을 때 Sun 및 Netscape 디렉터리 서버용 관리 에이전트 뿐만 아니라 파일 기반 및 데이터베이스 관리 에이전트에 의해 반환됩니다.   |
| multi-valued-anchor-component | 이 오류는 앵커 생성 규칙 특성에 값이 여러 개 있으므로 앵커를 생성할 수 없는 경우 Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 의해 반환됩니다. |   
|anchor-too-long | 이 오류는 앵커 생성이 FIM에 대한 최대 크기 제한을 초과하는 앵커를 생성할 때 파일 기반 및 데이터베이스 관리 에이전트는 물론, Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 의한 델타 가져오기 실행 동안 반환됩니다.|
|duplicate-object | 이 오류는 동일한 앵커를 갖는 개체가 이 실행 동안 이미 동기화 엔진에 보고된 경우 파일 기반 및 데이터베이스 관리 에이전트에 의한 전체 가져오기 시 반환됩니다.|

>[!Note]
현재 실행 단계가 성공으로 완료, 동기화 실패로 완료, 경고로 완료 또는 일시적으로 완료인 경우에만 커넥터 공간 개체가 더 이상 사용되지 않습니다.

|오류| 설명 |
|----|----|  
|missing-object-class | 이 오류는 손상된 변경 로그가 있는 경우 파일 기반 관리 에이전트(즉, 개체 클래스 특성이 구성된 DSML, LDIF 또는 플랫 파일용 관리 에이전트) 또는 Sun 및 Netscape 디렉터리 서비스용 관리 에이전트에서 반환됩니다. 이것은 관리 에이전트가 개체 클래스 특성에 대한 값을 읽을 수 없음을 나타냅니다. |
|missing-object-type | 이 오류는 손상된 파일 삭제에서 가져오기를 다시 시작할 때 반환됩니다. 이 오류는 정상적인 작업 중에는 발생하지 않아야 합니다.        |
|unmappable-object-type  | 이 오류는 접두사 매핑 중 하나와 일치하지 않는 개체 클래스 값 집합이 포함된 개체를 읽을 때 파일 기반 관리 에이전트에서 반환됩니다. |
|parse-error | 이 오류는 항목을 구문 분석할 수 없을 때 델타 모드의 Sun 및 Netscape 디렉터리 서버용 관리 에이전트 및 파일 기반 관리 에이전트에 의해 반환됩니다. `<entry-number>` 요소(및 대부분의 경우 `<line-number>` 및 `<column-number>`)는 오류를 찾는 데 도움을 주기 위해 표시됩니다. `<attribute-name>` 요소가 존재할 수 있습니다. Sun 및 Netscape 디렉터리 서버용 관리 에이전트는 이 문제가 발생할 경우 실행을 종료합니다. 파일 기반 관리 에이전트 검색는 오류를 기록하고 계속합니다. |   
|read-error | 이 오류는 특정 개체를 읽을 때 일반 오류가 있어면 호출 기반 관리 에이전트에 의해 반환됩니다. 일반적으로 이로 인해 실행이 종료됩니다. 연결된 데이터 소스 오류 요소가 제공되며 이를 사용하여 문제를 해결할 수 있습니다.    |
|staging-error | 이 오류는 대부분의 관리 에이전트에 의해 반환됩니다. 동기화 엔진이 커넥터 공간에 델타를 스테이징할 수 없음을 나타냅니다. 서버는 문제에 대한 정보를 제공하고 문제 해결에 사용할 수 있는 이벤트 로그를 만듭니다. 대부분의 관리 에이전트는 오류가 로깅될 때 가져오기 실행을 계속하지만 Sun 및 Netscape 델타용 관리 에이전트는 커넥터 공간의 일관되지 않은 상태 때문에 실행을 중지합니다. 이 오류는 정상적인 작업 중에는 발생하지 않아야 합니다.  |
|invalid-modification-type | 이 오류는 개체 수준 수정 형식이 표준 LDIF 수정 형식 중 하나가 아니거나 objectclass에 nonreplace 수정 형식(예: add: objectclass 또는 delete: objectclass)이 있을 때 LDIF 관리 에이전트의 델타 가져오기 동안 반환됩니다.    |
|conflicting-modification-types        | 이 오류는 LDIF 관리 에이전트에 의해 반환되며 동일한 레코드에서 다른 특성 수준 수정 형식이 발생했거나(이 경우 충돌하는 형식을 생성한 특성 이름이 보고됨) 같은 파일에서 여러 LDIF 델타 교체가 확인되었음을 나타냅니다.    <br/> [LDIF 파일 예제](./media/maerrorcodes/conflict-modification-types.jpg) |
|multi-single-mismatch | 이 오류는 파일 기반 관리 에이전트가 FIM에 단일 값 특성으로 정의된 특성에 대해 둘 이상의 값 추가 또는 둘 이상의 값 삭제를 보고할 때 반환됩니다. 이 오류는 FIM과 함께 저장된 연결된 데이터 소스 스키마가 잘못 지정되었거나(파일 기반 관리 에이전트) 현재 스키마보다 오래된 버전임을 나타낼 수 있습니다. 오류의 컨텍스트를 제공하는 `<attribute-name>` 요소가 포함되어 있습니다.    |
|invalid-attribute-value | 이 오류는 스키마에 선언된 특성 형식을 준수하지 않는 특성 값을 읽을 때 호출 기반 관리 에이전트에 의해 반환됩니다. 오류의 컨텍스트를 제공하는 `<attribute-name>` 요소가 포함되어 있습니다.  |
|invalid-base64-value| 이 오류는 잘못된 base64 문자열이 있을 때 LDIF, DSML, Sun 및 Netscape 디렉터리 서버용 관리 에이전트에서 반환됩니다.     |
|invalid-numeric-value | 이 오류는 숫자 값을 구문 분석할 수 없는 경우 파일 기반 관리 에이전트 및 LDAP용 관리 에이전트에서 반환됩니다. 오류의 컨텍스트를 제공하는 `<attribute-name>` 요소가 포함되어 있습니다. |
|invalid-boolean-value | 이 오류는 부울 값을 구문 분석할 수 없는 경우 파일 기반 관리 에이전트 및 LDAP용 관리 에이전트에서 반환됩니다. 오류의 컨텍스트를 제공하는 `<attribute-name>` 요소가 포함되어 있습니다.  |
|reference-value-not-ldap-conformant | 이 오류는 도메인 이름 값이 LDAP 사양을 준수하지 않을 때 도메인 이름 특성이 구성된 LDAP, LDIF, DSML 또는 플랫 파일용 관리 에이전트에서 반환됩니다. 이 오류 메시지에는 오류의 컨텍스트를 제공하는 `<attribute-name>` 요소가 포함되어 있습니다.|
|invalid-reference-value | 이 오류는 도메인 이름이 다음을 비롯한 FIM 제약 조건을 충족하지 않을 때 관리 에이전트에서 반환됩니다.  <br/>&#176; FIM에서 허용되지 않는 하나 이상의 문자   <br/>&#176; 빈 RDN(상대 고유 이름)  <br/>&#176; FIM에 대한 최대값을 초과하는 상대 고유 이름  <br/>&#176; 도메인 이름의 계층 수준 수가 FIM에 대한 최대값을 초과합니다.                |
|unsupported-value-type | 이 오류는 파일의 지정된 값 형식이 다음을 비롯한 특성 형식과 호환되지 않을 때 DSML 또는 LDIF 관리 에이전트에서 반환됩니다.    <br/>&#176; 비문자열 특성 또는 예약된 키워드(예: dn, objectclass 또는 changetype)에 대해 URI 또는 URL 값이 제공됩니다.  <br/>&#176; changetype 특성에 대해 base64 값이 제공됩니다.  <br/>&#176; 이진 특성에 대해 비 ASCII 문자를 포함하는 문자열 값이 제공됩니다. |

## <a name="synchronization-errors"></a>동기화 오류

| 오류               |설명      |
|---------|-------------|
| extension-dll-exception  | 규칙 확장이 예외를 발생하는 경우 이 오류가 발생합니다. 이 오류가 발생하는 경우 \<exception-error-info\> 요소를 확인하여 예외의 호출 스택을 검사합니다. 일부 경우에는 `<rule-error-info>`가 있으며 오류가 발생할 때 어떤 처리 중이던 규칙에 대한 추가 정보를 제공합니다.        |
| extension-dll-crash | 이 오류는 규칙 확장을 실행하는 프로세스가 예기치 않게 종료될 때 발생합니다. 이 오류는 규칙 확장이 out-of-process로 실행되는 동안에만 발생할 수 있습니다. 이 오류 값의 가능한 원인은 규칙 확장이 액세스 위반을 발생하는 코드를 호출하는 것입니다. |
| extension-dll-timeout                          | 이 오류는 고객이 확장 시간 제한을 구성했으며 단일 고객 확장 코드 진입점 호출이 구성된 시간 제한을 초과하는 경우에 발생합니다. `<exception-error-info>`는 시간이 초과될 때 호출되고 있던 진입점에 대한 컨텍스트 정보를 제공합니다. 일부 경우에는 `<rule-error-info>가 있으며 오류가 발생할 때 처리 중이던 규칙에 대한 추가 정보를 제공합니다. 확장을 실행하는 프로세스를 디버그하는 경우 시간 제한이 적용되지 않습니다.  |              
| extension-projection-object-type-not-set        | 이 오류는 규칙 확장의 **IMASynchronization.ShouldProjectToMV** 메서드가 메타버스 개체 형식을 지정하지 않을 때 발생합니다.                    |
| extension-projection-invalid-object-type        | 이 오류는 규칙 확장의 **IMASynchronization.ShouldProjectToMV** 메서드 구현이 아웃바운드 메타버스 개체 형식 값을 Synchronization Service Manager의 메타버스 디자이너에 나열되지 않은 값으로 설정할 때 발생합니다. 이 메서드가 지정된 개체 형식 값 중 하나를 사용하는지 확인합니다.   |
| extension-join-resolution-invalid-object-type   | 이 오류는 규칙 확장의 **IMASynchronization.ResolveJoinSearch** 메서드 구현이 아웃바운드 메타버스 개체 형식 값을 Synchronization Service Manager의 메타버스 디자이너에 나열되지 않은 값으로 설정할 때 발생합니다. 이 메서드가 아웃바운드 메타버스 개체 형식의 값을 나열된 개체 형식 값 중 하나로 설정하는지 확인합니다.   |
| extension-join-resolution-index-out-of-bounds   | 이 오류는 규칙 확장의 **IMASynchronization.ResolveJoinSearch** 메서드 구현이 음수이거나 메타버스 개체 수보다 크거나 같은 인덱스 값을 설정할 때 발생합니다.                     |
| extension-provisioning-call-limit-reached       | 이 오류는 단일 개체를 동기화하는 동안 **IMASynchronization.Provision** 메서드가 10번 넘게 호출될 때 발생합니다. 이 메서드는 Provision 메서드의 고객 논리가 개체를 프로비전 해제하고 메타버스 개체를 변경하여 Provision이 새로 호출되도록 하는 특성 회수가 있을 때 2번 이상 호출될 수 있습니다. Provision 메서드에 대한 10회 호출 제한은 가능한 무한 프로비저닝 노트를 중지하기 위해 설정됩니다.                                                                                                                                               |
| extension-deprovisioning-invalid-result         | 이 오류는 **IMASynchronization.Deprovision** 메서드의 구현이 잘못된 DeprovisionAction 열거형 값을 반환할 때 발생합니다. 이 메서드가 유효한 값을 반환하는지 확인합니다.        |
| extension-entry-point-not-implemented           | 이 오류는 규칙 확장이 **EntryPointNotImplementedException** 예외를 throw할 때 발생합니다. |
| extension-unexpected-attribute-value            | 이 오류는 규칙 확장이 **UnexpectedDataException** 예외를 throw할 때 발생합니다.            |
| flow-multi-values-to-single-value              | 이 오류는 Synchronization Service Manager에 구성된 가져오기 또는 내보내기 특성 흐름 규칙이 여러 값이 있는 특성을 단일 값 특성으로 보내려고 할 때 발생합니다. 이 오류는 Synchronization Service Manager에 구성된 직접 흐름 규칙에 대해서만 반환됩니다. 흐름 규칙이 여러 값을 단일 값 특성으로 보내는 규칙 확장을 사용하는 경우 **TooManyValuesException** 예외가 throw됩니다.          |
| cs-attribute-type-mismatch                     | 이 오류는 가져온 특성의 형식이 관리 에이전트 스키마에 지정된 특성 형식과 일치하지 않을 때 발생합니다. 이 오류의 한 가지 원인은 저장된 연결된 데이터 소스 스키마가 연결된 데이터 소스의 실제 스키마보다 오래된 것일 수 있습니다. 저장된 연결된 데이터 소스 스키마를 최신 상태로 가져오려면 Synchronization Service Manager를 사용하여 스키마를 새로 고칩니다.|
| join-object-id-must-be-single-valued            | 이 오류는 Synchronization Service Manager의 관리 에이전트 속성에 지정된 조인 규칙을 통해 메타버스 개체를 조인하는 데 사용된 데이터 소스 특성 값에 둘 이상의 값이 포함되어 있을 때 발생합니다. 조인 규칙에 사용된 데이터 소스 특성 값에는 단일 값만 포함될 수 있습니다.     |
| dn-index-out-of-bounds       | 이 오류는 Synchronization Service Manager의 관리 에이전트 속성에 구성된 가져오기 특성 흐름에 사용된 고유 이름 구성 요소 인덱스 값이 소스 개체의 고유 이름에 있는 구성 요소 수보다 클 때 발생합니다.      |
| connector-filter-rule-violation                | 이 오류는 프로비저닝 작업 추가/이름 바꾸기 또는 내보내기 특성 흐름을 수행할 때와 커넥터 개체가 커넥터 필터 구성의 결과로 필터링된 디스커넥터 개체가 될 때 발생합니다. 이 값은 명시적 커넥터 개체에서는 발생하지 않습니다.    |
| unsupported-container-delete   | 관리 에이전트가 프로비전 해제 동안 컨테이너 개체를 삭제하려고 합니다. FIM 관리 에이전트는 자식 개체가 있는 컨테이너 개체를 삭제할 수 없습니다.            |
| ambiguous-import-flow-from-multiple-connectors   | 이 오류는 소스 관리 에이전트 아래의 여러 커넥터가 메타버스 개체에 연결되어 있고 선언적 가져오기 특성 흐름 규칙이 정의될 때 발생합니다. 여러 커넥터가 있는 관리 에이전트를 통해 메타버스 개체로 특성을 가져오려면 규칙 확장을 사용하여 관리 에이전트의 속성에서 직접 규칙을 구성하는 대신 흐름 규칙을 정의합니다.                                                                                                                                                                                                          |
| ambiguous-export-flow-to-single-valued-attribute | 이 오류는 Synchronization Service Manager의 관리 에이전트 속성에 구성된 내보내기 흐름 규칙이 메타버스 개체에서 단일 값 특성으로 여러 값을 보내려고 할 때 발생합니다.       |
| cannot-parse-object-id | Synchronization Service Manager의 관리 에이전트 속성에 지정된 조인 규칙에서 메타버스 개체를 검색하는 데 사용되는 문자열 값이 올바른 GUID(Globally Unique Identifier) 형식이 아닙니다. GUID 형식은 {nnnnnnnn-nnnn-nnnnnnnn-nnnnnnnnnnnn}입니다. 여기서 n은 16진수 숫자입니다. |
| unexported-container-rename  | IMVSynchronization.Provision 또는 IMASynchronization.Deprovision 메서드의 구현은 하나 이상의 내보내지 않은 자식 개체가 있는 컨테이너 개체의 이름을 바꾸려고 합니다.   |
| mv-constraint-violation           | 이 오류는 직접 가져오기 특성 흐름이 발생하고 커넥터 공간의 특성 값이 메타버스 특성의 길이 제한을 초과하는 경우에 발생합니다. |
| locking-error-needs-retry   | 여러 관리 에이전트가 동일한 커넥터 공간 개체를 동기화하려고 합니다. 관리 에이전트를 다시 실행합니다.     |
| unique-index-violation              | 사용자가 메타버스 테이블에서 특성에 대한 고유 인덱스를 수동으로 설정하고 있습니다. 메타버스 테이블은 수동으로 구성하지 않아야 합니다.    |
| encryption-key-lost                            | 암호화 키 집합이 FIM을 실행하는 서버에 없습니다.  |
| unexpected-error                              | 이 오류는 동기화 엔진이 메타버스에 변경 내용(프로비저닝 및 내보내기 특성 흐름 포함)을 적용하려고 할 때 발생합니다. 이 오류는 메타버스에 변경 내용을 적용하는 실행 동안에만 발생할 수 있습니다. 자세한 내용은 이벤트 로그를 확인하십시오.  |
| exported-change-not-reimported                 | 이 오류는 관리 에이전트로 내보낸 변경 내용이 이 가져오기 관리 에이전트 실행 동안 다시 확인되지 않을 때 발생합니다. FIM 외부에서 작동하는 사용자 또는 시스템 프로세스가 내보내기 특성 흐름 규칙이 연결된 데이터 소스 개체로 값을 보내려고 할 때 구성 문제를 나타내는 방식으로 연결된 데이터 소스의 데이터를 변경했으나 연결된 데이터 소스가 관리 에이전트에 오류를 보고하지 않고 이 값을 자동으로 다른 값으로 재설정하려고 합니다. \<change-not-reimported\> 요소는 다시 확인되지 않은 변경 내용을 나타냅니다. |
| cannot-parse-dn-component                      | 이 오류가 LDAP 스타일 DN(고유 이름)이 구성된 관리 에이전트에 의해 반환되고 커넥터 공간에서 메타버스로의 동기화가 실패했습니다. 고유 이름 구성 요소가 대상 특성 형식에 대해 올바른 형식이 아니므로 구성 요소 매핑에 의해 구문 분석될 수 없습니다.|
| missing-partition-for-run-step                 | 이 오류는 실행 프로필에 지정된 파티션을 찾을 수 없음을 나타냅니다. 파티션이 삭제되었거나 이름이 바뀌지 않았는지 확인합니다.  |

## <a name="export-errors"></a>내보내기 오류

| 오류                    |설명     |
|--------------------------|------------|
| cd-missing-object         | 이 오류는 개체 수정 내용이 연결된 데이터 소스로 내보내졌으나 해당 개체를 연결된 데이터 소스에서 찾을 수 없을 때 반환됩니다. 이 오류는 호출 기반 관리 에이전트에 대해서만 반환됩니다. 이 오류의 원인은 사람 또는 외부 프로세스가 FIM 외부의 연결된 데이터 소스에서 개체를 삭제한 것입니다.  |
| cd-existing-object        | 이 오류는 추가 내용이 연결된 데이터 소스로 내보내졌으나 연결된 데이터 소스에 해당 개체가 이미 있을 때 반환됩니다. 이 오류는 호출 기반 관리 에이전트 및 관계형 데이터베이스 관리 에이전트에 대해서만 반환됩니다. |
| duplicate-anchor          | 이 오류는 새로 프로비전된 개체의 앵커가 고유하지 않은 경우에 반환됩니다. 이 오류는 호출 기반 데이터베이스 관리 에이전트 뿐만 아니라 Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 대해 반환됩니다. 이 오류가 발생하는 경우 앵커 생성 규칙을 확인하여 각 개체에 대한 고유 앵커 값이 정의되어 있는지 확인합니다.      |
| ambiguous-update          | 이 오류는 앵커가 고유하지 않으므로 관리 에이전트가 업데이트 또는 삭제 델타를 적용할 수 없을 때 반환됩니다. 이 오류는 Microsoft SQL Server 및 Oracle Database용 관리 에이전트에 대해서만 반환됩니다. 이 오류가 발생하는 경우 앵커 생성 규칙을 확인하여 각 개체에 대한 고유 앵커 값이 정의되어 있는지 확인합니다. |
| password-policy-violation | 이 오류는 암호 속성이 설정되어 있거나 연결된 데이터 소스의 관리자가 정의한 정책에 맞지 않는 값으로 변경될 때 Active Directory 및 Active Directory GAL(전체 주소 목록)에 대한 관리 에이전트에서 반환됩니다. |
| password-set-disallowed   | 이 오류는 암호 암호화가 암호 없음 또는 128비트 SSL(Secure Sockets Layer)로 설정되고 관리자가 이 시나리오의 암호 설정을 허용하기 위해 명시적으로 재정의를 수행하지 않았을 때 ADAM(Active Directory Application Mode)용 관리 에이전트에서 반환됩니다.      |
| kerberos-time-skew        | 이 오류는 암호 특성이 설정되거나 변경되고 FIM 서버 컴퓨터 시간이 도메인 컨트롤러의 시간보다 5분 넘게 차이를 보일 때 Active Directory 및 Active Directory GAL(전체 주소 목록)용 관리 에이전트에서 반환됩니다.  |
| kerberos-no-logon-server  | 이 오류는 Active Directory 및 Active Directory GAL(전체 주소 목록)용 관리 에이전트에서 암호 특성을 설정하거나 변경하려고 하며 로그온 자격 증명의 도메인 부분에서 서버를 확인할 수 없을 때 반환됩니다. 이 오류는 잘못된 NetBIOS 또는 DNS 구성에 의해 발생할 수 있습니다.  |
| encryption-not-enabled    | 이 오류는 암호 특성이 설정 또는 변경되고 있으며 관리 에이전트가 연결된 ㄷ이터 소스와 통신하는 데 사용하는 연결이 해당 암호화 메커니즘(128비트 SSL 또는 TLS)으로 구성되지 않았을 때 ADAM(Active Directory Application Mode)용 관리 에이전트에서 반환됩니다. ADAM에는 암호 설정을 위해 128비트 SSL 또는 TLS 구성이 필요합니다.   |
| invalid-dn               | 이 오류는 새로 프로비전된 개체를 내보내거나 기존 개체의 이름을 바꿀 때, 고유 이름이 연결된 데이터 소스 명명 요구 사항에 부합되지 않을 때 LDAP ㅁ및 Windows NT 4.0용 관리 에이전트에서 반환됩니다.|
| schema-violation          | 이 오류는 개체 수정 내용을 내보내고 연결된 데이터 소스 스키마에 없는 특성을 추가하거나 해당 스키마에 필요한 특성을 개체에서 제거할 때 LDAP용 관리 에이전트에서 반환됩니다. FIM은 해당 규칙이 연결된 데이터 소스 스키마의 저장된 사본을 확인하므로 이러한 작업이 발생하도록 허용하지 않습니다. 그러나 FIM 스키마가 연결된 데이터 소스 스키마보다 이전 버전이면 이 문제가 발생할 수 있습니다. 이 문제가 발생하는 경우 사용자 인터페이스를 사용하여 관리 에이전트 스키마를 새로 고칩니다.             |
| constraint-violation      | 이 오류는 추가, 수정 또는 삭제 내용의 내보내기가 연결된 데이터 소스 적용 제약 조건을 위반하는 경우 LDAP용 관리 에이전트 및 데이터베이스 관리 에이전트에서 반환됩니다. LDAP용 관리 에이전트에 대한 일반적인 원인에는 단일 값 특성에 대해 여러 값 설정, 문자열 및 이진 특성에 대해 필드 너비 제약 조건 초과, 숫자 특성에 대한 범위 제약 조건 위반 등이 포함됩니다. 해당 데이터베이스에 대해 정의될 수 있는 참조 무결성, 규칙 및 제약 조건을 포함하여 데이터베이스 관리 에이전트에 대한 여러 원인이 있을 수 있습니다. |
| syntax-violation                   | 이 오류는 특성 값이 특정 값 제약 조건을 위반하는 경우 LDAP 및 Windows NT 4.0용 관리 에이전트에서 반환됩니다. 예를 들어 내보내는 값에 잘못된 문자가 있는 경우가 있습니다. |
| modify-naming-attribute             | 이 오류는 명명 특성(예: 여러 개체 형식에 대한 CN)이 상대 DN(고유 이름) 값과 충돌하는 값으로 설정된 경우 LDAP에 대한 관리 에이전트에 의해 반환됩니다. 이 오류는 잘못 정의된 내보내기 특성 흐름 규칙 또는 새로 프로비전된 개체의 초기 값을 설정하는 스크립트 코드의 오류 때문에 발생할 수 있습니다.         |
| insufficient-field-width            | 이 오류는 추가 또는 수정 내용을 개체로 내보낼 때와 특성 값이 열 너비를 초과할 때 고정 너비 텍스트 파일용 관리 에이전트에서 반환됩니다.   |
| insufficient-columns                | 이 오류는 추가 또는 수정 내용을 개체로 내보낼 때와 여러 값 특성의 값 수가 해당 특성의 여러 값에 대해 구성된 열 수를 초과할 때 고정 너비 및 구분된 텍스트 파일용 관리 에이전트에서 반환됩니다. |
| permission-issue                    | 이 오류는 관리 에이전트가 연결된 데이터 소스에 대해 작업을 수행하기 위한 충분한 권한이 없기 때문에 추가, 수정 또는 삭제 내용의 내보내기가 실행할 때 LDAP 및 Windows NT 4.0용 관리 에이전트에서 반환됩니다.     |
| dn-attributes-failure               | 이 오류는 추가 또는 수정 내용을 내보낼 때 해당하는 연결된 데이터 소스 개체가 없는 참조 값이 설정될 경우 Active Directory, Active Directory GAL(전체 주소 목록) 및 ADAM(Active Directory Application Mode)용 관리 에이전트에서 반환됩니다. 이 오류가 표시되면 커넥터 공간 개체 뷰어를 사용하여 내보내지지 못한 참조 특성 변경 내용을 확인합니다. |
| non-existent-parent                 | 이 오류는 연결된 데이터 소스에 부모 개체가 없기 때문에 추가 또는 이름 바꾸기 내용의 내보내기가 실패할 때 LDAP용 관리 에이전트에서 반환됩니다.       |
| code-page-conversion                | 이 오류는 변환 오류로 인해 FIM을 실행하는 서버 내에 유니코드로 저장된 특성 값을 내보내기 파일의 코드 페이지로 변환하지 못할 때 파일 기반 관리 에이전트에서 반환됩니다.    |
| no-export-to-this-object-type       | 이 오류는 컴퓨터 개체에 대해 프로비저닝 작업 또는 내보내기 특성 흐름을 수행하려고 할 때 Windows NT 4.0용 관리 에이전트에서 반환됩니다. 이 형식의 개체에는 내보내기 작업이 허용되지 않지만 이 형식의 개체에 대해서는 가져오기를 수행할 수 있습니다.  |
| missing-provisioning-attribute       | 이 오류는 새로 프로비전된 개체를 내보낼 때와 새 개체의 프로비전에 필요한 특정 특성이 규칙 확장에 의해 설정되지 않았을 때 Lotus Notes용 관리 에이전트에서 반환됩니다. |
| invalid-provisioning-attribute-value | 이 오류는 새로 프로비전된 개체를 내보낼 때와 규칙 확장에 의해 설정된 프로비전용 특정 특성이 올바르지 않을 때(예를 들어 특정 값 범위에 속하지 않을 경우) 반환됩니다.       |
|provision-to-secondary-nab                 | 이 오류는 보조 Lotus Notes 주소록에 사람 또는 인증자 개체를 프로비전하려고할 때 Lotus Notes용 관리 에이전트에서만 발생합니다. Lotus Notes는 보조 주소록에 연락처를 프로비전하는 것만 허용합니다.  |                                                           
| missing-anchor-component                   | 이 오류는 새로 프로비전된 개체를 내보내며, 앵커 생성에 필요한 값을 사용할 수 없기 때문에 앵커를 생성할 수 없을 때 반환됩니다. 가능한 원인은 프로비저닝 동안 특성이 설정되지 않았거나(즉, Sun 또는 Netscape 디렉터리 서버, 데이터베이스 및 파일 기반 관리 에이전트용 관리 에이전트), 자동 증분 열에서 앵커를 생성할 때 연결된 데이터 소스에서 읽을 수 없는 경우입니다(즉, Active Directory, Sun 및 Netscape 디렉터리 서버, 데이터베이스 관리 에이전트용 관리 에이전트). |
| multi-valued-anchor-component               | 이 오류는 앵커를 생성할 때 사용된 특성 중 하나에 여러 값이 있기 때문에 새로 프로비전된 개체에 대해 앵커를 생성할 수 없을 때 Sun 및 Netscape 디렉터리 서버용 관리 에이전트에서 생성됩니다. 앵커 생성에 사용되는 특성을 연결된 데이터 소스 스키마에서 다중 값으로 정의할 수 있지만 FIM에서는 실제 개체에 대해 단일 값만 가져야 합니다.                                                                                                                                                                           |
| anchor-too-long                           | 이 오류는 앵커 생성이 FIM에 대한 최대 크기 제한을 초과하는 앵커를 생성할 때 파일 기반 및 데이터베이스 관리 에이전트는 물론, Sun 및 Netscape 디렉터리 서버용 관리 에이전트에 의한 델타 가져오기 실행 동안 반환됩니다. 커넥터 공간의 단일 특성에 대한 앵커 값 최대 길이는 398자입니다. 여러 특성에서 앵커가 생성되는 경우 각 추가 특성에 대해 2자를 뺍니다. 예를 들어 3개 특성으로 생성된 앵커(sn+location+telephoneNumber)는 392자로 제한됩니다.                                    |
| invalid-attribute-value                    | 이 오류는 연결된 데이터 소스에 대해 유효하지 않은 문자가 포함된 특성 값을 전달하려고 하면 발생합니다. 예를 들어 고정 너비 텍스트 파일, 구분된 텍스트 파일 및 특성-값 쌍 텍스트 파일에 대해 관리 에이전트로 내보낸 특성 값은 CR, LF 또는 EOF 문자를 포함할 수 없습니다.       |
| encryption-key-lost          | 이 오류는 정상적인 작업의 일부로 발생하지 않아야 합니다. FIM이 개체를 로드할 때 커넥터 공간에 저장된 암호화된 특성 값을 해독할 수 없음을 의미합니다. FIM에서 사용되는 암호화 키 집합이 컴퓨터에 없음을 나타낼 수 있습니다. 이 오류는 Active Directory, Active Directory GAL(전체 주소 목록), Sun 및 Netscape 디렉터리 서버, Lotus Notes 및 Windows NT 4.0 등의 암호 특성을 포함하는 관리 에이전트에서 생성될 수 있습니다.    |
| locking-error-needs-retry                  | 이 오류는 여러 관리 에이전트가 동일한 커넥터 공간 개체를 동시에 동기화하려고 할 때만 발생해야 합니다. 이 오류가 발생하는 경우 내보내기를 한 번 더 실행해 보세요.     |
| cd-error                                  | 이 오류는 연결된 데이터 소스에 특수화된 오류 형식이 있을 때 반환됩니다. 이 오류는 \<cd-error\> 요소와 함께 표시되며 포함된 정보는 문제 해결에 도움이 됩니다.|
| unexpected-error                           | 변경 내용을 내보내려고 할 때 이 오류가 반환되며 작업은 실패를 유발합니다. 이 오류가 발생하면 이벤트 로그에서 문제 해결에 도우밍 되는 정보를 찾아보세요. |
| no-export-to-this-object-type              | 이 오류는 컴퓨터 개체에 대해 프로비저닝 작업 또는 내보내기 특성 흐름을 수행하려고 할 때 Windows NT 4.0용 관리 에이전트에서 반환됩니다. Windows NT 4.0용 관리 에이전트는 이 형식의 개체에 대한 내보내기 작업을 지원하지 않습니다.|  
| certifier-ou-not-configured                | 이 오류는 새 사용자 또는 컨테이너를 프로비전하려고 하며 \_MMS_Certifier 특성에 대해 지정한 인증자 이름이 제대로 구성된 인증자 컨테이너가 아닐 때 Lotus Notes용 관리 에이전트에서 반환됩니다. 각 인증자 컨테이너를 프로비전에 사용하려면 먼저 Synchronization Service Manager를 사용하여 구성해야 합니다. |
| temporary- certifier-file-creation-failure | 이 오류는 새 사용자 또는 컨테이너가 프로비전되고 인증자 파일을 만드는 프로세스가 어떤 이유(예: 디스크 공간 부족, 사용 권한 부족 등)로든 실패할 때 Lotus Notes용 관리 에이전트에서 반환됩니다. 인증자 파일을 만들기 위한 FIM 프로세스는 \_MMS_Certifier 특성으로 지정된 인증자 컨테이너에 대한 인증자 정보를 가져오고, Notes API에서 사용할 수 있게 Lotus Notes용 관리 에이전트의 MAData 폴더에 임시로 인증자 파일을 만듭니다.                                                                               |
| unexpected-provisioning-attribute           | 이 오류는 새로 프로비전된 개체를 내보내고 고객 확장에 의해 설정된 프로비저닝용 특정 특성이 다른 프로비저닝 특성 값과 호환되지 않아 포함되면 안될 경우에 Lotus Notes용 관리 에이전트에 의해 반환됩니다. 예를 들어 다음 경우에 이러한 오류가 표시될 수 있습니다.  <br/>&#176; 연락처(_MMS_IDRegType=0)를 만들고 \_MMS_Certifier, \_MMS_OU, \_MMS_Password, \_MMS_IDStoreType,    \_MMS_IDPath 또는 MailFile 특성 중 하나를 제공하는 경우  <br/>&#176; 미국 사용자 또는 국제 사용자를 만들 때 ID 파일 만들기(_MMS_IDStoreType=0)를 지정하지 않고 \_MMS_IDPath 또는 MailFile 특성을 제공하는 경우, OU(인증자)를 만들 때 \_MMS_OU 특성을 제공하는 경우  <br/>&#176; O(인증자)를 만들 때 \_MMS_Certifier 특성을 제공하는 경우
