---
title: 在 Active Directory 模式下进行部署 - 先决条件
titleSuffix: SQL Server Big Data Cluster
description: 配置适用于 SQL Server 大数据群集的 Active Directory
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 09/30/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 3424863924338a0d3f02102609052d6c159e1043
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048007"
---
# <a name="deploy-big-data-clusters-2019-in-active-directory-mode-prerequisites"></a>在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]：Prerequisites

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文档介绍如何准备在 Active Directory 身份验证模式下部署 SQL Server 大数据群集 (BDC)。 群集使用现有 AD 域进行身份验证。

>[!Note]
>在 SQL Server 2019 CU5 版本之前，对大数据群集进行了限制，以便只能针对 Active Directory 域部署一个群集。 此限制已在 CU5 版本中删除，若要了解新功能的详细信息，请参阅[概念：在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)。 本文中的示例进行了调整以适应两种部署用例。

## <a name="background"></a>背景

要启用 Active Directory (AD) 身份验证，BDC 会自动创建群集中各种服务所需的用户、组、计算机帐户和服务主体名称 (SPN)。 要提供对这些帐户的某些控制并允许范围内权限，我们建议在群集部署之前创建一个组织单位 (OU)。 将在部署期间创建所有与 BDC 相关的 AD 对象。 

## <a name="pre-requisites"></a>先决条件

### <a name="organizational-unit-ou"></a>组织单位 (OU)
组织单位 (OU) 是 Active Directory 中放置用户、组，甚至其他组织单位的细分。 大图组织单位可用于镜像组织的功能或业务结构。 本文将创建一个名为 `bdc` 的 OU 作为示例。 

>[!NOTE]
>组织单位 (OU) 表示管理边界，并使客户能够控制数据管理员的授权范围。 

可以按照 [OU 设计原则](/windows-server/identity/ad-ds/plan/reviewing-ou-design-concepts)来决定使用组织中的 OU 的最佳结构。

### <a name="ad-account-for-bdc-domain-service-account"></a>BDC 域服务帐户的 AD 帐户

为了能够自动在 Active Directory 中创建所有必需对象，BDC 需要具有在提供的组织单位 (OU) 内创建用户、组和计算机帐户的特定权限的 AD 帐户。 本文将介绍如何配置此 AD 帐户的权限。 我们使用名为 `bdcDSA` 的 AD 帐户作为本文中的示例。

### <a name="auto-generated-active-directory-objects"></a>自动生成的 Active Directory 对象
BDC 部署会自动生成帐户名和组名。 每个帐户都代表 BDC 中的一个服务，并将在使用 BDC 群集的整个生存期内通过 BDC 进行管理。 这些帐户拥有每个服务所需的服务主体名称 (SPN)。  有关所管理的 AD 自动生成的帐户、组和服务的完整列表，请参阅[自动生成的 Active Directory 对象](active-directory-objects.md)。

>[!IMPORTANT]
>这些帐户的密码可能会过期，具体取决于域控制器中设置的密码过期策略。 默认的过期策略为 42 天。 没有任何机制可以轮换 BDC 中所有帐户的凭据，因此一到过期时间，群集将变为不可操作。 若要解决此问题，请在域控制器中将 BDC 服务帐户的过期策略更新为“密码永不过期”。 此操作可在过期时间之前或之后完成。 在后一种情况下，Active Directory 将重新激活过期的密码。
>
>下图显示了在“Active Directory 用户和计算机”中设置此属性的位置。
>
>:::image type="content" source="media/deploy-active-directory/image25.png" alt-text="设置密码过期策略":::

