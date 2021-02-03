---
title: “访问检查缓存”服务器配置选项 | Microsoft Docs
description: 了解访问检查结果缓存，以及控制该缓存行为的选项。 了解何时在 SQL Server 中更改这些选项。
ms.custom: ''
ms.date: 03/02/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: configuration
ms.topic: conceptual
helpviewer_keywords:
- access check cache option
- access check cache bucket count
- access check cache quota
ms.assetid: 0a992ea8-3ec6-4a4d-97b5-460ae7326247
author: markingmyname
ms.author: maghan
ms.openlocfilehash: dc1ff18d464c03d1eb5e96834f4d753bfead5559
ms.sourcegitcommit: b1cec968b919cfd6f4a438024bfdad00cf8e7080
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/01/2021
ms.locfileid: "99233974"
---
# <a name="access-check-cache-server-configuration-options"></a>“访问检查缓存”服务器配置选项
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]访问数据库对象时，访问检查缓存在一个名为 **访问检查结果缓存** 的内部结构中。 
  
“访问检查缓存桶计数”选项控制用于访问检查结果缓存的哈希桶的数目。 

“访问检查缓存配额”选项控制访问检查结果缓存中存储的条目数。 如果达到最大条目数，则从访问检查结果缓存中删除最早的条目。
  
默认值 0 表示 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 正在管理这些选项。 从 [!INCLUDE[sssql16-md](../../includes/sssql16-md.md)] 开始，默认值将转换为以下内部配置：
-   对于访问检查缓存桶计数，值 0 将设置为默认值（即 256 个桶）。
-   对于访问检查缓存配额，值 0 将设置为默认值（即 1,024 个条目）。

在极少数情况下，可以通过更改这些选项来提高性能。 例如，如果使用了太多内存，则可能希望减小访问检查结果缓存的大小。 或者，如果在重新计算权限时 CPU 使用率较高，则可能希望增加访问检查结果缓存的大小。
 
> [!IMPORTANT]
> Microsoft 建议仅在有 Microsoft 客户支持服务部门提供指导的情况下才更改这些选项。 如果必须更改“访问检查缓存桶计数”和“访问检查缓存配额”的值，请使用 1:4 的比率。 例如，如果将“访问检查缓存桶计数”的值更改为 512，则应将“访问检查缓存配额”的值更改为 2,048。 
  
## <a name="see-also"></a>另请参阅  
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)   
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)  
  
  
