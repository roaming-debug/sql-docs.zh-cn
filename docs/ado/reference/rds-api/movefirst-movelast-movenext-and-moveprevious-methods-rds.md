---
description: MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS)
title: MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- MoveLast method [RDS]
- MovePrevious method [RDS]
- MoveFirst method [RDS]
- MoveNext method [RDS]
ms.assetid: 45c80bb5-136f-4204-9df2-78740fa55574
author: rothja
ms.author: jroth
ms.openlocfilehash: 54395a8b0bdc88e873ad25d41ab92304e7f5efe6
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100049277"
---
# <a name="movefirst-movelast-movenext-and-moveprevious-methods-rds"></a>MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (RDS)
移动到指定 [记录集](../ado-api/recordset-object-ado.md) 对象中的第一条、最后一条、下一条记录或上一条记录。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
DataControl.Recordset.{MoveFirst | MoveLast | MoveNext | MovePrevious}  
```  
  
#### <a name="parameters"></a>参数  
 *DataControl*  
 表示 RDS 的对象变量 [。DataControl](./datacontrol-object-rds.md) 对象。  
  
## <a name="remarks"></a>备注  
 可以将 **Move** 方法用于 **RDS。DataControl** 对象，用于在网页上浏览数据绑定控件中的数据记录。 例如，假设您通过绑定到 RDS 在网格中显示一个 **记录集** **。DataControl** 对象。 然后，你可以包括 "第一个"、"最后一个"、"下一个" 和 "上一个" 按钮，用户可以单击这些按钮移动到所显示的 **记录集中** 的第一个、最后一个、下一个或 为此，可调用 **MoveFirst**、 **MoveLast**、 **MoveNext** 和 **MovePrevious** 方法 **。** 第一个、最后一个、下一个和上一个按钮的 onClick 过程中的 DataControl 对象。 [通讯簿示例](../../guide/remote-data-service/address-book-navigation-buttons.md)显示了如何执行此操作。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [ADO (移动方法) ](../ado-api/move-method-ado.md)   
 [MoveFirst、MoveLast、MoveNext 和 MovePrevious 方法 (ADO) ](../ado-api/movefirst-movelast-movenext-and-moveprevious-methods-ado.md)   
 [MoveRecord 方法 (ADO)](../ado-api/moverecord-method-ado.md)