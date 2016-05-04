---
# required metadata

title: MIM 2016 설치&#58; Active Directory와 MIM 서비스 동기화 | Microsoft Identity Manager
description: 관리 에이전트 및 MIM 동기화 서비스를 사용하여 Active Directory와 MIM 데이터베이스를 동기화합니다.
keywords:
author: kgremban
manager: stevenpo
ms.date: 04/28/2016
ms.topic: get-started-article
ms.prod: identity-manager-2015
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 5e532b67-64a6-4af6-a806-980a6c11a82d

# optional metadata

#ROBOTS:
#audience:
#ms.devlang:
ms.reviewer: mwahl
ms.suite: ems
#ms.tgt_pltfrm:
#ms.custom:

---

# MIM 2016 설치: Active Directory와 MIM 서비스 동기화

>[!div class="step-by-step"]
[« MIM 서비스 및 포털](install-mim-service-portal.md)

> [!NOTE]
> 아래의 모든 예제에서 **mimservername**은 도메인 컨트롤러의 이름을 나타내고 **contoso**는 도메인 이름을 나타내며 **Pass@word1**은 예제 암호를 나타냅니다.

기본적으로 MIM 동기화 서비스(동기화)에는 구성된 커넥터가 없습니다.  일반적인 첫 단계는 MIM 동기화를 사용하여 기존 Active Directory 계정으로 FIM 서비스 데이터베이스를 채우는 것입니다. 이를 위해 MIM 동기화 서비스 응용 프로그램을 사용합니다.

## MIM 관리 에이전트 만들기
MIM MA(관리 에이전트)는 MIM 서비스에 대한 MIM 동기화를 위한 커넥터입니다. 이 커넥터를 만들려면 관리 에이전트 만들기 마법사를 사용하면 됩니다.

MIM 관리 에이전트를 구성할 때 사용자 계정을 지정해야 합니다. 이 문서에서는 이 계정에 대한 이름으로 **MIMMA**를 사용합니다.

> [!CAUTION]
> MIM 관리 에이전트에 대해 사용하는 계정은 MIM 서비스를 설치하는 동안 지정한 계정과 동일해야 합니다.

###MIM MA를 만들려면

1.  동기화 서비스 관리자를 엽니다.

2.  관리 에이전트 만들기 마법사를 열려면 **작업** 메뉴에서 **만들기**를 클릭합니다.

3.  **관리 에이전트 만들기** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    -   관리 에이전트 대상: MIM 서비스 관리 에이전트

    -   이름: MIMMA

4.  **데이터베이스에 연결** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    -   서버: localhost

    -   데이터베이스: MIMService

    -   MIM 서비스 기준 주소: http://localhost:5725

    -   인증 모드: Windows 통합 인증

    -   사용자 이름: mimma

    -   암호: Pass@word

    -   도메인: contoso

5.  **선택한 개체 형식** 페이지에서 아래 나열된 개체 형식이 선택되었는지 확인하고 **다음**을 클릭합니다.

    -   ExpectedRuleEntry

    -   DetectedRuleEntry

    -   SynchronizationRule

    -   Person

    -   그룹

6.  **선택한 특성** 페이지에서 나열된 모든 특성이 선택되어 있는지 확인하고 **다음**을 클릭합니다.

7.  **커넥터 필터 구성** 페이지에서 **다음**을 클릭합니다.

8.  **개체 형식 매핑 구성** 페이지에서 다음 매핑을 추가하고 **다음**을 클릭합니다.

    -   **데이터 원본 개체 형식** 목록에서 **Person**을 선택합니다.

    -   매핑 대화 상자를 열려면 **매핑 추가**를 클릭합니다.

    -   **메타버스 개체 형식** 목록에서 **person**을 선택합니다.

    -   매핑 대화 상자를 닫으려면 **확인**을 클릭합니다.

9.  **특성 흐름 구성** 페이지에서 다음 특성 흐름 매핑을 적용하고 **다음**을 클릭합니다.

    ||||
    |-|-|-|
    | **흐름 방향** | **데이터 원본 특성** | **메타버스 특성** |
    |가져오기|가져오기|accountName|
    |가져오기|가져오기|company|
    |가져오기|가져오기|displayName|
    |가져오기|가져오기|employeeID|
    |가져오기|가져오기|employee유형|
    |가져오기|가져오기|firstName|
    |가져오기|가져오기|lastName|
    |가져오기|가져오기|Manager|
    |가져오기|가져오기|objectSid|
    |내보내기|내보내기|accountName|
    |내보내기|내보내기|company|
    |내보내기|내보내기|displayName|
    |내보내기|내보내기|도메인|
    |내보내기|내보내기|employeeID|
    |내보내기|내보내기|employee유형|
    |내보내기|내보내기|firstName|
    |내보내기|내보내기|lastName|
    |내보내기|내보내기|manager|
    |내보내기|내보내기|objectSid|

