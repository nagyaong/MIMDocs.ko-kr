---
title: "리소스 컨트롤 표시 구성 XML 참조 | Microsoft Docs"
description: 
keywords: 
author: fimguy
ms.author: fimguy
manager: mbaldwin
ms.date: 05/1/2017
ms.topic: reference
ms.prod: identity-manager-2016
ms.service: microsoft-identity-manager
ms.technology: security
ms.assetid: 
ms.openlocfilehash: c6ed843fb15b150fc934062945ab76ba32d72d33
ms.sourcegitcommit: 02fb1274ae0dc11288f8bd9cd4799af144b8feae
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/13/2017
---
# <a name="resource-control-display-configuration-xml-reference"></a>리소스 컨트롤 표시 구성 XML 참조

RCDC(리소스 컨트롤 표시 구성) 리소스는 MIM(Microsoft Identity Manager) 2016 SP1 데이터 저장소의 다른 리소스가 최종 사용자의 UI(사용자 인터페이스)에 표시되는 방법을 제어하는 데 사용할 수 있는 사용자 정의 리소스입니다. 각 RCDC 리소스에는 UI 텍스트 및 UI 컨트롤을 추가, 수정 또는 제거하기 위해 변경할 수 있는 XML 구성 파일이 포함되어 있습니다. MIM 2016 SP1은 여러 기본 RCDC 리소스를 제공하지만 사용자 지정 리소스에 대한 사용자 지정 RCDC 리소스를 만들 수도 있습니다. FIM 포털의 RCDC UI 사용에 대한 자세한 내용은 FIM 설명서의 [FIM 포털 구성 및 사용자 지정 소개](http://go.microsoft.com/fwlink/?LinkID=165848)를 참조하세요.


## <a name="known-issues"></a>알려진 문제

많은 리소스 컨트롤 표시 구성 컨트롤의 기본값을 사용할 수 없음

이번 릴리스에서 옵션 단추 컨트롤을 제외하고 리소스 컨트롤의 컨트롤에서 기본값을 설정하는 것이 지원되지 않습니다. 값과 기본값과 연결되지 않은 기본값을 지정하여 강제로 사용자가 선택을 변경하도록 함으로써 드롭다운 상자에 대해 이 문제를 해결할 수 있습니다. 다른 컨트롤에 대한 이 문제를 해결하려면 요청을 제출하는 동안 권한 부여 워크플로를 사용하여 기본값을 제공해야 합니다.

## <a name="basic-structure"></a>기본 구조

RCDC 리소스에 대한 XML 데이터는 단일 XML 요소인 **ObjectControlConfiguration**으로 구성되어 있습니다.

>[!NOTE]
전체 XSD 스키마의 경우 이 문서 뒷부분에 나오는 부록 A: 기본 XSD 스키마를 참조하세요.

다음은 ObjectControlConfiguration 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="ObjectControlConfiguration"\>
  <xsd:complexType\>
    <xsd:sequence\>
      <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
      <xsd:element ref="my:Panel"/>
      <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
    </xsd:sequence>
    <xsd:attribute ref="my:TypeName"/>
    <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
  </xsd:complexType>
</xsd:element>
```



**ObjectControlConfiguration** 요소는 다음을 포함합니다.

1.  **ObjectDataSource**: 이 요소는 RC(리소스 컨트롤)에서 사용하는 데이터 소스 클래스의 TypeName을 지정합니다. 설명 및 스키마 정의에 대해서는 이 문서의 다음 데이터 소스 섹션을 참조하세요. **ObjectControlConfiguration** 요소는 **ObjectDataSource** 요소의 노드를 최대 32개 포함할 수 있습니다.

2.  **XmlDataSource**: 요약 페이지의 디자인을 지정하는 데 가장 일반적으로 사용되는 간단한 데이터 소스입니다. 설명 및 스키마 정의에 대해서는 이 문서의 다음 데이터 소스 섹션을 참조하세요. **ObjectControlConfiguration** 요소는 **XmlDataSource** 요소의 노드를 최대 32개 포함할 수 있습니다.

3.  **Panel**: 관리자는 Panel 요소 내의 요소를 수정하여 RCDC 페이지의 레이아웃을 사용자 지정할 수 있습니다. 자세한 내용은 이 문서 뒷부분의 Panel 섹션을 참조하세요. **ObjectControlConfiguration** 요소에는 Panel 요소가 하나만 있어야 합니다.

4.  **Events**: 관리자는 사용자 지정 코드 숨김을 제공할 수 없으므로 이 기능은 제한됩니다. 상태 변경에 따라 패널 또는 컨트롤에서 내보낼 수는 이벤트입니다. 자세한 내용은 이 항목의 뒷부분에 나오는 Events 섹션을 참조하세요. **ObjectControlConfiguration** 요소는 필요에 따라 하나의 **Event** 요소를 포함할 수 있습니다. 일반적으로 사용자 지정 **Events**의 사용은 나중에 특별히 개선되지 않는 한, 지원되지 않습니다.

## <a name="data-sources"></a>데이터 원본

Microsoft Identity Manager는 UI 구성 요소에 데이터를 바인딩하는 방법으로 데이터 소스를 사용합니다. 이렇게 하면 프레젠테이션 계층에서 데이터를 용이하게 분리할 수 있습니다. RCDC 리소스 구성 데이터에는 2가지 종류의 데이터 소스, **ObjectDataSource** 및 **XmlDataSource**가 있습니다.

-   **ObjectDataSources**는 RC에는 데이터를 제공하는 Microsoft .NET 클래스를 지정합니다. 관리자가 RCDC를 제작할 때 사용하도록 선택할 수 있는 경우, ObjectDataSources 유형의 고정된 집합을 사용할 수 있습니다.

-   **XMLDataSources**는 XML 기반 데이터를 구조화하는 간단한 방법을 제공하며, 관리자가 사용자 지정된 데이터를 제공하는 데 사용할 수 있습니다. 미리 정의된 기본 제공 XML 구조를 사용하지 않는 한, XML 데이터는 RCDC에 직접 지정해야 합니다. 기본 제공 XML 구조는 RC에서 요약 페이지를 생성하는 데 사용됩니다.

RCDC에서 이러한 데이터 소스를 RCDC에 지정된 UI 컨트롤의 특성에 바인딩하여 UI를 생성할 수 있습니다.

### <a name="objectdatasource"></a>ObjectDataSource

Microsoft Identity Manager는 모든 리소스 형식에 사용할 수 있는 다음 표의 일반 데이터 소스 유형을 제공합니다(설명된 경우 제외).

| TypeName                        | 설명     | 양방향 바인딩 지원 | 지원되는 바인딩 구문        |
|----------------|-----------|-------------------------|--------------|
| PrimaryResourceObjectDataSource | 생성, 편집 또는 열람되는 FIM 2010 리소스를 나타냅니다. 바인딩 문자열의 경로는 특성 이름입니다. 리소스 형식이 RCDC.ConfigurationData 특성이 아닌 RCDC의 TargetObjectType 특성으로 지정되어 있는지 확인합니다. | 예                     | 이름으로 지정된 개체 특성의 [AttributeName] 값입니다.    |
| PrimaryResourceDeltaDataSource  | 이 데이터 소스는 FIM 2010 리소스의 원래 상태와 현재 상태를 비교하는 델타 XML을 작성합니다. 생성된 델타 XML은 RC 요약 컨트롤에서 사용자가 제출하는 요청에 대한 UI를 렌더링하기 위해 사용합니다.                                    | 아니요                      | DeltaXml: </br> 델타를 표시하기 위해 요약 컨트롤에서 사용됩니다.                                                 |
| PrimaryResourceRightsDataSource | 이 데이터 소스는 FIM 2010 리소스의 각 특성에 대한 인라인 권한을 제공합니다. 이를 통해 RC는 제출에 앞서, 사용자가 해당 특성에 대해 갖는 사용 권한을 확인한 다음 해당 특성에 대한 UI를 적절히 렌더링할 수 있습니다.                     | 아니요                      | [AttributeName]                                                                                         |
| SchemaDataSource                | 이 데이터 소스는 표시 이름, 설명, 특성 필요 여부 등의 스키마 관련 정보와 리소스 형식 정보에 액세스하는 데 사용될 수 있습니다.                                                                                             | 아니요                      | 특성이 유효하기 위해 값이 있어야 하는지 여부를 나타내는 [AttributeName].**Required** 부울 값 <br/> 바인딩의 표시 이름을 나타내는 [AttributeName].**DisplayNameString** 값 <br/>바인딩의 설명을 나타내는 [AttributeName].**DescriptionString** 값 <br/>바인딩의 문자열 Regex 나타내는 [AttributeName].StringRegexString 값 <br/>[AttributeName].**DisplayName** <br/> [AttributeName].**Description** <br/> [AttributeName]. [AttributeName].**IntergerValueMinimum** <br/>[AttributeName].**IntergerValueMaximum** <br/>[AttributeName].**LocalizedAllowedValues**|
| DomainDataSource                | 이 데이터 소스는 도메인 구성 리소스를 기준으로 도메인의 열거형을 제공합니다. 이 데이터 소스는 그룹 리소스 및 사용자 리소스에 대한 RCDC에서만 사용할 수 있습니다.                                                                           | 예                     | 도메인           |

다음은 3개의 데이터 소스를 UocTextBox 컨트롤에 바인딩하여 그룹의 Description 특성을 편집하는 예제 RCDC 조각입니다.

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceObjectDataSource" my:Name="object" my:Parameters=""/>
<my:ObjectDataSource my:TypeName="SchemaDataSource" my:Name="schema"/>
<my:ObjectDataSource my:TypeName="PrimaryResourceRightsDataSource" my:Name="rights"/>

     <my:Control my:Name="Description" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Description.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Description}">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
               <my:Property my:Name="Rows" my:Value="3"/>
               <my:Property my:Name="Columns" my:Value="60"/>
               <my:Property my:Name="MaxLength" my:Value="450"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=Description, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
```

### <a name="xmldatasource"></a>XMLDataSource

**XMLDataSource**를 사용하여 RCDC가 지정된 리소스에 대해 사용할 수 있는 사용자 지정 데이터를 지정할 수 있습니다. 이 경우 XML 데이터를 RCDC에 지정해야 합니다. 또는 이 데이터 소스를 기본 제공 XML 데이터 구조를 참조하여 요약 페이지에 대한 UI를 렌더링하는 데 사용할 수도 있습니다. RCDC에서 정의할 때 사용할 **XMLDataSource** 유형을 제어합니다.


| TypeName                 | 설명   | | |
|--------------------------|------------|
| **XMLDataSource**            | 이 데이터 소스는 XML 데이터를 나타냅니다. XSL 또는 포함된 XSL 형식일 수 있습니다. <br/>**XSL 형식:** <br/> Microsoft.IdentityManagement.WebUI.Controls.dll<my:XmlDataSource my:Name=" <br/>summaryTransformXsl"my:Parameters=”Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl”> </my:XmlDataSource><br/> **포함된 XSL 형식:** <br/> <my:XmlDataSource my:Name="RequestStatusTransformXsl"> <br/> <xsl:stylesheet version="1.0" xmlns:xsl=http://www.w3.org/1999/XSL/Transform <br/> xmlns:msxsl="urn:schemas-microsoft-com:xslt"><br/></xsl:stylesheet></my:XmlDataSource>                       |아니요 | ```Xpath[;namespaces]``` <br/> 여기서 Xpath는 필요한 메모를 선택하기 위한 유효한 XML xpath로, 가장 일반적으로는 "/"(루트)가 사용됩니다. <br/>xpath가 네임스페이스 지정 XML에 대해 작동하는 데 필요한 경우 네임스페이스는 세미콜론으로 구분된 prefix=URI 문자열의 선택적 목록입니다. |
| **ReferenceDeltaDataSource** | 데이터 소스는 다중 값 참조 특성의 델타를 나타냅니다. Group 및 Set에 대한 RCDC에서만 사용됩니다. <br/> 데이터 소스가 Group 또는 Set로 제한되지 않더라도 이러한 델타를 제출하려면 RCDC 호스트에서 코드를 변경해야 합니다. 현재 Group 및 Set는 이 데이터 소스를 인식하는 유일한 호스트입니다.  | 예                      | [AttributeName].Add 여기서 [AttributeName]은 참조 특성을 나타내며 반환된 데이터는 델타 추가입니다. <br/> 예: [ReferenceAttribute].Add <br/>예: ```<my:Property my:Name="Value" my:Value="{Binding Source=delta, Path=ExplicitMember.Add, Mode=TwoWay}" />``` <br/>[AttributeName].Remove 여기서 [AttributeName]은 참조 특성을 나타내며 반환된 데이터는 델타 제거입니다. <br/> DeltaXml |
|**RequestDetailsDataSource**| 데이터 소스는 Request 개체의 RequestParameter 특성을 나타냅니다. 이 매개 변수는 다중 값 특성당 표시할 특성 값의 최대 수를 설정하며 요청에 대한 RCDC에서만 사용됩니다. ```<my:ObjectDataSource my:TypeName="RequestDetailsDataSource" my:Name="requestDetails" my:Parameters="1000" />```| 아니요 | DeltaXml |
|**RequestStatusDataSource**| 데이터 소스는 Request 개체의 **RequestStatusDetails** 특성을 나타냅니다. <br/>요청에 대한 RCDC에서만 사용됩니다.  | 아니요 | DeltaXml |

-   사용자 지정 XML 데이터 소스를 정의하려면

 ```XML
   <my:XmlDataSource my:Name="MyCustomData" >
   %Insert custom, properly formatted XML data here%
   </my:XmlDataSource>
   ```

-   기본 제공 요약 컨트롤 XSL을 사용하려면 다음과 같이 데이터 소스를 정의합니다.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />
```

 사용자 지정 리소스 형식에 대한 RCDC를 만드는 경우 이 메서드를 사용하여 해당 사용자 지정 리소스에 대한 요약 페이지를 자동으로 렌더링할 수 있습니다.

다음은 기본 제공 XSL을 사용하고 XMLDataSource에서 PrimaryResourceDeltaDataSource를 사용하여 RCDC에서 요약 탭을 만드는 방법의 예입니다.

```XML
<my:ObjectDataSource my:TypeName="PrimaryResourceDeltaDataSource" my:Name="delta" />
<my:XmlDataSource my:Name="summaryTransformXsl" my:Parameters="Microsoft.IdentityManagement.WebUI.Controls.Resources.DefaultSummary.xsl" />

<my:Grouping my:Name="summaryGroup" my:Caption="Summary” my:IsSummary="true">
     <my:Control my:Name="summaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}" />
              <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}" />
          </my:Properties>
     </my:Control>
</my:Grouping>
```

사용자는 이전에 지정한 XmlDataSource 요소를 다음 형식으로 바꾸어 요약 페이지의 사용자 지정된 레이아웃을 정의할 수도 있습니다. 참조로 기본 FIM 2010 요약 XSL이 이 문서 뒷부분에 나오는 부록 B: 기본 요약 XSL에 포함됩니다.

```XML
<my:XmlDataSource my:Name="summaryTransformXsl">
     Insert valid XSL code here
</my:XmlDataSource>
```
### <a name="schema-for-data-sources"></a>데이터 소스의 스키마
다음은 두 가지 유형의 데이터 소스에 대한 XSD 스키마입니다.

```XML
<xsd:element name="ObjectDataSource">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
     </xsd:complexType>
</xsd:element>
<xsd:element name="XmlDataSource">
     <xsd:complexType  mixed="true">
          <xsd:sequence>
              <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Parameters"/>
    </xsd:complexType>
</xsd:element>
```

## <a name="events"></a>이벤트
Event는 컨트롤의 변경 상태를 정의합니다. 이벤트가 트리거된 후에 진행될 동작을 정의하는 사용자 지정된 함수(처리기)를 작성할 수 없으므로 이 기능의 확장성은 제한됩니다. 동일한 Event 요소를 Panel 요소에 사용할 수 있습니다. 자세한 내용은 이 문서 뒷부분의 Panel 섹션을 참조하세요. 다음은 Event 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Events">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Event">
     xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
     </xsd:element>
```


