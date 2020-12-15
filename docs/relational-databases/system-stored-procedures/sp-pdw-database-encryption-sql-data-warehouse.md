---
description: " (Azure Synapse Analytics sp_pdw_database_encryption) "
title: sp_pdw_database_encryption (Azure Synapse Analytics) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.service: sql-data-warehouse
ms.reviewer: ''
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: f5ccb424-7a95-4557-b774-c69de33c1545
author: ronortloff
ms.author: rortloff
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 73d1eb9fe27fa060a8bfcd13341a3908545aeed3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468338"
---
# <a name="sp_pdw_database_encryption-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_pdw_database_encryption) 
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  使用 **sp_pdw_database_encryption** 在设备上启用透明数据加密 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 。 如果 **sp_pdw_database_encryption** 设置为1，请使用 **ALTER database** 语句，通过使用 TDE 对数据库进行加密。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption [ [ @enabled = ] enabled ] ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

#### <a name="parameters"></a>parameters  
`[ @enabled = ] enabled` 确定是否已启用透明数据加密。 *enabled* 为 **int**，可以为以下值之一：  
  
-   0 - 禁用  
  
-   1 = 启用  
  
 如果执行不带参数的 **sp_pdw_database_encryption** ，则将在设备上将 TDE 的当前状态返回为标量结果集：0表示禁用，或1表示启用。  
  
## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 使用 **sp_pdw_database_encryption** 启用 TDE 时，将删除并重新创建和加密 tempdb 数据库。 出于此原因，在有其他活动会话使用 tempdb 时，无法在设备上启用 TDE。 在设备上启用或禁用 TDE 是一项操作，该操作将更改设备的状态，在大多数情况下，应在设备生存期内执行一次，并在设备上没有流量时执行。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定数据库角色的成员身份或 **CONTROL SERVER** 权限。  
  
## <a name="example"></a>示例  
 以下示例在设备上启用 TDE。  
  
```sql  
EXEC sys.sp_pdw_database_encryption 1;  
```  
  
## <a name="see-also"></a>请参阅  
 [&#40;Azure Synapse Analytics sp_pdw_database_encryption_regenerate_system_keys&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-regenerate-system-keys-sql-data-warehouse.md)   
 [&#40;Azure Synapse Analytics sp_pdw_log_user_data_masking&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
