---
description: updateAsciiStream 方法 (int, java.io.InputStream)
title: updateAsciiStream 方法 (int, java.io.InputStream)
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 1dcc3d4f-ae30-45c0-afad-a531358807af
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 596a09583c044c42f429b23a640c3df5d3a9d346
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99182116"
---
# <a name="updateasciistream-method-int-javaioinputstream"></a>updateAsciiStream 方法 (int, java.io.InputStream)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  使用 ASCII 流值更新指定列。  
  
## <a name="syntax"></a>语法  
  
```  
  
public void updateAsciiStream(int columnIndex,  
                              java.io.InputStream x)  
```  
  
#### <a name="parameters"></a>parameters  
 columnIndex   
  
 指示列索引的 int  。  
  
 *x*  
  
 InputStream 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 updateAsciiStream 方法是由 java.sql.ResultSet 接口中的 updateAsciiStream 方法指定的。  
  
 此方法将 InputStream 对象中的 ASCII 字符（字节）传递给可转换的字符列，即 Unicode 的 ASCII 范围 [0x00 - 0x7F] 以及 874、932、936、949、950 和 1250-1258 代码页。 此方法执行到目标排序规则页的转换。 尝试更新不可转换的目标列将引发异常。 对于二进制列，会传递原始字节。  
  
 将此方法应用于 image、text 和 ntext[!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 数据类型可能会影响性能。  
  
## <a name="see-also"></a>另请参阅  
 [updateAsciiStream 方法 (SQLServerResultSet)](../../../connect/jdbc/reference/updateasciistream-method-sqlserverresultset.md)   
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
