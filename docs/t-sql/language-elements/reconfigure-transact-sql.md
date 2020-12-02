---
description: RECONFIGURE (Transact-SQL)
title: RECONFIGURE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 05/20/2016
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- RECONFIGURE
- RECONFIGURE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- reconfiguring configuration options
- configuration options [SQL Server], reconfiguring
- updating configuration options
- RECONFIGURE, RECONFIGURE statement
- RECONFIGURE
- RECONFIGURE, WITH OVERRIDE statement
ms.assetid: 2e6e4eeb-b70b-4f45-a253-28ac4e595d75
author: rothja
ms.author: jroth
ms.openlocfilehash: 26c8d6f53ef87fa2d9e6ab5dcfee6b39be01834f
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92196785"
---
# <a name="reconfigure-transact-sql"></a>RECONFIGURE (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  更新使用 sp_configure 系统存储过程更改的配置选项的当前已配置值（sp_configure 结果集中的 config_value 列）。 由于有些配置选项需要服务器停止并重新启动才能更新当前运行的值，因此 RECONFIGURE 并不总是为已更改的配置值更新当前运行的值（sp_configure 结果集中的 run_value 列）。    
    
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)    
    
## <a name="syntax"></a>语法    
    
```syntaxsql
RECONFIGURE [ WITH OVERRIDE ]    
```    
    
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 RECONFIGURE    
 指定如果配置设置不需要服务器停止并重新启动，则更新当前运行的值。 RECONFIGURE 还会检查新的配置值中是否有无效值（例如，在 syscharsets 中不存在的排序顺序值）或非建议值。 对于那些不需要服务器停止并重新启动的配置选项，其当前运行的值和当前配置的值在指定 RECONFIGURE 之后应当相同。    
    
 WITH OVERRIDE    
 禁用对 recovery interval 高级配置选项的配置值检查（以查找无效值或非建议值）。    
    
 使用 WITH OVERRIDE 选项几乎可以重新配置任何配置选项，但仍然要避免某些灾难性错误。 例如，不能使用大于max server memory 配置选项中指定的值来配置 min server memory 配置选项。
      
## <a name="remarks"></a>备注    
 sp_configure 不接受超出所记录的各配置选项值有效范围的新配置选项值。    
    
 不允许在显式或隐式事务中使用 RECONFIGURE。 同时对多个选项进行重新配置时，如果任意重新配置操作失败，所有重新配置操作都将无效。    
    
 重新配置资源调控器时，请参阅的 [ALTER RESOURCE GOVERNOR (Transact-SQL)](../../t-sql/statements/alter-resource-governor-transact-sql.md) 的 RECONFIGURE 选项。    
    
## <a name="permissions"></a>权限    
 默认情况下，被授予 ALTER SETTINGS 权限的用户同时拥有 RECONFIGURE 权限。 sysadmin 和 serveradmin 固定服务器角色隐式持有该权限。    
    
## <a name="examples"></a>示例    
 以下示例将 `recovery interval` 配置选项的上限设置为 `75` 分钟，并使用 `RECONFIGURE WITH OVERRIDE` 进行安装。 建议恢复间隔不要大于 60 分钟，并且默认情况下是不接受的。 但是，由于指定了 `WITH OVERRIDE` 选项，因此 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会检查指定的值 (`75`) 是否为 `recovery interval` 配置选项的有效值。    
    
```sql    
EXEC sp_configure 'recovery interval', 75    
RECONFIGURE WITH OVERRIDE;    
GO    
```    
    
## <a name="see-also"></a>另请参阅    
 [服务器配置选项 (SQL Server)](../../database-engine/configure-windows/server-configuration-options-sql-server.md)     
 [sp_configure &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-configure-transact-sql.md)    
    
  
