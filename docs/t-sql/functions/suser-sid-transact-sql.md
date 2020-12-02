---
description: SUSER_SID (Transact-SQL)
title: SUSER_SID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- SUSER_SID
- SUSER_SID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- logins [SQL Server], users
- SIDs [SQL Server]
- security identifiers [SQL Server]
- users [SQL Server], logins
- 15401 (Database Engine error)
- IDs [SQL Server], logins
- identification numbers [SQL Server], logins
- SUSER_SID function
ms.assetid: 57b42a74-94e1-4326-85f1-701b9de53c7d
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: c3a08123af9155789fa2d14e2deac807061bf162
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91380052"
---
# <a name="suser_sid-transact-sql"></a>SUSER_SID (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回指定登录名的安全标识号 (SID)。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
SUSER_SID ( [ 'login' ] [ , Param2 ] )   
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 **'** *login* **'**  
**适用于**：[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 及更高版本
  
 用户的登录名。 login 为 sysname。 login 作为可选项，可以为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 登录名或 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 用户或组。 如果未指定 login，则返回有关当前安全上下文的信息。 如果此参数包含词 NULL，将返回 NULL。  
  
 Param2  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
 指定是否验证登录名。 Param2 的类型为 int，并且可选。 在 Param2 为 0 时，不验证登录名。 在 Param2 未指定为 0 时，对 Windows 登录名进行验证，以便确认是否与在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中存储的登录名完全相同。  
  
## <a name="return-types"></a>返回类型  
 **varbinary(85)**  
  
## <a name="remarks"></a>注解  
 SUSER_SID 在 ALTER TABLE 或 CREATE TABLE 中可用作 DEFAULT 约束。 SUSER_SID 可以在选择列表、WHERE 子句和任何允许使用表达式的地方使用。 SUSER_SID 必须始终后跟括号，即使在未指定参数的情况下也是如此。  
  
 在无参数的情况下调用时，SUSER_SID 将返回当前安全上下文的 SID。 当通过使用 EXECUTE AS 切换上下文的批处理中无参数调用时，SUSER_SID 将返回模拟上下文的 SID。 从模拟上下文中调用时，SUSER_SID(ORIGINAL_LOGIN()) 将返回原始上下文的 SID。  
  
 当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 排序规则和 Windows 排序规则不同时，如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 Windows 以不同格式存储登录名，SUSER_SID 可能会失败。 例如，如果 Windows 计算机 TestComputer 具有登录名 User，而 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将该登录名存储为 TESTCOMPUTER\User，则查找登录名 TestComputer\User 可能无法正确解析该登录名。 若要跳过此登录名的验证，请使用 Param2。 排序规则不同通常是 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误 15401 的原因：  
  
 `Windows NT user or group '%s' not found. Check the name again.`  
  
## <a name="sssdsfull-remarks"></a>[!INCLUDE[ssSDSfull](../../includes/sssdsfull-md.md)] 注释  
 SUSER_SID 始终返回当前安全上下文的登录 ID。 使用 [sys.database_principals](../../relational-databases/system-catalog-views/sys-database-principals-transact-sql.md) 获取不同登录名的 SID。
  
 SUSER_SID 语句不支持通过 EXECUTE AS 使用模拟安全上下文执行。  

## <a name="examples"></a>示例  
  
### <a name="a-using-suser_sid"></a>A. 使用 SUSER_SID  
 下面的示例返回当前安全上下文的安全标识号 (SID)。  
  
```sql
SELECT SUSER_SID();  
```  
  
### <a name="b-using-suser_sid-with-a-specific-login"></a>B. 将 SUSER_SID 用于特定的登录名  
 下面的示例返回 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] `sa` 登录名的安全标识号。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
```sql
SELECT SUSER_SID('sa');  
GO  
```  
  
### <a name="c-using-suser_sid-with-a-windows-user-name"></a>C. 对 Windows 用户名使用 SUSER_SID  
 以下示例返回 Windows 用户 `London\Workstation1` 的安全标识号。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
```sql
SELECT SUSER_SID('London\Workstation1');  
GO  
```  
  
### <a name="d-using-suser_sid-as-a-default-constraint"></a>D. 将 SUSER_SID 用作 DEFAULT 约束  
 下面的示例在 `SUSER_SID` 语句中使用 `DEFAULT` 作为 `CREATE TABLE` 约束。  
  
```sql
USE AdventureWorks2012;  
GO  
CREATE TABLE sid_example  
(  
login_sid   VARBINARY(85) DEFAULT SUSER_SID(),  
login_name  VARCHAR(30) DEFAULT SYSTEM_USER,  
login_dept  VARCHAR(10) DEFAULT 'SALES',  
login_date  DATETIME DEFAULT GETDATE()  
);   
GO  
INSERT sid_example DEFAULT VALUES;  
GO  
```  
  
### <a name="e-comparing-the-windows-login-name-to-the-login-name-stored-in-sql-server"></a>E. 将 Windows 登录名与在 SQL Server 中存储的登录名进行比较  
 下面的示例演示如何使用 Param2 从 Windows 获取 SID 并使用该 SID 作为对 `SUSER_SNAME` 函数的输入。 该示例以在 Windows 中存储的格式 (`TestComputer\User`) 提供登录名，并且以在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] (`TESTCOMPUTER\User`) 中存储的格式返回登录名。  
  
**适用于**：[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 及更高版本
  
```sql
SELECT SUSER_SNAME(SUSER_SID('TestComputer\User', 0));  
```  
  
## <a name="see-also"></a>另请参阅  
 [ORIGINAL_LOGIN (Transact-SQL)](../../t-sql/functions/original-login-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [binary 和 varbinary (Transact-SQL)](../../t-sql/data-types/binary-and-varbinary-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
