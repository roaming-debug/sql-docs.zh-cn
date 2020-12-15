---
description: 使用加密
title: 使用加密 |Microsoft Docs
ms.custom: ''
ms.date: 08/06/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: reference
helpviewer_keywords:
- database master key [SMO]
- cryptography [SMO]
- cryptography [SQL Server], SMO
- encryption keys [SMO]
- encryption [SQL Server], SMO
- encryption [SMO]
- certificates [SMO]
- service master key [SMO]
ms.assetid: 405e0ed7-50a9-430e-a343-471f54b4af76
author: markingmyname
ms.author: maghan
monikerRange: =azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: d5c9818d80ab7ad1eb57e108525ff5a182a01714
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97475388"
---
# <a name="using-encryption"></a>使用加密
[!INCLUDE [SQL Server ASDB, ASDBMI, ASDW ](../../../includes/applies-to-version/sql-asdb-asdbmi-asa.md)]

  在 SMO 中，服务主密钥由 <xref:Microsoft.SqlServer.Management.Smo.ServiceMasterKey> 对象表示。 它是由 <xref:Microsoft.SqlServer.Management.Smo.Server.ServiceMasterKey%2A> 对象的 <xref:Microsoft.SqlServer.Management.Smo.Server> 属性引用的。 可以通过使用 <xref:Microsoft.SqlServer.Management.Smo.ServiceMasterKey.Regenerate%2A> 方法重新生成服务主密钥。  
  
 数据库主密钥由 <xref:Microsoft.SqlServer.Management.Smo.MasterKey> 对象表示。 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.IsEncryptedByServer%2A> 属性指示是否通过服务主密钥对数据库主密钥进行加密。 数据库主密钥发生变化时，主数据库中的加密副本会自动进行更新。  
  
 可以采用 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.DropServiceKeyEncryption%2A> 方法删除服务密钥加密，并使用密码加密数据库主密钥。 在这种情况下，您必须显式打开数据库主密钥，然后才能访问已受到安全保护的私钥。  
  
 如果数据库已附加到 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例，则必须为数据库主密钥提供密码或执行 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.AddServiceKeyEncryption%2A> 方法，才能生成一个未加密的数据库主密钥的副本，以便使用服务主密钥进行加密。 建议执行此步骤，从而无需显式打开数据库主密钥。  
  
 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.Regenerate%2A> 方法将重新生成数据库主密钥。 当重新生成数据库主密钥时，会对所有使用该数据库主密钥加密的密钥进行解密，然后使用新的数据库主密钥对其进行加密。 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.DropServiceKeyEncryption%2A> 方法将通过服务主密钥删除对数据库主密钥的加密。 <xref:Microsoft.SqlServer.Management.Smo.MasterKey.AddServiceKeyEncryption%2A> 可以通过服务主密钥对主密钥的副本进行加密，然后将副本存储在当前数据库和主数据库中。  
  
 在 SMO 中，证书由 <xref:Microsoft.SqlServer.Management.Smo.Certificate> 对象表示。 <xref:Microsoft.SqlServer.Management.Smo.Certificate> 对象具有指定公钥、主题名称、有效期以及有关颁发者的信息的属性。 可采用 **Grant**、 **Revoke** 和 **Deny** 方法控制对证书的访问权限。  
  
## <a name="example"></a>示例  
 对于下列代码示例，您必须选择编程环境、编程模板和编程语言才能创建应用程序。 有关详细信息，请参阅 [在 Visual Studio .net 中创建 Visual C&#35; SMO 项目](../../../relational-databases/server-management-objects-smo/how-to-create-a-visual-csharp-smo-project-in-visual-studio-net.md)。  
  
## <a name="adding-a-certificate-in-visual-c"></a>在 Visual C# 中添加证书  
 该代码示例使用加密密码创建简单证书。 与其他对象不同，<xref:Microsoft.SqlServer.Management.Smo.Certificate.Create%2A> 方法具有若干种重载。 该示例中所使用的重载将使用加密密码创建一个新证书。  
  
```csharp  
{  
            //Connect to the local, default instance of SQL Server.   
            {  
                Server srv = new Server();  
  
                //Reference the AdventureWorks2012 database.   
                Database db = srv.Databases["AdventureWorks2012"];  
  
                //Define a Certificate object variable by supplying the parent database and name in the constructor.   
                Certificate c = new Certificate(db, "Test_Certificate");  
  
                //Set the start date, expiry date, and description.   
                System.DateTime dt;  
                DateTime.TryParse("January 01, 2010", out dt);  
                c.StartDate = dt;  
                DateTime.TryParse("January 01, 2015", out dt);  
                c.ExpirationDate = dt;  
                c.Subject = "This is a test certificate.";  
                //Create the certificate on the instance of SQL Server by supplying the certificate password argument.   
                c.Create("pGFD4bb925DGvbd2439587y");  
            }  
        }   
```  
  
## <a name="adding-a-certificate-in-powershell"></a>在 PowerShell 中添加证书  
 该代码示例使用加密密码创建简单证书。 与其他对象不同，<xref:Microsoft.SqlServer.Management.Smo.Certificate.Create%2A> 方法具有若干种重载。 该示例中所使用的重载将使用加密密码创建一个新证书。  
  
```powershell  
# Set the path context to the local, default instance of SQL Server and get a reference to AdventureWorks2012  
CD \sql\localhost\default\databases  
$db = get-item AdventureWorks2012  
  
#Create a certificate  
  
$c = New-Object -TypeName Microsoft.SqlServer.Management.Smo.Certificate -argumentlist $db, "Test_Certificate"  
$c.StartDate = "January 01, 2010"  
$c.Subject = "This is a test certificate."  
$c.ExpirationDate = "January 01, 2015"  
  
#Create the certificate on the instance of SQL Server by supplying the certificate password argument.  
$c.Create("pGFD4bb925DGvbd2439587y")  
  
```  
  
## <a name="see-also"></a>另请参阅  
 [使用加密密钥](../../../relational-databases/server-management-objects-smo/tasks/using-encryption.md)  
  
  