10.  데이터 원본 개체 형식으로 **Person**을 선택합니다.

    -   Select **Person** as the Metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the previous table, complete the following steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    -   Select **Group** as the data source type and as the metaverse object type.

    -   Select **Direct** as the Mapping Type.

    -   For each row in the following table, complete these steps:

        -   Select the **Flow direction** shown for that row in the table.

        -   Select the **Data source attribute** shown for that row in the table.

        -   Select the **Metaverse attribute** shown for that row in the table.

        -   To apply the flow mapping, click **New**.

    | Flow Direction | Data Source Attribute | Metaverse Attribute |
    |-|-|-|
    | Export | AccountName | accountName |
    | Export | DisplayName | displayName |
    | Export | Domain | domain |
    | Export | Scope | scope |
    | Export | Type | type |
    | Export | Member | member |
    | Export | MembershipLocked | membershipLocked |
    | Export | MembershipAddWorkflow | membershipAddWorkflow |
    | Export | Manager | manager |

11.  **프로비전 해제 구성** 페이지에서 **다음**을 클릭합니다.

12.  관리 에이전트를 만들려면 **확장 구성** 페이지에서 **마침**을 클릭합니다.

## AD 관리 에이전트 만들기
Active Directory 관리 에이전트는 AD 도메인 서비스용 커넥터입니다. 이 커넥터를 만들려면 관리 에이전트 만들기 마법사를 사용하면 됩니다.

1. 관리 에이전트 만들기 마법사를 열려면 **작업** 메뉴에서 **만들기**를 클릭합니다.

2. **관리 에이전트 만들기** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    - 관리 에이전트 대상: Active Directory 도메인 서비스
    - 이름: ADMA

3. **Active Directory 포리스트에 연결** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    - 포리스트 이름: contoso.local
    - 사용자 이름: administrator
    - 암호: &lt;계정의 암호&gt;
    - 도메인: contoso

4. **디렉터리 파티션 구성** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    - **디렉터리 파티션 선택** 목록에서 **DC=CONTOSO, DC=local**을 선택합니다.

    - 컨테이너 선택 대화 상자를 열려면 **컨테이너**를 클릭합니다.

    - 특정 컨테이너에 MIM 관리 개체만 있도록 컨테이너를 변경하려면 **DC=CONTOSO,DC=local** 노드를 클릭한 다음 관심 있는 컨테이너의 노드를 클릭합니다.

    - 컨테이너 선택 대화 상자를 닫으려면 **확인**을 클릭합니다.

5. **프로비전 계층 구성** 페이지에서 **다음**을 클릭합니다.

6. **개체 형식 선택** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    - **개체 형식** 목록에서 **사용자** 및 **그룹**을 선택합니다.

7. **특성 선택** 페이지에서 다음과 같이 설정하고 **다음**을 클릭합니다.

    - **모두 표시**를 선택합니다.

8. **특성** 목록에서 다음 특성을 선택합니다.

    -   company
    -   displayName
    -   employeeID
    -   employee유형
    -   givenName
    -   groupType
    -   manager
    -   managedBy
    -   멤버
    -   objectSid
    -   sAMAccountName
    -   sAMAccount유형
    -   sn
    -   unicodePwd
    -   userAccountControl

9. **커넥터 필터 구성** 페이지에서 **다음**을 클릭합니다.

10. **조인 및 프로젝션 규칙 구성** 페이지에서 **다음**을 클릭합니다.

11. **특성 흐름 구성** 페이지에서 **다음**을 클릭합니다.

12. **프로비전 해제 구성** 페이지에서 **다음**을 클릭합니다.

13. **확장 구성** 페이지에서 **마침**을 클릭합니다.


## 실행 프로필 만들기

ADMA 및 MIMMA 커넥터에 대해 실행 프로필을 만듭니다.

### ADMA 커넥터에 대해 실행 프로필을 만듭니다.

다음 표에는 ADMA 커넥터에 대해 만들 다섯 가지 실행 프로필이 나와 있습니다.

| Name | 유형 |
| ---- | ---- |
| Profile1 | 전체 가져오기(스테이지 전용) |
| Profile2 | 전체 동기화 |
| Profile3 | 델타 가져오기(스테이지 전용) |
| Profile4 | 델타 동기화 |
| Profile5 | 내보내기 |