Event는 빈 요소이며 2개의 특성이 있습니다.

**특성:**

1.  **Name**: 이벤트의 고유 이름입니다. **ObjectControlConfiguration**의 지원되는 유일한 이벤트는 Load 이벤트입니다. 이 이벤트는 페이지가 처음 로드될 때 트리거됩니다.

2.  **Handler**: 처리기의 고유 이름입니다. 이 이벤트가 트리거되면 일반적으로 컨트롤의 상태 변경을 처리하기 위해 프로그램 메서드가 호출됩니다. 기존 컨트롤에서 기존 처리기 제거, 새 처리기 생성, 기존 또는 새 컨트롤에 연결 등은 지원되지 않습니다.

샘플:

다음은 Event 요소의 예입니다.
```XML
<my:Events>
    <my:Event my:Name="Load" my:Handler="OnLoad"/>
</my:Events>
```

**Panel**


Panel 요소는 RCDC 레이아웃의 핵심 요소입니다. 다음은 Panel 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Panel">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:DisplayAsWizard"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:AutoValidate"/>
     </xsd:complexType>
</xsd:element>
```

이 요소는 되풀이 요소인 Grouping을 포함합니다. 자세한 내용은 이 문서의 Grouping 섹션을 참조하세요.

Panel 요소에는 다음 4가지 특성이 포함되어 있습니다.

1.  **Name**: Panel의 이름입니다. 필수 문자열 형식 특성입니다.

2.  **DisplayAsWizard**: 이 특성은 현재 사용되지 않습니다. RCDC의 해당 VerbContext 특성은 리소스 레이아웃이 마법사 모드인지 또는 탭 모드인지를 제어합니다. 0(만들기 모드)으로 설정되면 마법사 모드이기도 합니다. 그렇지 않으면 탭 모드입니다. 자세한 내용은 설명서의 FIM 포털 구성 및 사용자 지정 소개를 참조하세요.

3.  **Caption**: 이 특성은 현재 사용되지 않습니다. 사용자는 헤더 정보만 포함하는 Group을 포함하여 페이지에 대한 캡션을 지정할 수 있습니다. 자세한 내용은 이 문서의 Grouping 섹션을 참조하세요.

4.  **AutoValidate**: 선택적 부울 특성입니다. true로 설정되면 현재 탭에 있는 각 컨트롤에 대해 유효성 검사가 트리거됩니다. 기본적으로 이 특성이 없으면 true로 설정됩니다. RegularExpression 속성과 함께 사용할 수 있습니다. 자세한 내용은 이 문서 뒷부분의 "RegularExpression"을 참조하세요.

## <a name="grouping"></a>그룹화

Grouping 요소는 Panel의 전체 레이아웃을 정의합니다. 개별 컨트롤을 여러 다른 섹션 및 탭으로 그룹화하는 컨테이너로 작동합니다. 다음은 Grouping 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Grouping">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:IsHeader"/>
          <xsd:attribute ref="my:IsSummary"/>
     </xsd:complexType>
</xsd:element>
```


다음 세 가지 유형의 **그룹화**가 있습니다.

1.  **헤더 그룹화**: 헤더 그룹화는 선택 사항입니다. **Panel**에 헤더 그룹화가 하나만 있을 수 있습니다. 헤더 그룹화는 패널 맨 위에 캡션으로 나타납니다.
    이 그룹화에서는 하나의 UocCaptionControl만 사용할 수 있습니다. 헤더 그룹화의 예제를 보려면 샘플 섹션을 참조하세요.

2.  **콘텐츠 그룹화**: 하나 이상의 콘텐츠 그룹화가 필요합니다. 하나의 Panel에 여러 개의 콘텐츠 그룹화가 있을 수 있습니다. 콘텐츠 그룹화는 RCDC 페이지의 기본 콘텐츠로 나타납니다. 각 콘텐츠 그룹화는 같은 패널에 탭으로 표시되고 1~256개의 컨트롤을 포함할 수 있습니다. **콘텐츠 그룹화**의 예제를 보려면 아래의 샘플 섹션을 참조하세요.

3.  **요약 그룹화**: 요약 그룹화는 선택 사항입니다. Panel에 요약 그룹화가 하나만 있을 수 있습니다. 요약 그룹화는 Panel의 마지막 탭으로 나타납니다. 사용자가 요청을 제출하기 전에 변경한 내용을 표시하기 위해 요약 그룹화에서 하나의 **UocHtmlSummary** 컨트롤만 사용할 수 있습니다. 요약 그룹화의 예제를 보려면 아래의 샘플 섹션을 참조하세요.

각 Grouping 형식에는 다음 요소가 포함되어 있습니다.

1.  **Help**: 이 요소는 탭의 도움말 텍스트를 제공합니다. 이 요소를 사용하여 탭의 도움말 파일에 대한 링크를 추가할 수도 있습니다.

2.  **Controls**: 이 요소에 대한 자세한 내용은 이 문서의 Control 섹션을 참조하세요. 각 그룹화에는 그룹화의 형식에 따라 1에서 256개 사이의 컨트롤이 있어야 합니다.

3.  **Events**: 이 요소에 대한 자세한 내용은 이 문서의 Events 섹션을 참조하세요. 각 그룹화에는 옵션으로 하나의 이벤트가 있을 수 있습니다. Grouping 요소에서 지원되는 Events는 다음과 같습니다.

    - **BeforeLeave**: 이 이벤트는 사용자가 콘텐츠 그룹화의 탭을 닫을 준비가 되면 트리거됩니다.
    - **AfterEnter**: 이 이벤트는 사용자가 콘텐츠 그룹화의 탭을 열 준비가 되면 트리거됩니다.

특성:

1.  **Name**: Grouping의 필수 이름입니다. **Name**은 **Panel** 내에서 고유해야 합니다.

2.  **Caption**: **Caption**은 헤더 그룹화에 헤더 캡션으로 표시됩니다. 콘텐츠 또는 요약 그룹화의 탭 캡션으로 표시됩니다.

3.  **Description**: 선택적 문자열 특성인 **Description**은 콘텐츠 그룹화에서 사용할 때만 작동합니다. 이 요소를 사용하여 최종 사용자에게 동일한 탭 내의 정보에 대한 일부 세부 정보를 제공합니다.

  >[!NOTE]
  이 특성이 요약 그룹화에 사용되는 경우 XML은 잘못된 것으로 간주됩니다. 이 특성이 헤더 그룹화에 사용되는 경우 XML은 유효한 것으로 간주되지만 무시됩니다.

4.  **Enabled**: 선택적 부울 특성으로, 누락되면 Enabled가 true로 설정됩니다. Enabled가 false로 설정되면 최종 사용자에게 Disabled 탭이 표시됩니다. 이 특성은 콘텐츠 그룹화에만 작동합니다.

  >[!NOTE]
  이 특성이 요약 그룹화에 사용되는 경우 XML은 잘못된 것으로 간주됩니다. 이 특성이 헤더 그룹화에 사용되는 경우 XML은 유효한 것으로 간주되지만 무시됩니다.

5.  **Visible**: 이 특성을 false로 설정하여 RCDC 페이지 탭이나 해당 머리글을 숨길 수 있습니다. 기본적으로 이 선택적 부울 형식 특성은 true로 설정됩니다. 이 특성은 콘텐츠 그룹화에만 작동합니다.

  >[!NOTE]
  Panel에 콘텐츠 그룹화가 하나만 있으면 이 기능이 작동하지 않습니다. Panel에 콘텐츠 그룹화가 2개 이상 있을 때는 위에서 설명한 것처럼 동작합니다.

6.  **IsHeader**: 이 특성은 그룹화가 헤더 그룹화인지 여부를 정의하는 선택적 부울 특성입니다. 이 특성을 지정하지 않으면 false로 설정됩니다.

7.  **IsSummary**: 이 특성은 그룹화가 요약 그룹화인지 여부를 정의하는 선택적 부울 특성입니다. 이 특성을 지정하지 않으면 false로 설정됩니다.

![RCD 구성 XML](media/rcd-configuration-xml-reference/image005.jpg)

다음 XML 샘플 코드에서는 이전 헤더 그룹화를 생성합니다. 헤더 그룹화는 샘플 헤더 그룹화라는 텍스트가 있는 영역입니다.

```XML
<!--Sample for a Header Grouping-->
<my:Grouping my:Name="HeaderGroupingSample" my:IsHeader="true">
     <my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Header Grouping">
          <my:Properties>
               <my:Property my:Name="MaxHeight" my:Value="32"/>
               <my:Property my:Name="MaxWidth" my:Value="32"/>
          </my:Properties>
      </my:Control>
</my:Grouping>
<!--End of Header Grouping Sample-->
```

![RCD 구성 XML](media\rcd-configuration-xml-reference/image007.jpg)

다음 XML 샘플 코드에서는 이전 콘텐츠 그룹화를 생성합니다. 콘텐츠 그룹화는 **샘플 헤더 그룹화**라는 텍스트가 있는 맨 왼쪽 탭입니다.

```XML
<!--Sample for a Content Grouping-->
<my:Grouping my:Name="ContentGroupingSample" my:Caption="Sample Content Grouping" my:Description="Some description for content grouping">
     <my:Control my:Name="DisplayName" my:TypeName="UocTextBox" my:Caption="Display name" my:Description="This is the display name of the set.">
          <my:Properties>
               <my:Property my:Name="Required" my:Value="True"/>
               <my:Property my:Name="MaxLength" my:Value="128"/>
               <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Content Grouping Sample-->
```

![RCD 구성 XML](media/rcd-configuration-xml-reference/image010.jpg)

다음 XML 샘플 코드에서는 이전 요약 그룹화를 생성합니다. 요약 그룹화는 **요약**이라는 텍스트가 있는 맨 오른쪽 탭입니다.

```XML
<!--Sample for a Summary Grouping-->
<my:Grouping my:Name="Summary" my:Caption="Sample Summary Grouping" my:IsSummary="true">
     <my:Control my:Name="SummaryControl" my:TypeName="UocHtmlSummary" my:ExpandArea="true">
          <my:Properties>
               <my:Property my:Name="ModificationsXml" my:Value="{Binding Source=delta, Path=DeltaXml}"/>
               <my:Property my:Name="TransformXsl" my:Value="{Binding Source=summaryTransformXsl, Path=/}"/>
          </my:Properties>
     </my:Control>
</my:Grouping>
<!--End of Summary Grouping Sample-->
```
### <a name="help"></a>도움말

Help 요소를 Grouping 또는 Control 요소에 옵션으로 포함할 수 있습니다. Grouping에서 사용하는 경우 사용되는 첫 번째 요소여야 합니다. 정확한 정보를 제공하는 데 도움을 주기 위해 최종 사용자에게 텍스트 도움말을 제공합니다. 다음은 Help 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Help">
     <xsd:complexType>
          <xsd:sequence/>
          <xsd:attribute ref="my:HelpText"/>
          <xsd:attribute ref="my:Link"/>
     </xsd:complexType>
</xsd:element>
```
아래에는 Help 요소의 예제가 나와 있습니다.
```XML
<my:Help my:HelpText="Some Help Text for Group Basic Info" my:Link="03e258a0-609b-44f4-8417-4defdb6cb5e9.htm#bkmk_grouping_GroupingBasicInfo" />
```

### <a name="control"></a>컨트롤

Grouping 요소는 하나 이상의 Control 요소를 포함합니다. Control은 RCDC의 주요 요소입니다. 포함된 다양한 Control 요소를 정의하여 Grouping 요소를 사용자 지정할 수 있습니다. 다음은 Control 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Control">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
               <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
          </xsd:sequence>
          <xsd:attribute ref="my:Name"/>
          <xsd:attribute ref="my:TypeName"/>
          <xsd:attribute ref="my:Caption"/>
          <xsd:attribute ref="my:Enabled"/>
          <xsd:attribute ref="my:Visible"/>
          <xsd:attribute ref="my:Description"/>
          <xsd:attribute ref="my:ExpandArea"/>
          <xsd:attribute ref="my:Hint"/>
          <xsd:attribute ref="my:AutoPostback"/>
          <xsd:attribute ref="my:RightsLevel"/>
     </xsd:complexType>
</xsd:element>
```

Control에는 다음 요소가 포함되어 있습니다.

1.  **Help**: 이 요소는 무시됩니다. Grouping에만 작동합니다.

2.  **CustomProperties**: 이 요소는 지원되지 않습니다.

3.  **Options**: 이 요소는 반드시 **UocDropDownList** 또는 **UocRadioButtonList** 컨트롤과 함께 사용되어야 합니다. 다른 컨트롤에서는 작동하지 않습니다. 이 요소의 구조에 대해서는 이 문서의 Options 섹션을 참조하세요. Control 컨텍스트에서 사용되는 방식을 확인하려면 개별 Control을 참조하세요.

4.  **Buttons**: 이 요소는 반드시 **UocListView** 컨트롤과 함께 사용되어야 합니다. 다른 컨트롤에서는 작동하지 않습니다. 자세한 내용은 이 문서의 UocListView 섹션을 참조하세요.

5.  Properties: 이 요소는 Control의 추가 동작을 지정하기 위해 모든 Control에서 사용됩니다. 이 요소에 대한 자세한 내용은 이 문서의 Properties 섹션을 참조하세요.

6.  **Events**: 이 요소의 구조에 대해서는 이 문서 앞에 나오는 Events 섹션을 참조하세요. 해당 컨트롤에 사용되는 이벤트를 확인하려면 개별 Control 정의를 참조하세요.

Control은 다음 특성을 포함합니다.

1.  **Name**: 컨트롤의 이름입니다. 컨트롤의 이름은 각 패널 내에서 고유해야 합니다. 필수 문자열 형식 특성입니다.

2.  **TypeName**: 이 특성은 컨트롤의 형식을 지정합니다. 필수 문자열 형식 특성입니다. 각 컨트롤 이름에 대해서는 이 설명서에 있는 개별 컨트롤 섹션을 참조하세요.

3.  **Caption**: 이 특성을 사용하여 컨트롤의 캡션을 포함할 수 있습니다.
    캡션은 일반적으로 컨트롤이 표시하거나 입력하는 데이터의 표시 이름입니다. 명시적으로 캡션 값을 지정하거나 스키마 특성 표시 이름 정보에 바인딩할 수 있습니다. 캡션은 보통 크기 컨트롤의 맨 왼쪽에 나타납니다. 컨트롤이 전체 화면에 걸쳐 있으면 캡션은 컨트롤 위에 나타납니다. 선택적인 문자열 형식 특성입니다. 특성 또는 속성 값에 데이터 소스를 바인딩하는 방법에 대한 자세한 내용은 속성 섹션을 참조하세요.

   다음 예제에서는 캡션을 명시적으로 사용할 수 있는 방법을 보여 줍니다.

   ```XML
      <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias">…<my:Control/>
   ```
     다음 예제에서는 캡션을 데이터 소스와 함께 사용할 수 있는 방법을 보여 줍니다. 이 문서 앞부분에 표시된 데이터 소스에 대해 템플릿을 사용한 경우 데이터 소스는 스키마입니다. 특성의 DisplayName을 Caption 특성에 바인딩하는 것이 좋습니다.

    ```XML
    <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}">…<my:Control/>
    ```
4.  Enabled: 선택적 부울 형식 특성입니다. 이 특성 값을 false로 설정하여 컨트롤을 사용하지 않도록 설정할 수 있습니다. 기본값은 true로 설정됩니다.

5.  Visible: 선택적 부울 형식 특성입니다. 이 특성을 사용하여 전체 컨트롤을 숨길 수 있습니다. 기본값은 true로 설정됩니다.

