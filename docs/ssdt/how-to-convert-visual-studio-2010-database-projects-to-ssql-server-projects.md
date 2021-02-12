---
title: 将 Visual Studio 2010 数据库项目转换为 SQL Server 数据库项目
description: 将 Visual Studio 2010 数据库项目转换为 SQL Server 项目，并将其重新定位到其他平台。 查看 SSDT 可以和不能转换的对象。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
f1_keywords:
- sql.data.tools.projectconversion.dialog
- sql.data.tools.ImportDAC
ms.assetid: 7e5acf94-5c46-44c7-9ff5-ca7926f5332a
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: bf8db10c508ed2cdf5fd2f4bc96c0d1d2508d83d
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100018377"
---
# <a name="how-to-convert-a-visual-studio-2010-database-projects-to-sql-server-database-projects-and-retarget-to-a-different-platform"></a>如何：将 Visual Studio 2010 数据库项目转换为 SQL Server 数据库项目并重新以不同平台为目标

在 SQL Server Data Tools (SSDT) 中，可以将在 Visual Studio 2010 中创建的现有 SQL Server 数据库、CLR 和数据层应用程序项目转换为新的 SQL Server 数据库项目。 通过这样做，你可以利用 SSDT 提供的新的数据库开发体验（例如更新的 Transact\-SQL 编辑体验），并且能够通过代码验证将项目重新针对 Microsoft SQL Server 2012 和 SQL Azure 进行设计。 该转换过程转换在 SSDT 中具有等效类型的对象（表、视图、存储过程、属性文件或脚本），包括其权限和 DAC 策略文件。 无法转换的项目在转换日志报告中突出显示。  
  
下表列出可由 SSDT 转换或者不能由 SSDT 转换的所有项目项。  
  
|可以转换的项目项|不能转换的项目项|  
|-------------------------------------------|----------------------------------------------|  
|项目文件<br /><br />.dbproj（Visual Studio 2010 数据库和服务器项目、数据层应用程序项目）项目文件<br /><br />.csproj 和 .vbproj CLR 项目文件可转换，但可能导致数据丢失|数据库单元测试项目<br /><br />.files 项之类的部分项目|  
|属性文件<br /><br />*.sqldeployment、.sqlsettings 和 .sqlpolicy 文件将转换为其对应的项目属性页<br /><br />.sqlpermissions 文件将转换为 Transact\-SQL 脚本|Project Properties<br /><br />Server.sqlsettings<br /><br />在 .sqlcmd 文件中定义的 SQLCMD 变量|  
|使用其现有的文件夹结构导入 .sql 文件。|扩展性文件。|  
|预先部署和后期部署脚本|在项目转换后必须手动重新建立数据库引用。|  
|架构比较文件|数据生成文件。|  
  
### <a name="to-convert-a-project"></a>转换项目  
  
1.  打开 SQL Server 2005 或 2008 数据库项目。  
  
2.  “转换为 SQL Server 数据库项目”向导将自动打开。 选择“转换为 SQL Server 数据库项目”，然后单击“确定”。 保留默认设置以便备份选中的现有文件。  
  
3.  将自动生成一个转换报告，其中列出已转换的所有文件。 要了解有关转换过程的详细信息，请单击项目文件名旁边的 +（加号）。  
  
4.  请注意，在“解决方案资源管理器”中，项目文件、属性文件和架构对象都将转换。  
  
### <a name="to-change-a-projects-target-platform"></a>更改项目的目标平台  
  
1.  在“解决方案资源管理器”中，右键单击新转换的项目，然后选择“属性”以便访问“项目设置”对话框。  
  
2.  在“目标平台”下拉列表中选择任何 SSDT 支持的平台。  
  
## <a name="see-also"></a>另请参阅  
[如何：更改目标平台并发布数据库项目](../ssdt/how-to-change-target-platform-and-publish-a-database-project.md)  
  
