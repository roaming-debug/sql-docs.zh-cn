---
description: 配置和管理搜索筛选器
title: 配置和管理搜索筛选器 | Microsoft Docs
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: search, sql-database
ms.technology: search
ms.topic: conceptual
helpviewer_keywords:
- full-text search [SQL Server], filters
- filters [full-text search]
ms.assetid: 7ccf2ee0-9854-4253-8cca-1faed43b7095
author: pmasl
ms.author: pelopes
ms.reviewer: mikeray
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 3cb9d2938bddb44f22f4429daf0de0705ea53b65
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97460120"
---
# <a name="configure-and-manage-filters-for-search"></a>配置和管理搜索筛选器
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  对 varbinary、varbinary(max)、image 或 xml 数据类型列中的文档进行索引时，需要额外的处理工作。 该处理必须由筛选器执行。 筛选器从文档中提取文本信息（去除格式）。 然后，筛选器将这些文本发送至与表列相关联的语言的断字器组件。  
 
## <a name="filters-and-document-types"></a>筛选器和文档类型
给定筛选器特定于给定文档类型（.doc、.pdf、.xls、.xml 等等）。 这些筛选器实现 IFilter 接口。 有关这些文档类型的详细信息，请查询 [sys.fulltext_document_types](../../relational-databases/system-catalog-views/sys-fulltext-document-types-transact-sql.md) 目录视图。  
  
二进制文档可以存储在单个 **varbinary(max)** 或 **image** 列中。 对于每个文档， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 根据文件扩展名来选择正确的筛选器。 由于当文件存储在 **varbinary(max)** 或 **image** 列中时其文件扩展名不可见，因此文件扩展名（.doc、.xls、.pdf 等）必须存储在表内一个单独的列中，该列称为类型列。 此类型列可以是任意基于字符的数据类型，并且包含文档文件扩展名，例如 .doc 表示 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Word 文档。 在 **的** Document [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)]表中， **Document** 列的类型为 **varbinary(max)**，而类型列 **FileExtension** 的类型为 **nvarchar(8)**。  

**查看现有全文索引中的类型列**  
  
-   [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)  
  
> [!NOTE]  
>  筛选器有可能能够处理嵌入到父对象中的对象，具体取决于筛选器的实现方式。 不过， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会将筛选器配置为跟踪指向其他对象的链接。  

## <a name="installed-filters"></a>已安装的筛选器 
[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装它自己的 XML 和 HTML 筛选器。 此外，已在操作系统上安装的任何针对 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 专有格式（.doc、.xdoc、.ppt 等）的筛选器也由 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 加载。 若要标识当前加载到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的实例上的筛选器，请使用 [sp_help_fulltext_system_components](../../relational-databases/system-stored-procedures/sp-help-fulltext-system-components-transact-sql.md) 存储过程，如下所示：  

```sql
EXEC sp_help_fulltext_system_components 'filter';   
```  

> [!NOTE]
> 即使使用提供 .xlsx 支持的 Office 筛选包，SQL Server 也不支持 Strict Open XML 电子表格。  不返回任何错误，SQL Server 只是无法对任何 Strict Open XML 电子表格编制索引。

## <a name="non-microsoft-filters"></a>非 Microsoft 筛选器
但是，在可以使用针对非 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 格式的筛选器之前，必须将它们手动加载到服务器实例中。 有关安装其他筛选器的信息，请参阅 [查看或更改注册的筛选器和断字符](../../relational-databases/search/view-or-change-registered-filters-and-word-breakers.md)。  
  
  
## <a name="see-also"></a>另请参阅  
 [sys.fulltext_index_columns (Transact-SQL)](../../relational-databases/system-catalog-views/sys-fulltext-index-columns-transact-sql.md)   
 [FILESTREAM 与其他 SQL Server 功能的兼容性](../../relational-databases/blob/filestream-compatibility-with-other-sql-server-features.md)  
  
  