ADMA 커넥터에 대해 실행 프로필을 만들려면

1. 동기화 서비스 관리자를 열고 **도구** 메뉴에서 **관리 에이전트**를 클릭합니다.

2. **관리 에이전트** 목록에서 **ADMA**를 선택합니다.

3. 실행 프로필 구성 대화 상자를 열려면 **작업** 메뉴에서 **실행 프로필 구성**을 클릭합니다.

4. 표의 각 실행 프로필에 대해 다음 단계를 완료합니다.

    - 실행 프로필 구성 마법사를 열려면 **새 프로필**을 클릭합니다.

    - **이름** 상자에서 표에 표시된 프로필 이름을 입력하고 **다음**을 클릭합니다.

    - **형식** 목록에서 표에 표시된 단계 형식을 선택하고 **다음**을 클릭합니다.

    - **마침**을 클릭하여 실행 프로필을 만듭니다.

5. 실행 프로필 구성 대화 상자를 닫으려면 **확인**을 클릭합니다.

### MIMMA 커넥터에 대해 실행 프로필 만들기

다음 표에는 MIMMA 커넥터에 대한 일치하는 다섯 가지 실행 프로필이 나와 있습니다.

| Name | 유형 |
| -------- | -------- |
| Profile1 | 전체 가져오기(스테이지 전용) |
| Profile2 | 전체 동기화 |
| Profile3 | 델타 가져오기(스테이지 전용) |
| Profile4 | 델타 동기화 |
| Profile5 | 내보내기 |

MIMMA 커넥터에 대해 실행 프로필을 만들려면

1. 동기화 서비스 관리자를 열고 **도구** 메뉴에서 **관리 에이전트**를 클릭합니다.

2. **관리 에이전트** 목록에서 **MIMMA**를 선택합니다.

3. 실행 프로필 구성 대화 상자를 열려면 **작업** 메뉴에서 **실행 프로필 구성**을 클릭합니다.

4. 표의 각 실행 프로필에 대해 다음 단계를 완료합니다.

    - 실행 프로필 구성 마법사를 열려면 **새 프로필**을 클릭합니다.

    - **이름** 상자에서 표에 표시된 프로필 이름을 입력하고 **다음**을 클릭합니다.

    - **형식** 목록에서 표에 표시된 단계 형식을 선택하고 **다음**을 클릭합니다.

    - **마침**을 클릭하여 실행 프로필을 만듭니다.

5. 실행 프로필 구성 대화 상자를 닫으려면 **확인**을 클릭합니다.

## MIM 서비스 구성

MIM 포털을 사용하여 MIM 서비스에 대한 AD 사용자 인바운드 동기화 규칙을 만듭니다.

AD 사용자 인바운드 동기화 규칙을 만들려면

1. MIM 포털 홈 페이지의 탐색 모음에서 **관리**를 클릭합니다.

2. 동기화 규칙 페이지를 열려면 **동기화 규칙**을 클릭합니다.

3. 동기화 규칙 만들기 마법사를 열려면 도구 모음에서 **새로 만들기**를 클릭합니다.

4. **일반** 탭에서 다음 정보를 입력한 후 **다음**을 클릭합니다.

    -   표시 이름: AD 사용자 인바운드 동기화 규칙
    -   데이터 흐름 방향: 인바운드

5. **범위** 탭에서 다음 정보를 입력하고 **다음**을 클릭합니다.

    -   메타버스 리소스 종류: person
    -   외부 시스템: ADMA
    -   외부 시스템 리소스 종류: person

6. **관계** 탭에서 다음 정보를 입력하고 **다음**을 클릭합니다.

    -   관계 조건을 구성하려면 MetaverseObject:person(특성) 목록 및 ConnectedSystemObject:person(특성) 목록에서 **ObjectSID**를 선택합니다.

    -   **MIM에서 리소스 만들기**를 선택합니다.

7. **인바운드 특성 흐름** 페이지에서 다음 정보를 입력하고 **다음**을 클릭합니다.

    | 흐름 규칙 | 원본 | 대상 |
    |-|-|-|
    |규칙 1|samAccountName|f|
    |규칙 2|displayName|displayName|
    |규칙 3|Employee유형|Employee유형|
    |규칙 4|givenName|givenName|
    |규칙 5|sn|lastName|
    |규칙 6|Manager|manager|
    |규칙 7|objectSID|ObjectSID|
    |규칙 8|"Contoso"|도메인|

    이 표의 각 행에 대해 다음 단계를 수행합니다.

    -   흐름 정의 대화 상자를 열려면 **새 특성 흐름**을 클릭합니다.

    -   **원본** 탭에서 표의 해당 행에 대해 표시된 특성을 선택합니다.

    -   **대상** 탭에서 표의 해당 행에 대해 표시된 특성을 선택합니다.

    -   특성 흐름 구성을 적용하려면 **확인**을 클릭합니다.

