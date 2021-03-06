---
title: 持久性参与者
ms.date: 03/30/2017
ms.assetid: f84d2d5d-1c1b-4f19-be45-65b552d3e9e3
ms.openlocfilehash: 18614962708eafa192d8163638fce2b8154d6106
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59316357"
---
# <a name="persistence-participants"></a>持久性参与者
持久性参与者可以参与应用程序宿主触发的持久性操作（保存或加载）。 [!INCLUDE[netfx_current_long](../../../includes/netfx-current-long-md.md)]附带了两个抽象类**PersistenceParticipant**并**PersistenceIOParticipant**，可以用来创建持久性参与者。 持久性参与者派生自这些类之一，它实现所需的方法，然后将该类的实例添加到 <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> 上的 <xref:System.ServiceModel.Activities.WorkflowServiceHost> 集合。 应用程序宿主可在持久保存工作流实例时查找此类工作流扩展，并在适当时对持久性参与者调用适当的方法。  
  
 下面的列表介绍持久性子系统在持久保存（保存）操作的不同阶段执行的任务。 持久性参与者用在第三和第四个阶段。 如果参与者是 I/O 参与者 （还出现在 I/O 操作中的持久性参与者），该参与者还可在第六个阶段。  
  
1. 收集内置值，包括工作流状态、书签、映射变量和时间戳。  
  
2. 收集已添加到与工作流实例关联的扩展集合中的所有持久性参与者。  
  
3. 调用由所有持久性参与者实现的 <xref:System.Activities.Persistence.PersistenceParticipant.CollectValues%2A> 方法。  
  
4. 调用由所有持久性参与者实现的 <xref:System.Activities.Persistence.PersistenceParticipant.MapValues%2A> 方法。  
  
5. 将工作流持久保存或保存到持久性存储区。  
  
6. 调用<xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnSave%2A>对所有持久性 I/O 参与者的方法。 如果参与者不是 I/O 参与者，则跳过此任务。 如果持久性段是事务性的，则在 Transaction.Current 属性中提供事务。  
  
7. 等待所有持久性参与者完成。 如果所有参与者在持久保存实例数据时都成功，则提交事务。  
  
 持久性参与者派生**PersistenceParticipant**类，可以实现**CollectValues**并**MapValues**方法。 I/O 持久性参与者派生**PersistenceIOParticipant**类，可以实现**BeginOnSave**除了实现方法**CollectValues**并**MapValues**方法。  
  
 一个阶段完成之后才开始下一个阶段。 例如，从收集值**所有**第一阶段中的持久性参与者。 然后，将在第一个阶段中收集的所有值提供给第二个阶段中的所有持久性参与者进行映射。 接下来，将在第一个阶段中收集的和在第二个阶段中映射的所有值提供给第三个阶段中的持久性提供程序，依此类推。  
  
 下面的列表介绍持久性子系统在加载操作的不同阶段执行的任务。 持久性参与者用在第四个阶段中。 持久性 I/O 参与者 （还参与 I/O 操作的持久性参与者） 还用在第三个阶段。  
  
1. 收集已添加到与工作流实例关联的扩展集合中的所有持久性参与者。  
  
2. 从持久性存储区加载工作流。  
  
3. 调用<xref:System.Activities.Persistence.PersistenceIOParticipant.BeginOnLoad%2A>上所有持久性 I/O 参与者并等待所有持久性参与者完成。 如果持久性段是事务性的，则在 Transaction.Current 中提供事务。  
  
4. 基于从持久性存储区检索的数据将工作流实例加载到内存中。  
  
5. 对每个持久性参与者调用 <xref:System.Activities.Persistence.PersistenceParticipant.PublishValues%2A>。  
  
 持久性参与者派生**PersistenceParticipant**类，可以实现**PublishValues**方法。 I/O 持久性参与者派生**PersistenceIOParticipant**类，可以实现**BeginOnLoad**除了实现方法**PublishValues**方法。  
  
 在加载工作流实例时，持久性提供程序将在该实例上创建锁。 这避免该实例在多节点方案中被多个主机加载。 如果你尝试加载已被锁定的工作流实例会看到如下所示异常：异常"System.ServiceModel.Persistence.InstanceLockException:请求的操作无法完成，因为锁例如 00000000-0000-0000-0000-000000000000 无法获取"。 在发生下列情况之一时将会导致该错误：  
  
-   在多节点方案中，另一个主机加载了该实例。  有几种不同的方法可以解决这些类型的冲突：转移对拥有锁和重试的节点的处理，或者强制将导致其他主机无法保存其工作的负载。  
  
-   在单节点方案中并且主机出现崩溃。  在主机再次启动时（进程回收或者创建新的持久性提供程序工厂），新主机将尝试加载由于锁尚未到期而仍被旧主机锁定的实例。  
  
-   在单节点方案中并且有关实例在某个点上已中止并且创建具有不同主机 ID 的新的持久性提供程序实例。  
  
 锁定超时时间值默认为 5 分钟，您可以在调用 <xref:System.ServiceModel.Persistence.PersistenceProvider.Load%2A> 时指定其他超时时间值。  
  
## <a name="in-this-section"></a>本节内容  
  
-   [如何：创建自定义持久性参与者](how-to-create-a-custom-persistence-participant.md)  
  
## <a name="see-also"></a>请参阅

- [存储扩展性](store-extensibility.md)
