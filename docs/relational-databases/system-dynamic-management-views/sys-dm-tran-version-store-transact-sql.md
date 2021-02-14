---
description: sys.dm_tran_version_store (Transact-SQL)
title: sys.dm_tran_version_store (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/30/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: system-objects
ms.topic: reference
f1_keywords:
- sys.dm_tran_version_store_TSQL
- sys.dm_tran_version_store
- dm_tran_version_store
- dm_tran_version_store_TSQL
dev_langs:
- TSQL
helpviewer_keywords:
- sys.dm_tran_version_store dynamic management view
ms.assetid: 7ab44517-0351-4f91-bdd9-7cf940f03c51
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 83b1b28ad73a3fb3c8535f297f34fe9f95936e61
ms.sourcegitcommit: 8dc7e0ececf15f3438c05ef2c9daccaac1bbff78
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/13/2021
ms.locfileid: "100347983"
---
# <a name="sysdm_tran_version_store-transact-sql"></a>sys.dm_tran_version_store (Transact-SQL)
[!INCLUDE [SQL Server SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  返回一个虚拟表，其中显示有版本存储区中的所有版本记录。 **sys.dm_tran_version_store** 的运行效率较低，因为它会查询整个版本存储区，版本存储区可能会非常大。  
  
 每个有版本控制的记录均以二进制数据的形式与某些跟踪或状态信息存储在一起。 与数据库表中的记录相似，版本存储区记录存储在 8192 字节的页中。 如果记录超过 8192 字节，则该记录将拆分为两个不同的记录。  
  
 由于有版本控制的记录以二进制数据的形式存储，因此不同的数据库可以采用不同的排序规则。 使用 **sys.dm_tran_version_store** 可以查找二进制表示形式中的行的以前版本，因为它们存在于版本存储区中。  
  
  
## <a name="syntax"></a>语法  
  
```  
sys.dm_tran_version_store  
```  
  
## <a name="table-returned"></a>返回的表  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**transaction_sequence_num**|**bigint**|生成该记录版本的事务的序列号。|  
|**version_sequence_num**|**bigint**|版本记录序列号。 此值在生成事务的版本中是唯一的。|  
|database_id|**int**|有版本控制的记录的数据库 ID。|  
|**rowset_id**|**bigint**|记录的行集 ID。|  
|**status**|**tinyint**|指示有版本控制的记录是否已拆分为两个记录。 如果此值为 0，则记录存储在一页中。 如果此值为 1，则记录拆分为两个记录，且存储在两个不同页上。|  
|**min_length_in_bytes**|**smallint**|记录的最小长度（字节）。|  
|**record_length_first_part_in_bytes**|**smallint**|有版本控制的记录的第一部分的长度（字节）。|  
|**record_image_first_part**|varbinary(8000)|版本记录的第一部分的二进制图像。|  
|**record_length_second_part_in_bytes**|**smallint**|版本记录的第二部分的长度（字节）。|  
|**record_image_second_part**|varbinary(8000)|版本记录的第二部分的二进制图像。|  
  
## <a name="permissions"></a>权限

在上 [!INCLUDE[ssNoVersion_md](../../includes/ssnoversion-md.md)] ，需要 `VIEW SERVER STATE` 权限。   
在 SQL 数据库的基本、S0 和 S1 服务目标上，对于弹性池中的数据库， [服务器管理员](https://docs.microsoft.com/azure/azure-sql/database/logins-create-manage#existing-logins-and-user-accounts-after-creating-a-new-database) 帐户或 [Azure Active Directory 管理员](https://docs.microsoft.com/azure/azure-sql/database/authentication-aad-overview#administrator-structure) 帐户是必需的。 对于所有其他 SQL 数据库服务目标， `VIEW DATABASE STATE` 数据库中需要该权限。   
  
## <a name="examples"></a>示例  
 下面的示例使用具有四个并发事务的测试方案，每一个事务都由事务序列号 (XSN) 标识，并在 ALLOW_SNAPSHOT_ISOLATION 和 READ_COMMITTED_SNAPSHOT 选项设置为 ON 的数据库中运行。 下列事务正在运行：  
  
-   XSN-57 是序列化隔离下的更新操作。  
  
-   XSN-58 与 XSN-57 相同。  
  
-   XSN-59 是快照隔离下的选择操作。  
  
-   XSN-60 与 XSN-59 相同。  
  
 执行以下查询。  
  
```  
SELECT  
    transaction_sequence_num,  
    version_sequence_num,  
    database_id rowset_id,  
    status,  
    min_length_in_bytes,  
    record_length_first_part_in_bytes,  
    record_image_first_part,  
    record_length_second_part_in_bytes,  
    record_image_second_part  
  FROM sys.dm_tran_version_store;  
```  
  
 [!INCLUDE[ssResult](../../includes/ssresult-md.md)]  
  
```  
transaction_sequence_num version_sequence_num database_id  
------------------------ -------------------- -----------  
57                      1                    9             
57                      2                    9             
57                      3                    9             
58                      1                    9             
  
rowset_id            status min_length_in_bytes  
-------------------- ------ -------------------  
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038321152    0      12                   
72057594038386688    0      16                   
  
record_length_first_part_in_bytes  
---------------------------------  
29                                 
29                                 
29                                 
33                                 
  
record_image_first_part                                               
--------------------------------------------------------------------  
0x50000C0073000000010000000200FCB000000001000000270000000000          
0x50000C0073000000020000000200FCB000000001000100270000000000          
0x50000C0073000000030000000200FCB000000001000200270000000000          
0x500010000100000002000000030000000300F800000000000000002E0000000000  
  
record_length_second_part_in_bytes record_image_second_part  
---------------------------------- ------------------------  
0                                  NULL  
0                                  NULL  
0                                  NULL  
0                                  NULL  
```  
  
 输出显示 XSN-57 从一个表创建了三个行版本，XSN-58 从另一个表创建了一个行版本。  
  
## <a name="see-also"></a>另请参阅  
 [动态管理视图和函数 (Transact-SQL)](~/relational-databases/system-dynamic-management-views/system-dynamic-management-views.md)   
 [与事务相关的动态管理视图和函数 (Transact-SQL)](../../relational-databases/system-dynamic-management-views/transaction-related-dynamic-management-views-and-functions-transact-sql.md)  
  
  
