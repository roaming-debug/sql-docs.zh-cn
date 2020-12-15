---
description: 使用 XML 架构
title: 使用 XML 架构 |Microsoft Docs
ms.custom: ''
ms.date: 01/11/2019
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- XML [SMO]
ms.assetid: 9d04de01-efeb-4b2d-8c28-3234bc7ff2f3
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: f76bc99be907bdf3b46d478802c3c71b1d7028d6
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97467198"
---
# <a name="using-xml-schemas"></a>使用 XML 架构

[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  SMO 中的 XML 编程仅限于提供 XML 数据类型、XML 命名空间和对 XML 数据类型列的简单索引。  
  
 [!INCLUDE[msCoName](../../../includes/msconame-md.md)][!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]提供 XML 文档实例的本机存储。 通过 XML 架构可定义复杂的 XML 数据类型，这些类型可用于验证 XML 文档以确保数据完整性。 XML 架构在 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection> 对象中定义。  
  
## <a name="example"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="creating-an-xml-schema-in-visual-basic"></a>在 Visual Basic 中创建 XML 架构  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection> 对象创建 XML 架构。 定义 XML 架构集合的 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A> 属性包含多个双引号。 这些双引号将由 `chr(34)` 字符串替换。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server()
'Reference the AdventureWorks2012 2008R2 database.
Dim db As Database
db = srv.Databases("AdventureWorks2012")
'Define an XmlSchemaCollection object by supplying the parent database and name arguments in the constructor.
Dim xsc As XmlSchemaCollection
xsc = New XmlSchemaCollection(db, "MySampleCollection")
xsc.Text = "\<schema xmlns=" + Chr(34) + "http://www.w3.org/2001/XMLSchema" + Chr(34) + "  xmlns:ns=" + Chr(34) + "http://ns" + Chr(34) + ">\<element name=" + Chr(34) + "e" + Chr(34) + " type=" + Chr(34) + "dateTime" + Chr(34) + "/></schema>"
'Create the XML schema collection on the instance of SQL Server.
xsc.Create()
```
  
## <a name="creating-an-xml-schema-in-visual-c"></a>在 Visual C# 中创建 XML 架构  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection> 对象创建 XML 架构。 定义 XML 架构集合的 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A> 属性包含多个双引号。 这些双引号将由 `chr(34)` 字符串替换。  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            Server srv = default(Server);  
            srv = new Server();  
            //Reference the AdventureWorks2012 database.   
            Database db = default(Database);  
            db = srv.Databases["AdventureWorks2012"];  
            //Define an XmlSchemaCollection object by supplying the parent  
            // database and name arguments in the constructor.   
            XmlSchemaCollection xsc = default(XmlSchemaCollection);  
            xsc = new XmlSchemaCollection(db, "MySampleCollection");  
            xsc.Text = "\<schema xmlns=" + Strings.Chr(34) + "http://www.w3.org/2001/XMLSchema" + Strings.Chr(34) + " xmlns:ns=" + Strings.Chr(34) + "http://ns" + Strings.Chr(34) + ">\<element name=" + Strings.Chr(34) + "e" + Strings.Chr(34) + " type=" + Strings.Chr(34) + "dateTime" + Strings.Chr(34) + "/></schema>";  
            //Create the XML schema collection on the instance of SQL Server.   
            xsc.Create();  
        }  
```  
  
## <a name="creating-an-xml-schema-in-powershell"></a>在 PowerShell 中创建 XML 架构  
 此代码示例演示如何使用 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection> 对象创建 XML 架构。 定义 XML 架构集合的 <xref:Microsoft.SqlServer.Management.Smo.XmlSchemaCollection.Text%2A> 属性包含多个双引号。 这些双引号将由 `chr(34)` 字符串替换。  
  
```powershell   
#Get a server object which corresponds to the default instance replace LocalMachine with the physical server  
cd \sql\LocalHost  
$srv = get-item default  
  
#Reference the AdventureWorks database.  
$db = $srv.Databases["AdventureWorks2012"]  
  
#Create a new schema collection  
$xsc = New-Object -TypeName Microsoft.SqlServer.Management.SMO.XmlSchemaCollection `  
-argumentlist $db,"MySampleCollection"  
  
#Add the xml  
$dq = '"' # the double quote character  
$xsc.Text = "<schema xmlns=" + $dq + "http://www.w3.org/2001/XMLSchema" + $dq + `  
"  xmlns:ns=" + $dq + "http://ns" + $dq + "><element name=" + $dq + "e" + $dq +`  
 " type=" + $dq + "dateTime" + $dq + "/></schema>"  
  
#Create the XML schema collection on the instance of SQL Server.  
$xsc.Create()  
```  
  
  
