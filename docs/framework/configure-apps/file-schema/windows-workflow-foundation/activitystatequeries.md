---
title: <activityStateQueries>
ms.date: 03/30/2017
ms.topic: reference
ms.assetid: bdd3c8ae-a13f-4df1-9b3c-a9d6c4bb1b5f
ms.openlocfilehash: 90acda7277fd276f43a619a014fbce103261aa1e
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59112224"
---
# <a name="activitystatequeries"></a>\<activityStateQueries>
表示一个查询集合，这些查询用于跟踪构成工作流实例的活动的生命周期更改。 例如，你可能想要跟踪的每次在"发送电子邮件"活动完成工作流实例内。 跟踪参与者需要用此查询来订阅活动状态记录对象。 在 ActivityStates 中指定了要订阅的可用状态。  
  
 有关跟踪配置文件查询的详细信息，请参阅[跟踪配置文件](../../../../../docs/framework/windows-workflow-foundation/tracking-profiles.md)。  
  
\<system.serviceModel>  
\<tracking>  
\<trackingProfile>  
\<workflow>  
\<activityStateQueries>  
  
## <a name="syntax"></a>语法  
  
```xml
<tracking>
  <trackingProfile name="Name">
    <workflow>
      <activityStateQueries>
        <activityStateQuery activityName="String" />
        <arguments>
          <argument name="String" />
        </arguments>
        <states>
          <state name="String" />
        </states>
        <variables>
         <variable name="String" />
        </variables>
      </activityStateQueries>
    </workflow>
  </trackingProfile>
</tracking>  
```  
  
## <a name="attributes-and-elements"></a>特性和元素  
 下列各节描述了特性、子元素和父元素。  
  
### <a name="attributes"></a>特性  
 无。  
  
### <a name="child-elements"></a>子元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<activityStateQuery>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/activitystatequery.md)|一个查询，用于跟踪在活动中发生的错误的处理。  每次 FaultHandler 处理错误时，都会发生此事件。|  
  
### <a name="parent-elements"></a>父元素  
  
|元素|描述|  
|-------------|-----------------|  
|[\<workflow>](../../../../../docs/framework/configure-apps/file-schema/windows-workflow-foundation/workflow.md)|一个配置元素，包含由标识为特定工作流的所有查询**activityDefinitionId**属性。|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.Activities.Tracking.Configuration.ActivityStateQueryElementCollection?displayProperty=nameWithType>
- <xref:System.Activities.Tracking.ActivityStateQuery?displayProperty=nameWithType>
- [工作流跟踪](../../../../../docs/framework/windows-workflow-foundation/workflow-tracking-and-tracing.md)
- [跟踪配置文件](../../../../../docs/framework/windows-workflow-foundation/tracking-profiles.md)
