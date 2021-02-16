---
title: XML 格式化文件 (SQL Server) | Microsoft Docs
description: SQL Server 2019 提供了一种 XML 架构，该架构定义了编写 XML 格式文件以将数据批量导入 SQL Server 表中的语法。
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.reviewer: ''
ms.technology: data-movement
ms.topic: conceptual
helpviewer_keywords:
- format files [SQL Server], XML format files
- bulk importing [SQL Server], format files
- XML format files [SQL Server]
ms.assetid: 69024aad-eeea-4187-8fea-b49bc2359849
author: MashaMSFT
ms.author: mathoma
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 648766b45290fce3b954265b854c5bbd2fb0602c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100351292"
---
# <a name="xml-format-files-sql-server"></a>XML 格式化文件 (SQL Server)
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 提供了一个 XML 架构，该架构定义了编写“XML 格式化文件”（用于将数据大容量导入到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中）的语法。 XML 格式化文件必须符合用 XML 架构定义语言 (XSDL) 定义的这种架构。 只有当 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 工具和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 一起安装后，才支持 XML 格式化文件。  
  
 可以结合使用 XML 格式化文件和 **bcp** 命令、BULK INSERT 语句或 INSERT...SELECT \* FROM OPENROWSET(BULK...) 语句。 使用 **bcp** 命令，你可以自动生成表的 XML 格式化文件；有关详细信息，请参阅 [bcp Utility](../../tools/bcp-utility.md)。  
  
> [!NOTE]  
>  大容量导出和导入支持两种类型的格式化文件： *非 XML 格式化文件* 和 *XML 格式化文件*。 XML 格式化文件比非 XML 格式化文件更灵活，功能更强大。 有关非 XML 格式化文件的信息，请参阅 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)。  
  
 **本主题内容：**  
  
