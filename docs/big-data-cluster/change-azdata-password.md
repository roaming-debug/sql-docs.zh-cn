---
title: 更新 AZDATA_PASSWORD
description: 手动更新 `AZDATA_PASSWORD`
author: NelGson
ms.author: negust
ms.reviewer: mikeray
ms.date: 03/01/2021
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 062e574772c2a44b78772da4a979c81ed3deb959
ms.sourcegitcommit: 9413ddd8071da8861715c721b923e52669a921d8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/04/2021
ms.locfileid: "101836244"
---
# <a name="manually-update-azdata_password"></a>手动更新 `AZDATA_PASSWORD`

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

无论 [!INCLUDE[ssbigdataclusters-ss-nover](../includes/ssbigdataclusters-ss-nover.md)] 是否使用 Active Directory 集成运行，都会在部署期间设置 `AZDATA_PASSWORD`。 它为群集控制器和主实例提供了基本身份验证。 本文档介绍了如何手动更新 `AZDATA_PASSWORD`。

## <a name="change-azdata_password-for-controller"></a>更改控制器的 `AZDATA_PASSWORD`

如果群集在非 Active Directory 模式下运行，请执行以下操作来更新 Apache Knox 网关密码：

1. 通过运行以下命令，获取控制器 [!INCLUDE[ssNoVersion](../includes/ssnoversion-md.md)] 凭据：

   a. 以 Kubernetes 管理员身份运行以下命令：

   ```bash
   kubectl get secret controller-sa-secret -n <cluster name> -o yaml | grep password
   ```

   b. 对密码执行 Base64 解码：
   
   ```bash
   echo <password from kubectl command>  | base64 --decode && echo
   ```

1. 在单独的命令窗口中，公开控制器数据库服务器端口：

   ```bash
   kubectl port-forward controldb-0 1433:1433 --address 0.0.0.0 -n <cluster name>
   ```
 
1. 使用刚刚获取的系统管理员密码，从 SQL 客户端工具连接到控制器数据库服务器。

1. 为 `AZDATA_USERNAME` 生成新的复杂密码，以替换现有 `AZDATA_PASSWORD`。

   为了简化此示例，后续步骤使用“newPassword”，因为生成的密码是“newPassword”。 

1. 从 users 表获取 `hexsalt`：

   ```sql
   SELECT hexsalt FROM [auth].[users] WHERE username = '<username>'
   ```

   `hexsalt` 返回一个随机十六进制字符串（例如，`64FC59DF31244FFEE02F457BC0750226`）。

