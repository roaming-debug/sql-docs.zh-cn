---
description: 使用数据类型
title: 使用数据类型 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- DataType object
- SQL Server Management Objects, data types
- data types [SMO]
- SMO [SQL Server], data types
ms.assetid: 1e0e736a-c709-4d89-aeb2-b32dcfd641fa
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 1939f6608b6175463103fce2a69a3d5936346605
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462998"
---
# <a name="working-with-data-types"></a>使用数据类型
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  数据具有很多类型和不同的大小，例如具有定义长度的字符串、具有特定精度的数字或者作为具有其自身规则集的其他对象的用户定义数据类型。 <xref:Microsoft.SqlServer.Management.Smo.DataType>对象对数据类型进行分类，以便能够正确地对其进行处理 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 <xref:Microsoft.SqlServer.Management.Smo.DataType> 对象与接受数据的对象关联。 以下 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO) 接受必须由 <xref:Microsoft.SqlServer.Management.Smo.DataType> 对象属性定义的数据：  
  
-   <xref:Microsoft.SqlServer.Management.Smo.Column>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunctionParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.StoredProcedureParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunctionParameter>  
  
-   <xref:Microsoft.SqlServer.Management.Smo.UserDefinedAggregateParameter>  
  
 可以通过若干方式设置接受数据的对象的 **DataType** 属性。  
  
-   使用默认构造函数，并显式指定 <xref:Microsoft.SqlServer.Management.Smo.DataType> 对象属性。  
  
-   使用重载的构造函数，并指定 <xref:Microsoft.SqlServer.Management.Smo.DataType> 属性作为参数。  
  
-   在对象构造函数中内联指定 <xref:Microsoft.SqlServer.Management.Smo.DataType>。  
  
-   使用类的静态成员之一 <xref:Microsoft.SqlServer.Management.Smo.DataType> ，例如 **Int**。实际上，这将返回对象的实例 <xref:Microsoft.SqlServer.Management.Smo.DataType> 。  
  
 <xref:Microsoft.SqlServer.Management.Smo.DataType> 对象具有定义数据类型的几个属性。 例如，<xref:Microsoft.SqlServer.Management.Smo.SqlDataType> 属性指定 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 在 <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> 枚举中列出了表示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的常量值。 这是指诸如 **varchar**、 **nchar**、 **currency**、 **integer**、 **float** 和 **datetime** 这样的数据类型。  
  
 确立数据类型时，必须为数据设置具体的属性。 例如，如果它是 **nchar** 类型，则必须在 **Length** 属性中设置字符串数据的长度。 对数字值同样如此，这时必须指定精度和小数位数。  
  
 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 和 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType> 数据类型引用的对象包含由用户定义的数据类型的定义。 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 基于来自 <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> 枚举的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型。 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>基于 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] .net 数据类型。 通常，它们表示由于组织定义的业务规则而被数据库频繁重用的特定类型的数据。 例如，存储资金数量和货币币种的数据类型对于处理多种货币的公司将会非常有用。  
  
 <xref:Microsoft.SqlServer.Management.Smo.SqlDataType> 枚举包含 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持的所有数据类型的列表。  
  
## <a name="examples"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
  
## <a name="constructing-a-datatype-object-with-the-specification-in-the-constructor-in-visual-basic"></a>使用 Visual Basic 中构造函数的规范构造 DataType 对象  
 该代码示例显示如何使用构造函数创建基于不同 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的数据类型实例。  
  
> [!NOTE]  
>  <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>、<xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 和 XML 类型全都需要名称值以标识对象。  
  
```VBNET
'Declare a DataType object variable and define the data type in the constructor.
Dim dt As DataType
'For the decimal data type the following two arguments specify precision, and scale.
dt = New DataType(SqlDataType.Decimal, 10, 2)
``` 
  
## <a name="constructing-a-datatype-object-with-the-specification-in-the-constructor-in-visual-c"></a>使用 Visual C# 中构造函数的规范构造 DataType 对象  
 该代码示例显示如何使用构造函数创建基于不同 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的数据类型实例。  
  
> [!NOTE]  
>  <xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>、<xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 和 XML 类型全都需要名称值以标识对象。  
  
```csharp  
{   
//Declare a DataType object variable and define the data type in the constructor.   
DataType dt;   
//For the decimal data type the following two arguments specify precision, and scale.   
dt = new DataType(SqlDataType.Decimal, 10, 2);   
}  
```  
  
## <a name="constructing-a-datatype-object-by-using-the-default-constructor-in-visual-basic"></a>使用 Visual Basic 中的默认构造函数构造 DataType 对象  
 该代码示例显示如何使用默认构造函数创建基于不同 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的数据类型实例。 然后使用这些属性指定数据类型。  
  
 **注意**<xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>、 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 和 XML 类型都需要名称值以标识对象。  
  
```VBNET
'Declare and create a DataType object variable.
Dim dt As DataType
dt = New DataType
'Define the data type by setting the SqlDataType property.
dt.SqlDataType = SqlDataType.VarChar
'The VarChar data type requires a value for the MaximumLength property.
dt.MaximumLength = 100
```
  
## <a name="constructing-a-datatype-object-by-using-the-default-constructor-in-visual-c"></a>使用 Visual C# 中的默认构造函数构造 DataType 对象  
 该代码示例显示如何使用默认构造函数创建基于不同 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型的数据类型实例。 然后使用这些属性指定数据类型。  
  
 **注意**<xref:Microsoft.SqlServer.Management.Smo.UserDefinedType>、 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedDataType> 和 XML 类型都需要名称值以标识对象。  
  
```csharp  
{   
//Declare and create a DataType object variable.   
DataType dt;   
dt = new DataType();   
//Define the data type by setting the SqlDataType property.   
dt.SqlDataType = SqlDataType.VarChar;   
//The VarChar data type requires a value for the MaximumLength property.   
dt.MaximumLength = 100;   
}  
```  
  
  
