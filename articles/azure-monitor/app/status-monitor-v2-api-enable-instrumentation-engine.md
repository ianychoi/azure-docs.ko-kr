---
title: 'Azure 상태 모니터 v2 API 참조: 계측 엔진을 사용 하도록 설정 | Microsoft Docs'
description: 상태 모니터 v2-InstrumentationEngine API 참조 사용 하도록 설정 합니다. 웹 사이트를 다시 배포 하지 않고 웹 사이트 성능을 모니터링 합니다. VM 또는 Azure의 온-프레미스에서 호스트되는 ASP.NET 웹앱으로 작업합니다.
services: application-insights
documentationcenter: .net
author: MS-TimothyMothra
manager: alexklim
ms.assetid: 769a5ea4-a8c6-4c18-b46c-657e864e24de
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: tilee
ms.openlocfilehash: d886aa364ca928d32100c570689f13beb0c682c9
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65143429"
---
# <a name="status-monitor-v2-api-enable-instrumentationengine-v021-alpha"></a>상태 모니터 v2 API: 사용-InstrumentationEngine (v0.2.1 알파)

이 문서에서는의 구성원으로 제공 되는 cmdlet을 설명 합니다 [Az.ApplicationMonitor PowerShell 모듈](https://www.powershellgallery.com/packages/Az.ApplicationMonitor/)합니다.

> [!IMPORTANT]
> 상태 모니터 v2는 현재 공개 미리 보기로 제공 됩니다.
> 이 미리 보기 버전은 서비스 수준 계약 없이 제공되며 프로덕션 워크로드에는 사용하지 않는 것이 좋습니다. 특정 기능이 지원되지 않거나 기능이 제한될 수 있습니다.
> 자세한 내용은 참조 하세요. [추가 사용 약관에 대 한 Microsoft Azure 미리 보기](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)

## <a name="description"></a>설명

이 cmdlet을 일부 레지스트리 키를 설정 하 여 계측 엔진을 사용 합니다.
이러한 변경 내용을 적용 하려면 IIS를 다시 시작 합니다.

계측 엔진은.NET Sdk를 통해 수집 된 데이터를 보완할 수 있습니다.
이벤트 및 메시지는 수집 관리 되는 프로세스의 실행을 설명 하는. 종속성 결과 코드, HTTP 동사 및 SQL 명령 텍스트에 제한 되지 않고 포함 됩니다. 

계측 엔진을 사용 하도록 설정 하는 경우:
- 모니터링 사용 cmdlet을 사용 하 여 이미 활성화 해도 InstrumentationEngine를 사용 하지 않도록 합니다.
- 수동으로.NET Sdk를 사용 하 여 응용 프로그램 계측을 추가 원격 분석을 수집 하려면.

> [!IMPORTANT] 
> 이 cmdlet은 관리자 권한으로 PowerShell 세션에 필요합니다.

> [!NOTE] 
> 이 cmdlet를 검토 하 고이 라이선스 및 개인정보취급방침 적용 해야 합니다.

> [!NOTE] 
> 계측 엔진 추가 오버 헤드를 추가 및 기본적으로 해제 되어 있습니다.

## <a name="examples"></a>예

```powershell
PS C:\> Enable-InstrumentationEngine
```

## <a name="parameters"></a>매개 변수 

### <a name="-acceptlicense"></a>-AcceptLicense
**선택** 헤드리스 설치에서 라이선스 및 개인정보취급방침 적용할이 스위치를 사용 합니다.

### <a name="-verbose"></a>-Verbose
**일반적인 매개 변수입니다.** 자세한 로그를 출력 하려면이 스위치를 사용 합니다.

## <a name="output"></a>출력


#### <a name="example-output-from-successfully-enabling-the-instrumentation-engine"></a>예제 출력에서 성공적으로 계측 엔진을 사용 하도록 설정

```
Configuring IIS Environment for instrumentation engine...
Configuring registry for instrumentation engine...
```

## <a name="next-steps"></a>다음 단계

  원격 분석 보기:
 - [메트릭을 탐색하여](../../azure-monitor/app/metrics-explorer.md) 성능 및 사용량을 모니터링합니다.
- [검색 이벤트 및 로그](../../azure-monitor/app/diagnostic-search.md) 문제 진단
- [분석](../../azure-monitor/app/analytics.md)을 통해 고급 쿼리를 수행합니다.
- [대시보드를 만듭니다](../../azure-monitor/app/app-insights-dashboards.md).
 
 원격 분석 더 추가:
 - [웹 테스트를 만들어](monitor-web-app-availability.md) 사이트가 라이브 상태로 유지 되도록 합니다.
- [웹 클라이언트 원격 분석 추가](../../azure-monitor/app/javascript.md) 웹 페이지 코드에서 예외를 확인 하 고 추적 호출을 삽입할 수 있도록 합니다.
- [코드에 Application Insights SDK 추가](../../azure-monitor/app/asp-net.md) 추적을 삽입 하 고 호출을 기록할 수 있도록
 
 상태 모니터 v2 사용 하 여 더 수행 합니다.
 - 가이드를 사용 하 여 [문제 해결](status-monitor-v2-troubleshoot.md) v2 상태 모니터입니다.
 - [구성 가져오기](status-monitor-v2-api-get-config.md) 설정을 올바르게 기록 된를 확인 합니다.
 - [상태를 가져오려면](status-monitor-v2-api-get-status.md) 모니터링 검사 합니다.
