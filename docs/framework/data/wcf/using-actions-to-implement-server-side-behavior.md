---
title: 使用操作来实现服务器端行为
ms.date: 03/30/2017
ms.assetid: 11a372db-7168-498b-80d2-9419ff557ba5
ms.openlocfilehash: 4467d52df5dbb134947111e96782e257eef253e7
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59294933"
---
# <a name="using-actions-to-implement-server-side-behavior"></a>使用操作来实现服务器端行为

OData 操作提供了用于实现这样一种行为方法，即该行为将作用于从 OData 服务检索的资源。 例如，请考虑将一部数字电影作为资源，你可能需要完成许多事情：签出、评级/注释或签入。 这些是用于管理数字电影的 WCF 数据服务可能实现的所有动作示例。 动作在 OData 响应中描述，而此响应包含对其调用此动作的资源。 当用户请求表示数字电影的资源时，从 WCF 数据服务返回的响应将包含有关可用于该资源的动作的信息。 动作的可用性可能取决于数据服务或资源的状态。 例如，一旦数字电影已被签出，其他用户就无法签出。 客户只需指定 URL 即可调用动作。 例如，`http://MyServer/MovieService.svc/Movies(6)`将标识特定的数字电影和`http://MyServer/MovieService.svc/Movies(6)/Checkout`像调用对这部特定电影的操作。 动作使您能够公开服务模型，但不必公开数据模型。 继续探讨此电影服务示例，您可能希望允许用户对电影评级，但不能直接将评级数据公开为资源。 您可以实现评级动作，以使用户能够对电影评级，但不能直接将评级数据作为资源进行访问。
  
## <a name="implementing-an-action"></a>实现动作  
 若要实现服务动作，您必须实现<xref:System.IServiceProvider>， [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))，和[IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103))接口。 <xref:System.IServiceProvider> 使 WCF 数据服务获取的实现[IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))。 [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))使 WCF 数据服务能够创建、 查找、 描述和调用服务操作。 [IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) ，可调用实现服务动作行为的代码和获取结果，如果有的话。 请记住，WCF 数据服务是“每次调用”的 WCF 服务，也即，每次调用此服务时，都会创建此服务的一个新实例。  确保创建此服务时不执行多余的任务。  
  
### <a name="iserviceprovider"></a>IServiceProvider  
 <xref:System.IServiceProvider> 包含一个名为 <xref:System.IServiceProvider.GetService%2A> 的方法。 WCF 数据服务调用此方法来检索一些服务提供程序，包括元数据服务提供程序和数据服务动作提供程序。 当要求提供数据服务操作提供程序，请返回您[IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))实现。  
  
### <a name="idataserviceactionprovider"></a>IDataServiceActionProvider  
 [IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))包含可用于检索有关可用操作的信息的方法。 当您实现[IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))在你的服务的服务的实现定义的扩展元数据[IDataServiceActionProvider](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859915(v=vs.103))的操作和处理调度的根据这些操作。  
  
#### <a name="advertiseserviceaction"></a>AdvertiseServiceAction  
 [AdvertiseServiceAction 方法](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859971(v=vs.103))调用以确定哪些操作可用于指定的资源。 仅针对并非始终可用的动作才调用此方法。 该方法用于根据所请求的资源状态或服务状态，检查动作是否应包含在 OData 响应中。 如何实现这一检查完全由您决定。 如果计算可用性的代价很高，并且当前资源处于源中，则可以跳过此检查并公布动作。 如果要返回的当前资源是源的一部分，则 `inFeed` 参数设置为 `true`。  
  
#### <a name="createinvokable"></a>CreateInvokable  
 [CreateInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859940(v=vs.103))调用来创建[IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103)) ，其中包含用于封装实现操作的行为的代码的委托。 这将创建[IDataServiceInvokable](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859893(v=vs.103))实例，但不会调用该操作。 WCF 数据服务动作具有副作用，需要与更新提供程序结合使用才能将这些更改保存到磁盘。 [IDataServiceInvokable.Invoke](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh859924(v=vs.103))从更新提供程序的 savechanges （） 调用方法中调用方法。  
  
#### <a name="getserviceactions"></a>GetServiceActions  
 此方法返回的集合[ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))表示 WCF 数据服务公开的操作的所有实例。 [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))的操作，包括信息，如操作名称、 其参数和它的返回类型的元数据表示形式。  
  
