---
description: HOST_NAME (Transact-SQL)
title: HOST_NAME (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 09/21/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: reference
f1_keywords:
- HOST_NAME_TSQL
- HOST_NAME
dev_langs:
- TSQL
helpviewer_keywords:
- HOST_NAME function
- workstation names [SQL Server]
ms.assetid: 4b8b0705-c083-4b07-b954-c83ee73b2ebb
author: VanMSFT
ms.author: vanto
ms.openlocfilehash: 2cb8c8cc0b9945ba3ebc4b4c49ca9872a50469fa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99203577"
---
# <a name="host_name-transact-sql"></a>HOST_NAME (Transact-SQL)

[!INCLUDE [sqlserver2016-asdb-asdbmi](../../includes/applies-to-version/sqlserver2016-asdb-asdbmi.md)]

  返回工作站名。  
  
 ![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql
HOST_NAME ()  
```  

## <a name="return-types"></a>返回类型
 **nvarchar(128)**  
  
## <a name="remarks"></a>注解  
 当该参数对系统函数可选时，则采用当前数据库、主机、服务器用户或数据库用户。 内置函数后面必须跟括号。  
  
 系统函数可以在选择列表、WHERE 子句和任何允许使用表达式的地方使用。  
  
> [!IMPORTANT]  
>  客户端应用程序提供工作站名称，也会提供不准确的数据。 不要将 HOST_NAME 作为安全功能使用。  
  
## <a name="examples"></a>示例  
 以下示例创建一个表，该表在 `HOST_NAME()` 定义中使用 `DEFAULT` 来记录那些向记录顺序的表中插入行的计算机工作站的名称。  
  
```sql  
CREATE TABLE Orders  
   (OrderID     INT        PRIMARY KEY,  
    CustomerID  NCHAR(5)   REFERENCES Customers(CustomerID),  
    Workstation NCHAR(30)  NOT NULL DEFAULT HOST_NAME(),  
    OrderDate   DATETIME   NOT NULL,  
    ShipDate    DATETIME   NULL,  
    ShipperID   INT        NULL REFERENCES Shippers(ShipperID));  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [表达式 (Transact-SQL)](../../t-sql/language-elements/expressions-transact-sql.md)   
 [系统函数 (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)  
  
  
