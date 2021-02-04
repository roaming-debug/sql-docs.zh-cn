---
description: setSendStringParametersAsUnicode 方法 (SQLServerDataSource)
title: setSendStringParametersAsUnicode 方法 (SQLServerDataSource) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDataSource.setSendStringParametersAsUnicode
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 49198d63-76cb-4843-8d04-e49b1fbb6916
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b21999dfd754787dd74a6f2010f098121e5fc318
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173135"
---
# <a name="setsendstringparametersasunicode-method-sqlserverdatasource"></a>setSendStringParametersAsUnicode 方法 (SQLServerDataSource)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  设置指示是否启用以 UNICODE 格式将字符串参数发送到服务器的 boolean 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void setSendStringParametersAsUnicode(boolean sendStringParametersAsUnicode)  
```  
  
#### <a name="parameters"></a>参数  
 sendStringParametersAsUnicode   
  
 如果以 UNICODE 格式将字符串参数发送到服务器，则为 true。 否则为 **false**。  
  
## <a name="remarks"></a>备注  
 如果 sendStringParametersAsUnicode 属性设置为 true（默认值），字符串参数便会以 UNICODE 格式发送到服务器。 如果 sendStringParametersAsUnicode 设置为 false，字符串参数便会以 ASCII/MBCS 格式（而非 UNICODE 格式）发送到服务器。 如果 sendStringParametersAsUnicode 属性未设置，[getSendStringParametersAsUnicode](../../../connect/jdbc/reference/getsendstringparametersasunicode-method-sqlserverdatasource.md) 返回默认值 true。  
  
 有关 sendStringParametersAsUnicode 连接属性的详细信息，请参阅[设置连接属性](../../../connect/jdbc/setting-the-connection-properties.md)。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDataSource 成员](../../../connect/jdbc/reference/sqlserverdatasource-members.md)   
 [SQLServerDataSource 类](../../../connect/jdbc/reference/sqlserverdatasource-class.md)  
  
  
