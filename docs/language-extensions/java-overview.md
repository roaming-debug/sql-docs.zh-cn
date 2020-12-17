---
title: 什么是 Java 语言扩展？
titleSuffix: SQL Server Language Extensions
description: Java 语言扩展是 SQL Server 的一项功能，用于执行外部 Java 代码。 可以使用扩展性框架在外部 Java 代码中使用关系数据。
author: dphansen
ms.author: davidph
ms.date: 11/10/2020
ms.topic: overview
ms.prod: sql
ms.technology: language-extensions
monikerRange: '>=sql-server-ver15||>=sql-server-linux-ver15'
ms.openlocfilehash: f68821900b2e304028bccfd79e96f988f02267e9
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97471718"
---
# <a name="what-is-java-language-extension"></a>什么是 Java 语言扩展？
[!INCLUDE [SQL Server 2019 and later](../includes/applies-to-version/sqlserver2019.md)]

Java 语言扩展是 SQL Server 的一项功能，用于执行外部 Java 代码。 可以使用[扩展性框架](concepts/extensibility-framework.md)在外部 Java 代码中使用关系数据。 Java 语言扩展是 [SQL Server 语言扩展](language-extensions-overview.md)的一部分。

默认的 Java 运行时为 Zulu Open JRE。 此外，也可以使用其他 Java JRE 或 SDK。

## <a name="what-you-can-do-with-the-java-language-extension"></a>使用 Java 语言扩展可执行的操作

Java 语言扩展使用扩展性框架来执行外部 Java 代码。 代码执行与核心引擎进程隔离，但与 SQL Server 查询执行完全集成。 你可以在数据的源中执行 Java 代码，而无需通过网络提取数据。

外部 Java 语言通过 [CREATE EXTERNAL LANGUAGE](https://docs.microsoft.com/sql/t-sql/statements/create-external-language-transact-sql) 定义。 系统存储过程 [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) 用作执行 Java 代码的接口。

## <a name="get-started-with-java-language-extension"></a>Java 语言扩展入门

1. 在 [Windows](install/windows-java.md) 或 [Linux](../linux/sql-server-linux-setup-language-extensions-java.md) 上安装 SQL Server Java 语言扩展。

1. 配置开发工具。

    + 使用喜欢的 IDE 来开发 Java 代码。
    + 安装[用于 Java 的 Microsoft 扩展性 SDK](how-to/extensibility-sdk-java-sql-server.md) 以在 SQL Server 上执行 Java 代码。
    + 使用 [Azure Data Studio](../azure-data-studio/what-is.md) 在 SQL Server 上执行外部代码。
    + 使用系统存储过程 [sp_execute_external_script](https://docs.microsoft.com/sql/relational-databases/system-stored-procedures/sp-execute-external-script-transact-sql) 在 SQL Server 上执行 Java 代码。

1. 编写第一个 Java 代码。

    + [教程：正则表达式与 Java](tutorials/search-for-string-using-regular-expressions-in-java.md)

## <a name="limitations"></a>限制

输入和输出缓冲区中的值数量不得超过 `MAX_INT (2^31-1)`，因为这是 Java 数组中可以分配的最大元素数量。

## <a name="next-steps"></a>后续步骤

+ 在 [Windows](install/windows-java.md) 或 [Linux](../linux/sql-server-linux-setup-language-extensions-java.md) 上安装 SQL Server Java 语言扩展
+ 安装[用于 Java 的 Microsoft 扩展性 SDK](how-to/extensibility-sdk-java-sql-server.md)
