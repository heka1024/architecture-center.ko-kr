---
title: 조건부로 Azure Resource Manager 템플릿의 리소스 배포
description: 조건부로 매개 변수의 값에 종속되는 리소스를 배포하도록 Azure Resource Manager 템플릿의 기능을 확장하는 방법을 설명합니다.
author: petertay
ms.date: 10/30/2018
ms.openlocfilehash: 0e02fbbd130bd6be2fc10173c8466b028d5d61da
ms.sourcegitcommit: 1f4cdb08fe73b1956e164ad692f792f9f635b409
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 01/08/2019
ms.locfileid: "54113470"
---
# <a name="conditionally-deploy-a-resource-in-an-azure-resource-manager-template"></a>조건부로 Azure Resource Manager 템플릿의 리소스 배포

매개 변수 값이 있는지 여부와 같은 조건에 따라 리소스를 배포하도록 템플릿을 디자인해야 하는 시나리오가 있을 수 있습니다. 예를 들어 템플릿은 가상 네트워크를 배포하고, 피어링을 위한 다른 가상 네트워크를 지정하기 위한 매개 변수를 포함할 수 있습니다. 피어링에 대해 매개 변수 값을 지정하지 않은 경우 Resource Manager가 피어링 리소스를 배포하는 것을 원하지 않을 것입니다.

이렇게 하려면 리소스에 [조건 요소][azure-resource-manager-condition]를 사용하여 매개 변수 배열의 길이를 테스트합니다. 길이가 0인 경우 `false`를 반환하여 배포를 방지하고, 0보다 큰 모든 값의 경우에는 `true`를 반환하여 배포를 허용합니다.

## <a name="example-template"></a>예제 템플릿

이 내용을 설명하는 예제 템플릿을 살펴보겠습니다. 템플릿은 [조건 요소][azure-resource-manager-condition]를 사용하여 `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` 리소스의 배포를 제어합니다. 이 리소스는 동일한 지역에 있는 두 Azure Virtual Network 간에 피어링을 만듭니다.

템플릿의 각 섹션에 살펴보겠습니다.

`parameters` 요소는 `virtualNetworkPeerings`라는 단일 매개 변수를 정의합니다.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "virtualNetworkPeerings": {
      "type": "array",
      "defaultValue": []
    }
  },
```

`virtualNetworkPeerings` 매개 변수는 `array`이며 다음 스키마를 갖습니다.

```json
"virtualNetworkPeerings": [
    {
      "name": "firstVNet/peering1",
      "properties": {
          "remoteVirtualNetwork": {
              "id": "[resourceId('Microsoft.Network/virtualNetworks','secondVNet')]"
          },
          "allowForwardedTraffic": true,
          "allowGatewayTransit": true,
          "useRemoteGateways": false
      }
    }
]
```

매개 변수의 속성은 [가상 네트워크 피어링과 관련된 설정][vnet-peering-resource-schema]을 지정합니다. `resources` 섹션에서 `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` 리소스를 지정할 때 이러한 속성에 대한 값을 제공합니다.

```json
"resources": [
    {
      "type": "Microsoft.Resources/deployments",
      "apiVersion": "2017-05-10",
      "name": "[concat('vnp-', copyIndex())]",
      "condition": "[greater(length(parameters('virtualNetworkPeerings')), 0)]",
      "dependsOn": [
        "firstVNet", "secondVNet"
      ],
      "copy": {
          "name": "iterator",
          "count": "[length(variables('peerings'))]",
          "mode": "serial"
      },
      "properties": {
        "mode": "Incremental",
        "template": {
          "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
          },
          "variables": {
          },
          "resources": [
            {
              "type": "Microsoft.Network/virtualNetworks/virtualNetworkPeerings",
              "apiVersion": "2016-06-01",
              "location": "[resourceGroup().location]",
              "name": "[variables('peerings')[copyIndex()].name]",
              "properties": "[variables('peerings')[copyIndex()].properties]"
            }
          ],
          "outputs": {
          }
        }
      }
    }
]
```

템플릿의 이 부분에서 진행되는 몇 가지 작업이 있습니다. 먼저, 배포될 실제 리소스는 `Microsoft.Network/virtualNetworks/virtualNetworkPeerings`를 실제로 배포하는 자체 템플릿을 포함하는 `Microsoft.Resources/deployments` 형식의 인라인 템플릿입니다.

인라인 템플릿의 `name`은 `copyIndex()`의 현재 반복을 접두사 `vnp-`와 연결해서 고유하게 생성됩니다.

`condition` 요소는 `greater()` 함수가 `true`로 평가될 때 리소스가 처리되도록 지정합니다. 여기서는 `virtualNetworkPeerings` 매개 변수 배열이 0보다 `greater()` 상태인지를 테스트합니다. 0보다 크면 `true`로 평가되고 `condition`은 충족됩니다. 0보다 작으면 `false`입니다.

다음으로, `copy` 루프를 지정합니다. 루프가 순서대로 수행되며, 각 리소스는 마지막 리소스가 배포될 때까지 대기함을 의미하는 `serial` 루트입니다. `count` 속성은 루프 반복 횟수를 지정합니다. 여기에서, 일반적으로는 `virtualNetworkPeerings`의 길이로 설정합니다. 배포하려는 리소스를 지정하는 매개 변수 개체를 포함하기 때문입니다. 그러나 이렇게 할 경우, 존재하지 않는 속성에 액세스하려고 한다는 사실을 Resource Manager가 알게 되므로 배열이 비어 있으면 유효성 검사가 실패합니다. 그러나 이 문제는 해결할 수 있습니다. 필요한 변수를 살펴보겠습니다.

```json
  "variables": {
    "workaround": {
       "true": "[parameters('virtualNetworkPeerings')]",
       "false": [{
           "name": "workaround",
           "properties": {}
       }]
     },
     "peerings": "[variables('workaround')[string(greater(length(parameters('virtualNetworkPeerings')), 0))]]"
  },
