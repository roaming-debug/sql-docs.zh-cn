---
description: GetFileNamespacePath (Transact-SQL)
title: GetFileNamespacePath (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 06/10/2016
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: system-objects
ms.topic: language-reference
f1_keywords:
- GetFileNamespacePath
- GetFileNamespacePath_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- GetFileNamespacePath function
ms.assetid: b393ecef-baa8-4d05-a268-b2f309fce89a
author: WilliamDAssafMSFT
ms.author: wiassaf
ms.openlocfilehash: 9ecda7d44603636ff12eef955dd83e7659cb9dc7
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98097477"
---
# <a name="getfilenamespacepath-transact-sql"></a>GetFileNamespacePath (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  返回 FileTable 中文件或目录的 UNC 路径。  
  
## <a name="syntax"></a>语法  
  
```  
  
<column-name>.GetFileNamespacePath(is_full_path, @option)  
```  
  
## <a name="arguments"></a>参数  
 *列名*  
 VARBINARY (MAX) **file_stream** 列的列名称。  
  
 *列名* 的值必须是有效的列名称。 它不能是表达式，也不能是从其他数据类型的列转换或强制转换的值。  
  
 *is_full_path*  
 整数表达式，指定是返回相对路径还是绝对路径。 *is_full_path* 可以具有以下值之一：  
  
|值|描述|  
|-----------|-----------------|  
|**0**|返回数据库级目录内的相对路径。<br /><br /> 此为默认值。|  
|**1**|返回以 `\\computer_name` 开头的完整 UNC 路径。|  
  
 *\@选项*  
 一个整数表达式，定义路径的服务器组件应如何进行格式化。 *\@ 选项* 可以具有以下值之一：  
  
|值|描述|  
|-----------|-----------------|  
|**0**|返回转换为 NetBIOS 格式的服务器名称，例如：<br /><br /> `\\SERVERNAME\MSSQLSERVER\MyDocumentDatabase`<br /><br /> 这是默认值。|  
|**1**|返回未经转换的服务器名称，例如：<br /><br /> `\\ServerName\MSSQLSERVER\MyDocumentDatabase`|  
|**2**|返回完整的服务器路径，例如：<br /><br /> `\\ServerName.MyDomain.com\MSSQLSERVER\MyDocumentDatabase`|  
  
## <a name="return-type"></a>返回类型  
 **nvarchar(max)**  
  
 如果 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例属于故障转移群集系统，则作为此路径的一部分返回的计算机名称将是群集实例的虚拟主机名。  
  
 当数据库属于 Always On 可用性组时， **FileTableRootPath** 函数将返回虚拟网络名称 (VNN) ，而不是计算机名。  
  
## <a name="general-remarks"></a>一般备注  
 **GetFileNamespacePath** 函数返回的路径是采用以下格式的逻辑目录或文件路径：  
  
 `\\<machine>\<instance-level FILESTREAM share>\<database-level directory>\<FileTable directory>\...`  
  
 此逻辑路径不直接对应于某一物理 NTFS 路径。 它通过 FILESTREAM 的文件系统筛选器驱动程序和 FILESTREAM 代理转换为物理路径。 逻辑路径和物理路径之间的这一区别使 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 可以在内部重新组织数据并且不会影响路径的有效性。  
  
## <a name="best-practices"></a>最佳方案  
 若要使代码和应用程序独立于当前的计算机和数据库，应避免编写依赖于绝对文件路径的代码。 相反，在运行时获取文件的完整路径，方法是将 **FileTableRootPath** 和 **GetFileNamespacePath** 函数一起使用，如下面的示例中所示。 默认情况下， **GetFileNamespacePath** 函数返回数据库根路径下的文件的相对路径。  
  
```sql  
USE MyDocumentDatabase;  
@root varchar(100)  
SELECT @root = FileTableRootPath();  
  
@fullPath = varchar(1000);  
SELECT @fullPath = @root + file_stream.GetFileNamespacePath() FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="remarks"></a>备注  
  
## <a name="examples"></a>示例  
 下面的示例演示如何调用 **GetFileNamespacePath** 函数以获取 FileTable 中文件或目录的 UNC 路径。  
  
```  
-- returns the relative path of the form "\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath() AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
  
-- returns "\\MyServer\MSSQLSERVER\MyDocumentDatabase\MyFileTable\MyDocDirectory\document.docx"  
SELECT file_stream.GetFileNamespacePath(1, Null) AS FilePath FROM DocumentStore  
WHERE Name = N'document.docx';  
```  
  
## <a name="see-also"></a>另请参阅  
 [在 FileTable 中使用目录和路径](../../relational-databases/blob/work-with-directories-and-paths-in-filetables.md)  
  
  
