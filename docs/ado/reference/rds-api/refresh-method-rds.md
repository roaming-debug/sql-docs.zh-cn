---
description: Refresh 方法 (RDS)
title: Refresh 方法 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
f1_keywords:
- Refresh
- RDS.DataControl::Refresh
- DataControl::Refresh
helpviewer_keywords:
- Refresh method [RDS]
ms.assetid: c90a8050-0ff4-4c83-9925-261f2f2ccfe9
author: rothja
ms.author: jroth
ms.openlocfilehash: f35574b0e9623560ddfafe123340e302a9fc5220
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168801"
---
# <a name="refresh-method-rds"></a>Refresh 方法 (RDS)
重新查询 [连接](./connect-property-rds.md) 属性中指定的数据源，并更新查询结果。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.Refresh  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
## <a name="remarks"></a>备注  
 必须先设置 [Connect](./connect-property-rds.md)、 [Server](./server-property-rds.md)和 [SQL](./sql-property.md) 属性，然后才能使用 **Refresh** 方法。 窗体上的所有与 RDS 关联的数据绑定控件 **。DataControl** 对象将反映新的记录集。 将释放任何预先存在的 [记录集](../ado-api/recordset-object-ado.md) 对象，并放弃任何未保存的更改。 **Refresh** 方法自动使第一条记录成为当前记录。  
  
 在处理数据时，最好定期调用 **Refresh** 方法。 如果检索数据，然后将数据保留在客户端计算机上一段时间，则可能会过时。 您所做的任何更改都可能会失败，因为其他人可能已在您之前更改了记录并提交了更改。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ (VB) Refresh 方法示例 ](../ado-api/refresh-method-example-vb.md)   
 [ (VBScript) Refresh 方法示例 ](./refresh-method-example-vbscript.md)   
 [通讯簿命令按钮](../../guide/remote-data-service/address-book-command-buttons.md)   
 [CancelUpdate 方法 (RDS) ](./cancelupdate-method-rds.md)   
 [ (ADO 的 Refresh 方法) ](../ado-api/refresh-method-ado.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)