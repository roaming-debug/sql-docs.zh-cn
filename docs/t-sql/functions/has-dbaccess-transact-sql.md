---
description: HAS_DBACCESS (Transact-SQL)
title: HAS_DBACCESS (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 10/23/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, pdw, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HAS_DBACCESS_TSQL
- HAS_DBACCESS
dev_langs:
- TSQL
helpviewer_keywords:
- permissions [SQL Server], verifying
- permissions [SQL Server], user access status
- HAS_DBACCESS
- checking permission status
- verifying permission status
- users [SQL Server], access rights status
- testing permissions
- status information [SQL Server], user access
ms.assetid: 99b43a72-0722-4a7b-a493-bdee1c74c7b9
author: VanMSFT
ms.author: vanto
monikerRange: '>=aps-pdw-2016||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: a89367ad87f1368e2b29aadf5914352d8200cdef
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99159739"
---
# <a name="has_dbaccess-transact-sql"></a>HAS_DBACCESS (Transact-SQL)
[!INCLUDE [sql-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdbmi-asa-pdw.md)]

  返回信息，说明用户是否可以访问指定的数据库。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql  
HAS_DBACCESS ( 'database_name' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 'database_name'  
 数据库的名称，用户希望获取有关该数据库的访问信息。 database_name 的数据类型为 sysname。  
  
## <a name="return-types"></a>返回类型  
 **int**  
  
## <a name="remarks"></a>备注  
 如果用户可以访问该数据库，则 HAS_DBACCESS 返回 1。如果用户不能访问该数据库，则返回 0。如果该数据库名无效，则返回 NULL。  
  
 如果数据库处于脱机或可疑状态，HAS_DBACCESS 将返回 0。  
  
 如果数据库处于单用户模式并且该数据库正由另一个用户使用，HAS_DBACCESS 将返回 0。  
  
## <a name="permissions"></a>权限  
 要求具有 public 角色的成员身份。  
  
## <a name="examples"></a>示例  
 下面的示例测试当前用户是否有权访问 `AdventureWorks2012` 数据库。  
  
```sql  
SELECT HAS_DBACCESS('AdventureWorks2012');  
GO  
```  
  
## <a name="examples-sssdwfull-and-sspdw"></a>示例：[!INCLUDE[ssSDWfull](../../includes/sssdwfull-md.md)] 和 [!INCLUDE[ssPDW](../../includes/sspdw-md.md)]  
 下面的示例测试当前用户是否有权访问 `AdventureWorksPDW2012` 数据库。  
  
```sql  
SELECT HAS_DBACCESS('AdventureWorksPDW2012');  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [IS_MEMBER (Transact-SQL)](../../t-sql/functions/is-member-transact-sql.md)   
 [IS_SRVROLEMEMBER (Transact-SQL)](../../t-sql/functions/is-srvrolemember-transact-sql.md)  
  
  

