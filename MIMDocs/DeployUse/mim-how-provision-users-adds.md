---
title: "Microsoft Identity Manager 2016 | Microsoft 문서"
description: "Microsoft Identity Manager 2016을 사용하여 ADDS에 사용자를 만드는 프로세스 이해"
keywords: 
author: barclayn
ms.author: barclayn
manager: mbaldwin
ms.date: 05/11/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.translationtype: Human Translation
ms.sourcegitcommit: 1ef7b9816d265d17ef68fc54e010e655535dcdc8
ms.openlocfilehash: 5ab70bac8cbd874153fa56cf7b181144c394ec04
ms.contentlocale: ko-kr
ms.lasthandoff: 05/11/2017



---




# <a name="how-do-i-provision-users-to-ad-ds"></a>How Do I Provision Users to AD DS(AD DS로 사용자를 프로비전하는 방법)

적용 대상: Microsoft Identity Manager 2016 SP1(MIM)

ID 관리 시스템에 대한 기본 요구 사항 중 하나는 외부 시스템에 리소스를 프로비전하는 기능입니다.

이 가이드는 Active Directory® 도메인 서비스(AD DS)에서 Microsoft® Identity Manager(MIM) 2016로 사용자를 프로비전하는 과정과 관련된 주요 문서 블록에 대해 설명하고, 시나리오가 예상대로 작동하는지 확인하는 방법을 소개하며, MIM 2016을 사용하여 Active Directory 사용자를 관리하기 위한 제안과 추가 정보를 얻을 수 있는 목록을 제공합니다.

## <a name="before-you-begin"></a>시작하기 전 주의 사항


