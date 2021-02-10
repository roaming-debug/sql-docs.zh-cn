---
description: 使用 ADO 与 Microsoft Visual Basic 和 Visual Basic for Applications
title: 将 ADO 与 Microsoft Visual Basic 一起使用 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- VB
helpviewer_keywords:
- ADO, Visual Basic
- Visual Basic [ADO]
ms.assetid: 9dfb6784-037d-4f9d-bb7f-b506b4498573
author: rothja
ms.author: jroth
ms.openlocfilehash: 229662bd5db68039202f1f39d7309c12ff6354a4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100028856"
---
# <a name="using-ado-with-microsoft-visual-basic-and-visual-basic-for-applications"></a>使用 ADO 与 Microsoft Visual Basic 和 Visual Basic for Applications
无论你使用 Visual Basic 还是 Visual Basic for Applications，设置 ADO 项目和编写 ADO 代码都是类似的。 本主题介绍如何将 ADO 与 Visual Basic 和 Visual Basic for Applications 结合使用，并注意所有差异。

## <a name="referencing-the-ado-library"></a>引用 ADO 库
 ADO 库必须由您的项目引用。

#### <a name="to-reference-ado-from-microsoft-visual-basic"></a>从 Microsoft Visual Basic 引用 ADO

1.  在 Visual Basic 的 " **项目** " 菜单中，选择 " **引用 ...**"。

2.  从列表中选择 " **Microsoft ActiveX 数据对象 X.x.x.x 库** "。 验证是否还选择了以下库：

    -   Visual Basic for Applications

    -   Visual Basic 运行时对象和过程

    -   Visual Basic 对象和过程

    -   OLE 自动化

3.  单击“确定”。

 例如，使用 Microsoft Access 就可以轻松地使用 ADO，使用 Visual Basic for Applications。

#### <a name="to-reference-ado-from-microsoft-access"></a>引用 Microsoft Access 中的 ADO

1.  在 Microsoft Access 的 "**数据库**" 窗口中的 "**模块**" 选项卡中，选择或创建一个模块。

2.  在 " **工具** " 菜单上选择 " **引用 ...**"。

3.  从列表中选择 " **Microsoft ActiveX 数据对象 X.x.x.x 库** "。 验证是否还选择了以下库：

    -   Visual Basic for Applications

    -   Microsoft Access 8.0 对象库 (或更高版本) 

    -   Microsoft DAO 3.5 对象库 (或更高版本) 

4.  单击“确定”。

## <a name="creating-ado-objects-in-visual-basic"></a>在 Visual Basic 中创建 ADO 对象
 若要为该变量创建自动化变量和对象的实例，可以使用两种方法： **Dim** 或 **CreateObject**。

### <a name="dim"></a>Dim
 可以将 **New** 关键字与 **Dim** 一起使用来声明和创建 ADO 对象的实例，步骤如下：

```
Dim conn As New ADODB.Connection
```

 或者， **Dim** 语句声明和对象实例化也可以分为两个步骤：

```
Dim conn As ADODB.Connection
Set conn = New ADODB.Connection
```

> [!NOTE]
>  `ADODB`假设您已在项目中正确引用 ADO 库，则不需要使用 **Dim** 语句显式使用 progid。 但是，使用它可以确保不会与其他库发生命名冲突。

> [!NOTE]
>  例如，如果在同一个项目中同时包含对 ADO 和 DAO 的引用，则应包括一个限定符，以指定实例化 **记录集** 对象时要使用的对象模型，如以下代码所示：

```
Dim adoRS As ADODB.Recordset
Dim daoRS As DAO.Recordset
```

### <a name="createobject"></a>CreateObject
 对于 **CreateObject** 方法，声明和对象实例化必须是两个分离步骤：

```
Dim conn1
Set conn1 = CreateObject("ADODB.Connection") As Object
```

 用 **CreateObject** 实例化的对象是后期绑定的，这意味着它们不是强类型化的，并且禁用了命令行完成。 不过，它确实允许您跳过从项目引用 ADO 库，并使您能够实例化特定版本的对象。 例如：

```
Set conn1 = CreateObject("ADODB.Connection.2.0") As Object
```

 还可以通过专门创建对 ADO 版本2.0 类型库的引用并创建对象来实现此目的。

 使用 **CreateObject** 方法来实例化对象通常比使用 **Dim** 语句慢。

## <a name="handling-events"></a>处理事件
 若要在 Microsoft Visual Basic 中处理 ADO 事件，必须使用 **WithEvents** 关键字声明模块级变量。 变量只能声明为类模块的一部分，并且必须在模块级别声明。 有关处理 ADO 事件的更详尽的讨论，请参阅 [处理 Ado 事件](../data/handling-ado-events.md)。

## <a name="visual-basic-examples"></a>Visual Basic 示例
 ADO 文档附带了许多 Visual Basic 示例。 有关详细信息，请参阅 [Microsoft Visual Basic 中的 ADO 代码示例](../../reference/ado-api/ado-code-examples-in-visual-basic.md)。

## <a name="see-also"></a>另请参阅
 [Microsoft ActiveX 数据对象 (ado) ](../../microsoft-activex-data-objects-ado.md) [使用](./using-ado-with-microsoft-visual-c.md)ado 和使用[ado 和脚本语言](./using-ado-with-scripting-languages.md)进行 Microsoft Visual C++