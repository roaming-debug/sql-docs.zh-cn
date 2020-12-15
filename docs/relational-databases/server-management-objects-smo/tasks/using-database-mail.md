---
description: 使用数据库邮件
title: 使用数据库邮件 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- e-mail [SMO]
- Database Mail [SMO]
- mail [SMO]
ms.assetid: 7605390f-b485-48cc-8d97-e364a066067b
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 983524f042465efff6a39522466022e34849b74e
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475398"
---
# <a name="using-database-mail"></a>使用数据库邮件
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 SMO 中，数据库邮件子系统由 <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 属性引用的 <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> 对象表示。 使用 SMO <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 对象，可以配置数据库邮件子系统并管理配置文件和邮件帐户。 SMO <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 对象属于 **服务器** 对象，也就是说，邮件帐户的作用域处于服务器级别。  
  
## <a name="examples"></a>示例  
 若要使用所提供的任何代码示例，您必须选择创建应用程序所需的编程环境、编程模板和编程语言。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
 对于使用数据库邮件的程序 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ，必须包括 **Imports** 语句来限定 Mail 命名空间。 请在应用程序的其他 **Imports** 语句之后、任何声明之前插入该语句，例如：  
  
 `Imports Microsoft.SqlServer.Management.Smo`  
  
 `Imports Microsoft.SqlServer.Management.Common`  
  
 `Imports Microsoft.SqlServer.Management.Smo.Mail`  
  
## <a name="creating-a-database-mail-account-by-using-visual-basic"></a>使用 Visual Basic 创建数据库邮件帐户  
 此代码示例说明如何在 SMO 中创建电子邮件帐户。 数据库邮件由 <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 对象表示，并由 <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Server> 属性引用。 SMO 可用于以编程方式配置数据库邮件，但它无法用于发送或处理收到的电子邮件。  
  
```VBNET
'Connect to the local, default instance of SQL Server.
Dim srv As Server
srv = New Server()
'Define the Database Mail service with a SqlMail object variable and reference it using the Server Mail property.
Dim sm As SqlMail
sm = srv.Mail
'Define and create a mail account by supplying the Database Mail service, name, description, display name, and email address arguments in the constructor.
Dim a As MailAccount
a = New MailAccount(sm, "AdventureWorks Administrator", "AdventureWorks Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com")
a.Create()
```
  
## <a name="creating-a-database-mail-account-by-using-visual-c"></a>使用 Visual C# 创建数据库邮件帐户  
 此代码示例说明如何在 SMO 中创建电子邮件帐户。 数据库邮件由 <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 对象表示，并由 <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Server> 属性引用。 SMO 可用于以编程方式配置数据库邮件，但它无法用于发送或处理收到的电子邮件。  
  
```csharp  
{  
         //Connect to the local, default instance of SQL Server.  
         Server srv = default(Server);   
           srv = new Server();   
           //Define the Database Mail service with a SqlMail object variable   
           //and reference it using the Server Mail property.   
           SqlMail sm;   
           sm = srv.Mail;   
           //Define and create a mail account by supplying the Database Mail  
           //service, name, description, display name, and email address  
           //arguments in the constructor.   
           MailAccount a = default(MailAccount);   
           a = new MailAccount(sm, "AdventureWorks2012 Administrator", "AdventureWorks2012 Automated Mailer", "Mail account for administrative e-mail.", "dba@Adventure-Works.com");   
           a.Create();    
}  
```  
  
## <a name="creating-a-database-mail-account-by-using-powershell"></a>使用 PowerShell 创建数据库邮件帐户  
 此代码示例说明如何在 SMO 中创建电子邮件帐户。 数据库邮件由 <xref:Microsoft.SqlServer.Management.Smo.Mail.SqlMail> 对象表示，并由 <xref:Microsoft.SqlServer.Management.Smo.Server.Mail%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Server> 属性引用。 SMO 可用于以编程方式配置数据库邮件，但它无法用于发送或处理收到的电子邮件。  
  
  
  
```powershell  
#Connect to the local, default instance of SQL Server.  
  
#Get a server object which corresponds to the default instance  
$srv = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Server  
  
#Define the Database Mail; reference it using the Server Mail property.  
$sm = $srv.Mail  
  
#Define and create a mail account by supplying the Database Mail service,  
#name, description, display name, and email address arguments in the constructor.  
$a = New-Object -TypeName Microsoft.SqlServer.Management.SMO.Mail.MailAccount -argumentlist $sm, `  
"Adventure Works Administrator", "Adventure Works Automated Mailer",`  
 "Mail account for administrative e-mail.", "dba@Adventure-Works.com"  
$a.Create()  
```  
  
  
