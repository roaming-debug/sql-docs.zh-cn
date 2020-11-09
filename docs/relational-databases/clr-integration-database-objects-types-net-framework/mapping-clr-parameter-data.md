---
title: 映射 CLR 参数数据 |Microsoft Docs
description: 本文列出了 Microsoft SQL Server 数据类型、CLR 中用于 SQL Server 的等效项和 .NET Framework 中的本机 CLR 等效项。
ms.custom: ''
ms.date: 08/01/2016
ms.prod: sql
ms.reviewer: ''
ms.technology: clr
ms.topic: reference
helpviewer_keywords:
- SqlBinary data type
- SqlInt16 data type
- SqlMoney data type
- SqlString data type
- SqlSingle data type
- data types [CLR integration]
- SqlInt64 data type
- SqlDateTime data type
- SqlXml data type
- SqlBoolean data type
- SqlDecimal data type
- SqlBytes data type
- mapping data types [CLR integration]
- SqlChars data type
- SqlInt32 data type
ms.assetid: 89b43ee9-b9ad-4281-a4bf-c7c8d116daa2
author: rothja
ms.author: jroth
ms.openlocfilehash: 59cc4c80781f899701f872bd1e8cdd1eea823358
ms.sourcegitcommit: 49ee3d388ddb52ed9cf78d42cff7797ad6d668f2
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/09/2020
ms.locfileid: "94384720"
---
# <a name="mapping-clr-parameter-data"></a>映射 CLR 参数数据
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  下表列出了 [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] SqlTypes 命名空间中的数据类型、它们在公共语言运行时中的等效项 (CLR) [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，以及它们在 .NET Framework 中的本机 clr 等效项 **System.Data.SqlTypes** [!INCLUDE[msCoName](../../includes/msconame-md.md)] 。  
  
||||  
|-|-|-|  
|**SQL Server 数据类型**|类型（在 System.Data.SqlTypes 或 Microsoft.SqlServer.Types 中）|**CLR 数据类型 (.NET Framework)**|  
|**bigint**|**SqlInt64**|**Int64，可以为 Null\<Int64>**|  
|**binary**|**SqlBytes、SqlBinary**|**Byte[]**|  
|**bit**|**SqlBoolean**|**布尔值，可以为 Null\<Boolean>**|  
|**char**|无|无|  
|**cursor**|无|无|  
|**date**|**SqlDateTime**|**DateTime，可以为 Null\<DateTime>**|  
|**datetime**|**SqlDateTime**|**DateTime，可以为 Null\<DateTime>**|  
|**datetime2**|无|**DateTime，可以为 Null\<DateTime>**|  
|DATETIMEOFFSET|**无**|**DateTimeOffset，可以为 Null\<DateTimeOffset>**|  
|**decimal**|**SqlDecimal**|**十进制，可以为 Null\<Decimal>**|  
|**float**|**SqlDouble**|**Double，可以为 Null\<Double>**|  
|**地理**|**SqlGeography**<br /><br /> 在 Microsoft.SqlServer.Types.dll 中定义 **SqlGeography** ，它与 SQL Server 一起安装，并且可以从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [功能包](https://www.microsoft.com/download/details.aspx?id=100430)下载。|无|  
|**geometry**|**SqlGeometry**<br /><br /> 在 Microsoft.SqlServer.Types.dll 中定义 **SqlGeometry** ，它与 SQL Server 一起安装，并且可以从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [功能包](https://www.microsoft.com/download/details.aspx?id=100430)下载。|无|  
|**hierarchyid**|**SqlHierarchyId**<br /><br /> 在 Microsoft.SqlServer.Types.dll 中定义 **SqlHierarchyId** ，它与 SQL Server 一起安装，并且可以从 [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)] [功能包](https://www.microsoft.com/download/details.aspx?id=100430)下载。|无|  
|**图像**|无|无|  
|**int**|**SqlInt32**|**Int32，可以为 Null\<Int32>**|  
|**money**|**SqlMoney**|**十进制，可以为 Null\<Decimal>**|  
|**nchar**|**SqlChars、SqlString**|**String, Char[]**|  
|**ntext**|无|无|  
|**numeric**|**SqlDecimal**|**十进制，可以为 Null\<Decimal>**|  
|**nvarchar**|**SqlChars、SqlString**<br /><br /> **SQLChars** 是用于数据传输和访问的更好的匹配项， **SQLString** 是用于执行字符串操作的更好的匹配项。|**String, Char[]**|  
|**nvarchar (1) ，nchar (1)**|**SqlChars、SqlString**|**Char、String、Char []，可以为 Null\<char>**|  
|**real**|**SqlSingle** ( **SqlSingle** 的范围，但大于 **实** ) |**单个，可以为 Null\<Single>**|  
|**rowversion**|无|**Byte[]**|  
|**smallint**|**SqlInt16**|**Int16，可为 Null\<Int16>**|  
|**smallmoney**|**SqlMoney**|**十进制，可以为 Null\<Decimal>**|  
|**sql_variant**|无|**对象**|  
|**table**|无|无|  
|**text**|无|无|  
|**time**|无|**TimeSpan，可以为 Null\<TimeSpan>**|  
|**timestamp**|无|无|  
|**tinyint**|**SqlByte**|**字节，可以为 Null\<Byte>**|  
|**uniqueidentifier**|**SqlGuid**|**Guid，可以为 Null\<Guid>**|  
|**(UDT 的用户定义类型)**|无|绑定到相同程序集或依赖程序集中的用户定义类型的相同类。|  
|**varbinary**|**SqlBytes、SqlBinary**|**Byte[]**|  
|**varbinary (1) ，binary (1)**|**SqlBytes、SqlBinary**|**byte、Byte []、可以为 Null\<byte>**|  
|**varchar**|无|无|  
|**xml**|**SqlXml**|无|  
  
## <a name="automatic-data-type-conversion-with-out-parameters"></a>使用 Out 参数的自动数据类型转换  
 CLR 方法可以通过使用 **out** 修饰符将输入参数标记 (Microsoft Visual c # ) 或 **\<Out()> ByRef** (Microsoft) Visual Basic 来将信息返回到调用代码或程序。如果输入参数是 **SqlTypes** 命名空间中的 CLR 数据类型，并且调用程序将其等效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型指定为输入参数，则当 CLR 方法返回数据类型时，将自动发生类型转换。  
  
 例如，以下 CLR 存储过程具有 **SqlInt32** CLR 数据类型的输入参数，该参数标记有 **Out** (c # ) 或 **\<Out()> ByRef** (Visual Basic) ：  
  
```csharp  
[Microsoft.SqlServer.Server.SqlProcedure]  
public static void PriceSum(out SqlInt32 value)  
{ ... }  
```  
  
```vb  
\<Microsoft.SqlServer.Server.SqlProcedure> _  
Public Shared Sub PriceSum( \<Out()> ByRef value As SqlInt32)  
...  
End Sub  
```  
  
 在数据库中生成并创建了程序集之后，将在中创建存储过程，其中 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 包含以下 transact-sql，后者将 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] **int** 数据类型指定为 OUTPUT 参数：  
  
```  
CREATE PROCEDURE PriceSum (@sum int OUTPUT)  
AS EXTERNAL NAME TestStoredProc.StoredProcedures.PriceSum  
```  
  
 调用 CLR 存储过程时， **SqlInt32** 数据类型会自动转换为 **int** 数据类型并返回给调用程序。  
  
 但是，并非所有 CLR 数据类型都可以通过 out 参数自动转换到其等效的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 数据类型。 下表列出了这些例外类型。  
  
|||  
|-|-|  
|**CLR 数据类型 (SQL Server)**|**SQL Server 数据类型**|  
|**小数**|smallmoney|  
|**SqlMoney**|smallmoney|  
|**小数**|money|  
|**DateTime**|smalldatetime|  
|**SQLDateTime**|smalldatetime|  
  
## <a name="change-history"></a>更改历史记录  
  
|更新的内容|  
|---------------------|  
|向映射表中添加了 **SqlGeography** 、 **SqlGeometry** 和 **SqlHierarchyId** 类型。|  
  
## <a name="see-also"></a>另请参阅  
 [.NET Framework 中的 SQL Server 数据类型](../../relational-databases/clr-integration-database-objects-types-net-framework/sql-server-data-types-in-the-net-framework.md)  
  
  
