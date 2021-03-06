---
title: Azure PowerShell 스크립트 샘플 - 저장소 계정 액세스 키 회전 | Microsoft Docs
description: Azure Storage 계정을 만들고 나서 해당 계정 액세스 키 중 하나를 검색 및 회전합니다.
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: ''
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/13/2017
ms.author: tamram
ms.openlocfilehash: 0aae733f94ab4a217ca41ca3de6df2e5da47df50
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 02/14/2019
ms.locfileid: "56244354"
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>저장소 계정 만들기 및 계정 액세스 키 회전

이 스크립트는 Azure Storage 계정을 만들고, 새 스토리지 계정의 기본 액세스 키를 표시하고 나서, 키를 갱신(회전)합니다.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install-no-ssh-az.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>샘플 스크립트

[!code-powershell[main](../../../powershell_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.ps1 "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>배포 정리 

다음 명령을 실행하여 리소스 그룹, 저장소 계정 및 모든 관련된 리소스를 제거합니다.

```powershell
Remove-AzResourceGroup -Name rotatekeystestrg
```

## <a name="script-explanation"></a>스크립트 설명

이 스크립트는 다음 명령을 사용하여 저장소 계정을 만들고 해당 액세스 키 중 하나를 검색 및 회전합니다. 표에 있는 각 항목은 명령 관련 설명서에 연결됩니다.

| 명령 | 메모 |
|---|---|
| [Get-AzLocation](/powershell/module/az.resources/get-azlocation) | 모든 위치 및 각 위치에 대한 지원되는 리소스 공급자를 가져옵니다. |
| [New-AzResourceGroup](/powershell/module/az.resources/new-azresourcegroup) | Azure 리소스 그룹을 만듭니다. |
| [New-AzStorageAccount](/powershell/module/az.storage/new-azstorageaccount) | Storage 계정을 만듭니다. |
| [Get-AzStorageAccountKey](/powershell/module/az.storage/get-azstorageaccountkey) | Azure Storage 계정의 선택키를 가져옵니다. |
| [New-AzStorageAccountKey](/powershell/module/az.storage/new-azstorageaccountkey) | Azure Storage 계정의 액세스 키를 다시 생성합니다. |

## <a name="next-steps"></a>다음 단계

Azure PowerShell 모듈에 대한 자세한 내용은 [Azure PowerShell 설명서](/powershell/azure/overview)를 참조하세요.

추가 스토리지 PowerShell 스크립트 샘플은 [Azure Blob Storage에 대한 PowerShell 샘플](../blobs/storage-samples-blobs-powershell.md)에서 찾을 수 있습니다.
