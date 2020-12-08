---
title: 行压缩的实现 | Microsoft Docs
description: 了解 SQL Server 数据库引擎如何实现行压缩，以帮助计划数据所需的存储空间。
ms.custom: ''
ms.date: 06/30/2016
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: performance
ms.topic: conceptual
helpviewer_keywords:
- compression [SQL Server], row
- row compression [Database Engine]
ms.assetid: dcd97ac1-1c85-4142-9594-9182e62f6832
author: WilliamDAssafMSFT
ms.author: wiassaf
monikerRange: =azuresqldb-current||>=sql-server-2016||=sqlallproducts-allversions||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 829229371dfecd55a56fdbb9a6530635a6170904
ms.sourcegitcommit: 0e0cd9347c029e0c7c9f3fe6d39985a6d3af967d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/02/2020
ms.locfileid: "96506347"
---
# <a name="row-compression-implementation"></a>Row Compression Implementation
[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  本主题概述了 [!INCLUDE[ssDE](../../includes/ssde-md.md)] 是如何实现行压缩的。 此摘要提供了有助于您规划数据所需存储空间的基本信息。  
  
 启用压缩只会更改与数据类型相关联的数据的物理存储格式，而不会更改其语法或语义。 当对一个或多个表启用压缩时，不需要更改应用程序。 新的记录存储格式主要有以下更改：  
  
-   减少了与记录相关联的元数据开销。 此元数据为有关列、列长度和偏移量的信息。 在某些情况下，元数据开销可能大于旧的存储格式。  
  
-   它对于数值类型（例如， **integer**、 **decimal** 和 **float**）和基于数值的类型（例如， **datetime** 和 **money**）使用可变长度存储格式。  
  
-   它通过使用不存储空字符的可变长度格式来存储定长字符串。  
  
> [!NOTE]  
>  将对所有数据类型的 NULL 和 0 值进行优化，从而使它们不占用任何字节。  
  
## <a name="how-row-compression-affects-storage"></a>行压缩影响存储的方式  
 下表介绍了行压缩是如何影响 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 和 [!INCLUDE[ssSDSfull_md](../../includes/sssdsfull-md.md)]中的现有类型的。 此表不包括通过使用页压缩可以节省的空间。  
  
|数据类型|是否影响存储？|说明|  
|---------------|--------------------------|-----------------|  
|**tinyint**|否|1 个字节是所需的最小存储单位。|  
|**smallint**|是|如果值使用 1 个字节即可存储，则将只使用 1 个字节。|  
|**int**|是|只使用所需字节数。 例如，如果值可以用 1 个字节存储，则将只占用 1 个字节的存储空间。|  
|**bigint**|是|只使用所需字节数。 例如，如果值可以用 1 个字节存储，则将只占用 1 个字节的存储空间。|  
|**decimal**|是|此存储与 vardecimal 存储格式完全相同。|  
|**numeric**|是|此存储与 vardecimal 存储格式完全相同。|  
|**bit**|是|元数据开销使此类型达到 4 个位。|  
|**smallmoney**|是|使用 4 字节整数表示整数数据。 货币值乘以 10000，并在存储生成的整数值时删除小数点之后的所有位数。 此类型具有类似于整数类型存储优化的存储优化。|  
|**money**|是|使用 8 字节整数表示整数数据。 货币值乘以 10000，并在存储生成的整数值时删除小数点之后的所有位数。 此类型的范围比 **smallmoney** 更大。 此类型具有类似于整数类型存储优化的存储优化。|  
|**float**|是|不存储带零的最低有效字节。 **float** 压缩主要适用于尾数中的非小数值。|  
|**real**|是|不存储带零的最低有效字节。 **real** 压缩主要适用于尾数中的非小数值。|  
|**smalldatetime**|否|使用两个 2 字节整数表示整数数据。 日期占用 2 个字节。 它是自 1901 年 1 月 1 日以来经过的天数。 从 1902 年起便需要 2 个字节。 因此，自该时间之后的日期不会节省任何空间。<br /><br /> 时间是自午夜以来经过的分钟数。 超过 4AM 后，时间值就开始使用第二个字节。<br /><br /> 如果 **smalldatetime** 只用于表示日期（常见情况），则时间为 0.0。 通过为行压缩以最高有效字节格式存储时间，压缩操作可节省 2 个字节。|  
|**datetime**|是|使用两个 4 字节整数表示整数数据。 此整数值表示自 1900 年 1 月 1 日以来经过的天数。 前 2 个字节最高可以表示 2079 年。 在 2079 年之前，此类压缩始终可以节省 2 个字节。 每个整数值表示 3.33 毫秒。 压缩在前五分钟只占用前 2 个字节，在 4PM 之后将使用第四个字节。 因此，压缩在 4PM 之后只能节省 1 个字节。 当 **datetime** 像任何其他整数一样进行压缩时，压缩可在日期方面节省 2 个字节。|  
|**date**|否|使用 3 个字节表示整数数据。 这表示自 0001 年 1 月 1 日起的日期。 对于当代日期，行压缩使用所有 3 个字节。 这不会节省任何空间。|  
|**time**|否|使用 3 到 6 个字节表示整数数据。 有从 0 到 9 的各种精度，会占用 3 到 6 个字节。 压缩后的空间按如下方式使用：<br /><br /> **精度 = 0。字节数 = 3**。 每个整数值表示一秒。 使用 2 个字节，压缩最长可表示到 6PM，这样可以节省 1 个字节。<br /><br /> **精度 = 1。字节数 = 3**。 每个整数值表示 1/10 秒。 在 2AM 之前，压缩使用第三个字节。 结果是只能节省很少的空间。<br /><br /> **精度 = 2。字节数 = 3**。 与前一情况类似，节省空间的可能性很小。<br /><br /> **精度 = 3。字节数 = 4**。 由于直到 5AM 都将使用前 3 个字节，因而节省的空间很小。<br /><br /> **精度 = 4。字节数 = 4**。 前 27 秒会使用前 3 个字节。 不会节省任何空间。<br /><br /> **精度 = 5，字节数 = 5**。 在中午 12 点之后，将使用第五个字节。<br /><br /> **精度 = 6 和 7，字节数 = 5**。 不能实现任何节省。<br /><br /> **精度 = 8，字节数 = 6**。 在 3AM 之后将使用第六个字节。<br /><br /> <br /><br /> 请注意，进行行压缩时存储不做任何更改。 总体上而言，压缩 **time** 数据类型不会获得很大的空间节省。|  
|**datetime2**|是|使用 6 到 9 个字节表示整数数据。 前 4 个字节表示日期。 时间占用的字节数将取决于指定的时间精度。<br /><br /> 此整数值表示自 0001 年 1 月 1 日以来经过的天数，上限为 9999 年 12 月 31 日。 若要表示 2005 年的日期，压缩操作将使用 3 个字节。<br /><br /> 对于时间则不会节省任何空间，这是因为压缩允许对于各种时间精度使用 2 到 4 个字节。 因此，当时间精度为一秒时，压缩使用 2 个字节来存储时间，在 255 秒之后将使用第二个字节。|  
|**datetimeoffset**|是|类似于 **datetime2**，所不同的是格式为 HH:MM 的时区占用 2 个字节。<br /><br /> 就像 **datetime2**，压缩可以节省 2 个字节。<br /><br /> 对于时区值，在大多数情况下 MM 值都可能是 0。 因此，压缩可能会节省 1 个字节。<br /><br /> 进行行压缩时存储不做任何更改。|  
|**char**|是|将删除尾随填充字符。 请注意，不管使用何种排序规则， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 均将插入相同的填充字符。|  
|**varchar**|否|无效。|  
|**text**|否|无效。|  
|**nchar**|是|将删除尾随填充字符。 请注意，不管使用何种排序规则， [!INCLUDE[ssDE](../../includes/ssde-md.md)] 均将插入相同的填充字符。|  
|**nvarchar**|否|无效。|  
|**ntext**|否|无效。|  
|**binary**|是|将删除尾随的零。|  
|**varbinary**|否|无效。|  
|**图像**|否|无效。|  
|**cursor**|否|无效。|  
|**timestamp** / **rowversion**|是|使用 8 个字节表示整数数据。 每个数据库均维护有一个时间戳计数器，并且它的值从 0 开始。 会像压缩任何其他整数值一样压缩此值。|  
|**sql_variant**|否|无效。|  
|**uniqueidentifier**|否|无效。|  
|**table**|否|无效。|  
|**xml**|否|无效。|  
|用户定义类型|否|这在内部表示为 **varbinary**。|  
|FILESTREAM|否|这在内部表示为 **varbinary**。|  
  
## <a name="see-also"></a>另请参阅  
 [数据压缩](../../relational-databases/data-compression/data-compression.md)   
 [页压缩的实现](../../relational-databases/data-compression/page-compression-implementation.md)  
  
  
