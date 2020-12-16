---
description: 错误和事件参考（复制）
title: 错误和事件参考（复制）| Microsoft Docs
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- errors [SQL Server replication], troubleshooting
- troubleshooting [SQL Server replication], errors
- replication [SQL Server], troubleshooting
- errors [SQL Server replication]
- errors and events reference [SQL Server replication]
ms.assetid: e67d1bab-47b6-441d-ab9c-251a2ca499e1
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: 48d6556c834fa7e8505df3a328e934985f3e77b3
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97464798"
---
# <a name="errors-and-events-reference-replication"></a>错误和事件参考（复制）
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
  本部分文档包含与复制相关的多个错误的原因和解决方法的信息。  
  
|错误|消息|  
|-----------|-------------|  
|[MSSQL_ENG002601](../../relational-databases/replication/mssql-eng002601.md)|不能在具有唯一索引 '%.\*ls' 的对象 '%.*ls' 中插入重复键的行。|  
|[MSSQL_ENG002627](../../relational-databases/replication/mssql-eng002627.md)|违反了 %ls 约束 '%.*ls'。 不能在对象 '%.\*ls' 中插入重复键。|  
|[MSSQL_ENG003165](../../relational-databases/replication/mssql-eng003165.md)|数据库 '%ls' 已还原，但在还原/删除复制时出错。 该数据库仍保留为脱机状态。 请参阅 SQL Server 联机丛书中的主题 MSSQL_ENG003165。|  
|[MSSQL_ENG003724](../../relational-databases/replication/mssql-eng003724.md)|无法对 %S_MSG '%.*ls' 执行 %S_MSG，因为它正用于复制。|  
|[MSSQL_ENG004929](../../relational-databases/replication/mssql-eng004929.md)|无法更改 %S_MSG '%.*ls'，因为正在为复制而发布它。|  
|MSSQL_ENG007395。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|无法启动链接服务器“%ls”的 OLE DB 访问接口“%ls”的嵌套事务。 由于 XACT_ABORT 选项已设置为 OFF，因此必须使用嵌套事务。|  
|[MSSQL_ENG014005](../../relational-databases/replication/mssql-eng014005.md)|无法删除发布。 该发布已有订阅。|  
|[MSSQL_ENG014010](../../relational-databases/replication/mssql-eng014010.md)|未将服务器“%s”定义为订阅服务器。|  
|[MSSQL_ENG014114](../../relational-databases/replication/mssql-eng014114.md)|未将 '%s' 配置为分发服务器。|  
|[MSSQL_ENG014117](../../relational-databases/replication/mssql-eng014117.md)|未将 '%s' 配置为分发数据库。|  
|[MSSQL_ENG014120](../../relational-databases/replication/mssql-eng014120.md)|无法删除分发数据库 '%s'。 此分发服务器数据库与发布服务器相关联。|  
|[MSSQL_ENG014121](../../relational-databases/replication/mssql-eng014121.md)|无法删除分发服务器 '%s'。 此分发服务器与分发数据库相关联。|  
|[MSSQL_ENG014144](../../relational-databases/replication/mssql-eng014144.md)|无法删除订阅服务器 '%s'。 在发布数据库 '%s' 中已有此服务器的订阅。|  
|[MSSQL_ENG014150](../../relational-databases/replication/mssql-eng014150.md)|复制 - %s：代理 %s 成功。 %s|  
|[MSSQL_ENG014151](../../relational-databases/replication/mssql-eng014151.md)|复制 - %s: 代理 %s 失败。 %s|  
|[MSSQL_ENG014152](../../relational-databases/replication/mssql-eng014152.md)|复制 - %s：代理 %s 计划重试。 %s|  
|[MSSQL_ENG014157](../../relational-databases/replication/mssql-eng014157.md)|由发布 '%s' 的订阅服务器 '%s' 创建的订阅已过期，且已停止。|  
|[MSSQL_ENG014160](../../relational-databases/replication/mssql-eng014160.md)|已设置发布 [%s] 的阈值 [%s:%s]。 此发布的一个或多个订阅已过期。|  
|[MSSQL_ENG014161](../../relational-databases/replication/mssql-eng014161.md)|已设置发布 [%s] 的阈值 [%s:%s]。 请确保日志读取器和分发代理正在运行并且可以满足滞后时间要求。|  
|[MSSQL_ENG014162](../../relational-databases/replication/mssql-eng014162.md)|已设置发布 [%s] 的阈值 [%s:%s]。 请确保合并代理正在运行且符合要求。|  
|[MSSQL_ENG014163](../../relational-databases/replication/mssql-eng014163.md)|已设置发布 [%s] 的阈值 [%s:%s]。 请确保合并代理正在运行且符合要求。|  
|[MSSQL_ENG014164](../../relational-databases/replication/mssql-eng014164.md)|已设置发布 [%s] 的阈值 [%s:%s]。 请确保合并代理正在运行且符合要求。|  
|[MSSQL_ENG014165](../../relational-databases/replication/mssql-eng014165.md)|已设置发布 [%s] 的阈值 [%s:%s]。 请确保合并代理正在运行且符合要求。|  
|[MSSQL_ENG018456](../../relational-databases/replication/mssql-eng018456.md)|用户 '%.*ls'.%.\*ls 登录失败|  
|[MSSQL_ENG018752](../../relational-databases/replication/mssql-eng018752.md)|一次只能有一个日志读取器代理或日志相关过程(sp_repldone、sp_replcmds 和 sp_replshowcmds)连接到某个数据库。 如果执行了一个日志相关过程，那么在启动日志读取器代理或者执行另一个日志相关过程之前，请删除执行第一个过程时所用的连接，或者在该连接上执行 sp_replflush。|  
|[MSSQL_ENG020554](../../relational-databases/replication/mssql-eng020554.md)|复制代理在 %ld 分钟内没有记录任何进度消息。 这表明代理已停止响应或系统活动过多。 请确保正在将记录复制到目标，并且与订阅服务器、发布服务器和分发服务器的连接仍然是活动的。|  
|[MSSQL_ENG020557](../../relational-databases/replication/mssql-eng020557.md)|代理关闭。 有关详细信息，请参阅作业 '%s' 的 SQL Server 代理作业历史记录。|  
|[MSSQL_ENG020572](../../relational-databases/replication/mssql-eng020572.md)|在验证失败之后，订阅服务器“%s”对发布“%s”中项目“%s”的订阅已被重新初始化。|  
|[MSSQL_ENG020574](../../relational-databases/replication/mssql-eng020574.md)|订阅服务器“%s”对发布“%s”中项目“%s”的订阅未通过数据验证。|  
|[MSSQL_ENG020575](../../relational-databases/replication/mssql-eng020575.md)|订阅服务器“%s”对发布“%s”中项目“%s”的订阅已通过数据验证。|  
|[MSSQL_ENG020596](../../relational-databases/replication/mssql-eng020596.md)|只有 '%s' 或 db_owner 的成员可以删除匿名代理。|  
|[MSSQL_ENG020598](../../relational-databases/replication/mssql-eng020598.md)|应用复制的命令时在订阅服务器上找不到该行。|  
|[MSSQL_ENG021075](../../relational-databases/replication/mssql-eng021075.md)|发布 '%s' 的初始快照尚不可用。|  
|[MSSQL_ENG021076](../../relational-databases/replication/mssql-eng021076.md)|项目 '%s' 的初始快照尚不可用。|  
|[MSSQL_ENG021286](../../relational-databases/replication/mssql-eng021286.md)|冲突表 '%s' 不存在。|  
|[MSSQL_ENG021330](../../relational-databases/replication/mssql-eng021330.md)|无法在复制工作目录下创建子目录。(%ls)|  
|[MSSQL_ENG021331](../../relational-databases/replication/mssql-eng021331.md)|无法将用户脚本文件复制到分发服务器。(%ls)|  
|[MSSQL_ENG021385](../../relational-databases/replication/mssql-eng021385.md)|快照无法处理发布 '%s'。 可能是由于活动架构的更改操作或者是所添加的新项目所致。|  
|MSSQL_ENG021617。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|无法运行 SQL*PLUS。 请确保分发服务器上安装了最新版本的 Oracle 客户端代码。|  
|MSSQL_ENG021620。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|通过系统 Path 变量获得的 SQL*PLUS 版本不够新，无法支持 Oracle 发布。 请确保分发服务器上安装了最新版本的 Oracle 客户端代码。|  
|MSSQL_ENG021624。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|在分发服务器 '%s' 上找不到已注册的 Oracle OLEDB 访问接口 OraOLEDB.Oracle。 请确保分发服务器上安装并注册了最新版本的 Oracle OLEDB 访问接口。|  
|MSSQL_ENG021626。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|无法使用 Oracle OLEDB 访问接口 OraOLEDB.Oracle 连接到 Oracle 数据库服务器 '%s'。|  
|MSSQL_ENG021627。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|无法使用 Microsoft OLEDB 访问接口 MSDAORA 连接到 Oracle 数据库服务器“%s”。|  
|MSSQL_ENG021628。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|无法更新分发服务器 '%s' 的注册表，以允许 Oracle OLEDB 访问接口 OraOLEDB.Oracle 与 SQL Server 一起在进程中运行。 请确保当前登录名有权修改 SQL Server 拥有的注册表项。|  
|MSSQL_ENG021629。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|指示 Oracle 的 Oracle OLEDB 访问接口 OraOLEDB.Oracle 已注册的 CLSID 注册表项不在分发服务器上。 请确保分发服务器上安装并注册了 Oracle OLEDB 访问接口。|  
|MSSQL_ENG021642。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|异类发布服务器需要链接服务器。 已有一个名为 '%s' 的链接服务器。 请删除链接服务器或另选一个发布服务器名称。|  
|MSSQL_ENG021663。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|找不到源表 [%s].[%s] 的有效主键。|  
|MSSQL_ENG021684。 请参阅 [Troubleshooting Oracle Publishers](../../relational-databases/replication/non-sql/troubleshooting-oracle-publishers.md)。|与 Oracle 发布服务器‘%s’的管理员登录名关联的权限不足。|  
|[MSSQL_ENG021797](../../relational-databases/replication/mssql-eng021797.md)|%s 必须是以下格式的有效 Windows 登录信息:'MACHINE\Login' 或 'DOMAIN\Login'。 请参阅 '%s' 的文档。|  
|[MSSQL_ENG021798](../../relational-databases/replication/mssql-eng021798.md)|在继续操作之前，必须通过“%s”添加“%s”代理作业。 请参阅 '%s' 的文档。|  
|[MSSQL_REPL020011](../../relational-databases/replication/mssql-repl020011.md)|进程无法在“%2”上执行“%1”。|  
|[MSSQL_REPL027056](../../relational-databases/replication/mssql-repl027056.md)|合并进程无法更改“%1”上的生成历史记录。 进行故障排除时，请使用详细的历史日志记录来重新启动同步，并指定要写入的输出文件。|  
|[MSSQL_REPL027183](../../relational-databases/replication/mssql-repl027183.md)|合并进程未能使用参数化的行筛选器来枚举项目中的更改。 如果此操作仍失败，请增大该进程的查询超时值，缩短发布的保持期，并改进对已发布表的索引。|  
  
  
