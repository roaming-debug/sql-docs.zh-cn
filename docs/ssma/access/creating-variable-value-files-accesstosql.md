---
description: '创建变量值文件 (AccessToSQL) '
title: 创建变量值文件 (AccessToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 08/17/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 808595c3-8ef1-40bd-a93e-5cf237950e08
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: b7653d131ca9f345353de46b2579735a527bca5a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044927"
---
# <a name="creating-variable-value-files-accesstosql"></a>创建变量值文件 (AccessToSQL) 
变量值文件是一个 XML 文件，其中包含命令的参数值 (例如，通常跨服务器迁移更改) 源或目标服务器名称。 当发生大量的数据库迁移时，将在主脚本文件中创建多个用于存储每个源服务器的值的变量文件，并在命令行上使用 **-v** 开关来引用这些文件。 此行为有助于在包含多个变量文件中的变量值的几个脚本文件中维护静态值。  
  
> [!NOTE]  
> -  变量名称以 $ (美元) 符号为前缀并带有后缀。 如果变量值文件中没有为变量赋值，则会在分析脚本文件时出现错误，从而导致控制台执行过程停止。  
> -  的转义符 **$** 是 **$$** 。 如果参数的变量或静态值的值包含 **$** (美元) 符号，则 **$$** 必须指定，以将其视为字符而不是变量。  
> -  出于可维护性目的，可以在元素内声明变量 `'variable-group'` 以实现用户定义变量的逻辑分离。  此元素不是必需的。  
  
**示例：**  
  
**示例 1：**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$type$" value="MyProject"/>  
  
    <variable name="$project_folder$" value=".\$project_name$"/>  
  
    <variable name="$project_name$" value="$type$ConsoleProject"/>  
  
    <variable name="$project_overwrite$" value="true"/>  
  
    <variable name="$project_type$" value="sql-server-2008"/>  
  
  </variable-group>  
  
</variables>  
```  
**示例 2：**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetServerName$" value="xxx"/>  
  
      <variable name="$TargetDB$" value="xxx"/>  
  
      <variable name="$TargetUserName$" value="xxx"/>  
  
      <variable name="$TargetPassword$" value="xxx"/>  
  
      <variable name="$TargetIsTrusted$" value="xxx"/>  
  
      <variable name="$TrustedConnection$" value="xxx"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="TestTable1"/>  
  
      <variable name="$ObjectName2$" value="TestProc1"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>变量值文件验证  
用户可以根据 "架构" 文件夹中提供的架构定义文件 ConsoleScriptVariablesSchema 来轻松地验证其变量值文件 **。**  
  
## <a name="next-step"></a>后续步骤  
操作控制台的下一步是 [&#40;AccessToSQL 创建服务器连接文件&#41;](../../ssma/access/creating-the-server-connection-files-accesstosql.md)  
  
## <a name="see-also"></a>另请参阅  
[ (访问) 创建服务器连接文件 ](./creating-the-server-connection-files-accesstosql.md)  
