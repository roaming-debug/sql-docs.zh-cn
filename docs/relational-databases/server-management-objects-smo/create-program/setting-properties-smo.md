---
description: 设置属性 - SMO
title: 设置属性-SMO |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SMO [SQL Server], properties
- SQL Server Management Objects, properties
- properties [SMO]
ms.assetid: 342569ba-d2f7-44d2-8f3f-ae9c701c7f0f
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 692db5e2da92983ca385812b9d3891df5aa9e13d
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463018"
---
# <a name="setting-properties---smo"></a>设置属性 - SMO
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  属性是存储有关对象的说明性信息的值。 例如， [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置选项由 <xref:Microsoft.SqlServer.Management.Smo.Server.Configuration%2A> 对象的属性表示。 使用属性集合可以直接或间接访问属性。 直接访问属性使用以下语法：  
  
 `objInstance.PropertyName`  
  
 可以修改或检索属性值，具体取决于该属性是拥有读/写访问权限还是只读访问权限。 此外，还必须在创建对象之前设置某些特定属性。 有关详细信息，请参阅特定对象的 SMO 参考资料。  
  
> [!NOTE]  
>  某一对象的子对象集合显示为该对象的属性。 例如， **Tables** 集合是 **Server** 对象的属性。 有关详细信息，请参阅 [Using Collections](../../../relational-databases/server-management-objects-smo/create-program/using-collections.md)。  
  
 对象的属性是属性集合的成员。 属性集合可用于遍历对象的每个属性。  
  
 有时某一属性不可用，其原因如下：  
  
-   服务器版本不支持该属性，例如当尝试在旧版本的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 上访问表示 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]新功能的属性时。  
  
-   服务器未提供该属性的相应数据，例如当尝试访问表示尚未安装的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 组件的属性时。  
  
 通过捕获 <xref:Microsoft.SqlServer.Management.Smo.UnknownPropertyException> 和 <xref:Microsoft.SqlServer.Management.Smo.PropertyCannotBeRetrievedException> SMO 异常，可以处理上述情况。  
  
## <a name="setting-default-initialization-fields"></a>设置默认的初始化字段  
 SMO 检索对象时将执行优化功能。 通过在以下状态之间进行缩放，此优化功能最大程度地减少了加载的属性数目：  
  
1.  部分加载。 首次引用某一对象时，该对象具有最少的可用属性（例如 Name 和 Schema）。  
  
2.  完全加载。 当引用任一属性时，将初始化剩余属性中可快速加载的属性并使其可用。  
  
3.  占用大量内存的属性。 剩余的不可用属性占用大量内存，并将 <xref:Microsoft.SqlServer.Management.Smo.Property.Expensive%2A> 属性值设置为 True（例如 <xref:Microsoft.SqlServer.Management.Smo.Database.DataSpaceUsage%2A>）。 只有专门引用这些属性时才会进行加载。  
  
 除在部分加载状态中提供的属性之外，如果应用程序的确还需要提取额外属性，则会提交检索这些额外属性的查询，并向上扩展到完全加载状态。 这可能会在客户端和服务器之间造成不必要的通信流量。 调用 <xref:Microsoft.SqlServer.Management.Smo.Server.SetDefaultInitFields%2A> 方法可以实现更多优化。 使用 <xref:Microsoft.SqlServer.Management.Smo.Server.SetDefaultInitFields%2A> 方法可以指定在初始化对象时加载的属性。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Server.SetDefaultInitFields%2A> 方法设置其余应用程序或重置应用程序后的属性加载行为。 您可以使用 <xref:Microsoft.SqlServer.Management.Smo.Server.GetDefaultInitFields%2A> 方法保存原始行为，并根据需要还原。  
  
## <a name="examples"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  

  
## <a name="getting-and-setting-a-property-in-visual-basic"></a>在 Visual Basic 中获取和设置属性  
 此代码示例演示如何获取对象的 <xref:Microsoft.SqlServer.Management.Smo.Information.Edition%2A> 属性 <xref:Microsoft.SqlServer.Management.Smo.Information> ，以及如何将属性的属性设置 <xref:Microsoft.SqlServer.Management.Common.ServerConnection.SqlExecutionModes%2A> <xref:Microsoft.SqlServer.Management.Smo.Server.ConnectionContext%2A> 为枚举类型的 **ExecuteSql** 成员 <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes> 。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Get a property.
