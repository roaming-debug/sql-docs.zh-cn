---
description: 执行包含 Table-Valued 参数 SQL Server Native Client 命令
title: 具有 Table-Valued 参数的命令
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, executing commands containing
ms.assetid: 7ecba6f6-fe7a-462a-9aa3-d5115b6d4529
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 430ce9e2abb28b70cfec3509978ce1bedb1f8ea6
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104753507"
---
# <a name="executing-sql-server-native-client-commands-containing-table-valued-parameters"></a>执行包含 Table-Valued 参数 SQL Server Native Client 命令
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  执行包含表值参数的命令需要两个阶段：  
  
1.  指定参数类型。  
  
2.  绑定参数数据。  

## <a name="table-valued-parameter-specification"></a>指定表值参数  
 使用者可以指定表值参数的类型。 此信息包括表值参数的类型名称。 如果连接的当前默认架构中不存在该表值参数的用户定义表类型，则它还包括架构名称。 根据服务器的支持情况，使用者还可以指定可选的元数据信息（如列的排序），以及指定特定列的所有行都具有默认值。  
  
 要指定表值参数，使用者需要调用 ISSCommandWithParameter::SetParameterInfo，也可以选择调用 ISSCommandWithParameters::SetParameterProperties。 对于表值参数，DBPARAMBINDINFO 结构中 pwszDataSourceType 字段的值为 DBTYPE_TABLE  。 ulParamSize 字段设为 ~0 以指示长度是未知的  。 表值参数的特定属性（如架构名称、类型名称、列顺序和默认列）可以通过 ISSCommandWithParameters::SetParameterProperties 进行设置。  
  
## <a name="table-valued-parameter-binding"></a>绑定表值参数  
 表值参数可以是任意行集对象。 访问接口在执行期间从该对象中读取数据并将表值参数发送到服务器。  
  
 若要绑定表值参数，使用者可调用 IAccessor::CreateAccessor。 表值参数的 DBBINDING 结构的 wType 字段设置为 DBTYPE_TABLE  。 DBBINDING 结构的 pObject 成员为非 NULL 类型，并且 IID 的 pObject 成员设置为 IID_IRowset 或其他任何表值参数行集对象接口    。 DBBINDING 结构中其余字段的设置方式应与流式 BLOB 的对应字段的设置方式相同。  
  
 对于表值参数以及与表值参数关联的行集对象的绑定，存在如下限制：  
  
-   对于表值参数行集列数据，允许使用的状态值只有 DBSTATUS_S_ISNULL 和 DBSTATUS_S_OK。 DBSTATUS_S_DEFAULT 将导致失败，并且绑定的状态值将设置为 DBSTATUS_E_BADSTATUS。  
  
-   可以使用状态 DBSTATUS_S_DEFAULT 标记表值参数。 有效值只有 DBSTATUS_S_DEFAULT 和 DBSTATUS_S_OK。 当状态设置为 DBSTATUS_S_DEFAULT 时，表值参数的值将对应一个空表。  
  
-   必须使用 SSPROP_PARAM_TABLE_DEFAULT_COLUMNS 属性将表值参数中的只读列（标识列或计算列）标记为采用默认值。 此外，还必须通过 SSPROP_PARAM_TABLE_DEFAULT_COLUMNS 属性，将包含默认值的列标记为默认列，以便默认值可用于列的特定表值参数的数据值。 访问接口将忽略为标记为采用默认值的列绑定的数据值。  
  
-   对于具有 DBPROP_COL_AUTOINCREMENT 或 SSPROP_COL_COMPUTED 的列，数据将发送到服务器，除非还设置了 SSPROP_PARAM_TABLE_DEFAULT。  
  
## <a name="see-also"></a>另请参阅  
 [表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [使用表值参数 (OLE DB)](../../relational-databases/native-client-ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
