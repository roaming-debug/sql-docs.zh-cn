---
description: getSchemas 方法 (String, String)
title: getSchemas 方法 (String, String) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 672171ac-976f-4605-9bee-2a5e141d92cb
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 02451ce51b9d8df378f5560d06d8e716694fc0aa
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99175074"
---
# <a name="getschemas-method-string-string"></a>getSchemas 方法 (String, String)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  通过使用指定目录名称和架构名称来检索当前数据库中可用的架构名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
public ResultSet getSchemas(java.lang.String catalog,  
                       java.lang.String schemaPattern)  
```  
  
#### <a name="parameters"></a>参数  
 *catalog*  
  
 数据库中的目录名称。 如果该名称为空字符串 ""，则结果将包括无目录的架构。 如果此字符串为“null”，目录名称则不可用于搜索  。  
  
 schemaPattern  
  
 架构的名称。 如果此字符串为“null”，架构名称则不可用于搜索  。  
  
## <a name="return-value"></a>返回值  
 一个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getSchemas 方法是由 java.sql.DatabaseMetaData 接口中的 getSchemas 方法指定的。  
  
 由 getSchemas 方法返回的结果集包含以下信息：  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|TABLE_SCHEM|**字符串**|架构的名称。|  
|TABLE_CATALOG|**字符串**|架构的目录名称。|  
  
 结果先按 TABLE_CATALOG 再按 TABLE_SCHEM 排序。 各行均以 TABLE_SCHEM 作为第一列并以 TABLE_CATALOG 作为第二列。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
