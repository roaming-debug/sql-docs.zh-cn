---
description: DataFactory 对象 (RDSServer) 属性、方法和事件
title: DataFactory 对象 (RDSServer) 属性、方法和事件 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
helpviewer_keywords:
- DataFactory object [ADO], members
ms.assetid: 36a1f49b-91f4-44f4-b6e2-52fc7ed06d7e
author: rothja
ms.author: jroth
ms.openlocfilehash: 8914a431bdc1e13398087cced7cc4e1b8124fc21
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049447"
---
# <a name="datafactory-object-rdsserver-properties-methods-and-events"></a>DataFactory 对象 (RDSServer) 属性、方法和事件
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="properties"></a>属性  
 无。  
  
## <a name="methods"></a>方法  
  
|方法|说明|  
|-|-|  
|[ConvertToString 方法 (RDS)](./converttostring-method-rds.md)|将记录集转换为 MIME64 字符串。|  
|[CreateRecordset 方法 (RDS)](./createrecordset-method-rds.md)|创建并返回一个空的记录集。|  
|[Execute 方法 (RDS)](./execute-method-rds.md)|执行请求并创建高级数据行集 (以便与 ADO 2.5 或更高版本一起使用) 。|  
|[Execute21 方法 (RDS)](./execute21-method-rds.md)|执行请求并创建高级数据行集 (以便与 ADO 2.1) 一起使用。|  
|[Query 方法 (RDS)](./query-method-rds.md)|执行请求并创建高级数据行集。|  
|[SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)|如果记录集包含挂起的更改，则此方法会将它们提交到连接字符串中标识的数据库。|  
|[Synchronize 方法 (RDS)](./synchronize-method-rds.md)|使用连接字符串指定的数据库同步给定的记录集， (用于 ADO 2.5 或更高版本) 。|  
|[Synchronize21 方法 (RDS)](./synchronize21-method-rds.md)|使用连接字符串指定的数据库同步给定的记录集， (用于 ADO 2.1) 。|  
  
## <a name="events"></a>事件  
 无。  
  
## <a name="see-also"></a>另请参阅  
 [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)