6.  Description: 이 선택적 문자열 형식 특성을 사용하여 최종 사용자가 컨트롤에 추가하는 항목 또는 컨트롤이 수행하는 작업을 이해하는 데 도움이 되는 설명을 포함합니다. 명시적으로 설명 값을 지정하거나 스키마 특성 설명 정보에 바인딩할 수 있습니다. <br/>설명은 캔션 아래의 보통 크기 컨트롤 맨 왼쪽에 나타납니다. 컨트롤이 전체 화면에 걸쳐 있으면 설명은 캡션 아래 컨트롤 위쪽에 나타납니다. 특성 또는 속성 값에 데이터 소스를 바인딩하는 방법에 대한 자세한 내용은 이 문서의 속성 섹션을 참조하세요.

7.  다음 예제에서는 설명을 명시적으로 사용할 수 있는 방법을 보여 줍니다.
  ```XML
  <my:Control my:Name="ExplicitAlias" my:TypeName="UocTextBox" my:Caption="Explicit Alias" my:Description="This is explicit description.">…<my:Control/>
  ```
  이 예제에서는 설명을 데이터 소스와 함께 사용할 수 있는 방법을 보여 줍니다. 이 문서 앞부분에 표시된 데이터 소스에 대해 템플릿을 사용한 경우 데이터 소스는 **스키마**입니다. 특성의 **Description**을 Description 특성에 바인딩하는 것이 좋습니다.
  ```XML
  <my:Control my:Name="DynamicAlias" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=Alias.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=Alias.Description, Mode=OneWay}">…<my:Control/>
  ````
8. ExpandArea: 이 특성은 컨트롤이 전체 화면에 걸쳐 있는지 여부를 나타냅니다. 선택적 부울 형식 특성입니다. 기본값은 false로 설정됩니다.

    >[!NOTE]
    이 특성이 true로 설정되면 Caption 및 Description 특성이 사용되지 않게 설정됩니다. 확장된 컨트롤에 대한 캡션을 제공하려면 UocLabel 컨트롤을 사용해야 합니다.
9. **Hint**: 선택적인 문자열 형식 특성입니다. Hint 특성의 텍스트를 통해 최종 사용자는 컨트롤에 대한 유효한 입력을 결정합니다. Hint는 컨트롤 아래에 나타납니다.

10.  **AutoPostback**: 선택적 부울 형식 특성입니다. 기본값은 false입니다. false로 설정된 경우 페이지를 새로 고쳐도 컨트롤은 새로 고쳐지지 않습니다. AutoPostback에 대한 자세한 내용은 동일한 이름의 Microsoft ASP.NET UI 컨트롤 속성을 참조하세요.

11. **RightsLevel**: 선택적인 문자열 형식 특성입니다. 이 특성을 데이터 소스와의 인라인 권한에만 바인딩할 수 있습니다. 이 컨트롤은 사용자의 권한에 따라 동적으로 켜지거나 사용되지 않도록 설정됩니다. 특성 또는 속성 값에 데이터 소스를 바인딩하는 방법에 대한 자세한 내용은 이 문서의 속성 섹션을 참조하세요.

    이 예제에서는 **RightsLevel** 특성을 데이터 소스와 함께 사용할 수 있는 방법을 보여 줍니다. 이 문서 앞부분에 표시된 데이터 소스에 대해 템플릿을 사용한 경우 데이터 소스는 **권한**입니다. 특성 이름을 Path로 사용합니다.

### <a name="properties"></a>속성

Property를 사용하여 각 컨트롤의 동작을 추가로 사용자 지정할 수 있습니다. Property는 빈 요소입니다. 다음은 Property 요소에 대한 XSD 스키마입니다.
```XML
<xsd:element name="Properties">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Property">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```



모든 속성에는 다음과 같은 2개의 필수 특성이 있습니다.

1.  **Name**: 이 문자열 형식 특성은 Property의 고유한 이름입니다.
    컨트롤마다 속성이 다릅니다. 모든 컨트롤에서 사용할 수 있는 몇 가지 일반적인 속성이 있습니다. 지정된 컨트롤에 어떤 이름을 사용할 수 있는지에 대한 자세한 내용은 공용 속성 및 개별 컨트롤 섹션을 참조하세요.

2.  **Value**: Property의 값을 지정합니다. 이 값의 데이터 형식은 해당 값이 할당된 속성에 따라 다릅니다. 특정 속성에 대해 허용되는 값 형식에 대해서는 다음 섹션을 참조하세요.

일부 속성은 데이터 소스의 정보에 바인딩될 수 있습니다. 이렇게 하려면 다음 문자열 형식을 사용해야 합니다. 데이터 소스에 바인딩하는 방법을 알아보려면 개별 컨트롤 섹션에서 개별 속성을 참조하세요.

```
<my:Property my:Name="Required" my:Value="[Formatted String]"/>

Formatted String :=  “{Binding “ + [SourceExpression] + “,” + [PathExpression] + “,” + [ModeExpression]? + “}”

SourceExpression:= “Source=” + [ObjectDataSourceName]

PathExpression:= “Path=” + [AttributeName]|[AttributePropertyName]

ModeExpression:= “Mode=” + [ModeChoice]

ModeChoice:= “OneWay”|”TwoWay”

ObjectDataSourceName:= The value of any string assign to node /ObjectControlConfiguration/ObjectDataSource/Name.

AttributeName:= valid schema attribute name from the data source.

AttributePropertyName:= valid property name of a schema attribute from the data source.

````
**예제:**

```XML
<my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
<my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>

```



<a name="common-properties"></a>공용 속성
-----------------

이 문서에 지정된 모든 RCDC 컨트롤에는 다음과 같은 공용 속성이 있을 수 있습니다. 이러한 속성을 다른 컨트롤에 관련된 다른 속성과 함께 사용할 수 있습니다.

1.  Required: 이 속성은 필드가 필수 필드이거나 선택적 필드임을 나타냅니다. 필수 필드에는 값을 입력해야 합니다. 문자열 입력의 경우 빈 값이 지원되지 않습니다. 선택적 필드는 비워 둘 수 있습니다. 필수 필드에 값을 입력하지 않으면 입력 컨트롤 위에 오류 메시지가 나타납니다. 필드가 필수인지 선택적인지 여부를 명시적으로 지정할 수 있습니다. 이 필드를 특성 및 리소스 형식 사이의 지정된 바인딩에 대한 스키마 정보에 바인딩할 수도 있습니다. 기본적으로 이 속성이 없으면 컨트롤은 선택적 입력 컨트롤인 것입니다.

   다음은 이 속성에 대해 명시적 값을 사용하는 예제입니다.

    ```XML
    <my:Property my:Name="Required" my:Value="True"/>
    ```
   이 예제에서는 이 속성에 대해 동적 데이터 소스를 사용합니다. 이 문서의 이전 섹션에 표시된 데이터 소스에 대해 템플릿을 사용한 경우 데이터 소스는 스키마입니다. \<특성 이름\>.Required를 경로로 사용합니다.

    ```XML
    <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required}"/>
    ```
2. **ReadOnly**: 이 속성을 true로 설정하면 사용자에게 해당 컨트롤이 읽기 전용 모드로 표시됩니다. 선택적 부울 형식 특성입니다.
    기본값은 false로 설정됩니다. 그러나 경우에 따라 이 속성의 동작은 사용자가 컨트롤의 데이터 바인딩에 대해 갖는 권한 유형에 의해 덮어쓰여집니다. 예를 들어 사용자에게 필드를 업데이트하기 위한 권한이 없고 필드가 인라인 권한에 바인딩된 경우 이 속성이 false로 설정되어 있더라도 데이터는 읽기 전용 모드로 표시됩니다.

3.  **RegularExpression**: 이 속성은 컨트롤의 값에 적용되는 제한을 지정합니다. 이 속성 값의 형식은 .NET StringRegex 표준에서 지원되는 형식입니다. 자세한 내용은 [.NET Framework 정규식](http://go.microsoft.com/fwlink/?LinkId=165361)을 참조하세요. 컨트롤이 값을 입력하는 데 사용되면 사용자가 현재 페이지를 나가려고 할 때 이 속성에 지정된 제한을 토대로 값이 확인됩니다.
    잘못된 값이 입력된 컨트롤 맨 위에는 오류 메시지가 나타납니다. 사용자는 문자열 정규식을 명시적으로 지정할 수 있습니다. 또한 해당 정규식을 지정된 특성의 스키마 정보에 바인딩할 수도 수 있습니다. 기본적으로 이 속성이 없으면 컨트롤은 입력 문자열에 대한 제한을 확인하지 않습니다.
    다음은 이 속성에 대해 명시적 값을 사용하는 예제입니다.

    ```XML
    <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
    ```
    이 예제에서는 이 속성에 대해 동적 데이터 소스를 사용합니다. 이 문서 앞부분에 표시된 데이터 소스에 대해 템플릿을 사용한 경우 데이터 소스는 스키마입니다. <attribute name>.StringRegex를 경로로 사용합니다.
    ```XML
    <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=Alias.StringRegex, Mode=OneWay}"/>
    ```
4.  Visible: 선택적 부울 형식 특성입니다. 이 특성을 사용하여 전체 컨트롤을 숨길 수 있습니다. 기본값은 true로 설정됩니다.

### <a name="options"></a>옵션

Options 요소는 하나 이상의 Option 하위 노드를 포함합니다. Options: 이 요소는 반드시 UocRadioButtonList 및 UocDropDownList 컨트롤과 함께 사용되어야 합니다. 사용 방법에 대한 자세한 내용은 개별 컨트롤 섹션을 참조하세요. 다음은 Options 요소에 대한 XSD 스키마입니다.

```XML
<xsd:element name="Options">
     <xsd:complexType>
          <xsd:sequence>
               <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
          </xsd:sequence>
     </xsd:complexType>
</xsd:element>
<xsd:element name="Option">
     <xsd:complexType>
          <xsd:simpleContent>
               <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
               </xsd:extension>
          </xsd:simpleContent>
     </xsd:complexType>
</xsd:element>
```


특성:

1.  Value: 문자열 형식의 필수 특성입니다. Value 특성은 동일한 컨트롤 내에서 고유해야 합니다. A-Z 사이의 대/소문자 구분 문자만 사용할 수 있습니다.

2.  Caption: 이 필수 특성은 각 옵션의 표시 이름입니다.

3.  Hint: 선택적 특성입니다. 이 특성을 사용하여 최종 사용자에게 자세한 정보와 힌트를 제공합니다.

### <a name="environment-variables"></a>환경 변수

다음 표의 환경 변수는 RCDC 구성에서 사용할 수 있습니다.

| 변수 | description |
|--------|--------|
| `<LoginID>`       | 현재 로그온된 사용자의 ID를 표시합니다.           |
| `<LoginDomain>`   | 현재 로그온된 사용자의 도메인을 표시합니다.       |
| `<Today>  `       | 현재 날짜 및 시간을 표시합니다.                                |
| `<FromToday_nnn>` | 현재 날짜와 nnn 및 시간을 표시합니다. nnn은 정수입니다.  |
| `<ObjectID> `     | RCDC 기본 리소스 ID입니다.                                     |
| `<Attribute_xxx>` | RCDC 기본 리소스의 지정된 특성 xxx를 반환합니다. |

### <a name="debugging-xml-configuration-files"></a>XML 구성 파일 디버그


RCDC에 대한 XML 구성 파일을 개발하거나 수정하는 경우 Microsoft Visual Studio® 같은 편집기를 사용하여 XSD 파일에 대해 XML의 유효성을 확인하여 오류를 줄일 수 있습니다. 자세한 내용은 [Visual Studio 2005의 XML 도구 소개](http://go.microsoft.com/fwlink/?LinkID=74512)를 참조하세요.

### <a name="customizing-a-help-file"></a>도움말 파일 사용자 지정

새 리소스 및 특성을 만드는 경우 FIM 포털의 기존 도움말 파일을 사용자 지정된 리소스에 대한 콘텐츠로 업데이트할 수 있습니다. FIM 포털의 도움말 파일은 .htm 형식이며 수동으로 편집할 수 있습니다.

>[!IMPORTANT]
사용자 지정 특성을 만드는 방법에 대한 자세한 내용은 FIM 2010 설명서의 사용자 지정 리소스 및 특성 관리 소개를 참조하세요.

>[!IMPORTANT]
이 섹션에서는 HTML 서식 지정 또는 편집 기본 사항에 대한 정보를 제공하지 않습니다. 도움말 파일을 수정하려면 HTML 편집 작업을 잘 알고 있어야 합니다.


**도움말 파일의 위치**: Microsoft Identity Management 2016 SP1 포털에 대한 모든 도움말 파일은 MIM 서비스 서버의 다음 폴더에 있습니다.

  `<ProgramFiles>\Common Files\Microsoft Shared\Web Server Extensions\12\Template\Layouts\MSILM2\Help\1033\html`

**적절한 도움말 파일을 찾는 방법**: FIM 포털에 대한 모든 도움말 파일 이름은 GUID(Globally Unique Identifier)입니다. 사용자 지정 리소스에 대한 올바른 파일을 찾으려면

1.  FIM 포털에서 사용자 지정하려는 포털 페이지의 도움말 파일을 엽니다.

2.  도움말 파일을 마우스 오른쪽 단추로 클릭한 후 **속성**을 클릭합니다.

3.  'URL 주소' 필드에서 `<GUID\>.htm` 파일을 강조 표시하고 복사합니다.

4.  도움말 파일이 저장된 폴더로 이동한 후 해당 파일을 검색합니다.

**새 특성에 대한 콘텐츠 추가**: 기존 Grouping 요소(탭) 내에 새 특성에 대한 설명 콘텐츠를 추가하려면

1.  적절한 도움말 파일 확인하고 찾습니다.

2.  HTML 편집기에서 파일을 엽니다.

3.  콘텐츠를 추가하려는 위치를 찾습니다. 일반적으로 다음과 같은 추가 단락입니다.

`<p xmlns="">A new paragraph with customized information.</p>`

다음 예제와 같이 기존 목록에 삽입된 항목일 수도 있습니다.
```
<li class="unordered"><b>First Name</b> – The first name of the User.<br>

<li class="unordered"><b>Last Name</b> - The last name of the User.<br>

<li class="unordered"><b>Added a new line</b><br>
```

**새 Grouping 요소에 대한 콘텐츠 추가**: 대부분의 FIM 포털 페이지에는 여러 Grouping 요소(또는 탭)가 있으며 함께 제공되는 도움말 파일에서 각 Grouping 요소와 관련된 섹션에는 책갈피가 지정되어 있습니다. HTML의 책갈피는 섹션에 지정됩니다. 예를 들어 다음은 FIM 포털의 사용자 만들기 페이지에 대한 도움말 파일의 작업 정보 탭에 대한 HTML입니다.

```
<a name="bkmk_grouping_WorkInfo" xmlns=""></a><h3 class="subHeading" xmlns="">Work Info</h3><p class="subHeading" xmlns=""></p><div class="subSection" xmlns="">
```

**사용자 만들기에 대한 구성** RCDC의 구성 데이터 XML 파일에 있는 Grouping 요소 **WorkInfo**에서 참조됩니다. `\<GUID\>.htm` 파일 이름 및 책갈피는 my:Link 매개 변수에 지정됩니다.

```
<my:Grouping my:Name="WorkInfo" my:Caption="%SYMBOL_WorkInfoTabCaption_END%" my:Enabled="true" my:Visible="true"> <my:Help my:HelpText="%SYMBOL_WorkInfoTabHelpText_END%" my:Link="5e18a08b-4b20-48b8-90c6-c20f6cbeeb44.htm#bkmk_grouping_WorkInfo"/>
```

**간단한 컨트롤 샘플**

다음 그림에서는 여러 다른 모드의 여러 간단한 텍스트 상자 컨트롤의 예제를 보여 줍니다.

예:

![](media/rcd-configuration-xml-reference/image016.gif)

다음 코드 세그먼트는 모든 특성 및 속성에 대한 명시적 텍스트를 사용하는 첫 번째 텍스트 상자 컨트롤을 만듭니다.

```
<!-- Sample for a simple control to use explicit information. (with hints)-->
<my:Control my:Name="ExplicitControl" my:TypeName="UocTextBox" my:Caption="Explicit Control" my:Description="This is explicit description." my:Hint="This is a Hint (enter any text).">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="RegularExpression" my:Value="[A-Z]*"/>
          <my:Property my:Name="Text" my:Value="Enter Information Here"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use explicit information.-->

