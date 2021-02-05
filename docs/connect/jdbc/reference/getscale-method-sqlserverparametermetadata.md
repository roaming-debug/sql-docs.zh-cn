---
description: getScale 方法 (SQLServerParameterMetaData)
title: getScale 方法 (SQLServerParameterMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerParameterMetaData.getScale
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7b8d8d9c-74aa-4e6e-88f1-2fc5c74004ae
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 16fea3543505f839631c2492ab193f02f43dc748
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175092"
---
# <a name="getscale-method-sqlserverparametermetadata"></a>getScale 方法 (SQLServerParameterMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索指定参数的小数点右侧的位数。  
  
## <a name="syntax"></a>语法  
  
```  
  
public int getScale(int param)  
```  
  
#### <a name="parameters"></a>参数  
 *param*  
  
 指示参数索引的 int。  
  
## <a name="return-value"></a>返回值  
 指示指定参数的确定位数的 int。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getScale 方法是由 java.sql.ParameterMetaData 接口中的 getScale 方法指定的。  
  
 此方法获取小数点右侧的列位数。 对于不具有小数点的类型，此方法返回“0”。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerParameterMetaData 方法](../../../connect/jdbc/reference/sqlserverparametermetadata-methods.md)   
 [SQLServerParameterMetaData 成员](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [SQLServerParameterMetaData 类](../../../connect/jdbc/reference/sqlserverparametermetadata-class.md)  
  
  
