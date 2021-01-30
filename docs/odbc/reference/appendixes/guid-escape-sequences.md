---
description: GUID 转义序列
title: GUID 转义序列 |Microsoft Docs
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.technology: connectivity
ms.topic: reference
helpviewer_keywords:
- ODBC escape sequences [ODBC], GUID
- escape sequences [ODBC], guid
- guid escape sequence [ODBC]
ms.assetid: 71d43ef9-4a31-493e-b9e0-f864e9ef3ce6
author: David-Engel
ms.author: v-daenge
ms.openlocfilehash: 535d24034c219691b192d409f72df15c83ab22c7
ms.sourcegitcommit: 33f0f190f962059826e002be165a2bef4f9e350c
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 01/30/2021
ms.locfileid: "99208577"
---
# <a name="guid-escape-sequences"></a>GUID 转义序列
ODBC 对 GUID 文本使用转义序列。 此转义序列的语法如下所示：  
  
```  
{guid 'nnnnnnnn-nnnn-nnnn-nnnn-nnnnnnnnnnnn'}  
```  
  
## <a name="remarks"></a>备注  
 在 BNF 表示法中，语法如下：  
  
 *ODBC-guid-escape* ：： =  
     *ODBC-esc-发起程序 guid* "*guid-值*" *ODBC-esc-终止符*  
  
 *ODBC-esc-发起方* ：： = {  
  
 *ODBC-esc-终止符* ：： =}  
  
 *guid-值* ：： = *时钟-低值 guid-分隔符时钟中间值 guid 分隔符时钟-高值 guid 分隔符时钟-值 guid-分隔符节点-值*  
  
 *guid-分隔符* ：： =-  
  
 *时钟-低值*：： = *hex_digit hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit hex_digit  
  
 *时钟中间值* ：： = *hex_digit hex_digit hex_digit hex_digit*  
  
 *时钟-高值* ：： = *hex_digit hex_digit hex_digit hex_digit*  
  
 *时钟序列值* ：： = *hex_digit hex_digit hex_digit hex_digit*  
  
 *时钟-节点-值*：： = *hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit hex_digit* hex_digit hex_digit  
  
 *hex_digit* ：： = 0 &#124; 1 &#124; 2 &#124; 3 &#124; 4 &#124; 5 &#124; 6 &#124; 7 &#124; 8 &#124; &#124; &#124; &#124; &#124; &#124; &#124; E  
  
 如果数据源支持 GUID 数据类型，则支持 GUID 文本转义序列。 应用程序应调用 **SQLGetTypeInfo** 来确定此数据类型是否受支持。
