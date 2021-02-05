---
description: getApplicationName 方法 (SQLServerDataSource)
title: getApplicationName 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.getApplicationName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: f71e501c-ccd7-4a1e-b6ea-4d47a81c18c6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 45e4ce6a37e24ee4d3e8692fe4b145e8e8f019b6
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99168413"
---
# <a name="getapplicationname-method-sqlserverdatasource"></a>getApplicationName 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  返回应用程序名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.String getApplicationName()  
```  
  
## <a name="return-value"></a>返回值  
 一个字符串，此字符串包含应用程序名称，或者“[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]”（如果未设置值）。  
  
## <a name="remarks"></a>注解  
 此应用程序名称用于在各种 [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 分析和日志记录工具中标识特定的应用程序。 如果未设置应用程序名称，getApplicationName 方法则将返回未本地化的字符串“[!INCLUDE[jdbcNoVersion](../../../includes/jdbcnoversion_md.md)]”。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
