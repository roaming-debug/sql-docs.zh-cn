---
title: 将 sqlcmd 与脚本变量结合使用
description: 了解如何使用脚本变量来生成可在多个场景中使用的脚本。
ms.custom: seo-lt-2019
ms.date: 08/09/2016
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
dev_langs:
- TSQL
helpviewer_keywords:
- scripts [SQL Server], sqlcmd utility
- variables [SQL Server], scripts
- scripting variables [SQL Server]
- sqlcmd utility, scripts
- setvar command
ms.assetid: 793495ca-cfc9-498d-8276-c44a5d09a92c
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96f2987363fd52cc92127b6b54ffcd2a98b9ae10
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97476878"
---
# <a name="sqlcmd---use-with-scripting-variables"></a>sqlcmd - 与脚本变量结合使用
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  脚本中使用的变量称为脚本变量。 使用脚本变量，一个脚本可以应用于多个方案中。 例如，如果需要对多台服务器运行单个脚本，则可以用脚本变量来表示服务器名称，而不必为每台服务器修改脚本。 通过更改脚本变量表示的服务器名称，可以在不同的服务器上运行同一脚本。  
  
 可以使用 setvar 命令显式定义脚本变量，也可以使用 sqlcmd -v 选项隐式定义脚本变量。  
  
 本主题还包含有关使用 **SET** 在 Cmd.exe 命令提示符下定义环境变量的示例。  
  
## <a name="setting-scripting-variables-by-using-the-setvar-command"></a>使用 setvar 命令设置脚本变量  
 **setvar** 命令用于定义脚本变量。 内部存储使用 **setvar** 命令定义的变量。 不应将脚本变量与使用 **SET** 在命令提示符下定义的环境变量相混淆。 如果脚本引用的变量不是环境变量，或不是使用 **setvar** 定义的变量，则会返回错误消息，并将停止执行脚本。 有关详细信息，请参阅 **sqlcmd 实用工具** 中的 [-b](../../tools/sqlcmd-utility.md)选项。  
  
## <a name="variable-precedence-low-to-high"></a>变量优先级（从低到高）  
 如果有多类变量具有相同的名称，则使用优先级最高的变量。  
  
1.  系统级环境变量  
  
2.  用户级环境变量  
  
3.  启动 **SET X=Y** 之前在命令提示符下设置的命令 shell ( **SET X=Y**)  
  
4.  sqlcmd -v X=Y  
  
5.  **:Setvar** X Y  
  
> [!NOTE]  
>  若要查看环境变量，请在“控制面板” 中打开“系统” ，然后单击“高级”  选项卡。  
  
## <a name="implicitly-setting-scripting-variables"></a>隐式设置脚本变量  
 使用具有相关 **sqlcmd** 变量的选项启动 **sqlcmd** 时， **sqlcmd** 变量将被隐式设置为使用该选项指定的值。 在下面的示例中，启动 `sqlcmd` 时使用了 `-l` 选项。 这会隐式设置 SQLLOGINTIMEOUT 变量。  
  
```
c:\> sqlcmd -l 60
```
 
你还可以使用 **-v** 选项对脚本中的脚本变量进行设置。 在下面的脚本（文件名为 `testscript.sql`）中， `ColumnName` 是一个脚本变量。  
 
```
USE AdventureWorks2012;

SELECT x.$(ColumnName)
FROM Person.Person x
WHERE x.BusinessEntityID < 5;
```

然后，您可以使用 `-v` 选项指定要返回的列名称：  
 
```
sqlcmd -v ColumnName ="FirstName" -i c:\testscript.sql
```

若要使用同一个脚本返回其他列，请更改 `ColumnName` 脚本变量的值。  
  
```
sqlcmd -v ColumnName ="LastName" -i c:\testscript.sql
```

## <a name="guidelines-for-scripting-variable-names-and-values"></a>有关脚本变量名和变量值的原则  
 为脚本变量命名时，请考虑以下原则：  
  
-   变量名不能包含空格字符或引号。  
  
-   变量名不能与变量表达式（如 *$(var)* ）具有相同的形式。  
  
-   脚本变量不区分大小写。  
  
    > [!NOTE]  
    >  如果没有为 **sqlcmd** 环境变量分配任何值，则将删除该变量。 使用不包含值的 **:setvar VarName** 可以清除该变量。  
  
 为脚本变量指定值时，请考虑以下原则：  
  
