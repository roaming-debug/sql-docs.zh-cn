---
description: “项目属性”对话框
title: “项目属性”对话框 | Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: integration-services
ms.reviewer: ''
ms.technology: integration-services
ms.topic: conceptual
f1_keywords:
- sql13.ssis.ssms.isprojectprop.general.f1
- sql13.ssis.ssms.isprojectprop.permissions.f1
ms.assetid: d5cf52f5-1fe2-438a-98a3-fe117360acf8
author: chugugrace
ms.author: chugu
ms.openlocfilehash: a1be2d0dc581e67db28be7f5c1b96efc6850e609
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100349165"
---
# <a name="project-properties-dialog-box"></a>“项目属性”对话框

[!INCLUDE[sqlserver-ssis](../../includes/applies-to-version/sqlserver-ssis.md)]


  一个 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 项目就是一个部署单元。 每个项目都可以包含包、参数和环境引用。 项目是安全对象并且可为数据库主体定义权限。 在重新部署某一项目时，该项目的之前版本可以存储在 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 目录中。  
  
 项目参数和包参数可用于在执行时向包内的属性赋值。 某些参数要求首先具有值，之后才能运行包。 引用环境变量的参数值要求项目在执行前具有相应的环境引用。  
  
 **您希望做什么？**  
  
-   [打开“项目属性”对话框](#open_dialog)  
  
-   [设置“常规”页上的选项](#general)  
  
-   [设置“权限”页上的选项](#permissions)  
  
##  <a name="open-the-project-properties-dialog-box"></a><a name="open_dialog"></a> 打开“项目属性”对话框  
  
1.  在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中，连接到 [!INCLUDE[ssISnoversion](../../includes/ssisnoversion-md.md)] 服务器。  
  
     正在连接到承载 SSISDB 数据库的 [!INCLUDE[ssDEnoversion](../../includes/ssdenoversion-md.md)] 的实例。  
  
2.  在对象资源管理器中，展开树以便显示 **“Integration Services 目录”** 节点。  
  
3.  展开 **“SSISDB”** 节点。  
  
4.  展开包含您要设置其属性的项目的文件夹。  
  
5.  右键单击该项目，然后单击“属性”。  
  
##  <a name="set-the-options-on-the-general-page"></a><a name="general"></a> 设置“常规”页上的选项  
 使用“常规”页查看项目属性。  
  
 **名称**  
 列出项目名称。  
  
 **标识符**  
 列出项目 ID。  
  
 **说明**  
 显示项目的可选说明。  
  
 **项目版本**  
 列出项目版本。  
  
 **部署日期**  
 列出部署或重新部署项目的日期和时间。  
  
##  <a name="set-the-options-on-the-permissions-page"></a><a name="permissions"></a> 设置“权限”页上的选项  
 使用 **“权限”** 页可以查看和设置项目的显式权限。  
  
 浏览  
 使用“浏览所有主体”对话框，单击“浏览”选择要为其设置权限的用户和角色。  
  
 **名称**  
 列出用户或角色的名称。  
  
 **类型**  
 列出用户或角色的类型。  
  
 **权限**  
 列出权限。  
  
 **授权者**  
 列出授予权限的用户或角色。  
  
 **授予**  
 选择“授予”时，为所选用户或角色授予权限。  
  
 **拒绝**  
 选择“拒绝”时，为所选用户或角色拒绝权限。  
  
  