以下步骤假设已有一个 Active Directory 域控制器。 如果没有域控制器，以下[指南](https://social.technet.microsoft.com/wiki/contents/articles/37528.create-and-configure-active-directory-domain-controller-in-azure-windows-server.aspx)包含可提供帮助的步骤。

## <a name="create-ad-objects"></a>创建 AD 对象

在部署具有 AD 集成的 BDC 之前，请执行以下操作：

1. 创建一个将在其中存储所有与 BDC 相关的 AD 对象的组织单位 (OU)。 还可以选择在部署时选择现有 OU。
1. 为 BDC 创建 AD 帐户或使用现有帐户，并在提供的组织单位 (OU) 中向此 BDC AD 帐户提供正确的权限。

### <a name="create-a-user-in-ad-for-bdc-domain-service-account"></a>为 BDC 域服务帐户在 AD 中创建用户

大数据群集需要具有特定权限的帐户。 在继续操作之前，请确保已有 AD 帐户或创建一个新帐户，大数据群集可以使用该帐户来设置必要的对象。

若要在 AD 中创建新用户，可以右键单击域或 OU，然后选择“新建” > “用户” ：

![Active Directory 用户对话框](./media/deploy-active-directory/image12.png)

此用户在本文中将称为 BDC 域服务帐户。

### <a name="create-an-ou"></a>创建 OU

在域控制器上，打开“Active Directory 用户和计算机”。 在左侧面板上，右键单击要在其下创建 OU 的目录，然后选择“新建”\>“组织单位”，然后按照向导中的提示创建 OU 。 或者，可以使用 PowerShell 创建 OU：

```powershell
New-ADOrganizationalUnit -Name "<name>" -Path "<Distinguished name of the directory you wish to create the OU in>"
```

本文中的示例使用 `bdc` 作为 OU 名称。

![Active Directory 组织单位](./media/deploy-active-directory/image13.png)

![新建对象 - 组织单位](./media/deploy-active-directory/image14.png)

### <a name="set-permissions-for-an-ad-account"></a>设置 AD 帐户的权限

无论是创建新的 AD 用户还是使用现有的 AD 用户，用户都需要具有某些权限。 此帐户是 BDC 控制器在将群集加入 AD 时将使用的用户帐户。

BDC 域服务帐户 (DSA) 需要能够在 OU 中创建用户、组和计算机帐户。 在下面的步骤中，我们将 BDC 域服务帐户命名为 `bdcDSA`。 你可以为此帐户选择任何名称。

1. 在域控制器上，打开“Active Directory 用户和计算机”

1. 在左侧面板中，导航到你的域，然后导航到 `bdc` 将使用的 OU

1. 右键单击 OU，然后选择“属性”。

1. 转到“安全性”选项卡（通过右键单击 OU 并选择“视图”，确保选择“高级功能” ）

    ![BDC 对象属性](./media/deploy-active-directory/image15.png)

1. 单击“添加...”，然后添加“bdcDSA”用户 

    ![添加 BDC 对象属性](./media/deploy-active-directory/image16.png)

    ![选择对象](./media/deploy-active-directory/image17.png)

1. 选择 bdcDSA 用户并清除所有权限，然后单击“高级” 

1. 单击“添加”

    ![单击“添加”](./media/deploy-active-directory/image18.png)

    - 单击“选择主体”，插入 bdcDSA，然后单击“确定” 

    - 将“类型”设置为“允许” 

    - 将“应用对象”设置为“此对象和所有后代对象” 

        ![为属性设置允许条件](./media/deploy-active-directory/image19.png)

    - 向下滚动到底部并单击“全部清除”

    - 滚动回到顶部，然后选择：
       - “读取所有属性”
       - “写入所有属性”
       - “创建计算机对象”
       - “删除计算机对象”
       - “创建组对象”
       - “删除组对象”
       - “创建用户对象”
       - “删除用户对象”

    - 单击 **“确定”**

- 单击“添加”

    - 单击“选择主体”，插入 bdcDSA，然后单击“确定” 

    - 将“类型”设置为“允许” 

    - 将“应用对象”设置为“后代计算机对象” 

    - 向下滚动到底部并单击“全部清除”

    - 滚动回到顶部，然后选择“重置密码”

    - 单击 **“确定”**

- 单击“添加”

    - 单击“选择主体”，插入 bdcDSA，然后单击“确定” 

    - 将“类型”设置为“允许” 

    - 将“应用对象”设置为“后代用户对象” 

    - 向下滚动到底部并单击“全部清除”

    - 滚动回到顶部，然后选择“重置密码”

    - 单击 **“确定”**

- 再单击“确定”两次以关闭打开的对话框

## <a name="next-steps"></a>后续步骤

[在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deploy.md)

[对 SQL Server 大数据群集 Active Directory 集成进行故障排除](troubleshoot-active-directory.md)

[概念：在 Active Directory 模式下部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](active-directory-deployment-background.md)
