---
description: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server
title: CREATE EXTERNAL LIBRARY (Transact-SQL) - SQL Server | Microsoft Docs
ms.custom: ''
ms.date: 08/26/2020
ms.prod: sql
ms.reviewer: ''
ms.technology: machine-learning
ms.topic: language-reference
f1_keywords:
- CREATE EXTERNAL LIBRARY
- CREATE_EXTERNAL_LIBRARY_TSQL
- EXTERNAL LIBRARY
- EXTERNAL_LIBRARY_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- CREATE EXTERNAL LIBRARY
author: dphansen
ms.author: davidph
manager: cgronlund
monikerRange: '>=sql-server-2017||>=sql-server-linux-ver15||=azuresqldb-mi-current'
ms.openlocfilehash: 9839a6fa8bd678afdce426f9dcd5657500c5a6cd
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98100778"
---
# <a name="create-external-library-transact-sql"></a>CREATE EXTERNAL LIBRARY (Transact-SQL)  
[!INCLUDE [SQL Server 2017 SQL MI](../../includes/applies-to-version/sqlserver2017-asdbmi.md)]

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
从指定的字节流或文件路径上传 R、Python 或 Java 包文件至数据库。 此语句充当一种通用机制，可供数据库管理员上传任何新的外部语言运行时和 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 支持的 OS 平台所需的项目。 

> [!NOTE]
> 在 SQL Server 2017 中，支持 R 语言和 Windows 平台。 SQL Server 2019 及更高版本支持 Windows 和 Linux 平台上的 R、Python 和外部语言。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
从指定的字节流或文件路径上传 R 或 Python 包文件至数据库。 此语句充当数据库管理员上传所需项目的通用机制。 

> [!NOTE]
> 在 Azure SQL 托管实例中，可以使用 sqlmlutils 安装库。 有关详细信息，请参阅[使用 sqlmlutils 安装 Python 包](../../machine-learning/package-management/install-additional-python-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true)和[使用 sqlmlutils 安装新的 R 包](../../machine-learning/package-management/install-additional-r-packages-on-sql-server.md?context=%252fazure%252fazure-sql%252fmanaged-instance%252fcontext%252fml-context&view=azuresqldb-mi-current&preserve-view=true)。
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
## <a name="syntax-for-sql-server-2019"></a>SQL Server 2019 语法

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = <platform> ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'  
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<platform> :: = 
{
      WINDOWS
    | LINUX
}

<language> :: = 
{
      'R'
    | 'Python'
    | <external_language>
}

```
::: moniker-end
::: moniker range=">=sql-server-2017 <=sql-server-2017"
## <a name="syntax-for-sql-server-2017"></a>SQL Server 2017 语法

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = 'R' )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = { <client_library_specifier> | <library_bits> }  
    [, PLATFORM = WINDOWS ])  
}  

<client_library_specifier> :: = 
{
    '[file_path\]manifest_file_name'
} 

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}
```
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
## <a name="syntax-for-azure-sql-managed-instance"></a>Azure SQL 托管实例的语法

```syntaxsql
CREATE EXTERNAL LIBRARY library_name  
[ AUTHORIZATION owner_name ]  
FROM <file_spec> [ ,...2 ]  
WITH ( LANGUAGE = <language> )  
[ ; ]  

<file_spec> ::=  
{  
    (CONTENT = <library_bits>)  
}  

<library_bits> :: =  
{ 
      varbinary_literal 
    | varbinary_expression 
}

<language> :: = 
{
      'R'
    | 'Python'
}
```
::: moniker-end

### <a name="arguments"></a>参数

**library_name**

上传到实例的库可以是公共的也可以是私有的。 如果是由 `dbo` 成员创建的库，则该库是公共的且所有的用户都可以共享。 否则，该库就仅为用户私有。

