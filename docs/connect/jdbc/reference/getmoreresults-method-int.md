---
description: getMoreResults 方法 (int)
title: getMoreResults 方法 (int) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerStatement.getMoreResults (int)
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 6419e5a8-8b3a-4d5b-8226-95865c52c723
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: e29acc97b946253c30455ac4877e50de51449acd
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99162731"
---
# <a name="getmoreresults-method-int"></a>getMoreResults 方法 (int)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  移动到此 [SQLServerStatement](../../../connect/jdbc/reference/sqlserverstatement-class.md) 对象的下一个结果并根据给定模式所指定的说明处理任何当前打开的结果集对象。  
  
## <a name="syntax"></a>语法  
  
```  
  
public final boolean getMoreResults(int mode)  
```  
  
#### <a name="parameters"></a>参数  
 *mode*  
  
 指示如何处理当前打开的结果集对象的 int。 必须是下列常量之一：  
  
 CLOSE_CURRENT_RESULT  
  
 KEEP_CURRENT_RESULT（JDBC 驱动程序不支持）  
  
 CLOSE_ALL_RESULTS  
  
## <a name="return-value"></a>返回值  
 如果返回的结果为一个结果集，则为 true。 否则为 **false**。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getMoreResults 方法是由 java.sql.Statement 接口中的 getMoreResults 方法指定的。  
  
 如果在检索结果前调用 getMoreResults 方法，则该方法就会采用 mode 参数指定的行为方式并移动到下一个结果。  
  
> [!NOTE]  
>  JDBC 驱动程序不支持使用 KEEP_CURRENT_RESULT 常量。 如果使用该常量，将引发异常。  
  
## <a name="see-also"></a>另请参阅  
 [getMoreResults 方法 &#40;SQLServerStatement&#41;](../../../connect/jdbc/reference/getmoreresults-method-sqlserverstatement.md)   
 [SQLServerStatement 成员](../../../connect/jdbc/reference/sqlserverstatement-members.md)   
 [SQLServerStatement 类](../../../connect/jdbc/reference/sqlserverstatement-class.md)  
  
  
