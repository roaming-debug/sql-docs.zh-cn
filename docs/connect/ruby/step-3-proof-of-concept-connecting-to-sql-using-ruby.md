---
title: 步骤 3：使用 Ruby 连接到 SQL 的概念证明
description: 第 3 步是概念证明，展示了如何使用 Ruby 连接到 SQL Server。 基本示例展示了如何选择和插入数据。
ms.custom: ''
ms.date: 06/22/2020
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: conceptual
ms.assetid: cac20b18-0a6d-4243-bbda-a5d1b9476441
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 93c40d3c64f2a32cd9f318dc236c4c350cfddc28
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100058112"
---
# <a name="step-3-proof-of-concept-connecting-to-sql-using-ruby"></a>步骤 3：使用 Ruby 连接到 SQL 的概念证明

应只将此示例视为概念证明。  为清楚起见，示例代码已简化，不一定代表 Microsoft 建议的最佳做法。  
  
## <a name="step-1--connect"></a>步骤 1：连接  
  
[TinyTDS::Client](https://github.com/rails-sqlserver/tiny_tds) 函数用于连接到 SQL 数据库。  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
```  
  
## <a name="step-2--execute-a-query"></a>步骤 2：执行查询  
  
复制以下代码并将它粘贴到空文件中。 将文件命名为 test.rb。 然后，在命令提示符下输入以下命令以执行该文件：  
  
```ruby
    ruby test.rb  
```
  
在代码示例中，[TinyTds::Result](https://github.com/rails-sqlserver/tiny_tds) 函数用于检索针对 SQL 数据库执行的查询所返回的结果集。 此函数接受查询并返回结果集。 可使用 [result.each do |row|](https://github.com/rails-sqlserver/tiny_tds) 循环访问结果集。  
  
```ruby 
    require 'tiny_tds'    
    print 'test'       
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SELECT c.CustomerID, c.CompanyName,COUNT(soh.SalesOrderID) AS OrderCount FROM SalesLT.Customer AS c LEFT OUTER JOIN SalesLT.SalesOrderHeader AS soh ON c.CustomerID = soh.CustomerID GROUP BY c.CustomerID, c.CompanyName ORDER BY OrderCount DESC")  
    results.each do |row|  
    puts row  
    end  
```  
  
## <a name="step-3--insert-a-row"></a>步骤 3：插入行  
  
此示例展示了如何安全执行 [INSERT](../../t-sql/statements/insert-transact-sql.md) 语句，并传递用于保护应用程序免遭 [SQL 注入](../../relational-databases/tables/primary-and-foreign-key-constraints.md)值影响的参数。    
  
若要配合使用 TinyTDS 和 Azure，建议运行多个 `SET` 语句来更改当前会话处理特定信息的方式。 建议使用代码示例中所提供的 `SET` 语句。 例如，即使未显式指定列的可为 null 状态，通过 `SET ANSI_NULL_DFLT_ON` 依然可以创建新列来允许 null 值。  
  
为符合 Microsoft SQL Server [日期时间](../../t-sql/data-types/datetime-transact-sql.md)格式，请使用 [strftime](https://ruby-doc.org/core-2.2.0/Time.html#method-i-strftime) 函数转换成对应的日期时间格式。  
  
```ruby
    require 'tiny_tds'  
    client = TinyTds::Client.new username: 'yourusername@yourserver', password: 'yourpassword',  
    host: 'yourserver.database.windows.net', port: 1433,  
    database: 'AdventureWorks', azure:true  
    results = client.execute("SET ANSI_NULLS ON")  
    results = client.execute("SET CURSOR_CLOSE_ON_COMMIT OFF")  
    results = client.execute("SET ANSI_NULL_DFLT_ON ON")  
    results = client.execute("SET IMPLICIT_TRANSACTIONS OFF")  
    results = client.execute("SET ANSI_PADDING ON")  
    results = client.execute("SET QUOTED_IDENTIFIER ON")  
    results = client.execute("SET ANSI_WARNINGS ON")  
    results = client.execute("SET CONCAT_NULL_YIELDS_NULL ON")  
    require 'date'  
    t = Time.now  
    curr_date = t.strftime("%Y-%m-%d %H:%M:%S.%L")  
    results = client.execute("INSERT SalesLT.Product (Name, ProductNumber, StandardCost, ListPrice, SellStartDate)  
    OUTPUT INSERTED.ProductID VALUES ('SQL Server Express New', 'SQLEXPRESS New', 0, 0, '#{curr_date}' )")  
    results.each do |row|  
    puts row  
    end  
```
