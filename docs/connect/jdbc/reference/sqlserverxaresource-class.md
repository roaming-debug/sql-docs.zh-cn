---
description: SQLServerXAResource 类
title: SQLServerXAResource 类 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: df957b79-536f-4db7-b6ac-3d59343559fc
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 27faf26e6bd35d492bda5c3da9484b7fe50bcb7f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181170"
---
# <a name="sqlserverxaresource-class"></a>SQLServerXAResource 类
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  表示用于 XA 分布式事务管理的 XAResource。  
  
 **包：** com.microsoft.sqlserver.jdbc  
  
 **扩展：** java.lang.Object  
  
 **实现：** javax.transaction.xa.XAResource  
  
## <a name="syntax"></a>语法  
  
```  
  
public class SQLServerXAResource  
```  
  
## <a name="remarks"></a>备注  
 通过使用 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分布式事务管理器 (DTC) 在 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] 中实现 XA 事务。 SQLServerXAResource 类调用名为 sqljdbc_xa.dll 的 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 扩展 dll，此 dll 充当 DTC 接口。 SQLServerXAResource 接收到的 XA 调用（XA_START、XA_END、XA_PREPARE 等）将映射到针对 DTC 函数的相应调用。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerXAResource 成员](../../../connect/jdbc/reference/sqlserverxaresource-members.md)   
 [JDBC 驱动程序 API 参考](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