```

`workaround` 변수에는 `true` 및 `false`라는 2개의 속성이 포함됩니다. `true` 속성은 `virtualNetworkPeerings` 매개 변수 배열의 값으로 평가됩니다. `false` 속성은 유효성 검사를 충족하는 `virtualNetworkPeerings` 매개 변수와 마찬가지로, `false`가 실제로 배열이라는 &mdash; 사실을 알기 위해 Resource Manager가 예상하는 명명된 속성을 포함하는 빈 개체로 평가됩니다.

`peerings` 변수는 `workaround` 변수를 한 번 더 사용하여, `virtualNetworkPeerings` 매개 변수 배열의 길이가 0보다 큰지 테스트합니다. 0보다 크면 `string`은 `true`로 평가되고, `workaround` 변수는 `virtualNetworkPeerings` 매개 변수 배열로 평가됩니다. 그렇지 않은 경우 `false`로 평가되고, `workaround` 변수는 배열의 첫 번째 요소에 있는 빈 개체로 평가됩니다.

유효성 검사 문제가 해결되었으므로, 중첩된 템플릿에서 `Microsoft.Network/virtualNetworks/virtualNetworkPeerings` 리소스의 배포를 간단히 지정하고, `virtualNetworkPeerings` 매개 변수 배열의 `name` 및 `properties`를 제공할 수 있습니다. 이 내용은 리소스의 `properties` 요소에 중첩된 `template` 요소에서 확인할 수 있습니다.

## <a name="try-the-template"></a>템플릿 시도

[GitHub][github]에서 예제 템플릿을 사용할 수 있습니다. 템플릿을 배포하려면 다음 [Azure CLI][cli] 명령을 실행합니다.

```bash
az group create --location <location> --name <resource-group-name>
az group deployment create -g <resource-group-name> \
    --template-uri https://raw.githubusercontent.com/mspnp/template-examples/master/example2-conditional/deploy.json
```

## <a name="next-steps"></a>다음 단계

* 템플릿 매개 변수로 스칼라 값이 아닌 개체를 사용합니다. [Azure Resource Manager 템플릿에서 개체를 매개 변수로 사용](./objects-as-parameters.md)을 참조하세요.

<!-- links -->
[azure-resource-manager-condition]: /azure/azure-resource-manager/resource-manager-templates-resources#condition
[azure-resource-manager-variable]: /azure/azure-resource-manager/resource-group-authoring-templates#variables
[vnet-peering-resource-schema]: /azure/templates/microsoft.network/virtualnetworks/virtualnetworkpeerings
[cli]: /cli/azure/?view=azure-cli-latest
[github]: https://github.com/mspnp/template-examples
