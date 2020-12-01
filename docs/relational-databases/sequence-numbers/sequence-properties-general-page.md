---
description: 序列属性（“常规”页）
title: 序列属性（“常规”页）| Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.sequence.general.f1
ms.assetid: 0187f413-cdf0-48a2-b2e6-9b3578cd5811
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: c85265a8461fae5060ac7c8cdbffaf38ce8e9ee3
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88325851"
---
# <a name="sequence-properties-general-page"></a>序列属性（“常规”页）
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]
  创建一个序列对象并指定其属性。 序列是用户定义的绑定到架构的对象，该对象可根据创建序列所依据的规范来生成数值序列。 这组数值以定义的间隔按升序或降序生成，并且可配置为用尽时重新启动（循环）。 序列不与特定表相关联，这一点与标识列不同。 应用程序将引用某一序列对象以便检索其下一个值。 序列与表之间的关系由应用程序控制。 用户应用程序可以引用一个序列对象，并跨多个行和表协调值。  
  
 与在插入时生成的标识列值不同，应用程序可以获得下一个序列号，而不必通过调用 [NEXT VALUE FOR 函数](../../t-sql/functions/next-value-for-transact-sql.md)来插入行。 使用 [sp_sequence_get_range](../../relational-databases/system-stored-procedures/sp-sequence-get-range-transact-sql.md) 同时获取多个序列号。  
  
 有关同时使用 **CREATE SEQUENCE** 和 **NEXT VALUE FOR** 函数的信息，请参阅 [序列号](../../relational-databases/sequence-numbers/sequence-numbers.md)。  
  
 访问此页的方法有如下两种：在对象资源管理器中右键单击“序列”，再单击“新建序列”，或者右键单击现有序列，再单击“属性”。 如果右键单击现有序列，再单击“属性”，则以下某些选项是不可编辑的。 要更改序列选项，请使用 [ALTER SEQUENCE (Transact-SQL)](../../t-sql/statements/alter-sequence-transact-sql.md) 语句，或删除并重新创建序列对象。  
  
## <a name="options"></a>选项  
 **序列名称**  
 在此处输入序列名称。  
  
 **序列架构**  
 指定将拥有此序列的架构。  
  
 **Data type**  
 序列可定义为任何整数类型。 这包括：  
  
|数据类型|范围|  
|---------------|-----------|  
|**tinyint**|0 到 255|  
|**smallint**|-32,768 到 32,767|  
|**int**|-2,147,483,648 到 2,147,483,647|  
|**bigint**|-9,223,372,036,854,775,808 到 9,223,372,036,854,775,807|  
  
-   **decimal** 或 **numeric** ，小数位数为 0。  
  
-   基于这些类型之一的任何用户定义数据类型（别名类型）。  
  
 **精度**  
 对于“十进制”**decimal** 或“数值”数据类型，请指定精度。 （小数位数始终为 0。）  
  
 **起始值**  
 将由序列对象返回的第一个值。 **START** 值必须是小于或等于序列对象的最大值并大于或等于其最小值的值。 新序列对象的默认起始值是升序序列对象的最小值和降序序列对象的最大值。  
  
 **增量**  
 用于在每次调用 **NEXT VALUE FOR** 函数时递增（如果为负数，则为递减）序列对象的值的值。 如果增量是负值，则序列对象为降序，否则为升序。 增量不能为 0。  
  
 **最小值**  
 指定序列对象的边界。 一个新序列对象的默认最小值是该序列对象的数据类型的最小值。 对于 **tinyint** 数据类型，此值为零，对于所有其他数据类型则为负数。  
  
 **最大值**  
 指定序列对象的边界。 一个新序列对象的默认最大值是该序列对象的数据类型的最大值。  
  
 **在达到限制时，循环序列将重新开始**  
 选择此选项，则当超过序列对象的最小值或最大值时，允许序列对象从最小值（或对于降序顺序对象，则为最大值）重新开始。  
  
> [!NOTE]  
>  循环不从起始值重新开始，而是从最小值/最大值重新开始。  
  
 **高速缓存选项**  
 创建序列值的缓存可以通过最大限度地减少创建序列号所需的磁盘 IO 数来提高使用序列对象的应用程序性能。  
  
-   默认缓存大小 - [!INCLUDE[ssDE](../../includes/ssde-md.md)] 将选择一个大小，然而，用户不应依赖于所选择的大小来确保一致。 [!INCLUDE[msCoName](../../includes/msconame-md.md)] 可能会更改计算缓存大小的方法而不另行通知。  
  
-   无缓存 - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将不缓存序列号。  
  
-   缓存大小 - [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将缓存序列值。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 将保持跟踪当前值和缓存中留下的值数量。 因此，存储缓存所需的内存量始终为序列对象的数据类型的两个实例。  
  
 当使用 CACHE 选项创建时，意外关机（如电源故障）可能导致缓存中的序列号丢失。  
  
 有关创建序列选项的其他信息，请参阅 [CREATE SEQUENCE (Transact-SQL)](../../t-sql/statements/create-sequence-transact-sql.md)。  
  
## <a name="permissions"></a>权限  
 要求对 SCHEMA 拥有 **CREATE SEQUENCE**、 **ALTER** 或 **CONTROL** 权限。  
  
## <a name="see-also"></a>另请参阅  
 [sys.sequences (Transact-SQL)](../../relational-databases/system-catalog-views/sys-sequences-transact-sql.md)  
  
  
