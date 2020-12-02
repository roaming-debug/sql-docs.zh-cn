---
description: 发布数据库 (SQL Server Management Studio)
title: 发布数据库 (SQL Server Management Studio) | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: database-engine
ms.reviewer: ''
ms.technology: ''
ms.topic: conceptual
ms.assetid: 98b2914e-7147-40af-ba7d-87253bbe8bf9
author: stevestein
ms.author: sstein
ms.openlocfilehash: b7865180ffd1bcf090e51eafd1542e12c97d17ac
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "92194989"
---
# <a name="publish-a-database-sql-server-management-studio"></a>发布数据库 (SQL Server Management Studio)
 [!INCLUDE [SQL Server](../../includes/applies-to-version/sqlserver.md)]
   可以使用“生成和发布脚本向导”向 Web 宿主提供程序发布整个数据库或单独的数据库对象。  
  
> [!NOTE]  
>  本主题中介绍的功能以前往往由“发布数据库向导”提供。 发布功能已添加到“生成和发布脚本向导”，“发布数据库向导”已停止使用。  
  
## <a name="generate-and-publish-scripts-wizard"></a>“生成和发布脚本向导”  
 “生成和发布脚本向导”可用于向 Web 宿主提供程序发布数据库或所选数据库对象。 SQL Server Web 宿主提供程序是与 Web 服务之间的连接接口。 Web 服务是通过使用来自 CodePlex 上的 SQL Server Hosting Toolkit 的 Database Publishing Services 项目生成的。 通过使用“生成和发布脚本向导”，此 Web 服务使 Web 宿主客户能够轻松地将其数据库发布到该服务。 有关下载 SQL Server Hosting Toolkit 的详细信息，请参阅 [SQL Server Database Publishing Services](https://go.microsoft.com/fwlink/?LinkId=142025)（SQL Server 数据库发布服务）。  
  
 “生成和发布脚本向导”还可用于创建传输数据库的脚本。  
  
#### <a name="to-publish-a-database-to-a-web-service"></a>将数据库发布到 Web 服务  
  
1.  在对象资源管理器中，展开“数据库”，右键单击某个数据库，指向“任务”，然后单击“生成和发布脚本”。 按照向导中的步骤，创建用于发布的数据库对象的脚本。  
  
2.  在 **“选择对象”** 页上，选择要发布到 Web 宿主服务的对象。  
  
3.  在 **“设置脚本编写选项”** 页中，选择 **“发布到 Web 服务”**。  
  
    1.  在 **“提供程序”** 框中，指定 Web 服务的提供程序。 如果您尚未配置 Web 宿主提供程序，请选择 **“管理提供程序”** ，然后使用 **“管理提供程序”** 对话框来为您的 Web 服务配置一个提供程序。  
  
    2.  若要指定高级发布选项，请选择 **“发布到 Web 服务”** 部分中的 **“高级”** 按钮。  
  
4.  在 **“摘要”** 页上，检查你的选择。 单击 **“上一步”** 以更改您的选择。 单击 **“下一步”** 以发布所选的对象。  
  
5.  在 **“保存或发布脚本”** 页上，监视发布的进度。  

## <a name="see-also"></a>另请参阅  
 [生成脚本 (SQL Server Management Studio)](../../ssms/scripting/generate-scripts-sql-server-management-studio.md)   
 [将数据库复制到其他服务器](../../relational-databases/databases/copy-databases-to-other-servers.md)  
  
