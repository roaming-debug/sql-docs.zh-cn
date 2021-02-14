---
description: SQL 模式 (MySQLToSQL)
title: SQL 模式 (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: d840ee51-b863-4e77-84aa-37d3f094bfed
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e09a38ce70b4f144f6da324525a7fee925d586a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100074468"
---
# <a name="sql-modes-mysqltosql"></a>SQL 模式 (MySQLToSQL)
MySQL 的 SSMA 可以在不同的 SQL 模式下运行，并且可以针对不同的客户端以不同的方式应用这些模式。  
  
模式定义了 MySQL 应支持的 SQL 语法，以及应执行的数据验证检查的类型。 这样就可以更轻松地在不同的环境中使用 MySQL，并将 MySQL 与 SQL Server 配合使用。  
  
## <a name="sql-modes-grid"></a>SQL 模式网格：  
  
-   根级别的 SQL 模式网格包含以下列： **Sql 模式名称**、已 **加载的 Sql 模式** 和 **有效的 sql 模式**。  
  
-   "数据库"、"数据库"、"表"、"语句" 类别、"视图" 类别、"表"、"视图"、"函数"、"过程"、"UDF" 和 "事件对象" 级别的 SQL 模式网格包含以下列： **SQL 模式名称**、**继承的 Sql 模式** 和 **有效的 sql**  
  
-   存储过程、存储函数和触发器级别的 SQL 模式网格包含以下列： **Sql 模式名称**、  **原始 Sql 模式** 和 **有效的 sql 模式**。  
  
> [!NOTE]  
> 组模式将在 "SQL 模式名称" 列下以粗体显示。  
  
## <a name="loaded-sql-modes"></a>加载的 SQL 模式  
这些是在会话或根级别设置的 SQL 模式。 在将 SQL 模式加载到目标数据库后，无法编辑或修改这些模式。  
  
## <a name="inherited-sql-modes"></a>继承的 SQL 模式  
这些是 SQL 模式，它们继承自相应的父节点。  
  
除函数 category、过程类别、事件类别和触发器外，这些 SQL 模式 (数据库、表类别、语句类别、视图类别、表、视图、函数、过程、UDF 和事件对象) 的所有级别存在。  
  
> [!NOTE]  
> 通过选择 " **从父项继承** " 复选框，可以从父节点继承继承的 SQL 模式。 默认情况下，此复选框保持选中状态。  
  
## <a name="original-sql-modes"></a>原始 SQL 模式  
这些是仅存在于函数、过程和触发器级别的 SQL 模式。  
  
> [!NOTE]  
> 通过选中 " **使用原始** 项" 复选框，可以使用最初在相应函数或过程或触发器中使用的 SQL 模式。 默认情况下，此复选框保持选中状态。  
  
## <a name="effective-sql-modes"></a>有效的 SQL 模式  
可以通过以下方式在不同级别定义有效的 SQL 模式：  
  
-   **在会话级别：**  
  
    1.  所有已加载的 SQL 模式都可以调用 "有效的 SQL 模式"。  
  
    2.  在此级别，可以直接和显式修改有效的 SQL 模式。  
  
    3.  显式设置的有效 SQL 模式不会反映为已加载的 SQL 模式，最后应用于对象。  
  
-   **在函数或过程级别：**  
  
    1.  所有原始 SQL 模式都可以调用 "有效的 SQL 模式"。  
  
    2.  在此级别，仅当取消选中 " **使用原始** 项" 复选框时，才可以显式修改有效的 SQL 模式。  
  
    3.  显式设置的有效 SQL 模式不会反映为原始 SQL 模式，最后应用于对象。  
  
-   **在函数或过程或触发器级别以外的节点上：**  
  
    1.  所有继承的 SQL 模式都可以调用 "有效的 SQL 模式"。  
  
    2.  在此级别，仅当取消选中 " **从父项继承** " 复选框时，才可以显式修改有效的 SQL 模式。  
  
    3.  显式设置的有效 SQL 模式不会反映为继承的 SQL 模式，最后应用于对象。  
  
