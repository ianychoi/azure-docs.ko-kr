---
title: 네트워크 트래픽 라우팅 - Azure PowerShell | Microsoft Docs
description: 이 문서에서는 PowerShell을 사용하여 경로 테이블이 포함된 네트워크 트래픽을 라우팅하는 방법을 알아봅니다.
services: virtual-network
documentationcenter: virtual-network
author: KumudD
manager: twooley
editor: ''
tags: azure-resource-manager
Customer intent: I want to route traffic from one subnet, to a different subnet, through a network virtual appliance.
ms.assetid: ''
ms.service: virtual-network
ms.devlang: ''
ms.topic: article
ms.tgt_pltfrm: virtual-network
ms.workload: infrastructure
ms.date: 03/13/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: bbb614f9a540131ea25f53b8d566a4a5ee8c197b
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64692272"
---
# <a name="route-network-traffic-with-a-route-table-using-powershell"></a>PowerShell을 사용하여 경로 테이블이 포함된 네트워크 트래픽 라우팅

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

기본적으로 Azure에서는 가상 네트워크 내의 모든 서브넷 간에 트래픽을 자동으로 라우팅합니다. 고유의 라우팅을 만들어 Azure의 기본 라우팅을 재정의할 수 있습니다. 사용자 지정 경로를 만드는 기능은 예를 들어 NVA(네트워크 가상 어플라이언스)를 통해 서브넷 간 트래픽을 라우팅하려는 경우에 유용합니다. 이 문서에서는 다음 방법을 설명합니다.

* 경로 테이블 만들기
* 경로 만들기
* 여러 서브넷이 있는 가상 네트워크 만들기
* 서브넷에 경로 테이블 연결
* 트래픽을 라우팅하는 NVA 만들기
* 다른 서브넷에 VM(가상 머신) 배포
* NVA를 통해 한 서브넷에서 다른 서브넷으로 트래픽 라우팅

Azure 구독이 아직 없는 경우 시작하기 전에 [체험 계정](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)을 만듭니다.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

이 문서에는 Azure PowerShell 모듈 버전 1.0.0 설치 하 고 PowerShell을 로컬로 사용 하려는 경우 이상. 설치되어 있는 버전을 확인하려면 `Get-Module -ListAvailable Az`을 실행합니다. 업그레이드해야 하는 경우 [Azure PowerShell 모듈 설치](/powershell/azure/install-az-ps)를 참조하세요. 또한 PowerShell을 로컬로 실행하는 경우 `Connect-AzAccount`를 실행하여 Azure와 연결해야 합니다.

## <a name="create-a-route-table"></a>경로 테이블 만들기

경로 테이블을 만들려면 먼저 사용 하 여 리소스 그룹을 만듭니다 [새로 만들기-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup)합니다. 다음 예제에서는 이 문서에서 만든 모든 리소스에 대해 *myResourceGroup*이라는 리소스 그룹을 만듭니다.

```azurepowershell-interactive
New-AzResourceGroup -ResourceGroupName myResourceGroup -Location EastUS
```

사용 하 여 경로 테이블을 만듭니다 [새로 만들기-AzRouteTable](/powershell/module/az.network/new-azroutetable)합니다. 다음 예제에서는 *myRouteTablePublic*이라는 경로 테이블을 만듭니다.

```azurepowershell-interactive
$routeTablePublic = New-AzRouteTable `
  -Name 'myRouteTablePublic' `
  -ResourceGroupName myResourceGroup `
  -location EastUS
```

## <a name="create-a-route"></a>경로 만들기

사용 하 여 경로 테이블 개체를 검색 하 여 경로 만듭니다 [Get AzRouteTable](/powershell/module/az.network/get-azroutetable)를 사용 하 여 경로 [추가 AzRouteConfig](/powershell/module/az.network/add-azrouteconfig)에 다음 경로 테이블에 경로 구성을 작성 [ 집합 AzRouteTable](/powershell/module/az.network/set-azroutetable)합니다.

```azurepowershell-interactive
Get-AzRouteTable `
  -ResourceGroupName "myResourceGroup" `
  -Name "myRouteTablePublic" `
  | Add-AzRouteConfig `
  -Name "ToPrivateSubnet" `
  -AddressPrefix 10.0.1.0/24 `
  -NextHopType "VirtualAppliance" `
  -NextHopIpAddress 10.0.2.4 `
 | Set-AzRouteTable
