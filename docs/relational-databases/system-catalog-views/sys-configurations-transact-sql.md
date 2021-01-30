---
description: sys.configurations (Transact-SQL)
title: sys.configurations (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.configurations_TSQL
- configurations
- sys.configurations
- configurations_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.configurations catalog view
ms.assetid: c4709ed1-bf88-4458-9e98-8e9b78150441
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: bca35c6287a88c1439ca64c6f7cfd431de0269ea
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159107"
---
# <a name="sysconfigurations-transact-sql"></a>sys.configurations (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  系统中每个服务器范围的配置选项值各占一行。  

|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**configuration_id**|**int**|配置值的唯一 ID。|  
|name|**nvarchar(35)**|配置选项的名称。|  
|**value**|**sql_variant**|该选项的配置值。|  
|**最小值**|**sql_variant**|配置选项的最小值。|  
|**最大值**|**sql_variant**|配置选项的最大值。|  
|**value_in_use**|**sql_variant**|该选项当前使用的运行值。|  
|description|**nvarchar(255)**|配置选项的说明。|  
|**is_dynamic**|**bit**|1 = 执行 RECONFIGURE 语句时生效的变量。|  
|**is_advanced**|**bit**|1 = 只有在设置了 **show advancedoption** 时才会显示变量。|  
  
 ## <a name="remarks"></a>备注
  有关所有服务器配置选项的列表，请参阅 [服务器配置选项 &#40;SQL Server&#41;](../../database-engine/configure-windows/server-configuration-options-sql-server.md)。  
  
> [!NOTE]  
>  有关数据库级配置选项，请参阅 [ALTER DATABASE 作用域配置 &#40;transact-sql&#41;](../../t-sql/statements/alter-database-scoped-configuration-transact-sql.md)。 若要配置软件 NUMA，请参阅 [软 numa &#40;SQL Server&#41;](../../database-engine/configure-windows/soft-numa-sql-server.md)。  
 
sys.configurations 目录视图可用于确定 config_value (值列) 、run_value (value_in_use 列) ，以及配置选项是否为动态 (不需要服务器引擎重新启动或 is_dynamic 列) 。

> [!NOTE]
> Sp_configure 的结果集中的 config_value 与 **sys.config的 urations** 列相同。 **Run_value** 等效于 **sys.configurations.value_in_use** 列。

以下查询可用于确定是否已安装了任何配置值：

```SQL
select * from sys.configurations where value != value_in_use
```

如果值等于所做的配置选项的更改，但 **value_in_use** 不相同，请重新配置命令未运行或失败，或者必须重新启动服务器引擎。

有些配置选项的值和 value_in_use 可能不相同，这是预期的行为。 例如：

"max server memory (MB) "-默认配置值0显示为 **value_in_use** = 2147483647<br>

"min server memory (MB) "-默认配置值0可能显示为 **value_in_use** = 8 (32 位) 或 16 (64 位) 。 在某些情况下， **value_in_use** 为0。 在这种情况下，"true" **value_in_use** 为 8 (32 位) 或 16 (64 位) 。


**Is_dynamic** 列可用于确定配置选项是否需要重新启动。 is_dynamic = 1 表示当执行重新配置 (T-sql) 命令时，新值会立即生效 (在某些情况下，服务器引擎可能不会立即计算新值，而是在执行) 的正常过程中执行此操作。 is_dynamic = 0 意味着在重新启动服务器之前更改的配置值将不会生效，即使已执行重新配置 (T-sql) 命令。

对于非动态的配置选项，无法判断是否已运行重新配置 (T-sql) 命令，以执行安装配置更改的第一步。 在重新启动 SQL Server 以安装配置更改之前，请运行 "重新配置 (T-sql) " 命令，以确保所有配置更改在 SQL Server 重启之后生效。 
 
 
## <a name="permissions"></a>权限  
 要求 **公共** 角色具有成员身份。 有关详细信息，请参阅 [Metadata Visibility Configuration](../../relational-databases/security/metadata-visibility-configuration.md)。  
  
## <a name="see-also"></a>另请参阅  
 [服务器范围内的配置目录视图 &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/server-wide-configuration-catalog-views-transact-sql.md)   
 [目录视图 (Transact-SQL)](../../relational-databases/system-catalog-views/catalog-views-transact-sql.md)  
  
  
