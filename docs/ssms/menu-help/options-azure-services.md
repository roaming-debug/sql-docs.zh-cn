---
title: SQL Server 选项页 - Azure 服务
description: 选项（Azure 服务）
ms.prod: sql
ms.prod_service: sql-tools
ms.technology: ssms
ms.topic: conceptual
f1_keywords:
- VS.ToolsOptionsPages.Azure_Services.Azure_Cloud
dev_langs:
- TSQL
author: markingmyname
ms.author: maghan
ms.date: 01/15/2021
ms.openlocfilehash: 7e4bb93f4a3a992c5033061ca8a2e0209125a5fa
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100076618"
---
# <a name="options-azure-services"></a>选项（Azure 服务）

使用此页指定与 Azure 云服务相关的选项。 若要访问此对话框，请从顶部菜单栏中依次进入“工具”>“选项”>“Azure 服务” 。

## <a name="miscellaneous"></a>杂项

| 选项 | 信息 | 说明 |
|--------|-------------|-------------|
| ADAL 输出窗口跟踪级别 | **信息** <br> 无  <br> **详细** <br> **警告** | 要发送到“输出”窗口的 Azure Active Directory (AAD) 登录跟踪的级别。 |
| Azure 数据工厂门户 URL | `https://adf.azure.com` | 指定 Azure 数据工厂门户的 URL。 |
| 启用对 Azure SQL 数据库的条件访问（实验性） | **True** <br> **False** | 试验：如果为 true，则请求 Azure SQL 数据库的访问令牌，其中包含对所选服务器的访问权的声明。 设置可能需要重启 SSMS 才能生效。 |
| 库终结点 | `https://gallery.azure.com` | 指定部署模板的资源管理器库的终结点。 |
| Graph 访问群体 | `https://graph.windows.net` | Graph 终结点资源 ID。 |
| Graph 终结点 | `https://graph.windows.net` | 指定 Azure Active Directory Graph 请求的 URL。 |
| 管理门户 URL | `https://portal.azure.com` | 指定管理门户的 URL。 |
| 发布设置文件 URL | `https://go.microsoft.com/fwlink/?LinkID=335839` | 指定可下载 `.publishsettings` 文件的 URL。 |
| SQL 数据库服务主体名称 | `https://database.windows.net/` | 使用 AAD 身份验证时，使用 Azure SQL 数据库 SPN 获取令牌。 此外，还提供了用于服务器端 JSON Web 令牌 (JWT) 分析/验证的 JSON Web 令牌 (JWT) 的访问群体。 |

## <a name="resource-management"></a>资源管理

| 选项 | 信息 | 说明 |
|--------|-------------|-------------|
| Active Directory Authority | `https://login.microsoftonline.com` | 指定 Azure Active Directory (AAD) 身份验证的基本授权。 |
| Active Directory 服务终结点资源 ID | `https://management.core.windows.net` | 指定对 Azure 资源管理器 (ARM) 或服务管理 (RDFE) 终结点的请求进行身份验证的令牌的访问群体。 |
| 资源管理器终结点 | `https://management.azure.com` | 指定资源管理请求的 URL。 |
| 服务终结点 | `https://management.core.windows.net` | 指定服务管理请求的终结点。 |

## <a name="select-an-azure-cloud"></a>选择 Azure 云

| 选项 | 信息 | 说明 |
|--------|-------------|-------------|
| 名称 | **Azure 中国云** <br><br> **Azure 云** <br><br> **Azure 德国云** <br><br> **Azure 美国政府版** <br><br> **自定义** | 选择 Management Studio 连接到的 Azure 云，以发现和管理资源。 选择“自定义”以提供你自己的 URL 和 DNS 后缀。 |

## <a name="service-addresses"></a>源地址

| 选项 | 信息 | 说明 |
|--------|-------------|-------------|
| Azure Data Lake 服务终结点 | `azuredatalakeanalytics.net` | 指定 Azure Data Lake Analytics 目录和作业终结点后缀。 |
| Azure Data Lake Store 文件系统终结点 | `azuredatalakestore.net` | 指定 Azure Data Lake Store 文件系统终结点后缀。 | 
| Azure 密钥保管库访问群体 | `https://vault.azure.net` | 指定访问令牌的访问群体，这些令牌授权密钥保管库服务的请求。 |
| Azure 密钥保管库 DNS 后缀 | `vault.azure.net` | 指定 Azure 密钥保管库服务器的域名后缀。 |
| SQL 数据库 DNS 后缀 | `database.windows.net` | 指定 Azure SQL 数据库服务器的域名后缀。 |
| 存储终结点 | `core.windows.net` | 指定用于存储访问的终结点。 该选项包括 blob、表、队列和文件存储。 |
| 流量管理器 DNS 后缀 | `trafficmanager.net` | 指定 Azure 流量管理器服务的域名后缀。 |