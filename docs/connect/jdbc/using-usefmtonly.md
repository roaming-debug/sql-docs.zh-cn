---
description: 通过 useFmtOnly 检索 ParameterMetaData
title: 通过 useFmtOnly 检索 ParameterMetaData | Microsoft Docs
ms.custom: ''
ms.date: 08/12/2019
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: v-daenge
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: ''
caps.latest.revision: ''
author: rene-ye
ms.author: v-reye
manager: kenvh
ms.openlocfilehash: a88707cc4679343c2e29a86ecc3a9efb8a9719dd
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101837447"
---
# <a name="retrieving-parametermetadata-via-usefmtonly"></a>通过 useFmtOnly 检索 ParameterMetaData
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  The Microsoft JDBC Driver for SQL Server 提供从服务器查询参数元数据的另一种方法，即 useFmtOnly。 此功能是在驱动程序版本 7.4 中首次引入的，需要作为 `sp_describe_undeclared_parameters` 中已知问题的解决方法。
  
  驱动程序主要使用存储过程 `sp_describe_undeclared_parameters` 来查询参数元数据，因为在大多数情况下，这是用于检索参数元数据的推荐方法。 但是，在以下用例中，当前执行存储过程会失败：
  
-   针对 Always Encrypted 列
  
-   针对临时表和表变量
  
-   针对视图 
  
  针对这些用例的建议解决方案是，分析用户的参数和表目标的 SQL 查询，然后在启用 `FMTONLY` 的情况下执行 `SELECT` 查询。 以下代码片段具体演示了此功能。
  
```sql
--create a normal table 'Foo' and a temporary table 'Bar'
CREATE TABLE Foo(c1 int);
CREATE TABLE #Bar(c1 int);

EXEC sp_describe_undeclared_parameters N'SELECT * FROM Foo WHERE c1 = @p0' --works fine
EXEC sp_describe_undeclared_parameters N'SELECT * FROM #Bar WHERE c1 = @p0' --fails with "Invalid object name '#Bar'"

SET FMTONLY ON;
SELECT c1 FROM Foo; --works
SET FMTONLY OFF;
SET FMTONLY ON;
SELECT c1 FROM #Bar; --works
SET FMTONLY OFF;
```
 
## <a name="turning-the-feature-onoff"></a>启用/禁用功能 
 默认情况下，**useFmtOnly** 功能处于禁用状态。 用户可以通过指定 `useFmtOnly=true` 在连接字符串中启用此功能。 例如：`jdbc:sqlserver://<server>:<port>;databaseName=<databaseName>;user=<user>;password=<password>;useFmtOnly=true;`。
 
 或者，通过 `SQLServerDataSource` 提供该功能。
 ```java
SQLServerDataSource ds = new SQLServerDataSource();
ds.setServerName(<server>);
ds.setPortNumber(<port>);
ds.setDatabaseName("<databaseName>");
ds.setUser("<user>");
ds.setPassword("<password>");
ds.setUseFmtOnly(true);
try (Connection c = ds.getConnection()) {
    // do work with connection
}
 ```
 
 此功能也可在语句级别提供。 用户可以通过 `PreparedStatement.setUseFmtOnly(boolean)` 启用/禁用该功能。
> [!NOTE]  
>  驱动程序将对“语句级别”属性和“连接级别”属性设置优先级。

## <a name="using-the-feature"></a>使用功能
  启用新功能后，驱动程序将在内部开始使用该功能，而不是在查询参数元数据时使用 `sp_describe_undeclared_parameters`。 最终用户不需要执行任何其他操作。
```java
final String sql = "INSERT INTO #Bar VALUES (?)";
try (Connection c = DriverManager.getConnection(URL, USERNAME, PASSWORD)) {
    try (Statement s = c.createStatement()) {
        s.execute("CREATE TABLE #Bar(c1 int)");
    }
    try (PreparedStatement p1 = c.prepareStatement(sql); PreparedStatement p2 = c.prepareStatement(sql)) {
        ((SQLServerPreparedStatement) p1).setUseFmtOnly(true);
        ParameterMetaData pmd1 = p1.getParameterMetaData();
        System.out.println(pmd1.getParameterTypeName(1)); // prints int
        ParameterMetaData pmd2 = p2.getParameterMetaData(); // throws exception, Invalid object name '#Bar'
    }
}
```
> [!NOTE]  
>  该功能仅支持 `SELECT/INSERT/UPDATE/DELETE` 查询。 查询应以 4 个支持的关键字之一或[公用表表达式](../../t-sql/queries/with-common-table-expression-transact-sql.md)开头，后跟一个支持的查询。 不支持使用公用表表达式中的参数。

## <a name="known-issues"></a>已知问题
  此功能当前存在一些问题，是由 SQL 分析逻辑中的缺陷导致的。 这些问题可能会在以后对该功能的更新中得到解决，现已在下面列出，并提供了解决方法建议。
  
A. 使用“前向声明”别名
```sql
CREATE TABLE Foo(c1 int)

DELETE fooAlias FROM Foo fooAlias WHERE c1 > ?; --Invalid object name 'fooAlias'

--Workaround #1: Specify AS keyword
DELETE fooAlias FROM Foo AS fooAlias WHERE c1 > ?;
--Workaround #2: Use the table name
DELETE Foo FROM Foo fooAlias WHERE c1 > ?;
```

B. 当表具有共享的多个列名时，列名不明确
```sql
CREATE TABLE Foo(c1 int, c2 int, c3 int)
CREATE TABLE Bar(c1 int, c2 int, c3 int)

SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar WHERE c1 > ? and c2 < ? and c3 = ?); --Ambiguous Column Name

--Workaround: Use aliases
SELECT c1,c2 FROM Foo WHERE c3 IN (SELECT c3 FROM Bar b WHERE b.c1 = ? and b.c2 = ? and b.c3 = ?);
```

C. 从包含参数的子查询中 SELECT
```sql

CREATE TABLE Foo(c1 int)

SELECT * FROM (SELECT * FROM Foo WHERE c1 = ?) WHERE c1 = ?; --Incorrect syntax near '?'

--Workaround: N/A
```

D. SET 子句中存在子查询
```sql
CREATE TABLE Foo(c1 int)

UPDATE Foo SET c1 = (SELECT c1 FROM Foo) WHERE c1 = ?; --Incorrect syntax near ')'

--Workaround: Add a 'delimiting' condition
UPDATE Foo SET c1 = (SELECT c1 FROM Foo HAVING (HASH JOIN)) WHERE c1 = ?;
```

## <a name="see-also"></a>另请参阅  
 [设置连接属性](../../connect/jdbc/setting-the-connection-properties.md)  
  
