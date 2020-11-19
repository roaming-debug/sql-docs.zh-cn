---
title: 教程：Java 中的 regex 字符串搜索
description: 本教程演示如何使用 SQL Server 语言扩展以及运行使用正则表达式 (regex) 搜索字符串的 Java 代码。
author: dphansen
ms.author: davidph
ms.date: 11/05/2019
ms.topic: tutorial
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 21d981c75881d0d971b0f27757015792237f12e3
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870125"
---
# <a name="tutorial-search-for-a-string-using-regular-expressions-regex-in-java"></a>教程：在 Java 中使用正则表达式 (regex) 搜索字符串
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

本教程演示如何使用 [SQL Server 语言扩展](../language-extensions-overview.md)创建一个 Java 类，该类接收来自 SQL Server 的两列（ID 和 text），并接收一个正则表达式 (regex) 作为输入参数。 该类会将两列返回到 SQL Server（ID 和 text）。

对于发送到 Java 类的 text 列中的给定文本，代码会检查是否满足给定正则表达式，并将该文本与原始 ID 一起返回。

此示例代码使用检查文本是否包含单词“Java”或“java”的正则表达式。

## <a name="prerequisites"></a>先决条件

+ [Windows](../install/windows-java.md) 或 [Linux](../../linux/sql-server-linux-setup-language-extensions-java.md) 上的 SQL Server 2019 数据库引擎实例，具有扩展性框架和 Java 编程扩展。 有关详细信息，请参阅 [SQL Server 2019 中的语言扩展](../language-extensions-overview.md)。 有关编码要求的详细信息，请参阅[如何在 SQL Server 中调用 Java](../how-to/call-java-from-sql.md)。

+ 用于执行 T-SQL 的 SQL Server Management Studio 或 Azure Data Studio。

+ Windows 或 Linux 上的 Java SE 开发工具包 (JDK) 8 或 JRE 8。

+ 来自[用于 Microsoft SQL Server 的 Microsoft Java 扩展性 SDK](../how-to/extensibility-sdk-java-sql-server.md) 的 mssql-java-lang-extension.jar  文件。

使用 javac  的命令行编译足以满足本教程的要求。

## <a name="create-sample-data"></a>创建示例数据

首先创建新数据库，并使用 ID  和 text  列填充 testdata  表。

```sql
CREATE DATABASE javatest
GO

USE javatest
GO

CREATE TABLE testdata (
    id int NOT NULL,
    "text" nvarchar(100) NOT NULL
)
GO

-- Insert data into test table
INSERT INTO testdata(id, "text") VALUES (1, 'This sentence contains java')
INSERT INTO testdata(id, "text") VALUES (2, 'This sentence does not')
INSERT INTO testdata(id, "text") VALUES (3, 'I love Java!')
GO
```

## <a name="create-the-main-class"></a>创建主类

在此步骤中，创建名为 RegexSample.java  的类文件，并将以下 Java 代码复制到该文件中。

此 main 类会导入 SDK，这意味着需要可从此类发现步骤 1 中下载的 jar 文件。

```java
package pkg;

import com.microsoft.sqlserver.javalangextension.PrimitiveDataset;
import com.microsoft.sqlserver.javalangextension.AbstractSqlServerExtensionExecutor;
import java.util.LinkedHashMap;
import java.util.LinkedList;
import java.util.ListIterator;
import java.util.regex.*;

public class RegexSample extends AbstractSqlServerExtensionExecutor {
    private Pattern expr;

    public RegexSample() {
        // Setup the expected extension version, and class to use for input and output dataset
        executorExtensionVersion = SQLSERVER_JAVA_LANG_EXTENSION_V1;
        executorInputDatasetClassName = PrimitiveDataset.class.getName();
        executorOutputDatasetClassName = PrimitiveDataset.class.getName();
    }
    
    public PrimitiveDataset execute(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Validate the input parameters and input column schema
        validateInput(input, params);

        int[] inIds = input.getIntColumn(0);
        String[] inValues = input.getStringColumn(1);
        int rowCount = inValues.length;

        String regexExpr = (String)params.get("regexExpr");
        expr = Pattern.compile(regexExpr);

        System.out.println("regex expression: " + regexExpr);

        // Lists to store the output data
        LinkedList<Integer> outIds = new LinkedList<Integer>();
        LinkedList<String> outValues = new LinkedList<String>();

        // Evaluate each row
        for(int i = 0; i < rowCount; i++) {
            if (check(inValues[i])) {
                outIds.add(inIds[i]);
                outValues.add(inValues[i]);
            }
        }

        int outputRowCount = outValues.size();

        int[] idOutputCol = new int[outputRowCount];
        String[] valueOutputCol = new String[outputRowCount];

        // Convert the list of output columns to arrays
        outValues.toArray(valueOutputCol);

        ListIterator<Integer> it = outIds.listIterator(0);
        int rowId = 0;

        System.out.println("Output data:");
        while (it.hasNext()) {
            idOutputCol[rowId] = it.next().intValue();

            System.out.println("ID: " + idOutputCol[rowId] + " Value: " + valueOutputCol[rowId]);
            rowId++;
        }

        // Construct the output dataset
        PrimitiveDataset output = new PrimitiveDataset();

        output.addColumnMetadata(0, "ID", java.sql.Types.INTEGER, 0, 0);
        output.addColumnMetadata(1, "Text", java.sql.Types.NVARCHAR, 0, 0);

        output.addIntColumn(0, idOutputCol, null);
        output.addStringColumn(1, valueOutputCol);

        return output;
    }

    private void validateInput(PrimitiveDataset input, LinkedHashMap<String, Object> params) {
        // Check for the regex expression input parameter
        if (params.get("regexExpr") == null) {
            throw new IllegalArgumentException("Input parameter 'regexExpr' is not found");
        }

        // The expected input schema should be at least 2 columns, (INTEGER, STRING)
        if (input.getColumnCount() < 2) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }

        // Check that the input column types are expected
        if (input.getColumnType(0) != java.sql.Types.INTEGER &&
                (input.getColumnType(1) != java.sql.Types.VARCHAR && input.getColumnType(1) == java.sql.Types.NVARCHAR )) {
            throw new IllegalArgumentException("Unexpected input schema, schema should be an (INTEGER, NVARCHAR or VARCHAR)");
        }
    }

    private boolean check(String text) {
        Matcher m = expr.matcher(text);

        return m.find();
    }
}
```

