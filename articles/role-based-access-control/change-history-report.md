---
title: Azure 리소스에 대한 RBAC 변경 내용의 활동 로그 보기 | Microsoft Docs
description: 지난 90일 동안의 Azure 리소스에 대한 RBAC(역할 기반 액세스 제어) 변경 내용의 활동 로그를 봅니다.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 2bc68595-145e-4de3-8b71-3a21890d13d9
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/02/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b808654baded5bbe721866441a8d1115eff7bcaa
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60308895"
---
# <a name="view-activity-logs-for-rbac-changes-to-azure-resources"></a>Azure 리소스에 대한 RBAC 변경 내용의 활동 로그 보기

때로는 감사, 문제 해결 등의 목적으로 Azure 리소스에 대한 RBAC(역할 기반 액세스 제어) 변경 내용과 관련된 정보가 필요합니다. 구독 내에서 누군가가 역할 할당 또는 역할 정의를 변경할 때마다 변경 내용이 [Azure 활동 로그](../azure-monitor/platform/activity-logs-overview.md)에 기록됩니다. 활동 로그를 보고 지난 90일 동안의 RBAC 변경 사항을 모두 확인할 수 있습니다.

## <a name="operations-that-are-logged"></a>기록되는 작업

다음은 활동 로그에 기록되는 RBAC 관련 작업입니다.

- 역할 할당 만들기
- 역할 할당 삭제
- 사용자 지정 역할 정의 만들기 또는 업데이트
- 사용자 지정 역할 정의 삭제

## <a name="azure-portal"></a>Azure portal

가장 손쉽게 시작할 수 있는 방법은 Azure Portal을 사용하여 활동 로그를 보는 것입니다. 다음 스크린샷은 역할 할당 및 역할 정의 작업을 표시하도록 필터링된 활동 로그의 예를 보여 줍니다. 또한 로그를 CSV 파일로 다운로드하는 링크도 포함되어 있습니다.

![포털을 사용한 활동 로그 - 스크린샷](./media/change-history-report/activity-log-portal.png)

포털에서 활동 로그에는 몇 가지 필터가 있습니다. 다음은 RBAC 관련 필터입니다.

|필터  |Value  |
|---------|---------|
|이벤트 범주     | <ul><li>관리</li></ul>         |
|작업(Operation)     | <ul><li>역할 할당 만들기</li> <li>역할 할당 삭제</li> <li>사용자 지정 역할 정의 만들기 또는 업데이트</li> <li>사용자 지정 역할 정의 삭제</li></ul>      |


활동 로그에 대한 자세한 내용은 [활동 로그의 이벤트 보기](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)를 참조하세요.

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [az-powershell-update](../../includes/updated-for-az.md)]

Azure PowerShell을 사용하여 활동 로그를 보려면 [Get-AzLog](/powershell/module/Az.Monitor/Get-AzLog) 명령을 사용합니다.

