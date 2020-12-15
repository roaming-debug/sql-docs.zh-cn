---
title: Updategram (SQLXML) 的准则和限制
description: 了解在 SQLXML 4.0 中使用 XML updategram 的准则和限制。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- updategrams [SQLXML], about updategrams
ms.assetid: b5231859-14e2-4276-bc17-db2817b6f235
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 71bd56145c0ef26736a7b567db22b783ea258e1c
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479188"
---
# <a name="guidelines-and-limitations-of-xml-updategrams-sqlxml-40"></a>XML Updategram 的指导原则和限制 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  在使用 XML updategram 时，请记住以下事项：  
  
-   如果将 updategram 用于只包含一对和块的插入操作 **\<before>** **\<after>** ，则 **\<before>** 可以省略块。 相反，如果是删除操作，则 **\<after>** 可以省略块。  
  
-   如果在标记中使用具有多个 **\<before>** 和 **\<after>** 块的 updategram **\<sync>** ，则 **\<before>** **\<after>** 必须指定块和块以形成 **\<before>** 和 **\<after>** 配对。  
  
-   updategram 中的更新适用于 XML 架构提供的 XML 视图。 因此，为使默认映射成功，必须在 updategram 中指定架构文件名；或者，如果未提供文件名，则元素名称和属性名称必须匹配数据库中的表名和列名。  
  
-   SQLXML 4.0 要求 updategram 中的所有列值都必须显式映射在提供的架构（XDR 或 XSD）中，以便为其子元素编写 XML 视图。 此行为与 SQLXML 的早期版本不同，后者允许在 **sql： relationship** 批注中隐含为外键的一部分的列的值。 请注意，这一变化并不影响主键值传播到子元素，如果没有为子元素显式指定值，则对于 SQLXML 4.0，主键值仍会传播到子元素。  
  
-   如果使用 updategram 来修改二进制列 (如 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] **image** 数据类型) 中的数据，则必须提供 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] (数据类型的映射架构（例如， **sql： datatype = "image"**) 和 XML 数据类型） (例如， **dt： type = "binhex"** 或 **dt： type = "binbase64**) 必须指定。 必须在 updategram 中指定二进制列的数据;updategram 将忽略在映射架构中指定的 **sql： url 编码** 批注。  
  
-   编写 XSD 架构时，如果为 **sql： relation** 或 **sql： field** 批注指定的值包含特殊字符（例如空格字符） (例如，在 "订单详细信息" 表名称) 中，此值必须用方括号括起来 (例如，"[Order Details]" ) 。  
  
-   在使用 updategram 时，不支持链关系。 例如，如果表 A 和 C 通过使用表 B 的链关系相关，则在尝试运行和执行 updategram 时将发生以下错误：  
  
    ```  
    There is an inconsistency in the schema provided.  
    ```  
  
     即使架构和 updategram 均正确并且有效建立，但如果存在某一链关系，此错误仍将发生。  
  
-   Updategram 不允许在更新过程中将 **图像** 类型数据作为参数传递。  
  
-   当使用 updategram 时，不应在中的块中使用 (BLOB) 类型的二进制大型对象 **\<before>** ，因为这将包含它们以用于并发控制。 由于 BLOB 类型的比较限制，这可能导致 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 出现问题。 例如，在 WHERE 子句中使用 LIKE 关键字比较 **文本** 数据类型的列;但是，对于数据大小大于8K 的 BLOB 类型，比较将失败。  
  
-   由于 BLOB 类型的比较限制， **ntext** 数据中的特殊字符可能导致 SQLXML 4.0 问题。 例如，在对 ntext 类型的列进行并发检查时，在 updategram 块中使用 "[Serializable]" **\<before>** 会失败， 并出现以下 SQLOLEDB 错误说明：  
  
    ```  
    Empty update, no updatable rows found   Transaction aborted  
    ```  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