## <a name="compile-and-create-a-jar-file"></a>编译和创建 .jar 文件

将类和依赖项打包到 `.jar` 文件中。 大多数 Java IDE（例如 Eclipse 或 IntelliJ）在生成或编译项目时支持生成 `.jar` 文件。 将 `.jar` 文件命名为 regex.jar  。

如果使用的不是 Java IDE，则可以手动创建 `.jar` 文件。 有关详细信息，请参阅[如何从类文件创建 Java jar 文件](../how-to/create-a-java-jar-file-from-class-files.md)。

> [!NOTE]
> 本教程使用包。 类顶部的 `package pkg;` 行确保将编译的代码保存在名为 pkg  的子文件夹中。 如果使用 IDE，则编译的代码会自动保存在此文件夹中。 如果使用 javac  手动编译类，则需要将编译的代码放置在 pkg  文件夹中。

## <a name="create-external-language"></a>创建外部语言

需要在数据库中创建外部语言。 外部语言是数据库范围内的对象，这意味着需要为要在其中使用外部语言（如 Java）的每个数据库创建外部语言。

### <a name="create-external-language-on-windows"></a>在 Windows 上创建外部语言

如果使用的是 Windows，请按照以下步骤为 Java 创建外部语言。

1. 创建包含扩展名的 .zip 文件。

    作为 Windows 上的 SQL Server 安装程序的一部分，Java 扩展 .zip  文件安装在以下位置：`[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip`。 此 zip 文件包含 javaextension.dll  。

2. 通过 .zip 文件创建外部语言 Java：

    ```sql
    CREATE EXTERNAL LANGUAGE Java
    FROM
    (CONTENT = N'[SQL Server install path]\MSSQL\Binn\java-lang-extension.zip', FILE_NAME = 'javaextension.dll',
    ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
    GO
    ```

### <a name="create-external-language-on-linux"></a>在 Linux 上创建外部语言

在安装过程中，扩展 .tar.gz  文件会保存在以下路径下：`/opt/mssql-extensibility/lib/java-lang-extension.tar.gz`。

若要创建外部语言 Java，请在 Linux 上运行以下 T-SQL 语句：

```sql
CREATE EXTERNAL LANGUAGE Java
FROM (CONTENT = N'/opt/mssql-extensibility/lib/java-lang-extension.tar.gz', file_name = 'javaextension.so',
ENVIRONMENT_VARIABLES = N'{"JRE_HOME":"<path to JRE>"}' );
GO
```

### <a name="permissions-to-execute-external-language"></a>执行外部语言的权限

若要执行 Java 代码，需要向用户授予对该特定语言的外部脚本执行权限。

有关详细信息，请参阅 [CREATE EXTERNAL LANGUAGE](../../t-sql/statements/create-external-language-transact-sql.md)。

## <a name="create-external-libraries"></a>创建外部库

使用 [CREATE EXTERNAL LIBRARY](../../t-sql/statements/create-external-library-transact-sql.md) 可为 `.jar` 文件创建外部库。 SQL Server 将有权访问 `.jar` 文件，而你无需对 classpath  设置任何特殊权限。

在此示例中会创建两个外部库。 一个用于 SDK，另一个用于 RegEx Java 代码。

