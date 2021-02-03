---
description: MSSQLSERVER_8710
title: MSSQLSERVER_8710 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8710 (Database Engine error)
ms.assetid: 78b9f9da-5489-4be0-94df-f065d86ed18c
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: f515311384b33f508bbf125f80977a995fa7cb2b
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99194054"
---
# <a name="mssqlserver_8710"></a>MSSQLSERVER_8710
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|MSSQLSERVER|  
|事件 ID|8710|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|QUERY2_CUBE_ILLEGAL_AGG_FUNC|  
|消息正文|必须提供与 CUBE、ROLLUP 或 GROUPING SET 查询一起使用的聚合函数，才能合并子聚合。 若要修复此问题，请删除该聚合函数或在 GROUP BY 子句基础上使用 UNION ALL 编写查询。|  
  
## <a name="explanation"></a>说明  
CUBE、ROLLUP 或 GROUPING SETS 不提供合并子聚合的方法，而将它们与聚合函数一起使用即可合并子聚合。  
  
## <a name="user-action"></a>用户操作  
若要修复此问题，请删除该聚合函数或在 GROUP BY 子句基础上使用 UNION ALL 编写查询。  
  
