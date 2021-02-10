---
description: 通讯簿数据绑定对象
title: 通讯簿 Data-Binding 对象 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 11/09/2018
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- RDS scenarios [ADO], data-binding object
- address book application scenario [ADO], data-binding object
ms.assetid: 080c1925-d453-4b89-92ac-c93591490518
author: rothja
ms.author: jroth
ms.openlocfilehash: 91489445aef2034ed273a6d13cc5d6ae02b7f519
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100036587"
---
# <a name="address-book-data-binding-object"></a>通讯簿数据绑定对象
通讯簿应用程序使用 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象，用于将 SQL Server 数据库中的数据绑定到可视对象 (在这种情况下，将在应用程序的客户端 HTML 页中) 一个 DHTML 表。 事件驱动的 VBScript 程序逻辑使用 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) ：  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
-   查询数据库，将更新发送到数据库并刷新数据网格。  
  
-   允许用户移动到数据网格中的第一条、下一条、上一条或最后一条记录。  
  
 下面的代码定义了 **RDS。DataControl** 组件：  
  
```vb
<OBJECT classid="clsid:BD96C556-65A3-11D0-983A-00C04FC29E33"  
   ID=DC1 Width=1 Height=1>  
   <PARAM NAME="SERVER" VALUE="https://<%=Request.ServerVariables("SERVER_NAME")%>">  
   <PARAM NAME="CONNECT" VALUE="Provider=sqloledb;  
Initial Catalog=AddrBookDb;Integrated Security=SSPI;">  
</OBJECT>  
```  
  
 对象标记定义 **RDS。** 程序中的 DataControl 组件。 标记包含两种类型的参数：  
  
-   与泛型对象标记相关联的。  
  
-   特定于 RDS 的 **。DataControl** 对象。  
  
## <a name="generic-object-tag-parameters"></a>泛型对象标记参数  
 下表描述了与对象标记相关的参数。  
  
|参数|说明|  
|---------------|-----------------|  
|***CLASSID** _|唯一的128位数字，用于标识嵌入到系统的嵌入对象的类型。 此标识符保存在本地计算机的系统注册表中。 用于 _ RDS 的类 Id 的 (*。DataControl** 对象，请参阅 [RDS。DataControl 对象](../../reference/rds-api/datacontrol-object-rds.md)。 ) |  
|***ID***|定义用于在代码中标识的嵌入对象的文档范围标识符。|  
  
## <a name="rdsdatacontrol-tag-parameters"></a>RDS.DataControl 标记参数  
 下表描述了特定于 RDS 的参数 **。DataControl** 对象。 有关 RDS 的完整列表的 (**。DataControl** 对象参数和实现它们的时间，请参阅 [RDS。DataControl 对象](../../reference/rds-api/datacontrol-object-rds.md)。 )   
  
|参数|说明|  
|---------------|-----------------|  
|[服务](../../reference/rds-api/server-property-rds.md)|如果使用的是 HTTP，则值为前面带有的服务器计算机的名称 `https://` 。|  
|[CONNECT](../../reference/rds-api/connect-property-rds.md)|为 RDS 提供必要的连接信息 **。DataControl** 连接到 SQL Server。|  
|[SQL](../../reference/rds-api/sql-property.md)|设置或返回用于检索 [记录集](../../reference/ado-api/recordset-object-ado.md)的查询字符串。|  
  
## <a name="see-also"></a>另请参阅  
 [通讯簿命令按钮](./address-book-command-buttons.md)