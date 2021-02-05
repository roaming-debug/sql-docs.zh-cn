---
description: isNullable 方法 (SQLServerParameterMetaData)
title: isNullable 方法 (SQLServerParameterMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerParameterMetaData.sNullable
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: d7e07cff-6fc4-4c9c-8e8f-838c77734bc5
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 06f47866f65edda3102f5d3ad7c3bbdce87bafd1
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99177347"
---
# <a name="isnullable-method-sqlserverparametermetadata"></a>isNullable 方法 (SQLServerParameterMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索指定参数中是否允许 Null 值。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int isNullable(int param)  
```  
  
#### <a name="parameters"></a>参数  
 *param*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 指示指定参数的为 Null 性的 int，可以为以下值之一：  
  
 ParameterMetaData.parameterNoNulls  
  
 ParameterMetaData.parameterNullable  
  
 ParameterMetaData.parameterNullabilityUnknown  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 isNullable 方法是由 java.sql.ParameterMetaData 接口中的 isNullable 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerParameterMetaData 方法](../../../connect/jdbc/reference/sqlserverparametermetadata-methods.md)   
 [SQLServerParameterMetaData 成员](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [SQLServerParameterMetaData 类](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)  
  
  
