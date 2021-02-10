---
description: Connect 属性 (RDS)
title: " (RDS) 连接属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Connect property [ADO]
ms.assetid: dbad5e77-b213-4eb8-aecf-d60f203fdb59
author: rothja
ms.author: jroth
ms.openlocfilehash: 288efb8a2af9896e731bd0248967d1aaadcb2992
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049497"
---
# <a name="connect-property-rds"></a>Connect 属性 (RDS)
指示运行查询和更新操作的数据库名称。  
  
 您可以在设计时在 RDS 中设置 **Connect** 属性 [。DataControl](./datacontrol-object-rds.md) 对象的对象标记，或在运行时，在脚本代码 (例如，VBScript) 。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
Design time: <PARAM NAME="Connect" VALUE="ConnectionString">  
Run time: DataControl.Connect = "ConnectionString"  
```  
  
#### <a name="parameters"></a>参数  
 *ConnectionString*  
 有效的连接字符串。 有关连接字符串的更多常规信息，请参阅 [ConnectionString](../ado-api/connectionstring-property-ado.md) 属性或提供程序文档。  
  
> [!NOTE]
>  将 MS Remote 指定为 RDS 的提供程序 **。DataControl** 会创建一个四层方案。 超过三个层的方案尚未经过测试，因此不需要这样做。  
  
 *DataControl*  
 表示 RDS 的对象变量 **。DataControl** 对象。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VBScript) 连接属性示例 ](./connect-property-example-vbscript.md)   
 [查询方法 (RDS) ](./query-method-rds.md)   
 [Refresh 方法 (RDS) ](./refresh-method-rds.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)