```


다음 코드 세그먼트에는 동적 바인딩 방법을 사용하여 컨트롤을 다른 데이터 소스에 연결하는 두 번째 텍스트 상자 컨트롤을 만듭니다.

```
<!-- Sample for a simple control to use stored data information.-->
<my:Control my:Name="DynamicControl" my:TypeName="UocTextBox" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=DisplayName, Mode=OneWay}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=DisplayName.Required, Mode=OneWay}"/>
          <my:Property my:Name="RegularExpression" my:Value="{Binding Source=schema, Path=DisplayName.StringRegex, Mode=OneWay}"/>
          <my:Property my:Name="Text" my:Value="{Binding Source=object, Path=DisplayName, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple control to use stored data information.-->
```


다음 코드 세그먼트는 확장된 세 번째 레이블 및 텍스트 상자 컨트롤을 만듭니다.

```
<!-- Sample for a simple expanded control with caption control.-->
<my:Control my:Name="SampleExpandLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is an expanded control."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ExpandedControl" my:TypeName="UocTextBox"
          my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="Columns" my:Value="40"/>
          <my:Property my:Name="Text" my:Value="Expanded control (enter text)"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple expanded control.-->
```

다음 코드 세그먼트는 사용되지 않도록 설정된 네 번째 텍스트 상자 컨트롤을 만듭니다.
이 컨트롤은 사용 안 함 상태 및 사용 상태 간에 시각적으로 구분되지 않지만 텍스트 상자에 더 이상 데이터를 입력할 수 없습니다.

```
<!-- Sample for a simple disabled control.-->
<my:Control my:Name="DisabledControl" my:TypeName="UocTextBox" my:Caption="Disabled Control" my:Description="This is disabled simple control." my:Enabled="false">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="false"/>
          <my:Property my:Name="MaxLength" my:Value="128"/>
          <my:Property my:Name="Text" my:Value="Disabled control"/>
     </my:Properties>
</my:Control>
<!-- End of Sample for a simple disabled control.-->
```
## <a name="individual-controls"></a>개별 컨트롤

### <a name="uocbutton"></a>UocButton

**이름**: UocButton

**설명**: 특정 작업을 트리거하는 데 사용할 수 있는 간단한 단추 컨트롤입니다. 그러나 자체 처리기를 지정할 수 없으므로 이 컨트롤의 사용은 제한됩니다.

**속성**:

1.  **모든 공용 속성**: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **Text**: 이 속성은 단추에 표시되는 텍스트를 지정합니다. 선택적인 문자열 형식 특성입니다. Text는 명시적 문자열 값을 사용합니다.

이벤트:

   • **OnButtonClicked**: 이 이벤트는 단추를 클릭할 때 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image017.png)


다음 XML 세그먼트 다음과 같은 간단한 단추를 생성합니다.

```
<!--Sample enabled simple button control-->
<my:Control my:Name="ButtonControl" my:TypeName="UocButton" my:Caption="SampleButton" my:Description="This is a simple button."
my:Hint="Click the button">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="True"/>
          <my:Property my:Name="Text" my:Value="Click Me"/>
     </my:Properties>
</my:Control>
<!--End of sample enabled simple button control -->
```

### <a name="uoccaptioncontrol"></a>UocCaptionControl

**이름**: UocCaptionControl

**설명**: 이 컨트롤은 RCDC 페이지의 캡션을 표시하는 데 사용됩니다. 이 컨트롤은 헤더 그룹화에서 단일 컨트롤로만 사용하도록 디자인되었습니다.
다른 컨텍스트에서 이 컨트롤을 사용하면 렌더링 문제 또는 포털 오류가 발생할 수 있습니다.

**모드**: 읽기 전용(단방향)

**속성:**

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **MaxHeight:** 이 속성은 캡션 섹션에 있는 아이콘의 최대 높이를 지정합니다. 이 속성은 선택 사항입니다. 이 속성은 픽셀 정수 값을 사용합니다. 기본값은 32픽셀입니다.

예:

![](media/rcd-configuration-xml-reference/image020.jpg)

다음 코드 세그먼트에서는 **헤더 캡션**을 생성합니다.
```
<!--Sample header caption control-->
<my:Control my:Name="SampleHeaderCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Header Caption" my:Description="Description Starts here.">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="32"/>
          <my:Property my:Name="MaxWidth" my:Value="32"/>
     </my:Properties>
</my:Control>
<!--End of sample header caption control-->

```


이 코드 세그먼트에서는 **명시적 콘텐츠 캡션**을 생성합니다.

```
<my:Control my:Name="SampleContentCaption" my:TypeName="UocCaptionControl" my:ExpandArea="true" my:Caption="Sample Explicit Content Caption" my:Description="Explicit content caption with smaller icon">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="20"/>
          <my:Property my:Name="MaxWidth" my:Value="20"/>
     </my:Properties>
</my:Control>
<!--End of sample caption-->
```

다음 코드 세그먼트에서는 **표시 이름** 동적 캡션을 생성합니다.
```
<!--Sample content dynamic caption-->
<my:Control my:Name="Caption3" my:TypeName="UocCaptionControl" my:Caption="{Binding Source=schema, Path=DisplayName.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=DisplayName.Description, Mode=OneWay}"/>
<!--End of sample caption -->
```

### <a name="uoccheckbox"></a>UocCheckBox

**Name**: UocCheckBox

설명: 간단한 확인란 컨트롤입니다. 이 컨트롤을 부울 형식 데이터에 바인딩하는 것이 좋습니다. 이 컨트롤은 바인딩되는 데이터에 따라 읽기 전용 컨트롤 또는 업데이트 가능 컨트롤로 사용할 수 있습니다.

>[!NOTE]
이 릴리스에서는 편집 모드에서 이 확인란 컨트롤을 사용하여 부울 특성을 표시할 때 해당 특성에 이전에 값이 할당되지 않았으면 편집 모드에서 **확인**을 클릭할 때 Resource 컨트롤이 특성에 **false** 값을 추가합니다. 해결 방법은 존재하지 않는 것을 **false**와 같은 것으로 간주하는 부울 특성을 항상 만들거나 부울 특성에 대해 라디오 단추 등의 다른 컨트롤을 사용하는 것입니다.

**속성**:

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **DefaultValue**: 선택적 부울 형식 속성입니다. 기본값은 false로 설정됩니다. 이 필드는 확인란의 기본 동작을 지정합니다.
    명시적으로 지정할 수 있습니다.

3.  **Checked**: 선택적 부울 형식 속성입니다. 기본값은 false로 설정됩니다. 이 값을 DefaultValue와 함께 제공하면 DefaultValue 속성을 덮어씁니다. 이 필드는 확인란의 동작을 지정합니다. DefaultValue처럼 명시적으로 지정하거나 서버의 데이터와 바인딩할 수 있습니다.

4.  **Text**: 선택적인 문자열 형식 특성입니다. 텍스트는 확인란 오른쪽에 표시됩니다. 이 속성을 사용하여 최종 사용자에게 자세한 정보를 제공하는 텍스트를 지정할 수 있습니다.

**이벤트**:

   • CheckedChanged: 확인란이 해당 상태를 변경하는 경우 이 이벤트를 내보냅니다.

다음 예제에서는 사용자 지정 리소스 형식 및 **IsConfigurationType** 특성 간에 사용자 지정 바인딩이 생성됩니다. 사용자 지정 리소스 형식의 RCDC에 XML이 사용됩니다.

예:

![](media/rcd-configuration-xml-reference/image022.png)

다음 코드 세그먼트에서는 이전 그림에 나온 것과 같은 **동적 확인란**을 생성합니다. 이 바인딩 유형은 일반적으로 명시적 확인란보다 좀 더 다양하고 유용한 기능을 제공합니다. 해당 특성은 현재 리소스 형식에 속해야 합니다.

```
<!--Sample dynamic check box-->
<my:Control my:Name="SampleDynamicCheckBox" my:TypeName="UocCheckBox" my:Caption="Dynamic Check Box" my:Description="This is a dynamic check box. It saves to data source." my:RightsLevel="{Binding Source=rights, Path=IsConfigurationType}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="{Binding Source=schema, Path=IsConfigurationType.DisplayName, Mode=OneWay}"/>
          <my:Property my:Name="Checked" my:Value="{Binding Source=object, Path=IsConfigurationType, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample dynamic check box -->
```


### <a name="uoccommonmultivaluecontrol"></a>UocCommonMultiValueControl

**설명**: 특수 문자열 형식을 지원하는 여러 줄 텍스트 상자 컨트롤입니다. 다중 값 항목 중에서 각 값은 세미콜론으로 구분되고 텍스트 상자에서는 줄 바꿈으로 구분됩니다. 이 컨트롤을 다중 값의, 짧은 문자열 및 정수 형식에 바인딩하는 것이 좋습니다. 이 컨트롤은 읽기 전용 모드 및 업데이트 가능 모드를 모두 지원합니다.

**속성**:

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **DataType**: 필수 문자열 형식 특성입니다. 명시적으로 **문자열, 정수** 또는 **DateTime** 형식으로 지정할 수 있습니다. 이러한 특성을 스키마 특성의 **DataType** 속성에 바인딩할 수도 있습니다. 다중 값 참조 형식은 **UOCListView** 또는 **UOCIdentityPicker**로 처리되어야 합니다. 다중 값 부울은 지원되는 데이터 형식이 아닙니다.

3.  **Rows**: 선택적인 정수 형식 특성입니다. 상자 높이를 문자 수로 정의할 수 있습니다. 기본값은 1로 설정됩니다.

4.  **Columns**: 선택적인 정수 형식 특성입니다. 상자 너비를 문자 수로 정의할 수 있습니다. 기본값은 20으로 설정됩니다.
    20.

5.  **Value**: 선택적인 문자열 형식 특성입니다. 이 특성은 데이터 소스에만 바인딩할 수 있습니다.

이벤트

   • **ValueListChanged**: 이 이벤트는 컨트롤의 현재 값이 변경될 때 트리거됩니다.

다음 예제에서는 이름이 **AMultiValueString**인 다중 값 문자열 특성이 생성된 후 사용자 지정 리소스 형식에 바인딩됩니다. 이 예제는 이 바인딩을 만든 후에만 작동합니다.

**예:**

![](media/rcd-configuration-xml-reference/image024.jpg)

다음 코드 세그먼트에서는 이전 그림을 생성합니다.

```
<!--Sample multivalue control-->
<my:Control my:Name="SampleDynamicMultiValueControl" my:TypeName="UocCommonMultiValueControl" my:Caption="{Binding Source=schema, Path=AMultiValueString.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=AMultiValueString.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=AMultiValueString}">
     <my:Properties>
          <my:Property my:Name="Rows" my:Value="6"/>
          <my:Property my:Name="Columns" my:Value="60"/>
          <my:Property my:Name="DataType" my:Value="String"/>
          <!--not supported for above property my:Value={Binding Source=schema, Path=AMultiValueString.DataType, Mode=OneWay}"/>-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AMultiValueString, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of sample multivalue control -->
```



### <a name="uocdatetimecontrol"></a>UocDateTimeControl


**이름**: UocDateTimeControl

**설명**: 텍스트 상자 컨트롤과 비슷하지만 **Description**은 특정 형식만 수락합니다. 읽기 전용 모드에서는 레이블처럼 나타납니다. 지원되는 입력 문자열의 형식에 대해서는 이 섹션의 **DateTimeFormat** 속성을 참조하세요.

**속성**:

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **DateTimeFormat**: 선택적인 문자열 형식 특성입니다. 지원되는 형식은 DateTime 또는 DateOnly입니다. 기본값은 DateTime 형식으로 설정됩니다.
    a. DateTime 형식: 이 특성은 서식이 mm/dd/yyyy hh:mm:ss AM으로 지정됩니다.

      <[!NOTE]
      사용자에 따라 그 차이를 지정하는 경우도 있지만 **DateTime** 및 **DateOnly** 형식 둘 다 지원됩니다.
3.  **Value**: 선택적인 문자열 형식 특성입니다. 이 특성을 리소스 데이터 소스에 바인딩합니다. 이 특성의 값은 올바른 datetime 형식을 따라야 합니다.

이벤트

   • **DateTimeChanged**: datetime 값이 변경되면 이 이벤트가 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image027.jpg)

다음 코드 세그먼트에서는 첫 번째 **DateTime** 컨트롤을 생성합니다.

```
<!--Sample explicit DateTime control-->
<my:Control my:Name="SampleExplicitDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="Explicit Date Time Control" my:Description="The data shown here is explicit and in date time format.">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateTime"/>
          <my:Property my:Name="Value" my:Value="11/11/2008 00:00:00"/>
     </my:Properties>
</my:Control>
<!--End of sample explicit DateTime control -->
```


다음 코드 세그먼트에서는 두 번째 **DateTime** 컨트롤을 생성합니다. 데이터 소스 섹션의 샘플 코드를 사용한 경우 **ExpirationTime** 특성은 모든 리소스 형식에 바인딩됩니다. 따라서 다음 코드와 함께 사용할 수 있습니다.

```
<!--Sample dynamic DateTime control-->
<my:Control my:Name="SampleDynamicDateTimeControl" my:TypeName="UocDateTimeControl" my:Caption="{Binding Source=schema, Path=ExpirationTime.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ExpirationTime.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ExpirationTime}">
     <my:Properties>
          <my:Property my:Name="DateTimeFormat" my:Value="DateOnly"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExpirationTime, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic explicit DateTime control -->
```



### <a name="uocdropdownlist"></a>UocDropDownList

**이름**: UocDropDownList

설명: 간단한 드롭다운 상자 컨트롤입니다. 이 컨트롤은 정의된 선택 항목 집합에서 옵션을 선택하려는 경우에 주로 사용됩니다. 문자열, 정수, datetime 및 부울 데이터 형식이 이 컨트롤에 적합합니다.

**속성**:

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **ValuePath**: ItemSource에서 Value 특성을 가져오기 위한 속성입니다. ItemSource를 Custom으로 지정하면 ValuePath는 Value로 설정됩니다. 이 속성은 이 문서 뒷부분에 정의된 Option 요소의 Value 필드에 바인딩됩니다.

3.  **CaptionPath**: ItemSource에서 Value 특성을 가져오기 위한 속성입니다. ItemSource를 Custom으로 지정하면 ValuePath는 Caption으로 설정됩니다. 이 속성은 이 문서 뒷부분에 정의된 Option 요소의 Caption 필드에 바인딩됩니다.

4.  **HintPath**: ItemSource에서 Value 특성을 가져오기 위한 속성입니다. ItemSource를 Custom으로 지정하면 ValuePath는 Hint로 설정됩니다. 이 속성은 이 문서 뒷부분에 정의된 Option 요소의 Hint 필드에 바인딩됩니다.

5.  **ItemSource**: 목록의 선택 항목을 정의하는 ListControlItem 컬렉션입니다. 이 속성을 명시적으로 Custom으로 설정하고 Option 요소를 사용하여 문자열 값을 지정할 수 있습니다.

6.  **SelectedValue**: 현재 선택된 값입니다. 필수 문자열 형식 속성입니다. 이 속성은 데이터 소스의 문자열 데이터에 바인딩됩니다.

이벤트

  • SelectedIndexChanged: 드롭다운 상자의 선택이 변경될 때 이 이벤트가 발생합니다.

옵션:

Option 요소 구조에 대해서는 이 문서의 “Option” 섹션을 참조하세요.

1.  **Value**: 단일 Option 요소의 Value는 이 컨트롤이 바인딩되는 데이터 소스의 유효한 입력인 임의 문자열로 설정될 수 있습니다.

2.  **Caption**: Caption은 임의 문자열 값일 수 있습니다.

3.  **Hint**: Hint는 임의 문자열 값일 수 있습니다.

예:

![](media/rcd-configuration-xml-reference/image030.jpg)


![](media/rcd-configuration-xml-reference/image031.jpg)

>[!NOTE]
이 샘플이 작동되려면 기존 문자열 형식 특성 **Scope**를 RCDC가 적용되는 사용자 지정 리소스 형식에 바인딩해야 합니다.


이 코드 세그먼트에서는 드롭다운 목록을 생성합니다.

```
<!--Sample for drop-down list control-->
<my:Control my:Name="Scope" my:TypeName="UocDropDownList" my:Caption="{Binding Source=schema, Path=Scope.DisplayName}" my:RightsLevel="{Binding Source=rights, Path=Scope}">
     <my:Options>
          <my:Option my:Value="DomainLocal" my:Caption="Domain Local" my:Hint="to secure a local resource (i.e. a file share on your computer)" />
          <my:Option my:Value="Global" my:Caption="Global" my:Hint="to secure resources across your team or division" />
          <my:Option my:Value="Universal" my:Caption="Universal" my:Hint="to use this group across your organization" />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Scope.Required" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="ItemSource" my:Value="Custom" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=Scope, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for drop-down list control-->
