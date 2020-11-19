---
title: 用于 Java 的 Microsoft 扩展性 SDK
description: 了解如何才能使用用于 Java 的 Microsoft 扩展性 SDK 为 SQL Server 实现 Java 程序。
ms.prod: sql
ms.technology: language-extensions
ms.date: 11/05/2019
ms.topic: conceptual
author: dphansen
ms.author: davidph
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15||=sqlallproducts-allversions'
ms.openlocfilehash: 011d8ba2e9c92d0df6fe2956c190f61934948f54
ms.sourcegitcommit: 82b92f73ca32fc28e1948aab70f37f0efdb54e39
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/18/2020
ms.locfileid: "94870127"
---
# <a name="microsoft-extensibility-sdk-for-java-for-sql-server"></a>SQL Server 的用于 Java 的 Microsoft 扩展性 SDK
[!INCLUDE [SQL Server 2019 and later](../../includes/applies-to-version/sqlserver2019.md)]

了解如何才能使用用于 Java 的 Microsoft 扩展性 SDK 为 SQL Server 实现 Java 程序。 SDK 是用于与 SQL Server 交换数据并从 SQL Server 执行 Java 代码的 Java 语言扩展的接口。

SDK 作为 SQL Server 2019 候选发布版 1 的一部分安装在 Windows 和 Linux 上：

+ Windows 上的默认安装路径：[实例安装主目录]\MSSQL\Binn\mssql-java-lang-extension.jar
+ Linux 上的默认安装路径：/opt/mssql/lib/mssql-java-lang-extension.jar

该代码是开放源代码，可在 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions)中找到。

## <a name="implementation-requirements"></a>实现要求

SDK 接口定义了一组要求，SQL Server 需要满足这些要求才能与 Java 运行时通信。 若要使用 SDK，需要在主类中遵循一些实现规则。 SQL Server 随后可以执行 Java 类中的特定方法，并使用 Java 语言扩展交换数据。

有关如何使用 SDK 的示例，请参阅[教程：在 Java 中使用正则表达式 (regex) 搜索字符串](../tutorials/search-for-string-using-regular-expressions-in-java.md)。

## <a name="sdk-classes"></a>SDK 类

SDK 由三个类组成。

定义 Java 扩展用于与 SQL Server 交换数据的接口的两个抽象类：

- **AbstractSqlServerExtensionExecutor**
- **AbstractSqlServerExtensionDataset**

第三个类是帮助程序类，包含数据集对象的实现。 它是可用使用的可选类，这样可以更轻松地开始使用。 还可以改为使用你自己对这样一个类的实现。

- **PrimitiveDataset**

下面提供 SDK 中每个类的说明。 SDK 类的源代码在 [SQL Server 语言扩展 GitHub 存储库](https://github.com/microsoft/sql-server-language-extensions/tree/master/language-extensions/java/sdk)中提供。

### <a name="class-abstractsqlserverextensionexecutor"></a>类：AbstractSqlServerExtensionExecutor

抽象类 AbstractSqlServerExtensionExecutor 包含由 SQL Server 的 Java 语言扩展用于执行 Java 代码的接口。

主 Java 类需要从此类继承。 从此类继承表示类中有一些需要在自己的类中实现的特定方法。

若要从此抽象类继承，请在类声明中使用抽象类名进行扩展：

```java
public class <MyClass> extends AbstractSqlServerExtensionExecutor {}
```

主类至少需要实现 execute(...) 方法。

#### <a name="method-execute"></a>execute 方法

execute 方法是通过 Java 语言扩展从 SQL Server 进行调用，以便从 SQL Server 调用 Java 代码的方法。 这是一个关键方法，其中包含要从 SQL Server 执行的主要操作。

若要从 SQL Server 向 Java 传递方法参数，请在 `sp_execute_external_script` 中使用 `@param` 参数。 execute 方法通过此方式获取其参数。

```java
public AbstractSqlServerExtensionDataset execute(AbstractSqlServerExtensionDataset input, LinkedHashMap<String, Object> params)  {}
```

#### <a name="method-init"></a>init 方法

Init 方法在构造函数之后，execute 方法之前执行。 需要在 execute(...) 之前执行的任何操作都可以在此方法中进行。

```java
public void init(String sessionId, int taskId, int numtask) {}
```

### <a name="class-abstractsqlserverextensiondataset"></a>类：AbstractSqlServerExtensionDataset

抽象类 AbstractSqlServerExtensionDataset 包含用于处理 Java 扩展所使用的输入和输出数据的接口。


### <a name="class-primitivedataset"></a>类：PrimitiveDataset

类 PrimitiveDataset 是将简单类型存储为基元数组的 AbstractSqlServerExtensionDataset 的实现。

它在 SDK 中仅作为可选的帮助程序类而提供。 如果不使用此类，则需要实现从 AbstractSqlServerExtensionDataset 继承的自己的类。  

## <a name="next-steps"></a>后续步骤

+ [教程：在 Java 中使用正则表达式 (regex) 搜索字符串](../tutorials/search-for-string-using-regular-expressions-in-java.md)
+ [如何在 SQL Server 中调用 Java](call-java-from-sql.md)
