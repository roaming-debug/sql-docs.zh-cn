---
description: " ()  (DB2ToSQL 加载系统对象的项目设置) "
title: " (加载系统对象)  (DB2ToSQL) 的项目设置 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
ms.assetid: 9a545233-1b0a-488a-a1ec-c33aa608dcc1
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 79f359fa2a45db75fd48a6f9bbcbb774044c942a
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100072241"
---
# <a name="project-settingsloading-system-objects-db2tosql"></a> ()  (DB2ToSQL 加载系统对象的项目设置) 
您可以在 " **项目设置** " 对话框的 "加载系统对象" 页中指定 SSMA 转换和加载到哪些 DB2 系统对象 [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] 。  
  
" **项目设置** " 和 " **默认项目设置** " 对话框中提供了 "加载系统对象" 窗格：  
  
-   若要指定所有 SSMA 项目的设置，请在 " **工具** " 菜单上，选择 " **默认项目设置**"，从 " **迁移目标版本** " 下拉菜单中选择需要查看或更改其设置的迁移项目类型，然后单击左窗格底部的 " **常规** "，然后单击 " **加载系统对象**"。  
  
-   若要指定当前项目的设置，请在 " **工具** " 菜单上选择 " **项目设置**"，单击左侧窗格底部的 " **常规** "，然后单击 " **加载系统对象**"。  
  
## <a name="default-settings"></a>默认设置  
转换系统对象会消耗系统资源并花费时间。 为了提高性能，SSMA 仅选择最常使用的系统对象，如下面的列表所示：  
  
-   系统.DBMS_OUTPUT  
  
-   系统.DBMS_PIPE  
  
-   系统.DBMS_UTILITY  
  
-   系统.标准  
  
-   系统.UTL_FILE  
  
-   系统.DBMS_LOB  
  
-   系统.DBMS_SQL  
  
-   系统.DBMS_SESSION  
  
如果 DB2 对象引用其他系统对象，则应选择这些对象。 如果不选择 DB2 数据库对象引用的系统对象，SSMA 将报告转换错误。 如果收到由于缺少系统对象而导致的转换错误，请在此对话框中选择缺少的对象。 然后，可以根据需要重复转换。  
  
