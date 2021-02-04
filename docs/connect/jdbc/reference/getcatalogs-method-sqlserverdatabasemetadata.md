---
description: getCatalogs 方法 (SQLServerDatabaseMetaData)
title: getCatalogs 方法 (SQLServerDatabaseMetaData) | Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
apiname:
- SQLServerDatabaseMetaData.getCatalogs
apilocation:
- sqljdbc.jar
apitype: Assembly
ms.assetid: 7f8bd0f1-f340-4bb9-b559-0a6176124033
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 58ed8fc72dfb7d1abc4f6fc2c77b6c9325408dae
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99165553"
---
# <a name="getcatalogs-method-sqlserverdatabasemetadata"></a>getCatalogs 方法 (SQLServerDatabaseMetaData)
[!INCLUDE[Driver_JDBC_Download](../../../includes/driver_jdbc_download.md)]

  检索在连接的服务器中可用的目录名称。  
  
## <a name="syntax"></a>语法  
  
```  
  
public java.sql.ResultSet getCatalogs()  
```  
  
## <a name="return-value"></a>返回值  
 一个 [SQLServerResultSet](../../../connect/jdbc/reference/sqlserverresultset-class.md) 对象。  
  
## <a name="exceptions"></a>例外  
 [SQLServerException](../../../connect/jdbc/reference/sqlserverexception-class.md)  
  
## <a name="remarks"></a>备注  
 此 getCatalogs 方法是由 java.sql.DatabaseMetaData 接口中的 getCatalogs 方法指定。  
  
> [!NOTE]  
>  在 Azure SQL 数据库上，应连接到 master 数据库，以调用 SQLServerDatabaseMetaData.getCatalogs。 SQL 数据库不支持从用户数据库中返回整个目录集。 SQLServerDatabaseMetaData.getCatalogs 使用 sys.databases 视图来获取目录。 若要了解 SQL 上的 SQLServerDatabaseMetaData.getCatalogs 行为，请参阅 [sys.database_usage（Azure SQL 数据库）](../../../relational-databases/system-catalog-views/sys-database-usage-azure-sql-database.md)中关于权限的讨论 > 在 Azure SQL 数据库上，应连接到 master 数据库来调用 SQLServerDatabaseMetaData.getCatalogs。 SQL 数据库不支持从用户数据库中返回整个目录集。 SQLServerDatabaseMetaData.getCatalogs 使用 sys.databases 视图来获取目录。 若要了解 SQL 数据库上的 SQLServerDatabaseMetaData.getCatalogs 行为，请参阅 [sys.database_usage（Azure SQL 数据库）](../../../relational-databases/system-catalog-views/sys-database-usage-azure-sql-database.md)中关于权限的讨论。                      .  
  
 由 getCatalogs 方法返回的结果集将包含以下信息：  
  
|名称|类型|说明|  
|----------|----------|-----------------|  
|TABLE_CAT|**字符串**|目录名称，包括 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中的系统数据库。|  
  
## <a name="example"></a>示例  
 下面的示例展示了如何使用 getCatalogs 方法返回 [!INCLUDE[msCoName](../../../includes/msconame_md.md)] [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] 中包含的所有数据库（包括系统数据库）的名称。  
  
```  
public static void executeGetCatalogs(Connection con) {  
   try {  
      DatabaseMetaData dbmd = con.getMetaData();  
      ResultSet rs = dbmd.getCatalogs();  
      ResultSetMetaData rsmd = rs.getMetaData();  
  
      // Display the result set data.  
      int cols = rsmd.getColumnCount();  
      while(rs.next()) {  
         for (int i = 1; i <= cols; i++) {  
            System.out.println(rs.getString(i));  
         }  
      }  
      rs.close();  
   }   
  
   catch (Exception e) {  
      e.printStackTrace();  
   }  
}  
```  
  
## <a name="see-also"></a>另请参阅  
 [SQLServerDatabaseMetaData 方法](../../../connect/jdbc/reference/sqlserverdatabasemetadata-methods.md)   
 [SQLServerDatabaseMetaData 成员](../../../connect/jdbc/reference/sqlserverdatabasemetadata-members.md)   
 [SQLServerDatabaseMetaData 类](../../../connect/jdbc/reference/sqlserverdatabasemetadata-class.md)  
  
  
