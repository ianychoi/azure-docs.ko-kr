---
title: Azure Data Factory Mapping Data Flow 열 패턴
description: 기본 스키마 메타 데이터에 관계 없이 데이터 흐름에서 필드를 변환 하는 것에 대 한 일반화 된 템플릿을 패턴을 만드는 매핑 데이터 흐름에서 Azure 데이터 팩터리 열 패턴을 사용 하는 방법을 알아봅니다
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 01/30/2019
ms.openlocfilehash: 5131329f6675bc86374f5a5c081e0aaa7d36c0fe
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155231"
---
# <a name="azure-data-factory-mapping-data-flows-column-patterns"></a>Azure data factory 매핑 데이터 흐름 열 패턴

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

하드 코드된 열 이름 대신 패턴을 기준으로 템플릿 열을 만들 수 있도록 “열 패턴” 아이디어를 지원하는 Azure Data Factory Data Flow 변환도 있습니다. 식 작성기 내에서 이 기능을 사용하여 정확한 특정 필드 이름을 요구하는 대신 변환을 위한 열 일치에 사용할 패턴을 정의할 수 있습니다. 패턴은 텍스트 파일 또는 NoSQL 데이터베이스의 열을 변경 하는 경우에 특히 들어오는 원본 필드 자주 변경 하는 경우에 유용 합니다. 이 상태는 "스키마 드리프트" 라고도 합니다.

![열 패턴](media/data-flow/columnpattern2.png "열 패턴")

열 패턴은 스키마 드리프트 시나리오와 일반 시나리오 둘 다를 처리하는 데 유용합니다. 각 열 이름을 완전히 알 수 없는 상황에 적합합니다. 열 이름과 열 데이터 형식에 패턴 일치를 적용하고 `name` & `type` 패턴과 일치하는 데이터 스트림의 필드에 대해 해당 작업을 수행할 변환 식을 작성할 수 있습니다.

패턴을 허용하는 변환에 식을 추가하는 경우 “열 패턴 추가”를 선택합니다. 열 패턴을 사용하면 스키마 드리프트 열 일치 패턴이 허용됩니다.

템플릿 열 패턴을 빌드할 때는 식에 `$$`를 사용하여 입력 데이터 스트림에서 일치된 각 필드에 대한 참조를 나타냅니다.

식 작성기 regex 함수 중 하나를 사용하는 경우 나중에 $1, $2, $3...을 사용하여 regex 식에서 일치된 하위 패턴을 참조할 수 있습니다.

열 패턴 시나리오의 예는 일련의 수신 필드에 SUM을 사용하는 경우입니다. 집계 SUM 계산은 집계 변환에 있습니다. “정수”와 일치하는 필드 형식의 모든 일치 항목을 SUM한 다음, $$를 사용하여 각 일치 항목을 식에서 참조할 수 있습니다.
