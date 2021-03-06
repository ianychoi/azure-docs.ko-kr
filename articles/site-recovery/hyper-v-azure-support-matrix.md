---
title: 온-프레미스 Hyper-V VM과 Azure 간 재해 복구를 위한 지원 매트릭스 | Microsoft Docs
description: Azure Site Recovery를 사용한 Hyper-V VM과 Azure 간 재해 복구를 위해 지원되는 구성 요소 및 요구 사항이 요약되어 있습니다.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 05/07/2019
ms.author: raynew
ms.openlocfilehash: e311a328c1c3d78fa8e5ba7065dcc6484006eaaf
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65235887"
---
# <a name="support-matrix-for-disaster-recovery-of-on-premises-hyper-v-vms-to-azure"></a>온-프레미스 Hyper-V VM과 Azure 간 재해 복구를 위한 지원 매트릭스


이 문서에서는 [Azure Site Recovery](site-recovery-overview.md)를 사용하여 온-프레미스 Hyper-V VM을 Azure로 재해 복구하는 데 지원되는 구성 요소와 설정의 요약 정보를 제공합니다.


## <a name="supported-scenarios"></a>지원되는 시나리오

**시나리오** | **세부 정보**
--- | ---
Hyper-V(Virtual Machine Manager 있음) | System Center Virtual Machine Manager 패브릭에서 관리되는 Hyper-V 호스트에서 실행 중인 VM에 대해 Azure로의 재해 복구를 수행할 수 있습니다.<br/><br/> Azure Portal에서 또는 PowerShell을 사용하여 이 시나리오를 배포할 수 있습니다.<br/><br/> Virtual Machine Manager에서 Hyper-V 호스트를 관리하는 경우에는 보조 온-프레미스 사이트로의 재해 복구도 수행할 수 있습니다. 이 시나리오에 대해 자세히 알아보려면 [이 자습서](hyper-v-vmm-disaster-recovery.md)를 읽어보세요.
Hyper-V(Virtual Machine Manager 없음) | Virtual Machine Manager에서 관리하지 않는 Hyper-V 호스트에서 실행 중인 VM에 대해 Azure로의 재해 복구를 수행할 수 있습니다.<br/><br/> Azure Portal에서 또는 PowerShell을 사용하여 이 시나리오를 배포할 수 있습니다.


## <a name="on-premises-servers"></a>온-프레미스 서버

**서버** | **요구 사항** | **세부 정보**
--- | --- | ---
Hyper-V(Virtual Machine Manager 없이 실행 중) |  Windows Server 2019, Windows Server 2016 (포함 하 여 server core 설치), 최신 업데이트가 포함 된 Windows Server 2012 R2 | Azure Site Recovery 및 플랜을 사용하여 Windows Server 2012 R2 또는 SCVMM 2012 R2를 이미 구성했고 OS를 업그레이드할 계획이라면 지침 [설명서](upgrade-2012R2-to-2016.md)를 따르세요. 
Hyper-V(Virtual Machine Manager로 실행 중) | Virtual Machine Manager 2019, Virtual Machine Manager 2016, Virtual Machine Manager 2012 R2 | Virtual Machine Manager 사용 하는 경우 2019 Virtual Machine Manager에서 Windows Server 2019 호스트를 관리 합니다. 마찬가지로, Windows Server 2016 호스트는 Virtual Machine Manager 2016에서 관리 되어야 합니다.<br/><br/>


## <a name="replicated-vms"></a>복제된 VM


다음 표에는 VM 지원 정보가 요약되어 있습니다. Site Recovery는 지원되는 운영 체제에서 실행되는 모든 워크로드를 지원합니다.

 **구성 요소** | **세부 정보**
