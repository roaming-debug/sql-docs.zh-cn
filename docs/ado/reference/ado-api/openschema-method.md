---
description: OpenSchema 方法
title: OpenSchema 方法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: reference
apitype: COM
f1_keywords:
- Connection15::OpenSchema
- Connection15::raw_OpenSchema
helpviewer_keywords:
- OpenSchema method [ADO]
ms.assetid: 850cf3ce-f18f-4e7c-8597-96c1dc504866
author: rothja
ms.author: jroth
ms.openlocfilehash: 9b5ca92e42a68639f6b9e3044894ec779109fff5
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99170682"
---
# <a name="openschema-method"></a>OpenSchema 方法
从提供程序中获取数据库架构信息。  
  
## <a name="syntax"></a>语法  
  
```  
  
Set recordset = connection.OpenSchema(QueryType, Criteria, SchemaID)  
```  
  
## <a name="return-value"></a>返回值  
 返回一个包含架构信息的 [记录集](./recordset-object-ado.md) 对象。 **记录集** 将作为只读静态游标打开。 *QueryType* 确定在 **记录集中** 显示的列。  
  
#### <a name="parameters"></a>参数  
 *QueryType*  
 表示要运行的架构查询类型的任何 [SchemaEnum](./schemaenum.md) 值。  
  
 *条件*  
 可选。 每个 *QueryType* 选项的查询约束数组，如 [SchemaEnum](./schemaenum.md)中所列。  
  
 *SchemaID*  
 OLE DB 规范未定义的提供程序架构查询的 GUID。 如果将 *QueryType* 设置为 **adSchemaProviderSpecific**，则此参数是必需的。否则，不使用此方法。  
  
## <a name="remarks"></a>备注  
 **OpenSchema** 方法返回有关数据源的自定义信息，如数据源中的表、表中的列以及支持的数据类型。  
  
 *QueryType* 参数是一个 GUID，用于指示) 返回的架构 (列。 OLE DB 规范包含架构的完整列表。  
  
 *Criteria* 参数限制架构查询的结果。 *条件* 指定值的数组，这些值必须出现在生成的 **记录集中** 的列的相应子集（称为约束列）中。  
  
 如果提供程序定义了其自己的非标准架构查询（在前面列出）之外，则常量 **adSchemaProviderSpecific** 用于 *QueryType* 参数。 使用此常量时，需要使用 *SchemaID* 参数来传递要执行的架构查询的 GUID。 如果 *QueryType* 设置为 **adSchemaProviderSpecific** ，但未提供 *SchemaID* ，则会产生错误。  
  
 提供程序不需要支持所有 OLE DB 标准架构查询。 具体而言，OLE DB 规范只需要 **adSchemaTables**、 **adSchemaColumns** 和 **adSchemaProviderTypes** 。 但是，提供程序不需要支持前面列出的那些架构查询的 *条件* 约束。  
  
> [!NOTE]
>  **远程数据服务使用情况****OpenSchema** 方法在客户端 [连接](./connection-object-ado.md)对象上不可用。  
  
> [!NOTE]
>  在 Visual Basic 中，不能将具有四字节无符号整数的列 (在 **连接** 对象上从 **OpenSchema** 方法返回的 **记录集中** 的 UI4) 。 有关 OLE DB 数据类型的详细信息，请参阅 [OLE DB 中的数据类型 (OLE DB) ](/previous-versions/windows/desktop/ms714931(v=vs.85)) 和 [附录 A：](/previous-versions/windows/desktop/ms723969(v=vs.85)) Microsoft OLE DB 程序员参考中的数据类型。  
  
> [!NOTE]
>  **Visual c/c + + 用户** 当不使用客户端游标时，检索 ADO 中列架构的 "ORDINAL_POSITION" 将返回 MDAC 2.7、MDAC 2.8 和 Windows 数据访问组件 VT_R8 类型的变体， (Windows DAC) 6.0，而在 MDAC 2.6 中使用的类型则 VT_I4。 对于 MDAC 2.6 编写的程序，仅查找类型为 VT_I4 返回的变量，如果在不进行修改的情况下，将在 MDAC 2.7、MDAC 2.8 和 Windows DAC 6.0 下运行。 此更改是因为 OLE DB 返回的数据类型是 DBTYPE_UI4 的，而在已签名的 VT_I4 类型中没有足够的空间来包含所有可能的值，而不会发生截断，从而导致数据丢失。  
  
## <a name="applies-to"></a>应用于  
 [连接对象 (ADO)](./connection-object-ado.md)  
  
## <a name="see-also"></a>另请参阅  
 [OpenSchema 方法示例 (VB) ](./openschema-method-example-vb.md)   
 [OpenSchema 方法示例 (VC + +) ](./openschema-method-example-vc.md)   
 [开放式方法 (ADO 连接) ](./open-method-ado-connection.md)   
 [ (ADO 记录的 Open 方法) ](./open-method-ado-record.md)   
 [ADO 记录集 (打开方法) ](./open-method-ado-recordset.md)   
 [ADO 流 (打开方法) ](./open-method-ado-stream.md)   
 [附录 A：提供程序](../../guide/appendixes/appendix-a-providers.md)