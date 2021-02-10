---
description: OLE DB 提供程序 (ADO)
title: ADO) OLE DB 提供程序 (|Microsoft Docs
ms.prod: sql
ms.prod_service: connectivity
ms.technology: ado
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.topic: conceptual
helpviewer_keywords:
- OLE DB providers [ADO]
- ADO, OLE DB providers
ms.assetid: 6e0488c3-934d-4976-99dc-65c580dc7a3c
author: rothja
ms.author: jroth
ms.openlocfilehash: 7e70762a2cbaa6e2dd3d8d91b824591799605748
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100032659"
---
# <a name="ole-db-providers-ado"></a>OLE DB 提供程序 (ADO)
OLE DB 定义一组 COM 接口，以使应用程序能够统一访问存储在不同信息源中的数据。 此方法允许数据源通过支持适用于数据源的 DBMS 功能量的接口来共享其数据。 按照设计，OLE DB 的高性能体系结构基于基于组件的灵活服务模型的使用方式。 OLE DB 只需要满足特定任务所需的组件数量，而不是在应用程序和数据之间采用指定数目的中间层。  
  
 例如，假设用户要运行查询。 请考虑下列情形：  
  
-   数据驻留在一个关系数据库中，该数据库当前存在一个 ODBC 驱动程序，但没有本机 OLE DB 提供程序：该应用程序使用 ADO 与用于 ODBC 的 OLE DB 提供程序进行通信，后者随后加载相应的 ODBC 驱动程序。 驱动程序将 SQL 语句传递给 DBMS，后者检索数据。  
  
-   数据驻留在具有本机 OLE DB 提供程序 Microsoft SQL Server 中：应用程序使用 ADO 直接与 OLE DB 提供程序进行通信以实现 Microsoft SQL Server。 不需要任何中介。  
  
-   数据驻留在 Microsoft Exchange Server 中，其中有一个 OLE DB 提供程序，但没有公开引擎来处理 SQL 查询：应用程序使用 ADO 与 Microsoft Exchange 的 OLE DB 提供程序进行通信，并调用 OLE DB 查询处理器组件来处理查询。  
  
-   数据以文档的形式驻留在 Microsoft NTFS 文件系统中：通过 Microsoft 索引服务使用本机 OLE DB 提供程序访问数据，这会对文件系统中文档的内容和属性进行索引以实现高效的内容搜索。  
  
 在上述所有示例中，应用程序都可以查询数据。 使用最少数量的组件满足用户的需求。 在每种情况下，仅在需要时才使用其他组件，而只会调用所需的组件。 当使用 OLE DB 时，这种可重用和可共享组件的请求加载极大地提高了性能。  
  
 提供程序分为两类：提供数据的提供程序和提供服务的提供程序。 数据访问接口拥有自己的数据，并以表格形式向应用程序公开该数据。 服务提供程序通过生成和使用数据并在 ADO 应用程序中扩充功能来封装服务。 服务提供商还可以进一步定义为服务组件，该组件必须与其他服务提供商或组件一起使用。  
  
 ADO 为不同的 OLE DB 提供程序提供了一种一致的更高级别接口。  
  
 本部分包含以下主题。  
  
-   [数据提供程序](./data-providers.md)  
  
-   [服务提供商和组件](./service-providers-and-components.md)