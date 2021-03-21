---
title: 表值参数类型发现（OLE DB 驱动程序）
description: 了解使用者如何发现 OLE DB Driver for SQL Server 中表值参数各列的元数据信息。
ms.custom: ''
ms.date: 06/14/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- table-valued parameters, type discovery
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 33525b50f0f0320d8753d12afa9bfca5fb832a72
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104750757"
---
# <a name="table-valued-parameter-type-discovery-ole-db-driver"></a>表值参数类型发现（OLE DB 驱动程序）
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

  如果命令文本已提供给 OLE DB 提供程序，使用者（即使用 OLE DB Driver for SQL Server 的客户端应用程序）可以发现每个命令参数的类型。 了解表值参数的类型之后，使用者可以发现表值参数各列的元数据信息。  
  
 对于多数参数类型，ICommandWithParameters::GetParameterInfo 都支持过程参数的类型信息。 从 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 开始，随着用户定义类型和 xml 数据类型的引入，由于无法通过 ICommandWithParameters 提供用户定义类型信息（名称、架构和目录），GetParameterInfo 方法不再能够完全满足此目的  。 因此，定义了一个新接口 ISSCommandWithParameters 来提供扩展类型信息。  
  
 对于表值参数，同样使用 ISSCommandWithParameters 接口发现详细信息。 在准备好命令对象之后，客户端调用 ISSCommandWithParameters::GetParameterInfo。 对于表值参数，访问接口将 DBPARAMINFO 结构的 wType 成员设置为 DBTYPE_TABLE  。 DBPARAMINFO 结构的 ulParamSize 字段的值为 ~0  。  
  
 使用者随后使用 ISSCommandWithParameters::GetParameterProperties 请求获取附加属性（表值参数类型目录名称、表值参数类型架构名称、表值参数类型名称、列排序和默认列）。  
  
 了解类型名称之后，要检索各个列信息，使用者必须调用 IOpenRowset::OpenRowsetor ，或者通过将表值参数类型名称指定为表名来获取 DBSCHEMA_TABLE_TYPE_COLUMNS 行集。  
  
## <a name="see-also"></a>另请参阅  
 [表值参数 (OLE DB)](../../oledb/ole-db-table-valued-parameters/table-valued-parameters-ole-db.md)   
 [使用表值参数 (OLE DB)](../../oledb/ole-db-how-to/use-table-valued-parameters-ole-db.md)  
  
  
