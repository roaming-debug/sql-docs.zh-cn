---
title: 警报属性（“历史记录”页）
description: 警报属性（“历史记录”页）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- sql13.ag.alert.history.f1
ms.assetid: f5359f5c-93a3-4a4a-8286-e9fe6f0196c7
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 01/19/2017
monikerRange: = azuresqldb-mi-current || >= sql-server-2016
ms.openlocfilehash: 4dd7bce8b5894ab272250ae68b74ff7a0eac2b1a
ms.sourcegitcommit: cb8e2ce950d8199470ff1259c9430f0560f0dc1d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/05/2021
ms.locfileid: "97878962"
---
# <a name="alert-properties-history-page"></a>警报属性（“历史记录”页）
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]


> [!IMPORTANT]  
> [Azure SQL 托管实例](/azure/sql-database/sql-database-managed-instance)目前支持大多数（但不是所有）SQL Server 代理功能。 有关详细信息，请参阅 [Azure SQL 托管实例与 SQL Server 的 T-SQL 区别](/azure/sql-database/sql-database-managed-instance-transact-sql-information#sql-server-agent)。


使用此页可以查看和修改 [!INCLUDE[msCoName](../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理警报的历史记录。  

## <a name="options"></a>选项  
**上次警报的日期**  
显示指定事件上次发生的日期，或者如果自从创建警报后从未发生该事件则显示“(从未发生)”。  
  
**上次响应的日期**  
显示警报上次响应该事件的日期，或者如果自从创建警报后从未发生该事件则显示“(从未响应)”。  
  
**出现次数**  
自从创建警报后或从上次重置计数后，该事件发生的总次数。  
  
**重置计数**  
重置此页上的信息。  
  
## <a name="see-also"></a>另请参阅  
[警报](../../ssms/agent/alerts.md)  
