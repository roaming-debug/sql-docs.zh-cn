---
title: 在 Azure 虚拟机上部署 SQL Server
description: 本教程介绍如何在 Azure 虚拟机上创建 SQL Server
author: ninarn
ms.author: ninarn
ms.reviewer: alayu, maghan
ms.topic: tutorial
ms.prod: azure-data-studio
ms.technology: azure-data-studio
ms.custom: ''
ms.date: 10/14/2020
ms.openlocfilehash: c5cffc4964e7a91b3b63da56e43e01db41bbe8a9
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048627"
---
# <a name="create-sql-server-on-azure-virtual-machines-using-azure-data-studio"></a>使用 Azure Data Studio 在 Azure 虚拟机上创建 SQL Server

可以使用 Azure Data Studio 通过部署向导和笔记本创建 SQL 虚拟机 (VM)。

## <a name="pre-requisites"></a>先决条件

- 已安装 [Azure Data Studio](download-azure-data-studio.md)
- 有效的 Azure 帐户和订阅。 如果没有帐户，请[创建一个免费帐户](https://azure.microsoft.com/free/)。

## <a name="use-the-deployment-wizard"></a>使用部署向导

按照以下步骤使用部署向导，它将指导你在简单的 UI 体验中完成所需设置。

首先，在部署向导中找到并选择“Azure SQL VM”。

1. 在 Azure Data Studio 中，在左侧导航栏中选择“连接 Viewlet”。

2. 选择“连接”面板顶部的“...”按钮，然后选择“新建部署” 。

3. 在部署向导中，选择“Azure SQL VM”磁贴，并选中“接受条款”复选框

4. 如果系统提示，请安装所需的工具，然后选择底部的“选择”按钮。

接下来，在 Azure SQL VM 向导中输入所有必需的参数。

1. 登录到 Azure 帐户（如果尚未登录）。 如果向导的此页上出现问题，可以刷新连接。

2. 选择所需的订阅、资源组和区域。 然后，选择“下一步”。

3. 输入唯一的虚拟机名称、用户名以及密码凭据。

4. 选择首选的映像、SKU 和版本，然后选择首选的 VM 大小。 了解[可用 VM 大小](/azure/virtual-machines/sizes)的详细信息，以帮助你进行选择。 然后，选择“下一步”。

5. 从下拉列表中选择现有虚拟网络，或选中“新建虚拟网络”复选框，输入新虚拟网络的名称。

6. 选择或创建子网和公共 IP 地址时，请执行相同的操作。

7. 如果要通过远程桌面 (RDP) 连接到 VM，请选中“启用 RDP 入站端口”复选框。 然后，选择“下一步”。

8. 输入首选的 SQL Server 设置。 测试此体验的建议是，将 SQL 连接设置为公共 (Internet)，输入端口 1433，并使用首选用户名和密码启用 SQL 身份验证。 然后，选择“下一步”。

9. 查看已输入的参数，然后选择“脚本到笔记本”。

笔记本打开后，你可以查看内容和代码，并根据需要进行更改。 但是建议不要进行更改，因为这可能会导致验证错误。

最后一步是选择“全部运行”，以运行笔记本中的所有单元格。 完成此操作后，你应该已完全创建并可以运行：

- Azure 虚拟机
- SQL 虚拟机
- 虚拟网络、子网和公共 IP 地址
- 网络安全组和网络接口
- 访问 VM 中的 RDP
- 访问各种 SQL Server 可管理性功能，以轻松控制备份计划、修补计划、SQL Server 版本和许可、存储性能配置等。

## <a name="next-steps"></a>后续步骤

若要详细了解如何将数据迁移到新的 SQL VM，请参阅以下文章。

> [!div class="nextstepaction"]
> [将数据库迁移到 SQL VM](/azure/azure-sql/virtual-machines/windows/migrate-to-vm-from-sql-server)

有关在 Azure 中使用 SQL Server 的其他信息，请参阅 [Azure 虚拟机上的 SQL Server](/azure/azure-sql/virtual-machines/windows/sql-server-on-azure-vm-iaas-what-is-overview) 和[常见问题](/azure/azure-sql/virtual-machines/windows/frequently-asked-questions-faq)。