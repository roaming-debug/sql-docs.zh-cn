---
description: Recordset、SourceRecordset 属性 (RDS)
title: 记录集，SourceRecordset 属性 (RDS) |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Recordset property [ADO]
ms.assetid: a29e3fb9-306d-497a-9a59-1856a914e5e9
author: rothja
ms.author: jroth
ms.openlocfilehash: b4f8f1db220bdf68205e3bd4628a27d0b57705b8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168806"
---
# <a name="recordset-sourcerecordset-properties-rds"></a>Recordset、SourceRecordset 属性 (RDS)
指示从自定义业务对象返回的 **记录集** 对象。  
  
 **适用于：** [DATACONTROL 对象 (RDS)](./datacontrol-object-rds.md)  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.SourceRecordset = Recordset  
Recordset = DataControl.Recordset   
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *Recordset*  
 表示 **Recordset** 对象的对象变量。  
  
## <a name="remarks"></a>备注  
 可以将 **SourceRecordset** 属性设置为从自定义业务对象返回的 [记录集](../ado-api/recordset-object-ado.md) 。  
  
 通过这些属性，应用程序可以通过自定义进程来处理绑定过程。 它们将接收包装在 **记录集中** 的行集，以便可以直接与 **记录集** 交互，执行设置属性或循环访问 **记录集** 等操作。  
  
 您可以在运行时在脚本代码中设置 **SourceRecordset** 属性或读取 **Recordset** 属性。  
  
 **SourceRecordset** 是一个只写属性，与 " **记录集** " 属性相反，后者是一个只读属性。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [记录集和 SourceRecordset 属性示例 (VBScript) ](./recordset-and-sourcerecordset-properties-example-vbscript.md)   
 [CreateRecordset 方法 (RDS) ](./createrecordset-method-rds.md)   
 [Query 方法 (RDS)](./query-method-rds.md)