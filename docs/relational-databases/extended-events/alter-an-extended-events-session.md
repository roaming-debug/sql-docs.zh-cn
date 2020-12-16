---
title: 更改扩展事件会话
description: 使用 SQL Server 扩展事件向导更改扩展事件会话。 可以做出的更改取决于会话是处于活动状态还是非活动状态。
ms.date: 05/24/2019
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.technology: xevents
ms.topic: tutorial
ms.assetid: 114ec05b-7eca-4c87-b276-25e37b84be39
author: MightyPen
ms.author: genemi
monikerRange: =azuresqldb-current||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current
ms.openlocfilehash: 8ad23756f19dd2305215ed7e44f52e65f5e52f74
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97465598"
---
# <a name="alter-an-extended-events-session"></a>更改扩展事件会话

[!INCLUDE [SQL Server Azure SQL Database](../../includes/applies-to-version/sql-asdb.md)]

  在创建扩展事件会话后，可以使用 **“SQL Server 扩展事件向导”** 根据需要更改它。  
  
## <a name="before-you-begin"></a>开始之前  
 不能更改活动和不活动会话的目标，不能更改活动会话的高级属性配置。  
  
 可以对活动和不活动事件会话进行以下更改：  
  
-   在会话中添加或删除事件，更改事件配置（如事件字段、筛选器和操作）。  
  
-   添加或删除事件会话的目标。  
  
-   启用 **“在服务器启动时启动事件会话”** 选项。  
  
 可以对不活动的事件会话进行以下其他更改：  
  
-   启用 **“跟踪事件之间的关系”** 选项。  
  
-   更改高级属性配置。  
  
> [!NOTE]  
>  “SQL Server 扩展事件向导”  不支持事件会话修改。  
  
## <a name="how-to-alter-an-extended-events-session-using-the-sql-server-extended-events-wizard"></a>如何使用 SQL Server 扩展事件向导更改扩展事件会话  
  
-   在对象资源管理器中，依次展开 **“管理”** 、 **“扩展事件”** 和 **“会话”** 。  
  
-   右键单击要更改的会话，然后单击“属性”  。  
  
-   在 **“属性”** 对话框中进行相应的更改，然后单击 **“确定”** 。  
  
## <a name="see-also"></a>另请参阅  
 [ALTER EVENT SESSION (Transact-SQL)](../../t-sql/statements/alter-event-session-transact-sql.md)   
 [使用查询编辑器创建扩展事件会话](quick-start-extended-events-in-sql-server.md)  
  
  
