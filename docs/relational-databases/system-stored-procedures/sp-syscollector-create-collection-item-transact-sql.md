---
description: sp_syscollector_create_collection_item (Transact-SQL)
title: sp_syscollector_create_collection_item (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 08/09/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sp_syscollector_create_collection_item
- sp_syscollector_create_collection_item_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sp_syscollector_create_collection_item
- data collector [SQL Server], stored procedures
ms.assetid: 60dacf13-ca12-4844-b417-0bc0a8bf0ddb
author: markingmyname
ms.author: maghan
ms.openlocfilehash: e58f8407c152be375099e9eb70e3fe63d0100e67
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99207040"
---
# <a name="sp_syscollector_create_collection_item-transact-sql"></a>sp_syscollector_create_collection_item (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  在用户定义的收集组中创建一个收集项。 收集项定义要收集的数据以及数据收集所依据的频率。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sp_syscollector_create_collection_item   
      [ @collection_set_id = ] collection_set_id   
    , [ @collector_type_uid = ] 'collector_type_uid'  
    , [ @name = ] 'name'   
    , [ [ @frequency = ] frequency ]  
    , [ @parameters = ] 'parameters'  
    , [ @collection_item_id = ] collection_item_id OUTPUT  
```  
  
## <a name="arguments"></a>参数  
 [ @collection_set_id =] *collection_set_id*  
 收集组的唯一本地标识符。 *collection_set_id* 是 **int**。  
  
 [ @collector_type_uid =] "*collector_type_uid*"  
 标识要用于此项的收集器类型的 GUID *collector_type_uid* 是无默认值的 **uniqueidentifier** 。 有关收集器类型的列表，请查询 syscollector_collector_types 系统视图。  
  
 [ @name =] "*name*"  
 收集项的名称。 *名称* 为 **sysname** ，不能为空字符串或 NULL。  
  
 *名称* 必须是唯一的。 有关当前收集项名称的列表，请查询 syscollector_collection_items 系统视图。  
  
 [ @frequency =] *频率*  
 用于指定此收集项收集数据的频率（秒）。 *frequency* 的值为 **int**，默认值为5。 可指定的最小值为 5 秒。  
  
 如果将收集组设置为非缓存模式，则忽略频率，这是因为此模式将导致数据收集和上载按为收集组指定的计划执行。 若要查看收集组的收集模式，请查询 " [syscollector_collection_sets](../../relational-databases/system-catalog-views/syscollector-collection-sets-transact-sql.md) 系统" 视图。  
  
 [ @parameters =] "*parameters*"  
 收集器类型的输入参数。 *参数* 的值为 **xml** ，默认值为 NULL。 *参数* 架构必须与收集器类型的参数架构匹配。  
  
 [ @collection_item_id =] *collection_item_id*  
 标识收集组项的唯一标识符。 *collection_item_id* 为 **int** ，并且具有 OUTPUT。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 sp_syscollector_create_collection_item 必须在 msdb 系统数据库的上下文中运行。  
  
 必须先停止要向其添加收集项的收集组，然后才能创建收集项。 不能向系统收集组添加收集项。  
  
## <a name="permissions"></a>权限  
 需要具有 dc_admin（拥有 EXECUTE 权限）固定数据库角色的成员身份才能执行此过程。  
  
## <a name="examples"></a>示例  
 下面的示例根据收集类型`Generic T-SQL Query Collector Type`创建一个收集项，并将该项添加到名为 `Simple collection set test 2` 的收集组。 若要创建指定的收集组，请运行 [sp_syscollector_create_collection_set &#40;transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)中的示例 B。  
  
```  
USE msdb;  
GO  
DECLARE @collection_item_id int;  
DECLARE @collection_set_id int = (SELECT collection_set_id   
                                  FROM syscollector_collection_sets  
                                  WHERE name = N'Simple collection set test 2');  
DECLARE @collector_type_uid uniqueidentifier =   
    (SELECT collector_type_uid  
     FROM syscollector_collector_types  
     WHERE name = N'Generic T-SQL Query Collector Type');  
DECLARE @params xml =   
    CONVERT(xml, N'\<ns:TSQLQueryCollector xmlns:ns="DataCollectorType">  
            <Query>  
                <Value>SELECT * FROM sys.objects</Value>  
                <OutputTable>MyOutputTable</OutputTable>  
            </Query>  
            <Databases>   
                <Database> UseSystemDatabases = "true"   
                           UseUserDatabases = "true"  
                </Database>  
            </Databases>  
         \</ns:TSQLQueryCollector>');  
  
EXEC sp_syscollector_create_collection_item  
    @collection_set_id = @collection_set_id,  
    @collector_type_uid = @collector_type_uid,  
    @name = 'My custom TSQL query collector item',  
    @frequency = 6000,  
    @parameters = @params,  
    @collection_item_id = @collection_item_id OUTPUT;  
```  
  
## <a name="see-also"></a>另请参阅  
 [系统存储过程 (Transact-SQL)](../../relational-databases/system-stored-procedures/system-stored-procedures-transact-sql.md)   
 [数据收集](../../relational-databases/data-collection/data-collection.md)   
 [sp_syscollector_update_collection_item &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-update-collection-item-transact-sql.md)   
 [sp_syscollector_delete_collection_item &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-delete-collection-item-transact-sql.md)   
 [syscollector_collector_types &#40;Transact-sql&#41;](../../relational-databases/system-catalog-views/syscollector-collector-types-transact-sql.md)   
 [sp_syscollector_create_collection_set &#40;Transact-sql&#41;](../../relational-databases/system-stored-procedures/sp-syscollector-create-collection-set-transact-sql.md)   
 [syscollector_collection_items (Transact-SQL)](../../relational-databases/system-catalog-views/syscollector-collection-items-transact-sql.md)  
  
  
