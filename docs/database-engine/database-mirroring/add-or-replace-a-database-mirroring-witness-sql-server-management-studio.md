---
title: 添加或替换镜像见证服务器 (SSMS)
description: 了解当数据库镜像端点使用 Windows 身份验证时，如何使用 SQL Server Management Studio 添加或替换数据库镜像见证服务器。
ms.custom: seo-lt-2019
ms.date: 03/07/2017
ms.prod: sql
ms.prod_service: high-availability
ms.reviewer: ''
ms.technology: database-mirroring
ms.topic: conceptual
helpviewer_keywords:
- witness [SQL Server], establishing
- database mirroring [SQL Server], witness
ms.assetid: 4b5ecffd-f025-4ab7-b69d-8958c6477127
author: MikeRayMSFT
ms.author: mikeray
ms.openlocfilehash: 810b3ac70c66801335ac3235439e5a0731c43eb8
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100350741"
---
# <a name="add-or-replace-a-database-mirroring-witness-sql-server-management-studio"></a>添加或替换数据库镜像见证服务器 (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
  如果数据库镜像端点使用 Windows 身份验证，则可以使用 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] 添加或替换见证服务器。 在 [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] 中添加见证服务器还会将运行模式更改为具有自动故障转移功能的高安全性模式。  
  
> [!NOTE]  
>  我们极力建议将见证服务器置于独立于每个伙伴的单独计算机中。 见证服务器所用服务帐户必须位于主体服务器实例和镜像服务器实例所用服务帐户所在的域中，或者必须位于可信域中。  
  
### <a name="to-add-or-replace-a-witness"></a>添加或替换见证服务器  
  
1.  连接到主体服务器实例之后，在对象资源管理器中，单击服务器名称以展开服务器树。  
  
2.  展开 **“数据库”** ，然后选择要为其添加或替换见证服务器的会话的主体数据库。  
  
3.  右键单击数据库，选择 **“任务”** ，再单击 **“镜像”** 。 这样便可打开 **“数据库属性”** 对话框的 **“镜像”** 页。  
  
4.  单击 **“配置安全性”** 。  
  
5.  如果显示 **“配置数据库镜像安全向导”** 欢迎屏幕，则请单击 **“下一步”** 。  
  
6.  在 **“包括见证服务器”** 对话框中，单击 **“是”** ，再单击 **“下一步”** 。  
  
7.  在 **“选择要配置的服务器”** 对话框中，将自动选中 **“见证服务器实例”** 复选框。 单击“下一步”。  
  
8.  在 **“主体服务器实例”** 对话框中，保留现有的端口和端点。 单击“下一步”。  
  
9. 在 **“见证服务器实例”** 对话框中，单击 **“连接”** 。  
  
10. 在“连接到服务器”对话框的“服务器名称”字段中，指定见证服务器实例，并使用 Windows 身份验证（默认设置）。 单击“连接”。  
  
11. 建立连接之后，便会在 **“见证服务器实例”** 对话框中显示见证服务器实例的侦听器端口和数据库镜像端点。 单击“下一步”。  
  
12. **“服务帐户”** 对话框包含主体服务器实例、镜像服务器实例和见证服务器实例的域服务帐户字段。  
  
    -   如果服务器实例全部使用相同的服务帐户，则请将这些字段保留为空白。  
  
    -   如果见证服务器实例所用的服务帐户不同于两个伙伴所用的服务帐户，则请使用以下帐户名填充 **“主体”** 、 **“镜像”** 和 **“见证服务器”** 字段：  
  
         *DOMAINNAME* **\\** *username*  
  
         域名必须大写。  
  
     单击“下一步”。  
  
13. 在 **“完成该向导”** 摘要屏幕中，检查见证服务器配置（可选），再单击 **“完成”** 。  
  
14. 完成之后，该向导将返回到 **“数据库属性”** 对话框，此对话框的 **“见证服务器”** 字段中现在显示见证服务器的服务器网络地址。 此外，还会自动选中“具有自动故障转移功能的高安全模式(同步)”，使用见证服务器时需要此选项。  
  
     若要启用见证服务器并将会话更改为具有自动故障转移功能的高安全性模式，请单击“确定”。  
  
## <a name="see-also"></a>另请参阅  
 [数据库镜像见证服务器](../../database-engine/database-mirroring/database-mirroring-witness.md)   
 [数据库镜像 (SQL Server)](../../database-engine/database-mirroring/database-mirroring-sql-server.md)   
 [数据库属性（“镜像”页）](../../relational-databases/databases/database-properties-mirroring-page.md)   
 [使用 Windows 身份验证建立数据库镜像会话 (SQL Server Management Studio)](../../database-engine/database-mirroring/establish-database-mirroring-session-windows-authentication.md)   
 [数据库镜像见证服务器](../../database-engine/database-mirroring/database-mirroring-witness.md)  
  
  