```


### <a name="uocfiledownload"></a>UocFileDownload

이름: UocFileDownload

설명: 이 컨트롤에는 하이퍼링크가 포함되어 있습니다. 하이퍼링크를 클릭하면 Windows 파일 저장 페이지가 나타납니다. 사용자는 파일을 로컬 드라이브에 저장할 수 있습니다.
Internet Explorer에서 해당 파일 형식을 렌더링할 수 있는 경우 열기 옵션도 지원됩니다. 이 컨트롤과 함께 사용하도록 권장되는 데이터 형식은 형식 문자열(XML) 및 이진 형식입니다.

>[!NOTE]
이 Microsoft Identity Manager 2016 SP1 릴리스에서는 파일을 연 Internet Explorer 창을 닫은 다음 페이지를 새로 고쳐야 합니다. Internet Explorer 창을 새로 고친 후에 다운로드를 시작하여 같은 파일을 저장하거나 원래 창에서 다시 열 수 있습니다.


속성:

1.  **모든 공용 속성**: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  **Text**: 하이퍼링크 텍스트를 정의하는 선택적 문자열 형식 특성입니다. 이 속성에 대한 명시적 문자열을 지정할 수 있습니다.

3.  **Value**: 필수 특성입니다. 해당 콘텐츠를 다운로드할 서버에서 특성 바인딩을 지정합니다.

4.  **PromptedFileName**: 선택적인 문자열 형식 특성입니다. 다운로드한 파일을 저장할 때 제안되는 파일 이름입니다.

5.  **ContentType**: 필수 문자열 형식 특성입니다. 데이터가 저장되는 파일 형식입니다. 텍스트 또는 이진이 지원되는 두 가지 문자열 옵션입니다. 텍스트인 경우 반환 값은 긴 문자열로 간주됩니다.
    그렇지 않고 이진인 경우 반환 값은 byte[]로 간주됩니다. 텍스트를 선택하는 경우 사용자는 한 가지 옵션으로 텍스트의 형식을 지정하기 위한 접미사를 추가할 수 있습니다. 예를 들어 text/xml을 추가할 수 있습니다.


>[!NOTE]
이 컨트롤에 바인딩된 값이 비어 있으면 다운로드 작업을 트리거하는 데 사용할 하이퍼링크가 컨트롤에 없습니다. 다운로드할 항목이 없기 때문입니다.


**이벤트**:

이 컨트롤에 대한 이벤트는 없습니다.

**예제**:

![](media/rcd-configuration-xml-reference/image035.png)


>[!NOTE]
이 샘플 파일을 업로드하기 전에 사용자는 사용자 지정 리소스 형식과 기존 ConfigurationData 특성 간에 바인딩을 만들어야 합니다.


다음 코드 세그먼트에서는 이전 그림의 파일 다운로드 컨트롤을 생성합니다.

```
<!--Sample dynamic download control-->
<my:Control my:Name="SampleDynamicFileDownloadControl" my:TypeName="UocFileDownload" my:Caption="{Binding Source=schema, Path=ConfigurationData.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ConfigurationData.Description, Mode=OneWay}" my:RightsLevel="{Binding Source=rights, Path=ConfigurationData}">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="Download Dummy xml"/>
          <my:Property my:Name="PromptedFileName" my:Value="DummyXML.xml"/>
          <my:Property my:Name="ContentType" my:Value="text/xml"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ConfigurationData}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic download control -->
```


### <a name="uocfileupload"></a>UocFileUpload

**이름**: UocFileUpload

**설명**: 이 컨트롤에는 업로드될 로컬 파일의 위치, 찾아보기 파일 단추 및 업로드 단추를 표시하는 텍스트 상자가 포함되어 있습니다. 최종 사용자가 찾아보기 단추를 클릭하면 Windows 파일 열기 창이 나타납니다. 최종 사용자는 로컬 드라이브에서 업로드할 하나의 파일을 선택할 수 있습니다. 파일을 선택하면 파일의 위치가 텍스트 상자에 표시됩니다. 업로드 단추를 클릭하면 클라이언트 쪽 로컬 데이터 소스에 파일이 업로드됩니다. 파일 콘텐츠는 아직 서버로 제출되지 않습니다. 이 컨트롤을 사용하도록 권장되는 데이터 형식은 형식 문자열(XML) 또는 이진 형식입니다.

>[!NOTE]
업로드 진행률 또는 상태는 표시되지 않습니다. 로컬 데이터 소스로 파일이 업로드되면 텍스트 상자가 선택 취소됩니다.


속성:

1.  모든 공용 속성: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  Value: 필수 특성입니다. 데이터 업로드된 서버의 스키마 특성 바인딩을 지정합니다.

3.  ContentType: 선택적인 문자열 형식 특성입니다. 서버에서 파일에 저장되는 데이터 형식입니다. Text 또는 Binary로 설정할 수 있습니다. 이 속성이 없을 경우 기본값은 Binary입니다.

4.  MaxFileSize: 선택적인 문자열 형식 특성입니다. MaxFileSize는 업로드한 파일 크기를 정의합니다. 기본적으로 이 속성이 누락되면 최대 크기는 1MB(메가바이트)입니다.

5.  PromptedForNoValue: 선택적 문자열 형식 특성입니다. 파일 업로드되지 않을 때 사용자에게 표시되는 텍스트를 정의합니다.

이벤트

   • FileUploaded: 파일을 성공적으로 업로드되면 이 이벤트가 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image040.png)

>[!NOTE]
다음 샘플 코드가 작동되려면 ABinaryAttribute라는 새 이진 형식 특성을 만들고 사용자 지정 리소스 형식과 이 특성 간에 새 바인딩을 만들어야 합니다.


다음 코드 세그먼트에서는 이전 그림의 업로드 컨트롤을 생성합니다.
```
<!--Sample dynamic upload control-->
<my:Control my:Name="SampleDynamicFileUploadControl" my:TypeName="UocFileUpload" my:Caption="{Binding Source=schema, Path=ABinaryAttribute.DisplayName, Mode=OneWay}" my:Description="{Binding Source=schema, Path=ABinaryAttribute.Description, Mode=OneWay}” my:RightsLevel="{Binding Source=rights, Path=ABinaryAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABinaryAttribute.Required}"/>
          <my:Property my:Name="ContentType" my:Value="Binary"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ABinaryAttribute, Mode=TwoWay}"/>
     </my:Properties>
</my:Control>
<!--End of dynamic upload control -->
```

### <a name="uocfilterbuilder"></a>UocFilterBuilder

이름: UocFilterBuilder

설명: MIM 2016 XPath 식을 렌더링 수 있도록 하는 복합 컨트롤입니다. 일부 XPath 식은 지원되지 않습니다. 필터 작성기를 사용하는 방법에 대한 자세한 내용은 필터 작성기에 대한 도움말을 참조하세요.

속성:

1.  모든 공용 속성: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  PermittedObjectTypes: 필터 작성기의 select 문에 표시될 리소스 형식의 목록을 정의합니다. 필터 작성기를 사용하는 방법에 대한 자세한 내용은 필터 작성기 도움말을 참조하세요. 이 문자열은 ResourceTypeA, ResourceTypeB 형식입니다. 여기서 각 리소스 형식은 쉼표(,)로 구분됩니다.

3.  Value: 필터 작성기가 렌더링될 때 사용되는 값입니다.
    XPath 식이 포함된 문자열 형식 데이터와의 바인딩만 지원됩니다. Filter 특성은 이 컨트롤을 바인딩하기 위한 권장 특성입니다.

4.  PreviewButtonVisible: 선택적 부울 형식 속성입니다. 이 속성을 false로 설정하는 경우 미리 보기 단추가 표시되지 않습니다. 기본값은 true로 설정됩니다. 이 단추는 XPath 식의 결과를 미리 볼 수 있도록 하기 위해 목록 뷰 컨트롤과 함께 사용할 수 있습니다.

5.  ExcludeGroupMembership: 부울 속성입니다. 이 속성이 true로 설정되면 \<그룹 개체\>의 멤버인 \<참조 특성\>(예: ResourceID)을 사용하는 필터를 만들 수 없습니다. 즉, 이 속성이 true이면 그룹 멤버 자격 디렉터리를 사용하는 필터를 만들 수 없습니다.

6.  PreviewButtonCaption: 선택적 문자열입니다. PreviewButtonVisible이 true로 설정되면 이 속성을 사용하여 단추에 사용자 지정된 텍스트를 제공할 수 있습니다. 해당 텍스트는 미리 보기 단추에 표시됩니다.

이벤트

   • OnFilterChanged: 이 이벤트는 필터 작성기 콘텐츠가 변경될 때 트리거됩니다.

예:

![](media/rcd-configuration-xml-reference/image044.png)

>[!NOTE]
이 샘플 코드를 사용하기 전에 기존 Filter 특성 및 사용자 지정 리소스 형식 간에 새 바인딩을 만듭니다.


다음은 UOCLabel 컨트롤, PermittedObjectTypes을 갖는 간단한 필터 작성기 및 미리 보기 목록 뷰를 포함하는 샘플 코드입니다. 목록 뷰 ListFilter 속성 및 필터 작성기 Value 속성을 연결하려면 이 두 항목이 이 동일한 데이터 소스 특성을 가리켜야 합니다.

```
<!--Sample filter builder with preview list-->
<my:Control my:Name="ComplexFilterBuilderLabel" my:TypeName="UocLabel" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="Text" my:Value="This is a Filter Builder with preview."/>
     </my:Properties>
</my:Control>
<my:Control my:Name="ComplexFilterBuilder" my:TypeName="UocFilterBuilder" my:RightsLevel="{Binding Source=rights, Path=Filter}" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="PermittedObjectTypes" my:Value="Person,Group" />
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Filter, Mode=TwoWay}" />
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Filter.Required, Mode=OneWay}" />
     </my:Properties>
</my:Control>
<my:Control my:Name="FilterBuilderwithpreview" my:TypeName="UocListView" my:ExpandArea="true">
     <my:Properties>
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName,ObjectType,AccountName" />
          <my:Property my:Name="EmptyResultText" my:Value="There is no members according to the filter definition." />
       <my:Property my:Name="PageSize" my:Value="10" />
       <my:Property my:Name="ShowTitleBar" my:Value="false" />
       <my:Property my:Name="ShowActionBar" my:Value="false" />
       <my:Property my:Name="ShowPreview" my:Value="false" />
       <my:Property my:Name="ShowSearchControl" my:Value="false" />
       <my:Property my:Name="EnableSelection" my:Value="false" />
       <my:Property my:Name="SingleSelection" my:Value="false" />
       <my:Property my:Name="ItemClickBehavior" my:Value=" ModelessDialog "/>
       <my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}" />
     </my:Properties>
</my:Control>
<!--end of sample filter builder with preview-->
```


<a name="uochtmlsummary"></a>UocHtmlSummary
--------------

이름: UocHtmlSummary

설명: 이 컨트롤을 사용하여 RCDC 페이지의 요약 페이지를 정의할 수 있습니다.
이 요약 페이지는 최종 사용자가 요청을 제출한 후에 나타납니다. 이 컨트롤은 요약 그룹화에만 사용할 수 있으며 유일한 컨트롤이어야 합니다. 반드시 제공되는 샘플 코드를 사용하는 것이 좋습니다.

>[!NOTE]
이 컨트롤은 광범위하게 테스트되지 못했습니다.


속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  ModificationsXml: 이 속성은 {Binding Source=delta, Path=DeltaXml}로 형식이 지정되어야 합니다. 여기서 delta는 구성 헤더 ObjectDataSource에 정의됩니다.

3.  TransformXsl: 이 속성은 일반적으로 {Binding Source=summaryTransformXsl, Path=/}로 형식이 지정됩니다. 여기서 summaryTransformXsl은 구성 헤더 XmlDataSource에 정의됩니다.

이 컨트롤의 기존 샘플을 보려면 이 문서 앞부분의 Grouping 섹션에서 "요약 그룹화"를 참조하세요.

### <a name="uochyperlink"></a>UocHyperLink

이름: UocHyperLink

설명: 간단한 하이퍼링크 컨트롤입니다. 이 컨트롤을 사용하여 정보를 하이퍼링크로 표시할 수 있습니다.

속성:

1.  모든 공용 속성: 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  ObjectReference: 선택적 참조 형식 속성입니다. 유효한 리소스가 이 속성에 정의된 GUID에 의해 참조되면 하이퍼링크는 리소스에 액세스하는 방법을 제공합니다. 이 속성은 NavigateUrl 속성(아래 참조)과 함께 사용할 수 없습니다.

3.  Text: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 하이퍼링크로 표시되는 텍스트를 정의합니다.

4.  NavigateUrl: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 하이퍼링크가 연결되는 전체 경로 URL을 정의합니다. 이 속성은 ObjectReference 속성(위 참조)과 함께 사용할 수 없습니다.

예:

![](media/rcd-configuration-xml-reference/image049.jpg)

>[!NOTE]
연결할 리소스의 올바른 GUID가 필요합니다. 이 경우 올바른 GUID를 사용하여 두 번째 하이퍼링크가 생성됩니다. 첫 번째는 임의 웹 사이트일 수 있습니다.


다음 코드 세그먼트에서는 리디렉션 하이퍼링크가 생성됩니다.

```
<!--Sample for a hyperlink that redirects page.-->
<my:Control my:Name="RedirectHyperlink" my:TypeName="UocHyperLink" my:Caption="Redirect Hyperlink" my:Description="This is a hyperlink that takes you to other pages.">
     <my:Properties>
          <my:Property my:Name="NavigateUrl" my:Value="http://www.microsoft.com"/>
          <my:Property my:Name="Text" my:Value="Microsoft Home Page"/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that redirect page-->

```

다음 코드 세그먼트에서는 리소스를 참조하는 하이퍼링크가 생성됩니다. 데이터 소스에 바인딩하기 위해 명시적 참조를 식 {Binding Source=object, Path=Creator}로 바꿀 수 있습니다. 리소스 관리자가 있고 참조 형식 값일 때만 유효할 수 있습니다.

```
<!--Sample for a hyperlink that reference object-->
<my:Control my:Name="ReferenceHyperlink" my:TypeName="UocHyperLink" my:Caption="Reference Hyperlink" my:Description="This is a hyperlink gives you an object view of the reference object">
     <my:Properties>
          <my:Property my:Name="ObjectReference" my:Value="e4e048b1-9e43-415e-806c-cf44c429c34c"/>
          <my:Property my:Name="Text" my:Value="View a group in FIM 2010."/>
     </my:Properties>
