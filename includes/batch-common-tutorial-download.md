---
author: laurenhughes
ms.service: batch
ms.topic: include
ms.date: 11/09/2018
ms.author: lahugh
ms.openlocfilehash: bd51eabcff412de4928c682683b2a69b3e82e601
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66127351"
---
### <a name="retrieve-output-files"></a>출력 파일 검색

Azure Portal을 사용하여 ffmpeg 작업에서 생성된 출력 MP3 파일을 다운로드할 수 있습니다. 

1. **모든 서비스** > **저장소 계정**을 차례로 클릭한 다음, 저장소 계정의 이름을 클릭합니다.
2. **Blob** > *출력*을 차례로 클릭합니다.
3. 출력 MP3 파일 중 하나를 마우스 오른쪽 단추로 클릭한 다음, **다운로드**를 클릭합니다. 브라우저의 지시에 따라 파일을 열거나 저장합니다.

![출력 파일 다운로드](./media/batch-common-tutorial-download/download.png)

이 샘플에서는 표시되지 않지만, 계산 노드 또는 스토리지 컨테이너에서 프로그래밍 방식으로 파일을 다운로드할 수도 있습니다.
