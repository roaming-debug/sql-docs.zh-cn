---
description: HelloData：简单的 ADO 应用程序
title: HelloData：一个简单的 ADO 应用程序 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- HelloData sample application [ADO]
- ADO, samples
ms.assetid: de4bcd56-dac2-45e6-95ab-9fd7f25878fc
author: rothja
ms.author: jroth
ms.openlocfilehash: ec46ed0f94ffe9bd50d0576ab7b3855011bcf775
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033157"
---
# <a name="hellodata-a-simple-ado-application"></a>HelloData：简单的 ADO 应用程序
这个简单的应用程序逐步介绍四个主要 ADO 操作：获取、检查、编辑和更新数据。 这些操作是针对 Microsoft® SQL Server 包含的 Northwind 示例数据库执行的。 为了重点介绍 ADO 的基本知识并防止代码混乱，示例中的错误处理是最小的。  
  
### <a name="to-run-hellodata"></a>运行 HelloData  
  
1.  创建引用 ADO 库的新标准 EXE Visual Basic 项目。 有关详细信息，请参阅 [引用 ADO 库](../referencing-the-ado-libraries.md)。  
  
2.  在窗体顶部创建四个命令按钮，并将 " **名称** " 和 " **标题** " 属性设置为在本主题末尾的表中显示的值。  
  
3.  在按钮下方，添加一个 **Microsoft DataGrid 控件** (Msdatgrd) 。 Msdatgrd 文件随 Visual Basic 提供，位于 \windows\system32 或 \winnt\system32 目录中。 若要将 DataGrid 控件添加到 Visual Basic 的工具箱窗格，请从 "**项目**" 菜单中选择 "**组件 ...** "。 然后选中 "Microsoft DataGrid Control 6.0 (SP3)  (OLEDB) " 旁边的框，然后单击 **"确定**"。 若要将控件添加到项目中，请将 "DataGrid" 控件从工具箱拖动到 Visual Basic 窗体。  
  
4.  在网格下的窗体上创建一个 **文本框** ，并按表中所示设置其属性。 完成后，窗体应类似于下图。  
  
5.  最后，复制 [HelloData 代码](./hellodata-code.md)中列出的代码，并将其粘贴到窗体的代码编辑器窗口中。 按 **F5** 运行代码。  
  
> [!NOTE]
>  在以下示例中，在整个指南中，将使用密码为 "123aBc" 的用户 id "MyId" 对服务器进行身份验证。 应将这些值替换为服务器的有效登录凭据。 另外，请将 "MySQLServer" 值替换为服务器的名称。  
  
 有关代码的详细说明，请参阅 [HelloData 上的注释](./comments-on-hellodata.md)。  
  
 ![显示 HelloData VB 应用程序的 Form1](../../../ado/guide/data/media/hellodata.gif "HelloData")  
  
|控件类型|属性|值|  
|------------------|--------------|-----------|  
|窗体|名称|Form1|  
||高度|6500|  
||宽度|6500|  
|MS DataGrid|名称|grdDisplay1|  
|TextBox|名称|txtDisplay1|  
||多行|true|  
|命令按钮|名称|cmdGetData|  
||标题|获取数据|  
|命令按钮|名称|cmdExamineData|  
||标题|检查数据|  
|命令按钮|名称|cmdEditData|  
||标题| 编辑数据|  
|命令按钮|名称|cmdUpdateData|  
||标题|更新数据|