---
description: NEWID (Transact-SQL)
title: NEWID (Transact-SQL) | Microsoft Docs
ms.custom: ''
ms.date: 07/29/2017
ms.prod: sql
ms.prod_service: sql-data-warehouse, sql-database
ms.reviewer: ''
ms.technology: t-sql
ms.topic: language-reference
f1_keywords:
- NEWID
- NEWID_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- uniqueidentifier data type
- NEWID function
ms.assetid: f7014e60-96d5-457e-afc3-72b60ba20c0f
author: julieMSFT
ms.author: jrasnick
monikerRange: =azure-sqldw-latest||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: eb2e8c2d00ef42a43ea32ab55df6b14cb13d5dc1
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "91111073"
---
# <a name="newid-transact-sql"></a>NEWID (Transact-SQL)

[!INCLUDE [sql-asdb-asdbmi-asa-pdw](../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

创建 uniqueidentifier 类型的唯一值。  
  
![主题链接图标](../../database-engine/configure-windows/media/topic-link.gif "“主题链接”图标") [Transact-SQL 语法约定](../../t-sql/language-elements/transact-sql-syntax-conventions-transact-sql.md)  
  
## <a name="syntax"></a>语法  
  
```syntaxsql 
NEWID ( )  
```  
  
[!INCLUDE[sql-server-tsql-previous-offline-documentation](../../includes/sql-server-tsql-previous-offline-documentation.md)]

## <a name="return-types"></a>返回类型
 **uniqueidentifier**  
  
## <a name="remarks"></a>注解  
 `NEWID()` 遵从 RFC4122。  
  
## <a name="examples"></a>示例  
  
### <a name="a-using-the-newid-function-with-a-variable"></a>A. 对变量使用 NEWID 函数  
 以下示例使用 `NEWID()` 对声明为 uniqueidentifier 数据类型的变量赋值。 在测试 uniqueidentifier 数据类型变量的值之前，先打印该值。  
  
```sql
-- Creating a local variable with DECLARE/SET syntax.  
DECLARE @myid uniqueidentifier  
SET @myid = NEWID()  
PRINT 'Value of @myid is: '+ CONVERT(varchar(255), @myid)  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
Value of @myid is: 6F9619FF-8B86-D011-B42D-00C04FC964FF  
```  
  
> [!NOTE]  
>  NEWID 对每台计算机返回的值各不相同。 所显示的数字仅起解释说明的作用。  
  
### <a name="b-using-newid-in-a-create-table-statement"></a>B. 在 CREATE TABLE 语句中使用 NEWID  
  
适用范围：[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]
  
 以下示例创建数据类型为 uniqueidentifier 的 `cust` 表，并使用 NEWID 作为默认值填充该表。 为 `NEWID()` 赋予默认值时，每个新行和现有行均对 `CustomerID` 列具有唯一值。  
  
```sql
-- Creating a table using NEWID for uniqueidentifier data type.  
CREATE TABLE cust  
(  
 CustomerID uniqueidentifier NOT NULL  
   DEFAULT newid(),  
 Company VARCHAR(30) NOT NULL,  
 ContactName VARCHAR(60) NOT NULL,   
 Address VARCHAR(30) NOT NULL,   
 City VARCHAR(30) NOT NULL,  
 StateProvince VARCHAR(10) NULL,  
 PostalCode VARCHAR(10) NOT NULL,   
 CountryRegion VARCHAR(20) NOT NULL,   
 Telephone VARCHAR(15) NOT NULL,  
 Fax VARCHAR(15) NULL  
);  
GO  
-- Inserting 5 rows into cust table.  
INSERT cust  
(Company, ContactName, Address, City, StateProvince,   
 PostalCode, CountryRegion, Telephone, Fax)  
VALUES  
 ('Wartian Herkku', 'Pirkko Koskitalo', 'Torikatu 38', 'Oulu', NULL,  
 '90110', 'Finland', '981-443655', '981-443655')  
,('Wellington Importadora', 'Paula Parente', 'Rua do Mercado, 12', 'Resende', 'SP',  
 '08737-363', 'Brasil', '(14) 555-8122', '')  
,('Cactus Comidas para Ilevar', 'Patricio Simpson', 'Cerrito 333', 'Buenos Aires', NULL,   
 '1010', 'Argentina', '(1) 135-5555', '(1) 135-4892')  
,('Ernst Handel', 'Roland Mendel', 'Kirchgasse 6', 'Graz', NULL,  
 '8010', 'Austria', '7675-3425', '7675-3426')  
,('Maison Dewey', 'Catherine Dewey', 'Rue Joseph-Bens 532', 'Bruxelles', NULL,  
 'B-1180', 'Belgium', '(02) 201 24 67', '(02) 201 24 68');  
GO
```  
  
### <a name="c-using-uniqueidentifier-and-variable-assignment"></a>C. 使用 uniqueidentifier 和变量赋值  
 以下示例将名为 `@myid` 的局部变量声明为 uniqueidentifier 数据类型的变量。 然后使用 `SET` 语句为该变量赋值。  
  
```sql  
DECLARE @myid uniqueidentifier ;  
SET @myid = 'A972C577-DFB0-064E-1189-0154C99310DAAC12';  
SELECT @myid;  
GO  
```  
  
## <a name="see-also"></a>另请参阅  
 [NEWSEQUENTIALID (Transact-SQL)](../../t-sql/functions/newsequentialid-transact-sql.md)   
 [ALTER TABLE (Transact-SQL)](../../t-sql/statements/alter-table-transact-sql.md)   
 [CAST 和 CONVERT (Transact-SQL)](../../t-sql/functions/cast-and-convert-transact-sql.md)   
 [CREATE TABLE (Transact-SQL)](../../t-sql/statements/create-table-transact-sql.md)   
 [数据类型 (Transact-SQL)](../../t-sql/data-types/data-types-transact-sql.md)   
 [System Functions (Transact-SQL)](../../relational-databases/system-functions/system-functions-category-transact-sql.md)   
 [uniqueidentifier (Transact-SQL)](../../t-sql/data-types/uniqueidentifier-transact-sql.md)   
 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)  
  
  
