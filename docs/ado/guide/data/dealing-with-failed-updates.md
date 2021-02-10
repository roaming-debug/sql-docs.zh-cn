---
description: 处理失败的更新
title: 处理失败的更新 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- updates [ADO], dealing with failed updates
ms.assetid: 299c37bd-19ff-4261-8571-b9665687e075
author: rothja
ms.author: jroth
ms.openlocfilehash: a5ce736c8dd24f2398d7c8c374be260080c32e11
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037637"
---
# <a name="dealing-with-failed-updates"></a>处理失败的更新
如果更新结束时出现错误，则解决这些错误的方式取决于错误的性质和严重性以及应用程序的逻辑。 但是，如果数据库与其他用户共享，则典型的错误是，其他人在执行此操作之前会修改该字段。 这种类型的错误被称为冲突。 ADO 检测到这种情况并报告错误。  
  
## <a name="remarks"></a>备注  
 如果存在更新错误，将在错误处理例程中捕获这些错误。 筛选包含 adFilterConflictingRecords 常量的记录集，以便仅显示冲突的行。 在此示例中，错误解决策略只是打印作者的名字和姓氏 (au_fname 和 au_lname) 。  
  
 提醒用户发生更新冲突的代码如下所示：  
  
```  
objRs.Filter = adFilterConflictingRecords  
objRs.MoveFirst  
Do While Not objRst.EOF  
   Debug.Print "Conflict: Name =  "; objRs!au_fname; " "; objRs!au_lname  
   objRs.MoveNext  
Loop  
```  
  
## <a name="see-also"></a>另请参阅  
 [批处理模式](./batch-mode.md)