---
title: 迁移计算列 | Microsoft Docs
description: 了解如何在内存优化表中模拟计算列。 评估迁移后是否需要计算列功能。
ms.custom: ''
ms.date: 12/17/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: in-memory-oltp
ms.topic: conceptual
ms.assetid: 64a9eade-22c3-4a9d-ab50-956219e08df1
author: MightyPen
ms.author: genemi
monikerRange: =sql-server-2016
ms.openlocfilehash: 95f920fc8648ed646f1056c9d334ee831651f5e2
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465368"
---
# <a name="migrating-computed-columns"></a>迁移计算列

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

内存优化的表中不支持计算列。 但是，可模拟计算列。

从 [!INCLUDE[ssSQLv14_md](../../includes/sssqlv14-md.md)] 开始，内存优化表和索引中支持计算列。

将基于磁盘的表迁移到内存优化的表时，应考虑是否需要使计算列持久化。 内存优化的表和本机编译的存储过程的性能特征不同，因此可能不需要持久化。  
  
## <a name="non-persisted-computed-columns"></a>非持久化计算列  
 若要模拟非持久化计算列的效果，请在内存优化的表上创建一个视图。 在定义该视图的 SELECT 语句中，将计算列定义添加到该视图中。 除了在本机编译的存储过程中以外，使用计算列中的值的查询应从该视图进行读取。 在本机编译的存储过程中，应根据计算列定义更新任何 select、update 或 delete 语句。  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is not persisted.  
CREATE VIEW dbo.v_order_details AS  
   SELECT  
      OrderId,  
      ProductId,  
      SalePrice,  
      Quantity,  
      Quantity * SalePrice AS Total  
   FROM dbo.order_details  
```  
  
## <a name="persisted-computed-columns"></a>持久化计算列  
 若要模拟持久化计算列的效果，请创建一个存储过程用于插入到表中，创建另一个存储过程用于更新表。 插入或更新表时，调用这些存储过程以执行这些任务。 在存储过程中，根据输入内容算出计算字段的值，方式很像在基于磁盘的原始表上定义计算列。 然后，在存储过程中按需插入或更新表。  
  
```sql  
-- Schema for the table dbo.OrderDetails:  
-- OrderId int not null primary key,  
-- ProductId int not null,  
-- SalePrice money not null,  
-- Quantity int not null,  
-- Total money not null  
--  
-- Total is computed as SalePrice * Quantity and is persisted.  
-- we need to create insert and update procedures to calculate Total.  

CREATE PROCEDURE sp_insert_order_details   
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  
-- for bulk inserts, accept a table-valued parameter into the stored procedure  
-- and use an INSERT INTO SELECT statement.  

DECLARE @total money = @SalePrice * @Quantity  
INSERT INTO dbo.OrderDetails (OrderId, ProductId, SalePrice, Quantity, Total)  
VALUES (@OrderId, @ProductId, @SalePrice, @Quantity, @total)  
END  
GO  
  
CREATE PROCEDURE sp_update_order_details_by_id  
@OrderId int, @ProductId int, @SalePrice money, @Quantity int  
WITH NATIVE_COMPILATION, SCHEMABINDING, EXECUTE AS OWNER  
AS BEGIN ATOMIC WITH (LANGUAGE = N'english', TRANSACTION ISOLATION LEVEL = SNAPSHOT)  

-- compute the value here.   
-- this stored procedure works with single rows only.  

DECLARE @total money = @SalePrice * @Quantity  
UPDATE dbo.OrderDetails   
SET ProductId = @ProductId, SalePrice = @SalePrice, Quantity = @Quantity, Total = @total  
WHERE OrderId = @OrderId  
END  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [迁移到内存中 OLTP](./plan-your-adoption-of-in-memory-oltp-features-in-sql-server.md)  
  
