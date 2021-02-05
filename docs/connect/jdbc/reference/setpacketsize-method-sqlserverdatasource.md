---
description: setPacketSize 方法 (SQLServerDataSource)
title: setPacketSize 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setPacketSize
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 5d490edc-a223-4870-a838-784952497e5f
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: c178e8412de5bc59072fa9f6734fd79dad61ba52
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178741"
---
# <a name="setpacketsize-method-sqlserverdatasource"></a>setPacketSize 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置用于与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 通信的当前网络数据包大小，以字节为单位指定。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setPacketSize(int packetSize)  
```  
  
#### <a name="parameters"></a>参数  
 *packetSize*  
  
 一个 int 值，此值包含网络数据包大小  。  
  
## <a name="remarks"></a>备注  
 此属性可接受的值范围是 [-1 | 0 | 512..32767]。 如果将此属性设置为可接受范围外的值，将出现异常。  
  
 在使用传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）加密进行连接时，应用程序可能希望设置 packetSize 属性。 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将与服务器协商数据包大小。 如果 encrypt 属性设置为 true 并且协商的数据包大小大于 Java 虚拟机 (JVM) 的默认安全提供程序的 TLS 记录大小，则驱动程序将报错并终止连接  。  
  
 此外，应用程序可能希望在不请求 TLS 加密的情况下设置 packetSize 属性。 在这种情况下，如果服务器要求客户端支持 TLS 加密，则驱动程序将检查 JVM 的默认安全提供程序的 TLS 记录大小。 如果 packetSize 属性大于 JVM 的默认安全提供程序的 TLS 记录大小，则驱动程序将报错并终止连接。  
  
 有关使用 TLS 的详细信息，请参阅[使用加密](../../../connect/jdbc/using-ssl-encryption.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
