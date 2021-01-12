---
description: PathName (Transact-SQL)
title: PathName (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/02/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- PathName_TSQL
- PathName
dev_langs:
- TSQL
helpviewer_keywords:
- PathName FILESTREAM [SQL Server]
ms.assetid: 6b95ad90-6c82-4a23-9294-a2adb74934a3
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 85058a3d551a385e4d2de8aed2ea9f56dc3b19c7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093881"
---
# <a name="pathname-transact-sql"></a>PathName (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 FILESTREAM 二进制大型对象 (BLOB) 路径。 OpenSqlFilestream API 使用此路径返回应用程序可用于通过使用 Win32 Api 处理 BLOB 数据的句柄。 PathName 是只读的。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```  
  
column_name.PathName ( @option [ , use_replica_computer_name ] )  
```  
  
## <a name="arguments"></a>参数  
 column_name  
 **Varbinary (max)** FILESTREAM 列的列名称。 *column_name* 必须是列名称。 它不能是表达式，也不能是 CAST 或 CONVERT 语句的结果。  
  
 如果请求任何其他数据类型的列的路径名或对于 **varbinary (max)** columnthat，则不会有 FILESTREAM 存储属性导致查询编译时错误。  
  
 *\@选项*  
 一个整数 [表达式](../../t-sql/language-elements/expressions-transact-sql.md) ，定义路径的服务器组件应如何进行格式化。 *\@ 选项* 可以为下列值之一。 默认值为 0。  
  
|值|描述|  
|-----------|-----------------|  
|0|返回转换为 BIOS 格式的服务器名称，例如：`\\SERVERNAME\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
|1|返回未经转换的服务器名称，例如：`\\ServerName\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F1`|  
|2|返回完整的服务器路径，例如：`\\ServerName.MyDomain.com\MSSQLSERVER\v1\Archive\dbo\Records\Chart\A73F19F7-38EA-4AB0-BB89-E6C545DBD3F9`|  
  
 *use_replica_computer_name*  
 一个位值，用于定义在 Always On 可用性组中应如何返回服务器名称。  
  
 如果数据库不属于 Always On 可用性组，则忽略此参数的值。 在路径中始终使用计算机名称。  
  
 当数据库属于 Always On 可用性组时， *use_replica_computer_name* 的值对 **PathName** 函数的输出具有以下影响：  
  
|值|描述|  
|-----------|-----------------|  
|未指定。|函数返回路径中的虚拟网络名称 (VNN)。|  
|0|函数返回路径中的虚拟网络名称 (VNN)。|  
|1|函数返回路径中的计算机名称。|  
  
## <a name="return-type"></a>返回类型  
 **nvarchar(max)**  
  
## <a name="return-value"></a>返回值  
 返回的值是 BLOB 的完全限定逻辑路径或 NETBIOS 路径。 PathName 不返回 IP 地址。 尚未创建 FILESTREAM BLOB 时返回 NULL。  
  
## <a name="remarks"></a>备注  
 ROWGUID 列必须在任何调用 PathName 的查询中可见。  
  
 只能使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] 来创建 FILESTREAM BLOB。  
  
## <a name="examples"></a>示例  
  
### <a name="a-reading-the-path-for-a-filestream-blob"></a>A. 读取 FILESTREAM BLOB 的路径  
 下例将 `PathName` 赋给一个 `nvarchar(max)` 类型的变量。  
  
```sql  
DECLARE @PathName nvarchar(max);  
SET @PathName = (  
    SELECT TOP 1 photo.PathName()  
    FROM dbo.Customer  
    WHERE LastName = 'CustomerName'  
    );  
```  
  
### <a name="b-displaying-the-paths-for-filestream-blobs-in-a-table"></a>B. 在表中显示 FILESTREAM BLOB 的路径  
 下面的示例创建并显示三个 FILESTREAM BLOB 的路径。  
  
```sql  
-- Create a FILESTREAM-enabled database.  
-- The c:\data directory must exist.  
CREATE DATABASE PathNameDB  
ON  
PRIMARY ( NAME = ArchX1,  
    FILENAME = 'c:\data\archdatP1.mdf'),  
FILEGROUP FileStreamGroup1 CONTAINS FILESTREAM( NAME = ArchX3,  
    FILENAME = 'c:\data\filestreamP1')  
LOG ON  ( NAME = ArchlogX1,  
    FILENAME = 'c:\data\archlogP1.ldf');  
GO  
  
USE PathNameDB;  
GO  
  
-- Create a table, add some records, and  
-- create the associated FILESTREAM  
-- BLOB files.  
  
CREATE TABLE TABLE1  
    (  
        ID int,  
        RowGuidColumn UNIQUEIDENTIFIER  
                      NOT NULL UNIQUE ROWGUIDCOL,  
        FILESTREAMColumn varbinary(MAX) FILESTREAM  
    );  
GO  
  
INSERT INTO TABLE1 VALUES  
 (1, NEWID(), 0x00)  
,(2, NEWID(), 0x00)  
,(3, NEWID(), 0x00);  
GO  
  
SELECT FILESTREAMColumn.PathName() AS 'PathName' FROM TABLE1;  
  
--Results  
--PathName  
------------------------------------------------------------------------------------------------------------  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\DD67C792-916E-4A76-8C8A-4A85DC5DB908  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\2907122B-2560-4CB9-86DC-FBE7ABA1843B  
--\\SERVER\MSSQLSERVER\v1\PathNameExampleDB\dbo\TABLE1\FILESTREAMColumn\922BE0E0-CAB9-4403-90BF-945BD258E4BC  
--  
--(3 row(s) affected)  
GO  
  
--Drop the database to clean up.  
USE master;  
GO  
DROP DATABASE PathNameDB;  
```  
  
## <a name="see-also"></a>另请参阅  
 [二进制大型对象 &#40;Blob&#41; 数据 &#40;SQL Server&#41;](../../relational-databases/blob/binary-large-object-blob-data-sql-server.md)   
 [GET_FILESTREAM_TRANSACTION_CONTEXT &#40;Transact-sql&#41;](../../t-sql/functions/get-filestream-transaction-context-transact-sql.md)   
 [使用 OpenSqlFilestream 访问 FILESTREAM 数据](../../relational-databases/blob/access-filestream-data-with-opensqlfilestream.md)  
  
  
