---
description: sys.fn_cdc_has_column_changed (Transact-SQL)
title: sys.fn_cdc_has_column_changed (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- sys.fn_cdc_has_column_changed_TSQL
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed_TSQL
- fn_cdc_has_column_changed
dev_langs:
- TSQL
helpviewer_keywords:
- sys.fn_cdc_has_column_changed
- fn_cdc_has_column_changed
ms.assetid: 2b9e6278-050d-4ffc-8d1a-09606180facc
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 8e835647f387b4980a50a0f49e5c6bd967fe553f
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099656"
---
# <a name="sysfn_cdc_has_column_changed-transact-sql"></a>sys.fn_cdc_has_column_changed (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  标识指定的更新掩码是否指示已更新关联的更改行中的指定列。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
sys.fn_cdc_has_column_changed ( 'capture_instance','column_name' , update_mask )  
```  
  
## <a name="arguments"></a>参数  
 **"** *capture_instance* **"**  
 捕获实例的名称。 *capture_instance* **sysname**。  
  
 **"** *column_name* **"**  
 要报告的指定捕获实例中的已捕获列。 *column_name* **sysname**。  
  
 *update_mask*  
 用于标识任何关联更改行中的更新列的掩码。 *update_mask* 为 **varbinary (128)**。  
  
## <a name="return-type"></a>返回类型  
 **bit**  
  
## <a name="remarks"></a>备注  
 可使用此函数从更改数据查询中返回的更新掩码提取信息。 如果需要知道是否已修改关联更改行中的特定列，则在对更新掩码进行后期处理时该函数最为有用。 有关详细信息，请参阅[关于变更数据捕获 (SQL Server)](../../relational-databases/track-changes/about-change-data-capture-sql-server.md)。  
  
 如果将此信息作为更改数据查询的一部分返回，我们建议使用 [sys.fn_cdc_get_column_ordinal](../../relational-databases/system-functions/sys-fn-cdc-get-column-ordinal-transact-sql.md) 和 [sys.fn_cdc_is_bit_set](../../relational-databases/system-functions/sys-fn-cdc-is-bit-set-transact-sql.md) 函数，而不是此函数。 在查询更改数据之前，请使用函数 fn_cdc_get_column_ordinal，以便所需的列序号只计算一次。 在查询中使用 fn_cdc_is_bit_set 可以从每个返回行的更新掩码中提取信息。  
  
## <a name="permissions"></a>权限  
 要求具有 sysadmin 固定服务器角色或 db_owner 固定数据库角色的成员身份。 对于所有其他用户，要求对源表中的所有已捕获列具有 SELECT 权限；如果已定义捕获实例的访问控制角色，则还要求具有该数据库角色的成员身份。  
  
## <a name="see-also"></a>另请参阅  
 [&#60;capture_instance&#62;_CT &#40;Transact-sql&#41;](../../relational-databases/system-tables/cdc-capture-instance-ct-transact-sql.md)   
 [cdc.captured_columns &#40;Transact-sql&#41;](../../relational-databases/system-tables/cdc-captured-columns-transact-sql.md)  
  
  
