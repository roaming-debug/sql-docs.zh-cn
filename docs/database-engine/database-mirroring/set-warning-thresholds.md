---
title: 设置警告阈值 | Microsoft Docs
description: 了解如何使用“设置警告阈值”对话框为 SQL Server 中的选定镜像数据库启用和配置警告阈值。
ms.custom: ''
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
f1_keywords:
- sql13.swb.dbmmonitor.setwarningthreshold.f1
ms.assetid: 17f93147-e7d9-4092-b4c2-c11b38051171
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 999b5d9e1cf50a9ffc2ff349dafac73de9a1465b
ms.sourcegitcommit: 370cab80fba17c15fb0bceed9f80cb099017e000
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/17/2020
ms.locfileid: "97641934"
---
# <a name="set-warning-thresholds"></a>设置警告阈值
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  使用该对话框可为在 **“数据库镜像监视器”** 对话框的导航树中选定的数据库启用和配置一个或多个警告阈值。  
  
 该对话框尝试连接到两个服务器实例上。 将异步建立这两个连接。 对话框显示每个伙伴的连接状态。 如果伙伴没有连接，则可单击 **“连接”** 。  
  
 **使用 SQL Server Management Studio 监视数据库镜像**  
  
-   [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)  
  
## <a name="options"></a>选项  
 *服务器实例及其连接状态*  
 伙伴服务器实例的名称（格式为 SYSTEM\\INSTANCE_NAME）。 对于默认服务器实例，只显示系统名。  
  
 该字段还指示当前监视器是否连接至该服务器实例。 可能的连接状态为：  
  
-   未连接到 server_instance_name  
  
-   正尝试连接到 server_instance_name  
  
-   已连接到 server_instance_name  
  
    > [!NOTE]  
    >  如果你不是 sysadmin 固定服务器角色的成员，状态将为“已连接到 server_instance_name (受限权限)” 。  
  
 每个伙伴服务器实例的名称将在单独的 *“服务器实例及其连接状态”* 字段中显示。 当监视器开始运行时，顶部字段会列出主体服务器。  
  
 **连接**/**取消**  
 “连接/取消” 按钮和每个 *服务器实例及其连接状态* 字段相关联。 按钮状态取决于连接状态：  
  
-   如果未与服务器实例建立连接，则按钮文本为 **“连接”** 。 单击可连接到服务器实例。  
  
-   当正在进行连接尝试时，按钮文本为 **“取消”** 。 单击可取消连接尝试。  
  
-   如果服务器已连接，按钮文本为 **“已连接”** ，并且该按钮呈灰色。  
  
 **Thresholds**  
 **阈值** 网格显示两个服务器实例的警告设置。  
  
> [!NOTE]  
>  如果服务器实例未连接，它的列将显示为空单元格和灰色背景。 当连接打开时，网格将自动显示来自实例的内容。  
  
 该网格包含以下列：  
  
 **警告**  
 列出所支持的警告：  
  
|警告|说明|  
|-------------|-----------------|  
|**如果未发送日志超出了阈值，则发出警告**|该阈值指示在主体服务器的发送队列中未发送日志的 KB 值。|  
|**如果未还原日志超出了阈值，则发出警告**|该阈值指示在镜像服务器实例中重做队列的 KB 值。|  
|**如果最早的未发送事务的保留时间超出了阈值，则发出警告**|该阈值指示事务没有从发送队列发送到镜像服务器实例中的分钟数。 该值有助于测量数据丢失的可能性（以时间计）。|  
|**如果镜像提交开销超过了阈值则发出警告**|该阈值指示每个事务的延迟的毫秒数（只适用于高安全模式）。 此延迟是主体服务器实例等待镜像服务器实例将事务日志记录写入重做队列时，所发生的开销量。|  
  
 已在“\<server instance>”中启用  
 空白复选框指示当前在服务器实例中禁用该警告。 若要启用警告，请单击其复选框。  
  
 “\<server instance>”的阈值  
 启用警告时，可在该列的左边设置阈值。 更新状态表时，如果达到指定阈值，则会触发事件。 如果在配置一个值后禁用阈值，该值将保留在字段中。如果重新启用警告，将继续使用该值。  
  
 未启用警告时，该字段将处于不活动状态。  
  
 **确定**  
 单击“确定”可关闭此对话框，并显示当前在“警告”选项卡式页的“阈值”网格中指定的警告阈值。  
  
## <a name="remarks"></a>备注  
 在同一时间阈值只适用于一个伙伴，但我们建议您在两个伙伴上都为给定事件设置阈值，以确保数据库进行故障转移时，警告仍然存在。 每个伙伴的相应阈值取决于伙伴系统的性能。  
  
 更新状态表时，只有在性能值处于或高于阈值的情况下，才会将事件写入性能的事件日志。 如果峰值在两次状态更新之间瞬间达到阈值，峰值将丢失。  
  
## <a name="see-also"></a>另请参阅  
 [启动数据库镜像监视器 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-database-mirroring-monitor-sql-server-management-studio.md)   
 [监视数据库镜像 (SQL Server)](../../database-engine/database-mirroring/monitoring-database-mirroring-sql-server.md)   
 [启动配置数据库镜像安全向导 (SQL Server Management Studio)](../../database-engine/database-mirroring/start-the-configuring-database-mirroring-security-wizard.md)  
  
  
