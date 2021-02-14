---
title: 在 Azure Red Hat OpenShift python 脚本上部署
titleSuffix: SQL Server Big Data Clusters
description: 了解如何使用部署脚本在 Azure Red Hat OpenShift (ARO) 上部署 SQL Server 大数据群集。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: mihaelab
ms.date: 06/22/2020
ms.topic: conceptual
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: d53950d277b245b405420af2c28fa7666ba653dd
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100044102"
---
# <a name="use-a-python-script-to-deploy-a-sql-server-big-data-cluster-on-azure-red-hat-openshift-aro"></a>使用 python 脚本在 Azure Red Hat OpenShift (ARO) 上部署 SQL Server 大数据群集

[!INCLUDE[SQL Server 2019](../includes/applies-to-version/sqlserver2019.md)]

本教程中，使用示例 python 部署脚本将 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ver15.md)] 部署到 [Azure Red Hat OpenShift (ARO)](/azure/virtual-machines/linux/openshift-get-started)。 从 SQL Server 2019 CU5 开始，为此部署选项提供支持。

> [!TIP]
> ARO 只是为大数据群集托管 Kubernetes 的一种选择。 若要了解其他部署选项以及如何自定义部署选项，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。


> [!WARNING]
> 使用内置存储类“managed premium”创建的持久卷包含回收策略“删除”。 因此，如果你删除 SQL Server 大数据群集，永久性卷声明和永久性卷都会遭删除。 应通过结合使用 azure-disk 预配程序和“保留”回收策略，创建自定义存储类，如[存储概念](/azure/aks/concepts-storage/#storage-classes)所述。 下面的脚本使用的是 managed-premium 存储类。 有关更多详细信息，请参阅[数据暂留](concept-data-persistence.md)主题。

此处使用的默认大数据群集部署包括一个 SQL Master 实例、一个计算池实例、两个数据池实例和两个存储池实例。 通过 ARO 默认存储类使用 Kubernetes 永久性卷来保存数据。 本教程中使用的默认配置适用于开发/测试环境。

## <a name="prerequisites"></a>先决条件

- Azure 订阅。
- [oc](https://docs.openshift.com/container-platform/4.4/cli_reference/openshift_cli/getting-started-cli.html)
- [Python 最低版本 3.0](https://www.python.org/downloads)
- [`az` CLI](/cli/azure/install-azure-cli/)
- [[!INCLUDE [azure-data-cli-azdata](../includes/azure-data-cli-azdata.md)]](../azdata/install/deploy-install-azdata.md)
- **Azure Data Studio**

## <a name="log-in-to-your-azure-account"></a>登录 Azure 帐户

该脚本使用 Azure CLI 自动创建 ARO 群集。 在运行脚本之前，必须至少使用 Azure CLI 登录 Azure 帐户一次。 从命令提示符运行以下命令。

```azurecli
az login
```

## <a name="instructions"></a>Instructions

1. 下载 Python 脚本 `deploy-sql-big-data-aro.py` 和 yaml 文件 `bdc-scc.yaml`。

   >这些文件位于本文中的以下位置：
   - [`deploy-sql-big-data-aro.py`](#deploy-sql-big-data-aropy)
   - [`bdc-scc.yaml`](#bdc-sccyaml)

1. 使用以下内容运行脚本：

```console
python deploy-sql-big-data-aro.py
```

出现提示时，输入 Azure 订阅 ID 及在其中创建资源的 Azure 资源组。 还可以根据需要输入其他配置或使用提供的默认值。 例如：

- `azure_region`
- 用于 OpenShift 工作器节点的 `vm_size`。 若要在验证基本方案时获得最佳体验，我们建议在群集的所有工作器节点上至少有 8 个个 vCPU 和 64 GB 的内存。 该脚本使用 `Standard_D8s_v3` 和 3 个工作器节点作为默认值。 大数据群集的默认大小配置还使用大约 24 个磁盘用于所有组件中的持久卷声明。
- OpenShift 群集部署的网络配置 - 有关每个参数的详细信息，请参阅 [ARO 部署文章](\azure\openshift\tutorial-create-cluster)。
- `cluster_name` - 此值用于 ARO 群集和在 ARO 之上创建的 SQL Server 大数据群集。 请注意，SQL 大数据群集的名称将为 Kubernetes 命名空间。
- `username ` - 这是在部署期间为控制器管理员帐户、SQL Server 主实例帐户和网关预配的帐户的用户名。 请注意，将自动为你禁用 `sa` SQL Server 帐户，这是最佳做法。
- `password` - 将对所有帐户使用相同的值。

SQL Server 大数据群集现已部署在 ARO 上。 你现在可以使用 Azure Data Studio 连接到该群集。 有关详细信息，请参阅[使用 Azure Data Studio 连接到 SQL Server 大数据群集](connect-to-big-data-cluster.md)。

## <a name="clean-up"></a>清除

如果在 Azure 中测试 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]，则应在完成后删除 ARO 群集以避免意外收费。 如果你打算继续使用它，请不要删除该群集。

> [!WARNING]
> 以下步骤将删除 ARO 群集，删除该群集也将删除 SQL Server 大数据群集。 如果要保留任何数据库或 HDFS 数据，请在删除群集之前对该数据进行备份。

运行以下 Azure CLI 命令以删除 Azure 中的大数据群集和 ARO 服务（将 `<resource group name>` 替换为你在部署脚本中指定的“Azure 资源组”）：

```azurecli
az group delete -n <resource group name>
```

## `deploy-sql-big-data-aro.py` 

本部分中的脚本将 SQL Server 大数据群集部署到 Azure Red Hat OpenShift。 将脚本复制到工作站，并将其保存为 `deploy-sql-big-data-aro.py`，然后开始部署。

```python
#
# Prerequisites: 
# 
# Azure CLI (https://docs.microsoft.com/en-us/cli/azure/install-azure-cli), Azure Data CLI (`azdata`) (https://docs.microsoft.com/en-us/sql/big-data-cluster/deploy-install-azdata), oc CLI (https://www.openshift.com/blog/installing-oc-tools-windows)
#
# Run `az login` at least once BEFORE running this script
#

from subprocess import check_output, CalledProcessError, STDOUT, Popen, PIPE, getoutput
from time import sleep
import os
import getpass
import json

def executeCmd (cmd):
    if os.name=="nt":
        process = Popen(cmd.split(),stdin=PIPE, shell=True)
    else:
        process = Popen(cmd.split(),stdin=PIPE)
    stdout, stderr = process.communicate()
    if (stderr is not None):
        raise Exception(stderr)

#
# MUST INPUT THESE VALUES!!!!!
#
SUBSCRIPTION_ID = input("Provide your Azure subscription ID:").strip()
GROUP_NAME = input("Provide Azure resource group name to be created:").strip()
#
# This password will be use for Controller user, Gateway user and SQL Server Master SA accounts
AZDATA_USERNAME=input("Provide username to be used for Controller, SQL Server and Gateway endpoints - Press ENTER for using  `admin`:").strip() or "admin"
AZDATA_PASSWORD = getpass.getpass("Provide password to be used for Controller user, Gateway user and SQL Server Master accounts:")
#
# Optionally change these configuration settings
#
AZURE_REGION=input("Provide Azure region - Press ENTER for using `westus2`:").strip() or "westus2"
# MASTER_VM_SIZE=input("Provide VM size for master nodes for the ARO cluster - Press ENTER for using  `Standard_D2s_v3`:").strip() or "Standard_D2s_v3"
WORKER_VM_SIZE=input("Provide VM size for the worker nodes for the ARO cluster - Press ENTER for using  `Standard_D8s_v3`:").strip() or "Standard_D8s_v3"
OC_NODE_COUNT=input("Provide number of worker nodes for ARO cluster - Press ENTER for using  `3`:").strip() or "3"
VNET_NAME=input("Provide name of Virtual Network for ARO cluster - Press ENTER for using  `aro-vnet`:").strip() or "aro-vnet"
VNET_ADDRESS_SPACE=input("Provide Virtual Network Address Space for ARO cluster - Press ENTER for using  `10.0.0.0/16`:").strip() or "10.0.0.0/16"
MASTER_SUBNET_NAME=input("Provide Master Subnet Name for ARO cluster - Press ENTER for using  `master-subnet`:").strip() or "master-subnet"
MASTER_SUBNET_IP_RANGE=input("Provide address range of Master Subnet for ARO cluster - Press ENTER for using  `10.0.0.0/23`:").strip() or "10.0.0.0/23"
WORKER_SUBNET_NAME=input("Provide Worker Subnet Name for ARO cluster - Press ENTER for using  `worker-subnet`:").strip() or "worker-subnet"
WORKER_SUBNET_IP_RANGE=input("Provide address range of Worker Subnet for ARO cluster - Press ENTER for using  `10.0.2.0/23`:").strip() or "10.0.2.0/23"
#
# This is both Kubernetes cluster name and SQL Big Data cluster name
CLUSTER_NAME=input("Provide name of OpenShift cluster and SQL big data cluster - Press ENTER for using  `sqlbigdata`:").strip() or "sqlbigdata"
#
# Deploy the ARO cluster
#  
print ("Set azure context to subscription: "+SUBSCRIPTION_ID)
command = "az account set -s "+ SUBSCRIPTION_ID
executeCmd (command)
print ("Creating azure resource group: "+GROUP_NAME)
command="az group create --name "+GROUP_NAME+" --location "+AZURE_REGION
executeCmd (command)
command = "az network vnet create --resource-group "+GROUP_NAME+" --name "+VNET_NAME+" --address-prefixes "+VNET_ADDRESS_SPACE
print("Creating Virtual Network: "+VNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+MASTER_SUBNET_NAME+" --address-prefixes "+MASTER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Master Subnet: "+MASTER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet create --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --name "+WORKER_SUBNET_NAME+" --address-prefixes "+WORKER_SUBNET_IP_RANGE+" --service-endpoints Microsoft.ContainerRegistry"
print("Creating Worker Subnet: "+WORKER_SUBNET_NAME)
executeCmd(command)
command = "az network vnet subnet update --name "+MASTER_SUBNET_NAME+" --resource-group "+GROUP_NAME+" --vnet-name "+VNET_NAME+" --disable-private-link-service-network-policies true"
print("Updating Master Subnet by disabling Private Link Policies")
executeCmd(command)
command = "az aro create --resource-group "+GROUP_NAME+" --name "+CLUSTER_NAME+" --vnet "+VNET_NAME+" --master-subnet "+MASTER_SUBNET_NAME+" --worker-subnet "+WORKER_SUBNET_NAME+" --worker-count "+OC_NODE_COUNT+" --worker-vm-size "+WORKER_VM_SIZE +" --only-show-errors"
print("Creating OpenShift cluster: "+CLUSTER_NAME)
executeCmd (command)
#
# Login to oc console
#
command = "az aro list-credentials --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
OC_CLUSTER_USERNAME = str(output['kubeadminUsername'])
OC_CLUSTER_PASSWORD = str(output['kubeadminPassword'])
command = "az aro show --name "+CLUSTER_NAME+" --resource-group "+GROUP_NAME +" --only-show-errors"
output=json.loads(getoutput(command))
APISERVER = str(output['apiserverProfile']['url'])
command = "oc login "+ APISERVER+ " -u " + OC_CLUSTER_USERNAME + " -p "+ OC_CLUSTER_PASSWORD
executeCmd (command)
#
# Setup pre-requisites for deploying BDC on OpenShift
#
#
#Creating new project/namespace
command = "oc new-project "+ CLUSTER_NAME
executeCmd (command)
#
# create custom SCC for BDC
command = "oc apply -f bdc-scc.yaml"
executeCmd (command)
#
#Adding the custom scc to BDC namespace
command = "oc adm policy add-scc-to-group bdc-scc system:serviceaccounts:" + CLUSTER_NAME
executeCmd (command)
#
# Deploy big data cluster
#
print ('Set environment variables for credentials')
os.environ['AZDATA_PASSWORD'] = AZDATA_PASSWORD
os.environ['AZDATA_USERNAME'] = AZDATA_USERNAME
os.environ['ACCEPT_EULA']="Yes"
#
#Creating azdata configuration with aro-dev-test profile
command = "azdata bdc config init --source aro-dev-test --target custom --force"
executeCmd (command)
command="azdata bdc config replace -c custom/bdc.json -j ""metadata.name=" + CLUSTER_NAME + ""
executeCmd (command)
#
# Create BDC
command = "azdata bdc create --config-profile custom --accept-eula yes"
executeCmd(command)
#login into BDC cluster and list endpoints
command="azdata login -n " + CLUSTER_NAME
executeCmd (command)
print("")
print("SQL Server big data cluster endpoints: ")
command="azdata bdc endpoint list -o table"
executeCmd(command)
```

## `bdc-scc.yaml`

以下 .yaml 清单为大数据群集部署定义自定义的安全性上下文约束 (SCC)。 将其复制到与 `deploy-sql-big-data-aro.py` 相同的目录。

```yaml
allowHostDirVolumePlugin: false
allowHostIPC: false
allowHostNetwork: false
allowHostPID: false
allowHostPorts: false
allowPrivilegeEscalation: true
allowPrivilegedContainer: false
allowedCapabilities:
  - SETUID
  - SETGID
  - CHOWN
  - SYS_PTRACE
apiVersion: security.openshift.io/v1
defaultAddCapabilities: null
fsGroup:
  type: RunAsAny
kind: SecurityContextConstraints
metadata:
  annotations:
    kubernetes.io/description: SQL Server BDC custom scc is based on 'nonroot' scc plus additional capabilities required by BDC.
  generation: 2
  name: bdc-scc
readOnlyRootFilesystem: false
requiredDropCapabilities:
  - KILL
  - MKNOD
runAsUser:
  type: MustRunAsNonRoot
seLinuxContext:
  type: MustRunAs
supplementalGroups:
  type: RunAsAny
volumes:
  - configMap
  - downwardAPI
  - emptyDir
  - persistentVolumeClaim
  - projected
  - secret
```

## <a name="next-steps"></a>后续步骤

部署脚本配置了 Azure Kubernetes Service，并部署了 SQL Server 2019 大数据群集。 你还可以选择通过手动安装来自定义未来的部署。 若要了解有关如何部署大数据群集以及如何自定义部署的详细信息，请参阅[如何在 Kubernetes 上部署 [!INCLUDE[big-data-clusters-2019](../includes/ssbigdataclusters-ss-nover.md)]](deployment-guidance.md)。

现在已部署 SQL Server 大数据群集，你可以加载示例数据并浏览教程：

> [!div class="nextstepaction"]
> [教程：将示例数据加载到 SQL Server 2019 大数据群集中](tutorial-load-sample-data.md)