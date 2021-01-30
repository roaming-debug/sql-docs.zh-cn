---
description: SET COLLATE 命令
title: 设置逐份打印命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- set collate command [ODBC]
ms.assetid: 00efbcd4-fea8-4061-86a5-82de413cb753
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: f6158f79c589e446c2b3c106a1d14fd58715714f
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208645"
---
# <a name="set-collate-command"></a>SET COLLATE 命令
指定后续索引和排序操作中字符字段的排序规则顺序。  
  
## <a name="syntax"></a>语法  
  
```  
  
SET COLLATE TO cSequenceName  
```  
  
## <a name="arguments"></a>参数  
 *cSequenceName*  
 指定排序规则顺序。 下表介绍了可用的排序规则序列选项。  
  
|选项|语言|  
|-------------|--------------|  
|荷兰语|荷兰语|  
|GENERAL|英语、法语、德语、新式西班牙语、葡萄牙语和其他西欧语言|  
|德语|德国电话薄定购)  (|  
|冰岛|冰岛语|  
|设备|计算机 (早期 FoxPro 版本的默认排序规则顺序) |  
|NORDAN|挪威语、丹麦语|  
|西班牙语|传统西班牙语|  
|SWEFIN|瑞典语、芬兰语|  
|UNIQWT|唯一权重|  
  
> [!NOTE]  
>  指定西班牙语选项时， *ch* 是在 *c* 和 *d* 之间进行排序的单个字母，并将在 *l* 和 *m* 之间进行排序。   
  
 如果将排序规则序列选项指定为文本字符串，请确保将该选项用引号引起来：  
  
```  
SET COLLATE TO "SWEFIN"  
```  
  
 计算机是默认的排序规则序列选项，是用户熟悉的序列 Xbase。 字符在当前代码页中出现时按顺序排列。  
  
 通常，适用于美国和西欧用户。 字符在当前代码页中出现时按顺序排列。 在版本早于2.5 的 FoxPro 版本中，可能已使用 **上限** ( ) 或 **更低** ( ) 函数创建了索引，以便将字符字段转换为一致的大小写。 在超过2.5 的 FoxPro 版本中，你可以改为指定常规排序规则序列选项，并省略 ) 转换的 ( **上限** 。  
  
 如果你指定了除计算机之外的排序规则序列选项，并且创建 idx 文件，则始终会创建一个 compact. idx。  
  
 使用 SET ( "COLLATE" ) 返回当前排序规则序列。  
  
 您可以通过使用 " [ODBC Visual FoxPro 安装程序" 对话框](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md) 或通过在连接字符串中将 Collate 关键字与 [SQLDriverConnect](../../odbc/microsoft/sqldriverconnect-visual-foxpro-odbc-driver.md)一起使用来指定数据源的排序顺序。 这等同于发出以下命令：  
  
```  
SET COLLATE TO cSequenceName  
```  
  
## <a name="remarks"></a>备注  
 通过设置排序功能，您可以对任何支持的语言的包含重音字符的表进行排序。 更改集排序的设置不会影响以前打开的索引的排序顺序。 Visual FoxPro 自动维护现有索引，从而灵活地创建许多不同类型的索引，即使对于同一个字段也是如此。  
  
 例如，如果创建索引时设置了 "分页" 设置为 "常规"，并且 "设置排序规则" 设置稍后更改为西班牙语，则索引将保留常规排序规则顺序。  
  
## <a name="see-also"></a>另请参阅  
 [ODBC Visual FoxPro 设置对话框](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md)
