---
description: 辅助数据库设置
title: 辅助数据库设置 | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
f1_keywords:
- sql13.swb.databaseproperties.logshipping.settings.dest.f1
ms.assetid: f992ffc9-ee42-43fe-acec-512032f0ded1
author: stevestein
ms.author: sstein
ms.openlocfilehash: d6db17f4c7930e03e65412c9addaef5340ec4645
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88471119"
---
# <a name="secondary-database-settings"></a>辅助数据库设置
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用此对话框可以配置和修改日志传送配置中辅助数据库的属性。  
  
 有关日志传送概念的说明，请参阅 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)。  
  
## <a name="options"></a>选项  
 **辅助服务器实例**  
 显示日志传送配置中当前配置为辅助服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例的名称。  
  
 **辅助数据库**  
 显示日志传送配置的辅助数据库名称。 将新的辅助数据库添加到日志传送配置时，可以从列表中选择数据库或在该框中键入新数据库的名称。 如果输入新数据库的名称，则必须在 **“初始化”** 选项卡上选择一个选项，该选项卡可将主数据库的完整数据库备份还原到辅助数据库中。 新数据库将作为还原操作的一部分进行创建。  
  
 **“连接”**  
 连接到日志传送配置中用作辅助服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 实例。 用于连接的帐户必须是辅助服务器实例上 sysadmin 固定服务器角色的成员。  
  
 **“初始化”选项卡**  
 选项如下：  
  
 **是，生成主数据库的完整备份并将其还原到辅助数据库**  
 通过备份主数据库并在辅助服务器上还原该数据库，让 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 配置辅助数据库。 如果在 **“辅助数据库”** 框中输入新的数据库名称，数据库将作为还原操作的一部分进行创建。  
  
 **还原选项**  
 若要将辅助数据库的数据和日志文件还原到辅助服务器上的非默认位置，请单击此按钮。  
  
 单击此按钮将打开 **“还原选项”** 对话框。 在该对话框中，可以指定非默认文件夹的路径，用于驻留辅助数据库及其日志。 如果指定其中的一个文件夹，则必须指定这两个路径。  
  
 这些路径必须引用辅助服务器上的本地驱动器。 另外，这些路径必须以本地驱动器号和冒号开头（例如， `C:`）。 映射的驱动器号或网络路径无效。  
  
 如果单击 **“还原选项”** 按钮后决定使用默认文件夹，建议取消 **“还原选项”** 对话框。 如果已经指定非默认位置，但现在要使用默认位置，请再次单击 **“还原选项”** ，清除文本框，再单击“确定”。  
  
 **是，将主数据库的现有备份还原到辅助数据库**  
 让 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 使用主数据库的现有备份初始化辅助数据库。 在 **“备份文件”** 框中键入该备份的位置。 如果在“辅助数据库”框中输入新的数据库名称，数据库将作为还原操作的一部分进行创建。  
  
 **“备份文件”**  
 如果选择“是，将主数据库的现有备份还原到辅助数据库”选项，请键入要用于初始化辅助数据库的完整数据库备份的路径和文件名。  
  
 **还原选项**  
 参阅本主题前面对此按钮的说明。  
  
 **否，辅助数据库已初始化**  
 指定辅助数据库已初始化并准备接受主数据库的事务日志备份。 如果在 **“辅助数据库”** 框中键入新的数据库名称，则此选项不可用。  
  
 **“复制文件”选项卡**  
 选项如下：  
  
 **复制文件的目标文件夹**  
 键入事务日志备份应复制到的路径以还原到辅助数据库。 通常，此路径为辅助服务器上文件夹的本地路径。 但是，如果该文件夹位于其他服务器，则必须指定该文件夹的 UNC 路径。 辅助服务器实例的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 服务帐户必须具有此文件夹的读取权限。 此外，还必须向代理帐户授予此网络共享的读写权限。通过代理帐户，复制作业和还原作业将在辅助服务器实例上的该帐户下运行。 默认情况下，这是辅助服务器实例的 SQL Server 代理服务帐户，但是 sysadmin 可以为该作业选择其他代理帐户。  
  
 **在以下时间后删除复制的文件**  
 选择希望复制的事务日志备份文件在删除前保留在目标文件夹中的时间。  
  
 **作业名称**  
 显示用于将事务日志备份文件从主服务器复制到辅助服务器的 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 代理作业的名称。 首次创建此作业时，可以通过在该框中键入内容来更改名称。  
  
 **计划**  
 显示用于将事务日志备份从主服务器复制到辅助服务器的 SQL Server 代理复制作业的当前计划。 通过单击 **“计划...”** 可以修改此计划。  
  
 **“计划...”**  
 修改用于将事务日志备份从主服务器复制到辅助服务器的 SQL Server 代理作业的参数。  
  
 **禁用此作业**  
 挂起 SQL Server 代理复制作业。  
  
 **“还原事务日志”选项卡**  
 选项如下：  
  
 **在还原备份时断开数据库中用户的连接**  
 在还原事务日志备份时，自动断开用户与辅助数据库的连接。  
  
 **无恢复模式**  
 使辅助数据库处于 NORECOVERY 模式。  
  
 **备用模式**  
 使辅助数据库处于 STANDBY 模式。 此模式允许对数据库执行只读操作。  
  
> [!IMPORTANT]  
>   如果更改现有辅助数据库的恢复模式（例如，从 **“无恢复模式”** 到 **“备用模式”**），则更改仅在下一次日志备份还原到数据库后才会生效。  
  
 **延迟还原备份操作至少**  
 选择将事务日志备份还原到辅助数据库之前的延迟时间（如果有）。  
  
 **在以下时间内没有执行还原时报警**  
 选择在引发警报以报告未执行事务日志还原之前，希望日志传送等待的时间。  
  
 **作业名称**  
 显示用于将事务日志备份还原到辅助数据库的 SQL Server 代理作业的名称。 首次创建此作业时，可以通过在该框中键入内容来更改名称。  
  
 **计划**  
 显示用于将事务日志备份还原到辅助数据库的 SQL Server 代理作业的当前计划。 通过单击 **“计划...”** 可以修改此选项。  
  
 **“计划...”**  
 修改与 SQL Server 代理还原作业相关联的参数。  
  
 **禁用此作业**  
 挂起对辅助数据库的还原操作。  
  
## <a name="see-also"></a>另请参阅  
 [SQL Server 数据库的备份和还原](../../relational-databases/backup-restore/back-up-and-restore-of-sql-server-databases.md)   
 [关于日志传送 (SQL Server)](../../database-engine/log-shipping/about-log-shipping-sql-server.md)  
  
  
