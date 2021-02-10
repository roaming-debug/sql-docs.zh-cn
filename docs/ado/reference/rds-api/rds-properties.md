---
description: RDS 属性
title: RDS 属性 |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
helpviewer_keywords:
- RDS properties [ADO]
- properties [ADO], RDS
ms.assetid: e4e04cbd-21fc-44a1-9f21-49aa68746934
author: rothja
ms.author: jroth
ms.openlocfilehash: 641b2776efbd8b4e78348c45aecef3ace006dfdd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049217"
---
# <a name="rds-properties"></a>RDS 属性
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
|属性|说明|  
|-|-|  
|[连接 (RDS) ](./connect-property-rds.md)|指示运行查询和更新操作的数据库名称。|  
|[ExecuteOptions (RDS) ](./executeoptions-property-rds.md)|指示是否启用异步执行。|  
|[FetchOptions (RDS) ](./fetchoptions-property-rds.md)|指示异步提取的类型。|  
|[FilterColumn (RDS) ](./filtercolumn-property-rds.md)|指示用于计算筛选条件的列。|  
|[FilterCriterion (RDS) ](./filtercriterion-property-rds.md)|指示要在筛选器值中使用的求值运算符。|  
|[FilterValue (RDS) ](./filtervalue-property-rds.md)|指示用于筛选记录的值。|  
|[处理程序 (RDS) ](./handler-property-rds.md)|指示 (*处理* 程序) 的服务器端自定义项的名称，该程序扩展了 **DataFactory** 的功能，以及 *处理程序* 使用的任何参数。|  
|[InternetTimeout (RDS) ](./internettimeout-property-rds.md)|指示请求超时前等待的毫秒数。|  
|[ReadyState (RDS) ](./readystate-property-rds.md)|指示 **DataControl** 对象将数据提取到其 **Recordset** 对象中时的进度。|  
|[记录集和 SourceRecordset (RDS) ](./recordset-sourcerecordset-properties-rds.md)|指示从自定义业务对象返回的 **记录集** 对象。|  
|[服务器 (RDS) ](./server-property-rds.md)|指示 IIS) 名称和通信协议 (Internet Information Services。|  
|[SortColumn (RDS) ](./sortcolumn-property-rds.md)|指示对记录进行排序所依据的列。|  
|[SortDirection (RDS) ](./sortdirection-property-rds.md)|指示排序顺序是升序还是降序。|  
|[SQL (RDS) ](./sql-property.md)|指示用于检索 **记录集** 的查询字符串。|  
|[URL (RDS) ](./url-property-rds.md)|指示一个字符串，该字符串包含相对或绝对 URL。|