```

## <a name="associate-a-route-table-to-a-subnet"></a>서브넷에 경로 테이블 연결

서브넷에 경로 테이블을 연결하려면 먼저 가상 네트워크 및 서브넷을 만들어야 합니다. [New-AzVirtualNetwork](/powershell/module/az.network/new-azvirtualnetwork)를 사용하여 가상 네트워크를 만듭니다. 다음 예제는 주소 접두사 *10.0.0.0/16*을 사용하는 *myVirtualNetwork*라는 가상 네트워크를 만듭니다.

```azurepowershell-interactive
$virtualNetwork = New-AzVirtualNetwork `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name myVirtualNetwork `
  -AddressPrefix 10.0.0.0/16
```

사용 하 여 세 개의 서브넷 구성을 만들어 세 개의 서브넷을 만듭니다 [새로 만들기-AzVirtualNetworkSubnetConfig](/powershell/module/az.network/new-azvirtualnetworksubnetconfig)합니다. 다음 예제에서는 *공용*, *개인* 및 *DMZ* 서브넷에 대한 세 개의 서브넷 구성을 만듭니다.

```azurepowershell-interactive
$subnetConfigPublic = Add-AzVirtualNetworkSubnetConfig `
  -Name Public `
  -AddressPrefix 10.0.0.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigPrivate = Add-AzVirtualNetworkSubnetConfig `
  -Name Private `
  -AddressPrefix 10.0.1.0/24 `
  -VirtualNetwork $virtualNetwork

$subnetConfigDmz = Add-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -AddressPrefix 10.0.2.0/24 `
  -VirtualNetwork $virtualNetwork
```

가상 네트워크에 서브넷 구성을 씁니다 [집합 AzVirtualNetwork](/powershell/module/az.network/Set-azVirtualNetwork), 가상 네트워크에 서브넷을 만듭니다.

```azurepowershell-interactive
$virtualNetwork | Set-AzVirtualNetwork
```

연결 합니다 *myRouteTablePublic* 경로 테이블을를 *공용* 서브넷 [집합 AzVirtualNetworkSubnetConfig](/powershell/module/az.network/set-azvirtualnetworksubnetconfig) 서브넷 구성을 작성 합니다 사용 하 여 가상 네트워크 [집합 AzVirtualNetwork](/powershell/module/az.network/set-azvirtualnetwork)합니다.

```azurepowershell-interactive
Set-AzVirtualNetworkSubnetConfig `
  -VirtualNetwork $virtualNetwork `
  -Name 'Public' `
  -AddressPrefix 10.0.0.0/24 `
  -RouteTable $routeTablePublic | `
Set-AzVirtualNetwork
```

## <a name="create-an-nva"></a>NVA 만들기

NVA는 라우팅, 방화벽 또는 WAN 최적화와 같은 네트워크 기능을 수행하는 VM입니다.

VM을 만들기 전에 네트워크 인터페이스를 만듭니다.

### <a name="create-a-network-interface"></a>네트워크 인터페이스 만들기

네트워크 인터페이스를 만들기 전에 검색 해야 할 가상 네트워크 Id를 사용 하 여 [Get AzVirtualNetwork](/powershell/module/az.network/get-azvirtualnetwork), 서브넷 Id 사용 하 여 [Get AzVirtualNetworkSubnetConfig](/powershell/module/az.network/get-azvirtualnetworksubnetconfig)합니다. 사용 하 여 네트워크 인터페이스 만들기 [새로 만들기-AzNetworkInterface](/powershell/module/az.network/new-aznetworkinterface) 에 *DMZ* IP 전달 사용 하도록 설정 된 서브넷:

```azurepowershell-interactive
# Retrieve the virtual network object into a variable.
$virtualNetwork=Get-AzVirtualNetwork `
  -Name myVirtualNetwork `
  -ResourceGroupName myResourceGroup

# Retrieve the subnet configuration into a variable.
$subnetConfigDmz = Get-AzVirtualNetworkSubnetConfig `
  -Name DMZ `
  -VirtualNetwork $virtualNetwork

# Create the network interface.
$nic = New-AzNetworkInterface `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -Name 'myVmNva' `
  -SubnetId $subnetConfigDmz.Id `
  -EnableIPForwarding
```

### <a name="create-a-vm"></a>VM 만들기

VM을 만들고 기존 네트워크 인터페이스에 연결 하려면 먼저 만들어야로 VM 구성을 [새로 만들기-AzVMConfig](/powershell/module/az.compute/new-azvmconfig)합니다. 구성에는 이전 단계에서 만든 네트워크 인터페이스가 포함됩니다. 사용자 이름 및 암호에 대한 메시지가 나타나면 VM에 로그인할 사용자 이름과 암호를 선택합니다.

```azurepowershell-interactive
# Create a credential object.
$cred = Get-Credential -Message "Enter a username and password for the VM."

