---
title: 异常、事务和补偿
ms.date: 03/30/2017
helpviewer_keywords:
- programming [WF], error handling
ms.assetid: 694db4f9-7387-4b13-8f9f-b923b18c7490
ms.openlocfilehash: c4d66e252561ad7b896ff8092e80013c51bd463c
ms.sourcegitcommit: 160a88c8087b0e63606e6e35f9bd57fa5f69c168
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/09/2019
ms.locfileid: "57709466"
---
# <a name="exceptions-transactions-and-compensation"></a>异常、事务和补偿
[!INCLUDE[wf1](../../../includes/wf1-md.md)] 为在工作流中处理运行时错误条件提供了多个不同机制。 工作流可以结合使用异常处理程序、事务、取消以及补偿来妥善处理错误条件并从中恢复。  
  
## <a name="in-this-section"></a>本节内容  
 [异常](exceptions.md)  
 演示如何使用 <xref:System.Activities.Statements.TryCatch> 活动在工作流中处理异常。  
  
 [事务](workflow-transactions.md)  
 演示如何使用 <xref:System.Activities.Statements.TransactionScope> 活动在工作流中使用事务。  
  
 [补偿](compensation.md)  
 介绍工作流中的补偿并演示如何使用补偿活动，如 <xref:System.Activities.Statements.CompensableActivity>、<xref:System.Activities.Statements.Compensate> 和 <xref:System.Activities.Statements.Confirm>。  
  
 [取消](modeling-cancellation-behavior-in-workflows.md)  
 介绍如何使用内置活动以及自定义活动来在工作流中执行取消处理。  
  
 [调试工作流](debugging-workflows.md)  
 介绍如何调试工作流。
