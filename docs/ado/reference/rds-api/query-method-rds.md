---
description: Query 方法 (RDS)
title: " (RDS) 的 Query 方法 |Microsoft Docs"
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
helpviewer_keywords:
- Query method [ADO]
ms.assetid: 20f2480f-3758-405d-a379-05a0dce74796
author: rothja
ms.author: jroth
ms.openlocfilehash: 0826d80d786f6f6e944c284422e2a891b509025c
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99163703"
---
# <a name="query-method-rds"></a>Query 方法 (RDS)
使用有效的 SQL 查询字符串返回 [记录集](../ado-api/recordset-object-ado.md)。  
  
> [!IMPORTANT]
>  从 Windows 8 和 Windows Server 2012 开始，Windows 操作系统中不再包含 RDS 服务器组件 (参阅 Windows 8 和 [Windows Server 2012 兼容性指南](https://www.microsoft.com/download/details.aspx?id=27416) ，以了解更多详细信息) 。 在 Windows 的未来版本中将删除 RDS 客户端组件。 请避免在新的开发工作中使用该功能，并着手修改当前还在使用该功能的应用程序。 使用 RDS 的应用程序应迁移到 [WCF 数据服务](/dotnet/framework/wcf/)。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set Recordset = DataFactory.Query(Connection, Query)  
```  
  
#### <a name="parameters"></a>参数  
 *Recordset*  
 表示 **Recordset** 对象的对象变量。  
  
 *DataFactory*  
 表示 [RDSServer. DataFactory](./datafactory-object-rdsserver.md) 对象的对象变量。  
  
 *连接*  
 一个包含服务器连接信息的 **字符串** 值。 这类似于 [连接](./connect-property-rds.md) 属性。  
  
 *查询*  
 一个包含 SQL 查询的 **字符串** 。  
  
## <a name="remarks"></a>备注  
 查询应使用数据库服务器的 SQL 语言。 如果执行的查询出现错误，则返回结果状态。 **查询** 方法不对 **查询** 字符串执行任何语法检查。  
  
## <a name="applies-to"></a>应用于  
 [DataFactory 对象 (RDSServer)](./datafactory-object-rdsserver.md)  
  
## <a name="see-also"></a>另请参阅  
 [DataFactory 对象、Query 方法和 CreateObject 方法示例 (VBScript)](./datafactory-object-query-method-and-createobject-method-example-vbscript.md)