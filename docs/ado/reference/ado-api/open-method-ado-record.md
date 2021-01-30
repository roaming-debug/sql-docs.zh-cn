---
description: Open 方法（ADO 记录）
title: 开放式方法 (ADO 记录) |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- _Record::raw_Open
- _Record::Open
helpviewer_keywords:
- Open method [ADO]
ms.assetid: ab79a623-88a9-40b6-a017-a658bf19b778
author: rothja
ms.author: jroth
ms.openlocfilehash: cdac33c235e50abf35fbed670d872ca631403b5d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166990"
---
# <a name="open-method-ado-record"></a>Open 方法（ADO 记录）
打开现有 [记录](./record-object-ado.md) 对象，或创建由 **记录** 表示的新项，如文件或目录。  
  
## <a name="syntax"></a>语法  
  
```  
  
Open Source, ActiveConnection, Mode, CreateOptions, Options, UserName, Password  
```  
  
#### <a name="parameters"></a>参数  
 *Source*  
 可选。 一种 **变量** ，该变量可以表示此 **记录** 对象所表示的实体的 URL、 **命令**、打开的 [记录集](./recordset-object-ado.md) 或其他 **记录** 对象、包含 SQL SELECT 语句或表名称的字符串。  
  
 *ActiveConnection*  
 可选。 一个表示连接字符串或打开 [连接](./connection-object-ado.md)对象的 **变量**。  
  
 *模式*  
 可选。 一个 [ConnectModeEnum](./connectmodeenum.md) 值，该值指定结果 **记录** 对象的访问模式。 默认值为 **adModeUnknown**。  
  
 *CreateOptions*  
 可选。 一个 [RecordCreateOptionsEnum](./recordcreateoptionsenum.md) 值，该值指定是否应打开现有的文件或目录，或创建新的文件或目录。 默认值为 **adFailIfNotExists**。 如果设置为默认值，则将从 [mode](./mode-property-ado.md) 属性获取访问模式。 如果 *源* 参数不包含 URL，则会忽略此参数。  
  
 *选项*  
 可选。 一个 [RecordOpenOptionsEnum](./recordopenoptionsenum.md) 值，该值指定用于打开 **记录** 的选项。 默认值为 **adOpenRecordUnspecified**。 这些值可以组合在一起。  
  
 *UserName*  
 可选。 一个 **字符串** 值，该值包含用户 ID，该 ID 在需要时授予对 *源* 的访问权限。  
  
 *密码*  
 可选。 一个包含密码的 **字符串** 值（如果需要）将验证 *用户名*。  
  
## <a name="remarks"></a>备注  
 *源* 可能是：  
  
-   URL。 如果 URL 的协议是 http，则默认情况下将调用 Internet 访问接口。 如果 URL 指向包含可执行脚本的节点 (如。ASP page) 中，默认情况下会打开包含源（而不是执行的内容）的 **记录** 。 使用 *Options* 参数来修改此行为。  
  
-   **记录** 对象。 从另一个 **记录** 打开的 **记录** 对象将克隆原始 **记录** 对象。  
  
-   **命令** 对象。 已打开的 **记录** 对象表示通过执行 **命令** 返回的单个行。 如果结果包含多行，则将第一行的内容放在记录中，并将错误添加到 **错误** 集合中。  
  
-   一个 SQL SELECT 语句。 打开的 **记录** 对象表示通过执行字符串的内容返回的单个行。 如果结果包含多行，则将第一行的内容放在记录中，并将错误添加到 **错误** 集合中。  
  
-   表名称。  
  
 如果 **Record** 对象表示无法使用 URL 访问的实体 (例如，从数据库) 派生的 **记录集** 的行，则 [ParentURL](./parenturl-property-ado.md) 属性和使用 **adRecordURL** 常量访问的字段的值均为 null。  
  
> [!NOTE]
>  使用 http 方案的 Url 将自动调用 [用于 Internet 发布的 Microsoft OLE DB 提供程序](../../guide/appendixes/microsoft-ole-db-provider-for-internet-publishing.md)。 有关详细信息，请参阅 [绝对和相对 url](../../guide/data/absolute-and-relative-urls.md)。  
  
## <a name="applies-to"></a>应用于  
 [记录对象 (ADO)](./record-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [ADO 记录集 (打开方法) ](./open-method-ado-recordset.md)   
 [ADO 流 (打开方法) ](./open-method-ado-stream.md)   
 [OpenSchema 方法](./openschema-method.md)