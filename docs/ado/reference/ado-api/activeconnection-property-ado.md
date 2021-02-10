---
description: ActiveConnection 属性 (ADO)
title: " (ADO) 的 ActiveConnection 属性 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Command15::ActiveConnection
- Recordset15::get_ActiveConnection
- _Record::ActiveConnection
helpviewer_keywords:
- ActiveConnection property [ADO]
ms.assetid: 52d0a96c-14fb-4ad9-b004-4d821bc0a6db
author: rothja
ms.author: jroth
ms.openlocfilehash: 318bb7478bdbc6f3b4007f788045438cdb91dfe3
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100035917"
---
# <a name="activeconnection-property-ado"></a>ActiveConnection 属性 (ADO)
指示指定的[命令](./command-object-ado.md)、[记录集](./recordset-object-ado.md)或[记录](./record-object-ado.md)对象当前属于哪个[连接](./connection-object-ado.md)对象。  
  
## <a name="settings-and-return-values"></a>设置和返回值  
 如果连接已关闭，则设置或返回一个 **字符串** 值，该值包含连接的定义; 如果连接处于打开状态，则为包含当前 **连接** 对象的 **变体**。 默认值为 null 对象引用。 请参见 [ConnectionString](./connectionstring-property-ado.md) 属性。  
  
## <a name="remarks"></a>备注  
 使用 **ActiveConnection** 属性可确定要对其执行指定 **命令** 对象的 **连接** 对象，或者将打开指定的 **记录集**。  
  
## <a name="command"></a>命令  
 对于 **Command** 对象， **ActiveConnection** 属性是可读/写的。  
  
 如果尝试在将此属性设置为打开的 **连接** 对象或有效连接字符串之前对 **命令** 对象调用 [Execute](./execute-method-ado-command.md)方法，则会发生错误。  
  
 如果将 **连接** 对象分配给 **ActiveConnection** 属性，则必须打开该对象。 分配关闭的连接对象会导致错误。  
  
### <a name="note"></a>注意  
 **Microsoft Visual Basic** 将 **ActiveConnection** 属性设置为 " *无* " 将 **命令** 对象与当前 **连接** 取消关联，并使提供程序释放数据源上的任何关联资源。 然后，可以将 **命令** 对象与相同或其他 **连接** 对象关联。 某些提供程序允许您将属性设置从一个连接更改为另一个 **连接** ，而无需先将该属性设置为 *Nothing*。  
  
 如果 **Command** 对象的 [parameters](./parameters-collection-ado.md)集合包含提供程序提供的参数，则如果将 **ActiveConnection** 属性设置为 *Nothing* 或其他 **连接** 对象，则会清除该集合。 如果手动创建 [参数](./parameter-object.md)对象，并使用它们来填充 **Command** 对象的 **Parameters** 集合，则将 **ActiveConnection** 属性设置为 *Nothing* 或另一个 **连接** 对象会使 **参数** 集合保持不变。  
  
 关闭与 **命令** 对象关联的 **连接** 对象会将 **ActiveConnection** 属性设置为 *Nothing*。 如果将此属性设置为关闭的 **连接** 对象，则会生成错误。  
  
## <a name="recordset"></a>记录集  
 对于打开的 **记录集** 对象或 [源](./source-property-ado-recordset.md)属性设置为有效 **命令** 对象的 **记录集** 对象， **ActiveConnection** 属性是只读的。 否则为可读/写。  
  
 可将此属性设置为有效的 **连接** 对象或有效的连接字符串。 在这种情况下，提供程序使用此定义创建新的 **连接** 对象，并打开连接。 此外，提供程序可以将此属性设置为新的 **连接** 对象，以提供一种方法来访问 **连接** 对象，以获取扩展错误信息或执行其他命令。  
  
 如果使用 [open](./open-method-ado-recordset.md)方法的 *ActiveConnection* 参数打开 **Recordset** 对象，则 **ActiveConnection** 属性将继承参数的值。  
  
 如果将 **recordset** 对象的 **Source** 属性设置为有效的 **命令** 对象变量，则 **记录集** 的 **ActiveConnection** 属性将继承 **命令** 对象的 **ActiveConnection** 属性的设置。  
  
> [!NOTE]
>  **远程数据服务使用情况** 当在客户端 **记录集** 对象上使用此属性时，只能将此属性设置为 Microsoft Visual Basic 或 Visual Basic，脚本编写版) *中的 (。*  
  
## <a name="record"></a>记录  
 当 **记录** 对象关闭时，此属性是可读/写的，并且可能包含连接字符串或对打开的 **连接** 对象的引用。 当 **Record** 对象处于打开状态时，此属性为只读，并且包含对打开的 **连接** 对象的引用。  
  
 当从 URL 打开 **记录** 对象时，将隐式创建 **连接** 对象。 通过将 **连接** 对象分配给此属性，或使用 **连接** 对象作为 [open](./open-method-ado-record.md)方法调用中的参数，打开具有现有的开放式 **连接** 对象的 **记录**。 如果从现有 **记录** 或 [记录集中](./recordset-object-ado.md)打开 **记录**，则该记录将自动与该 **记录** 或 **记录集** 对象的 **连接** 对象关联。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  

:::row:::
    :::column:::
        [命令对象 (ADO)](./command-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录对象 (ADO)](./record-object-ado.md)  
    :::column-end:::
    :::column:::
        [记录集对象 (ADO)](./recordset-object-ado.md)  
    :::column-end:::
:::row-end:::

## <a name="see-also"></a>另请参阅  
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VB) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vb.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (VC + +) ](./activeconnection-commandtext-commandtimeout-commandtype-size-example-vc.md)   
 [ActiveConnection、CommandText、CommandTimeout、CommandType、Size 和 Direction 属性示例 (JScript) ](./activeconnection-commandtext-timeout-type-size-example-jscript.md)   
 [ADO) 的连接对象 (](./connection-object-ado.md)   
 [ConnectionString 属性 (ADO)](./connectionstring-property-ado.md)