---
title: 执行部分查询
description: 获取有关调试部分复杂查询的帮助。 使用 Transact-SQL 编辑器来突出显示特定的脚本段并将其作为单个查询执行。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: af04ab37-6cbb-4185-9382-e5922fa5b1df
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 25ad063fc90539647a48bac2702e0cd2b80cd53b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018197"
---
# <a name="how-to-execute-a-partial-query"></a>如何：执行部分查询

Transact\-SQL 编辑器允许突出显示特定的脚本段落并将其作为单个查询执行。 这便于您调试复杂查询部分。  
  
> [!WARNING]  
> 以下过程利用在[连接的数据库开发](../ssdt/connected-database-development.md)和[面向项目的脱机数据库开发](../ssdt/project-oriented-offline-database-development.md)部分中的之前过程中创建的实体。  
  
## <a name="to-partially-execute-a-query"></a>部分执行查询  
  
1. 在“SQL Server 对象资源管理器”中，双击“视图”下的 PerishableFruits 以在 Transact\-SQL 编辑器中将其打开。  
  
2. 突出显示代码中的 `SELECT p.Id, p.Name FROM dbo.Product p` 段，右键单击并选择“执行查询”。  
  
3. 请注意，在 `Products` 表中具有指定字段的所有行都将在“结果”窗格中返回。