--- | ---
VM 구성 | Azure로 복제하는 VM은 [Azure 요구 사항](#azure-vm-requirements)을 충족해야 합니다.
게스트 운영 체제 | [Azure에 지원되는](https://docs.microsoft.com/azure/cloud-services/cloud-services-guestos-update-matrix#family-5-releases) 모든 게스트 OS.<br/><br/> Windows Server 2016 Nano Server는 지원되지 않습니다.


## <a name="vmdisk-management"></a>VM/디스크 관리

**작업** | **세부 정보**
--- | ---
복제된 Hyper-V VM에서 디스크 크기 조정 | 지원되지 않습니다. 복제를 사용하지 않도록 설정하고, 변경 작업을 수행하고, VM에 복제를 다시 사용하도록 설정.
복제된 Hyper-V VM에서 디스크 추가 | 지원되지 않습니다. 복제를 사용하지 않도록 설정하고, 변경 작업을 수행하고, VM에 복제를 다시 사용하도록 설정.

## <a name="hyper-v-network-configuration"></a>Hyper-V 네트워크 구성

**구성 요소** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
호스트 네트워크: NIC 팀 | 예. | 예.
호스트 네트워크: VLAN | 예. | 예.
호스트 네트워크: IPv4 | 예 | 예.
호스트 네트워크: IPv6 | 아닙니다. | 아닙니다.
게스트 VM 네트워크: NIC 팀 | 아닙니다. | 아닙니다.
게스트 VM 네트워크: IPv4 | 예. | 예.
게스트 VM 네트워크: IPv6 | 아닙니다. | 예.
게스트 VM 네트워크: 고정 IP(Windows) | 예. | 예.
게스트 VM 네트워크: 고정 IP(Linux) | 아닙니다. | 아닙니다.
게스트 VM 네트워크: 다중 NIC | 예. | 예



## <a name="azure-vm-network-configuration-after-failover"></a>장애 조치(failover) 이후의 Azure VM 네트워크 구성

**구성 요소** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
Azure ExpressRoute | 예. | 예
ILB | 예. | 예.
ELB | 예. | 예.
Azure Traffic Manager | 예. | 예
다중 NIC | 예. | 예.
예약된 IP | 예. | 예.
IPv4 | 예. | 예.
원본 IP 주소 유지 | 예. | 예.
Azure Virtual Network 서비스 엔드포인트<br/> (Azure Storage 방화벽 없음) | 예. | 예.
가속 네트워킹 | 아닙니다. | 아닙니다.


## <a name="hyper-v-host-storage"></a>Hyper-V 호스트 저장소

**Storage** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | --- 
NFS | 해당 없음 | 해당 없음
SMB 3.0 | yes | 예
SAN(ISCSI) | 예 | 예.
다중 경로(MPIO). 테스트에 사용된 소프트웨어:<br></br> Microsoft DSM, EMC PowerPath 5.7 SP4<br/><br/> EMC PowerPath DSM for CLARiiON | 예 | 예.

## <a name="hyper-v-vm-guest-storage"></a>Hyper-V VM 게스트 저장소

**Storage** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
VMDK | 해당 없음 | 해당 없음
VHD/VHDX | 예 | 예.
2세대 VM | 예 | 예
EFI/UEFI| 예 | 예
공유 클러스터 디스크 | 아닙니다. | 아닙니다.
암호화된 디스크 | 아닙니다. | 아닙니다.
NFS | 해당 없음 | 해당 없음
SMB 3.0 | 아닙니다. | 아닙니다.
RDM | 해당 없음 | 해당 없음
디스크 1TB 이상 | 예, 최대 4,095GB | 예, 최대 4,095GB
디스크: 4K 논리/실제 섹터 | 지원되지 않음: 1세대/2세대 | 지원되지 않음: 1세대/2세대
디스크: 4K 논리/512바이트 물리 섹터 | 예. |  예
논리 볼륨 관리(LVM)입니다. LVM은 데이터 디스크에서만 지원됩니다. Azure는 단일 OS 디스크만 제공합니다. | 예. | 예.
스트라이프 디스크 포함 볼륨 1TB 이상 | 예. | 예
저장소 공간 | 예. | 예
디스크 핫 추가/제거 | 아닙니다. | 아닙니다.
디스크 제외 | 예. | 예.
다중 경로(MPIO) | 예 | 예.

## <a name="azure-storage"></a>Azure Storage

**구성 요소** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
로컬 중복 저장소 | 예 | 예.
지역 중복 저장소 | 예. | 예.
읽기 액세스 지역 중복 저장소 | 예 | 예.
쿨 저장소 | 아닙니다. | 아닙니다.
핫 저장소| 아닙니다. | 아닙니다.
블록 Blob | 아닙니다. | 아닙니다.
미사용 암호화(SSE)| 예 | 예.
Premium Storage | 예. | 예.
Import/Export 서비스 | 아닙니다. | 아닙니다.
대상 스토리지/캐시 스토리지(복제 데이터 저장에 사용됨) 계정에 구성된 가상 네트워크용 Azure Storage 방화벽 | 아닙니다. | 아닙니다.


## <a name="azure-compute-features"></a>Azure 계산 기능

**기능** | **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
가용성 집합 | 예. | 예.
HUB | 예 | 예.  
관리 디스크 | 예, 장애 조치(failover)용<br/><br/> 관리 디스크 장애 복구(failback)는 지원되지 않음 | 예, 장애 조치(failover)용<br/><br/> 관리 디스크 장애 복구(failback)는 지원되지 않음

## <a name="azure-vm-requirements"></a>Azure VM 요구 사항

Azure로 복제하는 온-프레미스 VM은 이 표에 요약되어 있는 Azure VM 요구 사항을 충족해야 합니다.

**구성 요소** | **요구 사항** | **세부 정보**
--- | --- | ---
게스트 운영 체제 | Site Recovery에서는 [Azure에서 지원](https://technet.microsoft.com/library/cc794868%28v=ws.10%29.aspx)하는 모든 운영 체제를 지원합니다.  | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
게스트 운영 체제 아키텍처 | 64비트 | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
운영 체제 디스크 크기 | 1세대 VM의 경우 최대 2,048GB<br/><br/> 2세대 VM의 경우 최대 300GB  | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
운영 체제 디스크 수 | 1 | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
데이터 디스크 수 | 16개 이하  | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
데이터 디스크 VHD 크기 | 최대 4,095GB | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
네트워크 어댑터 | 여러 어댑터가 지원됩니다. |
공유 VHD | 지원되지 않음 | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
FC 디스크 | 지원되지 않음 | 지원되지 않는 경우 필수 구성 요소 확인이 실패함
하드 디스크 형식 | VHD <br/><br/>  VHDX | Azure로의 장애 조치(failover)를 수행하면 Site Recovery는 VHDX를 VHD로 자동 변환합니다. 온-프레미스에 장애 복구 시 가상 머신에서 계속해서 VHDX 형식을 사용합니다.
BitLocker | 지원되지 않음 | VM의 복제를 사용하도록 설정하기 전에 BitLocker를 사용하지 않도록 설정해야 합니다.
VM 이름 | 1 자에서 63자 사이입니다. 문자, 숫자 및 하이픈으로 제한됩니다. VM 이름은 문자 또는 숫자로 시작하고 끝나야 합니다. | Site Recovery에서 VM 속성의 값을 업데이트합니다.
VM 형식 | 1세대<br/><br/> 2세대--Windows | 기본 OS 디스크 형식이 있는 2세대 VM(VHDX로 포맷된 한 개 또는 두 개의 데이터 볼륨을 포함) 및 300GB 미만의 디스크 공간이 지원됩니다.<br></br>Linux 2세대 VM은 지원되지 않습니다. [자세히 알아보기](https://azure.microsoft.com/blog/2015/04/28/disaster-recovery-to-azure-enhanced-and-were-listening/).|

## <a name="recovery-services-vault-actions"></a>Recovery Services 자격 증명 모음 작업

**작업** |  **Hyper-V(Virtual Machine Manager 있음)** | **Hyper-V(Virtual Machine Manager 없음)**
--- | --- | ---
리소스 그룹 간 자격 증명 모음 이동<br/><br/> 구독 내 및 구독 간 | 아닙니다. | 아닙니다.
저장소 그룹 간 저장소, 네트워크, Azure VM 이동<br/><br/> 구독 내 및 구독 간 | 아닙니다. | 아닙니다.

> [!NOTE]
> 온-프레미스에서 Azure로 Hyper-VM(SCVMM을 사용하거나 사용하지 않고 관리)을 복제할 때 하나의 특정 환경(해당하는 경우 Hyper-V 사이트 또는 SCVMM)에서 하나의 AD 테넌트에만 복제할 수 있습니다.


## <a name="provider-and-agent"></a>공급자 및 에이전트

배포가 이 문서에 나와 있는 설정과 호환되도록 하려면 최신 공급자 및 에이전트 버전을 실행 중인지 확인합니다.

**Name** | **설명** | **세부 정보**
--- | --- | --- 
Azure Site Recovery 공급자 | 온-프레미스 서버와 Azure 간 통신 조정 <br/><br/> Hyper-V(Virtual Machine Manager 있음): Virtual Machine Manager 서버에 설치됨<br/><br/> Hyper-V(Virtual Machine Manager 없음): Hyper-V 호스트에 설치됨| 최신 버전: 5.1.2700.1(Azure Portal에서 제공됨)<br/><br/> [최신 기능 및 수정](https://support.microsoft.com/help/4091311/update-rollup-23-for-azure-site-recovery)
Microsoft Azure Recovery Services 에이전트 | Hyper-V VM과 Azure 간 복제 조정<br/><br/> 온-프레미스 Hyper-V 서버에 설치됨(Virtual Machine Manager 있음/없음) | 포털에서 사용할 수 있는 최신 에이전트






## <a name="next-steps"></a>다음 단계
온-프레미스 Hyper-V VM 재해 복구용으로 [Azure를 준비](tutorial-prepare-azure.md)하는 방법을 알아봅니다.
