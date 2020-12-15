---
description: 传输数据
title: 传输数据 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- data transfers [SMO]
- transferring data
ms.assetid: eea255c3-8251-40f0-973b-fe4ef6cb5261
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 2745c1dba3e2ac0cc98d8d6a48920af9fface01a
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475408"
---
# <a name="transferring-data"></a>传输数据
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

   类是一个实用工具类，它提供用于传输对象和数据的工具。  
  
 通过在目标服务器上执行生成的脚本可以传输数据库架构中的对象。 使用动态创建的 DTS 包传输 <xref:Microsoft.SqlServer.Management.Smo.Table> 数据。  
  
 <xref:Microsoft.SqlServer.Management.Smo.Transfer>对象使用[SQLBulkCopy](/dotnet/api/system.data.sqlclient.sqlbulkcopy) API 传输数据。 同样，用于执行数据传输的方法和属性驻留在 <xref:Microsoft.SqlServer.Management.Smo.Transfer> 对象中，而不是 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象中。 将功能从实例类移到实用工具类符合轻型对象模型，因为仅在需要特定任务的代码时才加载它们。  
  
  对象不支持向  低于  的实例版本的目标数据库传输数据。  
  
## <a name="example"></a>示例  
若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
 
  
## <a name="transferring-schema-and-data-from-one-database-to-another-in-visual-basic"></a>在 Visual Basic 中于数据库之间传输架构和数据  
 此代码实例说明如何使用 <xref:Microsoft.SqlServer.Management.Smo.Transfer> 对象在数据库之间传输架构和数据。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server
'Reference the AdventureWorks2012 2008R2 database
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Create a new database that is to be destination database.
Dim dbCopy As Database
dbCopy = New Database(srv, "AdventureWorks2012Copy")
dbCopy.Create()
'Define a Transfer object and set the required options and properties.
Dim xfr As Transfer
xfr = New Transfer(db)
xfr.CopyAllTables = True
xfr.Options.WithDependencies = True
xfr.Options.ContinueScriptingOnError = True
xfr.DestinationDatabase = "AdventureWorks2012Copy"
xfr.DestinationServer = srv.Name
xfr.DestinationLoginSecure = True
xfr.CopySchema = True
'Script the transfer. Alternatively perform immediate data transfer with TransferData method.
xfr.ScriptTransfer()
```
  
## <a name="transferring-schema-and-data-from-one-database-to-another-in-visual-c"></a>在 Visual C# 中于数据库之间传输架构和数据  
 此代码实例说明如何使用 <xref:Microsoft.SqlServer.Management.Smo.Transfer> 对象在数据库之间传输架构和数据。  
  
```csharp  
{  
            Server srv;  
            srv = new Server();  
            //Reference the AdventureWorks2012 database   
            Database db;  
            db = srv.Databases["AdventureWorks2012"];  
            //Create a new database that is to be destination database.   
            Database dbCopy;  
            dbCopy = new Database(srv, "AdventureWorks2012Copy");  
            dbCopy.Create();  
            //Define a Transfer object and set the required options and properties.   
            Transfer xfr;  
            xfr = new Transfer(db);  
            xfr.CopyAllTables = true;  
            xfr.Options.WithDependencies = true;  
            xfr.Options.ContinueScriptingOnError = true;  
            xfr.DestinationDatabase = "AdventureWorks2012Copy";  
            xfr.DestinationServer = srv.Name;  
            xfr.DestinationLoginSecure = true;  
            xfr.CopySchema = true;  
            //Script the transfer. Alternatively perform immediate data transfer   
            // with TransferData method.   
            xfr.ScriptTransfer();  
        }   
```  
  
## <a name="transferring-schema-and-data-from-one-database-to-another-in-powershell"></a>在 PowerShell 中于数据库之间传输架构和数据  
 此代码实例说明如何使用 <xref:Microsoft.SqlServer.Management.Smo.Transfer> 对象在数据库之间传输架构和数据。  
  
```powershell  
#Connect to the local, default instance of SQL Server.  
  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Reference the AdventureWorks2012 database.  
$db = $srv.Databases["AdventureWorks2012"]  
  
#Create a database to hold the copy of AdventureWorks  
$dbCopy = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Database -argumentlist $srv, "AdventureWorksCopy"  
$dbCopy.Create()  
  
#Define a Transfer object and set the required options and properties.  
$xfr = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Transfer -argumentlist $db  
  
#Set this objects properties  
$xfr.CopyAllTables = $true  
$xfr.Options.WithDependencies = $true  
$xfr.Options.ContinueScriptingOnError = $true  
$xfr.DestinationDatabase = "AdventureWorksCopy"  
$xfr.DestinationServer = $srv.Name  
$xfr.DestinationLoginSecure = $true  
$xfr.CopySchema = $true  
"Scripting Data Transfer"  
#Script the transfer. Alternatively perform immediate data transfer with TransferData method.  
$xfr.ScriptTransfer()  
```  
  