-   [XML 格式化文件的优点](#BenefitsOfXmlFFs)  
  
-   [XML 格式化文件的结构](#StructureOfXmlFFs)  
  
-   [XML 格式化文件的架构语法](#SchemaSyntax)  
  
-   [XML 格式化文件示例](#SampleXmlFFs)  
  
-   [相关任务](#RelatedTasks)  
  
-   [相关内容](#RelatedContent)  
  
##  <a name="benefits-of-xml-format-files"></a><a name="BenefitsOfXmlFFs"></a> XML 格式化文件的优点  
  
-   XML 格式化文件是自描述型，因此易于阅读、 创建和扩展。 这些文件具有较强的可读性，这使您更容易理解在大容量操作过程中是如何解释数据的。  
  
-   XML 格式化文件包含目标列的数据类型。  XML 编码清晰地描述了数据文件的数据类型和数据元素以及数据元素和表列之间的映射。  
  
     这可以将数据文件中数据的表示形式与文件中每个字段相关联的数据类型分离开来。 例如，如果数据文件包含字符表示形式的数据，则相应的 SQL 列类型会丢失。  
  
-   XML 格式化文件允许从数据文件加载包含单一大型对象 (LOB) 数据类型的字段。  
  
-   XML 格式化文件得以增强的同时，仍与早期版本保持兼容。 此外，XML 编码清晰，有利于为给定数据文件创建多个格式化文件。 这有助于将所有或某些数据字段映射到不同表或视图中的列。  
  
-   XML 语法与操作的方向无关；即大容量导出和大容量导入中使用的语法相同。  
  
-   使用 XML 格式化文件，可以将数据大容量导入表或非分区视图，并可以大容量导出数据。  
  
-   对于 OPENROWSET(BULK...) 函数，指定目标表是可选的。 这是因为该函数依赖 XML 格式化文件从数据文件中读取数据。  
  
    > [!NOTE]  
    >  **bcp** 命令和 BULK INSERT 语句需要使用目标表，因为它们使用目标表列执行类型转换。  
  
##  <a name="structure-of-xml-format-files"></a><a name="StructureOfXmlFFs"></a> XML 格式化文件的结构  
 和非 XML 格式化文件一样，XML 格式化文件定义数据文件中数据字段的格式和结构，并将这些数据字段映射到单个目标表中的相应列。  
  
 XML 格式化文件有两个主要组件，即 \<RECORD> 和 \<ROW>：  
  
-   \<RECORD> 说明数据文件中存储的数据。  
  
     每个 \<RECORD> 元素包含一个或多个 \<FIELD> 元素。 这些元素与数据文件中的字段相对应。 基本语法如下：  
  
     \<RECORD>  
  
     \<FIELD .../> [ ...n ]  
  
     \</RECORD>  
  
     每个 \<FIELD> 元素说明特定数据字段的内容。 一个字段只能映射到表中的一列， 并不是所有字段都需要映射到列。  
  
     数据文件中字段的长度可以是固定或可变的，也可以由字符结尾。 *字段值* 可以表示为字符（使用单字节表示形式）、宽字符（使用 Unicode 双字节表示形式）、本机数据库格式或文件名。 如果字段值为文件名，则文件名指向包含目标表中 BLOB 列的值的文件。  
  
-   \<ROW> 说明在将数据从文件导入 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 表中时，如何构造数据文件中的数据行。  
  
     \<ROW> 元素包含一组 \<COLUMN> 元素。 这些元素与表列相对应。 基本语法如下：  
  
     \<ROW>  
  
     \<COLUMN .../> [ ...n ]  
  
     \</ROW>  
  
     每个 \<COLUMN> 元素均只能映射到数据文件中的一个字段。 \<ROW> 元素中 \<COLUMN> 元素的顺序定义了其在批量操作中返回的顺序。 XML 格式化文件为每个 \<COLUMN> 元素分配了一个本地名称，该名称与批量导入操作的目标表中的列没有关系。  
  
##  <a name="schema-syntax-for-xml-format-files"></a><a name="SchemaSyntax"></a> XML 格式化文件的架构语法  
 本节概要介绍 XML 格式化文件的 XML 架构的元素和属性。 格式化文件的语法与操作的方向无关；即大容量导出和大容量导入中使用的语法相同。 本部分还介绍批量导入如何使用 \<ROW> 和 \<COLUMN> 元素以及如何将元素的 xsi:type 值放入数据集。  
  
 若要查看该语法与实际的 XML 格式化文件的对应关系，请参阅本主题后面的 [XML 格式化文件示例](#SampleXmlFFs)。  
  
> [!NOTE]  
>  您可以修改格式化文件，以便从字段编号和/或顺序与表列的编号和/或顺序不同的数据文件进行大容量导入。 有关详细信息，请参阅 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)。  
  
 **本节内容：**  
  
-   [XML 架构的基本语法](#BasicSyntax)  
  
-   [批量导入如何使用 \<ROW> 元素](#HowUsesROW)  
  
-   [批量导入如何使用 \<COLUMN> 元素](#HowUsesColumn)  
  
-   [将 xsi:type 值放入数据集](#PutXsiTypeValueIntoDataSet)  
  
###  <a name="basic-syntax-of-the-xml-schema"></a><a name="BasicSyntax"></a> XML 架构的基本语法  
 此语法语句仅显示元素（\<BCPFORMAT>、\<RECORD>、\<FIELD>、\<ROW> 和 \<COLUMN>）及其基本属性。  
  
 \<BCPFORMAT ...>  
  
 \<RECORD>  
  
 \<FIELD ID = "*fieldID*" xsi:type = "*fieldType*" [...]  
  
 />  
  
 \</RECORD>  
  
 \<ROW>  
  
 \<COLUMN SOURCE = "*fieldID*" NAME = "*columnName*" xsi:type = "*columnType*" [...]  
  
 />  
  
 \</ROW>  
  
 \</BCPFORMAT>  
  
> [!NOTE]  
>  对于其他与 \<FIELD> 或 \<COLUMN> 元素中的 xsi:type 值相关的属性，将在本主题后面部分予以介绍。  
  
 **本节内容：**  
  
-   [架构元素](#SchemaElements)  
  
-   [\<FIELD> 元素的属性](#AttrOfFieldElement)（和 [\<FIELD> 元素的 xsi:type 值](#XsiTypeValuesOfFIELD)）  
  
-   [\<COLUMN> 元素的属性](#AttrOfColumnElement)（和 [\<COLUMN> 元素的 xsi:type 值](#XsiTypeValuesOfCOLUMN)）  
  
####  <a name="schema-elements"></a><a name="SchemaElements"></a> 架构元素  
 本节总结了 XML 架构为 XML 格式化文件定义的每个元素的作用。 本主题在后面有单独的章节介绍这些属性。  
  
 \<BCPFORMAT>  
 即格式化文件元素。它定义给定数据文件的记录结构及其与表中某行的各列的对应关系。  
  
 \<RECORD .../>  
 定义包含一个或多个 \<FIELD> 元素的复杂元素。 在格式化文件中声明的字段的顺序与那些字段在数据文件中出现的顺序相同。  
  
 \<FIELD .../>  
 定义数据文件中的字段，用来容纳数据。  
  
 本主题将在后面的 [\<FIELD> 元素的属性](#AttrOfFieldElement)中讨论此元素的属性。  
  
 \<ROW .../>  
 定义包含一个或多个 \<COLUMN> 元素的复杂元素。 \<COLUMN> 元素的顺序与 RECORD 定义中 \<FIELD> 元素的顺序无关。 但是，\<COLUMN> 元素在格式化文件中的顺序决定了所生成行集的列的顺序。 数据字段的加载将按照相应的 \<COLUMN> 元素在 \<COLUMN> 元素中的声明顺序进行。  
  
 有关详细信息，请参阅本主题后面的[批量导入如何使用 \<ROW> 元素](#HowUsesROW)。  
  
 \<COLUMN>  
 将列定义为元素 (\<COLUMN>)。 每个 \<COLUMN> 元素对应一个 \<FIELD> 元素（其 ID 在 \<COLUMN> 元素的 SOURCE 属性中指定）。  
  
 本主题将在后面的 [\<COLUMN> 元素的属性](#AttrOfColumnElement)中讨论此元素的属性。 另请参阅本主题后面的[批量导入如何使用 \<COLUMN> 元素](#HowUsesColumn)。  
  
 \</BCPFORMAT>  
 用于结束格式化文件。  
  
####  <a name="attributes-of-the-field-element"></a><a name="AttrOfFieldElement"></a>\<FIELD> 元素的属性  
 本部分介绍 \<FIELD> 元素的属性，现将其架构语法总结如下：  
  
 <FIELD  
  
 ID **="** _fieldID_ **"**  
  
 xsi **:** type **="** _fieldType_ **"**  
  
 [ LENGTH **="** _n_ **"** ]  
  
 [ PREFIX_LENGTH **="** _p_ **"** ]  
  
 [ MAX_LENGTH **="** _m_ **"** ]  
  
 [ COLLATION **="** _collationName_ **"** ]  
  
 [ TERMINATOR **="** _terminator_ **"** ]  
  
 />  
  
 每个 \<FIELD> 元素都与其他元素无关。 字段是通过下列属性进行描述的：  
  
|FIELD 属性|说明|可选/<br /><br /> 必选|  
|---------------------|-----------------|------------------------------|  
|ID **="** _fieldID_ **"**|指定数据文件中的字段的逻辑名称。 字段的 ID 是用于引用字段的键。<br /><br /> \<FIELD ID**="**_fieldID_**"**/> 映射到 \<COLUMN SOURCE**="**_fieldID_**"**/>|必选|  
|xsi:type **="** _fieldType_ **"**|这是一个 XML 构造，用法类似于属性。它定义元素实例的类型。 *fieldType* 的值决定了给定实例中需要下面哪个可选属性。|必需（取决于数据类型）|  
|LENGTH **="** _n_ **"**|此属性定义固定长度的数据类型实例的长度。<br /><br /> *n* 值必须是正整数。|除非是 xsi:type 值所必需，否则可选。|  
|PREFIX_LENGTH **="** _p_ **"**|此属性定义二进制数据表示形式的前缀的长度。 PREFIX_LENGTH 值 p 必须是下列值之一：1、2、4 或 8。|除非是 xsi:type 值所必需，否则可选。|  
|MAX_LENGTH **="** _m_ **"**|此属性为给定字段中可以存储的最大字节数。 如果没有目标表，列的最大长度就是未知的。 MAX_LENGTH 属性限定输出字符列的最大长度，从而限制为列值分配的存储空间。 当在 SELECT FROM 子句中使用了 OPENROWSET 函数的 BULK 选项时，使用该属性将带来极大的方便。<br /><br /> *m* 值必须是正整数。 默认情况下， **char** 列的最大长度为 8000 个字符， **nchar** 列的最大长度为 4000 个字符。|可选|  
|COLLATION **="** _collationName_ **"**|COLLATION 仅适用于字符字段。 有关 SQL 排序规则名称的列表，请参阅 [SQL Server 排序规则名称 (Transact SQL)](../../t-sql/statements/sql-server-collation-name-transact-sql.md)。|可选|  
|TERMINATOR **= "** _terminator_ **"**|此属性指定数据字段的终止符。 该终止符可以是任意字符。 该字符必须是数据中没有的唯一字符。<br /><br /> 默认情况下，该字段的终止符为制表符（用 \t 表示）。 若要表示段落标记，请使用 \r\n。|仅和需要该属性的字符数据 xsi:type 一起使用。|  
  
#####  <a name="xsitype-values-of-the-field-element"></a><a name="XsiTypeValuesOfFIELD"></a>\<FIELD> 元素的 xsi:type 值  
 xsi:type 值是标识元素实例的数据类型的 XML 构造（用法同属性）。 本节后面将介绍有关“在数据集中包含 xsi:type 值”的信息。  
  
 \<FIELD> 元素的 xsi:type 值支持下列数据类型。  
  
|\<FIELD> xsi:type 值|数据类型<br /><br /> 的可选 XML 属性|数据类型<br /><br /> 的可选 XML 属性|  
|-------------------------------|---------------------------------------------------|---------------------------------------------------|  
|**NativeFixed**|**LENGTH**|无。|  
|**NativePrefix**|**PREFIX_LENGTH**|MAX_LENGTH|  
|**CharFixed**|**LENGTH**|COLLATION|  
|**NCharFixed**|**LENGTH**|COLLATION|  
|**CharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH、COLLATION|  
|**NCharPrefix**|**PREFIX_LENGTH**|MAX_LENGTH、COLLATION|  
|**CharTerm**|**TERMINATOR**|MAX_LENGTH、COLLATION|  
|**NCharTerm**|**TERMINATOR**|MAX_LENGTH、COLLATION|  
  
 有关 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型的详细信息，请参阅[数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
####  <a name="attributes-of-the-column-element"></a><a name="AttrOfColumnElement"></a>\<COLUMN> 元素的属性  
 本部分介绍 \<COLUMN> 元素的属性，现将其架构语法总结如下：  
  
 <COLUMN  
  
 SOURCE = "*fieldID*"  
  
 NAME = "*columnName*"  
  
 xsi:type = "*columnType*"  
  
 [ LENGTH = "*n*" ]  
  
 [ PRECISION = "*n*" ]  
  
 [ SCALE = "*value*" ]  
  
 [ NULLABLE = { "YES"  
  
 "NO" } ]  
  
 />  
  
 使用下列属性将字段映射到目标表中的列：  
  
|COLUMN 属性|说明|可选/<br /><br /> 必选|  
|----------------------|-----------------|------------------------------|  
|SOURCE **="** _fieldID_ **"**|指定映射到列的字段 ID。<br /><br /> \<COLUMN SOURCE**="**_fieldID_**"**/> 映射到 \<FIELD ID**="**_fieldID_**"**/>|必选|  
|NAME = "*columnName*"|指定格式化文件所表示的行集中的列名。 此列名用于标识结果集中的列，并且该列不需要与目标表中使用的列名相对应。|必选|  
|xsi **:** type **="** _ColumnType_ **"**|这是一个 XML 构造，用法类似于属性。它定义元素实例的数据类型。 *ColumnType* 的值决定了给定实例中需要下面哪个可选属性。<br /><br /> 注意：[&lt;COLUMN&gt; 元素的 xsi:type 值](#XsiTypeValuesOfCOLUMN)部分中的 \<COLUMN> 元素表列出了 ColumnType 的可能值及其相关属性。|可选|  
|LENGTH **="** _n_ **"**|定义固定长度的数据类型实例的长度。 仅当 xsi:type 为字符串数据类型时，才使用 LENGTH。<br /><br /> *n* 值必须是正整数。|可选（仅当 xsi:type 是字符串数据类型时才可用）|  
|PRECISION **="** _n_ **"**|指示数字的位数。 例如，数 123.45 精度为 5。<br /><br /> 该值必须是正整数。|可选（仅在 xsi:type 是变量数字数据类型时才可用）|  
|SCALE **="** _int_ **"**|指示数字中小数点右边的位数。 例如，数字 123.45 的小数位数为 2。<br /><br /> 该值必须为整数。|可选（仅在 xsi:type 是变量数字数据类型时才可用）|  
|NULLABLE **=** { **"** YES **"**<br /><br /> **"** NO **"** }|指示列是否可以接受 NULL 值。 此属性与 FIELDS 完全无关。 但是，如果列不可为空值，而字段指定为 NULL（未指定任何值），将产生运行时错误。<br /><br /> NULLABLE 属性仅在您只执行普通 SELECT FROM OPENROWSET(BULK...) 语句时才使用。|可选（任何数据类型均可用）|  
  
#####  <a name="xsitype-values-of-the-column-element"></a><a name="XsiTypeValuesOfCOLUMN"></a>\<COLUMN> 元素的 xsi:type 值  
 xsi:type 值是标识元素实例的数据类型的 XML 构造（用法同属性）。 本节后面将介绍有关“在数据集中包含 xsi:type 值”的信息。  
  
 \<COLUMN> 元素支持下列本机 SQL 数据类型：  
  
|类型类别|\<COLUMN> 数据类型|数据类型<br /><br /> 的可选 XML 属性|数据类型<br /><br /> 的可选 XML 属性|  
|-------------------|---------------------------|---------------------------------------------------|---------------------------------------------------|  
|已修复|**SQLBIT**、 **SQLTINYINT**、 **SQLSMALLINT**、 **SQLINT**、 **SQLBIGINT**、 **SQLFLT4**、 **SQLFLT8**、 **SQLDATETIME**、 **SQLDATETIM4**、 **SQLDATETIM8**、 **SQLMONEY**、 **SQLMONEY4**、 **SQLVARIANT** 和 **SQLUNIQUEID**|无。|NULLABLE|  
|变量号|**SQLDECIMAL** 和 **SQLNUMERIC**|无。|NULLABLE、PRECISION、SCALE|  
|LOB|**SQLIMAGE**、 **CharLOB**、 **SQLTEXT** 和 **SQLUDT**|无。|NULLABLE|  
|字符 LOB|**SQLNTEXT**|无。|NULLABLE|  
|二进制字符串|**SQLBINARY** 和 **SQLVARYBIN**|无。|NULLABLE、LENGTH|  
|字符串|**SQLCHAR**、 **SQLVARYCHAR**、 **SQLNCHAR** 和 **SQLNVARCHAR**|无。|NULLABLE、LENGTH|  
  
> [!IMPORTANT]  
>  若要大容量导出或导入 SQLXML 数据，请在格式化文件中使用下列数据类型之一：SQLCHAR 或 SQLVARYCHAR（数据以客户端代码页或排序规则隐含的代码页的形式发送）、SQLNCHAR 或 SQLNVARCHAR（数据以 Unicode 的形式发送）或者 SQLBINARY 或 SQLVARYBIN（数据不经任何转换直接发送）。  
  
 有关值数据类型 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的详细信息，请参阅 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)。  
  
###  <a name="how-bulk-import-uses-the-row-element"></a><a name="HowUsesROW"></a>批量导入如何使用 \<ROW> 元素  
 在某些上下文中可以忽略 \<ROW> 元素。 \<ROW> 元素是否影响批量导入操作取决于操作的执行方式：  
  
-   **bcp** 命令  
  
     在目标表加载数据时，bcp 忽略 \<ROW> 组件。 相反， **bcp** 根据目标表的列类型来加载数据。  
  
-   [!INCLUDE[tsql](../../includes/tsql-md.md)] 语句（BULK INSERT 和 OPENROWSET 的大容量行集访问接口）  
  
     在将数据批量导入表中时，[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句使用 \<ROW> 组件来生成输入行集。 此外，[!INCLUDE[tsql](../../includes/tsql-md.md)] 语句还将根据 \<ROW> 下指定的列类型和目标表中的对应列，进行适当的类型转换。 如果格式化文件和目标表中指定的列类型之间存在不匹配，还将进行额外的类型转换。 与 **bcp** 相比，此额外的类型转换可能引起 BULK INSERT 或 OPENROWSET 的 BULK 行集提供程序中的行为出现某些差异（即损失精度）。  
  
     无需任何其他信息，仅利用 \<ROW> 元素中的信息即可构造行。 因此，可以使用 SELECT 语句 (SELECT \* FROM OPENROWSET(BULK *datafile* FORMATFILE=*xmlformatfile*) 来生成行集。  
  
    > [!NOTE]  
    >  OPENROWSET BULK 子句需要格式化文件（请注意，将字段的数据类型转换为列的数据类型只能使用 XML 格式化文件进行）。  
  
###  <a name="how-bulk-import-uses-the-column-element"></a><a name="HowUsesColumn"></a>批量导入如何使用 \<COLUMN> 元素  
 为了将数据批量导入表中，格式化文件中的 \<COLUMN> 元素将数据文件字段映射到表列，方法是指定：  
  
-   行中每个字段在数据文件中的位置。  
  
-   列类型，用于将字段数据类型转换为所需的列数据类型。  
  
 如果没有列映射到某个字段，该字段将不会被复制到生成的行。 此行为使得数据文件能够在不同的表中生成含有不同列的行。  
  
 同样，为了将数据从表中批量导出，格式化文件中的各个 \<COLUMN> 将输入表行的列映射到输出数据文件中与之对应的字段。  
  
###  <a name="putting-the-xsitype-value-into-a-data-set"></a><a name="PutXsiTypeValueIntoDataSet"></a> 将 xsi:type 值放入数据集  
 当通过 XML 架构定义 (XSD) 语言验证 XML 文档时，xsi:type 值不放入数据集。 但是，通过将 XML 格式化文件加载到 XML 文档（如 `myDoc`）中，可以将 xsi:type 信息放入数据集。如下列代码段所示：  
  
```cs
...;  
myDoc.LoadXml(xmlFormat);  
XmlNodeList ColumnList = myDoc.GetElementsByTagName("COLUMN");  
for(int i=0;i<ColumnList.Count;i++)  
{  
   Console.Write("COLUMN: xsi:type=" +ColumnList[i].Attributes["type",  
      "http://www.w3.org/2001/XMLSchema-instance"].Value+"\n");  
}  
```  
  
##  <a name="sample-xml-format-files"></a><a name="SampleXmlFFs"></a> XML 格式化文件示例  
 本节包含在各种情况下使用 XML 格式化文件的信息，并提供了一个 [!INCLUDE[ssSampleDBCoShort](../../includes/sssampledbcoshort-md.md)] 示例。  
  
> [!NOTE]  
>  在下列示例所示的数据文件中， `<tab>` 表示数据文件中的一个制表符， `<return>` 表示一个回车符。  
  
 这些示例说明了使用 XML 格式化文件的各个主要方面，如下所述：  
  
-   [对字符数据字段和表列进行相同的排序](#OrderCharFieldsSameAsCols)  
  
-   [对数据字段和表列进行不同的排序](#OrderFieldsAndColsDifferently)  
  
-   [省略数据字段](#OmitField)  
  
-   [将各种类型的字段映射到列](#MapXSItype)  
  
-   [将 XML 数据映射到表](#MapXMLDataToTbl)  
  
-   [导入固定长度或固定宽度的字段](#ImportFixedFields)  
  
-   [其他示例](#AdditionalExamples)  
  
> [!NOTE]  
>  有关创建格式化文件的信息，请参阅 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)。  
  
###  <a name="a-ordering-character-data-fields-the-same-as-table-columns"></a><a name="OrderCharFieldsSameAsCols"></a> A. 对字符数据字段和表列进行相同的排序  
 下面的示例显示了一个 XML 格式化文件，该文件描述一个包含三个字符数据字段的数据文件。 格式化文件将数据文件映射到包含三列的表中。 数据字段与表中的列一一对应。  
  
 **表（行）：** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **数据文件（记录）：** Age\<tab>Firstname\<tab>Lastname\<return>  
  
 以下 XML 格式化文件从数据文件读取数据到表中。  
  
 在 `<RECORD>` 元素中，格式化文件将所有三个字段中的数据值表示为字符数据。 对于每个字段， `TERMINATOR` 属性指示位于数据值后面的终止符。  
  
 数据字段与表中的列一一对应。 在 `<ROW>` 元素中，格式化文件将 `Age` 列映射到第一个字段，将 `FirstName` 列映射到第二个字段，将 `LastName` 列映射到第三个字段。  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>   
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="2" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="3" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  有关等效的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例，请参阅 [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)。  
  
###  <a name="b-ordering-data-fields-and-table-columns-differently"></a><a name="OrderFieldsAndColsDifferently"></a> B. 对数据字段和表列进行不同的排序  
 下面的示例显示了一个 XML 格式化文件，该文件描述一个包含三个字符数据字段的数据文件。 格式化文件将数据文件映射到包含三列（与数据文件的字段排序方式不同）的表中。  
  
 **表（行）：** Person (Age int, FirstName varchar(20), LastName varchar(30))  
  
 **数据文件**（记录）：Age\<tab>Lastname\<tab>Firstname\<return>  
  
 在 `<RECORD>` 元素中，格式化文件将所有三个字段中的数据值表示为字符数据。  
  
 在 `<ROW>` 元素中，格式化文件将 `Age` 列映射到第一个字段，将 `FirstName` 列映射到第三个字段，将 `LastName` 列映射到第二个字段。  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t" MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30" COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="2" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  有关等效的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例，请参阅 [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)。  
  
###  <a name="c-omitting-a-data-field"></a><a name="OmitField"></a> C. 省略数据字段  
 下面的示例显示了一个 XML 格式化文件，该文件描述一个包含四个字符数据字段的数据文件。 格式化文件将数据文件映射到包含三列的表中。 第二个数据字段不与任何表列对应。  
  
 **表（行）：** Person (Age int, FirstName Varchar(20), LastName Varchar(30))  
  
 **数据文件（记录）：** Age\<tab>employeeID\<tab>Firstname\<tab>Lastname\<return>  
  
 在 `<RECORD>` 元素中，格式化文件将所有四个字段中的数据值表示为字符数据。 对于每个字段，TERMINATOR 属性指示位于数据值后面的终止符。  
  
 在 `<ROW>` 元素中，格式化文件将 `Age` 列映射到第一个字段，将 `FirstName` 列映射到第三个字段，将 `LastName` 列映射到第四个字段。  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT   
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="12"/>  
    <FIELD ID="2" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="10"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\t"   
      MAX_LENGTH="20"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
    <FIELD ID="4" xsi:type="CharTerm" TERMINATOR="\r\n"   
      MAX_LENGTH="30"   
      COLLATION="SQL_Latin1_General_CP1_CI_AS"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="age" xsi:type="SQLINT"/>  
    <COLUMN SOURCE="3" NAME="firstname" xsi:type="SQLVARYCHAR"/>  
    <COLUMN SOURCE="4" NAME="lastname" xsi:type="SQLVARYCHAR"/>  
  </ROW>  
</BCPFORMAT>  
```  
  
> [!NOTE]  
>  有关等效的 [!INCLUDE[ssSampleDBobject](../../includes/sssampledbobject-md.md)] 示例，请参阅 [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)。  
  
###  <a name="d-mapping-field-xsitype-to-column-xsitype"></a><a name="MapXSItype"></a> D. 将 \<FIELD> xsi:type 映射到 \<COLUMN> xsi:type  
 下面的示例显示了各种类型的字段及其与列的映射。  
  
```xml
<?xml version = "1.0"?>  
<BCPFORMAT  
xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
   <RECORD>  
      <FIELD xsi:type="CharTerm" ID="C1" TERMINATOR="\t"   
            MAX_LENGTH="4"/>  
      <FIELD xsi:type="CharFixed" ID="C2" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="CharPrefix" ID="C3" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharTerm" ID="C4" TERMINATOR="\t"   
         MAX_LENGTH="4"/>  
      <FIELD xsi:type="NCharFixed" ID="C5" LENGTH="10"   
         COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NCharPrefix" ID="C6" PREFIX_LENGTH="2"   
         MAX_LENGTH="32" COLLATION="SQL_LATIN1_GENERAL_CP1_CI_AS"/>  
      <FIELD xsi:type="NativeFixed" ID="C7" LENGTH="4"/>  
   </RECORD>  
   <ROW>  
      <COLUMN SOURCE="C1" NAME="Age" xsi:type="SQLTINYINT"/>  
      <COLUMN SOURCE="C2" NAME="FirstName" xsi:type="SQLVARYCHAR"   
      LENGTH="16" NULLABLE="NO"/>  
      <COLUMN SOURCE="C3" NAME="LastName" />  
      <COLUMN SOURCE="C4" NAME="Salary" xsi:type="SQLMONEY"/>  
      <COLUMN SOURCE="C5" NAME="Picture" xsi:type="SQLIMAGE"/>  
      <COLUMN SOURCE="C6" NAME="Bio" xsi:type="SQLTEXT"/>  
      <COLUMN SOURCE="C7" NAME="Interest"xsi:type="SQLDECIMAL"   
      PRECISION="5" SCALE="3"/>  
   </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="e-mapping-xml-data-to-a-table"></a><a name="MapXMLDataToTbl"></a> E. 将 XML 数据映射到表  
 下面的示例创建了一个空的两列表 (`t_xml`)，表中的第一列映射到 `int` 数据类型，第二列映射到 `xml` 数据类型。  
  
```sql
CREATE TABLE t_xml (c1 int, c2 xml)  
```  
  
 以下 XML 格式化文件将数据文件加载到表 `t_xml`中。  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"   
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
 <RECORD>  
  <FIELD ID="1" xsi:type="NativePrefix" PREFIX_LENGTH="1"/>  
  <FIELD ID="2" xsi:type="NCharPrefix" PREFIX_LENGTH="8"/>  
 </RECORD>  
 <ROW>  
  <COLUMN SOURCE="1" NAME="c1" xsi:type="SQLINT"/>  
  <COLUMN SOURCE="2" NAME="c2" xsi:type="SQLNCHAR"/>  
 </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="f-importing-fixed-length-or-fixed-width-fields"></a><a name="ImportFixedFields"></a> F. 导入固定长度或固定宽度的字段  
 下面的示例分别介绍包含 `10` 个或 `6` 个字符的固定字段。 格式化文件将这些字段的长度/宽度分别表示为 `LENGTH="10"` 和 `LENGTH="6"`。 数据文件中的每行都以回车符-换行符组合 {CR}{LF} 结束，格式化文件将这表示为 `TERMINATOR="\r\n"`。  
  
```xml
<?xml version="1.0"?>  
<BCPFORMAT  
       xmlns="https://schemas.microsoft.com/sqlserver/2004/bulkload/format"  
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">  
  <RECORD>  
    <FIELD ID="1" xsi:type="CharFixed" LENGTH="10"/>  
    <FIELD ID="2" xsi:type="CharFixed" LENGTH="6"/>  
    <FIELD ID="3" xsi:type="CharTerm" TERMINATOR="\r\n"/>  
  </RECORD>  
  <ROW>  
    <COLUMN SOURCE="1" NAME="C1" xsi:type="SQLINT" />  
    <COLUMN SOURCE="2" NAME="C2" xsi:type="SQLINT" />  
  </ROW>  
</BCPFORMAT>  
```  
  
###  <a name="additional-examples"></a><a name="AdditionalExamples"></a> 其他示例  
 有关非 XML 格式化文件和 XML 格式化文件的其他示例，请参阅下列主题：  
  
-   [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-tasks"></a><a name="RelatedTasks"></a> 相关任务  
  
-   [创建格式化文件 (SQL Server)](../../relational-databases/import-export/create-a-format-file-sql-server.md)  
  
-   [使用格式化文件批量导入数据 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-bulk-import-data-sql-server.md)  
  
-   [使用格式化文件跳过表列 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)  
  
-   [使用格式化文件跳过数据字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-skip-a-data-field-sql-server.md)  
  
-   [使用格式化文件将表列映射到数据文件字段 (SQL Server)](../../relational-databases/import-export/use-a-format-file-to-map-table-columns-to-data-file-fields-sql-server.md)  
  
##  <a name="related-content"></a><a name="RelatedContent"></a> 相关内容  
 无。  
  
## <a name="see-also"></a>另请参阅  
 [批量导入和导出数据 (SQL Server)](../../relational-databases/import-export/bulk-import-and-export-of-data-sql-server.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [非 XML 格式化文件 (SQL Server)](../../relational-databases/import-export/non-xml-format-files-sql-server.md)   
 [用来导入或导出数据的格式化文件 (SQL Server)](../../relational-databases/import-export/format-files-for-importing-or-exporting-data-sql-server.md)  
  
  