#### <a name="getserviceactionsbybindingparametertype"></a>GetServiceActionsByBindingParameterType  
 此方法返回的所有集合[ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))可以绑定到指定的绑定参数类型的实例。 换而言之，所有[ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))s 可操作的指定的资源类型 （也称为绑定参数类型）。该服务返回资源以便包含可以针对该资源调用的操作的信息时使用。 此方法应只返回可绑定到确切绑定参数类型（无派生类型）的动作。 此方法针对遇到的每个请求和每种类型调用一次，结果由 WCF 数据服务进行缓存。  
  
#### <a name="tryresolveserviceaction"></a>TryResolveServiceAction  
 此方法搜索指定[ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103)) ，并返回`true`如果[ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))找到。 如果找到， [ServiceAction](https://docs.microsoft.com/previous-versions/dotnet/wcf-data-services/hh544089(v=vs.103))中返回`serviceAction``out`参数。  
  
### <a name="idataserviceinvokable"></a>IDataServiceInvokable  
 此接口提供执行 WCF 数据服务动作的方法。 当实现 IDataServiceInvokable 时，您应负责三件事：  
  
1. 捕获并可能封送处理参数  
  
2. 将参数调度到在调用 Invoke() 时实际实现动作的代码  
  
3. 存储来自 Invoke() 的任何结果，以便可以使用 GetResult() 检索这些结果  
  
 参数可作为标记进行传递。 这是因为可以编写与表示资源的标记结合使用的数据服务提供程序，如果是这种情况，则您可能需要将这些标记转换（封送处理）为实际资源，然后才能调度到实际动作。 在封送处理此参数之后，此参数必须处于可编辑状态，以便将在调用动作时对资源所做的任何更改保存并写入到磁盘中。  
  
 此接口需要两个方法：调用和 GetResult。 Invoke 调用用于实现动作行为的委托，而 GetResult 返回此动作的结果。  
  
## <a name="invoking-a-wcf-data-service-action"></a>调用 WCF 数据服务动作  
 可以使用 HTTP POST 请求来调用动作。 URL 指定资源（后跟动作名称）。 参数在请求的正文中传递。 例如，假设有一个名为 MovieService 的服务，该服务公开了一个名为 Rate 的动作。 您可以使用下面的 URL 针对特定电影调用 Rate 动作：  
  
 `http://MovieServer/MovieService.svc/Movies(1)/Rate`
  
 Movies(1) 指定您要评级的电影，而 Rate 指定 Rate（评级）动作。 评级的实际值将位于 HTTP 请求的正文中，如以下示例所示：  
  
```  
POST http://MovieServer/MoviesService.svc/Movies(1)/Rate HTTP/1.1   
Content-Type: application/json   
Content-Length: 20   
Host: localhost:15238  
{   
   "rating": 4   
}  
```  
  
> [!WARNING]
> 上面的示例代码只能与支持轻型 JSON 的 WCF 数据服务 5.2 和更高版本结合使用。 如果使用 WCF 数据服务的更低版本，您必须指定 json 详细内容类型，如下所示：`application/json;odata=verbose`。  
  
 此外，你也可以使用 WCF 数据服务客户端调用动作，如下面的代码段所示。  
  
```csharp
MoviesModel context = new MoviesModel (new Uri("http://MyServer/MoviesService.svc/"));  
//...  
context.Execute(new Uri("http://MyServer/MoviesService.svc/Movies(1)/Rate"), "POST", new BodyOperationParameter("rating",4) );
```
  
 在上面的代码段中，通过使用 Visual Studio 将服务引用添加到 WCF 数据服务，从而生成了 `MoviesModel` 类。  
  
## <a name="see-also"></a>请参阅

- [WCF Data Services 4.5](../../../../docs/framework/data/wcf/index.md)
- [定义 WCF Data Services](../../../../docs/framework/data/wcf/defining-wcf-data-services.md)
- [开发和部署 WCF 数据服务](../../../../docs/framework/data/wcf/developing-and-deploying-wcf-data-services.md)
- [自定义数据服务提供程序](../../../../docs/framework/data/wcf/custom-data-service-providers-wcf-data-services.md)
