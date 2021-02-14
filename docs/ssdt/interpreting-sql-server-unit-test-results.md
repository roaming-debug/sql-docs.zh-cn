---
title: 解释 SQL Server 单元测试结果
description: 了解如何使用测试结果窗口或 .trx 文件来查看 SQL Server 单元测试结果。 查看如何获取有关结果的详细信息。
ms.prod: sql
ms.technology: ssdt
ms.topic: conceptual
ms.assetid: fde3c95b-2f68-483d-a197-0f7161b72fa3
author: markingmyname
ms.author: maghan
ms.reviewer: “”
ms.custom: seo-lt-2019
ms.date: 02/09/2017
ms.openlocfilehash: 88fc08dcc00e64c4b107a99608b891a43f236d66
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100017907"
---
# <a name="interpreting-sql-server-unit-test-results"></a>解释 SQL Server 单元测试结果

运行 SQL Server 单元测试时，将会自动生成测试结果，将其保存到磁盘并在“测试结果”窗口中显示摘要信息。 启动测试运行后，“测试结果”窗口将出现并显示测试运行的进度。 此显示内容包括正在运行的测试和已完成的测试。  
  
若要查看有关某个测试结果的详细信息，请在“测试结果”窗口中双击该测试结果以显示“测试结果详细信息”页。 有关测试结果的详细信息，请双击测试结果。  
  
有关如何更改“测试结果”窗口的显示的详细信息，请参阅[如何：在测试窗口中添加或移除列 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182508(v=vs.100))或[如何：在测试窗口中添加或移除列 (Visual Studio 2012)](/previous-versions/visualstudio/visual-studio-2012/ms182508(v=vs.110))。  
  
## <a name="storing-test-results"></a>存储测试结果  
单元测试的结果将会自动存储在硬盘上文件扩展名为 .trx 的文件中。 .trx 文件是包含测试运行的详细信息的 XML 文件。 您可加载之前的测试运行的 .trx 文件以查看这些测试运行的结果或重新运行之前的测试。 有关详细信息，请参阅[操作说明：重新运行测试 (Visual Studio 2010)](/previous-versions/visualstudio/visual-studio-2010/ms182472(v=vs.100))。  
  
> [!NOTE]  
> 不能远程运行单元测试。  
  
如果团队使用 Visual Studio Team Foundation Server 团队项目来帮助管理团队工作，则还可将测试数据发布到称为“操作存储区”的 SQL Server 数据库。  
  
有关如何保存测试结果、重用测试结果以及与团队共享测试结果的详细信息，请参阅[如何：在 Visual Studio 2010 中保存和打开测试结果](/previous-versions/visualstudio/visual-studio-2010/ms404662(v=vs.100))或[如何：在 Visual Studio 2012 中保存和打开测试结果](/previous-versions/ms404662(v=vs.140))。  
  
## <a name="see-also"></a>另请参阅  
[运行 SQL Server 单元测试](../ssdt/running-sql-server-unit-tests.md)  
