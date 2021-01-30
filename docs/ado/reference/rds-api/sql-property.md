---
description: SQL 属性
title: SQL 属性 |Microsoft Docs
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.prod: sql
ms.prod_service: connectivity
ms.topic: reference
apitype: COM
helpviewer_keywords:
- SQL property [RDS]
ms.assetid: e0dabf23-a159-4fe5-a962-3df544a21f5c
author: rothja
ms.author: jroth
ms.openlocfilehash: c24b191e11f95428b80a9ecfb3cced4f605e3e76
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99166096"
---
# <a name="sql-property"></a>SQL 属性
指示用于检索 [记录集](../ado-api/recordset-object-ado.md)的查询字符串。  
  
 您可以在设计时在 RDS 中设置 **SQL** 属性 [。DataControl](./datacontrol-object-rds.md) 对象的对象标记，或在运行时在脚本代码中运行。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
Design time: <PARAM NAME="SQL" VALUE="QueryString">  
Run time: DataControl.SQL = "QueryString"  
```  
  
#### <a name="parameters"></a>参数  
 *字符串*  
 一个 **字符串** 值，该值包含有效的 SQL 数据请求。  
  
 *DataControl*  
 表示 RDS 的对象变量 **。DataControl** 对象。  
  
## <a name="remarks"></a>备注  
 通常，这是使用数据库服务器的方言 (的 SQL 语句) ，例如 `"Select * from NewTitles"` 。 若要确保记录匹配并正确更新，可更新查询必须包含长二进制字段或计算字段之外的字段。  
  
 如果自定义服务器端业务对象检索客户端的数据，则 **SQL** 属性是可选的。  
  
## <a name="applies-to"></a>应用于  
 [DataControl 对象 (RDS)](./datacontrol-object-rds.md)  
  
## <a name="see-also"></a>另请参阅  
 [SQL 属性示例 (VBScript) ](./sql-property-example-vbscript.md)   
 [ (RDS) 连接属性 ](./connect-property-rds.md)   
 [查询方法 (RDS) ](./query-method-rds.md)   
 [Refresh 方法 (RDS) ](./refresh-method-rds.md)   
 [SubmitChanges 方法 (RDS)](./submitchanges-method-rds.md)