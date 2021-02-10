---
description: 创建变量值文件 (MySQLToSQL)
title: 创建变量值文件 (MySQLToSQL) |Microsoft Docs
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Creating variable value files
- variable value file validation
ms.assetid: 1dc56a7b-8e3a-4576-ad4f-47050bf7e28a
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: bf77727aa8a00ef1d50b2503abbb79d132f5326f
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100016607"
---
# <a name="creating-variable-value-files-mysqltosql"></a>创建变量值文件 (MySQLToSQL)
变量值文件是一个 XML 文件，其中包含命令的参数值（例如），这是经常从一台服务器迁移到另一台服务器的源服务器或目标服务器名称。 当发生大量的数据库迁移时，将在主脚本文件中创建多个用于存储每个源服务器的值的变量文件，并在命令行上使用 **-v** 开关来引用这些文件。 这有助于在包含多个变量文件中的变量值的几个脚本文件中维护静态值。  
  
> [!NOTE]  
> 1.  变量名称以 $ (美元) 符号为前缀并带有后缀。 如果变量值文件中没有为变量赋值，则在分析脚本文件时将遇到错误，从而导致控制台执行过程停止。  
> 2.  的转义符 **$** 是 **$$** 。 如果参数的变量或静态值的值包含 **$** (美元) 符号，则 **$$** 必须指定，以将其视为字符而不是变量。  
> 3.  出于可维护性目的，可以在元素内声明变量 `'variable-group'` 以实现用户定义变量的逻辑分离。  此元素不是必需的。  
  
**示例：**  
  
**示例 1：**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="ProjectSpecs">  
  
    <variable name="$project_folder$" value="<folder-name>"/>  
  
    <variable name="$project_name$" value="<project-name>"/>  
  
    <variable name="$project_overwrite$" value="<true/false>"/>  
  
    <variable name="$project_type$" value="<project-type>"/>  
  
  </variable-group>  
  
</variables>  
```  
**示例 2：**  
  
```xml  
<!--Sample of variable value file commands-->  
  
<variables>  
  
  <variable-group name="SQLServerParams">  
  
    <variable-group name="SqlServerConnectionParams">  
  
      <variable name="$TargetUserName$ value="<user-name>"/>  
  
      <variable name="$TargetServerName$" value="<server-name>"/>  
  
      <variable name="$TargetDB$" value="<database-name>"/>  
  
      <variable name="$TargetPassword$" value="<password>"/>  
  
      <variable name="$TrustedConnection$" value="<true/false>"/>  
  
    </variable-group>  
  
    <variable-group name="SqlServerObjectParams">  
  
      <variable name="$ObjectName1$" value="<object-name>"/>  
  
      <variable name="$ObjectName2$" value="<object-name>"/>  
  
    </variable-group>  
  
  </variable-group>  
  
</variables>  
```  
  
## <a name="variable-value-file-validation"></a>变量值文件验证  
用户可以根据 "架构" 文件夹中提供的架构定义文件 **"ConsoleScriptVariablesSchema"** 轻松地验证其变量值文件。  
  
## <a name="next-step"></a>下一步  
操作控制台的下一步是 [&#40;MySQLToSQL 创建服务器连接文件&#41;](../../ssma/mysql/creating-the-server-connection-files-mysqltosql.md)  
  
## <a name="see-also"></a>另请参阅  
[ (MySQL 创建服务器连接文件) ](./creating-the-server-connection-files-mysqltosql.md)  
