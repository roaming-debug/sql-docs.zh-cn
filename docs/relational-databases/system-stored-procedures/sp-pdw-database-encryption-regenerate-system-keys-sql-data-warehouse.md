---
title: sp_pdw_database_encryption_regenerate_system_keys (Azure Synapse Analytics) |Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.technology: system-objects
ms.topic: language-reference
dev_langs:
- TSQL
ms.assetid: bb13e323-a984-4462-8b6d-6019c38ddd9d
author: ronortloff
ms.author: rortloff
ms.reviewer: ''
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 89be1645a93d10e09b43dd3c32fc1fee72785791
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97410503"
---
# <a name="sp_pdw_database_encryption_regenerate_system_keys-azure-synapse-analytics"></a> (Azure Synapse Analytics sp_pdw_database_encryption_regenerate_system_keys) 

[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  使用 **sp_pdw_database_encryption_regenerate_system_keys** 轮换在设备上启用了 TDE 时加密的内部数据库的证书和数据库加密密钥。 这包括 `tempdb`。 仅当启用了 TDE 时，此功能才会成功。  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Syntax for Azure Synapse Analytics and Parallel Data Warehouse  
  
sp_pdw_database_encryption_regenerate_system_keys  ;  
```  

[!INCLUDE[synapse-analytics-od-unsupported-syntax](../../includes/synapse-analytics-od-unsupported-syntax.md)]

## <a name="return-code-values"></a>返回代码值  
 **0** (成功) 或 **1** (失败)   
  
## <a name="remarks"></a>备注  
 此过程没有参数。  
  
 当设备中的流量较低时，应使用此过程。  
  
## <a name="permissions"></a>权限  
 要求具有 **sysadmin** 固定数据库角色的成员身份或 **CONTROL SERVER** 权限。  
  
## <a name="example"></a>示例  
 下面的示例重新生成数据库加密密钥。  
  
```sql  
EXEC sys.sp_pdw_database_encryption_regenerate_system_keys;  
```  
  
## <a name="see-also"></a>请参阅  
 [&#40;Azure Synapse Analytics sp_pdw_database_encryption&#41;](../../relational-databases/system-stored-procedures/sp-pdw-database-encryption-sql-data-warehouse.md)   
 [&#40;Azure Synapse Analytics sp_pdw_log_user_data_masking&#41;](../../relational-databases/system-stored-procedures/sp-pdw-log-user-data-masking-sql-data-warehouse.md)  
  
