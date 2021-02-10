---
description: 对 HelloData 的注释
title: HelloData 上的注释 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- hellodata sample application [ADO]
ms.assetid: a2831d77-7040-4b73-bbae-fe0bf78107ed
author: rothja
ms.author: jroth
ms.openlocfilehash: e0510d75c69caadd52a595d58900662cc8ef7ad4
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037707"
---
# <a name="comments-on-hellodata"></a>对 HelloData 的注释
HelloData 应用程序逐句通过典型 ADO 应用程序的基本操作：获取、检查、编辑和更新数据。 启动应用程序时，单击第一个按钮 " **获取数据**"。 这会 **运行 "有**  
  
## <a name="getdata"></a>GetData  
 有效的连接字符串将有效的连接 **字符串放入** 模块级变量， *m_sConnStr*。 有关连接字符串的详细信息，请参阅 [创建连接字符串](./creating-a-connection-string.md)。  
  
 使用 Visual Basic **OnError** 语句分配错误处理程序。 有关 ADO 中的错误处理的详细信息，请参阅 [错误处理](./error-handling.md)。 将创建一个新的 **连接** 对象，并将 **CursorLocation** 属性设置为 **adUseClient** ，因为 HelloData 示例会创建一个 *断开连接的记录集*。 这意味着，一旦从数据源中提取数据，与数据源的物理连接就会中断，但仍可以使用 **记录在记录集** 对象中的数据。  
  
 打开连接后，将 SQL 字符串分配给变量 (Sql) 。 然后创建新的 **记录集** 对象的实例 `m_oRecordset1` 。 在下一行代码中，通过现有 **连接** 打开 **记录集**，并传入 `sSQL` 作为 **记录集** 的源。 在确定作为 **记录集** 的源传递的 SQL 字符串是命令的文本定义时，可以通过将最后一个参数中的 **AdCmdText** 传递给 **RECORDSET Open** 方法来帮助 ADO。 此行还设置与 **记录集** 关联的 **LockType** 和 **CursorType** 。  
  
 下一行代码将 **MarshalOptions** 属性设置为与 **adMarshalModifiedOnly** 相等。 **MarshalOptions** 指示哪些记录应 (或 Web 服务器) 封送到中间层。 有关封送处理的详细信息，请参阅 COM 文档。 如果将 **adMarshalModifiedOnly** 与客户端光标一起使用 ([CursorLocation](../../reference/ado-api/cursorlocation-property-ado.md)  =  **adUseClient**) ，则仅在客户端上修改的记录写回中间层。 将 **MarshalOptions** 设置为 **adMarshalModifiedOnly** 可以提高性能，因为封送的行更少。  
  
 接下来，将 **记录集** 的 **ActiveConnection** 属性设置为 " **无**"，从而断开记录集。 有关详细信息，请参阅 [更新和保存数据](./updating-and-persisting-data.md)中的 "断开和重新连接记录集" 部分。  
  
 关闭与数据源的连接并销毁现有 **连接** 对象。 这将释放所使用的资源。  
  
 最后一步是将 **记录** 集设置为窗体上 Microsoft DataGrid 控件的数据 **源** ，以便可以轻松地在窗体上显示 **记录集中** 的数据。  
  
 单击第二个按钮 " **检查数据**"。 这将运行 **ExamineData** 子例程。  
  
## <a name="examinedata"></a>ExamineData  
 ExamineData 使用 **recordset** 对象的各种方法和属性来显示有关 **记录集中** 的数据的信息。 它通过使用 **RecordCount** 属性报告记录数。 它循环遍历 **记录集** 并在窗体上的 "显示" 文本框中打印 **AbsolutePosition** 属性的值。 此外，在循环中，第三个记录的 " **书签** " 属性的值将放入变量变量 *vBookmark*，以备以后使用。  
  
 例程使用之前存储的书签变量直接导航回第三条记录。 例程调用 **WalkFields** 子例程，该子例程循环遍历 **记录集** 的 **字段** 集合，并显示集合中每个 **字段** 的详细信息。  
  
 最后， **ExamineData** 将 **记录集** 的 Filter 属性仅用于 **筛选器****等于2的记录**。 应用此筛选器的结果立即显示在窗体上的显示网格中。  
  
 有关 **ExamineData** 子例程中所示功能的详细信息，请参阅 [检查数据](./examining-data.md)。  
  
 接下来，单击第三个按钮， **编辑数据**。 这将运行 **EditData** 子例程。  
  
## <a name="editdata"></a>EditData  
 当代码进入 **EditData** 子例程时， **记录集** 仍将 **在等于2的筛选** 器上进行筛选，以便只显示符合筛选条件的那些项。 它首先遍历 **记录集** ，并将 **记录集中** 每个可见项的价格增加10%。 通过将该字段的 "**值**" 属性设置为等于一个新的有效金额，可以更改 "**价格**" 字段的值。  
  
 请记住， **记录集** 与数据源断开连接。 在 **EditData** 中所做的更改仅对数据的本地缓存副本进行。 有关详细信息，请参阅 [编辑数据](./editing-data.md)。  
  
 在单击第四个按钮， **更新数据** 之前，将不会在数据源上进行更改。 这将运行 **UpdateData** 子例程。  
  
## <a name="updatedata"></a>UpdateData  
 UpdateData 首先删除已应用于 **记录集** 的筛选器。 此代码将删除和重置 `m_oRecordset1` 为窗体上 Microsoft 绑定的 DataGrid 的 **数据源** ，以便在网格中显示未筛选的 **记录集** 。  
  
 然后，该代码将检查是否可以通过使用带有 **adMovePrevious** 参数的 **支持** 方法在 **记录集中** 向后移动。  
  
 例程使用 **MoveFirst** 方法移动到第一条记录，并使用 **Field** 对象的 **OriginalValue** 和 **Value** 属性显示该字段的原始值和当前值。 这些属性与此处)  (未使用的 **UnderlyingValue** 属性一起讨论了如何 [更新和保留数据](./updating-and-persisting-data.md)。  
  
 接下来，将创建一个新的 **连接** 对象并用于重新建立与数据源的连接。 通过将新 **连接** 设置为 **记录集** 的 **ActiveConnection** ，将 **记录集** 重新连接到数据源。 若要将更新发送到服务器，代码将对 **记录集** 调用 **UpdateBatch** 。  
  
 如果批更新成功，则模块级标志变量 `m_flgPriceUpdated` 将设置为 True。 这会提醒你稍后清除对数据库进行的所有更改。  
  
 最后，该代码将移回 **记录集中** 的第一条记录，并显示原始值和当前值。 调用 **UpdateBatch** 后值是相同的。  
  
 有关如何更新数据的详细信息，包括在断开 **记录集** 的情况下服务器上的数据更改时要执行的操作，请参阅 [更新和保留数据](./updating-and-persisting-data.md)。  
  
## <a name="form_unload"></a>Form_Unload  
 **Form_Unload** 子例程之所以重要，是因为有多种原因。 首先，因为这是一个示例应用程序，所以 Form_Unload 清除在应用程序退出之前对数据库所做的更改。 其次，此代码演示如何使用 **Execute** 方法直接从打开的 **连接** 对象执行命令。 最后，该示例演示了如何对数据源执行 (更新查询) 的非行返回查询。