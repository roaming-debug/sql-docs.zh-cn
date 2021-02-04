---
description: 启用运行自定义报表警告
title: 启用运行自定义报表警告
ms.custom: seo-lt-2019
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.reviewer: ''
ms.technology: ssms
ms.topic: conceptual
helpviewer_keywords:
- SQL Server Management Studio [SQL Server], custom reports
ms.assetid: 0deed900-c910-4d12-aac0-6ab9e39eb068
author: markingmyname
ms.author: maghan
ms.openlocfilehash: cf87ce7e04e50ea64a6f98db0b29dfc7e90f813d
ms.sourcegitcommit: 38e055eda82d293bf5fe9db14549666cf0d0f3c0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/02/2021
ms.locfileid: "99250486"
---
# <a name="unsuppress-run-custom-report-warnings"></a>启用运行自定义报表警告
[!INCLUDE[SQL Server Azure SQL Database Synapse Analytics PDW ](../../includes/applies-to-version/sql-asdb-asdbmi-asa-pdw.md)]
对于自定义报表，有两个警告对话框。 本主题介绍如何通过使用 [!INCLUDE[ssnoversion](../../includes/ssnoversion-md.md)] 在 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]中取消显示这些对话框。  
  
默认情况下，在运行自定义报表之前会显示“运行自定义报表”  对话框。 如果选中“请不要再显示此警告”  复选框，将不再显示此对话框。 此外，在默认情况下，如果打开一个自定义报表然后单击链接打开另外一个自定义报表，则也将显示此“运行自定义报表”  对话框。 此对话框显示钻取自定义报表文件的填写路径。 如果选中“请不要再显示此警告”  复选框，将不再显示此对话框。  
  
## <a name="using-sql-server-management-studio"></a><a name="SSMSProcedure"></a>使用 SQL Server Management Studio  
  
#### <a name="to-unsuppress-the-main-custom-report-warning-dialog-box"></a>启用主自定义报表警告对话框  
  
1.  连接到 \<*Server*>\\<Share>|\<*Drive*>\Documents and Settings\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml。  
  
2.  右键单击 **reports.xml**，再单击“编辑”  。  
  
3.  将 <SuppressWarning>true\<\/SuppressWarning> 更改为 <SuppressWarning>false\<\/SuppressWarning>。  
  
4.  重新启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
#### <a name="to-unsuppress-the-drill-through-custom-report-warning-dialog-box"></a>启用钻取自定义报表警告对话框  
  
1.  连接到 \<*Server*>\\<Share>|\<*Drive*>\Documents and Settings\\<UserProfile>\Application Data\Microsoft\Microsoft SQL Server\130\Tools\Shell\reports.xml。  
  
2.  右键单击 **reports.xml**，再单击“编辑”  。  
  
3.  将 <SuppressDrillthroughWarning>true\<\/SuppressDrillthroughWarning>更改为 <SuppressDrillthroughWarning>false\<\/SuppressDrillthroughWarning>。  
  
4.  重新启动 [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)]。  
  
## <a name="see-also"></a>另请参阅  
[Management Studio 中的自定义报告](../../ssms/object/custom-reports-in-management-studio.md)  
[向 Management Studio 添加自定义报表](../../ssms/object/add-a-custom-report-to-management-studio.md)  
[将自定义报告与对象资源管理器节点属性一起使用](../../ssms/object/use-custom-reports-with-object-explorer-node-properties.md)  
  
