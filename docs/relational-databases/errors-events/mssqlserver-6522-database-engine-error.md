---
description: MSSQLSERVER_6522
title: MSSQLSERVER_6522
ms.custom: ''
ms.date: 12/25/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, vencher, tejasaks, docast
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 6522 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 836ad79e049d7c5613755e64ca441f9ed5d73048
ms.sourcegitcommit: d819173fb91af6f20ca6ee59686c35c71b060fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/28/2020
ms.locfileid: "97797794"
---
# <a name="mssqlserver_6522"></a>MSSQLSERVER_6522
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|6522|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|SQLCLR_UDF_EXEC_FAILED|
|消息正文|在执行用户定义例程或聚合“%.*ls”期间出现 .NET Framework 错误：%ls。|
||

## <a name="explanation"></a>说明

请考虑以下方案。

### <a name="scenario-1"></a>方案 1

创建一个引用 Microsoft .NET Framework 程序集的公共语言运行时 (CLR) 例程。 [922672](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies) 中未介绍 .NET Framework 程序集。 然后，安装 .NET Framework 3.5 或基于 .NET Framework 2.0 的修补程序。

### <a name="scenario-2"></a>方案 2

创建一个程序集，然后在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据库中注册该程序集。 然后，在全局程序集缓存 (GAC) 中安装程序集的不同版本。

在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中执行 CLR 例程或使用上述任一方案中的程序集时，会收到类似于以下内容的错误消息：

> 服务器:消息 6522，级别 16，状态 2，行 1  
在执行用户定义例程或聚合“getsid”期间出现 .NET Framework 错误：
>
> System.IO.FileLoadException：无法加载文件或程序集“System.DirectoryServices, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a”或其依赖项之一。 主机存储区中的程序集与 GAC 中的程序集的签名不同。 （异常来自 HRESULT：0x80131050)

## <a name="possible-cause"></a>可能的原因

