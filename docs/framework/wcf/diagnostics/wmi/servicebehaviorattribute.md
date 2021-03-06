---
title: ServiceBehaviorAttribute
ms.date: 03/30/2017
ms.assetid: 5faa266f-587f-4e03-828d-1c7dd5acfe65
ms.openlocfilehash: b6221e93f10b87a368bd594932a8c36ae14df8f3
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59772809"
---
# <a name="servicebehaviorattribute"></a>ServiceBehaviorAttribute
ServiceBehaviorAttribute  
  
## <a name="syntax"></a>语法  
  
```csharp
class ServiceBehaviorAttribute : Behavior  
{  
  boolean AutomaticSessionShutdown;  
  string ConcurrencyMode;  
  string ConfigurationName;  
  boolean IgnoreExtensionDataObject;  
  boolean IncludeExceptionDetailInFaults;  
  string InstanceContextMode;  
  sint32 MaxItemsInObjectGraph;  
  string Name;  
  string Namespace;  
  boolean ReleaseServiceInstanceOnTransactionComplete;  
  boolean TransactionAutoCompleteOnSessionClose;  
  string TransactionIsolationLevel;  
  datetime TransactionTimeout;  
  boolean UseSynchronizationContext;  
  boolean ValidateMustUnderstand;  
};  
```  
  
## <a name="methods"></a>方法  
 ServiceBehaviorAttribute 类未定义任何方法。  
  
## <a name="properties"></a>属性  
 ServiceBehaviorAttribute 类具有以下属性：  
  
### <a name="automaticsessionshutdown"></a>AutomaticSessionShutdown  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指示在客户端关闭输出会话时是否自动关闭会话。  
  
### <a name="concurrencymode"></a>ConcurrencyMode  
 数据类型：String  
访问类型：只读  
  
 指示服务是支持单线程、多线程还是支持可重入调用。  
  
### <a name="configurationname"></a>ConfigurationName  
 数据类型：String  
  
 访问类型：只读  
  
 服务配置的名称。  
  
### <a name="ignoreextensiondataobject"></a>IgnoreExtensionDataObject  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否要将未知序列化数据发送到网络上。  
  
### <a name="includeexceptiondetailinfaults"></a>IncludeExceptionDetailInFaults  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否在返回给客户端的 SOAP 错误详细信息中包含托管异常信息以供调试。  
  
### <a name="instancecontextmode"></a>InstanceContextMode  
 数据类型：String  
  
 访问类型：只读  
  
 指定何时创建新服务对象。  
  
### <a name="maxitemsinobjectgraph"></a>MaxItemsInObjectGraph  
 数据类型：sint32  
  
 访问类型：只读  
  
 序列化对象中允许的最大项数。  
  
### <a name="name"></a>名称  
 数据类型：String  
  
 访问类型：只读  
  
 WSDL 中服务的名称属性。  
  
### <a name="namespace"></a>命名空间  
 数据类型：String  
  
 访问类型：只读  
  
 WSDL 中服务的目标命名空间。  
  
### <a name="releaseserviceinstanceontransactioncomplete"></a>ReleaseServiceInstanceOnTransactionComplete  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定当前事务完成后，是否回收服务对象。  
  
### <a name="transactionautocompleteonsessionclose"></a>TransactionAutoCompleteOnSessionClose  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定当前会话关闭时，挂起的事务是否已完成。  
  
### <a name="transactionisolationlevel"></a>TransactionIsolationLevel  
 数据类型：String  
  
 访问类型：只读  
  
 指定事务的隔离级别。  
  
### <a name="transactiontimeout"></a>TransactionTimeout  
 数据类型：DateTime  
  
 访问类型：只读  
  
 事务必须在此期间完成的时间段。  
  
### <a name="usesynchronizationcontext"></a>UseSynchronizationContext  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定是否使用当前同步上下文来选择线程执行。  
  
### <a name="validatemustunderstand"></a>ValidateMustUnderstand  
 数据类型：Boolean  
  
 访问类型：只读  
  
 指定系统或应用程序是否强制执行 SOAP MustUnderstand 标头处理。  
  
## <a name="requirements"></a>要求  
  
|MOF|已在 Servicemodel.mof 中声明。|  
|---------|-----------------------------------|  
|命名空间|已在 root\ServiceModel 中定义|  
  
## <a name="see-also"></a>请参阅

- <xref:System.ServiceModel.ServiceBehaviorAttribute>
