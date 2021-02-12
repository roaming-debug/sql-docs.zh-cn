---
title: azdata bdc config 参考
titleSuffix: SQL Server big data clusters
description: azdata bdc config 命令的参考文章。
author: MikeRayMSFT
ms.author: mikeray
ms.reviewer: seanw
ms.date: 09/22/2020
ms.topic: reference
ms.prod: sql
ms.technology: big-data-cluster
ms.openlocfilehash: 28c9820cd147d5ce4b93fe7605a1f36187f4dfd7
ms.sourcegitcommit: 917df4ffd22e4a229af7dc481dcce3ebba0aa4d7
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 02/10/2021
ms.locfileid: "100048927"
---
# <a name="azdata-bdc-config"></a>azdata bdc config

适用于 [!INCLUDE [azure-data-cli-azdata](../../includes/azure-data-cli-azdata.md)]

以下文章提供了 azdata 工具中 sql 命令的参考。 有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)

## <a name="commands"></a>命令

|命令|描述|
| --- | --- |
[azdata bdc config init](#azdata-bdc-config-init) | 初始化可与 bdc create 一起使用的大数据群集配置文件。
[azdata bdc config list](#azdata-bdc-config-list) | 列出可用的配置文件选择。
[azdata bdc config show](#azdata-bdc-config-show) | 显示 BDC 的当前配置或指定的本地文件的配置，即 custom/bdc.json。
[azdata bdc config add](#azdata-bdc-config-add) | 在配置文件中为 json 路径添加值。
[azdata bdc config add](#azdata-bdc-config-remove) | 在配置文件中为 json 路径删除值。
[azdata bdc config replace](#azdata-bdc-config-replace) | 在配置文件中为 json 路径替换值。
[azdata bdc config replace](#azdata-bdc-config-patch) | 基于 json 修补程序文件来修补配置文件。
## <a name="azdata-bdc-config-init"></a>azdata bdc config init
初始化可与 bdc create 一起使用的大数据群集配置文件。 可以在参数中指定配置文件的特定源。
```bash
azdata bdc config init [--path -p] 
                       [--source -s]  
                       
[--force -f]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>示例
引导式的 BDC config init 体验 - 会收到所需值的提示。
```bash
azdata bdc config init
```
带参数的 BDC config init 创建 aks-dev-test in ./custom 的配置文件。
```bash
azdata bdc config init --source aks-dev-test --target custom
```
### <a name="optional-parameters"></a>可选参数
#### `--path -p`
用于放置配置文件的文件路径默认为 <cwd>/custom。
#### `--source -s`
配置文件源：['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--force -f`
强制覆盖目标文件。
#### `--accept-eula -a`
是否接受许可条款？ [是/否]。 如果不想使用此参数，可以将环境变量 ACCEPT_EULA 设置为“yes”。 可以在 https://aka.ms/eula-azdata-en 查看此产品的许可条款。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-list"></a>azdata bdc config list
列出用于在 `bdc config init` 中使用的可用配置文件选择
```bash
azdata bdc config list [--config-profile -c] 
                       [--type -t]  
                       
[--accept-eula -a]
```
### <a name="examples"></a>示例
显示所有可用的配置文件名称。
```bash
azdata bdc config list
```
显示特定配置文件的 json。
```bash
azdata bdc config list --config-profile aks-dev-test
```
### <a name="optional-parameters"></a>可选参数
#### `--config-profile -c`
默认配置文件：['openshift-prod', 'aks-dev-test-ha', 'aro-dev-test-ha', 'aks-dev-test', 'kubeadm-prod', 'aro-dev-test', 'openshift-dev-test', 'kubeadm-dev-test']
#### `--type -t`
要查看的配置类型。
#### `--accept-eula -a`
是否接受许可条款？ [是/否]。 如果不想使用此参数，可以将环境变量 ACCEPT_EULA 设置为“yes”。 可以在 https://aka.ms/eula-azdata-en 查看此产品的许可条款。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-show"></a>azdata bdc config show
显示 BDC 的当前配置或指定的本地文件的配置，即 custom/bdc.json。 如果只想获取一个部分，该命令还可以采用 json 路径。  还可以指定要输出到的目标文件。  如果未指定目标文件，会直接在终端输出。
```bash
azdata bdc config show [--config-file -c] 
                       [--target -t]  
                       
[--json-path -j]  
                       
[--force -f]
```
### <a name="examples"></a>示例
在控制台中显示 BDC 配置
```bash
azdata bdc config show
```
在本地配置文件中，在简单 json 密钥路径的末尾获取值。
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "metadata.name" --target section.json
```
在本地配置文件中，获取服务中的资源
```bash
azdata bdc config show --config-file custom-config/bdc.json --json-path "$.spec.services.sql.resources" --target section.json
```
### <a name="optional-parameters"></a>可选参数
#### `--config-file -c`
大数据群集配置文件路径（如果不想要当前登录到的群集的配置，即 custom/bdc.json）
#### `--target -t`
用于存储结果的输出文件。 默认：定向到 stdout。
#### `--json-path -j`
Json 密钥路径，通过该路径可找到配置中所需的部分或值（即 key1.key2.key3）。 使用 jsonpath 查询语言 https://jsonpath.com/ ，例如：-j '$.spec.pools[?(@.spec.type == "Master")]..endpoints'
#### `--force -f`
强制覆盖目标文件。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-add"></a>azdata bdc config add
将值添加到配置文件中的 json 路径。  下面的所有示例都以 Bash 形式提供。  如果使用其他命令行，请注意，可能需要适当地进行引用转义。  或者，可以使用修补程序文件功能。
```bash
azdata bdc config add --path -p 
                      --json-values -j
```
### <a name="examples"></a>示例
Ex 1 - 添加控制平面存储。
```bash
azdata bdc config add --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
### <a name="required-parameters"></a>必需的参数
#### `--path -p`
要设置的配置的大数据群集配置文件路径，即 custom/bdc.json
#### `--json-values -j`
值的 json 路径的键值对列表：key1.subkey1=value1,key2.subkey2=value2。 你可以提供内联 json 值，如 key='{"kind":"cluster","name":"test-cluster"}'，或 提供文件路径，如 key=./values.json。 Add 不支持条件语句。  如果提供的内联值本身是带有“=”和“,”的键值对，请对这些字符进行转义。  例如，key1="key2\=val2\,key3\=val3"。 有关路径的示例，请参阅 http://jsonpatch.com/ 。  如果要访问某个数组，须通过指示索引来实现，如 key. 0 = value
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-remove"></a>azdata bdc config remove
删除位于配置文件中 json 路径处的值。  下面的所有示例都以 Bash 形式提供。  如果使用其他命令行，请注意，可能需要适当地进行引用转义。  或者，可以使用修补程序文件功能。
```bash
azdata bdc config remove --path -p 
                         --json-path -j
```
### <a name="examples"></a>示例
Ex 1 - 删除控制平面存储。
```bash
azdata bdc config remove --path custom/control.json --json-path ".spec.storage"
```
### <a name="required-parameters"></a>必需的参数
#### `--path -p`
要设置的配置的大数据群集配置文件路径，即 custom/bdc.json
#### `--json-path -j`
基于 jsonpatch 库的 json 路径列表，该列表指示要删除的值，例如：key1.subkey1,key2.subkey2。 Remove 不支持条件语句。 有关路径的示例，请参阅 http://jsonpatch.com/ 。  如果要访问某个数组，须通过指示索引来实现，如 key. 0 = value
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-replace"></a>azdata bdc config replace
替换位于配置文件中 json 路径处的值。  下面的所有示例都以 Bash 形式提供。  如果使用其他命令行，请注意，可能需要适当地进行引用转义。  或者，可以使用修补程序文件功能。
```bash
azdata bdc config replace --path -p 
                          --json-values -j
```
### <a name="examples"></a>示例
Ex 1 - 替换单个终结点（控制器终结点）的端口。
```bash
azdata bdc config replace --path custom/control.json --json-values "$.spec.endpoints[?(@.name=="Controller")].port=30080"
```
Ex 2 - 替换控制平面存储。
```bash
azdata bdc config replace --path custom/control.json --json-values "spec.storage={"accessMode":"ReadWriteOnce","className":"managed-premium","size":"10Gi"}"
```
Ex 3 - 替换 storage-0 资源规格，包括副本。
```bash
azdata bdc config replace --path custom/bdc.json --json-values "$.spec.resources.storage-0.spec={"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}"
```
### <a name="required-parameters"></a>必需的参数
#### `--path -p`
要设置的配置的大数据群集配置文件路径，即 custom/bdc.json
#### `--json-values -j`
值的 json 路径的键值对列表：key1.subkey1=value1,key2.subkey2=value2。 你可以提供内联 json 值，如 key='{"kind":"cluster","name":"test-cluster"}'，或 提供文件路径，如 key=./values.json。 Replace 通过 jsonpath 库支持条件语句。  若要使用条件语句，路径开头应使用 $。 这样可执行诸如 -j $.key1.key2[?(@.key3=='someValue'].key4=value 的条件语句。 如果提供的内联值本身是带有“=”和“,”的键值对，请对这些字符进行转义。  例如，key1="key2\=val2\,key3\=val3"。 可以参阅下面的示例。 如需其他帮助，请参阅： https://jsonpath.com/
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。
## <a name="azdata-bdc-config-patch"></a>azdata bdc config patch
根据给定的修补程序文件对配置文件进行修补。 请访问 http://jsonpatch.com/ 以更好地了解如何构建路径。 通过 jsonpath 库 https://jsonpath.com/ ，替换操作可在其路径中使用条件语句。 所有修补程序 json 文件必须以名为 "patch" 的键开头，该键包含一系列修补程序及其相应操作（添加、替换、删除）、路径和值。 “删除”操作不需要值，只需要一个路径。 请参阅下面的示例。
```bash
azdata bdc config patch --path 
                        --patch-file -p
```
### <a name="examples"></a>示例
Ex 1 - 使用修补程序文件替换单个终结点（控制器终结点）的端口。
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.endpoints[?(@.name=="Controller")].port","value":30080}]}
```
Ex 2 - 使用修补程序文件替换控制平面存储。
```bash
azdata bdc config patch --path custom/control.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":".spec.storage","value":{"accessMode":"ReadWriteMany","className":"managed-premium","size":"10Gi"}}]}
```
Ex 3 - 使用修补程序文件替换池存储，包括副本（存储池）。
```bash
azdata bdc config patch --path custom/bdc.json --patch ./patch.json

    Patch File Example (patch.json):
        {"patch":[{"op":"replace","path":"$.spec.resources.storage-0.spec","value":{"replicas": 2,"storage": {"className": "managed-premium","size": "10Gi","accessMode": "ReadWriteOnce"},"type": "Storage"}}]}
```
### <a name="required-parameters"></a>必需的参数
#### `--path`
要设置的配置的大数据群集配置文件路径，即 custom/bdc.json
#### `--patch-file -p`
基于 jsonpatch 库的修补程序 json 文件的路径： http://jsonpatch.com/ 。 修补程序 json 文件必须以名为“patch”的键开头，其值是要执行的修补程序操作的数组。 对于修补操作的路径，可以使用点表示法，如用于大多数操作的 key1.key2。 如果要执行替换操作，并且要在需要条件语句的数组中替换值，请使用 jsonpath 表示法，以 $ 作为路径的开头。 这样可以执行诸如 $.key1.key2[?(@.key3=='someValue'].key4 的条件语句。 请参阅下面的示例。 如需有关条件语句的其他帮助，请参阅： https://jsonpath.com/ 。
### <a name="global-arguments"></a>全局参数
#### `--debug`
提高日志记录详细程度以显示所有调试日志。
#### `--help -h`
显示此帮助消息并退出。
#### `--output -o`
输出格式。  允许的值：json、jsonc、table、tsv。  默认值：json。
#### `--query -q`
JMESPath 查询字符串。 请参阅 [http://jmespath.org/](http://jmespath.org)，获取详细信息和示例。
#### `--verbose`
提高日志记录详细程度。 使用 --debug 获取完整的调试日志。

## <a name="next-steps"></a>后续步骤

有关其他 azdata 命令的详细信息，请参阅 [azdata 参考](reference-azdata.md)。 

有关如何安装 azdata 工具的详细信息，请参阅[安装 azdata](..\install\deploy-install-azdata.md)。

