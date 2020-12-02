---
description: 查看和生成补充日志记录脚本
title: 查看和生成补充日志记录脚本 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- scripts
ms.assetid: 5c858ae2-37d6-42e8-a252-7f6ed4e628a7
author: chugugrace
ms.author: chugu
ms.openlocfilehash: 77a6b4f11ea2e01f43f1be335fad893ee87f1e90
ms.sourcegitcommit: c5078791a07330a87a92abb19b791e950672e198
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 11/26/2020
ms.locfileid: "88457677"
---
# <a name="review-and-generate-supplemental-logging-scripts"></a>查看和生成补充日志记录脚本

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  使用“脚本”选项卡可对 Oracle 源数据库运行或重新运行一个设置补充日志记录的脚本。  
  
 在运行脚本前，请选择以下选项之一：  
  
 **包括此会话中的更改**  
 选择此选项可对已添加到 CDC 实例的任何新表运行该脚本，或者对使用属性编辑器以任何方式更改的表运行脚本。  
  
> [!NOTE]  
>  如果没有对 CDC 实例中的表进行任何更改，则 Oracle 补充日志记录脚本区域将是空的。  
  
 **包括所有表/捕获实例**  
 选择此选项可对 CDC 实例中的所有表和捕获实例运行脚本。  
  
 选择其中一个选项后，运行补充日志记录脚本。  
  
### <a name="to-run-the-supplemental-logging-scripts"></a>运行补充日志记录脚本  
  
1.  单击 **“运行脚本”** 可对为 CDC 实例定义的表运行补充日志记录脚本。 此脚本指示 Oracle 数据库在将 UPDATE 操作记录到捕获表时将有关的所有列写入其事务日志。 此脚本通常由 Oracle 系统管理员检查和执行。  
  
2.  在您运行补充日志记录脚本时，“用于运行脚本的 Oracle 凭据”对话框将打开，您可以在其中提供有效的 Oracle 用户名和密码。 有关如何提供适当的 Oracle 凭据的信息，请参阅 [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md)。  
  
 还可以根据需要使用 SQL\*Plus 手动运行脚本。  
  
### <a name="to-run-the-scripts-manually"></a>手动运行脚本  
  
1.  单击 **“复制”** 将脚本粘贴到剪贴板。 打开 SQL* Plus 并且转到具有您的 Oracle 源数据库的目录。 将脚本粘贴到 SQL\*Plus 中，以便运行该脚本。  
  
### <a name="to-save-the-supplemental-logging-script-in-a-text-file"></a>将补充日志记录脚本保存到一个文本文件中  
  
1.  单击 **“另存为”** 并且浏览到要保存该文件的位置。  
  
2.  为该文件提供一个名称，然后单击 **“保存”** 以便保存该文件。  
  
## <a name="see-also"></a>另请参阅  
 [如何编辑 CDC 实例属性](../../integration-services/change-data-capture/how-to-edit-the-cdc-instance-properties.md)   
 [Oracle Credentials for Running Script](../../integration-services/change-data-capture/oracle-credentials-for-running-script.md)  
  
  
