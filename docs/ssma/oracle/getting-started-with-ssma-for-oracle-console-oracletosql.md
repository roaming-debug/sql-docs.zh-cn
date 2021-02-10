---
description: SSMA for Oracle 控制台入门 (OracleToSQL)
title: SSMA for Oracle Console (OracleToSQL) 入门 |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Console, Console Output Conventions
- Oracle Console, Launching Console
ms.assetid: 667a5e4a-6848-4973-a72d-1287f64718ac
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: fd87502c1eda3dc8d4caf4c654804b164f24f36f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100068032"
---
# <a name="getting-started-with-ssma--for-oracle-console-oracletosql"></a>SSMA for Oracle 控制台入门 (OracleToSQL)
本部分介绍了启动和开始 Oracle 控制台应用程序的过程。 本文还列出了在典型的 SSMA 控制台输出窗口中使用的约定。  
  
## <a name="launching-ssma-console"></a>启动 SSMA 控制台  
使用以下步骤启动 SSMA 控制台应用程序：  
  
1.  中转到 " **开始** "，然后指向 " **所有程序**"。  
  
2.  单击 " **[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 用于 Oracle 命令提示符的迁移助手** 快捷方式" 快捷方式。  
  
    它显示 "SSMA 控制台使用情况" 菜单和 `(/? Help)` ，以帮助你开始使用控制台应用程序。  
  
## <a name="procedure-for-using-the-ssma-console"></a>使用 SSMA 控制台的过程  
在 Windows 系统上成功启动控制台后，可以使用以下步骤来处理它：  
  
1.  通过脚本文件配置 SSMA 控制台。 有关此部分的详细信息，请参阅 [创建脚本文件 &#40;OracleToSQL&#41;](../../ssma/oracle/creating-script-files-oracletosql.md) 。  
  
2.  [创建变量值文件 &#40;OracleToSQL&#41;](../../ssma/oracle/creating-variable-value-files-oracletosql.md)  
  
3.  [&#40;OracleToSQL 创建服务器连接文件&#41;](../../ssma/oracle/creating-the-server-connection-files-oracletosql.md)  
  
4.  基于你的项目需求[执行 SSMA 控制台 &#40;OracleToSQL&#41;](../../ssma/oracle/executing-the-ssma-console-oracletosql.md)  
  
其他功能：  
  
1.  [指定密码](managing-passwords-oracletosql.md) ，并将其导出或导入到其他窗口计算机上  
  
2.  [生成报表](generating-reports-oracletosql.md) 以查看详细的 xml 输出报告，以便进行评估/conversion 和数据迁移。 还可以为刷新和同步命令生成详细的错误报告。  
  
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
[安装 SSMA for Oracle](installing-ssma-for-oracle-oracletosql.md)  
  
