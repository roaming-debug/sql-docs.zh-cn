---
description: SESSION_ID (Transact-SQL)
title: SESSION_ID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 02/23/2018
ms.prod: sql
ms.prod_service: synapse-analytics, pdw
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
dev_langs:
- TSQL
author: VanMSFT
ms.author: vanto
monikerRange: '>= aps-pdw-2016 || = azure-sqldw-latest'
ms.openlocfilehash: 732badac9c998d59b015029d6c917f9f0cbe2574
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104746377"
---
# <a name="session_id-transact-sql"></a>SESSION_ID (Transact-SQL)
[!INCLUDE[applies-to-version/asa-pdw](../../includes/applies-to-version/asa-pdw.md)]

  返回当前 [!INCLUDE[ssSDW](../../includes/sssdw-md.md)] 或 [!INCLUDE[ssPDW_md](../../includes/sspdw-md.md)] 会话的 ID。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定 (Transact-SQL)](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
-- Azure Synapse Analytics and Parallel Data Warehouse  
SESSION_ID ( )  
```  
  
## <a name="return-value"></a>返回值  
 返回 nvarchar(32) 值。  
  
## <a name="general-remarks"></a>一般备注  
 会话 ID 在连接建立时分配给每个用户连接。 它在连接期间都存在。 连接结束时，会话 ID 也会解除。  
  
 会话 ID 以字母字符“SID”开头。 这些字母字符区分大小写，在 [!INCLUDE[DWsql](../../includes/dwsql-md.md)] 命令中使用会话 ID 时必须大写。  
  
 可以查询视图 [sys.dm_pdw_exec_sessions](../../relational-databases/system-dynamic-management-views/sys-dm-pdw-exec-sessions-transact-sql.md) 以检索与此函数相同的信息。  
  
## <a name="examples"></a>示例  
 以下示例返回当前会话 ID。  
  
```sql  
SELECT SESSION_ID();  
```  
  
## <a name="see-also"></a>另请参阅  
 [DB_NAME (Transact-SQL)](../../t-sql/functions/db-name-transact-sql.md)   
 [版本 (Azure Synapse Analytics)](../../t-sql/functions/version-transact-sql-configuration-functions.md)
  
  
