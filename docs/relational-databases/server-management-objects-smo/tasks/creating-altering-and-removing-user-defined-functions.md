---
description: 创建、更改和删除用户定义函数
title: 创建、更改和删除用户定义函数
ms.custom: seo-dt-2019
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- user-defined functions [SMO]
ms.assetid: 0ebebd3b-0775-41c2-989d-aa4cf81af12a
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: a5f2adc78d8885f534d852b8d7f51913e50f104a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97468508"
---
# <a name="creating-altering-and-removing-user-defined-functions"></a>创建、更改和删除用户定义函数
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]
  <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction>对象提供允许用户以编程方式管理中用户定义的函数的功能 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 。 用户定义函数支持输入和输出参数，还支持对表列的直接引用。  
  
 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 要求先在数据库中注册程序集，然后才能在存储过程、用户定义函数、触发器和用户定义数据类型中使用这些程序集。 SMO 使用 <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly> 对象支持此项功能。  
  
 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> 对象使用 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.AssemblyName%2A>、<xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.ClassName%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction.MethodName%2A> 属性来引用 .NET 程序集。  
  
 当 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction> 对象引用 .NET 程序集时，您必须通过创建 <xref:Microsoft.SqlServer.Management.Smo.SqlAssembly> 对象并将其添加到 <xref:Microsoft.SqlServer.Management.Smo.SqlAssemblyCollection> 对象（属于 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象）来注册该程序集。  
  
## <a name="example"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="creating-a-scalar-user-defined-function-in-visual-basic"></a>在 Visual Basic 中创建标量用户定义函数  
 此代码示例说明如何在 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] 中创建和删除具有 <xref:System.DateTime> 输入对象参数和整数返回类型的标量用户定义函数。 此用户定义函数是对 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 数据库创建的。 该示例创建了用户定义函数 ISOweek，此函数带有一个日期参数，用于计算 ISO 周号。 要使此函数能正确计算，必须在调用此函数之前将数据库 DATEFIRST 选项设置为 1。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 2008R2 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.
Dim udf As UserDefinedFunction
udf = New UserDefinedFunction(db, "IsOWeek")
'Set the TextMode property to false and then set the other properties.
udf.TextMode = False
udf.DataType = DataType.Int
udf.ExecutionContext = ExecutionContext.Caller
udf.FunctionType = UserDefinedFunctionType.Scalar
udf.ImplementationType = ImplementationType.TransactSql
'Add a parameter.
Dim par As UserDefinedFunctionParameter
par = New UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime)
udf.Parameters.Add(par)
'Set the TextBody property to define the user defined function.
udf.TextBody = "BEGIN  DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"
'Create the user defined function on the instance of SQL Server.
udf.Create()
'Remove the user defined function.
udf.Drop()
``` 
  
## <a name="creating-a-scalar-user-defined-function-in-visual-c"></a>在 Visual C# 中创建标量用户定义函数  
 此代码示例说明如何在 [!INCLUDE[csprcs](../../../includes/csprcs-md.md)] 中创建和删除具有 <xref:System.DateTime> 输入对象参数和整数返回类型的标量用户定义函数。 此用户定义函数是对 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 数据库创建的。 该示例将创建用户定义函数。 `ISOweek`. 此函数使用日期参数来计算 ISO 周数。 要使此函数能正确计算，必须在调用此函数之前将数据库 `DATEFIRST` 选项设置为 `1` 。  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
           Server srv = new Server();  
            //Reference the AdventureWorks2012 database.   
           Database db = srv.Databases["AdventureWorks2012"];  
  
            //Define a UserDefinedFunction object variable by supplying the parent database and the name arguments in the constructor.   
            UserDefinedFunction udf = new UserDefinedFunction(db, "IsOWeek");  
  
            //Set the TextMode property to false and then set the other properties.   
            udf.TextMode = false;  
            udf.DataType = DataType.Int;  
            udf.ExecutionContext = ExecutionContext.Caller;  
            udf.FunctionType = UserDefinedFunctionType.Scalar;  
            udf.ImplementationType = ImplementationType.TransactSql;  
  
            //Add a parameter.   
  
     UserDefinedFunctionParameter par = new UserDefinedFunctionParameter(udf, "@DATE", DataType.DateTime);  
            udf.Parameters.Add(par);  
  
            //Set the TextBody property to define the user-defined function.   
            udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;";  
  
            //Create the user-defined function on the instance of SQL Server.   
            udf.Create();  
  
            //Remove the user-defined function.   
            udf.Drop();  
        }  
```  
  
## <a name="creating-a-scalar-user-defined-function-in-powershell"></a>在 PowerShell 中创建标量用户定义函数  
 此代码示例说明如何在 [!INCLUDE[csprcs](../../../includes/csprcs-md.md)] 中创建和删除具有 <xref:System.DateTime> 输入对象参数和整数返回类型的标量用户定义函数。 此用户定义函数是对 [!INCLUDE[ssSampleDBnormal](../../../includes/sssampledbnormal-md.md)] 数据库创建的。 该示例将创建用户定义函数。 `ISOweek`. 此函数使用日期参数来计算 ISO 周数。 要使此函数能正确计算，必须在调用此函数之前将数据库 `DATEFIRST` 选项设置为 `1` 。  
  
```powershell   
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item Adventureworks2012  
  
# Define a user defined function object variable by supplying the parent database and name arguments in the constructor.   
$udf  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunction `  
-argumentlist $db, "IsOWeek"  
  
# Set the TextMode property to false and then set the other properties.   
$udf.TextMode = $false  
$udf.DataType = [Microsoft.SqlServer.Management.SMO.DataType]::Int   
$udf.ExecutionContext = [Microsoft.SqlServer.Management.SMO.ExecutionContext]::Caller  
$udf.FunctionType = [Microsoft.SqlServer.Management.SMO.UserDefinedFunctionType]::Scalar  
$udf.ImplementationType = [Microsoft.SqlServer.Management.SMO.ImplementationType]::TransactSql  
  
# Define a Parameter object variable by supplying the parent function, name and type arguments in the constructor.  
$type = [Microsoft.SqlServer.Management.SMO.DataType]::DateTime  
$par  = New-Object -TypeName Microsoft.SqlServer.Management.SMO.UserDefinedFunctionParameter `  
-argumentlist $udf, "@DATE",$type  
  
# Add the parameter to the function  
$udf.Parameters.Add($par)  
  
#Set the TextBody property to define the user-defined function.   
$udf.TextBody = "BEGIN DECLARE @ISOweek int SET @ISOweek= DATEPART(wk,@DATE)+1 -DATEPART(wk,CAST(DATEPART(yy,@DATE) as CHAR(4))+'0104') IF (@ISOweek=0) SET @ISOweek=dbo.ISOweek(CAST(DATEPART(yy,@DATE)-1 AS CHAR(4))+'12'+ CAST(24+DATEPART(DAY,@DATE) AS CHAR(2)))+1 IF ((DATEPART(mm,@DATE)=12) AND ((DATEPART(dd,@DATE)-DATEPART(dw,@DATE))>= 28)) SET @ISOweek=1 RETURN(@ISOweek) END;"  
  
# Create the user-defined function on the instance of SQL Server.   
$udf.Create()  
  
# Remove the user-defined function.   
$udf.Drop()  
```  
  
## <a name="see-also"></a>另请参阅  
 <xref:Microsoft.SqlServer.Management.Smo.UserDefinedFunction>  
  
  
