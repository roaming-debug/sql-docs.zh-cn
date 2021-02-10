---
description: CancelUpdate 方法 (RDS)
title: CancelUpdate 方法 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- CancelUpdate method [RDS]
ms.assetid: 76d8a6e9-bc6c-4ea0-8e7a-2bae5ed06650
author: rothja
ms.author: jroth
ms.openlocfilehash: a321c84beb8277f05b0779a540a72b6a82b87933
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049537"
---
# <a name="cancelupdate-method-rds"></a>CancelUpdate 方法 (RDS)
取消对 [记录集](../ado-api/recordset-object-ado.md) 对象的当前行或新行所做的任何更改。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.CancelUpdate  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
## <a name="remarks"></a>备注  
 OLE DB 的游标服务将保留原始值和更改缓存的副本。 调用 **CancelUpdate** 时，更改的缓存将重置为空，并且所有绑定控件都将用原始数据进行刷新。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [CancelUpdate 方法示例 (VBScript) ](./cancelupdate-method-example-vbscript.md)   
 [通讯簿命令按钮](../../guide/remote-data-service/address-book-command-buttons.md)   
 [ADO (取消方法) ](../ado-api/cancel-method-ado.md)   
 [ (RDS) 的 Cancel 方法 ](./cancel-method-rds.md)   
 [CancelBatch 方法 (ADO) ](../ado-api/cancelbatch-method-ado.md)   
 [CancelUpdate 方法 (ADO) ](../ado-api/cancelupdate-method-ado.md)   
 [Refresh 方法 (RDS) ](./refresh-method-rds.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)