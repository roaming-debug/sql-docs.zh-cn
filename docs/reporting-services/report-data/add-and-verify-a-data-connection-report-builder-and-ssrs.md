---
title: 添加和验证数据连接（报表生成器）| Microsoft Docs
description: 了解如何使用报表生成器添加和验证数据连接，以验证指定的凭据是否足够。
ms.prod: reporting-services
ms.prod_service: reporting-services-native
ms.technology: report-data
ms.topic: conceptual
author: maggiesMSFT
ms.author: maggies
ms.reviewer: ''
ms.custom: ''
ms.date: 12/13/2020
ms.openlocfilehash: b99ed7142dece1c256962b21dacf46bc78470967
ms.sourcegitcommit: 3bd188e652102f3703812af53ba877cce94b44a9
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/15/2020
ms.locfileid: "97489847"
---
# <a name="add-and-verify-a-data-connection-report-builder-and-ssrs"></a>添加和验证数据连接（报表生成器和 SSRS）

在报表生成器中，您可以从报表服务器添加共享数据源或为您的报表创建嵌入数据源。 在报表设计器中，您可以创建共享数据源或嵌入数据源，并且将其部署到报表服务器上。

若要将共享数据源添加到报表，请浏览报表服务器并选择共享数据源。 报表中的共享数据源指向报表服务器上的共享数据源定义。

若要创建嵌入数据源，您必须具有到外部数据源的连接信息并知道需要何种权限才能访问数据。 此信息通常来自数据源的所有者。 可以测试该连接以验证指定的凭据是否有效。

有关详细信息，请参阅[创建数据连接字符串 - 报表生成器和 SSRS](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md) 和[在报表生成器中指定凭据](./specify-credential-and-connection-information-for-report-data-sources.md)

> [!NOTE]  
> [!INCLUDE[ssRBRDDup](../../includes/ssrbrddup-md.md)]

## <a name="to-create-a-connection-to-a-shared-data-source-in-report-builder"></a>在报表生成器中创建共享数据源的连接的步骤

1. 在“报表数据”窗格的工具栏上，单击 **“新建”** ，然后单击 **“数据源”** 。 此时将打开 **“数据源属性”** 对话框。

2. 在 **“名称”** 文本框中，键入数据源的名称。

    > [!NOTE]  
    >  此名称保存在本地报表定义中， 它不是报表服务器上共享数据源的名称。 

3. 选择 **“使用共享连接或报表模型”** 。 将显示最近使用的共享数据源和报表模型的列表。 若要从报表服务器选择一个共享数据源，请单击 **“浏览”** 并找到报表服务器上包含共享数据源的文件夹。

4. 选择该共享数据源，然后单击 **“打开”** 。

5. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

数据源将显示在“报表数据”窗格中。

### <a name="to-verify-a-data-connection"></a>验证数据连接  

1. 在“报表数据”窗格的工具栏上，双击该数据源。 此时将打开 **“数据源属性”** 对话框。

2. 单击 **“测试连接”** 。

3. 如果连接成功，则显示以下消息：“已成功创建连接”。 [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

4. 如果连接不成功，则显示以下消息：“无法连接到数据源”。  

5. 单击 **“详细信息”** ，然后使用该信息来解决问题。

    有关详细信息，请参阅 [在报表生成器中指定凭据](./specify-credential-and-connection-information-for-report-data-sources.md)。

6. [!INCLUDE[clickOK](../../includes/clickok-md.md)]  

## <a name="see-also"></a>另请参阅

- [报表数据集 (SSRS)](../../reporting-services/report-data/report-datasets-ssrs.md)   
- [报表的嵌入数据集和共享数据集（报表生成器和 SSRS）](../../reporting-services/report-data/report-embedded-datasets-and-shared-datasets-report-builder-and-ssrs.md)
- [查找、查看和管理报表（报表生成器和 SSRS）](../../reporting-services/report-builder/finding-viewing-and-managing-reports-report-builder-and-ssrs.md)
- [创建数据连接字符串 - 报表生成器和 SSRS](data-connections-data-sources-and-connection-strings-report-builder-and-ssrs.md)