Console.WriteLine(srv.Information.Version)
'Set a property.
srv.ConnectionContext.SqlExecutionModes = SqlExecutionModes.ExecuteSql
```
  
## <a name="getting-and-setting-a-property-in-visual-c"></a>在 Visual C# 中获取和设置属性  
 此代码示例演示如何获取对象的 <xref:Microsoft.SqlServer.Management.Smo.Information.Edition%2A> 属性 <xref:Microsoft.SqlServer.Management.Smo.Information> ，以及如何将属性的属性设置 <xref:Microsoft.SqlServer.Management.Common.ServerConnection.SqlExecutionModes%2A> <xref:Microsoft.SqlServer.Management.Smo.Server.ConnectionContext%2A> 为枚举类型的 **ExecuteSql** 成员 <xref:Microsoft.SqlServer.Management.Common.SqlExecutionModes> 。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Get a property.   
Console.WriteLine(srv.Information.Version);   
//Set a property.   
srv.ConnectionContext.SqlExecutionModes = SqlExecutionModes.ExecuteSql;   
}  
```  
  
## <a name="setting-various-properties-before-an-object-is-created-in-visual-basic"></a>在 Visual Basic 中创建对象之前设置各种属性  
 此代码示例演示如何直接设置 <xref:Microsoft.SqlServer.Management.Smo.Table.AnsiNullsStatus%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Table> 属性，以及如何在创建 <xref:Microsoft.SqlServer.Management.Smo.Table> 对象之前创建和添加列。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Create a new table in the AdventureWorks2012 database. 
Dim db As Database
db = srv.Databases("AdventureWorks2012")
Dim tb As Table
'Specify the parent database, table schema and the table name in the constructor.
tb = New Table(db, "Test_Table", "HumanResources")
'Add columns because the table requires columns before it can be created. 
Dim c1 As Column
'Specify the parent table, the column name and data type in the constructor.
c1 = New Column(tb, "ID", DataType.Int)
tb.Columns.Add(c1)
c1.Nullable = False
c1.Identity = True
c1.IdentityIncrement = 1
c1.IdentitySeed = 0
Dim c2 As Column
c2 = New Column(tb, "Name", DataType.NVarChar(100))
c2.Nullable = False
tb.Columns.Add(c2)
tb.AnsiNullsStatus = True
'Create the table on the instance of SQL Server.
tb.Create()
```
  
## <a name="setting-various-properties-before-an-object-is-created-in-visual-c"></a>在 Visual C# 中创建对象之前设置各种属性  
 此代码示例演示如何直接设置 <xref:Microsoft.SqlServer.Management.Smo.Table.AnsiNullsStatus%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Table> 属性，以及如何在创建 <xref:Microsoft.SqlServer.Management.Smo.Table> 对象之前创建和添加列。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Create a new table in the AdventureWorks2012 database.   
Database db;   
db = srv.Databases("AdventureWorks2012");   
Table tb;   
//Specify the parent database, table schema, and the table name in the constructor.   
tb = new Table(db, "Test_Table", "HumanResources");   
//Add columns because the table requires columns before it can be created.   
Column c1;   
//Specify the parent table, the column name, and data type in the constructor.   
c1 = new Column(tb, "ID", DataType.Int);   
tb.Columns.Add(c1);   
c1.Nullable = false;   
c1.Identity = true;   
c1.IdentityIncrement = 1;   
c1.IdentitySeed = 0;   
Column c2;   
c2 = new Column(tb, "Name", DataType.NVarChar(100));   
c2.Nullable = false;   
tb.Columns.Add(c2);   
tb.AnsiNullsStatus = true;   
//Create the table on the instance of SQL Server.   
tb.Create();   
}  
```  
  
## <a name="iterating-through-all-properties-of-an-object-in-visual-basic"></a>在 Visual Basic 中遍历对象的所有属性  
 此代码示例将循环访问对象的 **Properties** 集合 <xref:Microsoft.SqlServer.Management.Smo.StoredProcedure> ，并将其显示在 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 输出屏幕上。  
  
 在本示例中，由于 <xref:Microsoft.SqlServer.Management.Smo.Property> 对象同时也是 [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] 关键字，因此将该对象置于方括号中。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Set properties on the uspGetEmployeeManagers stored procedure on the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