1. 使用 `hexsalt` 对新的复杂密码进行加密：

   为了方便起见，我们提供预置工具 `pbkdf2` 来加密此密码。 下载 [`pbkdf2`](https://github.com/microsoft/sql-server-samples/tree/master/samples/features/sql-big-data-cluster/security/password-hashing/pbkdf2/prebuilt-binaries) 的平台专用 .NET Core 应用。

   此应用是独立式，不需要任何系统必备（如 .NET 运行时）。 若要对密码进行加密，请运行以下命令：

   ```bash
   pbkdf2 <password> <hexsalt>
   J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=
   ```

1. 更新 users 表中的密码：

   ```sql
   UPDATE [auth].[users] SET password = 'J2y4E4dhlgwHOaRr3HKiiVAKBfjuGDyYmzn88VXmrzM=' WHERE username = '<username>'
   ```

## <a name="change-azdata_password-in-the-sql-server-master-instance"></a>更改 SQL Server 主实例中的 `AZDATA_PASSWORD`

1. 以任意管理员用户身份连接到主 SQL 终结点。

1. 若要更改在部署期间在参数 `AZDATA_USERNAME` 中定义的登录凭据的密码，请运行以下 TSQL 命令：

   ```sql
   ALTER LOGIN <AZDATA_USERNAME> WITH PASSWORD = 'newPassword'
   ```

## <a name="manually-updating-password-for-grafana-and-kibana"></a>手动更新 Grafana 和 Kibana 的密码

执行更新 AZDATA_PASSWORD 的步骤后，你将看到 [Grafana](app-monitor.md) 和 [Kibana](cluster-logging-kibana.md) 仍接受旧密码。 这是因为 Grafana 和 Kibana 对新 Kubernetes 机密没有可见性。 必须单独手动更新 Grafana 和 Kibana 密码。

## <a name="update-grafana-password"></a>更新 Grafana 密码

请按照以下选项手动更新 [Grafana](app-monitor.md) 密码。

1. htpasswd 实用程序是必备组件。 可以在任何客户端计算机上安装它。
    
    #### <a name="for-ubuntu"></a>[对于 Ubuntu](#tab/ubuntu)： 
    ```bash
    sudo apt install apache2-utils
    ```
    
    #### <a name="for-rhel"></a>[对于 RHEL](#tab/rhel)： 
    ```bash
    sudo yum install httpd-tools
    ```
    
    ---

2. 生成新密码。 
    
    ```bash
    htpasswd -nbs <username> <password>
    admin:{SHA}<secret>
    ```
    
    根据需要替换 /<username/>、/<password/>、/<secret/> 的值，例如：
    
    ```bash
    htpasswd -nbs admin Test@12345
    admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=
    ```

3. 现在对密码进行编码：
    
    ```bash
    echo "admin:{SHA}W/5VKRjIzjusUJ0ih0gHyEPjC/s=" | base64
    ```             
    
    保留输出 base64 字符串供以后使用。
    
4. 接下来，编辑 mgmtproxy-secret：
    
    ```bash
    kubectl edit secret -n mssql-cluster mgmtproxy-secret
    ```
         
5. 用上面生成的新编码的密码 base64 字符串更新 controller-login-htpasswd：
    
    ```console
    # Please edit the object below. Lines beginning with a '#' will be ignored,
    # and an empty file will abort the edit. If an error occurs while saving this file will be
    # reopened with the relevant failures.
    #
    apiVersion: v1
    data:
       controller-login-htpasswd: <base64 string from before>
       mssql-internal-controller-password: <password>
       mssql-internal-controller-username: <username>
    ```         

6. 标识并删除 mgmtproxy pod。 
     
    如有必要，请确定 mgmtproxy prod 的名称。
    
    #### <a name="for-windows"></a>[对于 Windows](#tab/windows)： 
    在 Windows 服务器上，可以使用以下各项：
    
    ```bash 
    kubectl get pods -n <namespace> -l app=mgmtproxy
    ```
    
    #### <a name="for-linux"></a>[对于 Linux](#tab/linux)： 
    在 Linux 上，可以使用以下各项：
    
    ```bash
    kubectl get pods -n <namespace> | grep 'mgmtproxy'
    ```
    
    ---
    
    删除 mgmtproxy pod：
    ```bash
    kubectl delete pod mgmtproxy-xxxxx -n mssql-clutser
    ```

7. 等待 mgmtproxy pod 进入联机状态和 Grafana 仪表板启动。  
 
    等待不会太久，pod 应在几秒内进入联机状态。 若要检查 pod 的状态，可以使用上一步中使用的相同 `get pods` 命令。 
    如果看到 mgmtproxy pod 立即恢复到“就绪”状态，请使用 kubectl 来描述 pod：
    
    ```bash
    kubectl describe pods mgmtproxy-xxxxx  -n <namespace>
    ```
    
    为了排除故障和进一步收集日志，请使用 Azure Data CLI `[azdata bdc debug copy-logs](../azdata/reference/reference-azdata-bdc-debug.md)` 命令。
    
8. 现在使用新密码登录到 Grafana。 


## <a name="update-the-kibana-password"></a>更新 Kibana 密码

请按照以下选项手动更新 [Kibana](cluster-logging-kibana.md) 密码。

> [!NOTE]
> 旧版 Microsoft Edge 浏览器与 Kibana 不兼容，因此必须使用基于 chromium 的 Edge 浏览器，才能正确显示仪表板。 使用不受支持的浏览器加载仪表板时，会看到一个空白页，请参阅 [Kibana 支持的浏览器](https://www.elastic.co/support/matrix#matrix_browsers)。

1. 打开 Kibana URL。
    
    可以从 [Azure Data Studio](manage-with-controller-dashboard#controller-dashboard) 内查找 Kibana 服务终结点 URL，或使用以下 azdata 命令：
    
    ```azurecli
    azdata login
    azdata bdc endpoint list -e logsui -o table
    ```
    
    例如：https://11.111.111.111:30777/kibana/app/kibana#/discover

2. 在左侧窗格中，单击“安全”选项。
    
    ![Kibana 左侧窗格菜单屏幕截图，其中选择了“安全”选项](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-1.jpg)

3. 在“安全”页上的“身份验证后端”标题下，单击“内部用户数据库”。

    ![“安全”页屏幕截图，其中选中了“内部用户数据库”框。](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-2.jpg)

4. 现在，你将看到“内部用户数据库”标题下的用户列表。 使用此页可以为 Kibana 终结点访问添加、修改和删除任何用户。 对于需要更新密码的用户，请单击用户右侧的“编辑”按钮。

    ![“内部用户数据库”页的屏幕截图。 在用户列表中，为 KubeAdmin 用户选择“编辑”按钮。](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-3.jpg)

5. 输入新密码两次，然后单击“提交”：

    ![内部用户编辑窗体的屏幕截图。 已在“密码”和“再次输入密码”字段中输入新密码。](\media\big-data-cluster-change-kibana-password\big-data-cluster-change-kibana-password-4.jpg)

6. 关闭浏览器并使用更新的密码重新连接到 Kibana URL。

> [!Note]
> 使用新密码登录后，如果在 Kibana 中看到空白页，请使用右上角的注销选项手动注销，然后再次登录。

## <a name="see-also"></a>另请参阅

* [azdata bdc (Azure Data CLI)](../../sql/azdata/reference/reference-azdata-bdc.md) 
* [使用 azdata 和 Grafana 仪表板监视应用程序](app-monitor.md)  
* [使用 Kibana 仪表板签出群集日志](cluster-logging-kibana.md)  