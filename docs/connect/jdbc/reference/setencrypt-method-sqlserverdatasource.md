---
description: setEncrypt 方法 (SQLServerDataSource)
title: setEncrypt 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- setEncrypt Method (SQLServerDataSource)
apilocation:
- setEncrypt Method (SQLServerDataSource)
apitype: Assembly
ms.assetid: 0c85a9c1-f27c-457e-8461-403cc03e2d17
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: a041a866e825cef59441cbfd42ca408d1d3cfd0d
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99179022"
---
# <a name="setencrypt-method-sqlserverdatasource"></a>setEncrypt 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置一个布尔值，此值指示是否启用了 encrypt 属性  。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setEncypt(boolean encrypt)  
```  
  
#### <a name="parameters"></a>参数  
 *encrypt*  
  
 如果在客户端和 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 之间启用传输层安全性 (TLS)（以前称为安全套接字层 (SSL)）加密，则为 true  。 否则为 **false**。  
  
## <a name="remarks"></a>备注  
 如果 encrypt 属性设置为 true，则在服务器已安装有证书的情况下，[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将确保 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 对在客户端与服务器之间发送的所有数据使用 TLS 加密  。 默认值是 **false** 秒。  
  
 JDBC 驱动程序在尝试建立 TLS 握手时，会检测运行它的 Java 虚拟机 (JVM)。  
  
 如果 encrypt 属性设置为 true，则 [!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将使用 JVM 的默认 JSSE 安全提供程序与 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 协商 TLS 加密  。 默认的安全提供程序可能不支持成功协商 TLS 加密所需的全部功能。 例如，默认安全提供程序可能不支持在 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] TLS/SSL 证书中使用的 RSA 公钥的大小。 在这种情况下，默认的安全提供程序可能报错，此错误将导致 JDBC 驱动程序终止连接。 为了解决这一问题，请执行下列操作之一：  
  
-   使用具有较小 RSA 公钥的服务器证书配置 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]  
  
-   在“\<java-home>/lib/security/java.security”安全属性文件中将 JVM 配置为使用其他 JSSE 安全提供程序  
  
-   使用其他 JVM  
  
 如果未指定 encrypt 属性或将此属性设置为 false，驱动程序将不会强制 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 支持 TLS 加密  。 如果 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例未配置为强制使用 TLS 加密，则将在不进行任何加密的情况下建立连接。 如果将 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 实例配置为强制使用 TLS 加密，则在经正确配置的 JVM 上运行时，[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)] 将自动启用 TLS 加密，否则连接将终止并且驱动程序将报错。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
