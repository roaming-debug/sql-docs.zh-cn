---
title: 使用数据库镜像 | Microsoft Docs
description: OLE DB Driver for SQL Server 支持数据库镜像。 一旦为数据库配置了镜像，开发人员就不需要采取其他任何操作。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- database mirroring [SQL Server], interoperability
- OLE DB Driver for SQL Server, database mirroring
- data access [OLE DB Driver for SQL Server], database mirroring
- database mirroring [SQL Server], connecting clients to
- MSOLEDBSQL, database mirroring
- OLE DB Driver for SQL Server, database mirroring
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f46815556d8387da8f99689716158d6cb62a0ff9
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751167"
---
# <a name="using-database-mirroring"></a>使用数据库镜像
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

    
> [!NOTE]  
>  [!INCLUDE[ssNoteDepFutureAvoid](../../../includes/ssnotedepfutureavoid-md.md)] 改为使用 [!INCLUDE[ssHADR](../../../includes/sshadr-md.md)]。  
  
 在 [!INCLUDE[ssVersion2005](../../../includes/ssversion2005-md.md)] 中引入的数据库镜像是一种提高数据库可用性和改善数据冗余性的解决方案。 适用于 SQL Server 的 OLE DB 驱动程序提供对数据库镜像的隐式支持，因此，在针对数据库配置数据库镜像之后，开发人员无需编写任何代码或采取任何其他操作。  
  
 数据库镜像是按数据库实现的，它在备用服务器上保留一份 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 产品数据库的副本。 该服务器根据数据库镜像会话的配置和状态，充当热备用或温备用服务器。 热备用服务器支持快速故障转移且不会丢失已提交事务，温备用服务器支持强制服务（可能丢失数据）。  
  
 产品数据库称为“主体数据库”，备份副本称为“镜像数据库” 。 主体数据库和镜像数据库必须位于单独的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例（服务器实例）中，并且应位于单独的计算机中（如果可能）。  
  
 生产服务器实例称为“主体服务器”，而备用服务器实例称为“镜像服务器”，两个实例相互通信。 主体服务器和镜像服务器在数据库镜像会话中充当伙伴。 如果主体服务器失败，镜像服务器可以通过称为“故障转移”的过程将其数据库转为主体数据库。 例如，Partner_A 和 Partner_B 为两个伙伴服务器，主数据库最初位于主服务器 Partner_A 上，镜像数据库位于镜像服务器 Partner_B 上。 如果 Partner_A 脱机，则 Partner_B 上的数据库便可通过故障转移而成为当前主数据库。 Partner_A 重新加入镜像会话后，它将成为镜像服务器，而其数据库将成为镜像数据库。  
  
 备用数据库镜像配置提供不同级别的性能和数据安全，并支持不同形式的故障转移。 有关详细信息，请参阅[数据库镜像 (SQL Server)](../../../database-engine/database-mirroring/database-mirroring-sql-server.md)。  
  
 指定镜像数据库名称时可以使用别名。  
  
> [!NOTE]  
>  有关与镜像数据库之间的初始连接尝试和重新连接尝试的信息，请参阅[将客户端连接到数据库镜像会话 (SQL Server)](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)。  
  
## <a name="programming-considerations"></a>编程注意事项  
 当主体数据库服务器失败时，客户端应用程序则接收错误以响应 API 调用，这指示与数据库之间的连接已丢失。 发生此种情况时，对数据库执行的任何未提交的更改将丢失，并回滚当前事务。 如果发生这种情况，应用程序应关闭连接（或释放数据源对象），然后再重新打开它。 连接将以透明方式重新定向到镜像数据库，该数据库现在充当主体服务器。  
  
 建立连接时，主体服务器将其故障转移伙伴的标识发送到客户端，以便在发生故障转移时使用。 当应用程序在主体服务器失败后尝试建立连接时，客户端不知道故障转移伙伴的标识。 为使客户端能够处理这种情况，初始化属性和关联的连接字符串关键字允许客户端自己指定故障转移伙伴的标识。 客户端属性仅在此种情况下使用；如果主体服务器可用，则不使用此属性。 如果客户端提供的故障转移伙伴服务器未引用充当故障转移伙伴的服务器，该服务器将拒绝连接。 若要使应用程序适应配置更改，可以通过在建立连接后检测该属性来确定实际故障转移伙伴的标识。 如果首次尝试建立连接失败，则应考虑缓存伙伴信息，以更新连接字符串或设计重试策略。  
  
> [!NOTE]  
>  如果希望在 DSN、连接字符串或连接属性/特性中使用此功能，必须显式指定连接要使用的数据库。 否则，OLE DB Driver for SQL Server 将不会尝试故障转移到伙伴数据库。  
>   
>  镜像是面向数据库的功能。 使用多个数据库的应用程序可能无法利用此功能。  
>   
>  此外，服务器名称不区分大小写，而数据库名称区分大小写。 因此，应确保在 DSN 和连接字符串中使用相同的大小写。  
  
## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序  
 适用于 SQL Server 的 OLE DB 驱动程序通过连接和连接字符串属性支持数据库镜像。 已向 DBPROPSET_SQLSERVERDBINIT 属性集添加 SSPROP_INIT_FAILOVERPARTNER 属性，FailoverPartner 关键字是 DBPROP_INIT_PROVIDERSTRING 的新连接字符串属性。 有关详细信息，请参阅[结合使用连接字符串关键字和 OLE DB Driver for SQL Server](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md)。  
  
 只要加载访问接口，故障转移缓存就会一直保留，直到调用 CoUninitialize 或应用程序引用由适用于 SQL Server 的 OLE DB 驱动程序管理的某一对象（如数据源对象）为止。  
  
 有关数据库镜像的 OLE DB Driver for SQL Server 支持的详细信息，请参阅[初始化和授权属性](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)。  
 
  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)   
 [将客户端连接到数据库镜像会话 (SQL Server)](../../../database-engine/database-mirroring/connect-clients-to-a-database-mirroring-session-sql-server.md)   
 [数据库镜像 (SQL Server)](../../../database-engine/database-mirroring/database-mirroring-sql-server.md)  
  
  