</my:Control>
<!--End of Sample for a hyperlink that reference object-->
```

### <a name="uocidentitypicker"></a>UocIdentityPicker

이름: UocIdentityPicker

설명: 이 컨트롤은 선택적 확인 상자 및 찾아보기 창으로 구성됩니다. 선택적 확인 상자는 ID를 입력하기 위한 선택적 텍스트 상자, ID를 확인하기 위한 확인 단추, 팝업 찾아보기 창을 프롬프트하기 위한 찾아보기 단추로 구성됩니다. 찾아보기 창에서 목록 뷰 컨트롤을 통해 ID를 선택할 수 있습니다. 찾아보기 창에서 선택한 ID가 확인 상자에 반영됩니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  UsageKeywords: 선택적 문자열 속성입니다. SearchScopeConfiguration 구조체가 지원하는 사용 키워드 목록을 제공하여 리소스 선택에서 사용할 검색 범위 목록을 정의할 수 있습니다. 여기서 각 키워드는 (’)로 구분됩니다.

3.  Filter: 선택적 문자열 속성입니다. 사용자는 정의된 범위 내에 맞는 항목만 표시하도록 리소스 선택의 범위를 지정하는 XPath 식을 제공합니다. 이 속성은 UsageKeywords 속성(위 참조)과 함께 사용할 수 없습니다. 검색 범위가 적용되면 이 속성은 아무 효과가 없습니다.

4.  ResultObjectType: 선택적 문자열 속성입니다. 이 리소스 형식은 팝업 대화 상자 목록에서 리소스를 렌더링하는 데 사용됩니다. ID 선택이 Filter에 의해 반환되는 리소스 형식을 식별하고 그에 따라 데이터를 렌더링하는 데 도움을 주기 위해 Filter와 함께 사용됩니다. 이 속성은 UsageKeywords 속성(위 참조)과 함께 사용할 수 없습니다. 검색 범위가 적용되면 이 속성은 아무 효과가 없습니다. 이 속성에 허용되는 문자열은 유효한 모든 단일 리소스 형식 이름(예: Person)입니다.
    필터가 여러 리소스 형식을 반환할 것으로 예상되면 Resource가 사용됩니다.

5.  PreviewTitle: 목록 뷰에 사용되는 미리 보기 제목입니다. 이 속성에 대한 자세한 내용은 UocListView 섹션을 참조하세요.

6.  ListViewTitle: 선택적 문자열 속성입니다. 이 속성을 사용하여 목록 뷰 맨 위에 표시되는 텍스트를 제목으로 정의할 수 있습니다.

7.  Value: 선택적 문자열 속성입니다. 이 속성을 스키마 특성에 바인딩하여 해당 값을 데이터 소스에 연결하는 것이 좋습니다.

8.  Mode: 선택적 문자열 속성입니다. 이 속성을 사용하여 ID 선택에서 1개의 값만 선택할 수 있는지 또는 여러 ID를 선택할 수 있는지를 정의합니다. SingleResult 및 MultipleResult가 허용되는 값입니다. 기본적으로 SingleResult로 설정됩니다.

9.  ObjectTypes: 선택적 문자열 형식 속성입니다. 최종 사용자가 ID 선택 확인 상자에서 항목을 확인하는 기준으로 사용할 수 있는 리소스 형식 목록을 정의할 수 있습니다. 이 목록은 쉼표(,)로 구분된 리소스 형식 이름 목록으로 구성됩니다.

10. AttributesToSearch: 선택적 문자열 형식 속성입니다. ID 선택에서 항목을 확인하는 데 사용할 수 있는 특성 목록을 정의할 수 있습니다. 여기서 목록은 쉼표(,)로 구분된 스키마 특성 목록입니다. 예를 들어 AttributesToSearch가 DisplayName, Alias로 설정되면 사용자가 DisplayName = \<검색 값\> 또는 Alias=\<검색 값\>을 포함하는 항목을 검색할 수 있습니다. 여기에는 Value에 지정된 데이터 소스의 대상 리소스 형식에 대한 유효한 특성 이름을 지정하는 것이 좋습니다. 대상 리소스 형식은 ObjectTypes 필드에는 찾을 수 있습니다. 모든 특성은 ObjectTypes 필드에 언급된 지정된 모든 리소스 형식에서 유효해야 합니다.

11. ColumnsToDisplay: 선택적 문자열 형식 속성입니다. 사용자는 쉼표(,)로 구분된 스키마 특성 이름 목록을 제공합니다. 여기에 정의된 특성은 ID 선택에서 목록 뷰의 열을 구성합니다.

12. Rows: 선택적 정수 속성입니다. Mode가 MultipleResult로 설정된 경우에만 작동합니다. 이 속성을 사용하여 확인 텍스트 상자의 높이를 지정된 크기(문자 단위)로 설정합니다.

13. MainSearchScreenText: 선택적 문자열 형식 속성입니다. 찾아보기 창에서 검색이 실행되는 동안 표시되는 사용자 지정된 텍스트입니다.

이벤트

 • SelectedObjectChanged: 이 이벤트는 사용자가 선택된 리소스를 변경할 때 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image052.png)

>[!NOTE]
이 샘플이 작동하려면 Manager 특성과 이 XML이 적용되는 임의 사용자 지정 리소스 형식 간에 새로운 바인딩을 만들어야 합니다.


다음 코드 세그먼트에서는 Filter 및 ResultObjectType 속성을 RCDC의 일부로 사용하여 단일 모드에서 ID 선택을 생성합니다.

```
<!--Sample for a single-selection identity picker using Filter and Result Object Type-->
<my:Control my:Name="SingleSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A Single Selection Identity Picker" my:Description="The user is allowed to select only one entry here." my:RightsLevel="{Binding Source=rights, Path=Manager}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=Manager.Required}"/>
          <my:Property my:Name="Mode" my:Value="SingleResult" />
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--single valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=Manager , Mode=TwoWay}" />
          <!--Scoping the list explicitly to All Persons name contains letter "e"-->
          <my:Property my:Name="Filter" my:Value="/Person[contains(JobTitle, 'Manager')]"/>
          <!--Result object type specify the type is Person-->
          <my:Property my:Name="ResultObjectType" my:Value="Person"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select only one entry" />
          <my:Property my:Name="PreviewTitle" my:Value="Entry selected:" />
     </my:Properties>
</my:Control>
<!--End of sample for a single-selection identity picker.-->
```



예:

![](media/rcd-configuration-xml-reference/image056.jpg)

>[!NOTE]
이 샘플 코드가 작동하려면 ExplicitMember 특성(다중 값 참조 특성)을 사용자 지정 리소스 형식에 바인딩해야 합니다. 또한 UsageKeyword 속성을 Person 및 Group으로 설정하고 검색 범위를 만들어야 합니다.


다음 코드 세그먼트에서는 이전 그림의 컨트롤을 만듭니다.

```
<!--Sample for a multiselection Identity Picker uses Search Scope-->
<my:Control my:Name="multiSelectionIdentityPicker" my:TypeName="UocIdentityPicker" my:Caption="A multi Selection Identity Picker" my:Description="The user is allowed to select more than one entry here" my:RightsLevel="{Binding Source=rights, Path=ExplicitMember}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ExplicitMember.Required}"/>
          <my:Property my:Name="Mode" my:Value="MultipleResult" />
          <my:Property my:Name="Rows" my:Value="10" />
          <!--There are existing search scopes that has key word "Person" and "Group" use both sets of search scopes here.-->
          <my:Property my:Name="UsageKeywords" my:Value="Person,Group"/>
          <!--Columns displayed in list view in pop-up window-->
          <my:Property my:Name="ColumnsToDisplay" my:Value="DisplayName, ObjectType" />
          <!--Identities will be resolved against following attribute in the resolve textbox when resolve button is clicked.-->
          <my:Property my:Name="AttributesToSearch" my:Value="DisplayName, AccountName" />
          <!--multi valued reference type attribute is used to bind the control-->
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=ExplicitMember , Mode=TwoWay}" />
          <my:Property my:Name="ResultObjectType" my:Value="Resource"/>
          <my:Property my:Name="ListViewTitle" my:Value="Select multiple entries" />
          <my:Property my:Name="PreviewTitle" my:Value="Entries selected" />
     </my:Properties>
</my:Control>
<!--End of sample for a multiselection Identity Picker.-->
```


### <a name="uoclabel"></a>UocLabel

이름: UocLabel

설명: 간단한 읽기 전용 텍스트 레이블 컨트롤입니다. 이 컨트롤은 읽기 전용 데이터를 표시하는 데 사용하는 것이 좋습니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  Text: 문자열 형식 특성입니다. 명시적 문자열 값을 제공하거나 데이터 소스에 바인딩하여 이 속성을 정의합니다. 이 속성의 값을 할당하는 샘플 바인딩은 {Binding Source=object, Path=\<올바른 특성 이름\>입니다.

UocLabel 컨트롤의 샘플을 보려면 간단한 컨트롤 샘플 섹션에서 간단한 컨트롤을 참조하세요.

### <a name="uoclistview"></a>UocListView

이름: UocListView

설명: 고급 목록 뷰 컨트롤입니다. 간단한 목록 뷰, 선택적 단순 검색, 선택적 고급 검색 컨트롤, 선택적 선택 미리 보기 상자 및 작업 단추 모음으로 구성됩니다. 선택적 단순 검색은 검색 범위 및 단순 검색 텍스트 상자로 구성됩니다. 고급 검색 컨트롤은 필터 작성기입니다. 목록 뷰에는 미리 렌더링된 리소스 목록이 표시됩니다. 이 컨트롤의 검색 컨트롤에서 얻은 검색 결과를 표시할 수도 있습니다. 작업 단추 모음은 목록 뷰의 선택 항목에 따라 수행할 수 있는 작업을 정의합니다. 선택 미리 보기 상자에는 목록 뷰에서 선택된 항목이 표시됩니다.

>[!IMPORTANT]
UocListView는 단일 값 참조 특성에 작동하지 않습니다. 다중 값 참조 특성에서만 사용할 수 있습니다. 단일 값 참조 특성에 대해서는 이 문서의 UocIdentityPicker를 참조하세요.


속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  SelectedValue: 일반적으로 GUID 형식 문자열 목록을 수락하는 다중 값 참조 특성에 바인딩되는 선택적 문자열 형식 속성입니다.

3.  PageSize: 선택적 정수 속성입니다. 사용자는 목록 뷰 컨트롤에서 한 페이지에 맞는 항목의 개수를 지정할 수 있습니다. 기본값은 10개 항목입니다. 양의 정수는 유효합니다.

4.  UsageKeyword: 선택적 문자열 형식 속성입니다. 사용자는 목록 뷰 검색 컨트롤에 사용되는 검색 범위를 정의하는 키워드 목록을 지정할 수 있습니다. FIM 2010 서버에는 검색 범위 리소스가 있습니다. UsageKeyword라는 SearchScopeConfiguration 구조체에 대한 특성은 검색 범위 집합을 그룹화하는 데 사용됩니다. 목록 뷰는 해당 키워드 목록을 사용합니다. 각 키워드는 쉼표(,)로 구분됩니다.
    이 목록 뷰에 표시하려는 해당 검색 범위에 사용되는 UsageKeyword입니다. ShowSearchControl 속성이 true로 설정된 경우에만 적용됩니다.

5.  SearchControlAutoPostback: 선택적 부울 속성입니다. 검색이 트리거될 때 자동 포스트백을 수행하려면 이 속성의 값을 true로 설정합니다. 기본적으로 SearchControlAutoPostback은 false로 설정됩니다.

6.  EmptyResultText: 선택적 문자열 형식 속성입니다. 기본적으로 No로 설정되면 임의 문자열 값으로 설정할 수 있습니다. 이 텍스트는 검색 결과가 비어 있을 때 나타납니다.

7.  ButtonHeight: 선택적 정수 형식 속성입니다. 이 속성의 값을 양의 정수 값으로 설정합니다. 이 속성은 작업 모음의 단추 높이(픽셀)를 정의합니다. 기본값은 32픽셀입니다.

8.  ButtonWidth: 선택적 정수 형식 속성입니다. 이 속성의 값을 양의 정수 값으로 설정합니다. 이 속성은 작업 모음의 단추 너비(픽셀)를 정의합니다. 기본값은 32픽셀입니다.

9.  CaptionImageMaxHeight: 선택적 정수 형식 속성입니다. 이 속성의 값을 양의 정수로 설정합니다. 이 속성은 선택적 캡션의 최대 아이콘 높이를 정의합니다. 기본값은 32픽셀입니다.

10. CaptionImageMaxWidth: 선택적 정수 형식 속성입니다. 이 속성의 값을 양의 정수로 설정합니다. 이 속성은 선택적 캡션의 최대 아이콘 너비를 정의합니다. 기본값은 32픽셀입니다.

11. CaptionImageUrl: 선택적 문자열 형식 속성입니다. 이 속성은 이미지에 연결되는 URL을 캡션 이미지로 정의합니다.

12. PreviewTitle: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 선택 미리 보기 상자 위에 표시되는 텍스트를 정의합니다.

13. EnableSelection: 선택적 부울 형식 속성입니다. 이 속성을 사용하여 목록 뷰가 선택 모드인지 여부를 정의합니다. 목록 뷰가 선택 모드이면 확인란의 열이 목록 뷰 맨 왼쪽 열에 나타나고 선택 미리 보기 상자가 목록 뷰 맨 아래에 나타납니다. 이 속성의 기본값은 true로 설정됩니다.

14. SingleSelection: 선택적 부울 형식 속성입니다. 목록 뷰에 대해 선택 모드가 켜져 있으면 이 값을 true로 설정할 경우 최종 사용자는 목록에서 하나의 항목만 선택하도록 제한됩니다. 기본적으로 이 속성의 값은 false로 설정됩니다. 즉, 기본적으로 최종 사용자는 목록에서 여러 항목을 선택할 수 있습니다.

15. RedirectUrl: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 목록에서 하이퍼링크로 연결된 항목을 클릭할 때 리디렉션할에 페이지를 지정합니다. 이 URL에는 런타임 중에 실제 값으로 대체되는 자리 표시자가 포함될 수 있습니다. 자리 표시자는 다음과 같습니다.

     ◦ {0} - objectType

     ◦ {1} - objectID

     ◦ {2} - displayName

16.  ShowTitleBar: 선택적 부울 형식 속성입니다. 이 속성을 사용하여 제목 표시줄을 표시할지 여부를 지정합니다. 이 속성의 기본값은 false입니다.

17.  ShowActionBar: 선택적 부울 형식 속성입니다. 이 속성을 사용하여 작업 모음 영역을 표시할지 여부를 지정합니다. 이 속성의 기본값은 true입니다.

18.  ShowPreview: 선택적 부울 형식 속성입니다. 이 속성을 사용하여 미리 보기 영역을 표시할지 여부를 지정합니다. 이 속성의 기본값은 true입니다.

19.  ShowSearchControl: 선택적 부울 형식 속성입니다. 이 속성을 사용하여 검색 컨트롤을 표시할지 여부를 지정합니다. 이 속성의 기본값은 true입니다.

20.  ResultObjectType: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 검색 결과의 예상된 개체 형식을 지정합니다. 이 속성의 기본값은 Resource입니다. 검색 결과에 여러 리소스 형식이 포함되어 있으면 이 값을 Resource로 지정해야 합니다.

21.  ColumnsToDisplay: 선택적 속성입니다. 이 속성을 사용하여 이 목록 뷰에서 열로 표시할 특성을 지정합니다. 이 속성의 기본값은 DisplayName, ResourceType입니다. 각 열은 특성의 시스템 이름으로 표시됩니다. 각 열은 쉼표(,)로 구분됩니다. 목록 뷰가 선택 모드에서 사용될 때 이 속성에 대한 값을 지정할 필요가 없습니다. 선택 모드에서 열 설정은 현재 선택된 검색 범위의 SearchScopeColumn 특성에서 가져옵니다.

22.  ListFilter: 선택적 문자열 형식 속성입니다. 목록 뷰를 렌더링 하는 사용되는 xpath이며 ShowSearchControl 속성이 false로 설정된 경우에만 적용됩니다. 이 값을 지정하는 경우 목록 뷰는 쿼리에 대해 이 속성 값을 사용하고 목록 뷰는 선택 모드에 있지 않습니다. 필터는 다음과 같이 리소스의 문자열 특성에 바인딩되거나<br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="{Binding Source=object, Path=Filter}"/>` <br/> <br/>
     다음과 같이 미리 정의된 일부 환경 변수를 포함하는 문자열일 수 있습니다. <br/><br/>
     `<my:Property my:Name="ListFilter" my:Value="/Approval[Request=''%ObjectID%'']"/>`

