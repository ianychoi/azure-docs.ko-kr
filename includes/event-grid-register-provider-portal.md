---
title: 포함 파일
description: 포함 파일
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: include
ms.date: 07/05/2018
ms.author: tomfitz
ms.custom: include file
ms.openlocfilehash: c3677e7897498aa06d7bd547988ad4dc0326f39b
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66150445"
---
## <a name="enable-event-grid-resource-provider"></a>Event Grid 리소스 공급자를 사용하도록 설정

이전에 Azure 구독에서 Event Grid를 사용하지 않은 경우 Event Grid 리소스 공급자를 등록해야 할 수 있습니다.

Azure Portal에서 다음을 수행합니다.

1. **구독**을 선택합니다.
1. Event Grid에 대해 사용 중인 구독을 선택합니다.
1. **설정**에서 **리소스 공급자**를 선택합니다.
1. **Microsoft.EventGrid**를 찾습니다.
1. 등록되지 않은 경우 **등록**을 선택합니다. 

등록을 완료하는 데 잠시 시간이 걸릴 수 있습니다. **새로 고침**을 선택하여 상태를 업데이트합니다. **상태**가 **등록됨**이면 진행할 준비가 된 것입니다.