이 섹션에서는 이 문서의 범위에 대해 알아볼 수 있습니다. 일반적으로 "어떻게 해야 하나요?" 질문에 대한 가이드는 관련 [시작 가이드](http://go.microsoft.com/FWLink/p/?LinkId=190486)에서 다룬 MIM과 개체를 동기화하는 프로세스를 기본적으로 수행한 독자를 대상으로 합니다.

### <a name="audience"></a>대상 그룹


이 가이드는 이미 MIM 동기화 프로세스가 어떻게 작동하는지에 대해 기본적인 이해를 하고 있으며 특정 시나리오에 대한 실제 경험과 개념 정보를 얻는 데 관심이 있는 정보 기술(IT) 전문가를 대상으로 합니다.

### <a name="prerequisite-knowledge"></a>필수 정보


이 문서에서는 실행 중인 MIM의 인스턴스에 액세스할 수 있고 다음 문서에 설명된 대로 간단한 동기화 시나리오를 구성해본 경험이 있다고 가정합니다.

-   [Introduction to Inbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189652)(인바운드 동기화 소개)

-   [Introduction to Outbound Synchronization](http://go.microsoft.com/FWLink/p/?LinkId=189653)(아웃바운드 동기화 소개)

이 문서의 콘텐츠는 이러한 소개 문서에 대한 확장 기능을 다루고 있습니다.

### <a name="scope"></a>범위


이 문서에서 설명하는 시나리오는 기본 랩 환경의 요구 사항에 대응하기 위하여 단순화되었습니다. 논의되는 개념과 기술을 이해하는 것이 중요합니다.

이 문서는 MIM을 사용하여 AD DS의 그룹을 관리하는 것과 관련된 솔루션을 개발하는 데 도움이 됩니다.

### <a name="time-requirements"></a>시간 요구 사항


이 문서에서 제시하는 절차를 완료하려면 90~120분이 소요됩니다.

이 예상 시간은 테스트 환경이 이미 구성되어 있다고 가정하며 테스트 환경을 설정하는 데 필요한 시간은 포함되지 않았습니다.

### <a name="getting-support"></a>지원 받기


이 문서의 내용과 관련하여 질문이 있거나 궁금한 점이 있거나 논의되길 원하는 일반적인 피드백을 제공하려면 [Forefront Identity Manager 2010 포럼](http://go.microsoft.com/FWLink/p/?LinkId=189654)에 메시지를 게시해 주세요.

## <a name="scenario-description"></a>시나리오 설명


가상 회사인 Fabrikam에서는 회사의 AD DS에서 MIM을 사용하여 사용자 계정을 관리할 계획입니다. 이 프로세스의 일부로 Fabrikam은 AD DS에 사용자를 프로비전해야 합니다. Fabrikam에서는 초기 테스트를 시작하기 위해 MIM 및 AD DS로 구성된 기본 랩 환경을 설치했습니다.
이 랩 환경에서 Fabrikam은 MIM 포털에서 수동으로 만든 사용자로 구성된 시나리오를 테스트합니다. 이 시나리오의 목표는 AD DS에 미리 정의된 암호를 사용하여 사용자를 활성화된 사용자로 프로비전하는 것입니다.

## <a name="scenario-design"></a>시나리오 디자인


이 가이드를 사용하려면 세 가지 아키텍처 구성 요소가 필요합니다.

-   Active Directory 도메인 컨트롤러

-   FIM Synchronization Service를 실행하는 컴퓨터
-   FIM 포털을 실행하는 컴퓨터

다음 그림은 필수 환경을 간략하게 설명합니다.

![필수 환경](media/how-provision-users-adds/image001.png)


모든 구성 요소를 하나의 컴퓨터에서 실행할 수 있습니다.

>[!NOTE]
MIM 설정에 대한 자세한 내용은 [FIM Installation Guide](http://go.microsoft.com/FWLink/p/?LinkId=165845)(FIM 설치 가이드)를 참조하세요.

## <a name="scenario-components-list"></a>시나리오 구성 요소 목록


다음 표에는 이 가이드의 시나리오에 포함된 구성 요소가 나와 있습니다.

| ![조직 구성 단위](media/how-provision-users-adds/image005.jpg)   | 조직 구성 단위                | MIM 개체 – 프로비전된 사용자를 대상으로 사용되는 조직 구성 단위(OU)                                                       |
|----------------------------------------|------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------|
| ![사용자 계정](media/how-provision-users-adds/image006.jpg)   | 사용자 계정                      | &#183; **ADMA** – AD DS에 연결할 수 있는 충분한 권한을 보유한 Active Directory 사용자 계정<br/> &#183; **FIMMA** - MIM에 연결할 수 있는 충분한 권한을 보유한 Active Directory 사용자 계정
                                                                 |
| ![관리 에이전트 및 실행 프로필](media/how-provision-users-adds/image007.jpg)  | 관리 에이전트 및 실행 프로필 | &#183; **Fabrikam ADMA** – AD DS를 사용하여 데이터를 교환하는 관리 에이전트 <br/> &#183; Fabrikam FIMMA - MIM을 사용하여 데이터를 교환하는 관리 에이전트                                                                                 |
| ![동기화 규칙](media/how-provision-users-adds/image008.jpg)  | 동기화 규칙              | Fabrikam 그룹 아웃바운드 동기화 규칙 – AD DS에 사용자를 프로비전하는 아웃바운드 동기화 규칙                                     |
| ![설정](media/how-provision-users-adds/image009.jpg)   | 설정                               | 모든 계약자 – 계약자 EmployeeType 특성 값을 사용하여 모든 개체에 대한 동적 멤버 자격을 설정                                |
| ![워크플로](media/how-provision-users-adds/image010.jpg)  | 워크플로                          | AD 프로비전 워크플로 - AD 아웃바운드 동기화 규칙의 범위에 MIM 사용자를 가져오는 워크플로                                |
| ![관리 정책 규칙](media/how-provision-users-adds/image011.jpg)   | 관리 정책 규칙            | AD 프로비전 관리 정책 규칙 - 리소스가 모든 계약자 집합의 구성원이 될 때 트리거되는 관리 정책 규칙(MPR) |
| ![MIM 사용자](media/how-provision-users-adds/image012.jpg) | MIM 사용자                          | Britta Simon - AD DS에 프로비전하는 MIM 사용자                                                                                             |



## <a name="scenario-steps"></a>시나리오 단계


이 가이드에 설명된 시나리오는 다음 그림과 같은 구성 요소로 이루어져 있습니다.

![시나리오 단계](media/how-provision-users-adds/image013.png)


## <a name="configuring-the-external-systems"></a>외부 시스템 구성


이 섹션에서는 MIM 환경의 외부에서 생성해야 하는 리소스에 대한 지침을 제공합니다.

### <a name="step-1-create-the-ou"></a>1단계: OU 만들기


프로비전된 샘플 사용자에 대한 컨테이너로서 OU가 필요합니다. OU를 만드는 방법에 대한 자세한 내용은 [Create a New Organizational Unit](http://go.microsoft.com/FWLink/p/?LinkId=189655)(새 OU(조직 구성 단위) 만들기)을 참조하세요.

AD DS에 MIMObjects가 호출된 OU를 만듭니다.

### <a name="step-2-create-the-active-directory-user-accounts"></a>2단계: Active Directory 사용자 계정 만들기

이 가이드의 시나리오에서는 두 개의 Active Directory 사용자 계정이 필요합니다.

- **ADMA** - Active Directory 관리 에이전트가 사용

- **FIMMA** – FIM 서비스 관리 에이전트가 사용

두 경우 모두 일반 사용자 계정을 만드는 데 충분합니다. 두 계정의 특정 요구 사항에 대한 자세한 내용은 이 문서의 뒷부분에 나와 있습니다. 사용자를 만드는 방법에 대한 자세한 내용은 [Create a New User Account](http://go.microsoft.com/FWLink/p/?LinkId=189656)(새 사용자 계정 만들기)를 참조하세요.


## <a name="configuring-the-fim-synchronization-service"></a>FIM Synchronization Service 구성


이 섹션의 구성 단계에서는 FIM Synchronization Service Manager를 시작해야 합니다.

### <a name="creating-the-management-agents"></a>관리 에이전트 만들기

이 가이드의 시나리오에서는 두 관리 에이전트를 만들어야 합니다.

-   **Fabrikam ADMA** – AD DS에 대한 관리 에이전트

-   **Fabrikam FIMMA** – FIM 서비스 관리 에이전트용 관리 에이전트

### <a name="step-3-create-the-fabrikam-adma-management-agent"></a>3단계: Fabrikam ADMA 관리 에이전트 만들기

AD DS용 관리 에이전트를 구성하는 경우 AD DS와의 데이터 교환에서 관리 에이전트가 사용하는 계정을 지정해야 하며, 일반 사용자 계정을 사용해야 합니다. 그러나 AD DS에서 데이터를 가져오려면 계정에 DirSync 컨트롤의 변경 내용을 폴링할 수 있는 권한이 있어야 합니다. 관리 에이전트가 AD DS로 데이터를 내보내도록 하려면 대상 OU에 대한 충분한 권한을 계정에 부여해야 합니다. 이 항목에 대한 자세한 내용은 [Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657)(ADMA 계정 구성)를 참조하세요.

AD DS에서 사용자를 만들려면 개체의 DN을 포함해야 합니다. 또한, 성, 이름, 표시 이름을 포함하여 개체를 찾을 수 있도록 하는 것이 좋습니다.

AD DS에서는 여전히 사용자가 sAMAccountName 특성을 사용하여 디렉터리 서비스에 로그온하는 경우가 많습니다. 이 특성에 대한 값을 지정하지 않으면 디렉터리 서비스에서 임의의 값을 생성합니다. 그러나 사용자에게 이러한 임의 값은 친숙하지 않으며, 사용자에게 친숙한 이 특성의 버전은 AD DS에 내보내기의 일부인 경우가 많습니다. 사용자가 AD DS에 로그온할 수 있도록 하려면 내보내기 논리에 unicodePwd 특성을 사용하여 만든 암호가 포함되도록 해야 합니다.

>[!Note]                                
UnicodePwd로 지정한 값이 대상 AD DS의 암호 정책을 준수하는지 확인해야 합니다.

AD DS 계정에 대한 암호를 설정하는 경우, 활성화된 계정으로 계정을 만들어야 합니다. userAccountControl 특성을 설정하여 이 작업을 수행합니다. userAccountControl 특성에 대한 자세한 내용은 [Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189658)(Active Directory에서 FIM을 사용하여 계정 활성화 또는 비활성화)를 참조하세요.

다음 표에는 구성해야 할 가장 중요한 시나리오별 설정이 나와 있습니다.

| 관리 에이전트 디자이너 페이지                          | 구성                                                  |
|---------------------------------------------------------|----------------------------------------------------------------|
| 관리 에이전트 만들기                                 | 1. **관리 에이전트 대상:** AD DS  <br/> 2.  **이름:** Fabrikam ADMA |
| Active Directory 포리스트에 연결                      | 1. **디렉터리 파티션 선택:** "DC = Fabrikam,DC = com"   <br/>   2. **컨테이너**를 클릭하여 **컨테이너 선택** 대화 상자를 열고 **MIMObjects**만이 선택된 OU인지 확인합니다.        |
| 개체 유형 선택                                     | 이미 선택한 개체 유형과 더불어 **사용자**를 선택합니다. |
| 특성 선택                                       | 1. **모두 표시**를 클릭합니다. <br/>   2. 다음 특성을 선택합니다. <br/> &nbsp;&nbsp;&nbsp;&#176; **displayName** <br/> &nbsp;&nbsp;&nbsp;&#176; **givenName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **sn** <br/> &nbsp;&nbsp;&nbsp;&#176;  **SamAccountName** <br/> &nbsp;&nbsp;&nbsp;&#176;  **unicodePwd** <br/> &nbsp;&nbsp;&nbsp;&#176;  **userAccountControl**     

자세한 내용은 도움말의 다음 항목을 참조하세요.
- 관리 에이전트 만들기
- Active Directory 포리스트에 연결
- Active Directory용 관리 에이전트 사용
- 디렉터리 파티션 구성

>[!Note]
ExpectedRulesList 특성에 대해 구성된 특성 흐름 규칙 가져오기가 구성되어 있는지 확인합니다.

### <a name="step-4-create-the-fabrikam-fimma-management-agent"></a>4단계: Fabrikam FIMMA 관리 에이전트 만들기

FIM 서비스 관리 에이전트를 구성하는 경우 FIM 서비스와의 데이터 교환에서 관리 에이전트에서 사용하는 계정을 지정해야 합니다.

일반 사용자 계정을 사용해야 합니다. 계정은 MIM을 설치하는 동안 지정한 계정과 같아야 합니다. 설치하는 동안 지정한 FIMMA 계정의 이름을 확인하고 이 계정이 여전히 유효한지 여부를 테스트하는 데 사용할 수 있는 스크립트는 [FIM MA Account Configuration Quick Test](http://go.microsoft.com/FWLink/p/?LinkId=189659)(FIM MA 계정 구성 빠른 테스트)를 위한 Windows PowerShell 사용을 참조하세요.

다음 표에는 구성해야 할 가장 중요한 시나리오별 설정이 나와 있습니다. 아래 표에서 제시하는 정보를 기반으로 하는 관리 에이전트를 만듭니다.  

| 관리 에이전트 디자이너 페이지 | 구성 |
|------------|------------------------------------|
| 관리 에이전트 만들기 | 1. **관리 에이전트 대상:** FIM 서비스 관리 에이전트 <br/> 2. **이름** Fabrikam FIMMA |
| 데이터베이스에 연결     | 다음 설정을 사용합니다. <br/> &#183; **서버:** localhost <br/> &#183; **데이터베이스:** FIMService <br/> &#183; **FIM 서비스 기준 주소:** http://localhost:5725 <br/> <br/> 이 관리 에이전트를 위해 만든 계정에 대한 정보 제공 |
| 개체 유형 선택                                     | 이미 선택한 개체 유형과 더불어 **사람**을 선택   |
| 개체 형식 매핑 구성                          | 기존 개체 형식 매핑 외에도 **데이터 원본 개체 형식** 사람에 대한 매핑을 **메타버스** 개체 유형 사람에 추가합니다. |
| 특성 흐름 구성                                | 기존 특성 흐름 매핑과 더불어 다음 특성 흐름 매핑을 추가합니다. <br/><br/> ![특성 흐름](media/how-provision-users-adds/image018.jpg) |




자세한 내용은 도움말의 다음 항목을 참조하세요.
-   관리 에이전트 만들기

-   Active Directory 데이터베이스에 연결

-   Active Directory용 관리 에이전트 사용

-   디렉터리 파티션 구성

>[!NOTE]
 ExpectedRulesList 특성에 대해 구성된 특성 흐름 규칙 가져오기가 구성되어 있는지 확인합니다.

### <a name="step-5-create-the-run-profiles"></a>5단계: 실행 프로필 만들기

다음 표에는 이 가이드의 시나리오에 필요한 실행 프로필이 나와 있습니다.

| 관리 에이전트  | 실행 프로필     |
|-------------------|--------------------------------------|
| Fabrikam ADMA     | 1. 전체 가져오기  <br/> 2. 전체 동기화 <br/> 3. 델타 가져오기 <br/> 4. 델타 동기화 <br/> 5. 내보내기                                                                    |
| Fabrikam FIMMA   | 1. 전체 가져오기 <br> 2. 전체 동기화 <br/> 3. 델타 가져오기 <br/> 4. 델타 동기화 <br/> 5. 내보내기|                                                                                                                                                                                   

이전 표에 따라 각 관리 에이전트에 대한 실행 프로필을 만듭니다.


>[!Note]
자세한 내용은 MIM 도움말의 관리 에이전트 실행 프로필 만들기를 참조하세요.                                                                                                                  


>[!Important]
 사용자 환경에서 프로비전이 활성화되어 있는지 확인합니다. Using Windows PowerShell to Enable Provisioning(Windows PowerShell을 사용한 프로비전 활성화)(http://go.microsoft.com/FWLink/p/?LinkId=189660) 스크립트를 실행하여 이 작업을 수행할 수 있습니다.


## <a name="configuring-the-fim-service"></a>FIM 서비스 구성


이 가이드의 시나리오에서는 다음 그림에 나와 있는 것처럼 프로비전 정책을 구성해야 합니다.

![프로비저닝 정책](media/how-provision-users-adds/image019.png)

이 프로비전 정책의 목표는 그룹을 AD 사용자 아웃바운드 동기화 규칙의 범위에 포함시키는 것입니다. 동기화 규칙의 범위에 리소스를 가져오면 구성에 따라 동기화 엔진을 활성화하여 AD DS에 리소스를 프로비전할 수 있습니다.

FIM 서비스를 구성하려면 Windows Internet Explorer®에서 http://localhost/identitymanagement로 이동합니다. MIM 포털 페이지에서 프로비전 정책을 만들려면 관리 섹션에서 관련 페이지로 이동합니다. 구성을 확인하려면 [Using Windows PowerShell to document your provisioning policy configuration](http://go.microsoft.com/FWLink/p/?LinkId=189661)(Windows PowerShell을 사용하여 프로비전 정책 구성 문서화)의 스크립트를 실행해야 합니다.

### <a name="step-6-create-the-synchronization-rule"></a>6단계: 동기화 규칙 만들기

다음 표는 필요한 Fabrikam 프로비전 동기화 규칙의 구성을 보여 줍니다. 다음 표의 데이터에 따라 동기화 규칙을 만듭니다.

| 동기화 규칙 구성                                                                         |                                                                             |                                                           
|------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------|-----------------------------------------------------------|
| Name                                                                                                       | Active Directory 사용자 아웃바운드 동기화 규칙                         |                                                          
| 설명                                                                                               |                                                                             |                                                           
| 우선 순위                                                                                                | 2                                                                           |                                                           
| 데이터 흐름 방향   | 아웃바운드             |       
| 종속성       |         |                                         


| 범위 |                                                                             |                                                           
|--------|-------|
| 메타버스 리소스 유형 | 개인 |                                                         
| 외부 시스템                   |Fabrikam ADMA                                                               |                                                       
| 외부 시스템 리소스 유형                                                                              | 사용자      



| Relationship ||
|------------|---------|
| 외부 시스템에서 리소스 만들기                                                                         | True                                                                        |                                                           
| 프로비전 해제 사용                                                                                      | False                                                                       |                                                           

| 관계 조건                                                                                      | |
|------------|----------|
| ILM 특성     | 데이터 원본 특성                                                       |
| 데이터 원본 특성         | sAMAccountName    |

| 초기 아웃바운드 특성 흐름        | |                                                             |
|-------------------|---------------------- |---------------|
| null 허용                 | 대상                                                                 | 원본                                                    |
| false                       | dn                                                                          | \+("CN=",displayName,",OU=MIMObjects,DC=fabrikam,DC=com") |
| false                       | userAccountControl                                                          | **상수:** 512                                         |
| false                                                                     | unicodePwd                    | 상수: P\@\$\$W0rd                                    |

| 영구 아웃바운드 특성 흐름  |                                                                     |                                                           |
|--------------------------------------|---------------------------------------------------------------------|-----------------------------------------------------------|
| null 허용                                                                                                | 대상                                                                 | 원본                                                    |
| false                                                                                                      | sAMAccountName                                                              | accountName                                               |
| false                                                                                                      | displayName                                                                 | displayName                                               |
| false                                                                                                      | givenName                                                                   | firstName                                                 |
| false                                                                                                      | sn                                                                          | lastName                                                  |



 >[!NOTE]
 DN을 대상으로 하는 특성 흐름에 대해 초기 흐름만을 선택했는지 확인합니다.                                                                          

### <a name="step-7-create-the-workflow"></a>단계 7: 워크플로 만들기

AD 프로비전 워크플로의 목표는 Fabrikam 프로비전 동기화 규칙을 리소스에 추가하는 것입니다. 다음 표에서 구성을 보여 줍니다.  아래 표의 데이터에 따라 워크플로를 만듭니다.

| 워크플로 구성               |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Active Directory 사용자 프로비저닝 워크플로                     |
| 설명                          |                                                                 |
| 워크플로 유형                        | 작업                                                          |
| 정책 업데이트 시 실행                 | False                                                           |

| 동기화 규칙                 |                                                                 |
|--------------------------------------|-----------------------------------------------------------------|
| Name                                 | Active Directory 사용자 아웃바운드 동기화 규칙             |
| 작업                               | 추가                                                             |




### <a name="step-8-create-the-mpr"></a>8단계: MPR 만들기

필수 MPR은 전환 설정 유형이며, 리소스가 모든 계약자 집합의 구성원이 되면 트리거됩니다. 다음 표에서 구성을 보여 줍니다.  아래 표의 데이터에 따라 MPR을 만듭니다.

| MPR 구성                    |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| Name                                 | AD 사용자 프로비저닝 관리 정책 규칙                 |
| 설명                          |                                                             |
| 유형                                 | 전환 설정                                              |
| 권한 부여                   | False                                                       |
| Disabled                             | False                                                       |

| 전환 정의                |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 전환 유형                      | 전환 위치                                               |
| 전환 집합                       | 모든 계약자                                             |

| 정책 워크플로                     |                                                             |
|--------------------------------------|-------------------------------------------------------------|
| 유형                                 | 작업                                                      |
| 표시 이름                         | Active Directory 사용자 프로비저닝 워크플로                 |




## <a name="initializing-your-environment"></a>환경 초기화


초기화 단계의 목표는 다음과 같습니다.

-   동기화 규칙을 메타버스로 가져옵니다.

-   Active Directory 커넥터 공간에 Active Directory 구조를 가져옵니다.

### <a name="step-9-run-the-run-profiles"></a>9단계: 실행 프로필 실행

다음 표에서 초기화 단계에 포함된 실행 프로필 목록을 보여 줍니다.  아래의 표에 따라 실행 프로필을 실행합니다.

| 실행                                                                                                           | 관리 에이전트                                      | 실행 프로필          |
|---------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|----------------------|
| 1                                                                                                             | Fabrikam FIMMA                                        | 전체 가져오기          |
| 2                                                                                                             |                                                       | 전체 동기화 |
| 3                                                                                                             |                                                       | 내보내기               |
| 4                                                                                                             |                                                       | 델타 가져오기         |
|                                                                                                               |                                                       |                      |
| 5                                                                                                             | Fabrikam ADMA                                         | 전체 가져오기          |
| 6                                                                                                             |                                                       | 전체 동기화 |



>[!NOTE]
아웃바운드 동기화 규칙이 메타버스에 성공적으로 프로젝션되었는지 확인해야 합니다.

## <a name="testing-the-configuration"></a>구성 테스트


이 섹션의 목적은 실제 구성을 테스트하는 것입니다. 구성을 테스트하려면

1.  FIM 포털에서 샘플 사용자를 만듭니다.

2.  샘플 사용자의 프로비전 요구 사항을 확인합니다.

3.  샘플 사용자를 AD DS에 프로비전합니다.

4.  사용자가 AD DS에 있는지 확인합니다.

### <a name="step-10-create-a-sample-user-in-mim"></a>10단계: MIM에서 샘플 사용자 만들기


다음 표에서는 샘플 사용자의 속성 목록을 보여 줍니다. 아래 표의 데이터에 따라 샘플 사용자를 만듭니다.

| 특성                              | 값                                                          |
|----------------------------------------|----------------------------------------------------------------|
| 이름                             | Britta                                                         |
| 성                              | Simon                                                          |
| 표시 이름                           | Britta Simon                                                   |
| 계정 이름                           | BSimon                                                         |
| 도메인                                 | Fabrikam                                                       |
| 직원 유형                          | 계약자                                                     |



### <a name="verify-the-provisioning-requisites-of-the-sample-user"></a>샘플 사용자의 프로비전 요구 사항을 확인합니다.


AD DS에 샘플 사용자를 프로비전하려면 두 가지 필수 조건이 충족되어야 합니다.

1.  사용자는 모든 계약자 집합의 구성원이어야 합니다.

2.  사용자 설정은 아웃바운드 동기화 규칙의 범위에 포함되어야 합니다.

### <a name="step-11-verify-the-user-is-a-member-of-all-contractors"></a>11단계: 사용자가 모든 계약자의 구성원인지 확인

사용자가 모든 계약자 집합의 구성원인지 확인하려면 집합을 열고 [구성원 보기]를 클릭합니다.

![사용자가 모든 계약자의 구성원인지 확인합니다.](media/how-provision-users-adds/image022.jpg)


### <a name="step-12-verify-the-user-is-in-the-scope-of-the-outbound-synchronization-rule"></a>12단계: 사용자가 아웃바운드 동기화 규칙의 범위 내에 있는지 확인

사용자가 동기화 규칙의 범위 내에 있는지 확인하려면 사용자 속성 페이지를 열어 프로비전 탭에 있는 예상 규칙 목록 특성을 검토합니다. 예상 규칙 목록 특성에서 AD 사용자 목록이 제시되어야 합니다.

아웃바운드 동기화 규칙. 다음 스크린샷은 예상 규칙 목록 특성의 예를 보여 줍니다.

![동기화 규칙 상태](media/how-provision-users-adds/image023.jpg)

프로세스의 이 시점에서 동기화 규칙 상태는 보류 중입니다. 즉, 동기화 규칙이 사용자에게 아직 적용되지 않았음을 의미합니다.



### <a name="step-13-synchronize-the-sample-group"></a>13단계: 샘플 그룹 동기화


테스트 개체에 대한 첫 번째 동기화 주기를 시작하기 전에 테스트 계획에서 각 실행 프로필을 실행한 후 예상되는 개체의 상태를 추적해야 합니다. 테스트 계획에는 개체의 일반적인 상태(생성됨, 업데이트됨, 삭제됨) 옆 예상 특성 값도 포함되어 있어야 합니다.
테스트 계획을 사용하여 테스트 계획 예상치를 확인합니다. 단계에서 예상 결과를 반환되지 않으면 예상 결과와 실제 결과의 불일치가 해결될 때까지는 다음 단계로 진행하지 마세요.

예상치를 확인하려면 동기화 통계를 첫 번째 지표로 사용할 수 있습니다. 예를 들어 커넥터 공간에서 새 개체를 예상하지만 가져오기 통계에서 “추가”를 반환하지 않는다면 사용자 환경에서 예상대로 작동하지 않는 것이 있다는 것을 의미합니다.

![동기화 통계](media/how-provision-users-adds/image024.jpg)

동기화 통계를 통해 시나리오가 예상대로 작동하는지 여부를 확인할 수 있지만 예상 특성 값을 확인하려면 동기화 서비스 관리자의 커넥터 공간 검색과 동기화 서비스 관리자의 메타버스 검색 기능을 사용해야 합니다.

AD DS에 사용자를 동기화하려면

1.  FIM MA 커넥터 공간에 사용자를 가져옵니다.

2.  사용자를 메타버스로 프로젝션합니다.

3.  사용자를 Active Directory 커넥터 공간에 프로비전합니다.

4.  FIM에 상태 정보를 내보냅니다.

5.  AD DS에 사용자를 내보냅니다.

6.  사용자가 만들어진 것을 확인합니다.

이러한 작업을 완료하려면 다음 실행 프로필을 실행해야 합니다.

| 관리 에이전트 | 실행 프로필  |
|------------------|--------------|
| Fabrikam FIMMA   | 1. 델타 가져오기 <br/> 2. 델타 동기화 <br/> 3. 내보내기 <br/> 4. 델타 가져오기 |
| Fabrikam FIMMA   | 1. 내보내기 <br/> 2. 델타 가져오기       |


FIM 서비스 데이터베이스에서 가져온 후 Britta Simon과 Britta에 연결하는 ExpectedRuleEntry 개체를

AD 사용자 아웃바운드 동기화 규칙은 Fabrikam FIMMA 커넥터 공간에서 준비됩니다. 검토 시

커넥터 공간의 Britta 속성은 FIM 포털에서 구성한 특성 값 옆에 있습니다. 예상 규칙 입력 개체에 대한 유효한 참조 또한 찾아야 합니다. 다음 스크린샷에서 이러한 예를 보여 줍니다.

![커넥터 공간 개체 속성](media/how-provision-users-adds/image025.jpg)

Fabrikam FIMMA에서 실행되는 델타 동기화의 목표는 여러 작업을 수행하는 것입니다.

-   프로젝션 - 새 사용자 개체 및 관련 예상 규칙 항목 개체가 메타버스에 프로젝션됩니다.

-   프로비전-새로 프로젝션된 Britta Simon 개체가 Fabrikam ADMA 커넥터 공간에 프로비전됩니다.

-   내보내기 특성 흐름 – 내보내기 특성 흐름은 두 관리 에이전트 모두에서 발생합니다. Fabrikam ADMA에서 새로 프로비전된 Britta Simon 개체는 새 특성 값으로 채워집니다. Fabrikam FIMMA에서 기존 Britta Simon 개 및 관련 ExpectedRuleEntry 개체는 프로젝션 결과의 특성 값으로 업데이트됩니다.

![동기화 통계](media/how-provision-users-adds/image026.jpg)

이미 동기화 통계에 나와 있는 것처럼 Fabrikam ADMA 커넥터 공간에서 프로비전 활동이 수행됩니다. Britta Simon의 메타버스 개체 속성을 검토하면 이 활동이 유효한 참조로 채워진 ExpectedRulesList 특성의 결과라는 것을 알 수 있습니다.

![메타버스 개체 속성](media/how-provision-users-adds/image027.jpg)

Fabrikam FIMMA에 다음 내보내기 작업을 수행하는 동안 Britta Simon의 동기화 규칙 상태가 [보류 중]에서 [적용됨]으로 업데이트되며 이는 사용자의 아웃바운드 동기화 규칙이 이제 메타버스에서 활성화되었음을 나타냅니다.

![응용 동기화 규칙](media/how-provision-users-adds/image028.jpg)

ADMA 커넥터 공간에 새 개체가 프로비전되었으므로 이 관리 에이전트에 대한 추가 보류 중인 내보내기가 하나 있어야 합니다. 이 목적으로 만들어진 스크립트를 사용하여 Fabrikam ADMA에 대한 보류 중인 추가 보고 하나를 볼 수 있습니다. 스크립트를 사용하려면 [Using Windows PowerShell to Display the Export State of a Management Agent](http://go.microsoft.com/FWLink/p/?LinkId=189664)(Windows PowerShell을 사용하여 관리 에이전트의 내보내기 상태 표시)를 참조하세요.

![관리 에이전트에 대한 보류 중인 내보내기](media/how-provision-users-adds/image029.jpg)

FIM의 각 내보내기 실행에서 내보내기 작업을 완료하려면 다음 델타 가져오기가 필요합니다. 이전 내보내기 후 실행하는 델타 가져오기는 가져오기 확인으로 알려져 있습니다. 연속적인 동기화 실행 중 적합한 업데이트 요구 사항을 확인하기 위해 FIM Synchronization Service를 활성화하려면 가져오기 확인이 필요합니다.


이 섹션의 지침에 따라 실행 프로필을 실행합니다.

>[!IMPORTANT]
각 실행 프로필은 오류 없이 성공적으로 실행되어야 합니다.

### <a name="step-14-verify-the-provisioned-user-in-ad-ds"></a>14단계: AD DS에서 프로비전된 사용자 확인

샘플 사용자가 AD DS에 프로비전되었는지 확인하려면 FIMObjects OU를 엽니다. Britta Simon은 FIMObjects OU 안에 있어야 합니다.

![FIMObjects OU에 사용자가 있는지 확인하는](media/how-provision-users-adds/image033.jpg)

<a name="summary"></a>요약
=======

이 문서의 목표는 AD DS와 MIM에서 사용자를 동기화하기 위한 주요 문서 블록을 소개하는 것입니다. 초기 테스트에서는 작업을 완료하는 데 필요한 최소한의 특성으로 시작하고, 일반 단계가 예상대로 진행되면 시나리오에 더 많은 특성을 추가합니다. 문제 해결 프로세스를 간소화하여 복잡성을 최소 수준으로 유지합니다.

구성을 테스트하는 경우, 새 테스트 개체를 삭제하고 다시 만드는 경우가 많습니다. ExpectedRulesList 특성으로 채워진 개체의 경우

분리된 ERE 개체가 나타날 수 있습니다.
테스트 환경에서 이러한 개체를 제거하는 방법에 대한 자세한 설명은 [A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667)(환경에서 분리된 ExpectedRuleEntry 개체를 제거하는 메서드)를 참조하세요.

동기화 대상으로 AD DS를 포함하는 일반적인 동기화 시나리오에서 MIM이 개체의 모든 특성에 대해 권한이 있는 것은 아닙니다. 예를 들어 최소한 FIM을 사용하여 AD DS에서 사용자 개체를 관리하는 경우에는 도메인 및 objectSID 특성이 AD DS 관리 에이전트에 의해 적용되어야 합니다.
사용자가 FIM 포털에 로그온할 수 있도록 하려는 경우 계정 이름, 도메인 및 objectSID 특성이 필요합니다. AD DS에서 이러한 특성을 채우려면 AD DS 커넥터 공간에 추가 인바운드 동기화 규칙이 있어야 합니다. 특성 값에 대한 여러 소스를 사용하여 개체를 관리하는 경우 프로그램 특성 흐름 우선 순위를 올바르게 구성해야 합니다. 특성 흐름 우선 순위가 올바르게 구성되지 않은 경우 동기화 엔진은 특성 값이 채워지지 못하게 합니다. 특성 흐름 우선 순위에 대한 자세한 내용은 [About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675)(특성 흐름 우선 순위 정보)에서 찾을 수 있습니다.

<a name="see-also"></a>참고 항목
=========

<a name="other-resources"></a>기타 리소스
---------------

[Using FIM to Enable or Disable Accounts in Active Directory](http://go.microsoft.com/FWLink/p/?LinkId=189670)(Active Directory에서 FIM을 사용하여 계정 활성화 또는 비활성화)

[About Reference Attributes](http://go.microsoft.com/FWLink/p/?LinkId=189671)(참조 특성 정보)

[How Can I Manage My FIM MA Account](http://go.microsoft.com/FWLink/p/?LinkId=189672)(FIM MA 계정 관리 방법)

[Detecting Nonauthoritative Accounts – Part 1: Envisioning](http://go.microsoft.com/FWLink/p/?LinkId=189673)(권한이 없는 계정 탐색 – 1부: 구상)

[The Poor Man’s Version of a Connector Detection Mechanism](http://go.microsoft.com/FWLink/p/?LinkId=189674)(커넥터 감지 메커니즘의 경제적 버전)

[Configuring the ADMA Account](http://go.microsoft.com/FWLink/p/?LinkId=189657)(ADMA 계정 구성)

[A Method to Remove Orphaned ExpectedRuleEntry Objects from Your Environment](http://go.microsoft.com/FWLink/p/?LinkId=189667)(환경에서 분리된 ExpectedRuleEntry 개체를 제거하는 메서드)

[About Attribute Flow Precedence](http://go.microsoft.com/FWLink/p/?LinkId=189675)(특성 흐름 우선 순위 정보)

[About Exports](http://go.microsoft.com/FWLink/p/?LinkId=189676)(내보내기 정보)

