---
title: "배스천 환경 계획 | Microsoft 문서"
description: 
keywords: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/16/2017
ms.topic: article
ms.service: microsoft-identity-manager
ms.technology: active-directory-domain-services
ms.assetid: bfc7cb64-60c7-4e35-b36a-bbe73b99444b
ms.reviewer: mwahl
ms.suite: ems
ms.openlocfilehash: b459906f0c8d2c631e9b63813e208c9098ea5a4e
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# 배스천 환경 계획
<a id="planning-a-bastion-environment" class="xliff"></a>

전용 관리 포리스트가 있는 배스천 환경을 Active Directory에 추가하면 조직은 기존 프로덕션 환경보다 더 강력한 보안 제어 기능을 포함하는 환경에서 관리 계정, 워크스테이션 및 그룹을 쉽게 관리할 수 있습니다.

이 아키텍처를 사용하면 단일 포리스트 아키텍처에서는 가능하지 않거나 쉽게 구성되지 않는 많은 제어가 가능해집니다. 프로덕션 환경에서는 높은 권한이 있지만 관리 포리스트에서는 권한 없는 표준 사용자가 되도록 계정을 프로비전하여 거버넌스를 보다 기술적으로 적용할 수 있습니다. 또한 이 아키텍처는 로그온(및 자격 증명 노출)을 허가된 호스트로만 제한하는 수단으로 트러스트의 선택적 인증 기능을 사용할 수 있도록 합니다. 비용을 들이거나 복잡하게 전체를 다시 작성하지 않으면서 프로덕션 포리스트에 대해 더 높은 수준의 보증을 적용하려는 경우 관리 포리스트가 프로덕션 환경의 보증 수준을 높이는 환경을 제공할 수 있습니다.

전용 관리 포리스트 외에도 추가 기술을 사용할 수 있습니다. 여기에는 관리 자격 증명이 노출되는 위치를 제한하고, 해당 포리스트에서의 사용자 역할 권한을 제한하고, 표준 사용자 작업(예: 메일 및 웹 검색)에 사용되는 호스트에서 관리 작업이 수행되지 않도록 하는 기능 등이 포함됩니다.

## 모범 사례 고려 사항
<a id="best-practice-considerations" class="xliff"></a>

전용 관리 포리스트는 Active Directory 관리에 사용되는 표준 단일 도메인 Active Directory 포리스트입니다. 관리 포리스트 및 도메인은 사용 사례가 제한되어 있으므로 프로덕션 포리스트보다 더 많은 보안 조치를 적용할 수 있는 이점이 있습니다. 또한 이 포리스트는 분리되고 조직의 기존 포리스트를 신뢰하지 않으므로 다른 포리스트의 보안이 손상되어도 이 전용 포리스트로 확장되지 않습니다.

관리 포리스트를 디자인할 때는 다음을 고려해야 합니다.

### 제한된 범위
<a id="limited-scope" class="xliff"></a>

관리 포리스트의 가치는 높은 수준의 보안을 보증하고 공격 노출 영역을 줄이는 것입니다. 이 포리스트는 추가적인 관리 기능 및 응용 프로그램을 포함할 수 있지만 범위가 증가할 때마다 포리스트의 공격 노출 영역과 해당 리소스가 증가합니다. 포리스트의 기능을 제한하여 공격 노출 영역을 최소화하는 것이 목표입니다.

관리자 권한 분할의 [계층 모델](tier-model-for-partitioning-administrative-privileges.md)에 따라 전용 관리 포리스트의 계정은 단일 계층, 일반적으로 계층 0 또는 계층 1이어야 합니다. 포리스트가 계층 1인 경우 특정 범위의 응용 프로그램(예: 재무 앱) 또는 사용자 커뮤니티(예: 아웃소싱된 IT 공급업체)로 제한하는 것이 좋습니다.

### 제한된 트러스트
<a id="restricted-trust" class="xliff"></a>

프로덕션 *CORP* 포리스트는 관리 *PRIV* 포리스트를 신뢰해야 하지만 그 반대의 경우는 아닙니다. 이것은 도메인 트러스트 또는 포리스트 트러스트일 수 있습니다. 추가 응용 프로그램에 양방향 트러스트 관계, 보안 유효성 검사 및 테스트가 필요할 수 있지만, 관리 포리스트 도메인이 Active Directory를 관리하기 위해 관리되는 도메인 및 포리스트를 신뢰할 필요는 없습니다.

