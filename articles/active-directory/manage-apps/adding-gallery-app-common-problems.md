---
title: Azure AD 갤러리 애플리케이션을 추가하는 문제 | Microsoft Docs
description: Azure AD 갤러리 애플리케이션을 추가하는 경우 직면하는 일반적인 문제 및 문제를 해결하기 위해 수행할 수 있는 작업 이해
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 09/11/2018
ms.author: mimart
ms.collection: M365-identity-device-management
ms.openlocfilehash: b2b42880f99f3e87d75854166047896860f9eb14
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/16/2019
ms.locfileid: "65784435"
---
# <a name="problem-adding-an-azure-ad-gallery-application"></a>Azure AD 갤러리 애플리케이션을 추가하는 문제

이 아티클은 Azure AD 갤러리 애플리케이션을 추가하는 경우 직면하는 일반적인 문제 및 문제를 해결하기 위해 수행할 수 있는 작업을 이해하는 데 도움이 됩니다.

## <a name="i-clicked-the-add-button-and-my-application-took-a-long-time-to-appear"></a>"추가" 단추를 클릭했고 애플리케이션이 나타나는 데 시간이 오래 걸렸음

경우에 따라 디렉터리에 추가한 후 애플리케이션이 나타나는 데 1-2분 정도(경우에 따라 더 길게) 걸릴 수 있습니다. 이는 예상한 정상 성능이 아니지만, [Azure Portal](https://portal.azure.com/)의 오른쪽 위에 있는 **알림** 아이콘(벨)을 클릭하고 **진행 중** 또는 **완료** 알림 레이블이 지정된 **애플리케이션 추가**를 찾아 애플리케이션 추가가 진행 중임을 볼 수 있습니다.

애플리케이션이 추가되지 않거나 **추가** 단추를 클릭할 때 오류가 발생하는 경우 **오류** 상태에 **알림**이 표시됩니다. 더 자세히 알아보거나 지원 엔지니어와 공유하기 위해 오류에 대한 자세한 정보를 원하는 경우 [포털 알림의 세부 정보를 확인하는 방법](#how-to-see-the-details-of-a-portal-notification) 섹션의 단계를 따라 오류에 대한 자세한 정보를 볼 수 있습니다.

## <a name="i-clicked-the-add-button-and-my-application-didnt-appear"></a>"추가" 단추를 클릭했고 애플리케이션이 나타나지 않았음

경우에 따라 일시적인 문제, 네트워킹 문제 또는 버그로 인해 애플리케이션 추가에 실패합니다. Azure Portal의 오른쪽 위에 있는 **알림** 아이콘(벨)을 클릭하고 **애플리케이션 추가** 알림 옆의 빨간색(!) 아이콘이 표시되는 경우 이러한 상황이 발생한다고 말할 수 있습니다. 이는 애플리케이션을 만들 때 오류가 있었음을 나타냅니다.

**추가** 단추를 클릭할 때 오류가 발생하는 경우 **오류** 상태에 **알림**이 표시됩니다. 더 자세히 알아보거나 지원 엔지니어와 공유하기 위해 오류에 대한 자세한 정보를 원하는 경우 [포털 알림의 세부 정보를 확인하는 방법](#how-to-see-the-details-of-a-portal-notification) 섹션의 단계를 따라 오류에 대한 자세한 정보를 볼 수 있습니다.

## <a name="i-dont-know-how-to-set-up-my-application-once-ive-added-it"></a>추가한 후 내 애플리케이션을 설정하는 방법을 모름

응용 프로그램에 대 한 학습을 사용 하 여 도움이 필요한 경우는 [Azure Active Directory와 SaaS Apps를 통합 하는 방법에 대 한 자습서 목록](https://docs.microsoft.com/azure/active-directory/active-directory-saas-tutorial-list) 문서는 시작 하는 것이 좋습니다.

또한 [Azure AD 애플리케이션 문서 라이브러리](https://docs.microsoft.com/azure/active-directory/active-directory-apps-index)를 통해 Azure AD로 Single Sign-On 및 작동 방법에 대해 자세히 알아볼 수 있습니다.

## <a name="how-to-see-the-details-of-a-portal-notification"></a>포털 알림의 세부 정보를 확인하는 방법

다음 단계를 수행하여 포털 알림의 세부 정보를 확인할 수 있습니다.

1.  선택 된 **알림을** Azure portal의 오른쪽 위에 있는 아이콘 (벨)

2.  **오류** 상태(옆에 빨간색(!)이 있는)에서 알림을 선택합니다.

    >[!NOTE]
    >**성공** 또는 **진행 중** 상태에서 알림을 클릭할 수 없습니다.
    >
    >

4.  **알림 세부 정보**에서 정보를 사용하여 문제에 대한 자세한 내용을 이해합니다.

5.  여전히 도움이 필요한 경우 문제에 대한 도움을 얻도록 이 정보를 지원 엔지니어 또는 제품 그룹과 공유할 수도 있습니다.

6.  **오류 복사** 텍스트 상자 오른쪽의 **복사** **아이콘**을 클릭하여 지원 또는 제품 그룹 엔지니어와 공유하도록 모든 알림 세부 정보를 복사합니다.

## <a name="how-to-get-help-by-sending-notification-details-to-a-support-engineer"></a>지원 엔지니어에게 알림 세부 정보를 전송하여 도움을 얻는 방법

도움이 필요한 경우 지원 엔지니어가 신속하게 도움을 줄 수 있도록 **아래에 나열된 모든 세부 정보**를 공유하는 것은 매우 중요합니다. **스크린샷을 찍거나** **오류 복사** 텍스트 상자 오른쪽에 있는 **오류 복사 아이콘**을 클릭하여 이를 쉽게 수행할 수 있습니다.

## <a name="notification-details-explained"></a>알림 세부 정보 설명

알림에 대한 자세한 내용은 다음 설명을 참조하세요.

### <a name="essential-notification-items"></a>중요 알림 항목

- **제목** - 알림의 설명이 포함된 제목

  * 예제 - **애플리케이션 프록시 설정**

- **설명** – 작업의 결과로 발생한 문제에 대한 설명

  -   예제 - **입력한 내부 url은 이미 다른 애플리케이션에서 사용 중입니다.**

- **알림 ID** - 알림의 고유 ID

  -   예제 – **clientNotification-2adbfc06-2073-4678-a69f-7eb78d96b068**

- **클라이언트 요청 ID** -브라우저에서 만든 특정 요청 ID

  -   예제 – **302fd775-3329-4670-a9f3-bea37004f0bc**

- **타임스탬프 UTC** – 알림이 발생한 동안의 타임스탬프(UTC)

  -   예제 – **2017-03-23T19:50:43.7583681Z**

- **내부 트랜잭션 ID** – 시스템에서 오류를 찾는 데 사용할 수 있습니다 내부 ID

  -   예제 – **71a2f329-ca29-402f-aa72-bc00a7aca603**

- **UPN** – 작업을 수행한 사용자

  -   예 – **tperkins\@f128.info**

- **테넌트 ID** – 작업을 수행한 사용자가 구성원인 테넌트의 고유 ID

  -   예제 – **7918d4b5-0442-4a97-be2d-36f9f9962ece**

- **사용자 개체 ID** – 작업을 수행한 사용자의 고유 ID

  -   예제 – **17f84be4-51f8-483a-b533-383791227a99**

### <a name="detailed-notification-items"></a>자세한 알림 항목

-   **표시 이름** – **(비어 있을 수 있음)** 오류에 대한 보다 자세한 표시 이름

    -   예제 - **애플리케이션 프록시 설정**

-   **상태** - 알림의 특정 상태

    -   예제 – **실패**

-   **개체 ID** – **(비어 있을 수 있음)** 작업이 수행된 개체 ID

    -   예제 – **8e08161d-f2fd-40ad-a34a-a9632d6bb599**

-   **세부 정보** – 작업의 결과로 발생한 문제에 대한 자세한 설명

    -   예 – **내부 url `https://bing.com/` 이미 사용 중 이므로 잘못 되었습니다**

-   **오류 복사** - **오류 복사** 텍스트 상자 오른쪽의 **복사 아이콘**을 클릭하여 지원 또는 제품 그룹과 공유하도록 모든 알림 세부 정보 복사 
-   엔지니어

    -   예제 ```{"errorCode":"InternalUrl\_Duplicate","localizedErrorDetails":{"errorDetail":"Internal url 'https://google.com/' is invalid since it is already in use"},"operationResults":\[{"objectId":null,"displayName":null,"status":0,"details":"Internal url 'https://bing.com/' is invalid since it is already in use"}\],"timeStampUtc":"2017-03-23T19:50:26.465743Z","clientRequestId":"302fd775-3329-4670-a9f3-bea37004f0bb","internalTransactionId":"ea5b5475-03b9-4f08-8e95-bbb11289ab65","upn":"tperkins@f128.info","tenantId":"7918d4b5-0442-4a97-be2d-36f9f9962ece","userObjectId":"17f84be4-51f8-483a-b533-383791227a99"}```

