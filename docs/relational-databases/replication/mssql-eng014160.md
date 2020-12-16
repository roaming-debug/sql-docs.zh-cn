---
description: MSSQL_ENG014160
title: MSSQL_ENG014160 | Microsoft Docs
ms.custom: ''
ms.date: 03/04/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: replication
ms.topic: conceptual
helpviewer_keywords:
- MSSQL_ENG014160 error
ms.assetid: d0f3855e-d095-4a81-a5bd-9d7ad51f2c77
author: MashaMSFT
ms.author: mathoma
monikerRange: =azuresqldb-mi-current||>=sql-server-2016
ms.openlocfilehash: e93ae1cc65b89a38b169926725947906e4656d46
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97473418"
---
# <a name="mssql_eng014160"></a>MSSQL_ENG014160
[!INCLUDE [SQL Server SQL MI](../../includes/applies-to-version/sql-asdbmi.md)]
    
## <a name="message-details"></a>消息详细信息  
  
|Attribute|值|  
|-|-|  
|产品名称|SQL Server|  
|事件 ID|14160|  
|事件源|MSSQLSERVER|  
|组件|[!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)]|  
|符号名称||  
|消息正文|已设置发布 [%s] 的阈值 [%s:%s]。 此发布的一个或多个订阅已过期。|  
  
## <a name="explanation"></a>说明  
 使用复制可以对一些情况启用警告。 例如，订阅即将过期时，即可以发出此警告。 如果在指定的“保持期” 内未同步订阅，则订阅会过期。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
 使用复制监视器或 [sp_replmonitorchangepublicationthreshold](../../relational-databases/system-stored-procedures/sp-replmonitorchangepublicationthreshold-transact-sql.md)启用警告时，请指定阈值以确定何时触发警告。 达到或超过该阈值时，复制监视器中将显示警告，并且将一个事件写入 Windows 事件日志。 达到阈值还会触发 SQL Server 代理警报。 有关详细信息，请参阅[在复制监视器中设置阈值和警告](../../relational-databases/replication/monitor/set-thresholds-and-warnings-in-replication-monitor.md)和[以编程方式监视复制](../../relational-databases/replication/monitor/programmatically-monitor-replication.md)。  
  
## <a name="user-action"></a>用户操作  
 对此问题的解决依赖于引起警告的原因：  
  
-   如果已超过阈值，但订阅尚未过期，则同步订阅。 有关详细信息，请参阅 [同步数据](../../relational-databases/replication/synchronize-data.md)。  
  
-   如果代理已经运行，但尚未正确复制更改，则会导致订阅过期。 对于事务复制，请确保分发代理和日志读取器代理正在运行。 对于合并复制，请确保合并代理正在运行。 有关如何启动这些代理的信息，请参阅[启动和停止复制代理 (SQL Server Management Studio)](../../relational-databases/replication/agents/start-and-stop-a-replication-agent-sql-server-management-studio.md) 和[复制代理可执行文件概念](../../relational-databases/replication/concepts/replication-agent-executables-concepts.md)。  
  
-   如果订阅已过期，则必须重新初始化订阅或删除然后重新创建订阅，具体取决于订阅的类型和已过期多长时间。 有关详细信息，请参阅 [Subscription Expiration and Deactivation](../../relational-databases/replication/subscription-expiration-and-deactivation.md)。  
  
## <a name="see-also"></a>另请参阅  
 [错误和事件参考（复制）](../../relational-databases/replication/errors-and-events-reference-replication.md)  
  
  
