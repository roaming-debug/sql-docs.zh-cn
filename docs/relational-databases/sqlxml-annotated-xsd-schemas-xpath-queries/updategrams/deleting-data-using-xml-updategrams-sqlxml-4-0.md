---
title: '使用 XML Updategram 删除数据 (SQLXML) '
description: 了解如何在 SQLXML 4.0 中使用 XML updategram 删除数据。
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- <after> block
- updategrams [SQLXML], deleting data
- <before> block
- mapping-schema attribute
- record deletions [SQLXML]
ms.assetid: 4fb116d7-7652-474a-a567-cb475a20765c
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 085a1f8def37870f4fe5e449e4c206ed376f8d2a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462878"
---
# <a name="deleting-data-using-xml-updategrams-sqlxml-40"></a>使用 XML updategram 删除数据 (SQLXML 4.0)
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  当记录实例出现在块中，而在块中没有相应记录时，updategram 指示删除操作 **\<before>** **\<after>** 。 在这种情况下，updategram 将从数据库中删除该记录 **\<before>** 。  
  
 下面是 updategram 的删除操作格式：  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
  <updg:sync [mapping-schema="SampleSchema.xml"]  >  
   <updg:before>  
       <ElementName />  
      [<ElementName .../>... ]  
   </updg:before>  
    [<updg:after>  
    </updg:after>]  
  </updg:sync>  
</ROOT>  
```  
  
 **\<after>** 如果 updategram 仅执行删除操作，则可以省略标记。 如果未指定可选的 **映射架构** 特性，则 **\<ElementName>** 在 updategram 中指定的将映射到数据库表，并且子元素或属性映射到表中的列。  
  
 如果 updategram 中指定的元素与表中的多行匹配或与任何行都不匹配，则 updategram 将返回一个错误，并取消整个 **\<sync>** 块。 updategram 中的元素每次只能删除一个记录。  
  
## <a name="examples"></a>示例  
 本节中的示例使用默认映射（即未在 updategram 中指定映射架构）。 有关使用映射架构的 updategram 的更多示例，请参阅 [在 Updategram &#40;SQLXML 4.0&#41;中指定带批注的映射架构 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/specifying-an-annotated-mapping-schema-in-an-updategram-sqlxml-4-0.md)。  
  
 若要创建使用以下示例的工作示例，必须满足 [运行 SQLXML 示例的要求](../../../relational-databases/sqlxml/requirements-for-running-sqlxml-examples.md)中指定的要求。  
  
### <a name="a-deleting-a-record-by-using-an-updategram"></a>A. 使用 updategram 删除记录  
 以下 updategram 将从 HumanResources.Shift 表中删除两个记录。  
  
 在这些示例中，updategram 不指定映射架构。 因此，updategram 使用默认映射，其中元素名称映射到表名称，而属性或子元素映射到列。  
  
 此第一个 updategram 是以属性为中心的，它标识在块中 (白天和晚上夜) 的两个班次 **\<before>** 。 因为块中没有相应 **\<after>** 的记录，所以这是一个删除操作。  
  
```  
<ROOT xmlns:updg="urn:schemas-microsoft-com:xml-updategram">  
<updg:sync >  
  <updg:before>  
       <HumanResources.Shift ShiftID="4"  
                        Name="Day-Evening"  
                        StartTime="1900-01-01 11:00:00.000"  
                        EndTime="1900-01-01 19:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
       <HumanResources.Shift ShiftID="5"  
                        Name="Evening-Night"  
                        StartTime="1900-01-01 19:00:00.000"  
                        EndTime="1900-01-01 03:00:00.000"  
                        ModifiedDate="2004-01-01 00:00:00.000" />  
  </updg:before>  
  <updg:after>  
  </updg:after>  
</updg:sync>  
</ROOT>  
```  
  
##### <a name="to-test-the-updategram"></a>测试 updategram  
  
1.  完成示例 B ( "使用 updategram 插入多个记录" ) [使用 XML updategram &#40;SQLXML 4.0&#41;插入数据 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)。  
  
2.  将上面的 updategram 复制到记事本，并将其保存为与用来完成 ( "使用 updategram 插入多个记录" ) 使用 "使用 XML Updategram 插入多个记录" [&#40;SQLXML 4.0&#41;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)的相同文件夹中的 Updategram-RemoveShifts.xml。  
  
3.  创建并使用 SQLXML 4.0 测试脚本 (Sqlxml4test.vbs) 以执行 updategram。  
  
     有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
## <a name="see-also"></a>另请参阅  
 [&#40;SQLXML 4.0&#41;的 Updategram 安全注意事项 ](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/security/updategram-security-considerations-sqlxml-4-0.md)  
  
  
