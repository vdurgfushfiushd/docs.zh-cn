---
title: 如何：使用“继承选择器”对话框继承窗体
ms.date: 03/30/2017
helpviewer_keywords:
- inheritance [Windows Forms], forms
- Inheritance Picker dialog box
- inherited forms [Windows Forms], creating
ms.assetid: 969b4c04-12aa-4297-93a2-0ae747447823
ms.openlocfilehash: 5ae1c236835141b10bc704cd39f55de6e3e974b0
ms.sourcegitcommit: 0be8a279af6d8a43e03141e349d3efd5d35f8767
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/18/2019
ms.locfileid: "59342084"
---
# <a name="how-to-inherit-forms-using-the-inheritance-picker-dialog-box"></a>如何：使用“继承选择器”对话框继承窗体
继承窗体或其他对象最简单的方法是使用“继承选择器”对话框。 通过该对话框，可以充分利用在其他解决方案中创建的代码或用户界面 (UI)。  
  
> [!NOTE]
>  若要使用“继承选择器”对话框继承窗体，必须先将包含该窗体的项目生成到可执行文件或 DLL 中。 若要生成项目，请从“生成”菜单选择“生成解决方案”。  
>   
>  显示的对话框和菜单命令可能会与“帮助”中的描述不同，具体取决于你现用的设置或版本。 若要更改设置，请在 **“工具”** 菜单上选择 **“导入和导出设置”** 。 有关详细信息，请参阅[个性化设置 Visual Studio IDE](/visualstudio/ide/personalizing-the-visual-studio-ide)。  
  
### <a name="to-create-a-windows-form-inherited-from-an-existing-form-by-using-the-inheritance-picker"></a>通过使用继承选择器创建继承自现有窗体的 Windows 窗体  
  
1. 从“项目”菜单中，选择“添加 Windows 窗体”。  
  
     此时将打开“添加新项”对话框。  
  
2. 搜索**继承的窗体**模板从 searchbox 或通过单击**Windows 窗体**类别中，选择它，并将其在命名**名称**框。 单击“添加”按钮继续。  
  
     此时将打开“继承选择器”。 如果当前项目已经包含窗体，则它们将显示在“继承选择器”对话框中。  
  
3. 若要继承另一个程序集中的窗体，请单击“浏览”按钮。  
  
4. 在“选择包含要继承组件的文件”对话框中，导航到包含所需窗体或模块的项目。  
  
5. 单击 .exe 或 .dll 文件的名称以选中该文件，然后单击“打开”按钮。  
  
     此操作将返回到“继承选择器”对话框，对话框中现在列出了组件及组件所在的项目。  
  
6. 选择组件。  
  
     在“解决方案资源管理器”中，将组件添加到项目。 如果它具有 UI，将用标志符号标记属于继承窗体的控件 (![的 Visual Basic 继承符号。 屏幕截图](./media/how-to-inherit-forms-using-the-inheritance-picker-dialog-box/visual-basic-inheritance-glyph.gif))，并选中时，有一个边框指示的控件具有的安全级别超类窗体。 下表中列出了与不同安全级别相对应的行为。  
  
    |控件的安全级别|可以通过设计器和代码编辑器与继承的窗体进行的交互|  
    |-------------------------------|--------------------------------------------------------------------------------|  
    |Public|带有大小调整控点的标准边框：可以调整控件大小和移动控件。 控件可以由声明它的类进行内部访问，也可由其他类进行外部访问。|  
    |Protected|带有大小调整控点的标准边框：可以调整控件大小和移动控件。 可以由声明它的类和任何继承自父类的类进行内部访问，但不能由外部类进行访问。|  
    |Protected Internal（Visual Basic 中为 Protected Friend）|带有大小调整控点的标准边框：可以调整控件大小和移动控件。 可以由声明它的类、任何继承自父类的类以及包含它的程序集的其他成员进行内部访问。|  
    |Internal（Visual Basic 中为 Friend）|窗体上显示不带有大小句柄的标准边框，属性在“属性”窗口中可见。 但是，该控件的所有方面都将考虑为只读。 无法对控件进行移动、调整大小或更改其属性。 如果控件是其他控件的容器（例如分组框），则无法添加新控件，也无法移除现有控件，即使那些控件是 public 控件。 此控件只能由包含它的程序集的其他成员进行访问。|  
    |Private|窗体上显示不带有大小句柄的标准边框，属性在“属性”窗口中可见。 但是，该控件的所有方面都将考虑为只读。 无法对控件进行移动、调整大小或更改其属性。 如果控件是其他控件的容器（例如分组框），则无法添加新控件，也无法移除现有控件，即使那些控件是 public 控件。 此控件只能由声明它的类进行访问。|  
  
     若要了解如何更改基窗体的外观，请参阅[修改基窗体外观的效果](effects-of-modifying-base-form-appearance.md)。  
  
    > [!NOTE]
    >  将继承的控件和组件与 Windows 窗体上的标准控件和组件组合到一起时，可能会与 Z 排序发生冲突。 可通过修改 z 顺序纠正冲突，方法是单击“格式”菜单，指向“排序”，然后单击“置于顶层”或“置于底层”。 控件的 z 顺序的详细信息，请参阅[如何：在 Windows 窗体上的对象分层](../controls/how-to-layer-objects-on-windows-forms.md)。  
  
## <a name="see-also"></a>请参阅

- [Inherits 语句](~/docs/visual-basic/language-reference/statements/inherits-statement.md)
- [using](~/docs/csharp/language-reference/keywords/using.md)
- [修改基窗体的外观的效果](effects-of-modifying-base-form-appearance.md)
- [Windows 窗体可视化继承](windows-forms-visual-inheritance.md)
