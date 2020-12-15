---
description: OPENDATASOURCE (Transact-SQL)
title: OPENDATASOURCE (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/26/2019
ms.prod: sql
ms.prod_service: sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- OPENDATASOURCE
- OPENDATASOURCE_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- data sources [SQL Server]
- OPENDATASOURCE function
- remote data access [SQL Server], OPENDATASOURCE
- ad hoc distributed queries
- OLE DB data sources [SQL Server]
- ad hoc connection information
ms.assetid: 5510b846-9cde-4687-8798-be9a273aad31
author: julieMSFT
ms.author: jrasnick
monikerRange: =azuresqldb-mi-current||>=sql-server-2016||>=sql-server-linux-2017
ms.openlocfilehash: 2e1245ccb29d0fd7982c62ff22ba7776b0bac839
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402064"
---
# <a name="opendatasource-transact-sql"></a>OPENDATASOURCE (Transact-SQL)
[!INCLUDE [SQL Server - ASDBMI](../../includes/applies-to-version/sql-asdbmi.md)]

  在四部分对象名称中提供连接信息，而不使用链接服务器名称。  

 ![链接图标](../../database-engine/configure-windows/media/topic-link.gif "“链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
OPENDATASOURCE ( 'provider_name', 'init_string' )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="arguments"></a>参数
 '*provider_name*'  
 注册为用于访问数据源的 OLE DB 访问接口的 PROGID 的名称。 provider_name 的数据类型为 char，并且没有默认值。  

 > [!IMPORTANT]
 > 以前的 Microsoft OLE DB Provider for SQL Server (SQLOLEDB) 和 SQL Server Native Client OLEDB 提供程序 (SQLNCLI) 仍然不推荐使用，不建议在新的开发工作中使用它们。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。
 
 '*init_string*'  
 连接字符串，该字符串将要传递给目标提供程序的 IDataInitialize 接口。 提供程序字符串语法是以关键字值对为基础的，这些关键字值对由分号隔开，例如：“keyword1=value；keyword2=value”  。  
  
 若要了解提供程序上支持的特定关键字值对，请参阅 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Data Access SDK。 该文档定义了基本语法。 下表列出了 init_string 参数中最常用的关键字。  
  
|关键字|OLE DB 属性|有效值和说明|  
|-------------|---------------------|----------------------------------|  
|数据源|DBPROP_INIT_DATASOURCE|要连接的数据源的名称。 不同的提供程序用不同的方法对此进行解释。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 访问接口，这指示服务器的名称。 对于 Jet OLE DB 访问接口来说，这指示 .mdb 文件或 .xls 文件的完整路径。|  
|位置|DBPROP_INIT_LOCATION|要连接的数据库的位置。|  
|扩展属性|DBPROP_INIT_PROVIDERSTRING|提供程序特定的连接字符串。|  
|连接超时|DBPROP_INIT_TIMEOUT|达到此超时值后，连接尝试就会失败。|  
|用户 ID|DBPROP_AUTH_USERID|用于该连接的用户 ID。|  
|密码|DBPROP_AUTH_PASSWORD|用于该连接的密码。|  
|目录|DBPROP_INIT_CATALOG|连接到数据源时的初始或默认的目录名称。|  
|集成安全性|DBPROP_AUTH_INTEGRATED|SSPI，指定 Windows 身份验证|  
  
## <a name="remarks"></a>注解  
`OPENROWSET` 始终继承实例排序规则，而不考虑列的排序规则集。

仅当 DisallowAdhocAccess 注册表选项针对指定的提供程序显式设置为 0，并且启用 Ad Hoc Distributed Queries 高级配置选项时，`OPENDATASOURCE` 才可用于访问 OLE DB 数据源中的远程数据。 如果未设置这些选项，则默认行为不允许即席访问。  
  
`OPENDATASOURCE` 函数可以在能够使用链接服务器名称的相同 [!INCLUDE[tsql](../../includes/tsql-md.md)] 语法位置中使用。 因此，可以将 `OPENDATASOURCE` 用作四部分名称的第一部分，该部分名称引用 SELECT、INSERT、UPDATE 或 DELETE 语句中的表或视图的名称；或者引用 EXECUTE 语句中的远程存储过程。 当执行远程存储过程时，`OPENDATASOURCE` 应该引用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的另一个实例。 OPENDATASOURCE 不接受参数变量。  
  
与 `OPENROWSET` 函数类似，`OPENDATASOURCE` 应该只引用那些不经常访问的 OLE DB 数据源。 对于访问次数较频繁的任何数据源，请为它们定义链接服务器。 无论 OPENDATASOURCE，还是 OPENROWSET，都不能提供链接服务器定义的全部功能，如安全管理以及目录信息查询功能。 每次调用 OPENDATASOURCE 时，都必须提供所有的连接信息（包括密码）。  
  
> [!IMPORTANT]  
> Windows 身份验证比 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 身份验证要安全得多。 应尽量使用 Windows 身份验证。 `OPENDATASOURCE` 不应该用于连接字符串中的显式密码。  
  
每个提供程序的连接要求与创建链接服务器时的参数要求相似。 [sp_addlinkedserver &#40;Transact-SQL&#41;](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md) 一文中列出了很多常见提供程序的详细信息。  
  
`FROM` 子句中对 `OPENDATASOURCE`、`OPENQUERY` 或 `OPENROWSET` 的任何调用与对用作更新目标的这些函数的任何调用都是分开独立计算的，即使为两个调用提供的参数相同也是如此。 具体而言，应用到上述任一调用的结果的筛选器或联接条件不会影响其他调用的结果。  
  
## <a name="permissions"></a>权限  
 任何用户都可以执行 OPENDATASOURCE。 用于连接到远程服务器的权限由连接字符串确定。  
  
## <a name="examples"></a>示例  

### <a name="a-using-opendatasource-with-select-and-the-sql-server-ole-db-driver"></a>A. 将 OPENDATASOURCE 与 SELECT 和 SQL Server OLE DB 驱动程序配合使用  
 以下示例使用 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) 访问远程服务器 `Seattle1` 上 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 数据库中的 `HumanResources.Department` 表。 使用 `SELECT` 语句定义返回的行集。 访问接口字符串包含 `Server` 和 `Trusted_Connection` 关键字。 这些关键字由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] OLE DB 驱动程序识别。  
  
```sql  
SELECT GroupName, Name, DepartmentID  
FROM OPENDATASOURCE('MSOLEDBSQL', 'Server=Seattle1;Database=AdventureWorks2016;TrustServerCertificate=Yes;Trusted_Connection=Yes;').HumanResources.Department  
ORDER BY GroupName, Name;  
``` 

### <a name="b-using-opendatasource-with-select-and-the-sql-server-ole-db-provider"></a>B. 将 OPENDATASOURCE 与 SELECT 和 SQL Server OLE DB 提供程序配合使用  
以下示例将创建与服务器 `Payroll` 上的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例 `London` 的即席连接，并查询 `AdventureWorks2012.HumanResources.Employee` 表。 

> [!NOTE] 
> 使用 SQLNCLI 会将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 重定向至 SQL Server Native Client OLE DB 提供程序的最新版本。 OLE DB 提供程序应以指定的 PROGID 在注册表中注册。 

> [!IMPORTANT]  
> SQL Server Native Client OLE DB 提供程序 (SQLNCLI) 仍然不推荐使用，不建议在新的开发工作中使用它。 相反，请使用新的 [Microsoft OLE DB Driver for SQL Server](../../connect/oledb/oledb-driver-for-sql-server.md) (MSOLEDBSQL)，其将使用最新的服务器功能进行更新。
  
```sql  
SELECT *  
FROM OPENDATASOURCE('SQLNCLI',  
    'Data Source=London\Payroll;Integrated Security=SSPI')  
    .AdventureWorks2012.HumanResources.Employee;  
```  

### <a name="c-using-the-microsoft-ole-db-provider-for-jet"></a>C. 使用 Microsoft OLE DB Provider for Jet   
 以下示例以 1997 - 2003 格式创建与 Excel 电子表格的即席连接。  
  
```sql  
SELECT * FROM OPENDATASOURCE('Microsoft.Jet.OLEDB.4.0',  
    'Data Source=C:\DataFolder\Documents\TestExcel.xls;Extended Properties=EXCEL 5.0')...[Sheet1$] ;  
```  
  
## <a name="see-also"></a>另请参阅  
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [sp_addlinkedserver (Transact-SQL)](../../relational-databases/system-stored-procedures/sp-addlinkedserver-transact-sql.md)  
  