23.  TargetAttribute: 더 이상 사용되지 않는 속성입니다. 해당 값은 다중 값 참조 특성의 시스템 이름이어야 합니다. 이 속성은 사용하지 않는 것이 좋습니다. 예를 들어 그룹 관리에서는 다음 대신

  `<my:Property my:Name="TargetAttribute" my:Value="ExplicitMember"/>` <br/>

  다음을 사용하는 것이 좋습니다. `<my:Property my:Name=”ListFilter” my:Value=”/Group[ObjectID=’%ObjectID%’]/ExplicitMember”/>` <br/><br/>
24.  ItemClickBehavior: 선택적 문자열 형식 속성입니다. 이 속성을 사용하여 목록 뷰 항목을 클릭하여 서버 포스트백을 트리거할지 또는 항목의 상세 뷰를 표시할지를 지정합니다. 지원되는 2가지 옵션은 ModelessDialog 및 Server입니다. 기본값은 ModelessDialog입니다.

25.  SearchOnLoad: 목록 뷰 컨트롤이 큐에 로드 대기 중인지 여부를 지정하는 선택적 부울 형식 속성입니다. 이 속성은 목록 뷰가 선택 모드에 있을 때만 적용됩니다. 이 속성에 대한 기본값은 true입니다. 사용자가 의미 있는 결과를 얻기 위해 검색에 일반적으로 텍스트를 입렫하도록 하려면 이 속성을 끌 수 있습니다. 이 경우 초기에 목록 뷰에는 사용자에게 검색을 수행하는 방법을 알리는 메시지가 표시됩니다. 이 텍스트는 다음과 같은 속성으로 사용자 지정할 수 있습니다.

26.  MainSearchScreenText: 이 선택적 문자열 형식 속성은 SearchOnload가 true로 설정된 경우에만 적용 가능합니다. 이 속성을 사용하여 목록 뷰가 자동으로 검색을 수행하지 않을 때 목록 뷰 중간에 나타나는 텍스트를 사용자 지정할 수 있습니다. 이 속성의 기본값은 위의 검색을 사용하여 원하는 리소스를 Find하는 것입니다. 텍스트를 시나리오에 더 적합하게 만드는 값을 지정할 수 있습니다.

27.  SubSearchScreenText: 이 선택적 문자열 형식 속성은 MainSearchScreenText 아래에 나타나는 텍스트를 사용자 지정하는 데 사용됩니다. 일반적으로 목록 뷰를 사용하는 방법에 대한 몇 가지 지침을 더 추가하려는 경우가 아니면 이 속성 값을 지정할 필요가 없습니다.

UocFilterBuilder 컨트롤과 함께 목록 뷰를 미리 보기 목록으로 사용하는 방법의 예제를 보려면 이 문서의 앞부분에서 UocFilterBuilder 샘플을 참조하세요. UocListView를 필터 작성기 없이 사용할 수도 있습니다.

### <a name="uocnumericbox"></a>UocNumericBox

이름: UocNumericBox

설명: 정수 값만 사용하는 간단한 텍스트 상자입니다. 이 컨트롤은 읽기 전용 모드 및 업데이트 가능 모드를 모두 지원합니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  MaxValue: 선택적 정수 형식 속성입니다. 이 속성을 사용하여 컨트롤에 대한 클라이언트 쪽 유효성 검사를 정의합니다. 최종 사용자가 입력하는 값은 이 값을 초과할 수 없습니다. 명시적 정수를 입력하거나 {Binding Source=schema Path=IntegerMaximum}을 사용하여 이 속성을 데이터 소스의 정수 데이터에 바인딩할 수 있습니다.

3.  MinValue: 선택적 정수 형식 속성입니다. 이 속성을 사용하여 컨트롤에 대한 클라이언트 쪽 유효성 검사를 정의합니다. 최종 사용자가 입력하는 값은 이 값보다 낮을 수 없습니다. 명시적 정수를 입력하거나 {Binding Source=schema, Path=IntegerMinimum}을 사용하여 이 속성을 데이터 소스의 정수 데이터에 바인딩할 수 있습니다.

4.  DefaultValue: 선택적 정수 형식 속성입니다. 이 속성을 사용하여 컨트롤이 새 데이터를 만드는 데 사용되는 경우 컨트롤에 대한 기본값을 정의합니다. 이 값은 반드시 명시적으로 정적 정수로 설정해야 합니다.

5.  Value: 선택적 정수 형식 속성입니다. 이 속성을 데이터 소스의 정수 형식 데이터에 바인딩하면 해당 특성 값이 페이지가 로드될 때 나타나고 제출 후에 데이터 소스에 저장됩니다.

처리기:

   • TextChanged: 이 이벤트는 컨트롤 내 현재 값이 변경될 때 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image061.jpg)

>[!NOTE]
다음 샘플 코드에서는 첫 번째 숫자 상자를 생성합니다. 숫자 상자는 데이터 소스 또는 스키마 정보에 연결되지 않습니다.

```
<!--Sample for an explicit Numeric Box-->
<my:Control my:Name="SampleExplicitNumericBox" my:TypeName="UocNumericBox" my:Caption="An Explicit NumericBox" my:Description="This is a dummy numeric box that is not linked with data source.">
     <my:Properties>
          <my:Property my:Name="MinValue" my:Value="1"/>
          <my:Property my:Name="MaxValue" my:Value="100"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
     </my:Properties>
</my:Control>
<!--End of sample for an explicit Numeric Box.-->

```

다음 샘플 코드에서는 두 번째 숫자 상자를 생성합니다.

>[!NOTE]
이 샘플이 작동하려면 먼저 AnIntegerAttribute라는 새 정수 형식 특성을 만든 후 사용자 지정 리소스 형식에 바인딩합니다.

```
<!--Sample for a dynamically rendered numeric box-->
<my:Control my:Name="SampleDynamicNumericBox" my:TypeName="UocNumericBox" my:Caption="{Binding Source=schema, Path=AnIntegerAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=AnIntegerAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=AnIntegerAttribute}">
     <my:Properties>
          <my:Property my:Name="MaxValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMaximum}"/>
          <my:Property my:Name="MinValue" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.IntegerMinimum}"/>
          <my:Property my:Name="DefaultValue" my:Value="1"/>
          <my:Property my:Name="Value" my:Value="{Binding Source=object, Path=AnIntegerAttribute, Mode=TwoWay}"/>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=AnIntegerAttribute.Required}"/>
     </my:Properties>
</my:Control>
<!--End of sample for a dynamically numeric box.-->
```


### <a name="uocpicturebox"></a>UocPictureBox

이름: UocPictureBox

설명: 이 컨트롤은 그림, 이진 형식 데이터를 렌더링하는 데 사용됩니다. 이 컨트롤은 이진 형식 데이터와 함께 사용하는 것이 좋습니다. 그림을 제공된 이미지 URL, 이진 형식 데이터 또는 그림 형식 데이터가 포함된 특성 소스로 렌더링할 수 있습니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  ImageUrl: 선택적 문자열 형식 속성입니다. 대상 그림의 URL을 입력합니다.

3.  MaxHeight: 선택적 문자열 형식 속성입니다. 렌더링할 이미지의 최대 높이(픽셀)를 정의합니다.

4.  MaxWidth: 선택적 문자열 형식 속성입니다. 렌더링할 이미지의 최대 너비(픽셀)을 정의합니다.

5.  ImageData: 이진 형식 속성입니다. 이 속성을 사용하여 데이터 소스를 표시된 이미지에 바인딩합니다. 바인딩된 데이터 소스는 이진이어야 합니다.
    이 필드에 byte[] 형식의 데이터를 제공하여 그림을 명시적으로 설정할 수도 있습니다.

6.  ImageResource: 선택적 이진 형식 속성입니다.

7.  AlternativeText: 선택적 문자열 형식 속성입니다. 이 속성은 그림을 표시할 수 없는 경우 대체 텍스트로 나타납니다.

샘플:

>[!NOTE]
이 샘플을 사용하려면 기존 이미지 데이터를 컨트롤에 바인딩해야 합니다.


다음 코드 세그먼트에서는 데이터 소스를 컨트롤에 바인딩하는 그림 상자 컨트롤을 생성합니다.

```
<!--Sample for a Picture Box control binding with a data source-->
<my:Control my:Name="SamplePictureBoxImageData" my:TypeName="UocPictureBox" my:RightsLevel="{Binding Source=rights, Path=Photo}">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageData" my:Value="{Binding Source=object, Path=Photo}" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

다음 코드 세그먼트에서는 URL 이미지를 컨트롤에 바인딩하는 그림 상자 컨트롤을 생성합니다.

```
<!--Sample for a Picture Box control bind with explicit URL-->
<my:Control my:Name="SamplePictureBoxImageUrl" my:TypeName="UocPictureBox">
     <my:Properties>
          <my:Property my:Name="MaxHeight" my:Value="100" />
          <my:Property my:Name="MaxWidth" my:Value="100" />
          <my:Property my:Name="ImageUrl" my:Value="http://www.microsoft.com/dummypicture.jpg" />
     </my:Properties>
</my:Control>
<!--End of Sample for a Picture Box control-->
```

### <a name="uocradiobuttonlist"></a>UocRadioButtonList

이름: UocRadioButtonList

설명: 간단한 옵션 단추 목록입니다. 이 목록에서는 반드시 1개의 선택 항목만 선택할 수 있습니다. 이 컨트롤은 선택할 옵션이 5개 이하일 때만 권장됩니다. 그렇지 않으면 UOCListView가 권장됩니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  ValuePath: 값 경로는 Value로 설정됩니다. 이 문서에 정의된 Option 요소의 Value 필드에 바인딩됩니다.

3.  CaptionPath: 값 경로는 Caption으로 설정됩니다. 이 문서에 정의된 Option 요소의 Caption 필드에 바인딩됩니다.

4.  CaptionPath: 값 경로는 Hint로 설정됩니다. 이 문서에 정의된 Option 요소의 Hint 필드에 바인딩됩니다.

5.  SelectedValue: 현재 선택된 값입니다. 필수 문자열 형식 속성입니다. 이 속성은 데이터 소스의 문자열 데이터에 바인딩됩니다.

이벤트

1.  SelectedIndexChanged

2.  CheckedChanged

옵션:

이 컨트롤에 대한 옵션에는 2개의 Option 요소만 있을 수 있습니다.

1.  Value: 단일 Option 요소의 Value 필드는 True 또는 False로 설정해야 합니다.

2.  Caption: 임의의 문자열 값일 수 있습니다.

3.  Hint: 임의의 문자열 값일 수 있습니다.

예:

![](media/rcd-configuration-xml-reference/image063.jpg)

>[!NOTE]
이 샘플이 작동하려면 새로운 부울 특성 ABooleanAttribute를 만들고 사용자 지정 리소스 형식에 바인딩해야 합니다.

다음 코드 세그먼트에서는 이전 그림의 옵션 단추 목록을 만듭니다.

```
<!--Sample for option button list control-->
<my:Control my:Name="SampleRadioButtonList" my:TypeName="UocRadioButtonList" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Options>
          <my:Option my:Value="False" my:Caption="Set Value To False" my:Hint="By selecting this option, you are setting the value of the attribute to false." />
          <my:Option my:Value="True" my:Caption="Set Value To True" my:Hint="By selecting this option, you are setting the value of the attribute to true." />
     </my:Options>
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="ValuePath" my:Value="Value" />
          <my:Property my:Name="CaptionPath" my:Value="Caption" />
          <my:Property my:Name="HintPath" my:Value="Hint" />
          <my:Property my:Name="SelectedValue" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for option button list control-->
```


### <a name="uocsimpleradiobutton"></a>UocSimpleRadioButton


이름: UocSimpleRadioButton

설명: 간단한 옵션 단추 컨트롤입니다. 이 컨트롤은 간단한 확인란과 비슷하게 사용합니다. 두 개의 옵션 단추가 텍스트 레이블과 나란히 표시됩니다. 컨트롤을 부울 형식 데이터에 바인딩하는 것이 좋습니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  TrueText: 선택적 문자열 형식 속성입니다. 옵션 단추를 선택할 때 표시되는 텍스트입니다.

3.  FalseText: 선택적 문자열 형식 속성입니다. 옵션 단추를 선택하지 않을 때 표시되는 텍스트입니다.

4.  FalseText: 선택적 부울 형식 속성입니다. 이 값은 옵션 단추가 선택되어 있음을 나타냅니다. 데이터 소스의 부울 형식 데이터에 바인딩될 수 있습니다. 기본값은 false로 설정됩니다.

이벤트

   • CheckedChanged: 옵션 단추 상태가 선택됨에서 선택 취소됨으로 바뀌거나 그 반대로 바뀔 경우 이 신호가 발생합니다.

예:

![](media/rcd-configuration-xml-reference/image066.png)

>[!NOTE]
샘플이 작업하려면 새 부울 특성 ABooleanAttribute를 만들고 사용자 지정 리소스 형식에 바인딩해야 합니다. RCDC 데이터는 동일한 사용자 지정 리소스 형식에 적용됩니다.

다음 코드 세그먼트에서는 이전 그림의 옵션 단추를 생성합니다.

```
<!--Sample for simple option button control-->
<my:Control my:Name="SampleSimpleRadioButton" my:TypeName="UocSimpleRadioButton" my:Caption="{Binding Source=schema, Path=ABooleanAttribute.DisplayName}" my:Description="{Binding Source=schema, Path=ABooleanAttribute.Description}" my:RightsLevel="{Binding Source=rights, Path=ABooleanAttribute}">
     <my:Properties>
          <my:Property my:Name="Required" my:Value="{Binding Source=schema, Path=ABooleanAttribute.Required}" />
          <my:Property my:Name="FalseText" my:Value="False"/>
          <my:Property my:Name="TrueText" my:Value="True"/>
          <my:Property my:Name="SelectedItem" my:Value="{Binding Source=object, Path=ABooleanAttribute, Mode=TwoWay}" />
     </my:Properties>
