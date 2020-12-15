---
title: SQLXML 4.0 SP1 中的新增功能&#39;
description: 查看 SQLXML 4.0 SP1 中的更新和增强功能摘要，其中包含指向更详细信息的链接。
ms.custom: ''
ms.date: 03/17/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: conceptual
helpviewer_keywords:
- registry keys [SQLXML]
- SQLNCLI, SQLXML
- what's new [SQLXML]
- SQLXML, what's new
- migrating SQLXML applications
- redistributing SQLXML
- SQL Server Native Client, SQLXML
- side-by-side installations [SQLXML]
ms.assetid: 48f7720b-1705-402d-93ce-097ff1737877
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2665584ac6ac791ca489ea452779a93b71b35c68
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97479208"
---
# <a name="what39s-new-in-sqlxml-40-sp1"></a>SQLXML 4.0 SP1 中的新增功能&#39;
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  [!INCLUDE[msCoName](../../includes/msconame-md.md)] SQLXML 4.0 SP1 包括各种更新和增强功能。 本主题简要总结这些更新，并提供一些介绍详细信息的链接供您查看。 SQLXML 4.0 SP1 提供附加的增强功能，以便支持 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 中引入的新数据类型。 本主题包括以下内容：  
  
-   安装 SQLXML 4.0 SP1  
  
-   并行安装问题  
  
-   SQLXML 4.0 和 MSXML  
  
-   再分发 SQLXML 4.0  
  
-   支持 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client  
  
-   支持 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的数据类型  
  
-   SQLXML 4.0 的 XML 大容量加载更改  
  
-   SQLXML 4.0 的注册表项更改  
  
-   迁移问题  
  