1. SDK jar 文件 mssql-java-lang-extension.jar  在 Windows 和 Linux 上作为 SQL Server 2019 的一部分进行安装。

    + Windows 上的默认安装路径：[实例安装主目录]\MSSQL\Binn\mssql-java-lang-extension.jar 

    + Linux 上的默认安装路径：/opt/mssql/lib/mssql-java-lang-extension.jar 

    该代码也是开放源代码，可在 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions)中找到。 有关详细信息，请参阅[面向 Microsoft SQL Server 的用于 Java 的 Microsoft 扩展性 SDK](../how-to/extensibility-sdk-java-sql-server.md)。

2. 创建 SDK 的外部库。

    ```sql
    CREATE EXTERNAL LIBRARY sdk
    FROM (CONTENT = '<OS specific path from above>/mssql-java-lang-extension.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

3. 创建 RegEx 代码的外部库。

    ```sql
    CREATE EXTERNAL LIBRARY regex
    FROM (CONTENT = '<path>/regex.jar')
    WITH (LANGUAGE = 'Java');
    GO
    ```

## <a name="set-permissions"></a>设置权限

> [!NOTE]
> 如果在上一步中使用外部库，请跳过此步骤。 推荐方法是从 `.jar` 文件创建外部库。

如果不想使用外部库，则需要设置所需权限。 脚本执行仅当进程标识有权访问代码时才会成功。 可以在[安装指南](../install/windows-java.md)中找到有关设置权限的详细信息。

### <a name="on-linux"></a>在 Linux 上

向 mssql_satellite  用户授予对 classpath 的读取/执行权限。

### <a name="on-windows"></a>在 Windows 上

对包含已编译 Java 代码的文件夹，向 SQLRUserGroup  和“所有应用程序包”  SID 授予“读取和执行”权限。

整个树都必须拥有权限（从根父文件夹到最后一个子文件夹）。

1. 右键单击该文件夹（例如 `C:\myJavaCode`），然后选择“属性”   > “安全性”  。
2. 单击 **“编辑”** 。
3. 单击“添加”  。
4. 在“选择用户、计算机、服务帐户或组”  中：
   1. 单击“对象类型”  并确保选择“内置安全原则”  和“组”  。
   2. 单击“位置”  在列表顶部选择本地计算机名称。
5. 输入 SQLRUserGroup  ，检查名称，然后单击“确定”以添加组。
6. 输入“所有应用程序包”  ，检查名称，然后单击“确定”以添加。 
    如果名称未解析，请重新访问“位置”步骤。 SID 是计算机的本地 SID。

确保两个安全标识都用于对文件夹和 pkg  子文件夹的“读取和执行”  权限。

<a name="call-method"></a>

## <a name="call-the-java-class"></a>调用 Java 类

创建一个存储过程，它调用 `sp_execute_external_script` 以从 SQL Server 调用 Java 代码。 在 script  参数中，定义要调用的 `package.class`。 在下面的代码中，类属于名为 pkg  的包和名为 RegexSample.java  的类文件。

> [!NOTE]
> 代码未定义要调用的方法。 默认情况下，会调用 execute  方法。 这意味着，如果希望能够从 SQL Server 调用类，则需要遵循 SDK 接口并在 Java 类中实现 execute 方法。

存储过程采用输入查询（输入数据集）和正则表达式，并返回满足给定正则表达式的行。 它使用检查文本是否包含单词 Java  或 java  的正则表达式 `[Jj]ava`。

```sql
CREATE OR ALTER PROCEDURE [dbo].[java_regex] @expr nvarchar(200), @query nvarchar(400)
AS
BEGIN
--Call the Java program by giving the package.className in @script
--The method invoked in the Java code is always the "execute" method
EXEC sp_execute_external_script
  @language = N'Java'
, @script = N'pkg.RegexSample'
, @input_data_1 = @query
, @params = N'@regexExpr nvarchar(200)'
, @regexExpr = @expr
with result sets ((ID int, text nvarchar(100)));
END
GO

--Now execute the above stored procedure and provide the regular expression and an input query
EXECUTE [dbo].[java_regex] N'[Jj]ava', N'SELECT id, text FROM testdata'
GO
```

### <a name="results"></a>结果

执行调用之后，应获得包含两行的结果集。

![Java 示例的结果](../media/java/java-sample-results.png "示例结果")

### <a name="if-you-get-an-error"></a>如果遇到错误

+ 编译类时，pkg  子文件夹应包含所有三个类的已编译代码。

+ 如果不使用外部库，请检查每个  文件夹的权限（从根  文件夹到 pkg  子文件夹），以确保运行外部进程的安全标识拥有读取和执行代码的权限。

## <a name="next-steps"></a>后续步骤

+ [如何在 SQL Server 中调用 Java](../how-to/call-java-from-sql.md)
+ [SQL Server 中的 Java 扩展](../language-extensions-overview.md)
+ [Java 和 SQL Server 数据类型](../how-to/java-to-sql-data-types.md)