---
title: 如何：使用针对多个结果形状映射的存储过程
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: c2b84dfe-7fec-489a-92de-45215cec4518
ms.openlocfilehash: 406e44a0ee3b086ceb47b25a80c4fd0ff5a92607
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59164666"
---
# <a name="how-to-use-stored-procedures-mapped-for-multiple-result-shapes"></a>如何：使用针对多个结果形状映射的存储过程
当存储过程可以返回多个结果形状时，返回类型无法强类型化为单个投影形状。 尽管 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] 可以生成所有可能的投影类型，但它无法获知将以何种顺序返回它们。  
  
 请将这种情况与按顺序产生多个结果形状的存储过程作一个对比。 有关详细信息，请参阅[如何：使用针对顺序结果形状映射的存储的过程](../../../../../../docs/framework/data/adonet/sql/linq/how-to-use-stored-procedures-mapped-for-sequential-result-shapes.md)。  
  
 <xref:System.Data.Linq.Mapping.ResultTypeAttribute> 属性适用于返回多个结果类型的存储过程，用以指定该过程可以返回的类型的集合。  
  
## <a name="example"></a>示例  
 在下面的 SQL 代码示例中，结果形状取决于输入（`shape =1` 或 `shape = 2`）。 您不知道将先返回哪个投影。  
  
```  
CREATE PROCEDURE VariableResultShapes(@shape int)  
AS  
if(@shape = 1)  
    select CustomerID, ContactTitle, CompanyName from customers  
else if(@shape = 2)  
    select OrderID, ShipName from orders  
```  
  
 [!code-csharp[DLinqSprox#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/northwind-sprox.cs#4)]
 [!code-vb[DLinqSprox#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/northwind-sprox.vb#4)]  
  
## <a name="example"></a>示例  
 你将使用类似于以下内容的代码来执行此存储过程。  
  
> [!NOTE]
>  您必须根据您对存储过程的认识，使用 <xref:System.Data.Linq.IMultipleResults.GetResult%2A> 模式获取正确类型的枚举数。  
  
 [!code-csharp[DLinqSprox#5](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqSprox/cs/Program.cs#5)]
 [!code-vb[DLinqSprox#5](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqSprox/vb/Module1.vb#5)]  
  
## <a name="see-also"></a>请参阅

- [存储过程](../../../../../../docs/framework/data/adonet/sql/linq/stored-procedures.md)
