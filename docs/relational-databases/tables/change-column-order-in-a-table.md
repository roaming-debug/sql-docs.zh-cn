---
description: 更改表中的列顺序
title: 更改表中的列顺序 | Microsoft Docs
ms.custom: ''
ms.date: 06/15/2018
ms.prod: sql
ms.prod_service: table-view-index, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: table-view-index
ms.topic: conceptual
helpviewer_keywords:
- columns [SQL Server], change order in a table
- column order, change
ms.assetid: cd99ef56-9085-431a-a0fc-58e7add5399f
author: stevestein
ms.author: sstein
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: d8064366c25925bf5ba6781e0afa0168a8df04fe
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181517"
---
# <a name="change-column-order-in-a-table"></a>更改表中的列顺序

[!INCLUDE [sqlserver2016-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi-asa-pdw.md)]

  您可以通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] ，在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]的表设计器中更改列顺序。  
  
> [!CAUTION]  
>  更改表中的列顺序可能会影响依赖于特定列顺序的代码和应用程序。 这些代码和应用程序包括查询、视图、存储过程、用户定义函数和客户端应用程序等。 请在需要对列顺序进行任何更改之前慎重考虑。 最佳做法是指定在应用程序级别和查询级别返回列的顺序。 您不应依赖于使用 SELECT * 基于在表中定义列的顺序以预期顺序返回所有列。 请始终按照您希望它们出现的顺序在您的查询和应用程序中按名称指定列。  
  
 **本主题内容**  
  
-   **使用以下工具更改列顺序：**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a> 使用 SQL Server Management Studio  
  
#### <a name="to-change-the-column-order"></a>更改列顺序  
  
1.  在“对象资源管理器”中，右键单击包含要重新排序的列的表，再单击“设计”。  
  
2.  选择要重新排序的列名称左侧的框。  
  
3.  将列拖动到表中的另一个位置。  
  
##  <a name="using-transact-sql"></a><a name="TsqlProcedure"></a> 使用 Transact-SQL  
 **更改列顺序**  
  
 不支持使用 Transact-SQL 语句执行此任务。  
  
###  <a name="TsqlExample"></a>  