-   如果字符串值包含空格，必须给使用 **setvar** 或 **-v** 选项定义的变量值加上引号。  
  
-   如果引号属于变量值的一部分，则必须对其进行转义。 例如：:`setvar MyVar "spac""e"`。  
  
## <a name="guidelines-for-cmdexe-set-variable-values-and-names"></a>有关 Cmd.exe SET 变量值和变量名的原则  
 使用 SET 定义的变量是 Cmd.exe 环境的一部分并可以通过 **sqlcmd** 进行引用。 遵循以下指南：  
  
-   变量名不能包含空格字符或引号。  
  
-   变量值可包含空格或引号。  
  
## <a name="sqlcmd-scripting-variables"></a>sqlcmd 脚本变量  
 将 **sqlcmd** 定义的变量称为脚本变量。 下表列出了 **sqlcmd** 脚本变量。  
  
|        变量         | 相关选项 | R/W |         默认         |
| ----------------------- | -------------- | --- | ----------------------- |
| SQLCMDUSER\*             | -U             | R   | ""                      |
| SQLCMDPASSWORD\*         | -P             | --  | ""                      |
| SQLCMDSERVER\*           | sqlcmd             | R   | "DefaultLocalInstance"  |
| SQLCMDWORKSTATION       | -H             | R   | "ComputerName"          |
| SQLCMDDBNAME            | -d             | R   | ""                      |
| SQLCMDLOGINTIMEOUT      | -l             | R/W | "8"（秒）           |
| SQLCMDSTATTIMEOUT       | -t             | R/W | "0" = 无限期等待 |
| SQLCMDHEADERS           | -H             | R/W | "0"                     |
| SQLCMDCOLSEP            | -S             | R/W | " "                     |
| SQLCMDCOLWIDTH          | -w             | R/W | "0"                     |
| SQLCMDPACKETSIZE        | -a             | R   | "4096"                  |
| SQLCMDERRORLEVEL        | -M             | R/W | "0"                     |
| SQLCMDMAXVARTYPEWIDTH   | -y             | R/W | "256"                   |
| SQLCMDMAXFIXEDTYPEWIDTH | -y             | R/W | "0" = 无限制         |
| SQLCMDEDITOR            |                | R/W | "edit.com"              |
| SQLCMDINI               |                | R   | ""                      |

使用 **:Connect** 时设置 SQLCMDUSER、SQLCMDPASSWORD 和 SQLCMDSERVER。  

R 表示在程序初始化过程中只能设置一次值。  
  
R/W 表示可以使用 **setvar** 命令重置值，并且后续命令将使用新值。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-setvar-command-in-a-script"></a>A. 在脚本中使用 setvar 命令  
 许多 **sqlcmd** 选项可以通过在脚本内使用 **setvar** 命令进行控制。 在下面的示例中，创建了一个脚本 `test.sql` ，其中 `SQLCMDLOGINTIMEOUT` 变量设置为 `60` 秒，另一个脚本变量 `server`设置为 `testserver`。 以下是 `test.sql`中的代码。  

```
:setvar SQLCMDLOGINTIMEOUT 60
:setvar server "testserver"
:connect $(server) -l $(SQLCMDLOGINTIMEOUT)

USE AdventureWorks2012;

SELECT FirstName, LastName
FROM Person.Person;
```

然后使用 sqlcmd 调用脚本：

```
sqlcmd -i c:\test.sql
```
  
### <a name="b-using-the-setvar-command-interactively"></a>B. 交互式使用 setvar 命令  
 下面的示例说明了如何使用 `setvar` 命令交互式设置脚本变量。  

```
sqlcmd
:setvar  MYDATABASE AdventureWorks2012
USE $(MYDATABASE);
GO
```

 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```
Changed database context to 'AdventureWorks2012'
1>
```
  
### <a name="c-using-command-prompt-environment-variables-within-sqlcmd"></a>C. 在 sqlcmd 中使用命令提示符环境变量  
 在下例中，设置了四个环境变量 `are` 然后从 `sqlcmd`加以调用。  

```
C:\>SET tablename=Person.Person
C:\>SET col1=FirstName
C:\>SET col2=LastName
C:\>SET title=Ms.
C:\>sqlcmd -d AdventureWorks2012
1> SELECT TOP 5 $(col1) + ' ' + $(col2) AS Name
2> FROM $(tablename)
3> WHERE Title ='$(title)'
4> GO
```
  
