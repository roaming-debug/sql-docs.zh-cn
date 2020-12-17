---
title: SQL Server 2019 发行说明 | Microsoft Docs
description: 查找有关 SQL Server 2019 (15.x) 的限制、已知问题、帮助资源和其他发行说明的信息。
ms.date: 11/04/2019
ms.prod: sql
ms.reviewer: ''
ms.technology: release-landing
ms.topic: article
ms.assetid: 13942af8-5a40-4cef-80f5-918386767a47
author: MikeRayMSFT
ms.author: mikeray
monikerRange: = sql-server-ver15
ms.openlocfilehash: 9762de193eae8ad4e67e77ae54c9778e92357c45
ms.sourcegitcommit: 1a544cf4dd2720b124c3697d1e62ae7741db757c
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 12/14/2020
ms.locfileid: "97402590"
---
# <a name="sql-server-2019-release-notes"></a>[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 发行说明
[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍了 [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 的限制和已知问题。 如需相关信息，请参阅：

> [[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 中的新增功能](../sql-server/what-s-new-in-sql-server-ver15.md)

## [!INCLUDE[sql-server-2019](../includes/sssqlv15-md.md)]

[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 是 [!INCLUDE[SQL Server 2019](../includes/ssnoversion-md.md)] 最新的公开版本。

有关授权的完整详细信息位于安装介质的 `License Terms` 文件夹中。

## <a name="documentation"></a>文档

- 问题和对客户的影响  ：可按版本筛选 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 文档。 使用每个文档页左上角的控件来筛选你的要求。

## <a name="build-number"></a>生成号

SQL Server 2019 的 RTM 生成号为 `15.0.2000.5`。

[!INCLUDE [sql-server-servicing-updates-version-15](../includes/sql-server-servicing-updates-version-15.md)]

## <a name="sql-server-installation-may-fail-if-ssms-18x-is-installed"></a>如果已安装 SSMS 18.x，SQL Server 安装可能会失败

- **问题及其对客户的影响**：如果按以下顺序进行以下安装，[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 安装将失败：
  1. 在服务器上安装了 SQL Server Management Studio (SSMS) 版本 18.0、18.1、18.2 或 18.3。
  1. [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] 安装是从可移动介质尝试进行的。 例如，安装介质是 DVD。

- **解决方法**：
  1. 卸载早于 SSMS 18.3.1 的任何 SSMS 版本。
  1. 安装更新版本的 SSMS（18.3.1 或更高版本）。 对于最新版本，请参阅[下载 SSMS](../ssms/download-sql-server-management-studio-ssms.md)。
  1. 正常安装 [!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)]。

- 适用于  ：[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)]

## <a name="utf-8-collations"></a>UTF-8 排序规则

- **问题及其对客户的影响**：支持 UTF-8 的排序规则不能与以下功能一起使用：
  - [内存中 OLTP 表](../relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables.md)
  - [具有安全 Enclave 的 Always Encrypted](../relational-databases/security/encryption/always-encrypted-enclaves.md)（不能使用安全 Enclaves 时，[Always Encrypted](../relational-databases/security/encryption/always-encrypted-database-engine.md) 可以使用 UTF-8）

  > [!WARNING]
  > 创建数据库的 [bacpac](../relational-databases/data-tier-applications/data-tier-applications.md#bacpac) 将失败，该数据库包含定义为使用超过 4000 个字节的 [CHAR 或 VARCHAR](../t-sql/data-types/char-and-varchar-transact-sql.md) 的表列。
  
  > [!NOTE]
  > 目前没有 UI 支持在 Azure Data Studio 或 SQL Server Data Tools (SSDT) 中选择支持 UTF-8 的排序规则。 最新的 [!INCLUDE[ssManStudioFull](../includes/ssmanstudiofull-md.md)] SSMS 版本 18 支持在 UI 中选择支持 UTF-8 的排序规则。

- **适用对象**：[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] RTM

## <a name="master-data-service-notification-email-contains-broken-link"></a>Master Data Service 通知电子邮件包含断开的链接

- **问题及其对客户的影响**：Master Data Services (MDS) 发出的通知电子邮件包含断开的链接。 通过链接导航到的页面返回与以下消息类似的错误：

   `The view 'Index' or its master was not found or no view engine supports the searched locations.`

- **解决方法**：打开 MDS 门户，并手动转到资源。

- **适用对象**：[!INCLUDE[SQL Server 2019](../includes/sssqlv15-md.md)] RTM

## <a name="see-also"></a>另请参阅

- [安装 SQL Server 的硬件和软件要求](../sql-server/install/hardware-and-software-requirements-for-installing-sql-server-ver15.md)

## <a name="machine-learning-services"></a>机器学习服务

有关 SQL Server 机器学习服务中的问题，请参阅 [SQL Server 机器学习服务中的已知问题](../machine-learning/troubleshooting/known-issues-for-sql-server-machine-learning-services.md)。

[!INCLUDE[get-help-options-msft-only](../includes/paragraph-content/get-help-options.md)]
