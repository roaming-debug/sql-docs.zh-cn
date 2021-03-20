---
description: 'SQL Server Native Client 数据源属性 (OLE DB) '
title: 数据源属性 (Native Client OLE DB 提供程序) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: native-client
ms.topic: reference
helpviewer_keywords:
- SQL Server Native Client OLE DB provider, data source properties
- properties [OLE DB]
- data source properties [OLE DB]
- OLE DB data source properties [SQL Server Native Client]
ms.assetid: 6e14fefc-4e0b-4847-a833-4cf0abe65d50
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: e9a87b7f4a43b61e5c4883b46168a5d6103bfced
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104756027"
---
#  <a name="sql-server-native-client-data-source-properties-ole-db"></a>SQL Server Native Client 数据源属性 (OLE DB) 
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

  [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]Native Client OLE DB 提供程序按如下方式实现数据源属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|DBPROP_CURRENTCATALOG|R/W：读/写 默认值：无<br /><br /> 说明：的值 DBPROP_CURRENTCATALOG 报告 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 本机客户端 OLE DB 提供程序会话的当前数据库。 设置该属性值与使用 [!INCLUDE[tsql](../../includes/tsql-md.md)] USE database 语句设置当前数据库的效果相同  。<br /><br /> 从 [!INCLUDE[ssVersion2005](../../includes/ssversion2005-md.md)] 开始，如果调用 [sp_defaultdb](../../relational-databases/system-stored-procedures/sp-defaultdb-transact-sql.md)，并以小写字母指定数据库名称，即使该数据库名称最初是使用大小写混合形式创建的，DBPROP_CURRENTCATALOG 仍将以小写字母返回该名称。 对于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的早期版本，DBPROP_CURRENTCATALOG 将返回预期的大小写混合形式。|  
|DBPROP_MULTIPLECONNECTIONS|R/W：读/写 默认值：VARIANT_FALSE<br /><br /> 说明：如果将 DBPROP_MULTIPLECONNECTIONS 设置为 VARIANT_TRUE，当连接运行的命令未生成行集或生成的行集不是服务器游标时，执行其他命令时将新建一个连接，以便执行该命令。<br /><br /> [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果 DBPROP_MULTIPLECONNECTION VARIANT_FALSE 或者如果某个事务在连接上处于活动状态，则 Native Client OLE DB 提供程序将不会创建另一个连接。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]如果 DBPROP_MULTIPLECONNECTIONS 是 VARIANT_FALSE，则 Native Client OLE DB 提供程序将返回 DB_E_OBJECTOPEN，并且如果存在活动事务，则返回 E_FAIL。 事务和锁定是在每个连接的基础上通过 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 来管理的。 如果生成第二个连接，则该独立连接上的命令不能共享锁定。 若要确保一个命令不会阻塞另一个命令，请对其他命令所请求的行保持锁定。 当创建多个会话时，将同样保持该锁定。<br /><br /> 每个会话都具有一个单独的连接。|  
  
 在特定于访问接口的属性集中 DBPROPSET_SQLSERVERDATASOURCE， [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] Native Client OLE DB 提供程序定义以下附加数据源属性。  
  
|属性 ID|说明|  
|-----------------|-----------------|  
|SSPROP_ENABLEFASTLOAD|R/W：读/写 默认值：VARIANT_FALSE<br /><br /> 说明：若要从内存启用大容量复制，应将 SSPROP_ENABLEFASTLOAD 属性设置为 VARIANT_TRUE。 对数据源设置该属性之后，使用者可通过新建的会话访问 [IRowsetFastLoad](../../relational-databases/native-client-ole-db-interfaces/irowsetfastload-ole-db.md) 接口。<br /><br /> 如果将该属性设置为 VARIANT_TRUE，则可以通过请求 IID_IRowsetFastLoad 接口或将 SSPROP_IRowsetFastLoad 设置为 VARIANT_TRUE，利用 IOpenRowset::OpenRowset 来使用 IRowsetFastLoad 接口     。|  
|SSPROP_ENABLEBULKCOPY|R/W：读/写 默认值：VARIANT_FALSE<br /><br /> 说明：若要从文件启用大容量复制，应将 SSPROP_ENABLEBULKCOPY 属性设置为 VARIANT_TRUE。 对数据源设置该属性之后，使用者可以访问与会话位于同一级别的 IBCPSession 接口。<br /><br /> 此外，还必须将 SSPROP_IRowsetFastLoad 设置为 VARIANT_TRUE。|  
  
## <a name="see-also"></a>另请参阅  
 [数据源对象 (OLE DB)](../../relational-databases/native-client-ole-db-data-source-objects/data-source-objects-ole-db.md)  
  
  
