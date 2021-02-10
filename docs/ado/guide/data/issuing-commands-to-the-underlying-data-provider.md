---
description: 向基础数据提供程序发出命令
title: 向基础数据提供程序发出命令 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO]
- underlying providers [ADO]
- data shaping [ADO], commands
ms.assetid: d6001863-7733-4c32-817f-081e48587fa1
author: rothja
ms.author: jroth
ms.openlocfilehash: c01178c70a0e6a9e049f9fcfbc0a584fa5fa7ef0
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032739"
---
# <a name="issuing-commands-to-the-underlying-data-provider"></a>向基础数据提供程序发出命令
不以 SHAPE 开头的任何命令都将传递给数据访问接口。 这等效于使用 "SHAPE {provider command}" 形式发出形状命令。 这些命令 *无* 需生成 **记录集**。 例如，如果数据访问接口支持 DROP TABLE，则 "SHAPE {DROP TABLE MyTable} 是一个完全有效的形状" 命令。  
  
 此功能允许普通提供程序命令和形状命令共享相同的连接和事务。  
  
## <a name="see-also"></a>另请参阅  
 [数据定形示例](./data-shaping-example.md)   
 [正式形状语法](./formal-shape-grammar.md)   
 [常用 Shape 命令](./shape-commands-in-general.md)