# Create a VM configuration.
$vmConfig = New-AzVMConfig `
  -VMName 'myVmNva' `
  -VMSize Standard_DS2 | `
  Set-AzVMOperatingSystem -Windows `
    -ComputerName 'myVmNva' `
    -Credential $cred | `
  Set-AzVMSourceImage `
    -PublisherName MicrosoftWindowsServer `
    -Offer WindowsServer `
    -Skus 2016-Datacenter `
    -Version latest | `
  Add-AzVMNetworkInterface -Id $nic.Id
```

사용 하 여 VM 구성을 사용 하 여 VM을 만듭니다 [New-azvm](/powershell/module/az.compute/new-azvm)합니다. 다음 예제에서는 *myVmNva*라는 VM을 만듭니다.

```azurepowershell-interactive
$vmNva = New-AzVM `
  -ResourceGroupName myResourceGroup `
  -Location EastUS `
  -VM $vmConfig `
  -AsJob
```

`-AsJob` 옵션은 백그라운드에서 VM을 만들기 때문에 다음 단계를 계속 진행할 수 있습니다.

## <a name="create-virtual-machines"></a>가상 머신 만들기

이후 단계에서 *공용* 서브넷의 해당 트래픽이 네트워크 가상 어플라이언스를 통해 *개인* 서브넷에 라우팅되는지 유효성을 검사할 수 있도록 가상 네트워크에 두 개의 VM을 만듭니다.

에 VM을 만듭니다는 *공개* 사용 하 여 서브넷 [New-azvm](/powershell/module/az.compute/new-azvm)합니다. 다음 예제는 *myVirtualNetwork* 가상 네트워크의 *공용* 서브넷에 *myVmPublic*이라는 VM을 만듭니다.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Public" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPublic" `
  -AsJob
```

*개인* 서브넷에 VM을 만듭니다.

```azurepowershell-interactive
New-AzVm `
  -ResourceGroupName "myResourceGroup" `
  -Location "East US" `
  -VirtualNetworkName "myVirtualNetwork" `
  -SubnetName "Private" `
  -ImageName "Win2016Datacenter" `
  -Name "myVmPrivate"
```

VM을 만드는 데 몇 분이 걸립니다. VM이 만들어지고 Azure에서 PowerShell로 출력을 반환할 때까지 나머지 단계를 진행하지 마십시오.

## <a name="route-traffic-through-an-nva"></a>NVA를 통한 트래픽 라우팅

사용 하 여 [Get AzPublicIpAddress](/powershell/module/az.network/get-azpublicipaddress) 의 공용 IP 주소를 반환 합니다 *myVmPrivate* VM. 다음 예제에서는 *myVmPrivate* VM의 공용 IP 주소를 반환합니다.

```azurepowershell-interactive
Get-AzPublicIpAddress `
  -Name myVmPrivate `
  -ResourceGroupName myResourceGroup `
  | Select IpAddress
```

다음 명령을 사용하여 로컬 컴퓨터에서 *myVmPrivate* VM과의 원격 데스크톱 세션을 만듭니다. `<publicIpAddress>`를 이전 명령에서 반환된 IP 주소로 바꿉니다.

```
mstsc /v:<publicIpAddress>
```

다운로드한 RDP 파일을 엽니다. 메시지가 표시되면 **연결**을 선택합니다.

VM을 만들 때 지정한 사용자 이름과 암호를 입력(VM을 만들 때 입력한 자격 증명을 지정하기 위해 **다른 옵션 선택**을 선택한 다음, **다른 계정 사용**을 선택해야 할 수도 있음)한 다음, **확인**을 선택합니다. 로그인 프로세스 중에 인증서 경고가 나타날 수 있습니다. **예**를 선택하여 연결을 진행합니다.

이후 단계에서의 `tracert.exe` 명령은 라우팅을 테스트 하는 데 사용 됩니다. Tracert는 ICMP(Internet Control Message Protocol)를 사용하는데, ICMP는 Windows 방화벽에서 허용되지 않습니다. *myVmPrivate* VM의 PowerShell에서 다음 명령을 입력하여 Windows 방화벽을 통해 ICMP를 사용하도록 설정합니다.

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4-In" -Protocol ICMPv4
```

이 문서에서는 라우팅을 테스트하는 데 경로 추적이 사용되지만, 프로덕션 배포에 Windows 방화벽을 통한 ICMP를 허용하지 않는 것이 좋습니다.

IP 전달을 사용하도록 설정에서 VM의 네트워크 인터페이스에 대해 Azure 내에서 IP 전달을 사용하도록 설정했습니다. VM 내 운영 체제 또는 VM 내에서 실행 중인 애플리케이션도 네트워크 트래픽을 전달할 수 있어야 합니다. *myVmNva*의 운영 체제 내에서 IP 전달을 사용하도록 설정합니다.

