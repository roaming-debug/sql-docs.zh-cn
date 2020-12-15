---
description: " (Azure Synapse Analytics sp_pdw_log_user_data_masking) "
title: sp_pdw_log_user_data_masking (Azure Synapse Analytics) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: 43c63b42-03cb-4fb5-8362-ec3b7e22a590
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 2d2b3de8cf86e7597c944b827326dd070bc2ffce
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97461548"
---
# <a name="sp_pdw_log_user_data_masking-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_pdw_log_user_data_masking) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  使用 **sp_pdw_log_user_data_masking** 在活动日志中启用用户数据屏蔽 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 用户数据掩码影响设备上所有数据库上的语句。  
  
> [!IMPORTANT]  
>  [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]受 **sp_pdw_log_user_data_masking** 影响的活动日志是某些 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志。 **sp_pdw_log_user_data_masking** 不影响数据库事务日志或 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志。  
  
 **背景：** 默认配置 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志包含完整的 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句，并且在某些情况下，可以包括 **INSERT**、 **UPDATE** 和 **SELECT** 语句等操作中包含的用户数据。 在设备上出现问题时，这允许分析导致问题的条件，而无需再现问题。 为了防止用户数据写入 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志，客户可以选择使用此存储过程打开用户数据掩码。 语句仍将写入 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志，但包含用户数据的语句中的所有文本都将被屏蔽; 已替换为一些预定义的常量值。  
  
 在设备上启用透明数据加密时， [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 会自动打开活动日志中的用户数据屏蔽。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_log_user_data_masking [ [ @masking_mode = ] value ] ;  
```

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>parameters  
`[ @masking_mode = ] masking_mode` 确定是否已启用透明数据加密日志用户数据掩码。 *masking_mode* 为 **int**，可以是下列值之一：  
  
-   0 = 禁用，用户数据显示在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志中。  
  
-   1 = 已启用，用户数据语句显示在 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志中，但用户数据被屏蔽。  
  
-   2 = 包含用户数据的语句不写入 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 活动日志。  
  
 如果不使用参数执行 **sp_pdw_ log_user_data_masking** ，则将在设备上以标量结果集的形式返回 TDE 日志用户数据掩码的当前状态。  
  
## <a name="remarks"></a>备注  
 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)]通过活动日志中的用户数据掩码，可以在 **SELECT** 和 DML 语句中替换包含预定义常量值的文本，因为它们可以包含用户数据。 将 *masking_mode* 设置为1不会掩盖元数据（如列名称或表名称）。 将 *masking_mode* 设置为2将删除带有元数据的语句，如列名称或表名称。  
  
 活动日志中的用户数据掩码 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 按以下方式实现：  
  
-   默认情况下，活动日志中的 TDE 和用户数据掩码处于 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 关闭状态。 如果未在设备上启用数据库加密，则不会自动屏蔽这些语句。  
  
-   在设备上启用 TDE 会自动启用活动日志中的用户数据屏蔽 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
-   禁用 TDE 不会影响活动日志中的用户数据屏蔽 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。  
  
-   您可以 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 使用 **sp_pdw_log_user_data_masking** 过程在活动日志中显式启用用户数据屏蔽。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定数据库角色的成员身份或 **CONTROL SERVER** 权限。  
  
## <a name="example"></a>示例  
 以下示例在设备上启用 TDE 日志用户数据屏蔽。  
  
```sql  
EXEC sp_pdw_log_user_data_masking 1;  
```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;Azure Synapse Analytics sp_pdw_database_encryption&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [&#40;Azure Synapse Analytics sp_pdw_database_encryption_regenerate_system_keys&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)  
  
  