在特定用户或所有者的上下文中，库名称必须是唯一的。 例如，两个用户 RUser1 和 RUser2 可以分别独立上传 R 库 `ggplot2` 。 不过，如果 RUser1 要上传新版 `ggplot2`，第二个实例要么必须以不同方式命名，要么必须替换现有库。

不能随意分配库名称；库名称应与在外部脚本中加载库时所需的名称相同。

**owner_name**

指定拥有外部库的用户或角色的名称。 如果未指定，则所有权授予当前用户。

对于数据库和运行时，数据库所有者所拥有的库均视为全局性的库。 换言之，数据库所有者可以创建库，而这些库包含一组由许多用户共享的公共库或包。 当由用户而不是 `dbo` 用户创建外部库时，该外部库便由该用户专用。

当用户 **RUser1** 执行外部脚本时，`libPath` 值可包含多个路径。 第一个路径始终为数据库所有者创建的共享库的路径。 `libPath` 的第二部分指定包含由 RUser1 单独上传的包的路径。

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
**file_spec**

指定特定平台的包的内容。 每个平台仅支持一个文件项目。

可以是以本地路径或网络路径的形式指定的文件。

尝试访问 <client_assembly_specifier> 中指定的程序集时，SQL Server 会模拟当前 Windows 登录的安全上下文。 如果 <client_assembly_specifier> 指定了网络位置（UNC 路径），则由于委托限制，当前登录名的模拟将不应用于网络位置。 在这种情况下，将使用 SQL Server 服务帐户的安全上下文进行访问。 有关详细信息，请参阅[凭据（数据库引擎）](../../relational-databases/security/authentication-access/credentials-database-engine.md)。

还可以为文件指定一个 OS 平台。 针对特定语言或运行时，每个 OS 平台只允许一个文件项目或内容。
::: moniker-end

**library_bits**

将包的内容指定为十六进制文本，类似于程序集。 

如果需要创建库或更改现有库（并具有执行此操作的所需权限），但服务器上的文件系统受限，无法将库文件复制到服务器可以访问的位置，此选项会非常有用。

::: moniker range="=sql-server-2017"
**PLATFORM = WINDOWS**

为库的内容指定平台。 该值默认为正在运行 SQL Server 的主机平台。 因此，用户不需要指定该值。 如果支持多个平台或用户需要指定不同的平台，则需要此值。
在 SQL Server 2017 中，Windows 是唯一受支持的平台。
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**PLATFORM**

为库的内容指定平台。 该值默认为正在运行 SQL Server 的主机平台。 因此，用户不需要指定该值。 如果支持多个平台或用户需要指定不同的平台，则需要此值。
在 SQL Server 2019 中，Windows 和 Linux 是受支持的平台。
::: moniker-end

::: moniker range="=sql-server-2017"
**LANGUAGE = 'R'**

指定包的语言。 SQL Server 2017 中支持 R。
::: moniker-end

::: moniker range="=azuresqldb-mi-current"
**language**

指定包的语言。 在 Azure SQL 托管实例中，该值可以为 `R` 或 `Python`。
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
**language**

指定包的语言。 该值可以是 `R`、`Python` 或外部语言的名称（请参阅[创建外部语言](create-external-language-transact-sql.md)）。
::: moniker-end

