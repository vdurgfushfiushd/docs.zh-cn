---
title: 命令目录树的形状
ms.date: 03/30/2017
ms.assetid: 2215585e-ca47-45f8-98d4-8cb982f8c1d3
ms.openlocfilehash: 08a67c8d181188cbc14c6f60876a7e26cd6de25a
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/22/2019
ms.locfileid: "59980078"
---
# <a name="the-shape-of-the-command-trees"></a>命令目录树的形状

SQL 生成模块负责生成基于给定输入查询命令目录树表达式的后端特定 SQL 查询。 本节讨论查询命令目录树的特征、属性和结构。

## <a name="query-command-trees-overview"></a>查询命令目录树概述

查询命令目录树是查询的对象模型表示形式。 查询命令目录树有两个用途：

- 表示针对[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]指定的输入查询。

- 表示为提供程序给定的输出查询并描述针对后端的查询。

查询命令目录树支持比符合 SQL:1999 的查询更丰富的语义，包括支持使用嵌套集合和类型操作（如检查一个实体是否属于某个特定类型或基于类型筛选集）。

DBQueryCommandTree.Query 属性是用于描述查询逻辑的表达式树的根。 DBQueryCommandTree.Parameters 属性包含查询中使用的参数的列表。 表达式树由 DbExpression 对象组成。

DbExpression 对象表示某个计算。 [!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]提供了用于编写查询表达式的多种表达式，包括常量、变量、函数、构造函数和标准关系运算符（例如筛选和联接）。 每个 DbExpression 对象都有一个 ResultType 属性，表示由该表达式生成的结果的类型。 此类型表示为 TypeUsage。

## <a name="shapes-of-the-output-query-command-tree"></a>输出查询命令目录树的形状

输出查询命令目录树紧密表示关系 (SQL) 查询，并遵循比应用于查询命令目录树的规则更严格的规则。 它们通常包含易于转换为 SQL 的构造。

输入命令目录树是针对概念模型表示的，此模型支持导航属性、实体间的关联以及继承。 输出命令目录树是针对存储模型表示的。 输入命令目录树允许你投影嵌套集合，而输出命令目录树不允许你这样做。

输出查询命令目录树是使用 DbExpression 对象的子集生成的，并且对该子集中的一些表达式的使用会受到限制。

输出命令目录树中不存在诸如检查一个给定表达式是否属于某个特定类型或检查是否基于某个类型筛选集这样的类型操作。

在输出命令目录树中，只有返回布尔值的表达式才用于投影，并且仅用于需要谓词的表达式中的谓词（如筛选语句或 Case 语句）。

输出查询命令目录树的根是一个 DbProjectExpression 对象。

### <a name="expression-types-not-present-in-output-query-command-trees"></a>输出查询命令目录树中不存在表达式类型

下列表达式类型在输出查询命令目录树中无效，并且提供程序无需处理这些类型：

- DbDerefExpression

- DbEntityRefExpression

- DbRefKeyExpression

- DbIsOfExpression

- DbOfTypeExpression

- DbRefExpression

- DbRelationshipNavigationExpression

- DbTreatExpression

### <a name="expression-restrictions-and-notes"></a>表达式限制和说明

许多表达式只能以受限方式在输出查询命令目录树中进行使用：

#### <a name="dbfunctionexpression"></a>DbFunctionExpression

可以传递以下函数类型：

- 由 Edm 命名空间识别的规范函数。

- 由 BuiltInAttribute 识别的内置（存储）函数。

- 用户定义的函数。

规范函数 (请参阅[规范函数](../../../../../docs/framework/data/adonet/ef/language-reference/canonical-functions.md)有关详细信息) 的一部分指定[!INCLUDE[adonet_ef](../../../../../includes/adonet-ef-md.md)]，和提供程序应提供的基于这些规范的规范函数的实现。 存储函数基于相应的提供程序清单中的规范。 用户定义的函数基于 SSDL 中的规范。

另外，具有 NiladicFunction 特性的函数没有任何参数，并且此类函数在转换时末尾不应有括号。  它是到 *\<functionName >* 而不是 *\<functionName > （)*。

#### <a name="dbnewinstanceexpression"></a>DbNewInstanceExpression

DbNewInstanceExpression 只能在以下两种情况下出现：

- 作为 DbProjectExpression 的 Projection 属性。  在此情况下，以下限制将适用：

  - 结果类型必须是行类型。

  - 它包含的每个参数都是一个生成带基元类型的结果的表达式。 通常，每个参数都是一个标量表达式（如 DbVariableReferenceExpression 上的 PropertyExpression）、一个函数调用或一个 DbVariableReferenceExpression 上的 DbPropertyExpression 或函数调用的算术计算。 然而，表示标量子查询的的表达式也可以在 DbNewInstanceExpression 的参数列表中出现。 一个表示标量子查询表达式是一个表示返回一个行和一个列包含 DbElementExpression 对象根的基元类型的子查询的表达式树

- 与集合返回类型一起使用，在此情况下，它定义一个作为自变量提供的表达式的新集合。

#### <a name="dbvariablereferenceexpression"></a>DbVariableReferenceExpression

A DbVariableReferenceExpression 必须是 DbPropertyExpression 节点的子级。

#### <a name="dbgroupbyexpression"></a>DbGroupByExpression

DbGroupByExpression 的 Aggregates 属性只能具有类型 DbFunctionAggregate 的元素。 不存在其他聚合类型。

#### <a name="dblimitexpression"></a>DbLimitExpression

属性 Limit 只能为 DbConstantExpression 或 DbParameterReferenceExpression。 另外，从 3.5 版的 .NET Framework 开始，属性 WithTies 始终为 false。

#### <a name="dbscanexpression"></a>DbScanExpression

输出命令目录树中使用时，DbScanExpression 有效地对表、 视图或存储查询，由 EntitySetBase::Target 表示一次扫描。

如果元数据属性"Defining Query"的目标为非 null，则它表示一个查询，该元数据属性中存储架构定义中指定的提供程序的特定语言 （或方言） 提供的查询文本。

否则，目标表示一个表或视图。 其架构前缀位于"Schema"元数据属性，如果不为 null，否则实体容器名称。  表或视图名称是"Table"元数据属性，如果不为 null，否则为该实体的名称属性设置基。

所有这些属性源自存储架构定义文件 (SSDL) 中对应的 EntitySet 的定义。

### <a name="using-primitive-types"></a>使用基元类型

在输出命令目录树中引用基元类型时，通常会在概念模型的基元类型中引用它们。 不过，对于某些表达式，提供程序需要相应的存储基元类型。 此类表达式包括 DbCastExpression 且可能包括 DbNullExpression（如果提供程序需要将 null 强制转换为相应类型）等。 在这些情况下，提供程序应基于基元类型的类别和 facet 对提供程序类型进行映射。

## <a name="see-also"></a>请参阅

- [SQL 生成](../../../../../docs/framework/data/adonet/ef/sql-generation.md)
