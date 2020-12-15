---
title: 通过命名空间 (SQLXMLOLEDB) 执行 XPath 查询
description: 了解如何在通过 SQLXMLOLEDB 提供程序执行 XPath 查询时，在 SQLXML 4.0 中指定命名空间。
ms.date: 03/16/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xml
ms.topic: reference
helpviewer_keywords:
- SQLXMLOLEDB Provider, executing XPath queries
- namespaces property
- queries [SQLXML], SQLXMLOLEDB Provider
- XPath queries [SQLXML], namespaces
- XPath queries [SQLXML], SQLXMLOLEDB Provider
- namespaces [SQLXML], XPath queries
ms.assetid: 024a4b7d-435d-47ba-9e80-2c2f640108f5
author: MightyPen
ms.author: genemi
ms.custom: seo-lt-2019
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: ee24d16dce8ae60ccf51a866a274b15b2737ca89
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97462908"
---
# <a name="executing-xpath-queries-with-namespaces-sqlxmloledb-provider"></a>执行带命名空间的 XPath 查询（SQLXMLOLEDB 访问接口）
[!INCLUDE [SQL Server Azure SQL Database](../../../includes/applies-to-version/sql-asdb.md)]
  XPath 查询可以包含命名空间。 如果架构元素为限定命名空间（即，包含目标命名空间），则针对该架构的 XPath 查询必须指定该命名空间。  
  
 由于 SQLXML 4.0 中不支持使用通配符 (*)，因此必须使用命名空间前缀来指定 XPath 查询。 若要解析此前缀，请使用 namespace 属性来指定命名空间绑定。  
  
 在下面的示例中，XPath 查询使用通配符指定命名空间 (\*) 和本地名称 ( # A3 和 namespace uri ( # A5 XPath 函数。 此 XPath 查询返回本地名称为 **Contact** 的所有元素，命名空间 URI 为 **urn： myschema.xml： Contacts**。  
  
```  
/*[local-name() = 'Contact' and namespace-uri() = 'urn:myschema:Contacts']  
```  
  
 在 SQLXML 4.0 中，必须用命名空间前缀来指定此 XPath 查询。 例如， **x:Contact**，其中 **x** 是命名空间前缀。 请看以下 XSD 架构：  
  
```  
<schema xmlns="http://www.w3.org/2001/XMLSchema"  
            xmlns:sql="urn:schemas-microsoft-com:mapping-schema"  
            xmlns:con="urn:myschema:Contacts"  
            targetNamespace="urn:myschema:Contacts">  
<complexType name="ContactType">  
  <attribute name="CID" sql:field="ContactID" type="ID"/>  
  <attribute name="FName" sql:field="FirstName" type="string"/>  
  <attribute name="LName" sql:field="LastName"/>   
</complexType>  
<element name="Contact" type="con:ContactType" sql:relation="Person.Contact"/>  
</schema>  
```  
  
 由于此架构定义了目标命名空间，因此针对此架构的 XPath 查询（如“Employee”）必须包含该命名空间。  
  
 以下是一个 [!INCLUDE[msCoName](../../../includes/msconame-md.md)] Visual Basic 应用程序示例，针对前述 XSD 架构执行 XPath 查询 (x:Employee)。 若要解析此前缀，使用 namespace 属性指定命名空间绑定。  
  
> [!NOTE]  
>  在该代码中，必须在连接字符串中提供 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例的名称。 此外，本示例还指定使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Native Client (SQLNCLI11) 作为数据访问接口，该访问接口需要安装其他网络客户端软件。 有关详细信息，请参阅 [SQL Server Native Client 的系统要求](../../../relational-databases/native-client/system-requirements-for-sql-server-native-client.md)。  
  
```  
Option Explicit  
Private Sub Form_Load()  
    Dim con As New ADODB.Connection  
    Dim cmd As New ADODB.Command  
    Dim stm As New ADODB.Stream  
    con.Open "provider=SQLXMLOLEDB.4.0;Data Provider=SQLNCLI11;Data Source=SqlServerName;Initial Catalog=AdventureWorks;Integrated Security=SSPI;"  
    Set cmd.ActiveConnection = con  
    stm.Open  
    cmd.Properties("Output Stream").Value = stm  
    cmd.Properties("Output Encoding") = "utf-8"  
    cmd.Properties("Mapping schema") = "C:\DirectoryPath\con-ex.xml"  
    cmd.Properties("namespaces") = "xmlns:x='urn:myschema:Contacts'"  
    '  Debug.Print "Set Command Dialect to DBGUID_XPATH"  
    cmd.Dialect = "{ec2a4293-e898-11d2-b1b7-00c04f680c56}"  
    cmd.CommandText = "x:Contact"  
    cmd.Execute , , adExecuteStream   
    stm.Position = 0  
    Debug.Print stm.ReadText(adReadAll)  
End Sub  
```  
  
### <a name="to-test-this-application"></a>测试此应用程序  
  
1.  将示例 XSD 架构保存到文件夹中。  
  
2.  创建 Visual Basic 可执行项目，并将代码复制到其中。 根据需要更改指定的目录路径。  
  
3.  添加以下项目引用：  
  
    ```  
    "Microsoft ActiveX Data Objects 2.8 Library"  
    ```  
  
4.  执行应用程序。  

 下面是部分结果：  
  
```  
<y0:Employee xmlns:y0="urn:myschema:Contacts"   
             LName="Achong" CID="1" FName="Gustavo"/>  
<y0:Employee xmlns:y0="urn:myschema:Employees"   
             LName="Abel" CID="2" FName="Catherine"/>  
```  
  
 在 XML 文档中生成的前缀是任意的，但都映射到同一个命名空间。  
  
  
