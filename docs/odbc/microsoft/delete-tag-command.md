---
description: DELETE TAG 命令
title: 删除标记命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- DELETE TAG command [ODBC]
ms.assetid: 4f4e1362-a5f3-4b15-8a3c-d4e96605f221
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: aa431c50fcd151d57ce7613586587a1c2342a5c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99181566"
---
# <a name="delete-tag-command"></a>DELETE TAG 命令
从复合索引 () 文件中移除一个或一些标记。  
  
## <a name="syntax"></a>语法  
  
```  
  
DELETE TAG TagName1 [OF CDXFileName1]  
   [, TagName2 [OF CDXFileName2]] ...  
  Or   
DELETE TAG ALL [OF CDXFileName]  
```  
  
## <a name="arguments"></a>参数  
 *TagName1* Of *CDXFileName1*[， *TagName2*[of *CDXFileName2*]] .。。  
 指定要从复合索引文件中移除的标记。 您可以使用一个删除标记来删除多个标记，方法是包含用逗号分隔的标记名称列表。 如果打开的索引文件中存在具有相同名称的两个或多个标记，则可以通过包含 *CDXFileName* 来从特定索引文件中删除标记。  
  
 ALL [of *CDXFileName*]  
 从复合索引文件中移除每个标记。 如果当前表具有结构化复合索引文件，则将从该索引文件中删除所有标记，将从磁盘中删除索引文件，并且会删除表标头中的标志，指示存在关联的结构复合索引文件。 使用 with of *CDXFileName* 将所有标记从结构化复合索引文件之外的任何打开的复合索引文件中删除。  
  
## <a name="remarks"></a>备注  
 用 INDEX 创建的复合索引文件包含与索引条目对应的标记。 删除标记用于删除打开的复合索引文件中的一个或一些标记。 只能删除在当前工作区中打开的复合索引文件中的标记。 如果从复合索引文件中删除所有标记，则该文件将从磁盘中删除。  
  
 Visual FoxPro 首先查找结构化复合索引文件中的标记， (如果打开了一个) 。 如果标记不在结构化复合索引文件中，Visual FoxPro 将在其他打开的复合索引文件中查找标记。  
  
## <a name="see-also"></a>另请参阅  
 [INDEX 命令](../../odbc/microsoft/index-command.md)
