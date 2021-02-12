---
title: 导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据
description: 在 SQL Server 2019 中，可结合使用 -V 开关和限定符，然后使用 bcp 来导入来自其他版本的 SQL Server 的本机格式数据和字符格式数据。
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- earlier versions [SQL Server], import and export data formats
- -V switch
- data formats [SQL Server], earlier versions
- previous versions [SQL Server], import and export data formats
ms.assetid: e644696f-9017-428e-a5b3-d445d1c630b3
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.custom: seo-lt-2019
ms.openlocfilehash: fbc7cfeb79b71c08d94e22245b8a63c4466e4bc9
ms.sourcegitcommit: 58e7069b5b2b6367e27b49c002ca854b31b1159d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/04/2021
ms.locfileid: "99552650"
---
# <a name="import-native-and-character-format-data-from-earlier-versions-of-sql-server"></a>导入来自早期版本的 SQL Server 的本机格式数据和字符格式数据
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

在 [!INCLUDE [sssql14-md](../../includes/sssql14-md.md)] 及更高版本中，可以使用 bcp 通过 -V 开关来导入 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]、[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]、[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]、[!INCLUDE[ssKilimanjaro](../../includes/sskilimanjaro-md.md)] 或 [!INCLUDE[ssSQL11](../../includes/sssql11-md.md)] 中的本机和字符格式数据。 **-V** 开关将使 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 使用指定的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]早期版本中的数据类型，并且数据文件格式与早期版本中的格式相同。  
  
若要为数据文件指定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本，可将 **-V** 开关与以下的任一限定符一起使用：  
  
|SQL Server 版本|Qualifier|  
|------------------------|---------------|  
|[!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)]|**-V80**|  
|[!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)]|**-V90**|  
|[!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)]|**-V100**|  
|[!INCLUDE[ssSQL11](../../includes/sssql11-md.md)]|**-V 110**|  
  
## <a name="interpretation-of-data-types"></a>对数据类型的解释  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本均支持一些新的类型。 如果要将新的数据类型导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 早期版本中，则必须以早期的 **bcp** 客户端可读的格式存储该数据类型。 下表总结了如何转换新数据类型以便与早期版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]兼容。  
  
|SQL Server 2005 中的新数据类型|版本 6 *x* 兼容的数据类型|版本 70 中兼容的数据类型|版本 80 中兼容的数据类型|  
|---------------------------------------|-------------------------------------------|-----------------------------------------|-----------------------------------------|  
|**bigint**|**decimal**|**decimal**|*|  
|**sql_variant**|**text**|**nvarchar(4000)**|*|  
|**varchar(max)**|**text**|**text**|**text**|  
|**nvarchar(max)**|**ntext**|**ntext**|**ntext**|  
|**varbinary(max)**|**图像**|**图像**|**图像**|  
|XML|**ntext**|**ntext**|**ntext**|  
|UDT**|**图像**|**图像**|**图像**|  
  
 *此类型受本机支持。  
  
 **UDT 表示用户定义的类型。  
  
## <a name="exporting-using--v-80"></a>使用 -V 80 进行导出  
 使用 -V80 开关批量导出数据时，本机模式下的 nvarchar(max)、varchar(max)、varbinary(max)、XML 和 UDT 数据将像 text、image 和 ntext 数据一样以带有 4 个字节的前缀形式存储，而不是像 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 及更高版本默认的那样以带有 8 个字节的前缀形式存储        。  
  
## <a name="copying-date-values"></a>复制日期值  
 **bcp** 将使用 ODBC 大容量复制 API。 因此，为了将日期值导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]中， **bcp** 使用了 ODBC 日期格式 (*yyyy-mm-dd hh:mm:ss*[ *.f...* ])。  
  
 对于字符格式的数据文件， **bcp** 命令会为 **datetime** 和 **smalldatetime** 类型的值使用 ODBC 默认格式将文件导出。 例如，包含日期 **的** datetime `12 Aug 1998` 列将以字符串 `1998-08-12 00:00:00.000`的形式大容量复制到数据文件中。  
  
> [!IMPORTANT]  
>  在使用 **bcp** 将数据导入到 **smalldatetime** 字段中时，请确保秒数值为 00.000，否则操作将失败。 **smalldatetime** 数据类型仅支持最接近的分钟值。 BULK INSERT 和 INSERT ...SELECT * FROM OPENROWSET(BULK...) 在这种情况下不会失败，但会截断秒值。  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
 **使用数据格式进行大容量导入或大容量导出**  
  
-   [使用字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-native-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 字符格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-character-format-to-import-or-export-data-sql-server.md)  
  
-   [使用 Unicode 本机格式导入或导出数据 (SQL Server)](../../relational-databases/import-export/use-unicode-native-format-to-import-or-export-data-sql-server.md)  
  
## <a name="see-also"></a>另请参阅  
 [bcp 实用工具](../../tools/bcp-utility.md)   
 [BULK INSERT (Transact-SQL)](../../t-sql/statements/bulk-insert-transact-sql.md)   
 [OPENROWSET (Transact-SQL)](../../t-sql/functions/openrowset-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [SQL Server 数据库引擎的向后兼容性](../../database-engine/discontinued-database-engine-functionality-in-sql-server.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)  
  
