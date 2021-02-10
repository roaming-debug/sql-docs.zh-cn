---
description: DataControl 对象错误代码
title: DataControl 错误代码 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- errors [ADO], DataControl
- DataControl errors [ADO]
ms.assetid: 293df9d5-e1a2-406d-9107-07bf7cdc6f96
author: rothja
ms.author: jroth
ms.openlocfilehash: dbcf0b6aef651e90c90df1cb49af2dbe6a29c674
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100029431"
---
# <a name="datacontrol-object-error-codes"></a>DataControl 对象错误代码
下表列出了 [RDS。DataControl](../../reference/rds-api/datacontrol-object-rds.md) 对象错误代码。 低2字节的正十进制转换、完整错误代码的负小数转换以及十六进制值。

|RDS.DataControl 错误代码|Number|说明|
|---------------------------------|------------|-----------------|
|**IDS_AsyncPending**|4107-2146824175 0x800A1011|异步操作挂起时，无法执行操作。|
|**IDS_BadInlineTablegram**|4105-2146824183 0x800A1009|错误的内联 tablegram。|
|**IDS_CantConnect**|4099-2146824189 0x800A1003|无法连接到服务器。|
|**IDS_CantCreateObject**|4100-2146824188 0x800A1004|无法创建业务对象。|
|**IDS_CantFindDataspace**|4102-2146824186 0x800A1006|"空间" 属性无效。|
|**IDS_CantInvokeMethod**|4101-2146824187 0x800A1005|不能对业务对象调用方法。|
|**IDS_CrossDomainWarning**|4112-2146824170 0x800A1016|此页访问另一个域中的数据。 是否允许这样做？ 若要避免 Internet Explorer 中出现此消息，可以在 " **Internet 选项**" 对话框的 "**安全**" 选项卡上将安全网站添加到 "受信任的站点" 区域。|
|**IDS_InvalidADCClientVersion**|4106-2146824176 0x800A1010|RDS 客户端版本无效-客户端比服务器新。|
|**IDS_INVALIDARG**|5376-2147019520 0x80071500|一个或多个参数无效。|
|**IDS_InvalidBindings**|4097-2146824191 0x800A1001|绑定属性错误。|
|**IDS_InvalidParam**|4110-2146824172 0x800A1014|一个或多个参数无效。|
|**IDS_NOINTERFACE**|5377-2147019519 0x80071501|不支持此类接口。|
|**IDS_NotReentrant**|4111-2146824171 0x800A1015|当事件处理程序仍在处理时，无法执行请求。|
|**IDS_ObjectNotSafe**|4103-2146824185 0x800A1007|此计算机上的安全设置禁止创建业务对象。|
|**IDS_RecordsetNotOpen**|4109-2146824173 0x800A1013|**记录集** 未打开。|
|**IDS_ResetInvalidField**|4108-2146824174 0x800A1012|在 **SortColumn** 或 **FilterColumn** 中指定的列不存在。|
|**IDS_RowsetNotUpdateable**|4104-2146824184 0x800A1008|行集不可更新。|
|**IDS_UnexpectedError**|4351-2146823937 0x800A10FF|意外错误。|
|**IDS_UpdatesFailed**|4098-2146824190 0x800A1002|无法更新数据库。|
|**IDS_URLMONNotFound**|4119-2146824169 0x800A1017|DataControl **URL** 属性需要系统文件 Urlmon.dll，但找不到该文件。|

## <a name="see-also"></a>另请参阅
 [DataControl 对象 (RDS)](../../reference/rds-api/datacontrol-object-rds.md)