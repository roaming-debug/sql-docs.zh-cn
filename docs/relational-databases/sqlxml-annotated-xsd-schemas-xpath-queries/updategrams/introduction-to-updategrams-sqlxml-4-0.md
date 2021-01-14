---
title: SQLXML) 的 Updategram 简介 (
description: 了解 SQLXML 4.0 updategram，可用于在数据库中插入、更新或删除数据。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- explicit schema mapping [SQLXML]
- updategrams [SQLXML], mapping schema specifying
- namespaces [SQLXML]
- updategrams [SQLXML], about updategrams
- attribute-centric mapping
- invalid characters [SQLXML]
- element-centric mapping [SQLXML]
- mapping schema [SQLXML], updategrams
- namespaces [SQLXML], updategrams
- executing updategrams [SQLXML]
- implicit schema mapping
ms.assetid: cfe24e82-a645-4f93-ab16-39c21f90cce6
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 6a416950247208c682d0fdc923db71357405fd0e
ms.sourcegitcommit: f29f74e04ba9c4d72b9bcc292490f3c076227f7c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/13/2021
ms.locfileid: "98170619"
---
# <a name="introduction-to-updategrams-sqlxml-40"></a>Updategram 简介 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  您可以 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通过使用 UPDATEGRAM 或 OPENXML 函数，从现有的 XML 文档中修改) 中的数据库 (插入、更新或删除 [!INCLUDE[tsql](../../../includes/tsql-md.md)] 。  
  
 OPENXML 函数通过拆分现有 XML 文档并提供可以传递给 INSERT、UPDATE 或 DELETE 语句的行集来修改数据库。 使用 OPENXML 时，直接针对数据库表进行操作。 因此，在行集提供程序（如表）可以显示为源时，最适合使用 OPENXML。  
  
 与 OPENXML 一样，updategram 允许您在数据库中插入、更新或删除数据；不过，updategram 针对带批注的 XSD（或 XDR）架构提供的 XML 视图进行操作，例如将更新应用于映射架构提供的 XML 视图。 而映射架构则具有将 XML 元素和属性映射到相应的数据库表和列所需的信息。 updategram 使用此映射信息更新数据库表和列。  
  
> [!NOTE]  
>  本文档假定您熟悉 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的模板和映射架构支持。 有关详细信息，请参阅 [&#40;SQLXML 4.0&#41;中带批注的 XSD 架构简介 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/introduction-to-annotated-xsd-schemas-sqlxml-4-0.md)。 对于使用 XDR 的旧版应用程序，请参阅 [SQLXML 4.0&#41;中 &#40;弃用的带批注 XDR 架构 ](../../../relational-databases/sqlxml/annotated-xsd-schemas/annotated-xdr-schemas-deprecated-in-sqlxml-4-0.md)。  
  
## <a name="required-namespaces-in-the-updategram"></a>Updategram 中必需的命名空间  
 Updategram 中的关键字（如 **\<sync>** 、 **\<before>** 和 **\<after>** ）存在于 **urn：架构-microsoft com： updategram** 命名空间中。 您可以为该命名空间使用任意前缀。 在本文档中， **updg** 前缀表示 **updategram** 命名空间。  
  
## <a name="reviewing-syntax"></a>检查语法  
 Updategram 是一个具有 **\<sync>** 、 **\<before>** 和 **\<after>** 块的模板，它们构成了 updategram 的语法。 以下代码显示了此语法的最简单形式：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema= "AnnotatedSchemaFile.xml"] >  
    <updg:before>  
        ...  
    </updg:before>  
    <updg:after>  
        ...  
    </updg:after>  
  </updg:sync>  
</ROOT>  
```  
  
 以下定义描述了其中每个块的作用：  
  
 **\<before>**  
 标识记录实例的现有状态（也称为“以前状态”）。  
  
 **\<after>**  
 标识要将数据更改到的新状态。  
  
 **\<sync>**  
 包含 **\<before>** 和 **\<after>** 块。 **\<sync>** 块可以包含多组 **\<before>** 和 **\<after>** 块。 如果有多个 **\<before>** 和 **\<after>** 块，则这些块 (即使为空) 也必须指定为成对。 此外，updategram 可以有多个 **\<sync>** 块。 每个 **\<sync>** 块都是一个事务单元 (这意味着块中的所有内容 **\<sync>** 均已完成或未完成任何操作) 。 如果 **\<sync>** 在 updategram 中指定多个块，则一个块的失败不 **\<sync>** 会影响其他 **\<sync>** 块。  
  
 Updategram 是删除、插入还是更新记录实例取决于 **\<before>** 和块的内容 **\<after>** ：  
  
-   如果记录实例只出现在块中 **\<before>** ，而在块中没有对应的实例 **\<after>** ，则 updategram 将执行删除操作。  
  
-   如果记录实例只出现在块中 **\<after>** ，而在块中没有对应的实例 **\<before>** ，则是插入操作。  
  
-   如果记录实例出现在块中 **\<before>** 并且在块中具有相应的实例 **\<after>** ，则是更新操作。 在这种情况下，updategram 会将记录实例更新为在块中指定的值 **\<after>** 。  
  
## <a name="specifying-a-mapping-schema-in-the-updategram"></a>在 Updategram 中指定映射架构  
 在 updategram 中，映射架构（支持 XSD 和 XDR 架构）所提供的 XML 抽象可以是隐式的，也可以是显式的（即无论是否指定映射架构，updategram 都可以工作）。 如果未指定映射架构，则 updategram 将采用默认映射)  (隐式映射，其中块或块中的每个元素都 **\<before>** **\<after>** 映射到表，每个元素的子元素或属性映射到数据库中的列。 如果显式指定映射架构，updategram 中的元素和属性必须与映射架构中的元素和属性匹配。  
  
### <a name="implicit-default-mapping"></a>隐式（默认）映射  
 在大多数情况下，执行简单更新的 updategram 可能不需要映射架构。 此时 updategram 依赖于默认映射架构。  
  
 以下 updategram 演示隐式映射。 在此示例中，updategram 在 Sales.Customer 表中插入一个新客户。 由于此 updategram 使用隐式映射，因此 \<Sales.Customer> 元素映射到 customer 表，CustomerID 和 SalesPersonID 属性映射到 customer 表中的相应列。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
<updg:before>  
</updg:before>  
<updg:after>  
    <Sales.Customer CustomerID="1" SalesPersonID="277" />  
    </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="explicit-mapping"></a>显式映射  
 如果指定映射架构（XSD 或 XDR），则 updategram 使用该架构确定要更新的数据库表和列。  
  
 如果 updategram 执行复杂更新 (例如，基于映射架构) 中指定的父子关系在多个表中插入记录，则必须通过使用 updategram 执行的 **映射架构** 特性显式提供映射架构。  
  
 由于 updategram 是模板，因此为 updategram 中的映射架构指定的路径是相对于模板文件的位置而言（即相对于存储 updategram 的位置而言）。 有关详细信息，请参阅 [在 Updategram 中指定带批注的映射架构 &#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
## <a name="element-centric-and-attribute-centric-mapping-in-updategrams"></a>Updategram 中以元素为中心的映射和以属性为中心的映射  
 使用默认映射（在 updategram 中未指定映射架构）时，如果是以元素为中心的映射，则 updategram 元素映射到表并且子元素映射到列。如果是以属性为中心的映射，则属性映射到列。  
  
### <a name="element-centric-mapping"></a>以元素为中心的映射  
 在以元素为中心的 updategram 中，元素包含指示元素属性的子元素。 请参阅以下 updategram 示例。 **\<Person.Contact>** 元素包含 **\<FirstName>** 和 **\<LastName>** 子元素。 这些子元素是元素的属性 **\<Person.Contact>** 。  
  
 由于此 updategram 未指定映射架构，因此 updategram 将使用隐式映射，其中 **\<Person.Contact>** 元素映射到 Person 表，而其子元素映射到 FirstName 和 LastName 列。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:after>  
    <Person.Contact>  
       <FirstName>Catherine</FirstName>  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="attribute-centric-mapping"></a>以属性为中心的映射  
 在以属性为中心的映射中，元素具有属性。 以下 updategram 使用以属性为中心的映射。 在此示例中， **\<Person.Contact>** 元素包含 **FirstName** 和 **LastName** 属性。 这些属性是元素的属性 **\<Person.Contact>** 。 如前面的示例所示，此 updategram 不指定任何映射架构，因此它依赖于隐式映射来将元素映射到 **\<Person.Contact>** 表中的相应列。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" LastName="Abel" />  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
### <a name="using-both-element-centric-and-attribute-centric-mapping"></a>同时使用以元素为中心的映射和以属性为中心的映射  
 可以组合使用以元素为中心的映射和以属性为中心的映射，如以下 updategram 中所示。 请注意， **\<Person.Contact>** 元素同时包含特性和子元素。 此 updategram 也依赖于隐式映射。 因此， **FirstName** 属性和 **\<LastName>** 子元素映射到 Person 表中的相应列。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
  </updg:before>  
  <updg:after>  
    <Person.Contact FirstName="Catherine" >  
       <LastName>Abel</LastName>  
    </Person.Contact>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
## <a name="working-with-characters-valid-in-sql-server-but-not-valid-in-xml"></a>使用在 SQL Server 中有效但在 XML 中无效的字符  
 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中，表名称可以包含空格。 但是，此类表名称在 XML 中无效。  
  
 若要对是有效 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 标识符，但不是有效 XML 标识符的字符进行编码，请使用 "__xHHHH \_ \_ " 作为编码值，其中 HHHH 代表最高有效位第一次的字符的四位十六进制 UCS-2 代码。 使用此编码方案时，空格字符将被替换为一个空格字符的四位数十六进制代码) 的 (x0020;因此，中的表名 [Order Details] 在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] \_ XML 中变为 _x005B_Order_x0020_Details_x005D。  
  
 同样，你可能需要指定由三个部分组成的元素名称，如 \<[database].[owner].[table]> 。 由于方括号 ( [and] ) 在 XML 中无效，因此您必须将其指定为 \<_x005B_database_x005D\_._x005B_owner_x005D\_._x005B_table_x005D\_> ，其中 _x005B \_ 是左括号的编码 ( [) ，_x005D \_ 是右大括号的编码 (] ) 。  
  
## <a name="executing-updategrams"></a>执行 Updategram  
 由于 updategram 是模板，因此模板的所有处理机制均适用于 updategram。 对于 SQLXML 4.0，可以通过以下方式之一来执行 updategram：  
  
-   在 ADO 命令中提交它。  
  
-   将其作为 OLE DB 命令提交。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
