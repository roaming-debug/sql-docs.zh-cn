---
description: 正式 Shape 语法
title: 正式形状语法 |Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- shape commands [ADO], shape grammar
- data shaping [ADO], shape grammar
ms.assetid: ea691475-0f03-4abe-a785-b77e77712d1d
author: rothja
ms.author: jroth
ms.openlocfilehash: cd626fe6887f784ec62e22ccc276e64feb4a9b14
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100037457"
---
# <a name="formal-shape-grammar"></a>正式 Shape 语法
这是用于创建任何 shape 命令的正式语法：  
  
-   必需的语法术语是由尖括号分隔的文本字符串 ( "<>" ) 。  
  
-   可选字词由方括号分隔 ( "[]" ) 。  
  
-   替代项由扭转 ( "&#124;" ) 表示。  
  
-   重复替换项用省略号 ( "..." ) 表示。  
  
-   *Alpha* 表示字母数字的字符串。  
  
-   *数字* 表示数字的字符串。  
  
-   *Unicode 数字* 表示 unicode 数字的字符串。  
  
 所有其他字词均为文本。  
  
|术语|定义|  
|----------|----------------|  
|\<shape-command>|SHAPE [[ \<table-exp> [AS] \<alias> ]] [ \<shape-action> ]|  
|\<table-exp>|{ \<provider-command-text> } &#124;<br /><br />  (\<shape-command>) &#124;<br /><br /> 表 \<quoted-name> &#124;<br /><br /> \<quoted-name>|  
|\<shape-action>|追加 \<aliased-field-list> &#124;<br /><br /> 计算 \<aliased-field-list> [依据 \<field-list> ]|  
|\<aliased-field-list>|\<aliased-field> [, \<aliased-field...>]|  
|\<aliased-field>|\<field-exp> [[AS] \<alias> ]|  
|\<field-exp>| (\<relation-exp>) &#124;<br /><br /> \<calculated-exp> &#124;<br /><br /> \<aggregate-exp> &#124;<br /><br /> \<new-exp>|  
|<relation_exp>|\<table-exp> [[AS] \<alias> ]<br /><br /> 相互 \<relation-cond-list>|  
|\<relation-cond-list>|\<relation-cond> [, \<relation-cond>...]|  
|\<relation-cond>|\<field-name> 自 \<child-ref>|  
|\<child-ref>|\<field-name> &#124;<br /><br /> 参数 \<param-ref>|  
|\<param-ref>|\<number>|  
|\<field-list>|\<field-name> [, \<field-name>]|  
|\<aggregate-exp>|SUM (\<qualified-field-name>) &#124;<br /><br /> 平均 (\<qualified-field-name>) &#124;<br /><br /> 最小 (\<qualified-field-name>) &#124;<br /><br /> 最大 (\<qualified-field-name>) &#124;<br /><br /> 计数 (\<qualified-alias> &#124; \<qualified-name>) &#124;<br /><br /> STDEV (\<qualified-field-name>) &#124;<br /><br /> 任何 (\<qualified-field-name>) |  
|\<calculated-exp>|计算 (\<expression>) |  
|\<qualified-field-name>|\<alias>.[\<alias>...]\<field-name>|  
|\<alias>|\<quoted-name>|  
|\<field-name>|\<quoted-name> [[AS] \<alias> ]|  
|\<quoted-name>|" \<string> " &#124;<br /><br /> " \<string> " &#124;<br /><br /> [ \<string> ] &#124;<br /><br /> \<name>|  
|\<qualified-name>|alias [alias ...]|  
|\<name>|alpha [alpha &#124; 数字 &#124; _ &#124; # &#124;： &#124; ...]|  
|\<number>|数字 [数字 ...]|  
|\<new-exp>|NEW \<field-type> [ (\<number> [， \<number> ] ) ]|  
|\<field-type>|OLE DB 或 ADO 数据类型。|  
|\<string>|unicode-char [unicode-char ...]|  
|\<expression>|一个 Visual Basic for Applications 表达式，其操作数为同一行中的其他非计算列。|  
  
## <a name="see-also"></a>另请参阅  
 [访问分层记录集中的行](./accessing-rows-in-a-hierarchical-recordset.md)   
 [数据定形概述](./data-shaping-overview.md)   
 [数据整理所需的提供程序](./required-providers-for-data-shaping.md)   
 [Shape APPEND 子句](./shape-append-clause.md)   
 [整体形状命令](./shape-commands-in-general.md)   
 [Shape 计算子句](./shape-compute-clause.md)   
 [Visual Basic for Applications 函数](./visual-basic-for-applications-functions.md)