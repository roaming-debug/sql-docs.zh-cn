---
description: MSSQLSERVER_8649
title: MSSQLSERVER_8649 | Microsoft Docs
ms.custom: ''
ms.date: 04/04/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: supportability
ms.topic: reference
helpviewer_keywords:
- 8649 (Database Engine error)
ms.assetid: 992dbc74-7c3a-498b-9f1b-b28387640677
author: MashaMSFT
ms.author: mathoma
ms.openlocfilehash: d1041fa6b16b93d2d3a78dbdc0c965228f5145f0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99205663"
---
# <a name="mssqlserver_8649"></a>MSSQLSERVER_8649
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  
## <a name="details"></a>详细信息  
  
| Attribute | 值 |  
| :-------- | :---- |  
|产品名称|SQL Server|  
|事件 ID|8649|  
|事件源|MSSQLSERVER|  
|组件|SQLEngine|  
|符号名称|COST_TOO_HIGH|  
|消息正文|查询已取消，因为此查询的估计开销 (%d) 出了配置的阈值 %d。 请与系统管理员联系。|  
  
## <a name="explanation"></a>说明  
查询已取消，因为此查询的估计开销超出了为 QUERY_GOVERNOR_COST_LIMIT 设置的配置阈值。  
  
## <a name="user-action"></a>用户操作  
将 QUERY_GOVERNOR_COST_LIMIT 选项设置为更大的值。  
  
## <a name="see-also"></a>另请参阅  
[SET QUERY_GOVERNOR_COST_LIMIT (Transact-SQL)](~/t-sql/statements/set-query-governor-cost-limit-transact-sql.md)  
  
