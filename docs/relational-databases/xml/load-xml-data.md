---
title: 加载 XML 数据 | Microsoft Docs
description: 了解将 XML 数据传输到 SQL Server 数据库中的几种方法。
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- XML data [SQL Server], loading
- loading XML data
ms.assetid: d1741e8d-f44e-49ec-9f14-10208b5468a7
author: MightyPen
ms.author: genemi
ms.openlocfilehash: 19a0518477c37b9f45f22cdf6b5ded1464cf145e
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250666"
---
# <a name="load-xml-data"></a>加载 XML 数据
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  您可以采用多种方式将 XML 数据传输到 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 中。 例如：  
  
-   如果您的数据位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的 [n]text 或 image 列中，则可以使用 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)]导入表。 使用 ALTER TABLE 语句将列类型更改为 XML。  
  
-   您可以使用 bcp out 将数据从其他 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中大容量复制出来，然后再使用 bcp in 将数据大容量插入到更高版本的数据库中。  
  
-   如果您的数据在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库的关系列中，则可以创建一个包含 [n]text 列的表，也可选择包含一个主键列来放置行标识符。 使用客户端编程检索在服务器中使用 FOR XML 生成的 XML，并将其写入 **[n]text** 列。 然后，使用上面提到的方法将数据传输到更高版本的数据库中。 您可以选择将 XML 直接写入更高版本数据库中的 XML 列。  
  
## <a name="bulk-loading-xml-data"></a>大容量加载 XML 数据  
 可以通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]的大容量加载功能（如 bcp）将 XML 数据大容量加载到服务器中。 通过使用 OPENROWSET 可以将文件中的数据加载到 XML 列中。 以下示例说明了这一点。  
  
##### <a name="example-loading-xml-from-files"></a>示例：从文件中加载 XML  
 此示例显示了如何在表 T 中插入行。从文件 C:\MyFile\xmlfile.xml 中将 XML 列的值作为 CLOB 加载，并为整数列提供了值 10。  
  
```  
INSERT INTO T  
SELECT 10, xCol  
FROM    (SELECT *      
    FROM OPENROWSET (BULK 'C:\MyFile\xmlfile.xml', SINGLE_CLOB)   
 AS xCol) AS R(xCol)  
```  
  
## <a name="text-encoding"></a>文本编码  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 以 Unicode (UTF-16) 存储 XML 数据。 从服务器检索的 XML 数据均采用 UTF-16 编码。 如果需要采用不同的编码，必须对检索到的数据执行所需的转换。 有时，XML 数据可能采用不同的编码。 如果是这样，加载数据时必须非常小心。 例如：  
  
-   如果文本 XML 采用 Unicode（UCS-2、UTF-16），可以将其赋给 XML 列、变量或参数，不会有任何问题。  
  
-   如果由于源代码页的原因，编码不是 Unicode 而是隐式的，则数据库中的字符串代码页应与要加载的码位相同或与其兼容。 如果需要，请使用 COLLATE。 如果不存在这样的服务器代码页，则必须添加使用正确编码的显式 XML 声明。  
  
-   若要使用显式编码，请使用不与代码页交互的 **varbinary()** 类型，或使用相应代码页的字符串类型。 然后，将数据赋给 XML 列、变量或参数。  
  
### <a name="example-explicitly-specifying-an-encoding"></a>示例：显式指定编码  
 假定你的 XML 文档和 vcdoc 存储为不具有显式 XML 声明的 **varchar(max)** 。 下面的语句添加了带有编码“iso8859-1”的 XML 声明，连接了 XML 文档，然后将结果转换为 **varbinary(max)** ，以便保留字节表示形式并最终将它转换为 XML。 这样，XML 处理器就可以根据指定的编码“iso8859-1”分析数据，并为字符串值生成相应的 UTF-16 表示形式。  
  
```  
SELECT CAST(   
CAST (('<?xml version="1.0" encoding="iso8859-1"?>'+ vcdoc) AS VARBINARY (MAX))   
 AS XML)  
```  
  
### <a name="string-encoding-incompatibilities"></a>字符串编码不兼容性  
 如果将 XML 作为字符串文字复制并粘贴到 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中的查询编辑器窗口，可能会遇到 [N]VARCHAR 字符串编码不兼容的情况。 这将取决于 XML 实例的编码。 在很多情况下，您可能需要删除 XML 声明。 例如：  
  
```  
<?xml version="1.0" encoding="UTF-8"?>  
  <xsd:schema ...  
```  
  
 然后，应包含 N 以使 XML 实例成为 Unicode 实例。 例如：  
  
```  
-- Assign XML instance to a variable.  
DECLARE @X XML  
SET @X = N'...'  
-- Insert XML instance into an xml type column.  
INSERT INTO T VALUES (N'...')  
-- Create an XML schema collection  
CREATE XML SCHEMA COLLECTION XMLCOLL1 AS N'<xsd:schema ... '  
```  
  
## <a name="see-also"></a>另请参阅  
 [XML 数据 (SQL Server)](../../relational-databases/xml/xml-data-sql-server.md)  
  
  
