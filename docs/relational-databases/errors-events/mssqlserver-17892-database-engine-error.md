---
description: MSSQLSERVER_17892
title: MSSQLSERVER_17892
ms.custom: ''
ms.date: 08/20/2020
ms.prod: sql
ms.reviewer: ramakoni1, pijocoder, suresh-kandoth, Masha
ms.technology: supportability
ms.topic: language-reference
helpviewer_keywords:
- 17892 (Database Engine error)
ms.assetid: ''
author: suresh-kandoth
ms.author: ramakoni
ms.openlocfilehash: 905b961e2fbf882f59b050a3acb7ba0f9c2f9046
ms.sourcegitcommit: a9e982e30e458866fcd64374e3458516182d604c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/11/2021
ms.locfileid: "98099309"
---
# <a name="mssqlserver_17892"></a>MSSQLSERVER_17892
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]

## <a name="details"></a>详细信息

|Attribute|值|
|---|---|
|产品名称|SQL Server|
|事件 ID|17892|
|事件源|MSSQLSERVER|
|组件|SQLEngine|
|符号名称|SRV_LOGON_FAILED_BY_TRIGGER|
|消息正文|由于执行触发器，登录名 \<Login Name> 的登录失败。|
||

## <a name="explanation"></a>说明

登录触发器代码无法成功执行时，会引发错误 17892。 [登录触发器](../triggers/logon-triggers.md)将为响应 LOGON 事件而激发存储过程。 与 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]实例建立用户会话时将引发此事件。 将向用户报告如下错误消息：

> 消息 17892，级别 14，状态 1，服务器 \<Server Name>，行 1  
由于执行触发器，登录名 \<Login Name> 的登录失败。

## <a name="possible-causes"></a>可能的原因

如果对该特定用户帐户执行触发器代码时存在错误，则可能出现此问题。 其中一些情况包括：

- 触发器尝试将数据插入到不存在的表中。
- 登录名对登录触发器引用的对象没有权限。

## <a name="user-action"></a>用户操作

可根据所面临的场景使用以下解决方案之一。

- **场景 1**：你目前可使用管理员帐户访问打开的指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的会话

  在这种情况下，你可采取修复触发器代码所需的纠正措施。

  - 示例 1：如果触发器代码引用的对象不存在，则创建该对象，以便登录触发器可成功执行。

  - 示例 2：如果触发器代码引用的对象的确存在，但用户没有相应权限，请向他们授予访问该对象所需的权限。  
  
  或者，你可只删除或禁用登录触发器，使用户能够继续登录到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。  

- **场景 2**：你当前没有任何会话在管理员权限下打开，但 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上已启用专用管理员连接 (DAC)。

    在这种情况下，由于 DAC 连接不受登录触发器的影响，因此可使用 DAC 连接来执行在案例 1 中讨论的相同步骤。 有关 DAC 连接的详细信息，请参阅：[用于数据库管理员的诊断连接](../../database-engine/configure-windows/diagnostic-connection-for-database-administrators.md)。

    若要确定 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 上是否已启用 DAC，可查看 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 错误日志中是否有如下所示的消息：

    > 2020-02-09 16:17:44.150 已建立服务器专用管理员连接支持，以在端口 1434 进行本地侦听。  

- **场景 3**：你既没有在服务器上启用 DAC，当前也没有指向 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 的管理员会话。

    在这种情况下，只能执行以下步骤来解决此问题：
  
    1. 停止 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 及相关服务。
    2. 使用启动参数 `-c`、`-m` 和 `-f` 从[命名提示符](/previous-versions/sql/sql-server-2008-r2/ms180965(v=sql.105))启动 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]。 这样做将禁用登录触发器，并使你能够执行上述案例 1 中讨论的补救措施。
  
        > [!NOTE]
        > 上述过程需要 SA 或等效的管理员帐户。
  
         若要详细了解这些内容和其他启动选项，请参阅：[数据库引擎服务启动选项](../../database-engine/configure-windows/database-engine-service-startup-options.md)。

## <a name="more-information"></a>详细信息

在使用 `EVENTDATA` 函数时，也会出现登录触发器失败的情况。 此函数会返回 XML 并区分大小写。  因此，你需要创建以下登录触发器来根据 IP 地址阻止访问，你可能会遇到以下问题：

``` sql
 CREATE TRIGGER tr_logon_CheckIP  
 ON ALL SERVER  
 FOR LOGON  
 AS
 BEGIN
  IF IS_SRVROLEMEMBER ( 'sysadmin' ) = 1  
     BEGIN
         DECLARE @IP NVARCHAR ( 15 );  
         SET @IP = ( SELECT EVENTDATA ().value ( '(/EVENT_INSTANCE/ClientHost)[1]' , 'NVARCHAR(15)' ));  
         IF NOT EXISTS( SELECT IP FROM DBAWork.dbo.ValidIP WHERE IP = @IP )  
         ROLLBACK ;  
     END ;  
 END ;  
 GO
```

用户通过 Internet 在此部分的触发器上复制此脚本时，未保留大小写格式：

```sql
 SELECT EVENTDATA ().value ( '(/event_instance/clienthost)[1]' , 'NVARCHAR(15)' ));  
```

因此，`EVENTDATA` 始终返回 NULL，且其所有 SA 等效登录访问都会被拒绝。 在这种情况下，不启用 DAC 连接，因此我们只能通过上面列出的启动参数重启服务器来删除触发器。