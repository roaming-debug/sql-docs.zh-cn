---
title: 报表设计器中的报表部件 | Microsoft Docs
description: 将分页报表项单独发布到报表服务器或 SharePoint 站点，以便你和他人能在报表生成器的其他报表中重复使用它们。
ms.date: 05/30/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-design
ms.topic: conceptual
f1_keywords:
- sql13.rtp.rptdesigner.components.f1
ms.assetid: 0c34311d-05d6-4bd2-b452-545fa95f8e7f
author: maggiesMSFT
ms.author: maggies
ms.openlocfilehash: 873d299829171f516da4a9eeacce37786b99347b
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100062822"
---
# <a name="report-parts-in-report-designer-ssrs"></a>报表设计器中的报表部件 (SSRS)

  在报表设计器中，在你创建了表、图表和项目中的其他分页报表项后，可以将它们作为“报表部件”发布到报表服务器或与报表服务器相集成的 SharePoint 站点中，以便你和他人可以在其他报表中重复使用它们。  
  
 通常，报表部件在报表设计器中和报表生成器中以相同的方式工作。 若要了解基本功能，请参阅[报表部件（报表生成器和 SSRS）](../../reporting-services/report-design/report-parts-report-builder-and-ssrs.md)。  
  
 但在报表设计器中，报表部件在工作方式上还存在一些基本差异。 一个主要的差异就是工作流。 报表生成器启用协同创作：我创建并发布报表部件。 您可以重复使用、修改和重新发布该报表部件。 在报表设计器中，发布是单向的：我可以从报表设计器发布报表部件，你能重用它。 但是，我不能在报表设计器中在报表中重复使用现有的报表部件。 本主题首先快速介绍一下报表部件，然后详细阐述这些差异。  
  
##  <a name="life-cycle-of-report-part-publishing"></a><a name="ComponentWorkflow"></a> 报表部件发布的生命周期  
 ![rs_ComponentCreation](../../reporting-services/report-design/media/rs-componentcreation.gif "rs_ComponentCreation")  
  
1.  在报表设计器中，人员 A 创建一个含报表的项目，而报表中的图表依赖于某一嵌入数据集。  
  
2.  人员 A 标记该图表及其嵌入的数据集以供发布。 报表设计器向其分配唯一 ID。 然后，人员 A 将该报表发布到报表服务器。 报表设计器发布该图表。  
  
3.  人员 B 在报表生成器中创建一个空白报表，然后向其添加该图表。 该图表现在是人员 B 的报表部件，同时嵌入数据集也是报表部件。 人员 B 可以修改位于报表中的该图表和数据集的实例。 这将不会对报表服务器上图表和数据集的实例产生影响，并且不会破坏报表和报表服务器上实例之间的关系。  
  
     ![rs_BIDScomponentupdate](../../reporting-services/report-design/media/rs-bidscomponentupdate.gif "rs_BIDScomponentupdate")  
  
4.  在报表设计器中，人员 A 修改原始报表中的图表。  
  
5.  人员 A 重新部署该报表，这会将该图表重新发布到服务器，因此更新服务器上的该图表。  
  
6.  在报表生成器中，人员 B 接受来自服务器的更新的图表。 这将覆盖人员 B 已对人员 B 的报表中的图表所做的更改。  
  
##  <a name="publishing-report-parts"></a><a name="PublishingComponents"></a> 发布报表部件  
 在您发布某一报表部件时，报表设计器将向其分配唯一 ID。 之后，它将保持该 ID，无论您对该报表部件进行何种更改。 该 ID 将您的报表中的原始报表项链接到该报表部件。 在其他报表作者在报表生成器中重复使用该报表部件时，该 ID 还将其报表中的报表部件链接到该报表部件。  
  
 以下是您可以作为报表部件发布的报表项：  
  
-   图表  
  
-   仪表  
  
-   图像和嵌入图像  
  
-   地图  
  
-   参数  
  
-   矩形  
  
-   表  
  
-   矩阵  
  
-   列表  
  
 如果您要发布显示数据（例如表、矩阵或图表）的某一报表部件，则可以将该报表部件基于某一共享数据集；否则，在您发布该报表部件时，它所依赖的数据集将作为嵌入数据集保存。 嵌入数据集可以基于嵌入数据源，但凭据不存储于嵌入数据源中。 因此，如果您的报表部件依赖于使用某一嵌入数据源的嵌入数据集，则重复使用此报表部件的任何人都将需要为该嵌入数据源提供凭据。 若要避免这种情况，请将您的嵌入数据集和共享数据集基于具有存储的凭据的共享数据源。 有关详细信息，请参阅 [报表生成器中的报表部件和数据集](../../reporting-services/report-data/report-parts-and-datasets-in-report-builder.md)。  
  
 在报表设计器中发布报表部件的过程分为以下两步：  
  
1.  在 **“发布报表部件”** 对话框中标记要发布的报表项。  
  
2.  部署报表。  
  
 在您部署报表时，报表部件将发布到某一 SharePoint 站点或报表服务器，并且其他人可以重复使用它。 若要发布某一报表部件，在部署报表时，必须具有与某一报表服务器的连接并且对其具有足够的权限。  
  
  
##  <a name="reusing-report-parts"></a><a name="SearchReuseComponents"></a> 重复使用报表部件  
 与报表生成器中不同，如果某一项目并非您在其中创建了报表部件的项目，则不能在该项目中搜索和重复使用该报表部件。  
  
 使用报表生成器的报表作者中可以搜索和重复使用您在他们所创建的报表中发布的报表部件。  
  
##  <a name="republishing-report-parts"></a><a name="RepublishingComponents"></a> 重新发布报表部件  
 在报表设计器中，您应该从创建了现有报表部件的报表内更新该报表部件。 在报表生成器中，报表作者可以重复使用该报表部件，并且可以将其作为新的报表部件发布而无需替换您发布的报表部件。 如果他们有足够的权限，也可以更新您发布的报表部件。 对网站或服务器上的文件夹具有足够权限的任何人也可以更新存储在那里的报表部件。 最后的更新会覆盖以前的更新。  
  
 您可以修改报表部件，然后将其重新发布到网站或服务器上。 对于将该报表部件添加到某一报表中的报表生成器报表作者，系统将在他们下次打开该报表时向他们通知所做更改。 他们可以选择接受或拒绝您的更改。  
  
 您还可以选择将已经发布的报表部件作为新报表部件发布。 在“发布报表部件”对话框中，单击“作为新报表部件发布”。 此新报表部件具有新的唯一 ID，并与旧的报表部件没有关系。  

## <a name="next-steps"></a>后续步骤

[管理报表部件](../../reporting-services/report-design/managing-report-parts.md)  

更多疑问？ [请访问 Reporting Services 论坛](https://go.microsoft.com/fwlink/?LinkId=620231)
