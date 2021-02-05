---
description: SQLServerParameterMetaData 类
title: SQLServerParameterMetaData 类 | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
ms.assetid: 546290e0-9411-4a2b-aa36-61251e70e9cf
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: cac82c9b917f4357f35f8e5bff2ea661f0a50ee3
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99178140"
---
# <a name="sqlserverparametermetadata-class"></a>SQLServerParameterMetaData 类
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  表示用于预定义语句参数的元数据。  
  
 **包：** com.microsoft.sqlserver.jdbc  
  
 **扩展：** java.lang.Object  
  
 **实现：** java.sql.ParameterMetaData  
  
## <a name="syntax"></a>语法  
  
```  
  
public class SQLServerParameterMetaData  
```  
  
## <a name="remarks"></a>备注  
 若要检索参数元数据，请使用 SET FMT ONLY 运行准备好的语句。 可调用的语句调用 sp_sproc_columns 来为过程参数检索名称和元数据。  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerParameterMetaData 成员](../../../connect/jdbc/reference/sqlserverparametermetadata-members.md)   
 [JDBC 驱动程序 API 参考](../../../connect/jdbc/reference/jdbc-driver-api-reference.md)  
  
  
