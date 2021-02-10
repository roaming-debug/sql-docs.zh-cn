---
description: 孙级聚合
title: 孙聚合 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- grandchild aggregates [ADO]
- data shaping [ADO], grandchild aggregates
ms.assetid: 4162d35f-2ce1-4218-80a5-b6933348837e
author: rothja
ms.author: jroth
ms.openlocfilehash: a5cae4f996112d660564cb8efd3ea9b7e53c4f9f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100033147"
---
# <a name="grandchild-aggregates"></a>孙级聚合
可以为 shape 命令的子句中创建的章节列指定 *章节别名* ， (通常使用 AS 关键字) 。 您可以使用标识包含列的子级的完全限定名称标识整形 **记录集** 的任何章节中的任何列。 例如，如果父章节 chap1 包含一个 chap2，其中包含一个 "金额" 列，则该限定名称为 "chap1"。限定名称随后可用作聚合函数之一的参数 (SUM、AVG、MAX、MIN、COUNT、STDEV 或 ANY) 。  
  
## <a name="see-also"></a>另请参阅  
 [数据整理示例](./data-shaping-example.md)