## <a name="installing-sqlxml-40-sp1"></a>安装 SQLXML 4.0 SP1  
 在 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 之前，SQLXML 4.0 随 SQL Server 一起发布，是所有 SQL Server 版本（SQL Server Express 除外）的默认安装的一部分。 从 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 开始，SQL Server 中不再包括 SQLXML 的最新版本 (SQLXML 4.0 SP1)。 若要安装 SQLXML 4.0 SP1，请从 [sqlxml 4.0 sp1 的安装位置进行](https://www.microsoft.com/download/details.aspx?id=30403)下载。  
  
 SQLXML 4.0 SP1 文件安装在以下位置：  
  
 `%PROGRAMFILES%\SQLXML 4.0\`  
  
> [!NOTE]  
>  SQLXML 4.0 的所有相应注册表设置都在安装过程中进行。  
  
 若要允许 32 位 SQLXML 应用程序在 64 位 Windows 操作系统上的 Windows on Windows (WOW64) 下运行，请运行名为 sqlxml4.msi 的 64 位 SQLXML 4.0 SP1 包，该包可从下载中心找到。  
  
#### <a name="uninstalling-sqlxml-40-sp1"></a>卸载 SQLXML 4.0 SP1  
 在 SQLXML 3.0 SP3、SQLXML 4.0 和 SQLXML 4.0 SP1 之间存在共享的注册表项。 如果在包含 SQLXML 3.0 SP3 的计算机上卸载 SQLXML 的更高版本，则可能需要重新安装 SQLXML 3.0 SP3。  
  
## <a name="side-by-side-installation-issues"></a>并行安装问题  
 SQLXML 4.0 的安装过程不会删除由早期版本的 SQLXML 安装的文件。 因此，您的计算机上可能有适合不同 SQLXML 安装版本的多个 DLL 文件。 您可以运行并行安装。 SQLXML 4.0 既包括版本无关的 PROGID，也包括与版本相关的 PROGID。 所有生产应用程序都应该使用与版本相关的 PROGID。  
  
## <a name="sqlxml-40-sp1-and-msxml"></a>SQLXML 4.0 SP1 和 MSXML  
 SQLXML 4.0 不安装 MSXML。 SQLXML 4.0 使用 MSXML 6.0，它作为 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 或更高版本安装的一部分安装。  
  
## <a name="redistributing-sqlxml-40-sp1"></a>再发行 SQLXML 4.0 SP1  
 您可以使用可再发行安装程序包发行 SQLXML 4.0 SP1。 安装多个包（对于用户而言就像是一次安装）的一种方法就是使用链接器和引导程序技术。 有关详细信息，请参阅“Authoring a Custom Bootstrapper Package for Visual Studio 2005”（为 Visual Studio 2005 创作自定义引导程序包）和“添加自定义系统必备”。  
  
 如果您的应用程序所针对的目标平台并非其开发时所使用的平台，则可以从 Microsoft 下载中心下载针对 x64、Itanium 和 x86 的 sqlncli.msi 版本。  
  
 同时还有单独再分发的 MSXML 6.0 (msxml6.msi) 安装程序。 这些程序位于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 安装 CD 的以下位置：  
  
 `%CD%\Setup\`  
  
 这些安装文件可用于直接从 CD 安装 MSXML 6.0。 它们可用于随您的自定义应用程序一起自由地再分发 MSXML 6.0 以及 SQLXML 4.0 SP1。  
  
 如果您将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 作为数据访问接口与您的应用程序一起使用，则需要再分发它。 有关详细信息，请参阅 [安装 SQL Server Native Client](../../relational-databases/native-client/applications/installing-sql-server-native-client.md)。  
  
## <a name="support-for-sql-server-native-client"></a>支持 SQL Server Native Client  
 SQLXML 4.0 支持 SQLOLEDB 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 提供程序。 建议你使用相同版本的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native client 提供程序， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 因为 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 开发 native client 是为了支持服务器中随附的任何新数据类型，例如中的 **Date、Time**、 **DateTime2** 和 **dateTimeOffset** 数据类型， [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 以及 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] native client 支持的。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 是在 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 中引入的一种数据访问技术。 它将 SQLOLEDB 访问接口和 SQLODBC 驱动程序合并到一个本机动态链接库 (DLL)，同时还提供一项从 Microsoft 数据访问组件 (MDAC) 分离出来并有所不同的新功能。  
  
 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client 可用于创建新的应用程序，也可以用于增强需要利用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 引入的新功能但 MDAC 和 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Windows 中的 SQLOLEDB 和 SQLODBC 不支持的现有应用程序。 例如， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 若要使用 **xml** 数据类型，Native client 是客户端 SQLXML 功能（如 for XML）所必需的。 有关详细信息，请参阅 [客户端 XML 格式 &#40;SQLXML 4.0&#41;](../../relational-databases/sqlxml/formatting/client-side-xml-formatting-sqlxml-4-0.md)， [使用 ADO 执行 sqlxml 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)，并 [SQL Server Native Client 编程](../../relational-databases/native-client/sql-server-native-client-programming.md)。  
  
> [!NOTE]  
>  SQLXML 4.0 不完全向后兼容 SQLXML 3.0。 由于一些 bug 修复和其他功能更改，特别是删除了 SQLXML ISAPI 支持，您不能将 SQLXML 4.0 与 IIS 虚拟目录一起使用。 虽然大多数应用程序稍加修改即可运行，但将它们放到生产环境中随 SQLXML 4.0 一起运行之前，必须先进行测试。  
  
## <a name="support-for-data-types-introduced-in-sql-server-2005-and-sql-server-2008"></a>支持 SQL Server 2005 和 SQL Server 2008 中引入的数据类型  
 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 引入了 **xml** 数据类型，SQLXML 4.0 支持 **xml** 数据类型。 有关详细信息，请参阅 [SQLXML 4.0 中的 Xml 数据类型支持](../../relational-databases/sqlxml/xml-data-type-support-in-sqlxml-4-0.md)。  
  
 有关如何在映射 XML 视图、大容量加载 XML 或执行 XML updategram 时使用 SQLXML 中的 **xml** 数据类型的示例，请参阅以下主题中提供的示例。  
  
-   [XSD 元素和属性到表和列的默认映射](../../relational-databases/sqlxml-annotated-xsd-schemas-using/default-mapping-of-xsd-elements-and-attributes-to-tables-and-columns-sqlxml-4-0.md)  
  
-   [通过使用 XML Updategram 插入数据](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/updategrams/inserting-data-using-xml-updategrams-sqlxml-4-0.md)  
  
-   [大容量加载 XML 文档的示例](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/xml-bulk-load-examples-sqlxml-4-0.md)  
  
 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 引入了 **Date、Time**、 **DateTime2** 和 **DateTimeOffset** 数据类型。 如果将 SQLXML 4.0 SP1 与 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] 中随附的 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] Native Client OLE DB Provider (SQLNCLI11) 一起使用，SQLXML 4.0 SP1 会将这四种新数据类型作为内置标量类型来使用。  
  
## <a name="xml-bulk-load-changes-for-sqlxml-40-sp1"></a>SQLXML 4.0 SP1 的 XML 大容量加载更改  
  
-   对于 SQLXML 4.0，SchemaGen 溢出字段是使用 **xml** 数据类型创建的。 有关详细信息，请参阅 [SQL SERVER XML 大容量加载对象模型](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/sql-server-xml-bulk-load-object-model-sqlxml-4-0.md)。  
  
-   如果您以前创建了 [!INCLUDE[msCoName](../../includes/msconame-md.md)] Visual Basic 应用程序，现在想使用 SQLXML 4.0，则必须引用 Xblkld4.dll 重新编译该应用程序。  
  
-   对于 Visual Basic Scripting Edition 应用程序，则必须注册想要使用的 DLL。 在下例中，如果您指定版本无关的 PROGID，应用程序将取决于最后注册的 DLL：  
  
    ```  
    set objBulkLoad = CreateObject("SQLXMLBulkLoad.SQLXMLBulkLoad")   
    ```  
  
    > [!NOTE]  
    >  与版本相关的 PROGID 是 SQLXMLBulkLoad.SQLXMLBulkLoad.4.0。  
  
## <a name="registry-key-changes-for-sqlxml-40"></a>SQLXML 4.0 的注册表项更改  
 在 SQLXML 4.0 中，注册表项已从早期版本更改为以下项：  
  
 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\TemplateCacheSize  
  
 HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\MSSQLServer\Client\SQLXML4\SchemaCacheSize  
  
 如果想让这些项在 SQLXML 4.0 中生效，则必须更改设置。  
  
 此外，SQLXML 4.0 还引入了以下注册表项：  
  
-   `HKEY_LOCAL_MACHINE\Software\Microsoft\MSSQLServer\Client\SQLXML4\ReportErrorsWithSQLInfo`  
  
     默认情况下，SQLXML 4.0 返回 OLE DB 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 提供的本机错误信息，而不是高级 SQLXML 错误（与早期版本的 SQLXML 一样）。 如果不想出现此行为，DWORD 类型的注册表项值必须设置为 0（默认值为 1）。  
  
-   HKEY_LOCAL_MACHINE\Software\Microsoft\MSSQLServer\Client\SQLXML4\FORXML_GenerateGUIDBraces  
  
     默认情况下，SQLXML 返回 SQL Server GUID 值，并且没有括号。 如果希望用大括号返回的 GUID 值 (例如，{*某个 GUID*} ) ，则必须将此注册表项的值设置为 1 (默认值为 0) 。  
  
-   HKEY_LOCAL_MACHINE\Software\Microsoft\MSSQLServer\Client\SQLXML4\SQL2000CompatMode  
  
     默认情况下，XML 分析器加载数据时，会根据 XML 1.0 规则规范化空格。 这会导致数据中的一些空格字符丢失。 因此，数据的文字表示在分析之后可能会不一样，尽管在语义上数据是一样的。  
  
     引入该项以便您可以选择是否在数据中保留空格字符。 如果添加此注册表项并将其值设置为 0，则对于 XML 中的属性值，将返回已编码的空格字符（LF、CR 和制表符）。 对于元素值，只返回已编码的 CR。  
  
     例如：  
  
    ```  
    CREATE TABLE T( Col1 int, Col2 nvarchar(100));  
    GO  
    -- Insert data with tab, line feed and carriage return).  
    INSERT INTO T VALUES (1, 'This is a tab    . This is a line feed and CR   
     more text');  
    GO  
    -- Test this query (without the registry key).  
    SELECT * FROM T   
    FOR XML AUTO;  
    -- This is the result (no encoding of special characters).  
    <?xml version="1.0" encoding="utf-8" ?>  
    <r>  
      <T Col1="1"   
         Col2="This is a tab    . This is a line feed and CR   
     more text"/>  
    </r>  
    -- Now add registry key with value 0 and execute the query again.  
    -- Note the encoding for carriage return, line-feed and tab in the attribute value.  
    <?xml version="1.0" encoding="utf-8" ?>  
    <r>  
      <T Col1="1"   
         Col2="This is a tab    . This is a line feed and CR   
     more text"/>  
    </r>  
  
    -- Update the query and specify ELEMENTS directive  
    SELECT * FROM T  
    FOR XML AUTO, ELEMENTS  
    -- Only the carriage return is returned encoded.  
    <?xml version="1.0" encoding="utf-8" ?>  
    <r>  
       <T>  
          <Col1>1</Col1>  
          <Col2>This is a tab    . This is a line feed and CR   
     more text</Col2>  
       </T>  
    </r>  
    ```  
  
## <a name="migration-issues"></a>迁移问题  
 下列问题可能影响您将旧版 SQLXML 应用程序迁移到 SQLXML 4.0。  
  
### <a name="ado-and-sqlxml-40-queries"></a>ADO 和 SQLXML 4.0 查询  
 在 SQLXML 的早期版本中，使用 IIS 虚拟目录和 SQLXML ISAPI 筛选器支持基于 URL 的查询执行。 对于使用 SQLXML 4.0 的应用程序来说，此支持不再可用。  
  
 取而代之的是可以通过在 Microsoft 数据访问组件 (MDAC) 2.6 中首次引入并在后期版本中延续使用的 ActiveX 数据对象 (ADO) 的 SQLXML 扩展，执行 SQLXML 查询、模板和 updategram。  
  
 有关详细信息，请参阅 [使用 ADO 执行 SQLXML 4.0 查询](../../relational-databases/sqlxml/using-ado-to-execute-sqlxml-4-0-queries.md)。  
  
### <a name="supportability-for-sqlxml-30-isapi-and-data-types-introduced-in-sql-server-2005"></a>SQL Server 2005 中引入的 SQLXML 3.0 ISAPI 和数据类型的可支持性  
 由于 ISAPI 支持已从 SQLXML 4.0 中移除，因此，如果您的解决方案需要中引入的增强型数据类型功能 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] （如 [xml 数据类型](../../t-sql/xml/xml-transact-sql.md) 或 [用户定义数据类型 (Udt) ](../../relational-databases/clr-integration-database-objects-user-defined-types/clr-user-defined-types.md) 和基于 Web 的访问），则需要使用其他解决方案（如 [SQLXML 托管类](../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/net-framework-classes/sqlxml-4-0-net-framework-support-managed-classes.md) 或其他类型的 HTTP 处理程序，如 SQL Server 2005 的本机 xml Web 服务）。  
  
 或者，如果不需要这些类型扩展，可以继续使用 SQLXML 3.0 连接到 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 和 [!INCLUDE[ssKatmai](../../includes/sskatmai-md.md)] 安装。 SQLXML 3.0 ISAPI 支持将针对这些更高版本工作，但不支持或识别中引入的 **xml** 数据类型或 UDT 类型支持 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 。  
  
### <a name="xml-bulk-load-security-changes-for-temporary-files"></a>临时文件的 XML 大容量加载安全更改  
 对于 SQLXML 4.0 和 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]，XML 大容量加载文件的权限授予执行大容量加载操作的用户。 读写权限都继承自文件系统。 在早期版本的 SQLXML 和 SQL Server 中，SQLXML 下的 XML 大容量加载将创建一些任何人都可读的不安全的临时文件。  
  
### <a name="migration-issues-for-client-side-for-xml"></a>客户端 FOR XML 迁移问题  
 由于执行引擎中的更改， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 如果在下执行 FOR XML 查询，则可能会在基表的元数据中返回不同的值 [!INCLUDE[ssVersion2000](../../includes/ssversion2000-md.md)] 。 如果发生这种情况，FOR XML 查询结果的客户端格式设置将有不同的输出，具体因运行查询的版本而异。  
  
 如果 FOR XML 查询是使用 SQLXML 3.0 在 **XML** 数据类型列上执行的，则结果中的数据将作为完全实体化的字符串返回。 在 SQLXML 4.0 中，如果将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) 指定为访问接口，数据将以 XML 返回。  
  
  
