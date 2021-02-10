---
description: 参数化命令与中间 COMPUTE 命令
title: 带有干预计算命令的参数化命令 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- data shaping [ADO], parameterized commands
- parameterized commands [ADO]
- APPEND clause [ADO]
- COMPUTE command [ADO]
ms.assetid: 732f624f-8900-4608-9815-194302d22e8b
author: rothja
ms.author: jroth
ms.openlocfilehash: ce78d5edf2ded2744c3633368d571d72912c1322
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037217"
---
# <a name="parameterized-commands-with-intervening-compute-commands"></a>参数化命令与中间 COMPUTE 命令
典型的参数化形状 APPEND 命令包含一个子句，该子句使用查询命令创建父 **记录集** ，并使用另一个子句创建具有参数化查询命令的子 **记录** 集，即包含参数占位符 (问号 "？" 的命令) 。 生成的整形 **记录集** 具有两个级别，其中父级占据较高级别，而子级占据较低级别。  
  
 创建子 **记录集** 的子句现在可以是任意数量的嵌套形状计算命令，其中最深层嵌套的命令包含参数化查询。 生成的整形 **记录集** 具有多个级别，其中父级占据最顶层，而子级占据 lowermost 级别，形状计算命令生成的任意数目的 **记录集** 都占用了中间级别。  
  
 此功能的典型用途是调用聚合函数和 shapeCOMPUTE 命令的分组功能，以创建包含关于子 **记录集** 的分析信息的 **记录集** 对象。 此外，由于这是一个参数化的 shape 命令，每次访问父级的章节列时，都可以检索到一个新的子 **记录集** 。 由于插入级别是从子级派生的，因此也会重新计算它们。  
  
## <a name="see-also"></a>另请参阅  
 [数据整理示例](../../../ado/guide/data/data-shaping-example.md)