관리 포리스트의 계정이 적합한 프로덕션 호스트만 사용하도록 선택적 인증을 사용해야 합니다. Active Directory에서 도메인 컨트롤러를 유지 관리하고 권한을 위임하려는 경우 일반적으로 도메인 컨트롤러에 대한 "로그온 허용" 권한을 관리 포리스트의 지정된 계층 0 계정에 부여해야 할 수 있습니다. 자세한 내용은 [선택적 인증 설정 구성](http://technet.microsoft.com/library/cc816580.aspx)을 참조하세요.

## 논리적 구분 유지
<a id="maintain-logical-separation" class="xliff"></a>

배스천 환경이 조직 Active Directory의 기존 또는 향후 보안 사고의 영향을 받지 않게 하려면 배스천 환경의 시스템을 준비할 때 다음 지침을 고려해야 합니다.

- Windows 서버는 도메인에 연결되지 않아야 하고, 기존 환경의 소프트웨어 또는 설정 배포를 활용하지 않아야 합니다.

- 배스천 환경은 자체 Active Directory 도메인 서비스를 포함해야 하며, Kerberos 및 LDAP, DNS, 시간 및 시간 서비스를 배스천 환경에 제공해야 합니다.

- MIM은 기존 환경에서 SQL 데이터베이스 팜을 사용하지 않아야 합니다. SQL Server는 배스천 환경의 전용 서버에 배포되어야 합니다.

- 배스천 환경에는 Microsoft Identity Manager 2016이 필요하며 특히 MIM 서비스 및 PAM 구성 요소를 배포해야 합니다.

- 배스천 환경을 위한 백업 소프트웨어와 미디어는 기존 포리스트의 시스템과는 별도로 유지해야 하므로 기존 포리스트의 관리자가 배스천 환경의 백업을 방해할 수 없습니다.

- 배스천 환경 서버를 관리하는 사용자는 기존 환경의 관리자가 액세스할 수 없는 워크스테이션에서 로그인해야 하므로 배스천 환경에 대한 자격 증명이 유출되지 않습니다.

## 관리 서비스 가용성 보장
<a id="ensure-availability-of-administration-services" class="xliff"></a>

응용 프로그램 관리가 배스천 환경으로 전환되므로 해당 응용 프로그램의 요구 사항에 맞게 충분한 가용성을 제공하는 방법을 고려해야 합니다. 해당 기술에는 다음이 포함됩니다.

- 배스천 환경의 여러 컴퓨터에 Active Directory 도메인 서비스를 배포합니다. 예약된 유지 관리를 위해 하나의 서버가 일시적으로 다시 시작되더라도, 인증 계속을 위해서는 적어도 두 대가 필요합니다. 더 높은 부하에 대비해서 또는 여러 지리적 지역 기반의 리소스와 관리자를 관리해야 하는 경우 추가 컴퓨터가 필요할 수 있습니다.

- 비상 사태에 대비해서 기존 포리스트 및 전용 관리 포리스트에 비상 계정을 준비합니다.

- 배스천 환경의 여러 컴퓨터에 SQL Server 및 MIM 서비스를 배포합니다.

- 전용 관리 포리스트의 사용자 또는 역할 정의를 변경할 때마다 AD 및 SQL의 백업 복사본을 유지합니다.

## 해당 Active Directory 사용 권한 구성
<a id="configure-appropriate-active-directory-permissions" class="xliff"></a>

관리 포리스트는 Active Directory 관리에 대한 요구 사항에 따라 최소 권한으로 구성되어야 합니다.

- 프로덕션 환경을 관리하는 데 사용되는 관리 포리스트의 계정에는 관리 포리스트, 포함된 도메인 또는 포함된 워크스테이션에 대한 관리자 권한이 부여되지 않아야 합니다.

- 공격자나 악의적인 내부자가 감사 로그를 지울 기회를 줄이려면 관리 포리스트 자체에 대한 관리자 권한을 오프라인 프로세스를 통해 강력하게 제어해야 합니다. 이렇게 하면 프로덕션 관리자 계정이 있는 직원이 자신의 계정에 대한 제한을 완화하여 조직에 대한 위험을 높일 수 없게 됩니다.

- 관리 포리스트는 인증 프로토콜에 대한 강력한 구성을 포함하여 도메인에 대한 Microsoft SCM(Security Compliance Manager) 구성을 수행해야 합니다.

배스천 환경을 만들 때 Microsoft Identity Manager를 설치하기 전에 이 환경 내의 관리에 사용할 계정을 식별하고 만듭니다. 여기에는 다음이 포함됩니다.

- **비상 계정**은 배스천 환경의 도메인 컨트롤러에만 로그인할 수 있습니다.

- **"레드 카드" 관리자**는 다른 계정을 프로비전하고 예약되지 않은 유지 관리를 수행합니다. 이러한 계정에서는 기존 포리스트 또는 배스천 환경 외부의 시스템에 액세스할 수 없습니다. 자격 증명(예: 스마트 카드)은 물리적으로 보호되어야 하며 이러한 계정의 사용은 기록되어야 합니다.

- **서비스 계정**은 Microsoft Identity Manager, SQL Server 및 기타 소프트웨어에 필요합니다.

## 호스트 보안 강화
<a id="harden-the-hosts" class="xliff"></a>

관리 포리스트에 연결된 도메인 컨트롤러, 서버 및 워크스테이션을 비롯한 모든 호스트에는 최신 운영 체제와 서비스 팩이 설치되어 있어야 하고 최신 상태로 유지되어야 합니다.

- 관리를 수행하는 데 필요한 응용 프로그램은 워크스테이션을 사용하는 계정이 해당 응용 프로그램을 설치하기 위해 로컬 관리자 그룹에 포함될 필요가 없도록 워크스테이션에 미리 설치되어야 합니다. 도메인 컨트롤러 유지 관리는 일반적으로 RDP 및 원격 서버 관리 도구를 사용하여 수행할 수 있습니다.

- 관리 포리스트 호스트는 보안 업데이트로 자동 업데이트해야 합니다. 이로 인해 도메인 컨트롤러 유지 관리 작업이 중단될 수 있지만 패치가 적용되지 않은 취약성으로 인한 보안 위험이 크게 완화될 수 있습니다.

### 관리 호스트 식별
<a id="identify-administrative-hosts" class="xliff"></a>

시스템 또는 워크스테이션의 위험은 수행되는 가장 위험한 작업(예: 인터넷 검색, 메일 주고받기 또는 알 수 없거나 신뢰할 수 없는 콘텐츠를 처리하는 다른 응용 프로그램의 사용)으로 측정해야 합니다.

관리 호스트에는 다음 컴퓨터가 포함됩니다.

- 관리자의 자격 증명이 실제로 입력되는 데스크톱

- 관리 세션 및 도구가 실행되는 관리 "점프 서버"

- 관리 작업이 수행되는 모든 호스트(RDP 클라이언트가 실행되는 표준 사용자 데스크톱을 사용하여 서버 및 응용 프로그램을 원격으로 관리하는 호스트 포함)

- 관리되어야 하는 응용 프로그램을 호스트하고, 제한된 관리자 모드 또는 Windows PowerShell 원격 기능과 함께 RDP를 사용해서 액세스되지 않는 서버

### 전용 관리 워크스테이션 배포
<a id="deploy-dedicated-administrative-workstations" class="xliff"></a>

불편할 수 있지만 높은 영향력의 관리자 자격 증명이 있는 사용자에게 보안이 강화된 워크스테이션 관리를 전담시켜야 할 수 있습니다. 자격 증명에 위임된 권한 수준보다 크거나 같은 보안 수준을 호스트에 제공하는 것은 중요합니다. 추가로 보호하기 위해 다음과 같은 조치를 통합하는 것이 좋습니다.

- **빌드의 모든 미디어가 깨끗한지 확인**: 다운로드 또는 저장 중에 마스터 이미지에 설치되었거나 설치 파일에 주입된 맬웨어의 위협을 완화합니다.

- **보안 기준** 을 시작 구성으로 사용해야 합니다. Microsoft SCM(Security Compliance Manager)은 관리 호스트에 초기 계획을 구성하는 데 도움이 될 수 있습니다.

- **보안 부팅** : 부팅 프로세스에 서명되지 않은 코드를 로드하려고 하는 공격자 또는 맬웨어의 위협을 완화합니다.

- **소프트웨어 제한** : 허가된 관리 소프트웨어만 관리 호스트에서 실행되도록 합니다. 고객은 이 작업에 대해 AppLocker와 권한이 부여된 응용 프로그램의 허용 목록을 사용하여 악성 소프트웨어 및 지원되지 않는 응용 프로그램이 실행되지 않도록 할 수 있습니다.

- **전체 볼륨 암호화** : 원격으로 사용되는 관리 랩톱과 같은 컴퓨터가 실제로 손상되는 경우를 완화합니다.

- **USB 제한**: 물리적 감염으로부터 보호합니다.

- **네트워크 격리** : 네트워크 공격 및 부주의한 관리 작업으로부터 보호합니다. 호스트 방화벽은 명시적으로 필요한 경우를 제외하고 들어오는 모든 연결을 차단하고 불필요한 모든 아웃바운드 인터넷 액세스를 차단해야 합니다.

- **맬웨어 방지** : 알려진 위협 및 맬웨어로부터 보호합니다.

- **악용 완화** : EMET(Enhanced Mitigation Experience Toolkit)를 포함하여 알 수 없는 위협 및 악용을 완화합니다.

- **공격 노출 영역 분석** : 새 소프트웨어 설치 중에 Windows에 새로운 공격 벡터가 발생하지 않도록 합니다. ASA(Attack Surface Analyzer)와 같은 도구는 호스트의 구성 설정을 평가하고 소프트웨어 또는 구성 변경으로 인해 공격 벡터를 식별하는 데 도움이 됩니다.

- 해당 로컬 컴퓨터의 사용자에게 **관리자 권한**을 부여하지 않아야 합니다.

- 나가는 RDP 세션은 역할에 필요한 경우를 제외하고 **RestrictedAdmin 모드**입니다. 자세한 내용은 [Windows Server 원격 데스크톱 서비스의 새로운 기능](https://technet.microsoft.com/library/dn283323.aspx)을 참조하세요.

이러한 조치 일부는 극단적인 것처럼 보일 수 있지만 최근 몇 년 동안 알려진 바에 따르면 숙련된 악의적 사용자 중 일부는 대상을 손상시키는 엄청난 능력을 보유하고 있습니다.

## 배스천 환경에서 관리하도록 기존 도메인 준비
<a id="prepare-existing-domains-to-be-managed-by-the-bastion-environment" class="xliff"></a>

MIM은 PowerShell cmdlet을 사용하여 배스천 환경의 기존 AD 도메인 및 전용 관리 포리스트 간에 신뢰를 설정합니다. 배스천 환경이 배포된 후 사용자 또는 그룹이 JIT로 변환되기 전에 `New-PAMTrust` 및 `New-PAMDomainConfiguration` cmdlet은 도메인 트러스트 관계를 업데이트하고 AD 및 MIM에 필요한 아티팩트를 만듭니다.

기존 Active Directory 토폴로지가 변경되면 `Test-PAMTrust`, `Test-PAMDomainConfiguration`, `Remove-PAMTrust` 및 `Remove-PAMDomainConfiguration` cmdlet을 사용하여 트러스트 관계를 업데이트할 수 있습니다.

## 각 포리스트에 대한 트러스트 설정
<a id="establish-trust-for-each-forest" class="xliff"></a>

`New-PAMTrust` cmdlet은 기존 포리스트마다 한 번씩 실행해야 합니다. 관리 도메인의 MIM 서비스 컴퓨터에서 호출됩니다. 이 명령의 매개 변수는 기존 포리스트에서 상위 도메인의 도메인 이름과 각 도메인 관리자의 자격 증명입니다.

```
New-PAMTrust -SourceForest "contoso.local" -Credentials (get-credential)
```

트러스트를 설정한 후 다음 섹션에 설명된 대로 배스천 환경에서 관리를 수행하도록 각 도메인을 구성합니다.

## 각 도메인의 관리를 사용하도록 설정
<a id="enable-management-of-each-domain" class="xliff"></a>

기존 도메인에 대한 관리를 사용하도록 설정하기 위한 7가지 요구 사항이 있습니다.

### 1. 로컬 도메인의 보안 그룹
<a id="1-a-security-group-on-the-local-domain" class="xliff"></a>

해당 이름이 NetBIOS 도메인 이름과 세 개의 달러 기호(예: *CONTOSO$$$*)로 구성된 기존 도메인의 그룹이어야 합니다. 그룹 범위는 *도메인 로컬*이고 그룹 유형은 *보안*이어야 합니다. 이 도메인의 그룹과 동일한 보안 식별자를 사용하여 전용 관리 포리스트에 그룹을 만들려면 이러한 조건이 충족되어야 합니다. 기존 도메인의 관리자가 기존 도메인에 연결된 워크스테이션에서 다음 PowerShell 명령을 실행하여 이 그룹을 만듭니다.

```
New-ADGroup -name 'CONTOSO$$$' -GroupCategory Security -GroupScope DomainLocal -SamAccountName 'CONTOSO$$$'
```

### 2. 성공 및 실패 감사
<a id="2-success-and-failure-auditing" class="xliff"></a>

감사를 위한 도메인 컨트롤러의 그룹 정책 설정에는 감사 계정 관리 및 감사 디렉터리 서비스 액세스에 대한 성공 및 실패 감사가 모두 포함되어야 합니다. 이 작업은 기존 도메인의 관리자가 기존 도메인에 연결된 워크스테이션에서 그룹 정책 관리 콘솔을 사용하여 수행할 수 있습니다.

3. **시작** > **관리 도구** > **그룹 정책 관리**로 이동합니다.

4. **포리스트: contoso.local** > **도메인** > **contoso.local** > **도메인 컨트롤러** > **기본 도메인 컨트롤러 정책**으로 이동합니다. 정보 메시지가 표시됩니다.

    ![기본 도메인 컨트롤러 정책 - 스크린샷](media/pam-group-policy-management.jpg)

5. **기본 도메인 컨트롤러 정책**을 마우스 오른쪽 단추로 클릭하고 **편집**을 선택합니다. 새 창이 표시됩니다.

6. 그룹 정책 관리 편집기 창의 기본 도메인 컨트롤러 정책 트리에서 **컴퓨터 구성** > **정책** > **Windows 설정** > **보안 설정** > **로컬 정책** > **감사 정책**으로 이동합니다.

    ![그룹 정책 관리 편집기 - 스크린샷](media/pam-group-policy-management-editor.jpg)

5. 세부 정보 창에서 **계정 관리 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란 및 **실패** 확인란을 선택한 다음 **적용** 및 **확인**을 클릭합니다.

6. 세부 정보 창에서 **디렉터리 서비스 액세스 감사**를 마우스 오른쪽 단추로 클릭하고 **속성**을 선택합니다. **이 정책 설정 정의**를 클릭하고 **성공** 확인란 및 **실패** 확인란을 선택한 다음 **적용** 및 **확인**을 클릭합니다.

    ![성공 및 실패 정책 설정 - 스크린샷](media/pam-group-policy-management-editor2.jpg)

7. 그룹 정책 관리 편집기 창 및 그룹 정책 관리 창을 닫습니다. 그런 다음 PowerShell 창을 시작하고 다음을 입력하여 감사 설정을 적용합니다.

    ```
    gpupdate /force /target:computere
    ```

잠시 후 "컴퓨터 정책 업데이트가 완료되었습니다."라는 메시지가 표시됩니다.

### 3. 로컬 보안 기관에 대한 연결 허용
<a id="3-allow-connections-to-the-local-security-authority" class="xliff"></a>

도메인 컨트롤러는 배스천 환경의 LSA(로컬 보안 기관)에 대해 RPC over TCP/IP 연결을 허용해야 합니다. 이전 버전의 Windows Server에서 LSA의 TCP/IP 지원은 다음과 같이 레지스트리에서 사용하도록 설정해야 합니다.

```
New-ItemProperty -Path HKLM:SYSTEM\\CurrentControlSet\\Control\\Lsa -Name TcpipClientSupport -PropertyType DWORD -Value 1
```

### 4. PAM 도메인 구성 만들기
<a id="4-create-the-pam-domain-configuration" class="xliff"></a>

`New-PAMDomainConfiguration` cmdlet은 관리 도메인의 MIM 서비스 컴퓨터에서 실행해야 합니다. 이 명령의 매개 변수는 기존 도메인의 도메인 이름과 해당 도메인 관리자의 자격 증명입니다.

```
 New-PAMDomainConfiguration -SourceDomain "contoso" -Credentials (get-credential)
```

### 5. 계정에 읽기 사용 권한 부여
<a id="5-give-read-permissions-to-accounts" class="xliff"></a>

역할을 설정하는 데 사용하는 배스천 포리스트의 계정( `New-PAMUser` 및 `New-PAMGroup` cmdlet을 사용하는 관리자)과 MIM 모니터 서비스에서 사용되는 계정에는 해당 도메인의 읽기 권한이 필요합니다.

다음 단계에 따라 *CORPDC* 도메인 컨트롤러 내의 도메인 *Contoso*에 대해 사용자 *PRIV\Administrator*의 읽기 권한을 사용하도록 설정합니다.

1. CORPDC에 Contoso 도메인 관리자(예: Contoso\Administrator)로 로그인해야 합니다.

2. Active Directory 사용자 및 컴퓨터를 시작합니다.

3. 도메인 **contoso.local**을 마우스 오른쪽 단추로 클릭하고 **위임 컨트롤**을 선택합니다.

4. 선택한 사용자 및 그룹탭에서 **추가**를 클릭합니다.

5. 사용자, 컴퓨터 또는 그룹 선택 팝업에서 **위치**를 클릭하고 위치를 *priv.contoso.local*로 변경합니다. 개체 이름에서 *Domain Admins*를 입력하고 **이름 확인**을 클릭합니다. 팝업이 표시되면 사용자 이름에 대해 *priv\administrator*와 암호를 입력합니다.

6. Domain Admins 뒤에 *; MIMMonitor*를 입력합니다. Domain Admins 및 MIMMonitor 이름에 밑줄이 표시되면 **확인**을 클릭하고 **다음**을 클릭합니다.

7. 일반 작업 목록에서 **모든 사용자 정보 읽기**를 선택한 후 **다음**과 **마침**을 차례로 클릭합니다.

18. Active Directory 사용자 및 컴퓨터를 닫습니다.

### 6. 비상 계정 준비
<a id="6-a-break-glass-account" class="xliff"></a>

권한 있는 액세스 관리 프로젝트의 목표가 도메인에 영구적으로 할당된 도메인 관리자 권한이 있는 계정의 수를 줄이는 것이면 나중에 트러스트 관계에 문제가 발생할 경우에 대비하여 도메인에 *비상* 계정이 있어야 합니다. 프로덕션 포리스트에 비상 시에 액세스하기 위한 계정이 각 도메인에 있어야 하고 해당 계정만 도메인 컨트롤러에 로그인할 수 있어야 합니다. 여러 사이트가 있는 조직의 경우 중복성을 위해 추가 계정이 필요할 수 있습니다.

### 7. 배스천 환경에서 사용 권한 업데이트
<a id="7-update-permissions-in-the-bastion-environment" class="xliff"></a>

해당 도메인의 시스템 컨테이너에 있는 *AdminSDHolder* 개체에 대한 사용 권한을 검토합니다. *AdminSDHolder* 개체에는 기본 제공 권한 있는 Active Directory 그룹의 구성원인 보안 주체의 사용 권한을 제어하는 데 사용되는 고유한 ACL(액세스 제어 목록)이 있습니다. 도메인의 관리자 권한이 있는 사용자에게 영향을 미치는 방식으로 기본 권한이 변경되어도 배스천 환경에 계정이 있는 사용자에게는 해당 권한이 적용되지 않습니다.

## 포함할 사용자 및 그룹 선택
<a id="select-users-and-groups-for-inclusion" class="xliff"></a>

다음 단계는 PAM 역할을 정의하고, 해당 역할에서 액세스할 수 있는 사용자 및 그룹을 연결하는 것입니다. 일반적으로는 배스천 환경에서 관리되는 것으로 식별된 계층에 대한 사용자 및 그룹 하위 집합이 여기에 해당합니다. 자세한 내용은 [Privileged Access Management를 위한 역할 정의](defining-roles-for-pam.md)를 참조하세요.
