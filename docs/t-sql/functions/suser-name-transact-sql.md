---
description: SUSER_NAME (Transact-SQL)
title: SUSER_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 12/12/2020
ms.prod: sql
ms.prod_service: sql-data-warehouse, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SUSER_NAME
- SUSER_NAME_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- security identification names [SQL Server]
- logins [SQL Server], users
- identification names for logins [SQL Server]
- users [SQL Server], logins
- SUSER_NAME function
- logins [SQL Server], names
- names [SQL Server], logins
ms.assetid: ae598d9f-9baa-49b8-b1c1-042854206de4
author: VanMSFT
ms.author: vanto
monikerRange: =azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 880d2405053d4dad832e0efa7d8c4c5c9d7395f3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97484099"
---
# <a name="suser_name-transact-sql"></a>SUSER_NAME (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

返回用户的登录标识名。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SUSER_NAME ( [ server_user_id ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
_server\_user\_id_  
用户的登录标识号。 可选参数 _server\_user\_id_ 的数据类型为 **int**。_server\_user\_id_ 可以是有权连接到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的任何 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户或用户组的登录标识号。 如果未指定 _server\_user\_id_，则返回当前用户的登录标识名。 如果参数包含 NULL 一词，它将返回 NULL。  
  
## <a name="return-types"></a>返回类型  
**nvarchar(128)**  
  
## <a name="remarks"></a>注解  
在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 7.0 中，安全标识号 (SID) 取代了服务器用户标识号 (SUID)。  
  
SUSER_NAME 只返回在 syslogins 系统表中有条目的登录的登录名。  
  
SUSER_NAME 可用于选择列表、WHERE 子句和任何允许使用表达式的地方。 即使未指定任何参数，也请在 SUSER_NAME 后使用括号。  

> [!NOTE]
> 尽管 Azure SQL 数据库支持 SUSER_NAME，但 Azure SQL 数据库不支持使用 SUSER_NAME 的 Execute as。 
  
## <a name="examples"></a>示例  
以下示例将返回登录标识号为 `1` 的用户的登录标识名。  
  
```sql
SELECT SUSER_NAME(1);  
```  
  
## <a name="see-also"></a>另请参阅  
[SUSER_ID (Transact-SQL)](../../t-sql/functions/suser-id-transact-sql.md)   
[主体（数据库引擎）](../../relational-databases/security/authentication-access/principals-database-engine.md)  
  
  
