---
title: 调用 Java 运行时
titleSuffix: SQL Server Language Extensions
description: 了解如何使用 SQL Server 扩展从 SQL Server 存储过程调用 Java 类。
author: dphansen
ms.author: davidph
ms.date: 06/25/2020
ms.topic: how-to
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: c4d12382cf1e498b33f202bb1b775ba63458a5a5
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870175"
---
# <a name="how-to-call-the-java-runtime-in-sql-server-language-extensions"></a>如何在 SQL Server 语言扩展中调用 Java 运行时
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

[SQL Server 语言扩展](../language-extensions-overview.md)使用 [sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 系统存储过程作为接口来调用 Java 运行时。 

本操作说明文章介绍了在 SQL Server 上执行的 Java 类和方法的实现详细信息。

## <a name="where-to-place-java-classes"></a>放置 Java 类的位置

可以采用两种方法在 SQL Server 中调用 Java 类：

1. 将 .class 或 .jar 文件放置在 [Java classpath](#classpath) 中   。 

2. 使用[外部库](#external-library) DDL 将已编译的类上传到 .jar 文件中，并且将其他依赖项上传到数据库中  。 

> [!NOTE]
> 通常建议使用 .jar 文件而不是单独的 .class 文件   。 这在 Java 中是常见的做法，这将使整体体验更加轻松。 另请参阅[如何从类文件创建 jar 文件](create-a-java-jar-file-from-class-files.md)。

<a name="classpath"></a>

## <a name="use-classpath"></a>使用 Classpath

### <a name="basic-principles"></a>基本原则

下面是在 SQL Server 上执行 Java 时的一些基本原则。

* 已编译的自定义 Java 类必须存在于 Java classpath 中的 .class 文件或 .jar 文件中   。 [CLASSPATH 参数](#set-classpath)提供已编译的 Java 文件的路径。 

* 要调用的 Java 方法必须在存储过程上的 script 参数中提供  。

* 如果类属于某个包，则必须提供 packageName  。

* params 用于将参数传递到 Java 类  。 不支持调用需要参数的方法。 因此，形参是将实参值传递到方法的唯一方式。 

> [!NOTE]
> 此说明重述了 SQL Server 2019 候选发布 1 中特定于 Java 的受支持和不受支持的操作。
> * 在存储过程上，输入参数是受支持的。 输出参数则不受支持。

### <a name="call-java-class"></a>调用 Java 类

[sp_execute_external_script](../../relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql.md) 系统存储过程是用于调用 Java 运行时的接口。 以下示例显示了使用 Java 扩展的 `sp_execute_external_script`以及用于指定路径、脚本和自定义代码的参数。

> [!NOTE]
> 请注意，你无需定义要调用的方法。 默认情况下，会调用名为“execute”的方法  。 这意味着你需要遵循 [SQL Server 中用于 Java 的扩展性 SDK](extensibility-sdk-java-sql-server.md)，并在 Java 类中实现 execute 方法。

```sql
DECLARE @param1 int
SET @param1 = 3

EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'<packageName>.<ClassName>'
, @input_data_1 = N'<Input Query>'
, @param1 = @param1
```

<a name="set-classpath"></a>

### <a name="set-classpath"></a>设置 CLASSPATH

在你编译一个或多个 Java 类或在 Java classpath 中创建 jar 文件后，有两个选项可用于向 SQL Server Java 扩展提供 classpath：

1. 使用外部库

    最简单的选项是通过创建外部库并将此库指向 jar 来使 SQL Server 自动找到你的类。 [使用 Java 的外部库](#external-library)

2. 注册系统环境变量

    你可以创建系统环境变量，并提供指向包含这些类的 jar 文件的路径。 创建名为“CLASSPATH”的系统环境变量  。

<a name="external-library"></a>

## <a name="use-external-library"></a>使用外部库

在 SQL Server 2019 候选发布 1 中，你可以在 Windows 和 Linux 上使用适用于 Java 语言的外部库。 可以使用 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) DDL 将类编译到 .jar 文件中，并将 .jar 文件和其他依赖项上传到数据库中。

如何使用外部库上传 .jar 文件的示例：

```sql 
CREATE EXTERNAL LIBRARY myJar
FROM (CONTENT = '<local path to .jar file>') 
WITH (LANGUAGE = 'Java'); 
GO
```

通过创建外部库，SQL Server 将自动具有对 Java 类的访问权限，并且你无需将任何特殊权限设置为 classpath。

从上传为外部库的包调用类中的方法的示例：

```sql
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'MyPackage.MyCLass'
, @input_data_1 = N'SELECT * FROM MYTABLE'
with result sets ((column1 int))
```

有关详细信息，请参阅 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md)。

## <a name="loopback-connection-to-sql-server"></a>到 SQL Server 的环回连接

使用环回连接通过 JDBC 连接回 SQL Server，以从 `sp_execute_external_script` 执行的 Java 中读取或写入数据。 当无法使用 `sp_execute_external_script` 的 InputDataSet 和 OutputDataSet 参数时，可以使用此选项 。
若要在 Windows 中建立环回连接，请使用以下示例：

```
jdbc:sqlserver://localhost:1433;databaseName=Adventureworks;integratedSecurity=true;
``` 

若要在 Linux 中建立环回连接，JDBC 驱动程序需要以下证书中定义的三个连接属性：

[Client-Certificate-Authenication](https://github.com/microsoft/mssql-jdbc/wiki/Client-Certificate-Authentication-for-Loopback-Scenarios)


## <a name="next-steps"></a>后续步骤

+ [教程：在 Java 中使用正则表达式搜索字符串](../tutorials/search-for-string-using-regular-expressions-in-java.md)