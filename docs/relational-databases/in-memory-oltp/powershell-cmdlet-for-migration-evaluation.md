---
title: 用于迁移评估的 PowerShell Cmdlet | Microsoft Docs
description: 了解 Save-SqlMigrationReport，它将评估 SQL Server 数据库中对象针对内存中 OLTP 的迁移适用性。
ms.custom: ''
ms.date: 07/30/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 117250d3-9982-47fe-94fd-6f29f6159940
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 522045cafa1effd04dae5b20193089ea34b4145e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97485239"
---
# <a name="powershell-cmdlet-for-migration-evaluation"></a>用于迁移评估的 PowerShell Cmdlet

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

`Save-SqlMigrationReport` 工具可评估 SQL Server 数据库中多个对象的迁移适用性。

目前，此 cmdlet 仅限于评估内存中 OLTP 的迁移适用性。 该 cmdlet 可以在提升的 Windows PowerShell 环境和 sqlps 中运行。

作为直接运行此 PowerShell cmdlet 的替代方法，可以使用 SQL Server Management Studio (SSMS) 隐式运行此 cmdlet。 在 SSMS“对象资源管理器”中，可以右键单击某个表，然后单击“内存优化顾问”。

## <a name="syntax"></a>语法

```
Save-SqlMigrationReport
    -FolderPath <output_path>
    [ -MigrationType <migration_scenario_type> ]
    [
        [ -Server <server_name> -Database <database_name>
            [ -Schema <schema_name> ] [ -Object <object_name> ]
        ]
       |
        [ -InputObject <smo_object> ]
    ]
;
```

## <a name="parameters"></a>参数

下表对这些参数进行了说明。

有一些语法方面的内容应进行强调。 如果指定参数 `-InputObject`，则不能指定以下任何参数：

- `-Server`
- `-Database`
- `-Schema`
- `-Object`

反之，如果未指定 `-InputObject`，则必须指定 `-Server` 和 `-Database`。 如果指定 `-Server`，则可以选择通过指定 `-Schema` 或 `-Object` 或是同时指定两者来缩小范围。

| 参数名称 | 说明 |
| :------------- | :---------- |
| 数据库 | 目标 SQL Server 数据库的名称。 强制（当 `-Server` 为强制参数时）。<br/><br/> 在 SQLPS 中为可选。 |
| FolderPath | 该 cmdlet 应在其中存储所生成的报告的文件夹。<br/><br/> 必需。 |
| InputObject | 该 cmdlet 设定为目标的 SMO 对象。<br/><br/> 如果未提供 `-Server`，则在 Windows Powershell 环境中为强制参数。<br/><br/> 在 SQLPS 中为可选。 |
| MigrationType | 该 cmdlet 设定为目标的迁移方案的类型。 目前，唯一的值是默认“OLTP”。<br/><br/> 可选。 |
| 对象 | 要报告的对象的名称。 可以是表或存储过程。 |
| 密码 | 必需（当 `-Username` 是必需参数时）。 |
| 架构 | 拥有要报告的对象的架构的名称。<br/><br/> 可选。 |
| 服务器 | 目标 SQL Server 实例的名称。 如果未提供 `-InputObject` 参数，则在 Windows Powershell 环境中为强制参数。<br/><br/> 在 SQLPS 中为可选。 |
| 用户名 | 当通过 SQL Server 身份验证（而不是 Windows 身份验证）进行连接时是必需的。 否则省略。 |
| &nbsp; | &nbsp; |

## <a name="prerequisites"></a>先决条件

必须先安装名为 SqlServer 的模块，然后才能运行此 cmdlet：

- `Install-Module -Name SqlServer`

> [!NOTE]
> 不再维护旧 `SQLPS` 模块。 使用较新的 `SqlServer` 模块。

有关详细信息，请参阅[安装 SQL Server PowerShell 模块](../../powershell/download-sql-server-ps-module.md)。

## <a name="example-cmdlet-line"></a>示例 cmdlet 行

接下来是运行的实际 cmdlet 行，用于生成在本文后面显示的报告。

```powershell
Save-SqlMigrationReport `
  -FolderPath 'C:\Test\PowerShell-ps1\Save-SqlMigrationReport\' `
  -Server 'MyUserName123456.database.windows.net' `
  -Database 'MyDatabaseName_31' `
  -Schema 'dbo' `
  -Object 'Table2' `
  -Username 'MyUserName' `
  -Password 'MyPassword' `
  -MigrationType 'OLTP' `
;
```

## <a name="example-output-report"></a>示例输出报告

在为 `-FolderPath` 参数指定的文件夹下，通过运行此 cmdlet 来创建以下两个文件夹路径。 这两个路径都以 server\_name 值开头：

- MyDatabaseName_31\Tables\
- MyDatabaseName_31\Stored Procedures\

每个对象报告文件都存储在相应的文件夹下。

报告文件名具有扩展名 .html。 例如，实际生成的文件名为 MigrationAdvisorChecklistReport_Table2_20190728.html。

该 HTML 主要是具有以下标头的由两列组成的表：

- 说明
- 验证结果

接下来是一个表的 HTML 报告的实际示例。

```html
<?xml version="1.0" encoding="utf-8"?>
<html>
  <head>
    <title>Memory optimization checklist for [MyDatabaseName_31].[Table2]</title>
  </head>
  <body>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 14pt;">
      <b>Memory optimization checklist for [MyDatabaseName_31].[Table2]</b>
    </p>
    <p STYLE="font-family: Verdana, Arial, sans-serif; font-size: 10pt;">
      <b>Report Date/Time:</b>7/28/2019 2:25 PM<br /></p>
    <table border="1" cellpadding="5" cellspacing="0" STYLE="font-family: Verdana, Arial, sans-serif; font-size: 9pt;">
      <tr style="background-color:Silver">
        <th colspan="2" align="center">Description</th>
        <th align="center">Validation Result</th>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported data types are defined on this table. </td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No sparse columns are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No identity columns with unsupported seed and increment are defined for this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No foreign key relationships are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported constraints are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">No unsupported indexes are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">No unsupported triggers are defined on this table.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top" style="background-color:LightYellow">
        <td colspan="2">Post migration row size does not exceed the row size limit of memory-optimized tables.</td>
        <td>Succeeded</td>
      </tr>
      <tr valign="top">
        <td colspan="2">Table is not partitioned or replicated.</td>
        <td>Succeeded</td>
      </tr>
    </table>
  </body>
</html>
```

接下来是表的大致内容。

| 说明 | 验证结果 |
| :---------- | :---------------- |
| 此表未定义不支持的数据类型。 | 已成功 |
| 此表未定义稀疏列。 | 已成功 |
| 此表未定义具有不支持的种子和增量的标识列。 | 已成功 |
| 此表未定义外键关系。 | 已成功 |
| 此表未定义不支持的约束。 | 已成功 |
| 此表未定义不支持的索引。 | 已成功 |
| 此表未定义不支持的触发器。 | 已成功 |
| 迁移后行大小未超过内存优化表的行大小限制。 | 已成功 |
| 表未分区或复制。 | 已成功 |
| &nbsp; | &nbsp; |

## <a name="related-links"></a>相关链接

- 参考文档：[Save-SqlMigrationReport](/powershell/module/sqlserver/save-sqlmigrationreport?view=sqlserver-ps)
