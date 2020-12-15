---
description: 使用集合
title: 使用集合 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- SQL Server Management Objects, collections
- SMO [SQL Server], collections
- collections [SMO]
ms.assetid: 209eb175-2514-4de1-bc32-b2e6a469d945
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 701cb855cbeb0395ba224d1b353d469ea5c71c98
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97463008"
---
# <a name="using-collections"></a>使用集合
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  集合是指从相同对象类构造的并共享同一父对象的对象列表。 集合对象始终包含对象类型的名称并具有 Collection 后缀。 例如，若要访问指定表中的列，请使用 <xref:Microsoft.SqlServer.Management.Smo.ColumnCollection> 对象类型。 它包含所有属于同一 <xref:Microsoft.SqlServer.Management.Smo.Column> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Table> 对象。  
  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[vbprvb](../../../includes/vbprvb-md.md)] **For...Each** 语句或 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] [!INCLUDE[csprcs](../../../includes/csprcs-md.md)] **foreach** 语句可用于遍历集合的每个成员。  
  
## <a name="examples"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="referencing-an-object-by-using-a-collection-in-visual-basic"></a>在 Visual Basic 中使用集合来引用对象  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.TableViewTableTypeBase.Columns%2A>、<xref:Microsoft.SqlServer.Management.Smo.Database.Tables%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.Server.Databases%2A> 属性来设置列属性。 这些属性表示集合，当这些属性与指定对象名称的参数一起使用时可用来标识特定对象。 <xref:Microsoft.SqlServer.Management.Smo.Database.Tables%2A> 集合对象属性需要名称和架构。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Modify a property using the Databases, Tables, and Columns collections to reference a column.
srv.Databases("AdventureWorks2012").Tables("Person", "Person").Columns("ModifiedDate").Nullable = True
'Call the Alter method to make the change on the instance of SQL Server.
srv.Databases("AdventureWorks2012").Tables("Person", "Person").Columns("ModifiedDate").Alter()
```
  
## <a name="referencing-an-object-by-using-a-collection-in-visual-c"></a>在 Visual C# 中使用集合来引用对象  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.TableViewTableTypeBase.Columns%2A>、<xref:Microsoft.SqlServer.Management.Smo.Database.Tables%2A> 和 <xref:Microsoft.SqlServer.Management.Smo.Server.Databases%2A> 属性来设置列属性。 这些属性表示集合，当这些属性与指定对象名称的参数一起使用时可用来标识特定对象。 <xref:Microsoft.SqlServer.Management.Smo.Database.Tables%2A> 集合对象属性需要名称和架构。  
  
```csharp  
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Modify a property using the Databases, Tables, and Columns collections to reference a column.   
srv.Databases("AdventureWorks2012").Tables("Person", "Person").Columns("LastName").Nullable = true;   
//Call the Alter method to make the change on the instance of SQL Server.   
srv.Databases("AdventureWorks2012").Tables("Person", "Person").Columns("LastName").Alter();   
}  
```  
  
## <a name="iterating-through-the-members-of-a-collection-in-visual-basic"></a>在 Visual Basic 中遍历集合中的成员  
 此代码示例可遍历 <xref:Microsoft.AnalysisServices.Server.Databases%2A> 集合属性并显示数据库与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例之间的所有连接。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
Dim count As Integer
Dim total As Integer
'Iterate through the databases and call the GetActiveDBConnectionCount method.
Dim db As Database
For Each db In srv.Databases
    count = srv.GetActiveDBConnectionCount(db.Name)
    total = total + count
    'Display the number of connections for each database.
    Console.WriteLine(count & " connections on " & db.Name)
Next
'Display the total number of connections on the instance of SQL Server.
Console.WriteLine("Total connections =" & total)
```
  
## <a name="iterating-through-the-members-of-a-collection-in-visual-c"></a>在 Visual C# 中遍历集合中的成员  
 此代码示例可遍历 <xref:Microsoft.AnalysisServices.Server.Databases%2A> 集合属性并显示数据库与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例之间的所有连接。  
  
```csharp  
//Connect to the local, default instance of SQL Server.   
{   
Server srv = default(Server);   
srv = new Server();   
int count = 0;   
int total = 0;   
//Iterate through the databases and call the GetActiveDBConnectionCount method.   
Database db = default(Database);   
foreach ( db in srv.Databases) {   
  count = srv.GetActiveDBConnectionCount(db.Name);   
  total = total + count;   
  //Display the number of connections for each database.   
  Console.WriteLine(count + " connections on " + db.Name);   
}   
//Display the total number of connections on the instance of SQL Server.   
Console.WriteLine("Total connections =" + total);   
}   
```  
  
  
