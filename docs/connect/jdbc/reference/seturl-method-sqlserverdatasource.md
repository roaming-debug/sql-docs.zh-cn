---
description: setURL 方法 (SQLServerDataSource)
title: setURL 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setURL
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: bea70100-ac98-4625-8748-ef7cc0b111ea
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 0c2af37087028c520d3cfbb7c8b8918009c53d11
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178440"
---
# <a name="seturl-method-sqlserverdatasource"></a>setURL 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置用于连接数据源的 URL。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setURL(java.lang.String url)  
```  
  
#### <a name="parameters"></a>参数  
 *url*  
  
 一个包含 URL 的字符串。  
  
## <a name="remarks"></a>注解  
 出于安全原因，请不要在提供给 setURL 方法的 URL 中包括密码。 这是因为：第三方 Java 应用程序服务器经常会在其数据源配置用户界面中显示为 URL 属性设置的值。 请改用 [setPassword](../../../connect/jdbc/reference/setpassword-method-sqlserverdatasource.md) 方法设置密码值。 Java 应用程序服务器不会在配置用户界面中显示在其数据源中设置的密码。  
  
> [!NOTE]  
>  如果在调用 [getURL](../../../connect/jdbc/reference/geturl-method-sqlserverdatasource.md) 方法之前未调用 setURL 方法，则 getURL 将返回默认值“jdbc:sqlserver://”。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
