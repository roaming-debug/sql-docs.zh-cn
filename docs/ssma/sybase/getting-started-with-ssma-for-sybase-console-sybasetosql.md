---
description: '入门用于 Sybase 控制台的 SSMA (SybaseToSQL) '
title: 入门用于 Sybase 控制台的 SSMA (SybaseToSQL) |Microsoft Docs
ms.custom: ''
ms.date: 09/30/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Sybase Console,Launching SSMA Console
- Sybase Console,Output Conventions
- Sybase Console,Procedure for Using Console
ms.assetid: 43219dbe-bcfa-427d-9242-f07b1455f15f
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 19fca27ba0606a8e56c1594df852edbc439e7a92
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100064962"
---
# <a name="getting-started-with-the-ssma-for-sybase-console-sybasetosql"></a>入门用于 Sybase 控制台的 SSMA (SybaseToSQL) 
本部分介绍了启动和启动用于 Sybase 控制台应用程序的 SSMA 的过程。 本文还列出了在典型的 SSMA 控制台输出窗口中使用的约定。  
  
## <a name="launching-the-ssma-console"></a>启动 SSMA 控制台  
使用以下步骤启动 SSMA 控制台应用程序：  
  
1.  中转到 "开始"，然后指向 "所有程序"。  
  
2.  单击 " **Sybase 命令提示符** " 快捷方式的 SQL Server 迁移助手。  
  
    它显示 "SSMA 控制台使用情况" 菜单和 `(/? Help)` ，以帮助你开始使用控制台应用程序。  
  
## <a name="using-the-ssma-console"></a>使用 SSMA 控制台  
在 Windows 系统上成功启动控制台后，可以使用以下步骤对其进行操作：  
  
1.  通过脚本文件配置 SSMA 控制台。 有关此部分的详细信息，请参阅 [创建脚本文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-script-files-sybasetosql.md)。  
  
2.  [创建变量值文件 &#40;SybaseToSQL&#41;](../../ssma/sybase/creating-variable-value-files-sybasetosql.md)  
  
3.  [&#40;SybaseToSQL 创建服务器连接文件&#41;](../../ssma/sybase/creating-the-server-connection-files-sybasetosql.md)  
  
4.  根据项目需要，[执行 SSMA 控制台 &#40;SybaseToSQL&#41;](../../ssma/sybase/executing-the-ssma-console-sybasetosql.md) 。 
  
其他功能：  
  
1.  [指定密码](managing-passwords-sybasetosql.md) ，并将其导出/导入到其他窗口计算机。  
  
2.  [生成报表](generating-reports-sybasetosql.md) 以查看详细的 xml 输出报告，以便进行评估/转换和数据迁移。 你还可以为刷新和同步命令生成详细的错误报告。  
  
## <a name="ssma-console-output-conventions"></a>SSMA 控制台输出约定  
执行 SSMA 脚本命令和选项时，控制台程序会将结果和消息显示 (信息、错误等 ) 到控制台上的用户，或者在必要时将其重定向到 xml 输出文件。 输出中的每种消息类型都用一种独特的颜色表示。 例如，以白色表示的短信表示脚本文件命令;绿色颜色表示用户输入的提示，等等。  
  
![显示 SSMA 控制台 Sybase 输出示例的屏幕截图。](../../ssma/sybase/media/ssmaconsoleoutput_sybase.JPG "SSMAConsoleOutput_Sybase")  
  
下表显示了控制台输出的颜色解释：  
  
|Color|说明|  
|---------|---------------|  
|红色|执行过程中出现错误|  
|灰色|日期和时间戳，向用户发送消息|  
|White|脚本文件命令，消息类型|  
|Yellow|警告|  
|绿色|提示输入用户-输入|  
|蓝|操作的开始时间、完成时间和结果|  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for SAP ASE &#40;SybaseToSQL&#41;](../../ssma/sybase/installing-ssma-for-sybase-sybasetosql.md)  
