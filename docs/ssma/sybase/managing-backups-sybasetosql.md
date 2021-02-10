---
description: 管理备份 (SybaseToSQL)
title: " (SybaseToSQL) 管理备份 |Microsoft Docs"
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Tester Component,Managing Backups
ms.assetid: 266d987c-ecc5-4fa4-bfdf-8c584f1a1332
author: nahk-ivanov
ms.author: alexiva
ms.openlocfilehash: 1c29cec3ec38dc56636df0c3f063f4a758e6e87e
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017297"
---
# <a name="managing-backups-sybasetosql"></a>管理备份 (SybaseToSQL)
使用 Sybase 备份管理，你可以在运行测试之前或之后备份和还原表数据。 还可以通过 "管理备份内容" 对话框管理备份内容。  
  
## <a name="sybase-backup-management"></a>Sybase 备份管理  
  
### <a name="backup"></a>备份  
若要打开 "备份" 对话框，请在 "测试人员" 菜单上指向 "Sybase 备份管理"，然后单击 "备份 ..."。在备份对话框中，你将找到 Sybase 元数据树，其中显示了已加载的 Sybase 架构的所有表。 选择要执行备份的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **备份** " 按钮以备份表的数据。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="restore"></a>还原  
若要打开 "还原" 对话框，请在 "测试人员" 菜单上指向 "Sybase 备份管理"，然后单击 "还原 ..."。可在其中找到包含备份中可用表的树。 选择要还原其数据的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **还原** " 按钮，将备份数据还原到表中。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="managing-backup-contents"></a>管理备份内容  
若要打开管理备份内容，请在 "测试器" 菜单上指向 "Sybase 备份管理"，然后单击 "备份内容 ..."。可以在其中找到包含备份中的表的树。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **删除** " 按钮以从备份中删除该表。  
  
-   单击 " **关闭** " 按钮以关闭对话框。  
  
## <a name="sql-server-backup-management"></a>SQL Server 备份管理  
SQL Server 备份管理，你可以在运行测试之前或之后备份和还原表数据。 还可以通过 "管理备份内容" 对话框管理备份内容。  
  
### <a name="backup"></a>备份  
若要打开 "备份" 对话框，请在 "测试人员" 菜单上指向 "SQL Server 备份管理"，然后单击 "备份 ..."。 在备份对话框中，你将找到 SQL Server 元数据树，其中显示了已加载的 SQL Server 数据库的所有表。 选择要执行备份的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **备份** " 按钮以备份表的数据。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="restore"></a>还原  
若要打开 "还原" 对话框，请在 "测试人员" 菜单上指向 "SQL Server 备份管理"，单击 "还原 ..."。可在其中找到包含备份中可用表的树。 选择要还原其数据的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **还原** " 按钮，将备份数据还原到表中。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="managing-backup-contents"></a>管理备份内容  
若要打开管理备份内容，请在 "测试人员" 菜单上指向 "SQL Server 备份管理"，然后单击 "备份内容 ..."。可以在其中找到包含备份中的表的树。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **删除** " 按钮以从备份中删除该表。  
  
-   单击 " **关闭** " 按钮以关闭对话框。  
  
## <a name="see-also"></a>另请参阅  
[测试迁移的数据库对象 &#40;SybaseToSQL&#41;](../../ssma/sybase/testing-migrated-database-objects-sybasetosql.md)  
  
