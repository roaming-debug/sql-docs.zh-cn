---
description: SubmitChanges 方法 (RDS)
title: SubmitChanges 方法 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SubmitChanges method [ADO]
ms.assetid: 250062a4-13c4-4bed-807d-8b9ad81536d4
author: rothja
ms.author: jroth
ms.openlocfilehash: 052c131cb430d911d37f6782565c00a1d3cc53d0
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166103"
---
# <a name="submitchanges-method-rds"></a>SubmitChanges 方法 (RDS)
将本地缓存的可更新 [记录集](../ado-api/recordset-object-ado.md) 的挂起的更改提交到 [Connect](./connect-property-rds.md) 属性或 [URL](./url-property-rds.md) 属性中指定的数据源。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.SubmitChanges DataFactory.SubmitChanges Connection, Recordset  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *DataFactory*  
 表示 [RDSServer. DataFactory](./datafactory-object-rdsserver.md) 对象的对象变量。  
  
 *连接*  
 一个 **字符串** 值，该值表示使用 RDS 创建的连接 **。DataControl** 对象的 [连接](./connect-property-rds.md) 属性。  
  
 *Recordset*  
 表示 **Recordset** 对象的对象变量。  
  
## <a name="remarks"></a>备注  
 必须先设置 [Connect](./connect-property-rds.md)、 [Server](./server-property-rds.md)和 [SQL](./sql-property.md) 属性，然后才能将 **SubmitChanges** 方法与 RDS 一起使用 **。DataControl** 对象。  
  
 如果在为同一 **Recordset** 对象调用 **SubmitChanges** 后调用 [CancelUpdate](./cancelupdate-method-rds.md)方法，则 **CancelUpdate** 调用失败，因为这些更改已提交。  
  
 仅发送已更改的记录进行修改，所有更改都成功，否则所有更改将一起失败。  
  
 只能将 **SubmitChanges** 与默认的 **RDSServer** 对象结合使用。 自定义业务对象无法使用此方法。  
  
 如果已设置 **url** 属性， **SubmitChanges** 会将更改提交到 url 指定的位置。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
    :::column-end:::
    :::column:::
        [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [SubmitChanges 方法示例 (VBScript) ](./submitchanges-method-example-vbscript.md)   
 [通讯簿命令按钮](../../guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate 方法 (RDS) ](./cancelupdate-method-rds.md)   
 [Refresh 方法 (RDS)](./refresh-method-rds.md)