이 명령은 지난 7일 동안 구독에서 발생한 모든 역할 할당 변경 내용을 나열합니다.

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleAssignments/*'}
```

이 명령은 지난 7일 동안 리소스 그룹에서 발생한 모든 역할 정의 변경 내용을 나열합니다.

```azurepowershell
Get-AzLog -ResourceGroupName pharma-sales -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/roleDefinitions/*'}
```

이 명령은 지난 7일 동안 구독에서 발생한 모든 역할 할당 및 역할 정의 변경 내용을 나열하고 결과를 목록에 표시합니다.

```azurepowershell
Get-AzLog -StartTime (Get-Date).AddDays(-7) | Where-Object {$_.Authorization.Action -like 'Microsoft.Authorization/role*'} | Format-List Caller,EventTimestamp,{$_.Authorization.Action},Properties
```

```Example
Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:07 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          statusCode     : Created
                          serviceRequestId: 11111111-1111-1111-1111-111111111111

Caller                  : alain@example.com
EventTimestamp          : 4/20/2018 9:18:05 PM
$_.Authorization.Action : Microsoft.Authorization/roleAssignments/write
Properties              :
                          requestbody    : {"Id":"22222222-2222-2222-2222-222222222222","Properties":{"PrincipalId":"33333333-3333-3333-3333-333333333333","RoleDefinitionId":"/subscriptions/00000000-0000-0000-0000-000000000000/providers
                          /Microsoft.Authorization/roleDefinitions/b24988ac-6180-42a0-ab88-20f7382dd24c","Scope":"/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/pharma-sales"}}

```

## <a name="azure-cli"></a>Azure CLI

Azure CLI를 사용하여 활동 로그를 보려면 [az monitor activity-log list](/cli/azure/monitor/activity-log#az-monitor-activity-log-list) 명령을 사용합니다.

이 명령은 시작 시간 이후 리소스 그룹의 활동 로그를 나열합니다.

```azurecli
az monitor activity-log list --resource-group pharma-sales --start-time 2018-04-20T00:00:00Z
```

이 명령은 시작 시간 이후 인증 리소스 공급자의 활동 로그를 나열합니다.

```azurecli
az monitor activity-log list --resource-provider "Microsoft.Authorization" --start-time 2018-04-20T00:00:00Z
```

## <a name="azure-monitor-logs"></a>Azure Monitor 로그

[Azure Monitor 로그](../log-analytics/log-analytics-overview.md) 수집 하 고 모든 Azure 리소스에 대 한 RBAC 변경을 분석 하 여 다른 도구입니다. Azure Monitor 로그에는 다음과 같은 이점이 있습니다.

- 복잡한 쿼리 및 로직 작성
- 경고, Power BI 및 기타 도구와 통합
- 장기 보존 기간 동안 데이터 저장
- 보안, 가상 머신, 사용자 지정과 같은 다른 로그와 상호 참조

기본적인 시작 단계는 다음과 같습니다.

1. [Log Analytics 작업 영역을 만듭니다](../azure-monitor/learn/quick-create-workspace.md).

1. 작업 영역에 대해 [활동 로그 분석 솔루션을 구성](../azure-monitor/platform/collect-activity-logs.md#configuration)합니다.

1. [활동 로그를 봅니다](../azure-monitor/platform/collect-activity-logs.md#using-the-solution). Activity Log Analytics 솔루션 개요 페이지로 이동 하는 빠른 방법은 클릭 하는 것은 **Log Analytics** 옵션입니다.

   ![포털에서 azure Monitor 로그 옵션](./media/change-history-report/azure-log-analytics-option.png)

1. 선택적으로 [로그 검색](../log-analytics/log-analytics-log-search.md) 페이지 또는 [고급 분석 포털](../azure-monitor/log-query/get-started-portal.md)을 사용하여 로그를 쿼리하고 봅니다. 이러한 두 옵션에 대한 자세한 내용은 [로그 검색 페이지 또는 고급 분석 포털](../azure-monitor/log-query/portals.md)을 참조하세요.

다음은 대상 리소스 공급자가 구성한 새 역할 할당을 반환하는 쿼리입니다.

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments/write" and ActivityStatus == "Succeeded"
| parse ResourceId with * "/providers/" TargetResourceAuthProvider "/" *
| summarize count(), makeset(Caller) by TargetResourceAuthProvider
```

다음은 차트에 표시되는 역할 할당 변경 내용을 반환하는 쿼리입니다.

```
AzureActivity
| where TimeGenerated > ago(60d) and OperationName startswith "Microsoft.Authorization/roleAssignments"
| summarize count() by bin(TimeGenerated, 1d), OperationName
| render timechart
```

![고급 분석 포털을 사용한 활동 로그 - 스크린샷](./media/change-history-report/azure-log-analytics.png)

## <a name="next-steps"></a>다음 단계
* [활동 로그의 이벤트 보기](/azure/azure-resource-manager/resource-group-audit?toc=%2fazure%2fmonitoring-and-diagnostics%2ftoc.json)
* [Azure 활동 로그로 구독 활동 모니터링](/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs)
