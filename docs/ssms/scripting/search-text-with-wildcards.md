---
title: 使用通配符搜索文本
description: 了解如何在“查找和替换”对话框的“查找内容”字段中使用通配符来指定要匹配的模式。
ms.custom: seo-lt-2019
ms.date: 03/01/2017
ms.prod: sql
ms.technology: ssms
ms.reviewer: ''
ms.topic: conceptual
f1_keywords:
- vswildcardsbuilder
helpviewer_keywords:
- searches [SQL Server Management Studio], wildcards
- Query Editor [SQL Server Management Studio], wildcard searches
- wildcard options [SQL Server Management Studio]
ms.assetid: 449600f8-cc87-4b3f-878a-59c158a88a40
author: markingmyname
ms.author: maghan
monikerRange: '>=aps-pdw-2016||=azuresqldb-current||=azure-sqldw-latest||>=sql-server-2016||>=sql-server-linux-2017||=azuresqldb-mi-current'
ms.openlocfilehash: 96ed98a5be4ad75dadb232a6ace2d15170f9390f
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97478758"
---
# <a name="search-text-with-wildcards"></a>使用通配符搜索文本
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
  以下表达式可以替换 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]“查找和替换”对话框的“查找内容”字段中的字符或数字 。  
  
#### <a name="to-search-using-wildcards"></a>使用通配符进行搜索  
  
1.  若要在“快速查找”、 **“在文件中查找”** 、 **“快速替换”** 或 **“在文件中替换”** 操作过程中在 **“查找内容”** 字段中使用通配符，请选择 **“查找选项”** 下的 **“使用”** 选项，然后选择 **“通配符”**。  
  
2.  **“查找内容”** 字段旁边的 **“引用列表”** 三角形按钮将变为可用状态。 单击此按钮以显示可用通配符列表。 从 **“引用列表”** 中选择任意项后，便会将该项插入到 **“查找内容”** 字符串中。  
  
 下表介绍了 **“引用列表”** 中可用的通配符。  
  
|表达式|语法|说明|  
|----------------|------------|-----------------|  
|任何单个字符|?|与任何单个字符匹配。|  
|任何单个数字|#|与任何单个数字匹配。 例如，7# 与包括 7 且其后跟随另一数字的数字匹配，例如 71，但不能是 17。|  
|集合中没有的字符|[! ]|与未在集合中指定的任一字符匹配。|  
|一个或多个字符|*|与任何一个或多个字符匹配。 例如，new* 与任何包括“new”的文本匹配，例如 newfile.txt。|  
|字符集|[ ]|与在集合中指定的任一字符匹配。|  
  
## <a name="see-also"></a>另请参阅  
 [搜索和替换](./search-and-replace.md)   
 [使用正则表达式搜索文本](./search-text-with-regular-expressions.md)