当 CLR 加载程序集时，CLR 将验证 GAC 中是否有相同的程序集。 如果 GAC 中有相同的程序集，则 CLR 将验证这些程序集的模块版本 ID (MVID) 是否匹配。 如果这些程序集的 MVID 不匹配，你将收到[说明](#explanation)部分提及的错误消息。

重新编译程序集时，程序集的 MVID 会发生变更。 因此，如果更新 .NET Framework，则 .NET Framework 程序集具有不同的 MVID，因为这两个程序集将被重新编译。 此外，如果更新你自己的程序集，也会重新编译该程序集。 因此，该程序集也会有不同的 MVID。

## <a name="user-action"></a>用户操作

### <a name="action-1"></a>Action1

若要解决[说明](#explanation)部分中的方案 1，必须手动更新 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中的 .NET Framework 程序集。 为此，请使用 `ALTER ASSEMBLY` 语句指向以下文件夹中 .NET Framework 程序集的新版本：

`%Windir%\Microsoft.NET\Framework\Version`

> [!NOTE]
> 版本表示你安装或更新的 .NET Framework 版本。

### <a name="action-2"></a>Action 2

若要解决[说明](#explanation)部分中的方案 2，请使用 `ALTER ASSEMBLY` 语句来更新数据库中的程序集。

如果执行此操作后问题仍然存在，请从数据库中删除该程序集，然后在数据库中注册该程序集的新版本。

## <a name="more-information"></a>详细信息

不建议使用 .NET Framework 程序集，这些程序集未在 [SQL Server CLR 托管环境中未经测试的 .NET Framework 程序集的支持策略](/troubleshoot/sql/database-design/policy-untested-net-framework-assemblies)中作记录。 它列出了在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] CLR 托管环境中测试的程序集。

### <a name="description-of-clr-routines"></a>CLR 例程说明

CLR 例程包含以下对象，这些对象是通过使用 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 与 .NET Framework CLR 的集成来实现的：

- 标量值用户定义函数（标量 UDF）
- 表值用户定义函数 (TVF)
- 用户定义的过程 (UDP)
- 用户定义的触发器
- 用户定义数据类型
- 用户定义聚合

### <a name="assemblies-to-update-after-you-install-the-net-framework-35"></a>安装 .NET Framework 3.5 后要更新的程序集

安装 .NET Framework 3.5 后，必须使用 ALTER ASSEMBLY 语句来更新以下程序集：

- Accessibility.dll
- AspNetMMCExt.dll
- Cscompmgd.dll
- IEExecRemote.dll
- IEHost.dll
- IIEHost.dll
- Microsoft.Build.Conversion.dll
- Microsoft.Build.Engine.dll
- Microsoft.Build.Framework.dll
- Microsoft.Build.Tasks.dll 
- Microsoft.Build.Utilities.dll 
- Microsoft.CompactFramework.Build.Tasks.dll 
- Microsoft.JScript.dll 
- Microsoft.VisualBasic.Vsa.dll 
- Microsoft.Vsa.dll 
- Microsoft.Vsa.Vb.CodeDOMProcessor.dll 
- Microsoft_VsaVb.dll 
- Sysglobl.dll 
- System.Configuration.Install.dll 
- System.Design.dll 
- System.DirectoryServices.dll 
- System.DirectoryServices.Protocols.dll 
- System.Drawing.dll 
- System.Drawing.Design.dll 
- System.EnterpriseServices.dll 
- System.Management.dll 
- System.Messaging.dll 
- System.Runtime.Serialization.Formatters.Soap.dll 
- System.ServiceProcess.dll 
- System.Web.dll 
- System.Web.Mobile.dll 
- System.Web.RegularExpressions.dll 

这些程序集位于以下文件夹中：

`%Windir%\Microsoft.NET\Framework\v2.0.50727`

### <a name="how-to-preserve-the-data-from-user-defined-data-types-after-you-drop-an-assembly"></a>删除程序集后如何保留用户定义数据类型中的数据

如果删除来自 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的用户定义数据类型使用的程序集，则可以使用以下方法之一来保留数据。

假定场景如下：

- 你创建了一个名为“MyAssembly.dll”的程序集。
- MyAssembly 程序集引用 `System.DirectoryServices.dll` 程序集。
- 用户定义的数据类型名称为“MyDateTime”。
- MyDateTime 数据类型使用 MyAssembly.dll 程序集。
- 创建的表名称为“MyTable”。
- MyTable 表包含 MyDateTime 数据类型的数据。

#### <a name="method-1-use-the-bcpexe-utility"></a>方法 1：使用 bcp.exe 实用工具

1. 将 Bcp.exe 实用工具与 -n 开关一起使用，以将 MyTable 表中的数据复制到文件中。 例如，在命令提示符下运行以下命令：

    ```console
    bcp MyDatabase.dbo.MyTable out C:\MyFile.bcp -n -SSQLServerName -T
    ```

2. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 中，执行以下步骤：
   1. 删除 MyTable 表。
   2. 删除 MyDateTime 数据类型。
   3. 删除 `System.DirectoryServices.dll` 程序集。
   4. 删除 MyAssembly 程序集。
3. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 中，执行以下步骤：

   1. 注册 `System.DirectoryServices.dll` 程序集。
   2. 注册 MyAssembly 程序集。
   3. 创建 MyDateTime 数据类型。
   4. 创建一个新表，该表与 MyTable 表具有相同的表结构。
4. 将 Bcp.exe 实用工具与 -n 开关一起使用，以将数据从文件导入到 MyTable 表。 例如，在命令提示符下运行以下命令：
  
    ```console
    bcp MyDatabase.dbo.MyTable in C:\MyFile.bcp -n -SSQLServerName -T
    ```

#### <a name="method-2-use-the-insert--select-statement"></a>方法 2：使用 INSERT ...SELECT 语句

假设 MyDateTime 数据类型在存储中占用 9 个字节。

1. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 中，通过运行以下语句来创建包含 `VARBINARY(9)` 数据类型的列的新表：

    ```sql
    CREATE TABLE TempTable (c1 VARBINARY(9));
    ```

2. 运行以下 INSERT ...SELECT 语句以填充 TempTable 表：

    ```sql
    INSERT INTO TempTable SELECT CAST(c1 as VARBINARY(9)) FROM MyTable;
    ```

3. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 中，执行以下步骤：
   1. 删除 MyTable 表。
   2. 删除 MyDateTime 数据类型。
   3. 删除 System.DirectoryServices.dll 程序集。
   4. 删除 MyAssembly 程序集。
4. 在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Management Studio 中，执行以下步骤：
   1. 注册 System.DirectoryServices.dll 程序集。
   2. 注册 MyAssembly 程序集。
   3. 创建 MyDateTime 数据类型。
   4. 创建一个新表，该表与 MyTable 表具有相同的表结构。
5. 运行以下 INSERT ...SELECT 语句以填充 MyTable 表：

    ```sql
    INSERT INTO MyTable SELECT c1 FROM TempTable;
    ```

## <a name="references"></a>参考

- 有关程序集版本的详细信息，请参阅 [Visual Studio 2005 已停用的文档](https://www.microsoft.com/download/details.aspx?id=55984)。
- 有关如何更新程序集的详细信息，请参阅 [ALTER ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/alter-assembly-transact-sql)。
- 有关如何删除程序集的详细信息，请参阅 [DROP ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/drop-assembly-transact-sql)。
- 有关如何在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 中注册程序集的详细信息，请参阅 [CREATE ASSEMBLY (Transact-SQL)](/sql/t-sql/statements/create-assembly-transact-sql)。
- 有关 Bcp.exe 实用工具的详细信息，请参阅 [https://msdn2.microsoft.com/library/ms162802.aspx](/sql/tools/bcp-utility)。
