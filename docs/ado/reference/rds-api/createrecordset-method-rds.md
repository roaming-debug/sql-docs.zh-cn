---
description: CreateRecordset 方法 (RDS)
title: CreateRecordset 方法 (RDS) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- DataControl::CreateRecordset
- RDS.DataControl::CreateRecordset
- CreateRecordset
- RDSServer.DataFactory::CreateRecordset
- DataFactory::CreateRecordset
helpviewer_keywords:
- CreateRecordset method [RDS]
ms.assetid: 6840b1e5-c04d-4d3e-9dcc-42128c83492f
author: rothja
ms.author: jroth
ms.openlocfilehash: 4a88898c884cbaf0a9c82a882b15ef48c1e3e3a8
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163854"
---
# <a name="createrecordset-method-rds"></a>CreateRecordset 方法 (RDS)
创建一个空的、断开连接的 [记录集](../ado-api/recordset-object-ado.md)。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
object.CreateRecordset(ColumnInfos)  
```  
  
#### <a name="parameters"></a>参数  
 *对象*  
 一个对象变量，它表示 [RDSServer](./datafactory-object-rdsserver.md) 或 [RDS。DataControl](./datacontrol-object-rds.md) 对象。  
  
 *ColumnsInfos*  
 特性的一个 **变量** 数组，用于定义创建的 **记录集中** 的每一列。 每个列定义都包含一个包含四个必需属性和一个可选属性的数组。  
  
|Attribute|说明|  
|---------------|-----------------|  
|名称|列标题的名称。|  
|类型|数据类型的整数。|  
|大小|字符宽度的整数，与数据类型无关。|  
|可空性|布尔值。|  
|缩放 (可选) |此可选特性定义数值字段的小数位数。 如果未指定此值，数值将被截断为三个小数位数。 精度不受影响，但小数点后的位数将被截断为三。|  
  
 然后，将列数组集分组到一个数组中，该数组定义 **记录集**。  
  
## <a name="remarks"></a>备注  
 服务器端业务对象可以使用非 OLE DB 的数据访问接口中的数据（例如包含股票行情的操作系统文件）来填充生成的 **记录集** 。  
  
 下表列出了 **CreateRecordset** 方法支持的 [DataTypeEnum](../ado-api/datatypeenum.md)值。 列出的数字是用于定义字段的引用编号。  
  
 每个数据类型均为固定长度或可变长度。 应将固定长度类型定义为-1，因为大小是预先确定的，并且仍需要大小定义。 可变长度数据类型允许从1到32767的大小。  
  
 对于某些可变数据类型，可以将类型强制转换为替换列中所述的类型。 在创建并填充 **记录集** 之前，你将看不到替换项。 如果需要，可以检查实际数据类型。  
  
|长度|返回的常量|数字|替换|  
|------------|--------------|------------|------------------|  
|固定|**adTinyInt**|16||  
|固定|**adSmallInt**|2||  
|固定|**adInteger**|3||  
|固定|**adBigInt**|20||  
|固定|**adUnsignedTinyInt**|17||  
|固定|**adUnsignedSmallInt**|18||  
|固定|**adUnsignedInt**|19||  
|固定|**adUnsignedBigInt**|21||  
|固定|**adSingle**|4||  
|固定|**adDouble**|5||  
|固定|**adCurrency**|6||  
|固定|**adDecimal**|14||  
|固定|**adNumeric**|131||  
|固定|**adBoolean**|11||  
|固定|**adError**|10||  
|固定|**adGuid**|72||  
|固定|**adDate**|7||  
|固定|**adDBDate**|133||  
|固定|**adDBTime**|134||  
|固定|**adDBTimestamp**|135|7|  
|变量|**adBSTR**|8|130|  
|变量|**adChar**|129|200|  
|变量|**adVarChar**|200||  
|变量|**adLongVarChar**|201|200|  
|变量|**adWChar**|130||  
|变量|**adVarWChar**|202|130|  
|变量|**adLongVarWChar**|203|130|  
|变量|**adBinary**|128||  
|变量|**adVarBinary**|204||  
|变量|**adLongVarBinary**|205|204|  
  
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
 [CreateRecordset 方法示例 (VB) ](../ado-api/createrecordset-method-example-vb.md)   
 [CreateRecordset 方法示例 (VBScript) ](./createrecordset-method-example-vbscript.md)   
 [CreateObject 方法 (RDS)](./createobject-method-rds.md)