### <a name="d-using-user-level-environment-variables-within-sqlcmd"></a>D. 在 sqlcmd 中使用用户级环境变量  
 在下面的示例中，在命令提示符下设置了用户级环境变量 `%Temp%` ，并将其传递给了 `sqlcmd` 输入文件。 若要获取用户级环境变量，请在“控制面板”中双击“系统”。 单击 **“高级”** 选项卡，再单击 **“环境变量”** 。  
  
 下列代码位于输入文件 `c:\testscript.txt`:

```
:OUT $(MyTempDirectory)
USE AdventureWorks2012;

SELECT FirstName
FROM AdventureWorks2012.Person.Person
WHERE BusinessEntityID` `< 5;
```

以下是在命令提示符下输入的代码：

```
C:\ >SET MyTempDirectory=%Temp%\output.txt
C:\ >sqlcmd -i C:\testscript.txt
```

 以下是发送给输出文件 C:\Documents and Settings\\<user\>\Local Settings\Temp\output.txt 的结果。  

```
Changed database context to 'AdventureWorks2012'.
FirstName
--------------------------------------------------
Gustavo
Catherine
Kim
Humberto

(4 rows affected)
```

### <a name="e-using-a-startup-script"></a>E. 使用启动脚本  
 将在 **sqlcmd** 启动时执行 **sqlcmd** 启动脚本。 下面的示例设置了环境变量 `SQLCMDINI`。 下面是 `init.sql.`  

```
SET NOCOUNT ON
GO

DECLARE @nt_username nvarchar(128)
SET @nt_username = (SELECT rtrim(convert(nvarchar(128), nt_username))
FROM sys.dm_exec_sessions WHERE spid = @@SPID)
SELECT  @nt_username + ' is connected to ' +
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('servername'))) +
' (' +`  
rtrim(CONVERT(nvarchar(20), SERVERPROPERTY('productversion'))) +
')'
:setvar SQLCMDMAXFIXEDTYPEWIDTH 100
SET NOCOUNT OFF
GO

:setvar SQLCMDMAXFIXEDTYPEWIDTH
```

 这将在 `init.sql` 启动时调用 `sqlcmd` 文件。  
  
```
c:\> SET sqlcmdini=c:\init.sql
>1 Sqlcmd
```

 这是输出。  

```
>1 < user > is connected to < server > (9.00.2047.00)
```

  
> [!NOTE]  
>  **-X** 选项禁用启动脚本功能。  
  
### <a name="f-variable-expansion"></a>F. 变量扩展  
 下面的示例演示了以 **sqlcmd** 变量的形式处理数据。  

```
USE AdventureWorks2012;
CREATE TABLE AdventureWorks2012.dbo.VariableTest
(
Col1 nvarchar(50)
);
GO
```

 在 `Col1` （包含值 `dbo.VariableTest` ）的 `$(tablename)`中插入一行。  

```
INSERT INTO AdventureWorks2012.dbo.VariableTest(Col1)
VALUES('$(tablename)');
GO
```
  
 在 `sqlcmd` 提示符下，如果没有将任何变量设置为 `$(tablename)`，则以下语句将返回该行。  
  
```
C:\> sqlcmd
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>2 GO
>3 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>4 GO
```

[!INCLUDE[ssResult](../../includes/ssresult-md.md)]  

```
>1 Col1
>2 ------------------
>3 $(tablename)
>4
>5 (1 rows affected)
```

 假设将变量 `MyVar` 设置为 `$(tablename)`。  

```
>6 :setvar MyVar $(tablename)
```

 这些语句返回该行，并且还返回了消息：“未定义‘tablename’脚本变量”。  

```
>6 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(tablename)';
>7 GO

>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(tablename)';
>2 GO
```

 这些语句返回该行。  

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = '$(MyVar)';
>2 GO
```

```
>1 SELECT Col1 FROM dbo.VariableTest WHERE Col1 = N'$(MyVar)';
>2 GO
```
  
## <a name="see-also"></a>另请参阅  
 [使用 sqlcmd 实用工具](./sqlcmd-use-the-utility.md)   
 [sqlcmd 实用工具](../../tools/sqlcmd-utility.md)   
 [命令提示实用工具参考（数据库引擎）](../../tools/command-prompt-utility-reference-database-engine.md)  
  
