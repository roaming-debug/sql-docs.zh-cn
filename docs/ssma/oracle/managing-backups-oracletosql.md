---
description: 管理备份 (OracleToSQL)
title: " (OracleToSQL) 管理备份 |Microsoft Docs"
ms.prod: sql
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.technology: ssma
ms.topic: conceptual
helpviewer_keywords:
- Oracle Backup Management
- SQL Server Backup Management
ms.assetid: a1a03ef9-b6e8-4127-bad0-eae261251472
author: nahk-ivanov
ms.author: alexiva
manager: alexiva
ms.openlocfilehash: 65efad5247054ec308383074be71e7c75ef4694c
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100080738"
---
# <a name="managing-backups-oracletosql"></a>管理备份 (OracleToSQL)
Oracle 备份管理允许您在运行测试之前或之后备份和还原表数据。 还可以通过 "管理备份内容" 对话框管理备份内容。  
  
## <a name="oracle-backup-management"></a>Oracle 备份管理  
  
### <a name="backup"></a>备份  
若要打开 "备份" 对话框，请在 "测试人员" 菜单上指向 "Oracle 备份管理"，然后单击 "备份 ..."。在备份对话框中，你将找到 Oracle 元数据树，其中显示了已加载的 Oracle 架构的所有表。 选择要执行备份的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **备份** " 按钮以备份表的数据。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="restore"></a>还原  
若要打开 "还原" 对话框，请在 "测试人员" 菜单上指向 "Oracle 备份管理"，然后单击 "还原 ..."。可在其中找到包含备份中可用表的树。 选择要还原其数据的一个或多个表。  
  
对话框中提供了以下按钮：  
  
-   单击 " **检查状态** " 按钮，查看表的备份状态。  
  
-   单击 " **还原** " 按钮，将备份数据还原到表中。  
  
-   单击 " **取消** " 按钮以关闭对话框。  
  
### <a name="managing-backup-contents"></a>管理备份内容  
若要打开管理备份内容，请在 "测试器" 菜单上指向 "Oracle 备份管理"，然后单击 "备份内容 ..."。可以在其中找到包含备份中的表的树。  
  
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
[测试迁移的数据库对象 &#40;OracleToSQL&#41;](../../ssma/oracle/testing-migrated-database-objects-oracletosql.md)  
  