8. **요약** 탭에서 **제출**을 클릭합니다.

## 테스트 환경 초기화
AD 데이터를 사용하여 구성을 테스트하려면 먼저 구성을 초기화해야 합니다. 이 프로세스에는 다음과 같은 네 가지 단계가 있습니다.

### 프로비전 사용

1. 동기화 서비스 관리자를 엽니다.

2. 옵션 대화 상자를 열려면 **도구** 메뉴에서 **옵션**을 클릭합니다.

3. **동기화 규칙 프로비전 사용**을 선택합니다.

4. 옵션 대화 상자를 닫으려면 **확인**을 클릭합니다.

### MIMMA 초기화

이 커넥터에서 전체 동기화 주기를 실행합니다. 전체 주기는 다음 실행 프로필로 구성됩니다.

    -   Full Import

    -   Full Synchronization

    -   Export

    -   Delta Import

1. 동기화 서비스 관리자를 열고 **도구** 메뉴에서 **관리 에이전트**를 클릭합니다.

2. **관리 에이전트** 목록에서 **MIMMA**를 선택합니다.

3. 관리 에이전트 실행 대화 상자를 열려면 **작업** 메뉴에서 **실행**을 클릭합니다.

4. 위에 나열된 각 실행 프로필에 대해 다음 단계를 완료합니다.

    - 관리 에이전트 실행 대화 상자를 열려면 **작업** 메뉴에서 **실행**을 클릭합니다.

    - **실행 프로필** 목록에서 실행할 실행 프로필을 선택합니다.

    - 실행 프로필을 시작하려면 **확인**을 클릭합니다.

#### 특성 흐름 우선 순위 구성

MIM 커넥터를 초기화하는 동안 구성된 동기화 규칙을 메타버스로 가져왔습니다.

이 커넥터에서 제공한 특성의 특성 흐름 우선 순위를 조정하여 AD에 이미 있는 해당 특성이 메타버스로 이동하고, 이후에 MIM 서비스 데이터베이스로도 이동할 수 있도록 합니다.

### ADMA 초기화

Active Directory 커넥터를 초기화하려면 전체 가져오기 및 전체 동기화를 실행해야 합니다. 전체 가져오기는 AD의 기존 개체를 커넥터 공간으로 가져오는 데 필요합니다. 전체 동기화는 MIM 커넥터 공간의 새 동기화 규칙을 메타버스로 프로젝팅함으로써 동기화 규칙이 변경되었기 때문에 필요합니다. 다음 작업을 수행합니다.

    -   Full Import

    -   Full Synchronization

1. 동기화 서비스 관리자를 열고 **도구** 메뉴에서 **관리 에이전트**를 클릭합니다.

2. **관리 에이전트** 목록에서 **ADMA**를 선택합니다.

3. 관리 에이전트 실행 대화 상자를 열려면 **작업** 메뉴에서 **실행**을 클릭합니다.

4. 위에 나열된 각 실행 프로필에 대해 다음 단계를 완료합니다.

    - 관리 에이전트 실행 대화 상자를 열려면 **작업** 메뉴에서 **실행**을 클릭합니다.

    - **실행 프로필** 목록에서 실행할 실행 프로필을 선택합니다.

    - 실행 프로필을 시작하려면 **확인**을 클릭합니다.

### MIM 서비스 데이터베이스 채우기

MIM 서비스 데이터베이스를 개체로 채우려면 MIMMA 커넥터에서 동기화 주기를 실행해야 합니다. 주기는 다음 실행 프로필 실행으로 구성됩니다.

    -   Export

    -   Full Import

    -   Full Sync

    1. Open the Synchronization Service Manager and, on the **Tools** menu, click **Management Agents**.

    2. In the **Management Agents** list, select **MIMMA**.

    3. To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

    4. For each run profile listed above, complete the following steps:

        - To open the Run Management Agent dialog box, on the **Actions** menu, click **Run**.

        - In the **Run profiles** list, select the run profile you want to run.

        - To start the run profile, click **OK**.

>[!div class="step-by-step"]
[« MIM 서비스 및 포털](install-mim-service-portal.md)


<!--HONumber=Apr16_HO2-->

