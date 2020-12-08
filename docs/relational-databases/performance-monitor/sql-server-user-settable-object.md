---
title: SQL Server - User Settable 对象 | Microsoft Docs
description: 了解用户可设置对象，该对象可在 SQL Server 中创建自定义计数器实例，以监视不受现有计数器监视的服务器方面。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- User Settable object
- SQLServer:User Settable
ms.assetid: 633de3ef-533c-4f0c-9c7b-c105129d8e94
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 980a7161eba7cc40da6de3f2c4172df7e2349c78
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96505556"
---
# <a name="sql-server-user-settable-object"></a>SQL Server User Settable 对象
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  通过 Microsoft **中的** User Settable [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 对象可以创建自定义计数器实例。 自定义计数器实例用于监视服务器上现有计数器没有监视到的方面，例如您的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库唯一具有的组件（例如，记录的客户定单数或产品目录）。  
  
 “User Settable”对象包含 10 个查询计数器实例：“用户计数器 1”到“用户计数器 10” 。 这些计数器映射到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 存储过程 **sp_user_counter1** 到 **sp_user_counter10**。 由于这些存储过程由用户应用程序执行，因此，这些存储过程设置的值显示在系统监视器中。 计数器可以监视任何单一的整型值，例如，用于计算某产品在一天中获得的订单数的存储过程。  
  
> [!NOTE]  
>  系统监视器不会自动轮询用户计数器存储过程。 必须由一个用户应用程序明确执行用户计数器存储过程，以更新计数器值。 可以使用触发器自动更新计数器值。 例如，要创建一个计数器来监视某表中的行数，可以针对执行以下语句的表创建 INSERT 和 DELETE 触发器： `SELECT COUNT(*) FROM table`。 每当由于对表执行了 INSERT 或 DELETE 操作而激发了触发器时，系统监视器计数器就会自动更新。  
  
 下表介绍 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] User Settable 对象。  
  
|SQL Server User Settable 计数器|说明|  
|---------------------------------------|-----------------|  
|**查询**|**User Settable** 对象包含 Query 计数器。 用户对查询对象中的 **用户计数器** 进行配置。|  
  
 此表列出了 **Query** 计数器的 **实例** 。  
  
|Query 计数器实例|说明|  
|-----------------------------|-----------------|  
|**用户计数器 1**|使用 **sp_user_counter1** 定义。|  
|**用户计数器 2**|使用 **sp_user_counter2** 定义。|  
|**用户计数器 3**|使用 **sp_user_counter3** 定义。|  
|...||  
|**用户计数器 10**|使用 **sp_user_counter10** 定义。|  
  
 要使用用户计数器存储过程，只需从自己的应用程序中执行它们，并用一个整型参数表示计数器的新值。 例如，若要将 **用户计数器 1** 的值设置为 10，执行下面的 Transact-SQL 语句：  
  
```  
EXECUTE sp_user_counter1 10  
```  
  
 用户计数器存储过程可从任何可以调用其他存储过程（例如自己的存储过程）的位置调用。 例如，可以创建以下存储过程来对自一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动以来连接和尝试连接的次数计数：  
  
```  
DROP PROC My_Proc  
GO  
CREATE PROC My_Proc  
AS   
   EXECUTE sp_user_counter1 @@CONNECTIONS  
GO  
```  
  
 @@CONNECTIONS 函数返回自一个 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例启动以来连接或尝试连接的次数。 此值作为参数传递给 **sp_user_counter1** 存储过程。  
  
> [!IMPORTANT]  
>  应使用户计数器存储过程中定义的查询尽可能简单。 执行排序或哈希操作等会占用大量内存的查询，或执行大量 I/O 操作的查询，开销会很大，并且会影响性能。  
  
## <a name="permissions"></a>权限  
 **sp_user_counter** 可供所有用户使用，但仅限于针对查询计数器。  
  
## <a name="see-also"></a>另请参阅  
 [监视资源使用情况（系统监视器）](../../relational-databases/performance-monitor/monitor-resource-usage-system-monitor.md)  
  
  
