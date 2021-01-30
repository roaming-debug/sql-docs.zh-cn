---
description: SET REPROCESS 命令
title: 设置重新处理命令 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- SET REPROCESS command [ODBC]
ms.assetid: b0708757-b1d7-42f3-8988-787f2a806b8b
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: b45188db00eda553bb3443d791d889a4d8c31a77
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208597"
---
# <a name="set-reprocess-command"></a>SET REPROCESS 命令
指定锁定尝试失败后锁定文件或记录的次数或时间。  
  
## <a name="syntax"></a>语法  
  
```  
  
SET REPROCESS TO nAttempts [SECONDS] | TO AUTOMATIC  
```  
  
## <a name="arguments"></a>参数  
 TO *nAttempts*[秒]  
 指定在初次尝试失败后尝试锁定记录或文件的次数或秒数。 默认值为 0;最大值为32000。  
  
 SECONDS 指定视觉 FoxPro 尝试锁定文件或记录 *nAttempts* 秒。 仅当 *nAttempts* 大于零时才能使用。  
  
 例如，如果 *nAttempts* 为30，则 Visual FoxPro 会尝试将记录或文件锁定最多30次。 如果还包括秒 (将重新处理设置为30秒) ，Visual FoxPro 会持续尝试将记录或文件锁定最多30秒。  
  
 如果 ON ERROR 例程有效并且如果尝试通过命令锁定记录或文件失败，则执行 ON ERROR 例程。 但是，如果函数尝试锁定，则不会执行 ON ERROR 例程，并且函数将返回 False ( ) 。  
  
 如果 ON ERROR 例程不起作用，则命令尝试锁定记录或文件，并且不能进行锁定，会生成错误。 如果函数尝试放置锁，则不会显示警报，并且函数返回 False ( ) 。  
  
 如果 *nAttempts* 为 0 (默认值) 并且发出尝试锁定记录或文件的命令或函数，则 Visual FoxPro 将尝试无限期地锁定记录或文件。 如果在等待时记录或文件可用于锁定，则会放置锁定，并清除系统消息。 如果函数试图放置锁，则函数将返回 True ( ) 。  
  
 如果 ON ERROR 例程有效并且命令正在尝试锁定记录或文件，则 ON ERROR 例程优先于其他尝试锁定记录或文件。 立即执行 ON ERROR 例程。 Visual FoxPro 不会尝试其他记录或文件锁定，也不会显示系统消息。  
  
 如果 *nAttempts* 为1，则 Visual FoxPro 会尝试无限期地锁定记录或文件，并且不会执行出错例程。  
  
 如果其他用户对您尝试锁定的记录或文件发出了锁，则必须等待用户释放该锁。  
  
 为自动  
 指定视觉 FoxPro 尝试无限期地锁定记录或文件。  (将重新处理设置为-2 是等效命令。 )   
  
## <a name="remarks"></a>备注  
 第一次尝试锁定记录或文件并不总是成功。 通常，记录或文件被网络上的其他用户锁定。 SET 重新处理将确定在初始尝试不成功时，Visual FoxPro 是否额外尝试锁定记录或文件。 您可以指定执行其他尝试的次数或尝试的时间。 "出错时" 例程会影响如何处理失败的锁定尝试。
