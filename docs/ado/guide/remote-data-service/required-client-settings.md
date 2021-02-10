---
description: 必需的客户端设置
title: 必需的客户端设置 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- DataFactory handler in RDS [ADO]
ms.assetid: e776b4e3-fcc4-4bfb-a7e8-5ffae1d83833
author: rothja
ms.author: jroth
ms.openlocfilehash: c33a92334c410e7cd2d33cc7f48360a3e44b15eb
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100031829"
---
# <a name="required-client-settings"></a>必需的客户端设置
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
 指定以下设置以使用自定义 **DataFactory** 处理程序。  
  
-   在 [连接对象 (ado)](../../reference/ado-api/connection-object-ado.md) 对象 [提供程序属性 (ado)](../../reference/ado-api/provider-property-ado.md) 属性或 **连接** 对象连接字符串 "**Provider**=" 关键字中指定 "provider = MS Remote"。  
  
-   将 [ (ADO)](../../reference/ado-api/cursorlocation-property-ado.md) 属性的 "CursorLocation" 属性设置为 " **adUseClient**"。  
  
-   在 DataControl 对象中指定要在对象中使用的处理程序的名称 [ (RDS)](../../reference/rds-api/datacontrol-object-rds.md) 对象的 **处理程序** 属性或 [记录集对象 (ADO)](../../reference/ado-api/recordset-object-ado.md) 对象的连接字符串 "**handler**=" 关键字。  (无法在 **连接** 对象连接字符串中设置处理程序。 )   
  
 RDS 在名为 MSDFMAP 的服务器上提供了一个默认的处理程序 **。处理程序**。  (默认的自定义文件命名为 MSDFMAP.INI。 )   
  
 **示例**  
  
 假设之前已定义 **MSDFMAP.INI** 和数据源名称 advworks-srv01 中的以下部分：  
  
```console
[connect CustomerDataBase]  
Access=ReadWrite  
Connect="DSN=AdvWorks"  
  
[sql CustomerById]  
SQL="SELECT * FROM Customers WHERE CustomerID = ?"  
```  
  
 以下代码片段以 Visual Basic 编写：  
  
## <a name="rdsdatacontrol-version"></a>RDS.DataControl 版本  
  
```vb
Dim dc as New RDS.DataControl  
Set dc.Handler = "MSDFMAP.Handler"  
Set dc.Server = "https://yourServer"  
Set dc.Connect = "Data Source=CustomerDatabase"  
Set dc.SQL = "CustomerById(4)"  
dc.Refresh  
```  
  
## <a name="recordset-version"></a>记录集版本  
  
```vb
Dim rs as New ADODB.Recordset  
rs.CursorLocation = adUseClient  
```  
  
  (RDS) 属性或关键字指定 [处理程序属性](../../reference/rds-api/handler-property-rds.md) ，则为; [提供程序属性 (ADO)](../../reference/ado-api/provider-property-ado.md) 属性或关键字;和 *CustomerById* 和 *CustomerDatabase* 标识符。 然后打开 **Recordset** 对象  
  
 rs-232c.打开 "CustomerById (4) "、"Handler = MSDFMAP"。处理程序; "& _  
  
```vb
"Provider=MS Remote;Data Source=CustomerDatabase;" & _  
"Remote Server=https://yourServer"  
```  
  
## <a name="see-also"></a>另请参阅  
 [自定义文件连接部分](./customization-file-connect-section.md)   
 [自定义文件 SQL 部分](./customization-file-sql-section.md)   
 [自定义文件 UserList 部分](./customization-file-userlist-section.md)   
 [自定义 DataFactory](./datafactory-customization.md)   
 [了解自定义文件](./understanding-the-customization-file.md)   
 [编写自己的自定义处理程序](./writing-your-own-customized-handler.md)