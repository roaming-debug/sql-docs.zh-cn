---
description: SSMS 实用工具
title: SSMS 实用工具
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], opening
- command prompt utilities [SQL Server], sqlwb
- sqlwb utility
- Management Studio command line
- opening SQL Server Management Studio
ms.assetid: aafda520-9e2a-4e1e-b936-1b165f1684e8
author: markingmyname
ms.author: maghan
ms.reviewer: ''
ms.custom: seo-lt-2019
ms.date: 07/24/2020
ms.openlocfilehash: 102a07f1b26b8c1e31807227686dd8f1f6856237
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100344186"
---
# <a name="ssms-utility"></a>SSMS 实用工具

[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

SSMS 实用工具打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。 如果指定， **Ssms** 还可以与服务器建立连接并打开查询、脚本、文件、项目和解决方案。

可以指定包含查询、项目或解决方案的文件。 如果提供了连接信息并且文件类型与服务器类型关联，则包含查询的文件将自动连接到该服务器。 例如，.sql 文件在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开一个 SQL 查询编辑器窗口，.mdx 文件在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开一个 MDX 查询编辑器窗口。 SQL Server 解决方案和项目  在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开。

> [!NOTE]
> **Ssms** 实用工具不能运行查询。 若要从命令行运行查询，请使用 **sqlcmd** 实用工具。 

## <a name="syntax"></a>语法

```syntaxsql
Ssms
[scriptfile] [projectfile] [solutionfile] 
[-S servername] [-d databasename] [-G] [-U username] [-E] [-nosplash] [-log [filename]?] [-?] 
```

## <a name="arguments"></a>参数

scriptfile 指定一个或多个要打开的脚本文件  。 该参数必须包含文件的完整路径。 

projectfile 指定要打开的脚本项目  。 该参数必须包含脚本项目文件的完整路径。 

solutionfile 指定要打开的解决方案  。 该参数必须包含解决方案文件的完整路径。 

[ **-S** _servername_] 服务器名称

[ **-d** _databasename_] 数据库名称

[-G  ] 使用 Active Directory 身份验证进行连接。 连接类型的确定取决于是否包含 -U  。

> [!Note]
> 当前不支持“Active Directory - 含 MFA 支持的通用身份验证”  。

[ **-U** _username_] 通过“SQL 身份验证”进行连接时的用户名

> [!Note]
> SSMS 版本 18.0 中删除了 -P。
>
> 解决方法：尝试使用 UI 一次连接到服务器，并保存密码。

[-E] 使用 Windows 身份验证进行连接

[-nosplash] 阻止 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 在打开时显示初始屏幕。 在带宽有限的情况下，通过终端服务连接到运行 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 的计算机上时，使用此选项。 该参数不区分大小写，并且可放在其他参数前后

[-log[filename]?] 将 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 活动记录到指定的文件中以便进行故障排除

[-?] 显示命令行帮助

## <a name="remarks"></a>备注

上述所有开关都是可选的，并用空格分隔，但文件用逗号分隔。 如果不指定任何开关，则 **Ssms** 将按照“工具”菜单的“选项”设置中指定的方式打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]。 例如，如果“环境/常规”页的“启动时”选项指定了“打开新查询窗口”，Ssms 则会在打开时显示一个空白查询编辑器。

-log 开关必须出现在命令行的末尾，位于所有其他开关的后面。 文件名参数是可选的。 如果指定了一个文件名，并且该文件不存在，则创建该文件。 如果无法创建该文件（例如，由于没有足够的写访问权限），日志将改为写入非本地化的 APPDATA 位置（见下文）。 如果未指定该文件名参数，则两个文件将写入当前用户的非本地化的应用程序数据文件夹。 SQL Server 的非本地化的应用程序数据文件夹可以从 APPDATA 环境变量中找到。 例如，对于 SQL Server 2012，文件夹为 \<system drive>:\Users\\<username\>\AppData\Roaming\Microsoft\AppEnv\10.0\\。 默认情况下，这两个文件分别命名为 ActivityLog.xml 和 ActivityLog.xsl。 前者包含活动日志数据，后者是一种 XML 样式表，提供了更方便的方法来查看 XML 文件。 按照下列步骤操作，在默认 XML 查看器（如 Internet Explorer）中查看日志文件：依次单击“开始”和“运行...”，在提供的字段中键入“\<system drive>:\Users\\<username\>\AppData\Roaming\Microsoft\AppEnv\10.0\ActivityLog.xml”，再按 Enter。

如果提供了连接信息并且文件类型与服务器类型关联，则包含查询的文件会立即连接到该服务器。 例如，.sql 文件在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开一个 SQL 查询编辑器窗口，.mdx 文件在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开一个 MDX 查询编辑器窗口。 SQL Server 解决方案和项目 在 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] 中打开。

下表列出了与服务器类型相对应的文件扩展名。

| 服务器类型 | 分机 |
|-------------|-----------|
|[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)]|.sql|
|SQL Server Analysis Services|.mdx<br /><br /> .xmla|

## <a name="examples"></a>示例

以下脚本将在命令指示符下使用默认设置打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ：

```console
  Ssms
```

以下脚本在命令提示符下使用“Active Directory - 集成”打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)]：

```console
Ssms.exe -S servername.database.windows.net -G
```

以下脚本将在命令指示符下，使用 Windows 身份验证打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ，其中代码编辑器设置为 `ACCTG and the database AdventureWorks2012,` ，并且不显示初始屏幕：

```console
Ssms -E -S ACCTG -d AdventureWorks2012 -nosplash
```

以下脚本将在命令指示符下打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ，并打开 MonthEndQuery 脚本。

```console
Ssms "C:\Documents and Settings\username\My Documents\SQL Server Management Studio Projects\FinanceScripts\FinanceScripts\MonthEndQuery.sql"
```

以下脚本在命令指示符下打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ，并打开名为 `developer`的计算机上的 NewReportsProject 项目：

```console
Ssms "\\developer\fin\ReportProj\ReportProj\NewReportProj.ssmssqlproj"
```

以下脚本在命令提示符下打开 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] ，并打开 MonthlyReports 解决方案： 

```console
Ssms "C:\solutionsfolder\ReportProj\MonthlyReports.ssmssln"
```

## <a name="see-also"></a>另请参阅

[使用 SQL Server Management Studio](./sql-server-management-studio-ssms.md)