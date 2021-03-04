---
description: 分析结果
title: 分析结果 | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: 7b56223594d1126c373b4fd8b24dabc1e81dab67
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837076"
---
# <a name="parsing-the-results"></a>分析结果

[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

本文介绍 SQL Server 如何期望用户完全处理从任何查询返回的结果。

## <a name="update-counts-and-result-sets"></a>更新计数和结果集

本节将讨论从 SQL Server 返回的两个最常见的结果：Update Count 和 ResultSet。 通常，用户执行的任何查询都将导致返回其中的一个结果，在处理结果时，用户应同时处理这两种结果。

下面的代码示例说明用户如何循环访问来自服务器的所有结果：
```java
try (Connection connection = DriverManager.getConnection(URL); Statement statement = connection.createStatement()) {
    boolean resultsAvailable = statement.execute(USER_SQL);
    int updateCount = -2;
    while (true) {
        updateCount = statement.getUpdateCount();
        if (!resultsAvailable && updateCount == -1)
            break;
        if (resultsAvailable) {
            // handle ResultSet
        } else {
            // handle Update Count
        }
        resultsAvailable = statement.getMoreResults();
    }
}
```

## <a name="exceptions"></a>例外
在执行导致错误或信息性消息的语句时，SQL Server 的响应方式将有所不同，具体取决于它是否可以生成执行计划。 在执行语句后可能会立即引发错误消息，也可能需要单独的结果集才能引发。 在后一种情况下，应用程序需要分析结果集以检索异常。

如果 SQL Server 无法生成执行计划，将立即引发异常。

```java
String SQL = "SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    statement.execute(SQL);
} catch (SQLException e) {
    e.printStackTrace();
}
```

如果 SQL Server 在结果集中返回错误消息，则需要处理结果集以检索异常。

```java
String SQL = "SELECT 1/0;";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    if (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            // Exception is thrown on next().
            while (rs.next()) {}
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

如果语句执行生成了多个结果集，则需要处理每个结果集，直到发现异常结果集。

```java
String SQL = "SELECT 1; SELECT * FROM nonexistentTable;";
try (Statement statement = connection.createStatement();) {
    // Does not throw an exception on execute().
    boolean hasResult = statement.execute(SQL);
    while (hasResult) {
        try (ResultSet rs = statement.getResultSet()) {
            while (rs.next()) {
                System.out.println(rs.getString(1));
            }
        }
        // Moves the next result set that generates the exception.
        hasResult = statement.getMoreResults();
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```

在 `String SQL = "SELECT * FROM nonexistentTable; SELECT 1;";` 情况下，会立即在 `execute()` 时引发异常，并且完全不会执行 `SELECT 1`。

如果来自 SQL Server 的错误严重性级别为 `9` 到 `0`，则将其视为信息性消息并作为 `SQLWarning` 返回。

```java
String SQL = "RAISERROR ('WarningLevel5', 5, 2);";
try (Statement statement = connection.createStatement();) {
    boolean hasResult = statement.execute(SQL);
    SQLWarning warning = statement.getWarnings();
    System.out.println(warning);
}
```

## <a name="see-also"></a>另请参阅

[JDBC 驱动程序概述](../../connect/jdbc/overview-of-the-jdbc-driver.md)
