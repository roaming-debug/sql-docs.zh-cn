---
title: 使用不带验证的加密 | Microsoft Docs
description: 了解无需验证的 SQL Server 连接加密。 OLE DB Driver for SQL Server 支持不带验证的加密。
ms.custom: ''
ms.date: 06/12/2018
ms.prod: sql
ms.prod_service: database-engine, sql-database, synapse-analytics, pdw
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- data access [OLE DB Driver for SQL Server], encryption
- cryptography [OLE DB Driver for SQL Server]
- MSOLEDBSQL, encryption
- encryption [OLE DB Driver for SQL Server]
- OLE DB Driver for SQL Server, encryption
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 805350b09c624709fa1ea6746c2e97eb2bdbed59
ms.sourcegitcommit: 0310fdb22916df013eef86fee44e660dbf39ad21
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/20/2021
ms.locfileid: "104751117"
---
# <a name="using-encryption-without-validation"></a>使用不带验证的加密
[!INCLUDE [SQL Server](../../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]

[!INCLUDE[Driver_OLEDB_Download](../../../includes/driver_oledb_download.md)]

[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 始终对与登录相关的网络数据包进行加密。 如果在服务器启动时未为其提供任何证书，[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将生成可用于对登录数据包进行加密的自签名证书。  

自签名证书不能保证安全性。 加密握手基于 NT LAN Manager (NTLM)。 强烈建议在 SQL Server 上预配可验证的证书，以实现连接安全性。 传输安全性层 (TLS) 只能通过证书验证来确保安全。

应用程序还可以通过使用连接字符串关键字或连接属性请求对所有网络流量加密。 将访问接口字符串用于 IDbInitialize::Initialize 时，OLE DB 中的关键字为“Encrypt”；将初始化字符串用于 IDataInitialize 时，ADO 和 OLE DB 中的关键字为“Use Encryption for Data”。 这也可以由 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器使用“强制协议加密”选项进行配置，并通过将客户端配置为请求加密连接。 默认情况下，对某一连接的所有网络流量加密要求在服务器上设置证书。 通过将客户端设置为信任服务器上的证书，可能会容易受到中间人攻击。 如果在服务器上部署可验证的证书，请务必将客户端关于信任证书的设置更改为 FALSE。

有关连接字符串关键字的信息，请参阅[将连接字符串关键字用于适用于 SQL Server 的 OLE DB 驱动程序](../../oledb/applications/using-connection-string-keywords-with-oledb-driver-for-sql-server.md )。  
  
 若要在服务器上未设置证书时启用加密，可以使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 配置管理器设置“强制协议加密”和“信任服务器证书”选项。 在这种情况下，如果服务器上未设置可验证的证书，加密将使用不带验证的自签名服务器证书。  
  
 应用程序还可以使用 "TrustServerCertificate" 关键字或与其关联的连接属性确保执行加密。 应用程序设置从不会降低 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 客户端配置管理器设置的安全级别，相反还可能提高安全级别。 例如，如果没有为客户端设置“强制协议加密”，应用程序可能自行请求加密。 甚至是在未设置服务器证书的情况下，为保证加密，应用程序也可能请求加密和 "TrustServerCertificate"。 不过，如果在客户端配置中未启用 "TrustServerCertificate"，则仍需要已设置的服务器证书。 下表介绍了各种情况：  
  
|“强制协议加密”客户端设置|“信任服务器证书”客户端设置|连接字符串/连接属性加密/对数据使用加密|连接字符串/连接属性信任服务器证书|结果|  
|----------------------------------------------|---------------------------------------------|------------------------------------------------------------------------------|----------------------------------------------------------------------|------------|  
|否|空值|否（默认值）|忽略|无加密。|  
|否|不适用|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|否|不适用|是|是|始终加密，但可能使用自签名的服务器证书。|  
|是|否|忽略|忽略|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|否（默认值）|忽略|始终加密，但可能使用自签名的服务器证书。|  
|是|是|是|否（默认值）|仅当存在可验证的服务器证书时才加密，否则连接尝试将失败。|  
|是|是|是|是|始终加密，但可能使用自签名的服务器证书。|  
||||||

> [!CAUTION]
> 上表只提供了不同配置下系统行为的指南。 为了实现连接安全性，请确保客户端和服务器都需要加密。 另请确保，服务器有可验证的证书，且客户端上的 TrustServerCertificate 设置设为 FALSE。

## <a name="ole-db-driver-for-sql-server"></a>适用于 SQL Server 的 OLE DB 驱动程序 
 适用于 SQL Server 的 OLE DB 驱动程序通过添加 SSPROP_INIT_TRUST_SERVER_CERTIFICATE 数据源初始化属性（在 DBPROPSET_SQLSERVERDBINIT 属性集中实现）支持不带验证的加密。 此外，还添加了新的连接字符串关键字“TrustServerCertificate”。 它接受值 yes 或 no；默认值为 no。 使用服务组件时，它接受值 true 或 false；默认值为 false。  
  
 若要详细了解 DBPROPSET_SQLSERVERDBINIT 属性集的增强，请参阅[初始化和授权属性](../../oledb/ole-db-data-source-objects/initialization-and-authorization-properties.md)。  

  
## <a name="see-also"></a>另请参阅  
 [适用于 SQL Server 的 OLE DB 驱动程序功能](../../oledb/features/oledb-driver-for-sql-server-features.md)  
  
  