</my:Control>
<!--End of Sample for simple option button control-->
```


### <a name="uoctextbox"></a>UocTextBox

이름: UocTextBox

설명: 문자열 형식 입력을 지원하는 단순 텍스트 상자입니다. 이 컨트롤을 사용하여 문자열 형식 데이터에 바인딩하는 것이 좋습니다.

속성:

1.  모든 공용 속성: 이 속성에 대한 자세한 내용은 이 문서의 공용 속성 섹션을 참조하세요.

2.  MaxLength: 선택적 정수 형식 특성입니다. 이 속성은 문자열 입력에 대한 최대 길이를 지정합니다. 이 속성의 기본값은 128자입니다.

3.  Text: 선택적 문자열 형식 속성입니다. 텍스트 상자에 표시되는 텍스트입니다. 컨트롤의 초기 로드 동안 텍스트 상자에 표시되는 명시적 문자열을 정의하거나 문자열 형식의 스키마 특성에 바인딩해야 합니다.

4.  Rows: 선택적 정수 형식 속성입니다. 이 속성은 텍스트 상자의 높이를 문자 단위로 정의합니다. 기본값은 1자입니다.

5.  Columns: 선택적 정수 형식 속성입니다. 이 속성은 텍스트 상자의 너비를 문자 단위로 정의합니다. 기본값은 20자입니다.

6.  Wrap: 선택적 부울 형식 속성입니다. 이 속성 값을 true로 설정하면 사용자가 텍스트 상자에서 자동 줄 바꿈 기능을 사용하도록 설정합니다. 이 속성의 기본값은 true로 설정됩니다.

7.  UniquenessValidationXPath: 선택적 문자열 형식 속성입니다. 이 속성은 유효한 FIM XPath 필터 식을 사용하고 사용자가 입력한 값이 필터 범위에 있는 리소스 내에서 고유한지 확인합니다.
    예를 들어 사용자 요청 표시 이름이 FIM 서비스 DB의 모든 메일 사용 가능 보안 그룹 내에서 고유하도록 하려면 XPath `/Group[DisplayName=’%VALUE%’ and Type=’MailEnabledSecurity’`를 사용해야 합니다. 유효성 검사 작업은 사용자가 해당 페이지에서 나갈 때 수행됩니다. 이 속성은 리소스 생성을 위한 RCDC에서만 지원됩니다.

8.  UniquenessErrorMessage: 선택적 문자열 형식 속성입니다. 이 문자열은 UniquenessValidationXPath 유효성 검사가 실패할 경우 오류 메시지를 표시하는 데 사용되며, 명시적 텍스트이거나 문자열 리소스 변수일 수 있습니다. 이 속성을 지정하지 않으면 실패한 유효성 검사에 대한 기본 오류 메시지는 "%VALUE%이(가) 이미 있습니다. 다른 값을 사용하세요."입니다.

이벤트

   • TextChanged: 이 이벤트는 텍스트 상자 내의 텍스트가 변경될 때 발생합니다.

이 컨트롤의 전체 샘플에 대해서는 간단한 컨트롤 샘플 섹션을 참조하세요.

## <a name="appendix-a-default-xsd-schema"></a>부록 A: 기본 XSD 스키마

다음은 Microsoft Identity Manager 2016 SP1에 제공된 모든 기본 RCDC에 대한 전체 XSD 스키마입니다.

```XML
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<xsd:schema targetNamespace="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:my="http://schemas.microsoft.com/2006/11/ResourceManagement" xmlns:xd="http://schemas.microsoft.com/office/infopath/2003" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <xsd:attribute name="TypeName" type="my:requiredString"/>
    <xsd:attribute name="Name" type="my:requiredAlphanumericString"/>
    <xsd:attribute name="Parameters" type="xsd:string"/>
    <xsd:attribute name="DisplayAsWizard" type="xsd:boolean"/>
    <xsd:attribute name="Caption" type="xsd:string"/>
    <xsd:attribute name="AutoValidate" type="xsd:boolean"/>
    <xsd:attribute name="Enabled" type="xsd:string"/>
    <xsd:attribute name="Visible" type="xsd:string"/>
    <xsd:attribute name="IsSummary" type="xsd:boolean"/>
    <xsd:attribute name="IsHeader" type="xsd:boolean"/>
    <xsd:attribute name="HelpText" type="xsd:string"/>
    <xsd:attribute name="Link" type="xsd:string"/>
    <xsd:attribute name="Description" type="xsd:string"/>
    <xsd:attribute name="ExpandArea" type="xsd:boolean"/>
    <xsd:attribute name="Hint" type="xsd:string"/>
    <xsd:attribute name="AutoPostback" type="xsd:string"/>
    <xsd:attribute name="RightsLevel" type="my:requiredString"/>
    <xsd:attribute name="Value" type="xsd:string"/>
    <xsd:attribute name="Handler" type="my:requiredString"/>
    <xsd:attribute name="ImageUrl" type="xsd:string"/>
    <xsd:attribute name="RedirectUrl" type="xsd:string"/>
    <xsd:attribute name="ClickBehavior" type="xsd:string"/>
    <xsd:attribute name="EnableMode" type="xsd:string"/>
    <xsd:attribute name="ValueType" type="xsd:string"/>
    <xsd:attribute name="Condition" type="xsd:string"/>
    <xsd:element name="ObjectControlConfiguration">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:ObjectDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:XmlDataSource" minOccurs="0" maxOccurs="32"/>
                <xsd:element ref="my:Panel"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:anyAttribute processContents="lax" namespace="http://www.w3.org/XML/1998/namespace"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="ObjectDataSource">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="XmlDataSource">
        <xsd:complexType  mixed="true">
      <xsd:sequence>
        <xsd:any minOccurs="0" maxOccurs="unbounded" processContents="lax"/>
      </xsd:sequence>
      <xsd:attribute ref="my:Name"/>
      <xsd:attribute ref="my:Parameters"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Panel">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Grouping" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:DisplayAsWizard"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:AutoValidate"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Grouping">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Control" minOccurs="1" maxOccurs="256"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:IsHeader"/>
            <xsd:attribute ref="my:IsSummary"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Help">
        <xsd:complexType>
            <xsd:sequence/>
            <xsd:attribute ref="my:HelpText"/>
            <xsd:attribute ref="my:Link"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Control">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Help" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:CustomProperties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Options" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Buttons" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Properties" minOccurs="0"  maxOccurs="1"/>
                <xsd:element ref="my:Events" minOccurs="0" maxOccurs="1"/>
            </xsd:sequence>
            <xsd:attribute ref="my:Name"/>
            <xsd:attribute ref="my:TypeName"/>
            <xsd:attribute ref="my:Caption"/>
            <xsd:attribute ref="my:Enabled"/>
            <xsd:attribute ref="my:Visible"/>
            <xsd:attribute ref="my:Description"/>
            <xsd:attribute ref="my:ExpandArea"/>
            <xsd:attribute ref="my:Hint"/>
            <xsd:attribute ref="my:AutoPostback"/>
            <xsd:attribute ref="my:RightsLevel"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="CustomProperties">
        <xsd:complexType mixed="true">
            <xsd:sequence>
                <xsd:any minOccurs="0" maxOccurs="unbounded" namespace="##targetNamespace" processContents="lax"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Options">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Option" minOccurs="0" maxOccurs="unbounded"/>
            </xsd:sequence>
            <xsd:attribute ref="my:ValueType"/>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Option">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Buttons">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Button" minOccurs="1" maxOccurs="8"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Button">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Caption"/>
                    <xsd:attribute ref="my:Hint"/>
                    <xsd:attribute ref="my:ImageUrl"/>
                    <xsd:attribute ref="my:ClickBehavior"/>
                    <xsd:attribute ref="my:RedirectUrl"/>
                    <xsd:attribute ref="my:Enabled"/>
                    <xsd:attribute ref="my:Visible"/>
                    <xsd:attribute ref="my:EnableMode"/>
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Properties">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Property" minOccurs="1" maxOccurs="32"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Property">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Value"/>
                    <xsd:attribute ref="my:Condition"/>                 
                </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Events">
        <xsd:complexType>
            <xsd:sequence>
                <xsd:element ref="my:Event" minOccurs="1" maxOccurs="16"/>
            </xsd:sequence>
        </xsd:complexType>
    </xsd:element>
    <xsd:element name="Event">
        <xsd:complexType>
            <xsd:simpleContent>
                <xsd:extension base="xsd:string">
                    <xsd:attribute ref="my:Name"/>
                    <xsd:attribute ref="my:Handler"/>
          <xsd:attribute ref="my:Parameters"/>
        </xsd:extension>
            </xsd:simpleContent>
        </xsd:complexType>
    </xsd:element>
    <xsd:simpleType name="requiredString">
        <xsd:restriction base="xsd:string">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
  <xsd:simpleType name="requiredAlphanumericString">
    <xsd:restriction base="xsd:string">
      <xsd:pattern value="[A-Za-z0-9_]{1,128}"/>
    </xsd:restriction>
  </xsd:simpleType>
    <xsd:simpleType name="requiredAnyURI">
        <xsd:restriction base="xsd:anyURI">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
    <xsd:simpleType name="requiredBase64Binary">
        <xsd:restriction base="xsd:base64Binary">
            <xsd:minLength value="1"/>
        </xsd:restriction>
    </xsd:simpleType>
</xsd:schema>
```



## <a name="appendix-b-default-summary-xsl"></a>부록 B: 기본 요약 XSL

다음은 Microsoft Identity Manager 2016 SP1에 제공된 전체 요약 XSL입니다.
```XML
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform" xmlns:msxsl="urn:schemas-microsoft-com:xslt">
  <xsl:template name="output-attribute-value">
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <xsl:choose>
      <xsl:when test="$type='Binary'">
        <xsl:value-of select="$attribute" disable-output-escaping="yes"/>
      </xsl:when>
      <xsl:when test="$type='Text'">
        <xsl:text xml:space="preserve" disable-output-escaping="yes">(text data)</xsl:text>
      </xsl:when>
      <xsl:otherwise>
        <xsl:value-of select="translate($attribute,' ','&#160;')" disable-output-escaping="yes"/>
      </xsl:otherwise>
    </xsl:choose>
  </xsl:template>

  <xsl:template name="output-modified-value">
    <xsl:param name="name"/>
    <xsl:param name="attribute1"/>
    <xsl:param name="text1"/>
    <xsl:param name="attribute2"/>
    <xsl:param name="text2"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$name" disable-output-escaping="yes"/>
        </td>
        <xsl:choose>
          <xsl:when test="$attribute1 and $attribute1!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute1"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text1"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
        <xsl:choose>
          <xsl:when test="$attribute2 and $attribute2!=''">
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:call-template name="output-attribute-value">
                <xsl:with-param name="attribute" select="$attribute2"/>
                <xsl:with-param name="type" select="$type"/>
              </xsl:call-template>
            </td>
          </xsl:when>
          <xsl:otherwise>
            <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
              <xsl:copy-of select="$text2"/>
            </td>
          </xsl:otherwise>
        </xsl:choose>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template name="output-localized-attribute-value">
    <xsl:param name="locale"/>
    <xsl:param name="attribute"/>
    <xsl:param name="type"/>
    <tr class="listViewRow" style="height:22px;">
      <xsl:if test="position() mod 2 != 0">
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
      <xsl:if test="position() mod 2 != 1">
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:value-of select="$locale" disable-output-escaping="yes"/>
        </td>
        <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
          <xsl:call-template name="output-attribute-value">
            <xsl:with-param name="attribute" select="$attribute"/>
            <xsl:with-param name="type" select="$type"/>
          </xsl:call-template>
        </td>
      </xsl:if>
    </tr>
  </xsl:template>

  <xsl:template match="/">
    <xsl:choose>
      <xsl:when test="ModifiedAttributes[@ActionType='Create']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Create">
          <Attribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the value]"/>
          other <Attribute> elements
        </ModifiedAttributes>
        -->
        <table cellspacing="0" cellpadding="3" class="commonSummaryListViewGridBorder">
          <tr align="left" class="listViewHeader" style="height:22px;">
            <th class="commonSummaryListViewHeaderCellBR">Attribute</th>
            <th class="commonSummaryListViewHeaderCellBR">Value</th>
          </tr>
          <xsl:for-each select="ModifiedAttributes/Attribute">
            <xsl:sort select="@DisplayName" order="ascending"/>
            <tr class="listViewRow" style="height:22px;">
              <xsl:if test="position() mod 2 != 0">
                <td class="commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
              <xsl:if test="position() mod 2 != 1">
                <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                  <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                </td>
                <xsl:if test="count(LocalizedValue)!=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <table cellspacing="0" style="width:100%">
                      <tr class="listViewHeader">
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Language</th>
                        <th align="left" class="commonSummaryListViewHeaderCellBR">Status</th>
                      </tr>
                      <xsl:if test="@InitializedValue and @InitializedValue != ''">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="@DataType"/>
                        </xsl:call-template>
                      </xsl:if>
                      <xsl:for-each select="LocalizedValue">
                        <xsl:call-template name="output-localized-attribute-value">
                          <xsl:with-param name="locale" select="@Locale"/>
                          <xsl:with-param name="attribute" select="@InitializedValue"/>
                          <xsl:with-param name="type" select="../@DataType"/>
                        </xsl:call-template>
                      </xsl:for-each>
                    </table>
                  </td>
                </xsl:if>
                <xsl:if test="count(LocalizedValue)=0">
                  <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                    <xsl:call-template name="output-attribute-value">
                      <xsl:with-param name="attribute" select="@InitializedValue"/>
                      <xsl:with-param name="type" select="@DataType"/>
                    </xsl:call-template>
                  </td>
                </xsl:if>
              </xsl:if>
            </tr>
          </xsl:for-each>
        </table>
      </xsl:when>
      <xsl:when test="ModifiedAttributes[@ActionType='Modify']">
        <!-- expected XML
        <ModifiedAttributes ActionType="Modify">
          <SingleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InitializedValue="[the old value]" SetValue="[the new value]"/>
          other <SingleAttribute> elements
          <MultipleAttribute Name="[attribute's system name]" DisplayName="[attribute's display name]" DataType="[all kinds of ILM data type]" InsertedItem="[inserted items separated by ';']" RemovedItem="[removed items separated by ';']"/>
          other <MultipleAttribute> elements
        </ModifiedAttributes>
        -->
        <table class="commonSummaryListViewGridBorder" cellspacing="0" cellpadding="3">
          <xsl:if test="ModifiedAttributes[count(SingleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Single-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
              <th class="commonSummaryListViewHeaderCellBR">New Value</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/SingleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="listViewRow">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" cellpadding="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td colSpan="2">
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Old Value</th>
                          <th class="commonSummaryListViewHeaderCellBR">New Value</th>
                        </tr>
                        <xsl:if test="(@InitializedValue and @InitializedValue !='') or (@SetValue and @SetValue != '')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@InitializedValue"/>
                            <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@SetValue"/>
                            <xsl:with-param name="text2">(value removed)</xsl:with-param>
                            <xsl:with-param name="type" select="../@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@InitializedValue"/>
                  <xsl:with-param name="text1">(no initial value)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@SetValue"/>
                  <xsl:with-param name="text2">(value removed)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
          <xsl:if test="ModifiedAttributes[count(MultipleAttribute)!=0]">
            <tr align="left" class="listViewHeader">
              <th class="commonSummaryListViewHeaderCellBR">Multiple-Value Attributes</th>
              <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
              <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
            </tr>
            <xsl:for-each select="ModifiedAttributes/MultipleAttribute">
              <xsl:sort select="@DisplayName" order="ascending"/>
              <xsl:if test="count(LocalizedValue)!=0">
                <tr class="uocSummaryTitleTR">
                  <xsl:if test="position() mod 2 != 0">
                    <td class="commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                  <xsl:if test="position() mod 2 != 1">
                    <td class="ms-alternating commonSummaryListViewCellBR ms-vb">
                      <xsl:value-of select="@DisplayName" disable-output-escaping="yes"/>
                    </td>
                    <td>
                      <table cellspacing="0" style="width:100%">
                        <tr align="left" class="listViewHeader">
                          <th class="commonSummaryListViewHeaderCellBR">Language</th>
                          <th class="commonSummaryListViewHeaderCellBR">Removed Items</th>
                          <th class="commonSummaryListViewHeaderCellBR">Inserted Items</th>
                        </tr>
                        <xsl:if test="(@RemovedItem and @RemovedItem!='') or (@InsertedItem and @InsertedItem!='')">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:if>
                        <xsl:for-each select="LocalizedValue">
                          <xsl:call-template name="output-modified-value">
                            <xsl:with-param name="name" select="@Locale"/>
                            <xsl:with-param name="attribute1" select="@RemovedItem"/>
                            <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                            <xsl:with-param name="attribute2" select="@InsertedItem"/>
                            <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                            <xsl:with-param name="type" select="@DataType"/>
                          </xsl:call-template>
                        </xsl:for-each>
                      </table>
                    </td>
                  </xsl:if>
                </tr>
              </xsl:if>
              <xsl:if test="count(LocalizedValue)=0">
                <xsl:call-template name="output-modified-value">
                  <xsl:with-param name="name" select="@DisplayName"/>
                  <xsl:with-param name="attribute1" select="@RemovedItem"/>
                  <xsl:with-param name="text1">(no removed item)</xsl:with-param>
                  <xsl:with-param name="attribute2" select="@InsertedItem"/>
                  <xsl:with-param name="text2">(no inserted item)</xsl:with-param>
                  <xsl:with-param name="type" select="@DataType"/>
                </xsl:call-template>
              </xsl:if>
            </xsl:for-each>
          </xsl:if>
        </table>
      </xsl:when>
    </xsl:choose>
  </xsl:template>
</xsl:stylesheet>
```
