---
title: SQL Server 二进制和大值数据
ms.date: 03/30/2017
ms.assetid: e00827b3-7511-4b2d-91d7-851ca86cc6b5
ms.openlocfilehash: 4b7a3f16726d6363cd702fb912bb7be281a25000
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59192961"
---
# <a name="sql-server-binary-and-large-value-data"></a>SQL Server 二进制和大值数据
SQL Server 提供 `max` 说明符，它可扩展 `varchar`、`nvarchar` 和 `varbinary` 数据类型的存储容量。 `varchar(max)``nvarchar(max)`，并`varbinary(max)`统称为*大型值数据类型*。 您可以使用大值数据类型来存储最大为 2^31-1 个字节的数据。  
  
 SQL Server 2008 引入了 FILESTREAM 属性，该属性不是一种数据类型，而是一种可在列上定义的属性，因而允许将大值数据存储在文件系统上而不是存储在数据库中。  
  
## <a name="in-this-section"></a>本节内容  
 [在 ADO.NET 中修改大值 (max) 数据](../../../../../docs/framework/data/adonet/sql/modifying-large-value-max-data.md)  
 说明如何使用大值数据类型。  
  
 [FILESTREAM 数据](../../../../../docs/framework/data/adonet/sql/filestream-data.md)  
 描述如何使用在 SQL Server 2008 中通过 FILESTREAM 属性存储的大值数据。  
  
## <a name="see-also"></a>请参阅

- [SQL Server 数据类型和 ADO.NET](../../../../../docs/framework/data/adonet/sql/sql-server-data-types.md)
- [ADO.NET 中的 SQL Server 数据操作](../../../../../docs/framework/data/adonet/sql/sql-server-data-operations.md)
- [在 ADO.NET 中检索和修改数据](../../../../../docs/framework/data/adonet/retrieving-and-modifying-data.md)
- [ADO.NET 托管提供程序和数据集开发人员中心](https://go.microsoft.com/fwlink/?LinkId=217917)
