---
title: 插入外侧 Transact-SQL 代码段
description: 了解如何插入外侧代码片段，将其用作在代码块中放置语句的起点。
ms.custom: seo-lt-2019
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- snippets [Transact-SQL], surround with
- IntelliSense [SQL Server], surround with snippets
- Transact-SQL snippets, surround with
ms.assetid: 5b5a8c6c-968e-4361-a7f5-9e2ac186d927
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 1d9316a8c181c6db8eeec8228229d9f11cf4604b
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97440368"
---
# <a name="insert-surround-with-transact-sql-snippets"></a>插入外侧 Transact-SQL 代码段
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  外侧代码段是一种模板，可将其作为在 BEGIN、IF 或 WHILE 块中插入一组 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句的起点。  
  
## <a name="inserting-surround-with-snippets"></a>插入外侧代码段  
 外侧代码段可以通过以下三种方式之一实现：键盘快捷键、“编辑”菜单和上下文菜单。  
  
 插入代码段后，必须更改替换文本，以形成有效的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句。 有关详细信息，请参阅 [完成 Transact-SQL 代码段](./complete-transact-sql-snippets.md)。  
  
#### <a name="to-insert-a-surround-with-snippet"></a>插入外侧代码段  
  
1.  在 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 查询编辑器窗口中，选择要包括在块中的语句组。  
  
2.  使用以下三种方法之一显示外侧代码段列表：  
  
    -   按 Ctrl+K、Ctrl+S。  
  
    -   在 **“编辑”** 菜单中，指向 **“IntelliSense”** ，然后选择 **“外侧代码”** 命令。  
  
    -   右键单击选定的文本，然后在上下文菜单中选择“外侧代码”命令。  
  
3.  使用鼠标或通过键入代码段名称并按 Tab 或 Enter 从列表中选择代码段名称（BEGIN、IF 或 WHILE）。  
  
## <a name="see-also"></a>另请参阅  
 [插入 Transact-SQL 代码片段](./insert-transact-sql-snippets.md)  
  
