---
title: 应用部署扩展
titleSuffix: SQL Server big data clusters
description: 在 SQL Server 大数据群集上部署 Python 或 R 脚本作为应用程序。
author: cloudmelon
ms.author: melqin
ms.reviewer: mikeray
ms.date: 08/21/2019
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 74f3306167a4c2fbc248f65e5384ea9847f48f7a
ms.sourcegitcommit: 22e97435c8b692f7612c4a6d3fe9e9baeaecbb94
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/27/2020
ms.locfileid: "92678913"
---
# <a name="how-to-use-visual-studio-code-to-deploy-applications-to-big-data-clusters-2019"></a>如何使用 Visual Studio Code 将应用程序部署到 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本文介绍如何使用带有应用部署扩展的 Microsoft Visual Studio Code 将应用程序部署到 SQL Server 大数据群集。

## <a name="prerequisites"></a>必备条件

- [Visual Studio Code](https://code.visualstudio.com/)
- [SQL Server 大数据群集](big-data-cluster-overview.md)

## <a name="capabilities"></a>功能

此扩展支持 Visual Studio Code 中的以下任务：

- 使用 SQL Server 大数据群集进行身份验证。
- 从 GitHub 存储库中检索应用程序模板，以部署支持的运行时。
- 在用户的工作区中管理当前打开的应用程序模板。
- 通过 YAML 格式的规范来部署应用程序。
- 管理 SQL Server 大数据群集中已部署的应用。
- 在侧边栏中查看部署的所有应用以及附加信息。
- 生成运行规范以使用应用或从群集中删除应用。
- 通过运行规范 YAML 使用已部署的应用。

以下部分介绍了安装过程，并概述了扩展的工作原理。 

### <a name="install"></a>安装

首先在 Visual Studio Code 中安装应用部署扩展：

1. 下载[应用部署扩展](https://aka.ms/app-deploy-vscode)以安装属于 Visual Studio Code 的扩展。

1. 启动 Visual Studio Code 并导航到“扩展”侧边栏。

1. 单击侧边栏顶部的 `Install from vsix` 上下文菜单，然后选择 `…`。

   ![安装 VSIX](media/vs-extension/install_vsix.png)

1. 找到下载的 `sqlservbdc-app-deploy.vsix` 文件，然后选择要安装的文件。

安装 SQL Server 大数据群集应用部署扩展后，系统会提示重载 Visual Studio Code。 现在应该会在 Visual Studio Code 侧边栏中看到 SQL Server BDC 应用资源管理器。

### <a name="app-explorer"></a>应用资源管理器

单击侧边栏中的扩展，加载显示应用资源管理器的侧面板。 下面的应用资源管理器屏幕截图示例显示没有可用的应用或应用规范：

![应用资源管理器](media/vs-extension/app_explorer.png)

#### <a name="connect-to-cluster"></a>连接到群集

若要连接到群集终结点，请使用下列方法之一：

- 单击底部显示 `SQL Server BDC Disconnected` 的状态栏。
- 也可以单击顶部箭头指向门口的 `Connect to Cluster` 按钮。

Visual Studio Code 提示输入相应的终结点、用户名和密码。

要连接的终结点是带有端口 30080 的 `Cluster Management Service` 终结点。

还可以从命令行通过以下方式找到该终结点 

```
azdata bdc endpoint list
```

获取此信息的另一种方法是在 Azure Data Studio 的服务器上右键单击“管理”，你将在其中找到列出的服务的终结点  。

![ADS 终结点](media/vs-extension/ads_end_point.png)

找到要使用的终结点后，请连接到群集。

![新建连接](media/vs-extension/connect_to_cluster.png)

 如果提供正确的凭据和应用终结点，Visual Studio Code 会通知你已连接到群集，并且你将看到所有部署的应用都填充到了侧边栏中。 如果成功连接，终结点和用户名将作为用户配置文件的一部分保存到 `./sqldbc`。 不保存任何密码或令牌。 重新登录时，提示将预先填写保存的主机和用户名，但始终会要求输入密码。 如果要连接到其他群集终结点，只需再次单击 `New Connection` 即可。 如果关闭 Visual Studio Code 或者如果打开其他工作区并且需要重新连接，则连接将自动关闭。

### <a name="app-template"></a>应用模板

需要在 Visual Studio Code 中打开工作区，并在其中保存应用的项目  。

若要从某个模板部署新应用，请单击 `App Specifications` 窗格上的 `New App Template` 按钮，系统将提示你输入名称、运行时以及要在本地计算机上放置新应用的位置。 提供的名称和版本应为 DNS-1035 标签，且必须由小写字母数字字符或“-”组成，以字母字符开头，以字母数字字符结尾。

建议将其放在当前的 Visual Studio Code 工作区中，以便可以使用扩展的全部功能，不过也可以将其放在本地文件系统中的任何位置。

![新建应用模板](media/vs-extension/new_app_template.png)

完成后，将在指定位置为你的一个新应用模板搭建基架，并在你的工作区中打开部署 `spec.yaml`。 如果所选的目录位于你的工作区中，还应该看到该目录在 `App Specifications` 窗格下列出：

![加载的应用模板](media/vs-extension/loading_app_template.png)

此模板是一个简单的 `helloworld` 应用，在“应用规范”窗格中按如下方式布局：

- **spec.yaml**
   - 告知群集如何部署应用
- **run-spec.yaml**
   - 告知群集你希望如何调用应用

可工作区文件夹中找到该应用的源代码。

- **源文件名**
   - 这是 `spec.yaml` 中 `src` 指定的源代码文件
   - 该文件有一个名为 `handler` 的函数，它被视为应用的 `entrypoint`，如 `spec.yaml` 所示。 它采用名为 `msg` 的字符串输入，并返回一个名为 `out` 的字符串输出。 这些内容在 `spec.yaml` 的 `inputs` 和 `outputs` 中指定。

如果不想使用搭建基架的模板且只想通过 `spec.yaml` 来部署已构建的应用，请单击 `New App Template` 按钮旁边的 `New Deploy Spec` 按钮并完成相同的过程，不过你将仅收到 `spec.yaml`，但你可以修改选择方式。

### <a name="deploy-app"></a>部署应用

可以通过 `spec.yaml` 中的代码可重用功能区 `Deploy App` 立即部署此应用，也可以按“应用规范”菜单中 `spec.yaml` 文件旁边的闪电文件夹按钮来进行部署。 扩展将压缩 `spec.yaml` 所在目录中的所有文件，并将应用部署到群集。 

>[!NOTE]
>请确保所有应用文件都与 `spec.yaml` 位于同一目录中。 `spec.yaml` 必须位于应用源代码目录的根级别。 

![部署应用按钮](media/vs-extension/deploy_app_lightning.png)

![部署应用 CodeLens](media/vs-extension/deploy_app_codelens.png)

根据侧边栏中应用的状态，你将在应用可以使用时收到通知：

![应用已部署](media/vs-extension/app_deploy.png)

![应用就绪侧边栏](media/vs-extension/app_ready_side_bar.png)

![应用就绪通知](media/vs-extension/app_ready_notification.png)

在侧窗格中，你将看到向你提供的以下内容：

在侧边栏中可以看到部署的所有应用以及以下信息：

- state
- 版本
- 输入参数
- 输出参数
- 链接
  - swagger
  - 详细信息

如果单击 `Links`，你将发现你可以访问已部署应用的 `swagger.json`，从而可以编写你自己的可调用应用的客户端：

![VS Code UI 的屏幕截图，其中显示了 swagger.json 文件。](media/vs-extension/swagger.png)

有关详细信息，请参阅[在大数据群集上使用应用程序](app-consume.md)。

### <a name="app-run"></a>应用运行

应用准备就绪后，使用作为应用模板的一部分提供的 `run-spec.yaml` 来调用应用：

![运行规范](media/vs-extension/run_spec.png)

指定要替换 `hello` 的任意字符串，然后通过代码可重用功能区链接或 `run-spec.yaml` 旁边的侧边栏中的闪电按钮再次运行该字符串。 如果由于某种原因没有运行规范，请从群集中部署的应用生成一个规范：

![获取运行规范](media/vs-extension/get_run_spec.png)

已有规范且编辑到满意后，请运行该规范。 Visual Studio Code 在应用完成运行后返回相应的反馈：

![应用输出](media/vs-extension/app_output.png)

如上所示，工作区中的临时 `.json` 文件提供输出。 如果想保留此输出，请随时保存，否则，它将在关闭时删除。 如果应用没有要打印到文件的输出，则底部仅显示 `Successful App Run` 通知。 如果运行没有成功，你将收到相应的错误消息，它可以帮助你确定错误详情。

运行应用时，可以通过多种方式传递参数：

可以通过 `.json` 指定所需的所有输入，即：

- `inputs: ./example.json`

在调用部署的应用时，如果任何输入参数是指定应用或用户的固有参数，并且给定的输入参数不是基元（如数组、向量、数据帧、复杂 JSON 等），请在调用应用时在代码行中直接指定参数类型，即：

- 向量
    - `inputs:`
        - `x: [1, 2, 3]`
- 矩阵
    - `inputs:`
        - `x: [[A,B,C],[1,2,3]]`
- Object
    - `inputs:`
        - `x: {A: 1, B: 2, C: 3}`

或者提供 `.txt`、`.json` 或 `.csv` 的相对或绝对文件路径字符串，以应用所需的格式提供所需的输入。 文件分析基于 `Node.js Path library`，其中文件路径定义为 `string that contains a / or \ character`。

如果没有按需提供输入参数，系统将显示相应的错误消息，并显示错误文件路径（若提供了字符串文件路径）或参数无效。 应用创建者有责任确保他们了解他们定义的参数。

要删除应用，只需单击 `Deployed Apps` 侧窗格中应用旁边的“回收站”按钮即可。

## <a name="next-steps"></a>后续步骤

如需了解如何将部署在 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 上的应用集成到自己的应用程序中，请参阅[使用大数据群集上的应用程序](app-consume.md)获取详细信息。 也可以参考[应用部署示例](https://aka.ms/sql-app-deploy)中的其他示例来试用扩展。

有关 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)] 的详细信息，请参阅[什么是 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)]？](big-data-cluster-overview.md)。


我们希望此扩展能够为你带来帮助，感谢你的反馈。 请将反馈发送给 [[!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 团队](https://aka.ms/sqlfeedback)。
