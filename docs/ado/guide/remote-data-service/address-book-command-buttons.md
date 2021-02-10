---
description: 通讯簿命令按钮
title: 通讯簿命令按钮 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- address book application scenario [ADO], command buttons
- RDS scenarios [ADO], command buttons
ms.assetid: 80676831-6488-4dad-a558-c47c52256a22
author: rothja
ms.author: jroth
ms.openlocfilehash: dccdb22981bb1662cd50da1e28db8fa0a2ce47d6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032040"
---
# <a name="address-book-command-buttons"></a>通讯簿命令按钮
通讯簿应用程序包括以下命令按钮：  
  
-   用于将查询提交到数据库的 " **查找** " 按钮。  
  
-   用于在开始新搜索前清除文本框的 " **清除** " 按钮。  
  
-   用于保存对员工记录所做更改的 " **更新配置文件** " 按钮。  
  
-   用于放弃更改的 " **取消更改** " 按钮。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="find-button"></a>查找按钮  
 单击 " **查找** " 按钮将激活 VBScript Find_OnClick Sub 过程，该过程将生成并发送 SQL 查询。 单击此按钮将填充数据网格。  
  
## <a name="building-the-sql-query"></a>生成 SQL 查询  
 Find_OnClick Sub 过程的第一部分将通过向全局 SQL SELECT 语句追加文本字符串，一次生成一个短语。 首先，将变量设置 `myQuery` 为从数据源表请求所有数据行的 SQL SELECT 语句。 接下来，Sub 过程扫描页面上四个输入框中的每一个。  
  
 由于程序 `like` 在生成 SQL 语句时使用单词，因此查询是子字符串搜索，而不是完全匹配。  
  
 例如，如果 " **姓氏** " 框中包含条目 "Berge"，并且 **标题** 框中包含 "项目经理" 条目，则) 的 SQL 语句 (值 `myQuery` 将为：  
  
```sql
Select FirstName, LastName, Title, Email, Building, Room, Phone from Employee where lastname like 'Berge%' and title like 'Program Manager%'  
```  
  
 如果查询成功，则姓氏中包含文本 "Berge" 的所有人员 (如 Berge 和 Berger) 并且标题包含 "项目经理" 字样 (例如，"项目经理"、"高级技术) " 将显示在 "HTML 数据" 网格中。  
  
## <a name="preparing-and-sending-the-query"></a>准备并发送查询  
 Find_OnClick Sub 过程的最后一部分包含两个语句。 第一个语句分配 RDS 的 [SQL](../../reference/rds-api/sql-property.md) 属性 [。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象等于动态生成的 SQL 查询。 第二个语句将导致 **RDS。DataControl** 对象 (`DC1`) 查询数据库，然后在网格中显示查询的新结果。  
  
```vb
Sub Find_OnClick  
   '...  
   DC1.SQL = myQuery  
   DC1.Refresh  
End Sub  
```  
  
## <a name="update-profile-button"></a>"更新配置文件" 按钮  
 单击 " **更新配置文件** " 按钮将激活 VBScript Update_OnClick Sub 过程，该过程将执行 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象的 (`DC1`) [SubmitChanges](../../reference/rds-api/submitchanges-method-rds.md) 和 [Refresh](../../reference/rds-api/refresh-method-rds.md) 方法。  
  
```vb
Sub Update_OnClick  
   DC1.SubmitChanges  
   DC1.Refresh  
End Sub  
```  
  
 当 `DC1.SubmitChanges` 执行时，远程数据服务将打包所有更新信息，并通过 HTTP 将其发送到服务器。 更新为 "完全" 或 "无";如果部分更新不成功，则不进行任何更改，并返回状态消息。 `DC1.Refresh` 在 **SubmitChanges** 远程数据服务后，并不是必需的，但它可确保最新的数据。  
  
## <a name="cancel-changes-button"></a>"取消更改" 按钮  
 单击 " **取消更改** " 将激活 VBScript Cancel_OnClick Sub 过程，该过程将执行 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象的 (`DC1)` [CancelUpdate](../../reference/rds-api/cancelupdate-method-rds.md) 方法。  
  
```vb
Sub Cancel_OnClick  
   DC1.CancelUpdate  
End Sub  
```  
  
 `DC1.CancelUpdate`执行时，它会丢弃自上次查询或更新以来用户对数据网格中的雇员记录所做的任何编辑。 它还原原始值。  
  
## <a name="see-also"></a>另请参阅  
 [通讯簿导航按钮](./address-book-navigation-buttons.md)   
 [DataControl 对象 (RDS)](../../reference/rds-api/datacontrol-object-rds.md)