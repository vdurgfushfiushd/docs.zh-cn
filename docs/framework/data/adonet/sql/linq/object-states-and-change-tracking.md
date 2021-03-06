---
title: 对象状态和更改跟踪
ms.date: 03/30/2017
ms.assetid: 7a808b00-9c3c-479a-aa94-717280fefd71
ms.openlocfilehash: 63b04d3a4b6e48594e9664833a6e539d62bbab0e
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59191150"
---
# <a name="object-states-and-change-tracking"></a>对象状态和更改跟踪
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 对象始终处于某种*状态*。 例如，当 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 创建一个新对象时，该对象就处于 `Unchanged` 状态。 您自己创建的新对象对于 <xref:System.Data.Linq.DataContext> 而言是未知的，因而处于 `Untracked` 状态。 在成功执行 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 后，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 已知的所有对象均处于 `Unchanged` 状态。 （唯一的例外是已从数据库中成功删除的那些对象，它们处于 `Deleted` 状态，并且在该 <xref:System.Data.Linq.DataContext> 实例中无法使用。）  
  
## <a name="object-states"></a>对象状态  
 下表列出了 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 对象可能的状态。  
  
|状态|描述|  
|-----------|-----------------|  
|`Untracked`|[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 未跟踪的对象。 示例包括：<br /><br /> 不通过当前查询的对象<xref:System.Data.Linq.DataContext>（例如新创建的对象）。<br />的创建通过反序列化对象<br />-通过其他查询一个对象<xref:System.Data.Linq.DataContext>。|  
|`Unchanged`|通过使用当前 <xref:System.Data.Linq.DataContext> 检索到的对象，并且尚未获知此对象自创建以来已被修改。|  
|`PossiblyModified`|是的对象*附加*到<xref:System.Data.Linq.DataContext>。 有关详细信息，请参阅[数据中检索和 CUD 操作 N 层应用程序 (LINQ to SQL)](../../../../../../docs/framework/data/adonet/sql/linq/data-retrieval-and-cud-operations-in-n-tier-applications.md)。|  
|`ToBeInserted`|使用当前 <xref:System.Data.Linq.DataContext> 未检索到的对象。 这会导致在 `INSERT` 期间执行数据库 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 操作。|  
|`ToBeUpdated`|符合如下条件的对象：已获知自检索到该对象以来它已被修改。 这会导致在 `UPDATE` 期间执行数据库 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 操作。|  
|`ToBeDeleted`|标记为删除，从而导致在 `DELETE` 期间执行数据库 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 操作的对象。|  
|`Deleted`|已从数据库中删除的对象。 此状态为最终状态，不允许再进行其他转换。|  
  
## <a name="inserting-objects"></a>插入对象  
 您可以通过使用 `Inserts` 显式请求 <xref:System.Data.Linq.Table%601.InsertOnSubmit%2A>。 此外，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 还可以通过查找与必须更新的已知对象之一相连接的对象来推断 `Inserts`。 例如，如果将 `Untracked` 对象添加到 <xref:System.Data.Linq.EntitySet%601> 或将 <xref:System.Data.Linq.EntityRef%601> 设置为 `Untracked` 对象，则可以通过关系图中的被跟踪对象使 `Untracked` 对象可访问。 在处理 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 遍历被跟踪的对象，并查找未被跟踪的任何可访问的持久性对象。 此类对象是要插入到数据库中的候选对象。  
  
 继承层次结构中的类<xref:System.Data.Linq.Table%601.InsertOnSubmit%2A>(`o`) 也设置为指定的成员的值*鉴别器*要匹配的对象类型`o`。 对于与默认鉴别器值匹配的类型，此操作会导致鉴别器值被默认值覆盖。 有关详细信息，请参阅[层次结构支持](../../../../../../docs/framework/data/adonet/sql/linq/inheritance-support.md)。  
  
> [!IMPORTANT]
>  添加到 `Table` 的对象不在标识缓存中。 标识缓存仅反映从数据库中检索到的内容。 调用 <xref:System.Data.Linq.Table%601.InsertOnSubmit%2A> 后，直到 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 成功完成，所添加的实体才会出现在对数据库的查询中。  
  
## <a name="deleting-objects"></a>删除对象  
 通过对相应的 `o` 调用 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A>(o)，可以将被跟踪的对象 <xref:System.Data.Linq.Table%601> 标记为删除。 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 将从 <xref:System.Data.Linq.EntitySet%601> 中移除对象视为更新操作，并且将对应的外键值设置为 null。 不会将操作的目标 (`o`) 从其所在表中删除。 例如，当通过将外键 `cust.Orders.DeleteOnSubmit(ord)` 设置为 null 切断 `cust` 与 `ord` 之间的关系时，`ord.CustomerID` 指示更新操作。 它不会导致删除与 `ord` 对应的行。  
  
 将对象从其所在表中删除 ([!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]) 时，<xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A> 执行以下处理：  
  
-   调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时，会对该对象执行 `DELETE` 操作。  
  
-   不论相关对象是否已加载，都不会将此移除操作传播到相关对象。 具体而言，不会为更新关系属性而加载相关对象。  
  
-   在成功执行 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 后，会将这些对象设置为 `Deleted` 状态。 因此，您不能在该 `id` 中使用此类对象或其 <xref:System.Data.Linq.DataContext>。 由 <xref:System.Data.Linq.DataContext> 实例维护的内部缓存不会消除检索到的对象或作为新对象添加的对象，即使这些对象已从数据库中删除也不例外。  
  
 您只能对由 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A> 跟踪的对象调用 <xref:System.Data.Linq.DataContext>。 对于 `Untracked` 对象，您必须先调用 <xref:System.Data.Linq.Table%601.Attach%2A>，再调用 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A>。 对 <xref:System.Data.Linq.Table%601.DeleteOnSubmit%2A> 对象调用 `Untracked` 会引发异常。  
  
> [!NOTE]
>  从表中移除对象使得 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 在 `DELETE` 时生成对应的 SQL <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 命令。 此操作不会从缓存中移除该对象或将删除操作传播到相关对象。  
>   
>  若要回收已删除对象的 `id`，请使用新的 <xref:System.Data.Linq.DataContext> 实例。 对于清理相关对象时，你可以使用*级联删除*功能的数据库，否则需手动删除相关的对象。  
>   
>  无需按任何特殊顺序删除相关对象（与在数据库中进行删除不同）。  
  
## <a name="updating-objects"></a>更新对象  
 您可以通过观察有关更改的通知来检测 `Updates`。 通知是通过属性 setter 中的 <xref:System.ComponentModel.INotifyPropertyChanging.PropertyChanging> 事件提供的。 当 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 收到有关对象的第一项更改的通知时，它会创建该对象的一个副本，并将该对象视为用于生成 `Update` 语句的候选对象。  
  
 对于未实现 <xref:System.ComponentModel.INotifyPropertyChanging> 的对象，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 保存这些对象在第一次具体化时所具有的值的副本。 调用 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 时，[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 会比较当前值和原始值，以确定对象是否已更改。  
  
 对于关系的更新，从子级到父级的引用（即与外键对应的引用）被视为授权。 反方向上（即从父级到子级）的引用是可选的。 关系类（<xref:System.Data.Linq.EntitySet%601> 和 <xref:System.Data.Linq.EntityRef%601>）保证双向引用对于一对多和一对一关系是一致的。 如果对象模型不使用 <xref:System.Data.Linq.EntitySet%601> 或 <xref:System.Data.Linq.EntityRef%601>，且存在反向引用，则需要您来确保在更新关系时反向引用与前向引用一致。  
  
 如果您既更新所需的引用，又更新对应的外键，则必须确保它们一致。 如果在您调用 <xref:System.InvalidOperationException> 时这两者不同步，则会引发 <xref:System.Data.Linq.DataContext.SubmitChanges%2A> 异常。 尽管外键值的更改足以影响基础行的更新，但您应更改相应的引用以维护对象图的连接性和关系的双向一致性。  
  
## <a name="see-also"></a>请参阅

- [背景信息](../../../../../../docs/framework/data/adonet/sql/linq/background-information.md)
- [插入、更新和删除操作](../../../../../../docs/framework/data/adonet/sql/linq/insert-update-and-delete-operations.md)
