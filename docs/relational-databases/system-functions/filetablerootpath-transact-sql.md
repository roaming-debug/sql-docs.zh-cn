---
description: FileTableRootPath (Transact-SQL)
title: FileTableRootPath (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- FileTableRootPath_TSQL
- FileTableRootPath
dev_langs:
- TSQL
helpviewer_keywords:
- FileTableRootPath function
ms.assetid: 0cba908a-c85c-4b09-b16a-df1cb333c629
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: f08273adcf261ecfdafaa70793d50b31360f7e40
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98093852"
---
# <a name="filetablerootpath-transact-sql"></a>FileTableRootPath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回特定 FileTable 或当前数据库的根级 UNC 路径。  
  
## <a name="syntax"></a>语法  
  
```  
  
FileTableRootPath ( [ '[schema_name.]FileTable_name' ], @option )  
```  
  
## <a name="arguments"></a>参数  
 *FileTable_name*  
 FileTable 的名称。 *FileTable_name* 的类型为 **nvarchar**。 这是一个可选参数。 默认值为当前数据库。 指定 *schema_name* 也是可选的。 可以将 NULL 传递给 *FileTable_name* 以使用默认参数值  
  
 *\@选项*  
 一个整数表达式，定义路径的服务器组件应如何进行格式化。 *\@ 选项* 可以具有以下值之一：  
  
|值|描述|  
|-----------|-----------------|  
|**0**|返回转换为 NetBIOS 格式的服务器名称，例如：<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> 这是默认值。|  
|**1**|返回未经转换的服务器名称，例如：<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|返回完整的服务器路径，例如：<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>返回类型  
 **nvarchar(4000)**  
  
 当数据库属于 Always On 可用性组时， **FileTableRootPath** 函数将返回虚拟网络名称 (VNN) ，而不是计算机名。  
  
## <a name="general-remarks"></a>一般备注  
 如果满足以下条件之一， **FileTableRootPath** 函数将返回 NULL：  
  
-   *FileTable_name* 的值无效。  
  
-   调用方没有足够的权限引用指定表或当前数据库。  
  
-   没有为当前数据库设置 *database_directory* 的 FILESTREAM 选项。  
  
 有关详细信息，请参阅 [Work with Directories and Paths in FileTables](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)。  
  
## <a name="best-practices"></a>最佳方案  
 若要使代码和应用程序独立于当前的计算机和数据库，应避免编写依赖于绝对文件路径的代码。 相反，在运行时获取文件的完整路径，方法是将 **FileTableRootPath** 和 **GetFileNamespacePath** 函数一起使用，如下面的示例中所示。 默认情况下， **GetFileNamespacePath** 函数返回数据库根路径下的文件的相对路径。  
  
```sql  
USE MyDocumentDatabase;  
  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
@fullPath = varchar(1000);  
  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath()  
FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="security"></a>安全性  
  
### <a name="permissions"></a>权限  
 **FileTableRootPath** 函数需要：  
  
-   拥有 FileTable 的 SELECT 权限以获取特定 FileTable 的根路径。  
  
-   **db_datareader** 或更高权限以获取当前数据库的根路径。  
  
## <a name="examples"></a>示例  
 下面的示例演示如何调用 **FileTableRootPath** 函数。  
  
```  
USE MyDocumentDatabase;  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase"  
SELECT FileTableRootPath();  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'dbo.MyFileTable');  
  
-- returns "\\MYSERVER\MSSQLSERVER\MyDocumentDatabase\MyFileTable"  
SELECT FileTableRootPath(N'MyFileTable');  
```  
  
## <a name="see-also"></a>另请参阅  
 [在 FileTable 中使用目录和路径](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
