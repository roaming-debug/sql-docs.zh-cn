---
description: " (迁移的项目设置)  (AccessToSQL) "
title: " (迁移的项目设置)  (AccessToSQL) |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Migration settings
- Project Settings dialog box, Migration
ms.assetid: 4caebc9c-8680-4b99-a8fa-89c43161c95d
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: e1771bebb7619cbbc752485fb649331dab62d0dc
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100066407"
---
# <a name="project-settings-migration-accesstosql"></a> (迁移的项目设置)  (AccessToSQL) 
通过迁移项目设置，你可以配置如何将数据迁移到 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 或 SQL Azure。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "迁移" 窗格。  
  
-   使用 " **项目设置** " 对话框可以设置当前项目的配置选项。 若要访问迁移设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后单击 " **迁移**"。  
  
-   使用 " **默认项目设置** " 对话框可以为所有项目设置配置选项。 若要访问迁移设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，在 " **迁移目标版本** " 组合框中选择要访问设置的项目类型，单击左侧窗格底部的 " **常规** "，然后单击 " **迁移**"。  
  
## <a name="options"></a>选项  
**检查约束**  
指定 SSMA 在向表中添加数据时是否应检查约束。  
  
-   **默认模式**： False  
  
-   **乐观模式**： True  
  
-   **完整模式**： False  
  
**激发触发器**  
指定 SSMA 在向表中添加数据时是否应激发插入触发器 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   **默认模式**： False  
  
-   **乐观模式**： True  
  
-   **完整模式**： False  
  
**保留标识**  
指定 SSMA 在将数据添加到时是否保留访问标识值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。 如果此值为 False，则 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 分配标识值。  
  
-   **默认模式**： True  
  
-   **乐观模式**： True  
  
-   **完整模式**： False  
  
**保留 Null**  
指定 SSMA 在将数据添加到时是否保留源数据中的空值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] ，而不考虑在中指定的默认值 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
-   **默认模式**： True  
  
-   **乐观模式**： False  
  
-   **完整模式**： True  
  
**表锁**  
指定 SSMA 在数据迁移过程中将数据添加到表时是否锁定表。 如果该值为 False，SSMA 将使用行锁。  
  
-   **默认模式**： True  
  
-   **乐观模式**： True  
  
-   **完整模式**： True  
  
**替换不受支持的日期**  
指定 SSMA 是否应更正早于 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 1753 年1月) 之前的日期时间早于最早日期的访问日期 (。  
  
-   若要保留当前日期值，请选择 " **不执行任何操作**"。 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 不会接受日期时间列中01年1月 1753 1 日之前的日期。 如果使用旧日期，则必须将日期时间值转换为字符值。  
  
-   若要将1753年1月之前的日期转换为 NULL，请选择 " **替换为 null**"。  
  
-   若要将1753年1月之前的日期替换为支持的日期，请选择 " **替换为支持的最早日期**"。 如果选择此值，则默认情况下，"支持的最晚日期" 将选为01年1月1753。  
  
**批大小**  
数据迁移期间使用的批大小。 每个批处理后记录一个事务。 默认情况下，所有方案的批大小均为10000。  
  
## <a name="see-also"></a>另请参阅  
[ (访问) 的用户界面参考 ](./user-interface-reference-accesstosql.md)  
