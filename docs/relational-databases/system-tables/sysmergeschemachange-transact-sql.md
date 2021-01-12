---
description: sysmergeschemachange (Transact-SQL)
title: sysmergeschemachange (Transact-sql) |Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: language-reference
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
author: cawrites
ms.author: chadam
ms.openlocfilehash: 2a0315c49710ac324234710d30c223dfb9b27b12
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98101997"
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

  包含有关快照代理生成的已发布项目的信息。 该表存储在发布数据库和订阅数据库中。  
  
|列名称|数据类型|说明|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|发布 ID。|  
|**artid**|**uniqueidentifier**|项目的 ID。|  
|**schemaversion**|**int**|最后一个架构更改的编号。|  
|**schemaguid**|**uniqueidentifier**|最后一个架构的唯一 ID。|  
|**schematype**|**int**|架构更改的类型：<br /><br /> **-1** = 无效。<br /><br /> **1** = SQL 命令。<br /><br /> **2** = 架构脚本。<br /><br /> **3** = 本机大容量复制程序实用工具 (BCP) 。<br /><br /> **4** = 字符 BCP。<br /><br /> **5** = 上一次记录的生成。<br /><br /> **6** = 上次发送的生成。<br /><br /> **7** = 目录。<br /><br /> **8** = 优先级。<br /><br /> **9** = 保持期。<br /><br /> **10** = 触发器脚本。<br /><br /> **11** = Alter table。<br /><br /> **12** = 重新初始化所有。<br /><br /> **13** = ALTER TABLE (非 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]) 。<br /><br /> **14** = 通过上载进行重新初始化。<br /><br /> **15** = 约束和索引脚本。<br /><br /> **16** = 清除元数据。<br /><br /> **17** = 更新上次发送的生成。<br /><br /> **18** = 向后兼容级别。<br /><br /> **19** = 验证订阅服务器信息。<br /><br /> **20** = 分区良好。<br /><br /> **21** = 自定义解析程序。<br /><br /> **22** = 项目处理顺序。<br /><br /> **23** = 在事务发布中发布。<br /><br /> **24** = 补偿错误。<br /><br /> **25** = 备用快照文件夹。<br /><br /> **26** = 仅下载。<br /><br /> **27** = 删除跟踪。<br /><br /> **40** = 预创建快照脚本。<br /><br /> **45** = 创建后的快照脚本。<br /><br /> **46** = 按需用户脚本。<br /><br /> **50** = 快照标头开始。<br /><br /> **51** = 快照标头结束。<br /><br /> **52** = 快照尾部。<br /><br /> **53** = 文件传输协议 (FTP) 地址。<br /><br /> **54** = FTP 端口。<br /><br /> **55** = FTP 子目录。<br /><br /> **56** = 快照已压缩。<br /><br /> **57** = FTP 登录名。<br /><br /> **58** = FTP 密码。<br /><br /> **60** = 系统预创建脚本。<br /><br /> **61** = 存储过程架构。<br /><br /> **62** = 视图架构。<br /><br /> **63** = 用户定义函数架构。<br /><br /> **64** = 视图索引。<br /><br /> **65** = 扩展属性。<br /><br /> **66** = 验证。<br /><br /> **67** = 快照前的 SQL 命令。<br /><br /> **71** = 动态快照验证。<br /><br /> **80** = 系统表本机 BCP 9.0。<br /><br /> **81** = 系统表字符 BCP 9.0。<br /><br /> **82** = 系统表本机 BCP 9.0 (仅限全局) 。<br /><br /> **83** = 系统表字符 BCP 9.0 (仅限全局) 。<br /><br /> **84** = 系统表本机 BCP 9.0 (轻型) 。<br /><br /> **85** = 系统表字符 BCP 9.0 (轻型) 。<br /><br /> **128** = 动态 BCP (位) 。<br /><br /> **131** = 动态本机 BCP。<br /><br /> **132** = 动态字符 BCP。<br /><br /> **208** = 动态系统表本机 BCP 9.0。<br /><br /> **209** = 动态系统表字符 BCP 9.0。<br /><br /> **210** = 动态系统表本机 BCP 9.0 (仅限全局) 。<br /><br /> **211** = 动态系统表字符 BCP 9.0 (仅限全局) 。<br /><br /> **212** = 动态系统表本机 BCP 9.0 (轻型) 。<br /><br /> **213** = 动态系统表字符 BCP 9.0 (轻型) 。<br /><br /> **300** = (DDL) 操作的数据定义语言。<br /><br /> **1024** = 增量快照控件。<br /><br /> **1049** = 增量快照文件夹。<br /><br /> **1074** = 增量快照开始标头。<br /><br /> **1075** = 增量快照结束标头。<br /><br /> **1076** = 增量快照尾端。<br /><br /> **1077** = 增量 FTP 地址。<br /><br /> **1078** = 增量 FTP 端口。<br /><br /> **1079** = 增量 FTP 子目录。<br /><br /> **1080** = 增量压缩快照。<br /><br /> **1081** = 增量 FTP 登录名。<br /><br /> **1082** = 增量 FTP 密码。|  
|**schematext**|**nvarchar (2000)**|脚本文件名，或包含文件名的命令|  
|**schemastatus**|**tinyint**|表示是否为项目挂起架构更改，可以是下列值之一：<br /><br /> **0** = 非活动。<br /><br /> **1** = 活动。<br /><br /> 当架构更改处于挂起状态时，此值设置为 **1**。|  
|**schemasubtype**|**int**|架构更改的子类型：<br /><br /> **1** = ADDCOLUMN<br /><br /> **2** = DROPCOLUMN<br /><br /> **3** = ALTERCOLUMN<br /><br /> **4** = ADDPRIMARYKEY<br /><br /> **5** = ADDUNIQUE<br /><br /> **6** = ADDREFERENCE<br /><br /> **7** = DROPCONSTRAINT<br /><br /> **8** = ADDDEFAULT<br /><br /> **9** = ADDCHECK<br /><br /> **10** = DISABLETRIGGER<br /><br /> **11** = ENABLETRIGGER<br /><br /> **12** = DISABLETRIGGER<br /><br /> **13** = ENABLETRIGGER<br /><br /> **14** = ENABLECONSTRAINT<br /><br /> **15** = DISABLECONSTRAINT<br /><br /> **16** = ENABLECONSTRAINT<br /><br /> **17** = DISABLECONSTRAINT|  
  
## <a name="see-also"></a>另请参阅  
 [Transact-sql&#41;&#40;复制表 ](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [复制视图 (Transact-SQL)](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  
