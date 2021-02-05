---
description: getParameterClassName 方法 (SQLServerParameterMetaData)
title: getParameterClassName 方法 (SQLServerParameterMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerParameterMetaData.getParameterClassName
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 545634d8-f06b-429a-9293-0087d758f359
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b91669a76fe2b7ef3fa38a37360ecd05a1cf9d62
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175285"
---
# <a name="getparameterclassname-method-sqlserverparametermetadata"></a>getParameterClassName 方法 (SQLServerParameterMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索指定 Java 类的完全限定名称，而该 Java 类的实例应传递给 [SQLServerPreparedStatement](../../../connect/jdbc/reference/sqlserverpreparedstatement-class.md) 类中的 [setObject](../../../connect/jdbc/reference/setobject-method-sqlserverpreparedstatement.md) 方法。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.lang.String getParameterClassName(int param)  
```  
  
#### <a name="parameters"></a>参数  
 *param*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 包含完全限定类名称的 String。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getParameterClassName 方法是由 java.sql.ParameterMetaData 接口中的 getParameterClassName 方法指定的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerParameterMetaData 方法](../../../connect/jdbc/reference/sqlserverparametermetadata-methods.md)   
 [SQLServerParameterMetaData 成员](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [SQLServerParameterMetaData 类](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)  
  
  
