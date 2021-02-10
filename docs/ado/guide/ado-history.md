---
description: 每个版本的 ADO 功能
title: ADO 历史记录 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2019
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.topic: conceptual
helpviewer_keywords:
- ADO, what's new
ms.assetid: 667673f2-3151-432b-894a-3fc60b704ea4
author: rothja
ms.author: jroth
ms.openlocfilehash: 7247523daf68116c949d4ec8ad3eec90fe4c8fe2
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100030615"
---
# <a name="ado-features-for-each-release"></a>每个版本的 ADO 功能

本主题列出了每个版本的 ADO、ADO MD 和 ADOX 引入的新功能。

## <a name="ado-60"></a>ADO 6。0

ADO 6.0 包含在 windows Vista 中，作为 windows (Windows DAC) 6.0 的 Windows 数据访问组件的一部分。 ADO 6.0 在功能上等同于 ADO 2.8。

## <a name="ado-28"></a>ADO 2。8

ADO 2.8 包含在 Windows XP 和 Windows Server 2003 中，作为 Microsoft 数据访问组件的一部分 (MDAC) 2.8。 还提供了 MDAC 2.8 的可再发行版本;请注意，此可再发行版本只能安装在 Windows 2000 上。 ADO 2.8 解决了几个与安全相关的问题：

*不允许在受信任区域外部进行硬盘驱动器访问。*
在涉及不信任站点的跨域脚本中，禁用了以下操作： **SaveToFile**、 **LoadFromFile**、 **recordset、Save** 和 **recordset。 Open**，与 **AdCmdFile** 标志一起使用，或者与 Microsoft OLE DB 永久性提供程序一起使用 (MSPersist) 。

**Recordset. Open** _、_  **recordset** _、_  **SaveToFile** _和_  **LoadFromFile**  _仅对物理文件执行操作。_
这些方法现在会验证文件句柄是否仅指向物理文件。

**ActiveCommand**  _从 HTML/ASP 页调用时返回错误。_
这会阻止 **命令** 对象被滥用。

_嵌套_**形状** 命令返回 _的_**记录集** 数 _有上限。_        
嵌套的 shape 命令现在最多返回512个 **记录集**。 这意味着不能再在任何深度上嵌套 **Shape** 命令。 相反，如果每个命令产生单个 (子) **记录集**，则最大级别深度为512。 如果任何级别的 **Shape** 命令返回多个 **记录集**，则最大深度级别将小于512。

## <a name="ado-27"></a>ADO 2。7

*64 位平台支持* ADO 2.7 引入了对64位处理器的支持。

## <a name="ado-26"></a>ADO 2。6

**CubDef GetSchemaObject**  _方法_ 从 ADO 2.6 开始，可使用唯一名称检索 ADO MD 对象，如 UniqueName 属性指定的名称 [ (ADO MD)](../reference/ado-md-api/uniquename-property-ado-md.md)。 父对象的名称无需知道，并且无需填充父集合即可检索架构对象。 请参阅 [GetSchemaObject 方法 (ADO MD) ](../reference/ado-md-api/getschemaobject-method-ado-md.md)。

*命令流***Command** 对象支持流格式的命令，作为使用 **CommandText** 属性的替代方法。 [ (ADO) 的 CommandStream 属性](../reference/ado-api/commandstream-property-ado.md)可用于将 XML 模板或 updategram 指定为用于 SQL Server 的 Microsoft OLE DB 提供程序的 **命令** 输入。

**方言**_属性_ 
 [方言](../reference/ado-api/dialect-property.md)是一个新的属性，用于定义提供程序用于分析字符串或流的语法和一般规则。  

**Command.Exe** 一种非常好的 _方法_： ADO **命令** 对象的 [Execute 方法](../reference/ado-api/execute-method-ado-command.md)已得到增强，可以使用流进行输入和输出。  

*字段 statusvalues* 如果用户在修改 **记录集** 的 **字段** 时遇到 DB_E_ERRORSOCCURRED 错误，则 ADO 现在将使用适当的状态信息填充 "**字段" 字段。状态** 属性，以便用户可以获得有关发生错误的详细信息。 请参阅 [状态属性 (ADO 字段) ](../reference/ado-api/status-property-ado-field.md)。

**NamedParameters**_属性_ 
 [NamedParameters](../reference/ado-api/namedparameters-property-ado.md)是 **命令** 对象的新属性，指示提供程序应使用命名参数。  

*流中的结果* 集ADO 可以从 **流** 中的数据源（而不是 **记录集** 对象）返回结果集。 使用最新版本的 Microsoft OLE DB Provider for SQL Server，你可以通过执行 "For XML" 查询从提供程序中获取 XML 结果。 可以使用 "For XML" 命令作为源来打开接收结果集的 **流** 。 请参阅 [检索流中的结果集](./data/retrieving-resultsets-into-streams.md)。

*单行结果集* 现在，可以在命令字符串或 **命令** 对象上打开 ADO **记录** 对象，该对象可从提供程序返回一行数据。 这会提高 MDAC 2.6 提供程序的性能。 请参阅 [Open Method (ADO Record) ](../reference/ado-api/open-method-ado-record.md)。

## <a name="ado-25"></a>ADO 2。5

**记录**_对象_ ADO 2.5 引入了 **record** 对象，用于表示和管理 **记录集** 或数据提供程序中的行，或封装半结构化数据（如文件或目录）的对象。

**流**_对象_ ADO 2.5 还引入了 *和* stream * * 对象来表示二进制或文本数据的流。

*URL 绑定* ADO 2.5 介绍了如何使用 URL 作为连接字符串和命令文本的替代方法来命名数据存储对象。 URL 可以与现有 **连接** 和 **记录集** 对象以及新的 **记录** 和 **流** 对象结合使用。

*支持 URL 绑定的数据访问接口* ADO 2.5 支持可识别 URL 方案 OLE DB 提供程序。 这包括用于 Internet 发布的 OLE DB 提供程序，可访问 Windows 2000 文件系统并识别现有的 HTTP 方案。
