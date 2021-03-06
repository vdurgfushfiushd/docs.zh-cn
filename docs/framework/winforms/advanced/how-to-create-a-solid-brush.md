---
title: 如何：创建实心画笔
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- solid color brushes
- brushes [Windows Forms], examples
- brushes [Windows Forms], creating solid
ms.assetid: 85c3fe7d-fb1d-4591-8a9f-d75b556b90af
ms.openlocfilehash: ed9ec1f52b41c83b3cc6e36dedf97f1c00db42e6
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59213436"
---
# <a name="how-to-create-a-solid-brush"></a>如何：创建实心画笔
此示例将创建<xref:System.Drawing.SolidBrush>对象，它可由<xref:System.Drawing.Graphics>用于填充形状的对象。  
  
## <a name="example"></a>示例  
 [!code-cpp[System.Drawing.ConceptualHowTos#1](~/samples/snippets/cpp/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/cpp/form1.cpp#1)]
 [!code-csharp[System.Drawing.ConceptualHowTos#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/CS/form1.cs#1)]
 [!code-vb[System.Drawing.ConceptualHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.ConceptualHowTos/VB/form1.vb#1)]  
  
## <a name="robust-programming"></a>可靠编程  
 使用它们完成后，应调用<xref:System.IDisposable.Dispose%2A>占用系统资源，例如画笔对象的对象。  
  
## <a name="see-also"></a>请参阅

- <xref:System.Drawing.SolidBrush>
- <xref:System.Drawing.Brush>
- [图形编程入门](getting-started-with-graphics-programming.md)
- [GDI+ 中的画笔和填充形状](brushes-and-filled-shapes-in-gdi.md)
- [使用画笔填充形状](using-a-brush-to-fill-shapes.md)