[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="remarks"></a>备注

::: moniker range=">=sql-server-2017 <=sql-server-2017"
对于 R 语言，使用文件时，必须使用 Windows 的 .ZIP 扩展名以压缩存档文件的形式准备包。 
在 SQL Server 2017 中，仅支持 Windows 平台。
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
对于 R 语言，使用文件时，必须使用 .ZIP 扩展名以压缩存档文件的形式准备包。  

对于 Python 语言，必须以压缩存档文件的形式准备 .whl 或 .zip 文件中的包。 如果包已经是 .zip 文件，则必须将其包含在新的 .zip 文件中。 目前不支持将包作为 .whl 或 .zip 文件直接上传。
::: moniker-end

`CREATE EXTERNAL LIBRARY` 语句将库位上载到数据库。 当用户使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 运行外部脚本并调用包或库时，会安装该库。

上传到实例的库可以是公共的也可以是私有的。 如果是由 `dbo` 成员创建的库，则该库是公共的且所有的用户都可以共享。 否则，该库就仅为用户私有。

许多包（称为“系统包”）都预安装在 SQL 实例中。 用户无法添加、更新或删除系统包。

## <a name="permissions"></a>权限

需要 `CREATE EXTERNAL LIBRARY` 权限。 默认情况下，dbo 用户或担任 db_owner 角色的任何成员都有权创建外部库。 对于其他所有用户，必须使用 [GRANT](./grant-database-permissions-transact-sql.md) 语句显式授予他们权限，同时将 CREATE EXTERNAL LIBRARY 指定为特权。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
在 SQL Server 2019 中，除了“CREATE EXTERNAL LIBRARY”权限之外，用户还需要外部语言的引用权限才能为该外部语言创建外部库。

```sql
GRANT REFERENCES ON EXTERNAL LANGUAGE::Java to user
GRANT CREATE EXTERNAL LIBRARY to user
```

::: moniker-end

更改任何库都需要单独的权限，`ALTER ANY EXTERNAL LIBRARY`。

若要通过使用文件路径创建外部库，用户必须使用经过 Windows 身份验证的登录名或必须是 sysadmin 固定服务器角色的成员。

## <a name="examples"></a>示例

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"
### <a name="add-an-external-library-to-a-database"></a>将外部库添加到数据库  

下面的示例将一个名为 `customPackage` 的外部库添加到数据库。

```sql
CREATE EXTERNAL LIBRARY customPackage
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\customPackage.zip') WITH (LANGUAGE = 'R');
```

在库成功上传到实例之后，用户可执行 `sp_execute_external_script` 步骤来安装库。

```sql
EXEC sp_execute_external_script 
@language =N'R', 
@script=N'library(customPackage)'
```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
对于 SQL Server 2019 中的 Python 语言，可通过将 `'R'` 替换为 `'Python'` 使用示例。
::: moniker-end

### <a name="installing-packages-with-dependencies"></a>安装具有依赖项的包

如果想要安装的包有任何依赖项，务必要分析第一级和第二级依赖项，并在尝试安装目标包之前，确保所需的所有包都可用。

例如，假设想要安装新包 `packageA`：

+ `packageA` 具有 `packageB` 上的依赖项
+ `packageB` 具有 `packageC` 上的依赖项

若要成功安装 `packageA`，则必须在将 `packageA` 添加到 SQL Server 的同时为 `packageB` 和 `packageC` 创建库。 同时请务必检查所需的包版本。

在实践中，常用包的依赖项通常比简单示例复杂得多。 例如，ggplot2 可能需要超过 30 个包，而这些包可能还需要服务器上没有的其他包。 任何缺少的包或错误的包版本都可能会导致安装失败。

由于仅通过查看程序包清单可能很难确定所有依赖项，因此建议使用 [miniCRAN](https://cran.r-project.org/web/packages/miniCRAN/index.html) 等包，以标识成功完成安装可能需要的所有包。

::: moniker range=">=sql-server-2017||>=sql-server-linux-ver15"

+ 上传目标包及其依赖项。 所有文件都必须位于服务器可访问的文件夹中。

    ```sql
    CREATE EXTERNAL LIBRARY packageA 
    FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageA.zip') 
    WITH (LANGUAGE = 'R'); 
    GO

    CREATE EXTERNAL LIBRARY packageB FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageB.zip') 
    WITH (LANGUAGE = 'R');
    GO

    CREATE EXTERNAL LIBRARY packageC FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL14.MSSQLSERVER\packageC.zip') 
    WITH (LANGUAGE = 'R');
    GO
    ```

+ 首先安装所需的包。

    如果所需的包已上传到实例，则不需要再次添加。 请务必检查现有包的版本是否正确。 
    
    第一次运行 `sp_execute_external_script` 以安装包 `packageA` 时，所需的包 `packageC` 和 `packageB` 也会按照正确的顺序安装。

    但是，如果所需的包均不可用，则安装目标包 `packageA` 时会失败。

    ```sql
    EXEC sp_execute_external_script 
    @language =N'R', 
    @script=N'
    # load the desired package packageA
    library(packageA)
    '
    ```
::: moniker-end

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
对于 SQL Server 2019 中的 Python 语言，可通过将 `'R'` 替换为 `'Python'` 使用示例。
::: moniker-end

### <a name="create-a-library-from-a-byte-stream"></a>从字节流创建库

如果无法将包文件保存到服务器上的某个位置，可以在变量中传递包内容。 下面的示例通过将位传递为十六进制文本来创建库。

```SQL
CREATE EXTERNAL LIBRARY customLibrary FROM (CONTENT = 0xABC123...) WITH (LANGUAGE = 'R');
```

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
对于 SQL Server 2019 中的 Python 语言，可通过将 **“R”** 替换为 **“Python”** 使用示例。
::: moniker-end

> [!NOTE]
> 此代码示例只用于展示语法；为了提高可读性，`CONTENT =` 中的二进制值已遭截断，无法创建能够正常运行的库。 二进制变量的实际内容要长得多。

### <a name="change-an-existing-package-library"></a>更改现有包库

`ALTER EXTERNAL LIBRARY` DDL 语句可用于向新库添加内容或更改现有库的内容。 修改现有库需要 `ALTER ANY EXTERNAL LIBRARY` 权限。

有关详细信息，请参阅 [ALTER EXTERNAL LIBRARY](alter-external-library-transact-sql.md)。

::: moniker range=">=sql-server-ver15||>=sql-server-linux-ver15"
### <a name="add-a-java-jar-file-to-a-database"></a>向数据库添加 Java .jar 文件  

下面的示例将一个名为 `customJar` 的外部 jar 文件添加到数据库。

```sql
CREATE EXTERNAL LIBRARY customJar
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\customJar.jar') 
WITH (LANGUAGE = 'Java');
```

在库成功上传到实例之后，用户可执行 `sp_execute_external_script` 步骤来安装库。

```sql
EXEC sp_execute_external_script
    @language = N'Java'
    , @script = N'customJar.MyCLass.myMethod'
    , @input_data_1 = N'SELECT * FROM dbo.MyTable'
WITH RESULT SETS ((column1 int))
```

### <a name="add-an-external-package-for-both-windows-and-linux"></a>为 Windows 和 Linux 添加外部包

最多可以指定两个 `<file_spec>`，一个用于 Windows，另一个用于 Linux。

```sql
CREATE EXTERNAL LIBRARY lazyeval 
FROM (CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.zip', PLATFORM = WINDOWS),
(CONTENT = 'C:\Program Files\Microsoft SQL Server\MSSQL15.MSSQLSERVER\packageA.tar.gz', PLATFORM = LINUX)
WITH (LANGUAGE = 'R')
```

根据运行 SQL Server 实例的平台，在使用 `sp_execute_external_script` 来安装包时，将使用此平台的库内容。

```sql
EXECUTE sp_execute_external_script 
    @LANGUAGE = N'R',
    @SCRIPT = N'
library(packageA)
```
::: moniker-end

## <a name="see-also"></a>另请参阅

[ALTER EXTERNAL LIBRARY (Transact-SQL)](alter-external-library-transact-sql.md)  
[DROP EXTERNAL LIBRARY (Transact-SQL)](drop-external-library-transact-sql.md)  
[sys.external_library_files](../../relational-databases/system-catalog-views/sys-external-library-files-transact-sql.md)  
[sys.external_libraries](../../relational-databases/system-catalog-views/sys-external-libraries-transact-sql.md)