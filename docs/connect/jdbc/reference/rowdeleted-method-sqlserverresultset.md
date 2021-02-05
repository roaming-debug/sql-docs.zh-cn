---
description: rowDeleted 方法 (SQLServerResultSet)
title: rowDeleted 方法 (SQLServerResultSet) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerResultSet.rowDeleted
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 9c6db315-e614-4604-b020-41af6a214cc1
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 1b3557c1df6b128b69671a3fa6846944e466a28e
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99174142"
---
# <a name="rowdeleted-method-sqlserverresultset"></a>rowDeleted 方法 (SQLServerResultSet)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索是否已删除某行。  
  
## <a name="syntax"></a>语法  
  
```  
  
public boolean rowDeleted()  
```  
  
## <a name="return-value"></a>返回值  
 如果删除了某行并且检测到了删除，则为“true”。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 rowDeleted 方法是由 java.sql.ResultSet 接口中的 rowDeleted 方法指定的。  
  
 已删除的行可能在结果集中留有可见孔。 此方法可用于检测结果集中的孔。 返回的值取决于此 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象能否可以检测到删除。  
  
> [!NOTE]  
>  [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 将针对所有可更新的游标类型检测已删除的行，尽管此检测对于前进和动态游标是暂时的。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerResultSet 成员](../../../connect/jdbc/reference/sqlserverresultset-members.md)   
 [SQLServerResultSet 类](../../../connect/jdbc/reference/sqlserverresultset-class.md)  
  
  
