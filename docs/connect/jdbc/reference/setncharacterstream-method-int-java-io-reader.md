---
description: 设置为 Reader 对象的 setNCharacterStream 方法 - int
title: 设置为 Reader 对象的 setNCharacterStream 方法 - int | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 7732746b-eda5-469e-8567-e8546c4d81cd
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 460e9b843d60e492eb435493a2c3abaeebac4b98
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99173343"
---
# <a name="setncharacterstream-method-int-javaioreader"></a>setNCharacterStream 方法 (int, java.io.Reader)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  将指定参数设置为指定的 Reader 对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final void setNCharacterStream(int parameterIndex,  
                                                 java.io.Reader value)  
```  
  
#### <a name="parameters"></a>参数  
 parameterIndex  
  
 指示参数索引的 int。  
  
 *value*  
  
 包含参数值的 Reader 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 setNCharacterStream 方法是由 java.sql.PreparedStatement 接口中的 setNCharacterStream 方法指定的。  
  
 此方法应用于 NCHAR、NVARCHAR、NTEXT 和 XML 数据类型。  
  
## <a name="see-also"></a>另请参阅  
 [setNCharacterStream 方法 &#40;SQLServerPreparedStatement&#41;](../../../connect/jdbc/reference/setncharacterstream-method-sqlserverpreparedstatement.md)   
 [SQLServerPreparedStatement 成员](../../../connect/jdbc/reference/sqlserverpreparedstatement-members.md)  
  
  
