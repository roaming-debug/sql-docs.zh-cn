---
description: 处理 SMO 事件
title: 处理 SMO 事件 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- events [SMO]
- SQL Server Management Objects, events
- SMO [SQL Server], events
- events [SMO], about events
ms.assetid: b4f120dd-ba78-46ff-99c5-e47effac8544
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ea6e1e8c1c995a450f113fe67f916d501dbedd0f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467248"
---
# <a name="handling-smo-events"></a>处理 SMO 事件
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  某些服务器事件类型可以通过使用事件处理程序和 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象来进行订阅。  
  
 当服务器上发生某些操作时，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 管理对象 (SMO) 中的许多实例类可以触发事件。  
  
 通过设置事件处理程序并订阅相关事件，可以以编程方式对这些事件进行处理。 这种事件处理只是暂时性的，因为当 SMO 客户端程序退出时会删除所有的订阅。  
  
## <a name="connectioncontext-event-handling"></a>ConnectionContext 事件处理  
 <xref:Microsoft.SqlServer.Management.Common.ServerConnection> 对象支持若干种事件类型。 事件属性必须设置为相应事件处理程序的实例，且必须将事件处理程序对象定义为用于处理事件的受保护函数。  
  
## <a name="event-subscription"></a>事件订阅  
 您可以通过以下方法处理事件：编写事件处理程序类，创建该类的实例，将事件处理程序分配给父对象，然后订阅事件。  
  
 必须编写事件处理程序类才能处理事件。 事件处理程序类可以包含多个事件处理程序函数，且只有在安装事件处理程序类后才能处理事件。 事件处理程序函数从 *ServerEventNotificatificationArgs* 参数接收有关事件的信息，该事件可用于报告有关事件的信息。  
  
 类和类中列出了可以处理的数据库和服务器事件的类型 <xref:Microsoft.SqlServer.Management.Smo.DatabaseEventSet> <xref:Microsoft.SqlServer.Management.Smo.ServerEventSet> 。  
  
## <a name="example"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  

  
## <a name="registering-event-handlers-and-subscribing-to-event-handling-in-visual-basic"></a>在 Visual Basic 中注册事件处理程序并订阅事件处理  
 此代码示例演示如何设置事件处理程序以及如何订阅数据库事件。  
  
```VBNET
'Create an event handler subroutine that runs when a table is created.
Private Sub MyCreateEventHandler(ByVal sender As Object, ByVal e As ServerEventArgs)
    Console.WriteLine("A table has just been added to the AdventureWorks2012 2008 database.")
End Sub
'Create an event handler subroutine that runs when a table is deleted.
Private Sub MyDropEventHandler(ByVal sender As Object, ByVal e As ServerEventArgs)
    Console.WriteLine("A table has just been dropped from the AdventureWorks2012 2008 database.")
End Sub
Sub Main()
    'Connect to the local, default instance of SQL Server.
    Dim srv As Server
    srv = New Server
    'Reference the AdventureWorks2012 database.
    Dim db As Database
    db = srv.Databases("AdventureWorks2012")
    'Create a database event set that contains the CreateTable event only.
    Dim databaseCreateEventSet As New DatabaseEventSet
    databaseCreateEventSet.CreateTable = True
    'Create a server event handler and set it to the first event handler subroutine.
    Dim serverCreateEventHandler As ServerEventHandler
    serverCreateEventHandler = New ServerEventHandler(AddressOf MyCreateEventHandler)
    'Subscribe to the first server event handler when a CreateTable event occurs.
    db.Events.SubscribeToEvents(databaseCreateEventSet, serverCreateEventHandler)
    'Create a database event set that contains the DropTable event only.
    Dim databaseDropEventSet As New DatabaseEventSet
    databaseDropEventSet.DropTable = True
    'Create a server event handler and set it to the second event handler subroutine.
    Dim serverDropEventHandler As ServerEventHandler
    serverDropEventHandler = New ServerEventHandler(AddressOf MyDropEventHandler)
    'Subscribe to the second server event handler when a DropTable event occurs.
    db.Events.SubscribeToEvents(databaseDropEventSet, serverDropEventHandler)
    'Start event handling.
    db.Events.StartEvents()
    'Create a table on the database.
    Dim tb As Table
    tb = New Table(db, "Test_Table")
    Dim mycol1 As Column
    mycol1 = New Column(tb, "Name", DataType.NChar(50))
    mycol1.Collation = "Latin1_General_CI_AS"
    mycol1.Nullable = True
    tb.Columns.Add(mycol1)
    tb.Create()
    'Remove the table.
    tb.Drop()
    'Wait until the events have occured.
    Dim x As Integer
    Dim y As Integer
    For x = 1 To 1000000000
        y = x * 2
    Next
    'Stop event handling.
    db.Events.StopEvents()

End Sub
``` 
  
## <a name="registering-event-handlers-and-subscribing-to-event-handling-in-visual-c"></a>在 Visual C# 中注册事件处理程序并订阅事件处理  
 此代码示例演示如何设置事件处理程序以及如何订阅数据库事件。  
  
```csharp  
//Create an event handler subroutine that runs when a table is created.   
private void MyCreateEventHandler(object sender, ServerEventArgs e)   
{   
Console.WriteLine("A table has just been added to the AdventureWorks2012 database.");   
}   
//Create an event handler subroutine that runs when a table is deleted.   
private void MyDropEventHandler(object sender, ServerEventArgs e)   
{   
Console.WriteLine("A table has just been dropped from the AdventureWorks2012 database.");   
}   
public void Main()   
{   
//Connect to the local, default instance of SQL Server.   
Server srv;   
srv = new Server();   
//Reference the AdventureWorks2012 database.   
Database db;   
db = srv.Databases("AdventureWorks2012");   
//Create a database event set that contains the CreateTable event only.   
DatabaseEventSet databaseCreateEventSet = new DatabaseEventSet();   
databaseCreateEventSet.CreateTable = true;   
//Create a server event handler and set it to the first event handler subroutine.   
ServerEventHandler serverCreateEventHandler;   
serverCreateEventHandler = new ServerEventHandler(MyCreateEventHandler);   
//Subscribe to the first server event handler when a CreateTable event occurs.   
db.Events.SubscribeToEvents(databaseCreateEventSet, serverCreateEventHandler);   
    //Create a database event set that contains the DropTable event only.   
DatabaseEventSet databaseDropEventSet = new DatabaseEventSet();   
databaseDropEventSet.DropTable = true;   
//Create a server event handler and set it to the second event handler subroutine.   
ServerEventHandler serverDropEventHandler;   
serverDropEventHandler = new ServerEventHandler(MyDropEventHandler);   
//Subscribe to the second server event handler when a DropTable event occurs.   
db.Events.SubscribeToEvents(databaseDropEventSet, serverDropEventHandler);   
//Start event handling.   
db.Events.StartEvents();   
//Create a table on the database.   
Table tb;   
tb = new Table(db, "Test_Table");   
Column mycol1;   
mycol1 = new Column(tb, "Name", DataType.NChar(50));   
mycol1.Collation = "Latin1_General_CI_AS";   
mycol1.Nullable = true;   
tb.Columns.Add(mycol1);   
tb.Create();   
//Remove the table.   
tb.Drop();   
//Wait until the events have occured.   
int x;   
int y;   
for (x = 1; x <= 1000000000; x++) {   
    y = x * 2;   
}   
//Stop event handling.   
db.Events.StopEvents();   
}  
```  
  
  
