---
description: SSMA for DB2 Console (DB2ToSQL) 入门
title: SSMA for DB2 Console (DB2ToSQL) 入门 |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: f245c017-023e-4880-8721-8908d339525e
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 59caa0bd32c0fe0838b4d2d8528a83b084d6cd81
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100073521"
---
# <a name="getting-started-with-ssma--for-db2-console-db2tosql"></a>SSMA for DB2 Console (DB2ToSQL) 入门
本部分介绍启动和启动 DB2 控制台应用程序的过程。 本文还列出了在典型的 SSMA 控制台输出窗口中使用的约定。  
  
## <a name="launching-ssma-console"></a>启动 SSMA 控制台  
使用以下步骤启动 SSMA 控制台应用程序：  
  
1.  中转到 " **开始** "，然后指向 " **所有程序**"。  
  
2.  单击 " **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于 DB2 命令提示符的迁移助手**" 快捷方式。  
  
    它显示 "SSMA 控制台使用情况" 菜单和 `(/? Help)` ，以帮助你开始使用控制台应用程序。  
  
## <a name="procedure-for-using-the-ssma-console"></a>使用 SSMA 控制台的过程  
在 Windows 系统上成功启动控制台后，可以使用以下步骤来处理它：  
  
1.  通过脚本文件配置 SSMA 控制台。 有关此部分的详细信息，请参阅 [创建脚本文件 &#40;DB2ToSQL&#41;](../../ssma/db2/creating-script-files-db2tosql.md) 。  
  
2.  [创建变量值文件 &#40;DB2ToSQL&#41;](../../ssma/db2/creating-variable-value-files-db2tosql.md)  
  
3.  [&#40;DB2ToSQL 创建服务器连接文件&#41;](../../ssma/db2/creating-the-server-connection-files-db2tosql.md)  
  
4.  基于你的项目需求[执行 SSMA 控制台 &#40;DB2ToSQL&#41;](../../ssma/db2/executing-the-ssma-console-db2tosql.md)  
  
其他功能：  
  
1.  [管理密码](./managing-passwords-db2tosql.md) ，并将其导出或导入到其他窗口计算机上  
  
2.  [生成报表](./generating-reports-db2tosql.md) 以查看详细的 xml 输出报告，以便进行评估/conversion 和数据迁移。 还可以为刷新和同步命令生成详细的错误报告。  
  
## <a name="ssma-console-output-conventions"></a>SSMA 控制台输出约定  
执行 SSMA 脚本命令和选项后，控制台程序会在控制台上显示 (信息、错误等 ) 的结果和消息，或者，如果需要，则重定向到 xml 输出文件。 输出中的每种消息类型都用一种独特的颜色表示。 例如，以白色表示的短信表示脚本文件命令;绿色颜色表示用户输入的提示，等等。  
  
![SSMAConsoleOutput_Oracle](../../ssma/db2/media/ssmaconsoleoutput_oracle.jpg "SSMAConsoleOutput_Oracle")  
  
下表中的控制台输出的颜色解释：  
  
|Color|说明|  
|---------|---------------|  
|红色|执行过程中出现错误|  
|灰色|日期和时间戳，向用户发送消息|  
|White|脚本文件命令，消息类型|  
|Yellow|警告|  
|绿色|提示输入用户-输入|  
|蓝|操作的开始、完成和结果|  
  
## <a name="see-also"></a>另请参阅  
[安装 SSMA for DB2](./installing-ssma-for-db2-db2tosql.md)  