Dim sp As StoredProcedure
sp = db.StoredProcedures("uspGetEmployeeManagers")
sp.AnsiNullsStatus = False
sp.QuotedIdentifierStatus = False
'Iterate through the properties of the stored procedure and display.
'Note the Property object requires [] parentheses to distinguish it from the Visual Basic key word.
Dim p As [Property]
For Each p In sp.Properties
    Console.WriteLine(p.Name & p.Value)
Next
```
  
## <a name="iterating-through-all-properties-of-an-object-in-visual-c"></a>在 Visual C# 中遍历对象的所有属性  
 此代码示例将循环访问对象的 **Properties** 集合 <xref:Microsoft.SqlServer.Management.Smo.StoredProcedure> ，并将其显示在 [!INCLUDE[vsprvs](../../../includes/vsprvs-md.md)] 输出屏幕上。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Set properties on the uspGetEmployeedManagers stored procedure on the AdventureWorks2012 database.   
Database db;   
db = srv.Databases("AdventureWorks2012");   
StoredProcedure sp;   
sp = db.StoredProcedures("uspGetEmployeeManagers");   
sp.AnsiNullsStatus = false;   
sp.QuotedIdentifierStatus = false;   
//Iterate through the properties of the stored procedure and display.   
  Property p;   
  foreach ( p in sp.Properties) {   
    Console.WriteLine(p.Name + p.Value);   
  }   
}  
```  
  
## <a name="setting-default-initialization-fields-in-visual-basic"></a>在 Visual Basic 中设置默认的初始化字段  
 此代码示例演示如何使 SMO 程序中初始化的对象属性的数目降到最低。 您必须包括 `using System.Collections.Specialized`; 语句，以便使用 <xref:System.Collections.Specialized.StringCollection> 对象。  
  
 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] 可用于将发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的语句的数目和此优化进行比较。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Assign the Table object type to a System.Type object variable.
Dim tb As Table
Dim typ As Type
tb = New Table
typ = tb.GetType
'Assign the current default initialization fields for the Table object type to a 
'StringCollection object variable.
Dim sc As StringCollection
sc = srv.GetDefaultInitFields(typ)
'Set the default initialization fields for the Table object type to the CreateDate property.
srv.SetDefaultInitFields(typ, "CreateDate")
'Retrieve the Schema, Name, and CreateDate properties for every table in AdventureWorks2012.
'Note that the improvement in performance can be viewed in SQL Profiler.
For Each tb In db.Tables
    Console.WriteLine(tb.Schema + "." + tb.Name + " " + tb.CreateDate)
Next
'Set the default initialization fields for the Table object type back to the original settings.
srv.SetDefaultInitFields(typ, sc)
```
  
## <a name="setting-default-initialization-fields-in-visual-c"></a>在 Visual C# 中设置默认的初始化字段  
 此代码示例演示如何使 SMO 程序中初始化的对象属性的数目降到最低。 您必须包括 `using System.Collections.Specialized`; 语句，以便使用 <xref:System.Collections.Specialized.StringCollection> 对象。  
  
 [!INCLUDE[ssSqlProfiler](../../../includes/sssqlprofiler-md.md)] 可用于将发送到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的语句的数目和此优化进行比较。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Reference the AdventureWorks2012 database.   
Database db;   
db = srv.Databases("AdventureWorks2012");   
//Assign the Table object type to a System.Type object variable.   
Table tb;   
Type typ;   
tb = new Table();   
typ = tb.GetType;   
//Assign the current default initialization fields for the Table object type to a   
//StringCollection object variable.   
StringCollection sc;   
sc = srv.GetDefaultInitFields(typ);   
//Set the default initialization fields for the Table object type to the CreateDate property.   
srv.SetDefaultInitFields(typ, "CreateDate");   
//Retrieve the Schema, Name, and CreateDate properties for every table in AdventureWorks2012.   
   //Note that the improvement in performance can be viewed in SQL Server Profiler.   
foreach ( tb in db.Tables) {   
   Console.WriteLine(tb.Schema + "." + tb.Name + " " + tb.CreateDate);   
}   
//Set the default initialization fields for the Table object type back to the original settings.   
srv.SetDefaultInitFields(typ, sc);   
}  
```  
  
  