*myVmPrivate* VM의 명령 프롬프트에서 다음과 같이 *myVmNva*에 대한 원격 데스크톱 연결을 만듭니다.

``` 
mstsc /v:myvmnva
```

운영 체제 내에서 IP 전달을 사용하도록 설정하려면 *myVmNva* VM의 PowerShell에서 다음 명령을 입력합니다.

```powershell
Set-ItemProperty -Path HKLM:\SYSTEM\CurrentControlSet\Services\Tcpip\Parameters -Name IpEnableRouter -Value 1
```

*myVmNva* VM을 다시 시작합니다. 그러면 원격 데스크톱 세션의 연결도 끊어집니다.

아직 *myVmPrivate* VM에 연결된 상태에서, *myVmNva* VM이 다시 시작된 후 *myVmPublic* VM에 대한 원격 데스크톱 세션을 만듭니다.

``` 
mstsc /v:myVmPublic
```

*myVmPublic* VM의 PowerShell에서 다음 명령을 입력하여 Windows 방화벽을 통해 ICMP를 사용하도록 설정합니다.

```powershell
New-NetFirewallRule –DisplayName “Allow ICMPv4-In” –Protocol ICMPv4
```

*myVmPublic* VM에서 *myVmPrivate* VM으로의 네트워크 트래픽 라우팅을 테스트하려면 *myVmPublic* VM의 PowerShell에서 다음 명령을 입력합니다.

```
tracert myVmPrivate
```

응답은 다음 예제와 유사합니다.

```
Tracing route to myVmPrivate.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.1.4]
over a maximum of 30 hops:

1    <1 ms     *        1 ms  10.0.2.4
2     1 ms     1 ms     1 ms  10.0.1.4

Trace complete.
```

첫 번째 홉이 NVA의 개인 IP 주소인 10.0.2.4인 것을 확인할 수 있습니다. 두 번째 홉은 *myVmPrivate* VM의 개인 IP 주소인 10.0.1.4입니다. 경로가 *myRouteTablePublic* 경로 테이블에 추가되고 *공용* 서브넷에 연결되었으므로 Azure는 트래픽을 직접 *개인* 서브넷에 라우팅하는 대신 NVA를 통해 트래픽을 라우팅합니다.

*myVmPublic* VM에 대한 원격 데스크톱 세션을 닫습니다. 그러면 *myVmPrivate* VM에 연결된 상태가 유지됩니다.

*myVmPrivate* VM에서 *myVmPublic* VM으로의 네트워크 트래픽 라우팅을 테스트하려면 *myVmPrivate* VM의 명령 프롬프트에서 다음 명령을 입력합니다.

```
tracert myVmPublic
```

응답은 다음 예제와 유사합니다.

```
Tracing route to myVmPublic.vpgub4nqnocezhjgurw44dnxrc.bx.internal.cloudapp.net [10.0.0.4]
over a maximum of 30 hops:

1     1 ms     1 ms     1 ms  10.0.0.4

Trace complete.
```

트래픽이 *myVmPrivate* VM에서 *myVmPublic* VM으로 직접 라우팅되는지 볼 수 있습니다. 기본적으로 Azure는 트래픽을 서브넷 간에 직접 라우팅합니다.

*myVmPrivate* VM에 대한 원격 데스크톱 세션을 닫습니다.

## <a name="clean-up-resources"></a>리소스 정리

사용 하 여 더 이상 필요 [제거 AzResourcegroup](/powershell/module/az.resources/remove-azresourcegroup) 리소스 그룹 및 포함 된 리소스를 모두 제거 합니다.

```azurepowershell-interactive
Remove-AzResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>다음 단계

이 문서에서는 경로 테이블을 만들고 서브넷에 연결했습니다. 공용 서브넷에서 개인 서브넷으로 트래픽을 라우팅하는 간단한 네트워크 가상 어플라이언스를 만들었습니다. 이제 [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking)에서 방화벽 및 WAN 최적화 같은 네트워크 기능을 수행하는 다양한 미리 구성된 네트워크 가상 어플라이언스를 배포합니다. 라우팅에 대한 자세한 내용은 [라우팅 개요](virtual-networks-udr-overview.md) 및 [경로 테이블 관리](manage-route-table.md)를 참조하세요.

가상 네트워크 내에서 여러 Azure 리소스를 배포할 수 있는 반면, 일부 Azure PaaS 서비스에 대한 리소스는 가상 네트워크에 배포할 수 없습니다. 하지만 일부 Azure PaaS 서비스의 리소스에 대한 액세스를 가상 네트워크 서브넷의 트래픽만으로 제한할 수 있습니다. 방법을 알아보려면 [PaaS 리소스에 대한 네트워크 액세스 제한](tutorial-restrict-network-access-to-resources-powershell.md)을 참조하세요.