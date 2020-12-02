---
description: 在 SQL Server 和客户端应用中使用 FOR JSON 输出 (SQL Server)
title: 在 SQL Server 和客户端应用中使用 FOR JSON 输出
ms.date: 06/03/2020
ms.prod: sql
ms.technology: ''
ms.topic: conceptual
helpviewer_keywords:
- FOR JSON, using in client apps
- FOR JSON, using in SQL Server
ms.assetid: 302e5397-b499-4ea3-9a7f-c24ccad698eb
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: jroth
ms.custom: seo-dt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: fb663a8ccc5dc08b186abce1f90bb4ce55b89ed4
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "89386008"
---
# <a name="use-for-json-output-in-sql-server-and-in-client-apps-sql-server"></a>在 SQL Server 和客户端应用中使用 FOR JSON 输出 (SQL Server)
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

下面的示例演示了在 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或客户端应用中使用 FOR JSON 子句或其 JSON 输出的几种方式。  
  
## <a name="use-for-json-output-in-sql-server-variables"></a>在 SQL Server 变量中使用 FOR JSON 输出  
FOR JSON 子句的输出类型为 NVARCHAR(MAX)，因此可以将它赋给任何变量，如下面的示例中所示。  
  
```sql  
DECLARE @x NVARCHAR(MAX) =
  (SELECT TOP 10 *
     FROM Sales.SalesOrderHeader
     FOR JSON AUTO)  
```  
  
## <a name="use-for-json-output-in-sql-server-user-defined-functions"></a>在 SQL Server 用户定义函数中使用 FOR JSON 输出  
 你可以创建将结果集格式化为 JSON 并返回此 JSON 输出的用户定义函数。 下面的示例创建一个用户定义函数，该函数提取一些销售订单详细信息行，并将它们格式化为 JSON 数组。  
  
```sql  
CREATE FUNCTION GetSalesOrderDetails(@salesOrderId int)  
 RETURNS NVARCHAR(MAX)  
AS  
BEGIN  
   RETURN (SELECT UnitPrice, OrderQty  
           FROM Sales.SalesOrderDetail  
           WHERE SalesOrderID = @salesOrderId  
           FOR JSON AUTO)  
END
```  
  
 你可以在批或查询中使用此函数，如下面的示例中所示。  
  
```sql  
DECLARE @x NVARCHAR(MAX) = dbo.GetSalesOrderDetails(43659)

PRINT dbo.GetSalesOrderDetails(43659)

SELECT TOP 10
  H.*, dbo.GetSalesOrderDetails(H.SalesOrderId) AS Details
FROM Sales.SalesOrderHeader H
```  
  
## <a name="merge-parent-and-child-data-into-a-single-table"></a>将父数据和子数据合并到单个表中  
下面的示例将每组子行格式化为 JSON 数组。 JSON 数组将成为父表中“详细信息”列的值。  
  
```sql  
SELECT TOP 10 SalesOrderId, OrderDate,  
      (SELECT TOP 3 UnitPrice, OrderQty  
         FROM Sales.SalesOrderDetail D  
         WHERE H.SalesOrderId = D.SalesOrderID  
         FOR JSON AUTO) AS Details  
INTO SalesOrder  
FROM Sales.SalesOrderHeader H  
```  
  
## <a name="update-the-data-in-json-columns"></a>更新 JSON 列中的数据  
 下面的示例演示如何更新包含 JSON 文本的列的值。  
  
```sql  
UPDATE SalesOrder  
SET Details =  
     (SELECT TOP 1 UnitPrice, OrderQty  
       FROM Sales.SalesOrderDetail D  
       WHERE D.SalesOrderId = SalesOrder.SalesOrderId  
      FOR JSON AUTO) 
```  
  
## <a name="use-for-json-output-in-a-c-client-app"></a>在 C# 客户端应用中使用 FOR JSON 输出  
 下面的示例演示如何在 C# 客户端应用中将查询的 JSON 输出检索到 StringBuilder 对象中。 假设变量 `queryWithForJson` 包含带有 FOR JSON 子句的 SELECT 语句的文本。  
  
```csharp  
var queryWithForJson = "SELECT ... FOR JSON";
var conn = new SqlConnection("<connection string>");
var cmd = new SqlCommand(queryWithForJson, conn);
conn.Open();
var jsonResult = new StringBuilder();
var reader = cmd.ExecuteReader();
if (!reader.HasRows)
{
    jsonResult.Append("[]");
}
else
{
    while (reader.Read())
    {
        jsonResult.Append(reader.GetValue(0).ToString());
    }
}
```  

## <a name="learn-more-about-json-in-sql-server-and-azure-sql-database"></a>详细了解 SQL Server 和 Azure SQL 数据库中的 JSON  
  
### <a name="microsoft-videos"></a>Microsoft 视频

有关 SQL Server 和 Azure SQL 数据库中内置 JSON 支持的视频介绍，请观看以下视频：

-   [SQL Server 2016 和 JSON 支持](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

-   [在 SQL Server 2016 和 Azure SQL 数据库中使用 JSON](https://channel9.msdn.com/Shows/Data-Exposed/Using-JSON-in-SQL-Server-2016-and-Azure-SQL-Database)

-   [JSON 充当 NoSQL 和关系环境之间的桥梁](https://channel9.msdn.com/events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds)
 
## <a name="see-also"></a>另请参阅  
 [借助 FOR JSON 将查询结果的格式设置为 JSON (SQL Server)](../../relational-databases/json/format-query-results-as-json-with-for-json-sql-server.md)  
  
  
