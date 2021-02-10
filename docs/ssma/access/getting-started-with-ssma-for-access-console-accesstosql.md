---
description: SSMA for Access Console (AccessToSQL) 入门
title: 入门使用 SSMA 访问控制台 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 8585ec16-7e0a-483a-b250-adab9b9232a3
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: d39f130c2b2ff8edd95fb52a78b7865b5c6dc868
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044507"
---
# <a name="getting-started-with-ssma-for-access-console-accesstosql"></a>SSMA for Access Console (AccessToSQL) 入门
本部分介绍了启动访问控制台应用程序并开始使用该应用程序的过程。 本文还列出了在典型的 SSMA 控制台输出窗口中使用的约定。  
  
## <a name="launching-ssma-console"></a>启动 SSMA 控制台  
使用以下步骤启动 SSMA 控制台应用程序：  
  
1.  中转到 " **开始** "，然后指向 " **所有程序**"。  
  
2.  单击 " **访问命令提示符** " 快捷方式的 SQL Server 迁移助手。  
  
    它显示 "SSMA 控制台使用情况" 菜单和 `(/? Help)` ，以帮助你开始使用控制台应用程序。  
  
## <a name="procedure-for-using-the-ssma-console"></a>使用 SSMA 控制台的过程  
在 Windows 系统上成功启动控制台后，可以使用以下步骤来处理它：  
  
1.  通过脚本文件配置 SSMA 控制台。 有关此部分的详细信息，请参阅 [创建脚本文件 &#40;AccessToSQL&#41;](../../ssma/access/creating-script-files-accesstosql.md)。  
  
2.  [创建变量值文件 &#40;AccessToSQL&#41;](../../ssma/access/creating-variable-value-files-accesstosql.md)  
  
3.  [&#40;AccessToSQL 创建服务器连接文件&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
4.  基于你的项目需求[执行 SSMA 控制台 &#40;AccessToSQL&#41;](../../ssma/access/executing-the-ssma-console-accesstosql.md)  
  
其他功能：  
  
1.  [指定密码](managing-passwords-accesstosql.md) ，并将其导出或导入到其他窗口计算机上  
  
2.  [生成报表](generating-reports-accesstosql.md) 以查看详细的 xml 输出报告，以便进行评估/conversion 和数据迁移。 还可以为刷新和同步命令生成详细的错误报告。  
  
## <a name="ssma-console-output-conventions"></a>SSMA 控制台输出约定  
执行 SSMA 脚本命令和选项后，控制台程序会在控制台上显示 (信息、错误等 ) 的结果和消息，或者，如果需要，则重定向到 xml 输出文件。 输出中的每种消息类型都用一种独特的颜色表示。 例如，以白色表示的短信表示脚本文件命令;绿色颜色表示用户输入的提示，等等。  
  
![SSMA 控制台输出](../../ssma/access/media/ssmaconsoleoutput.jpg "SSMA 控制台输出")  
  
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
[安装访问 SQL Server 迁移助手](installing-sql-server-migration-assistant-for-access-accesstosql.md)  
  
