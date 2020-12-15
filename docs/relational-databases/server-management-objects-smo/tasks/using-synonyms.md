---
description: 使用同义词
title: 使用同义词 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- synonyms [SMO]
ms.assetid: db0a9022-9549-43e5-b6b3-deb236f05fb8
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 47deb46f1ff11a207aebcb1b5a77fb67b6f2c2b8
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467178"
---
# <a name="using-synonyms"></a>使用同义词
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  同义词是架构范围内的对象的另一名称。 在 SMO 中，同义词由 <xref:Microsoft.SqlServer.Management.Smo.Synonym> 对象表示。 <xref:Microsoft.SqlServer.Management.Smo.Synonym> 对象是 <xref:Microsoft.SqlServer.Management.Smo.Database> 对象的子对象。 这意味着同义词仅在定义它们的数据库范围内有效。 但是，同义词可以引用位于另一个数据库或 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]远程实例上的对象。  
  
 具有另一名称的对象称为基对象。 <xref:Microsoft.SqlServer.Management.Smo.Synonym> 对象的名称属性即为提供给基对象的另一名称。  
  
## <a name="example"></a>示例  
 对于下列代码示例，您必须选择编程环境、编程模板和编程语言才能创建应用程序。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="creating-a-synonym-in-visual-c"></a>在 Visual C# 中创建同义词  
 代码示例演示了如何为架构范围内的对象创建同义词或备用名称。 通过使用同义词，客户端应用程序可以使用对基对象的单一引用，而不必使用由多部分组成的名称来引用基对象。  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv = new Server();  
  
            //Reference the AdventureWorks2012 database.   
            Database db = srv.Databases["AdventureWorks2012"];  
  
            //Define a Synonym object variable by supplying the   
            //parent database, name, and schema arguments in the constructor.   
            //The name is also a synonym of the name of the base object.   
            Synonym syn = new Synonym(db, "Shop", "Sales");  
  
            //Specify the base object, which is the object on which   
            //the synonym is based.   
            syn.BaseDatabase = "AdventureWorks2012";  
            syn.BaseSchema = "Sales";  
            syn.BaseObject = "Store";  
            syn.BaseServer = srv.Name;  
  
            //Create the synonym on the instance of SQL Server.   
            syn.Create();  
        }  
```  
  
## <a name="creating-a-synonym-in-powershell"></a>在 PowerShell 中创建同义词  
 代码示例演示了如何为架构范围内的对象创建同义词或备用名称。 通过使用同义词，客户端应用程序可以使用对基对象的单一引用，而不必使用由多部分组成的名称来引用基对象。  
  
```powershell  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#And the database object corresponding to Adventureworks  
$db = $srv.Databases["AdventureWorks2012"]  
  
$syn = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Synonym `  
-argumentlist $db, "Shop", "Sales"  
  
#Specify the base object, which is the object on which the synonym is based.  
$syn.BaseDatabase = "AdventureWorks2012"  
$syn.BaseSchema = "Sales"  
$syn.BaseObject = "Store"  
$syn.BaseServer = $srv.Name  
  
#Create the synonym on the instance of SQL Server.  
$syn.Create()  
```  
  
## <a name="see-also"></a>另请参阅  
 [CREATE SYNONYM (Transact-SQL)](../../../t-sql/statements/create-synonym-transact-sql.md)  
  
  
