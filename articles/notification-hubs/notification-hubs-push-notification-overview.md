---
title: Azure Notification Hubs란 무엇인가요?
description: Azure Notification Hubs에 푸시 알림 기능을 추가하는 방법을 알아봅니다.
author: jwargo
manager: patniko
editor: spelluru
services: notification-hubs
documentationcenter: ''
ms.assetid: fcfb0ce8-0e19-4fa8-b777-6b9f9cdda178
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: overview
ms.custom: mvc
ms.date: 04/30/2019
ms.author: jowargo
ms.openlocfilehash: 03d4c269f76a89c43dec253367d07f3bf71a06d8
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65141203"
---
# <a name="what-is-azure-notification-hubs"></a>Azure Notification Hubs란 무엇인가요?

Azure Notification Hubs는 모든 백 엔드(클라우드 또는 온-프레미스)에서 모든 플랫폼(iOS, Android, Windows, Kindle, Baidu 등)에 알림을 보낼 수 있도록 하는 사용하기 쉬운 스케일 아웃 푸시 엔진을 제공합니다. Notification Hubs는 엔터프라이즈 시나리오 및 소비자 시나리오 모두에 적합합니다. 다음은 몇 가지 샘플 시나리오입니다.

- 수백만 명의 사용자에게 대기 시간이 짧은 최신 뉴스 알림을 보냅니다.
- 관심 있는 사용자 세그먼트에 위치 기반 쿠폰을 보냅니다.
- 미디어/스포츠/금융/게임 애플리케이션을 위해 사용자 또는 그룹에 이벤트 관련 알림을 보냅니다.
- 프로모션 콘텐츠를 애플리케이션에 푸시하여 고객을 참여시키고 마케팅 활동을 전개합니다.
- 새 메시지 및 작업 항목과 같은 엔터프라이즈 이벤트를 사용자에게 알립니다.
- 다단계 인증을 위한 코드를 보냅니다.

## <a name="what-are-push-notifications"></a>푸시 알림이란 무엇인가요?

푸시 알림은 모바일 앱 사용자들에게 대개 모바일 디바이스의 팝업 또는 대화 상자로 특정 정보를 알려주는 앱-사용자 통신의 한 형식입니다. 사용자는 일반적으로 메시지를 보거나 해제하도록 선택할 수 있습니다. 전자를 선택하면 알림을 전달한 모바일 애플리케이션이 열립니다. 앱이 백그라운드에서 처리하고 무엇을 수행할지 결정하도록 일부 알림은 무음으로 전달됩니다.

푸시 알림은 앱 참여와 사용량이 증가하는 소비자 앱과 최신 비즈니스 정보를 전달하는 엔터프라이즈 앱에 매우 중요합니다. 모바일 디바이스에는 에너지 효율이 높고, 알림 보낸 사람에게는 유연하며, 해당 애플리케이션이 활성화되어 있지 않을 때 사용할 수 있으므로 최상의 앱-사용자 통신입니다.

인기 있는 몇 가지 플랫폼의 푸시 알림에 대한 자세한 내용은 다음 항목을 참조하세요.

- [Android](https://developer.android.com/guide/topics/ui/notifiers/notifications.html)
- [iOS](https://developer.apple.com/notifications/)
- [Windows](https://msdn.microsoft.com/library/windows/apps/hh779725.aspx)

## <a name="how-push-notifications-work"></a>푸시 알림은 어떻게 작동하나요?

푸시 알림은 *PNS(플랫폼 알림 시스템)* 라는 플랫폼별 인프라를 통해 전달됩니다. 메시지를 배달하는 베어본 푸시 기능을 제공되는 핸들이 있는 디바이스에 제공하며 공통 인터페이스가 없습니다. Android, iOS 및 Windows 버전의 앱에서 모든 고객에게 알림을 보내려면 개발자는 APNS(Apple Push Notification Service), FCM(Firebase Cloud Messaging) 및 WNS(Windows 알림 서비스)를 개별적으로 사용해야 합니다.

높은 수준의 푸시 작동 방식은 다음과 같습니다.

1. 애플리케이션이 알림을 받는다고 결정하면, 앱이 실행 중인 대상 플랫폼에 대해 PNS에 연락하여 고유하고 임시적인 푸시 핸들을 요청합니다. 핸들 형식은 시스템에 따라 다릅니다(예: WNS는 URI를 사용하고, APNS는 토큰을 사용함).
2. 클라이언트 앱은 이 핸들을 앱 백 엔드 또는 공급 기업에 저장합니다.
3. 푸시 알림을 보내기 위해 앱 백 엔드에서 핸들을 사용하여 PNS에 연결하여 특정 클라이언트 앱을 대상으로 합니다.
4. PNS가 핸들에 의해 지정된 디바이스로 알림을 전달합니다.

![푸시 알림 워크플로](./media/notification-hubs-overview/registration-diagram.png)

## <a name="the-challenges-of-push-notifications"></a>푸시 알림의 문제

PNS는 강력합니다. 그러나 분할된 사용자에게 푸시 알림을 브로드캐스트하는 것처럼 일반적인 푸시 알림 시나리오를 구현하기 위해 앱 개발자는 많은 작업을 수행해야 합니다.

알림을 푸시하려면 애플리케이션의 기본 비즈니스 논리와 관련 없는 복잡한 인프라가 필요합니다. 인프라 문제 중 일부는 다음과 같습니다.

- **플랫폼 종속성**
  - PNS가 통합되지 않기 때문에 다양한 플랫폼의 디바이스에 알림을 보내기 위해 백 엔드에 복잡하고 유지하기가 힘든 플랫폼 종속 논리가 필요합니다.
- **규모**
  - PNS 지침에 따라 앱이 시작될 때마다 디바이스 토큰을 새로 고쳐야 합니다. 백 엔드는 토큰을 최신 상태로 유지하는 데에만 대량의 트래픽과 데이터베이스 액세스를 처리하고 있습니다. 디바이스의 수가 수백, 수천 또는 수백만 개에 이르면 이 인프라를 만들고 유지 관리하는 비용이 엄청납니다.
  - 대부분의 PNS는 여러 디바이스로의 브로드캐스트를 지원하지 않습니다. 단순히 백만 개의 디바이스로 브로드캐스트하면 PNS를 백만 번 호출하게 됩니다. 최소 대기 시간으로 트래픽 양을 조정하는 것이 중요합니다.
- **라우팅**
  - PNS에서 메시지를 디바이스로 보내는 방법을 제공하지만 대부분의 앱 알림은 사용자 또는 관심 그룹을 대상으로 합니다. 백 엔드에서 관심 그룹, 사용자, 속성 등과 디바이스를 연결하는 레지스트리를 유지해야 합니다. 이러한 오버헤드로 인해 앱의 출시 기간 및 유지 관리 비용이 늘어납니다.

## <a name="why-use-azure-notification-hubs"></a>Azure Notification Hubs를 사용하는 이유는 무엇인가요?

Notification Hubs는 앱 백 엔드에서 직접 알림을 푸시하는 것과 관련된 모든 복잡성을 제거합니다. 다중 플랫폼의 확장된 푸시 알림 인프라는 푸시 관련 코딩을 줄이고 백 엔드를 간소화합니다. Notification Hubs를 사용하면 다음 그림과 같이 백 엔드에서 사용자 또는 관심 그룹에 메시지를 보내는 동안 디바이스는 PNS 핸들을 허브에 등록하는 역할만 수행합니다.

![알림 허브 다이어그램](./media/notification-hubs-overview/notification-hub-diagram.png)

Notification Hubs는 다음과 같은 장점으로 즉시 사용할 수 있는 푸시 엔진입니다.

- **플랫폼 간**
  - 모든 주요 푸시 플랫폼(iOS, Android, Windows, Kindle 및 Baidu 포함) 지원
  - 플랫폼 특정 작업 없이 플랫폼별 또는 플랫폼 독립적 형식으로 모든 플랫폼에 푸시하는 공통 인터페이스
  - 한 곳에서 디바이스 핸들 관리
- **백 엔드 간**
  - 클라우드 또는 온-프레미스
  - .NET, Node.js, Java 등
- **다양한 배달 패턴**
  - 하나 이상의 플랫폼에 브로드캐스트: 단일 API 호출로 여러 플랫폼에서 수백만 개의 디바이스로 즉시 브로드캐스트할 수 있습니다.
  - 디바이스에 푸시: 개별 디바이스에 알림을 대상으로 지정할 수 있습니다.
  - 사용자에게 푸시: 태그 및 템플릿 기능을 사용하면 사용자의 모든 플랫폼 간 디바이스에 연결할 수 있습니다.
  - 동적 태그를 사용하여 세그먼트로 푸시: 태그 기능을 사용하면 하나의 세그먼트 또는 세그먼트의 식(예: 활성 AND 시애틀 거주 NOT 새 사용자)으로 보낼지 여부에 관계 없이 사용자의 요구에 따라 디바이스를 분류하고 푸시할 수 있습니다. 디바이스 태그를 pub-sub로 제한하는 대신 언제 어디서나 업데이트할 수 있습니다.
  - 지역화된 푸시: 템플릿 기능을 사용하면 백 엔드 코드에 영향을 미치지 않고도 지역화를 달성할 수 있습니다.
  - 자동 푸시: 디바이스에 자동 알림을 보내고 특정 끌어오기 또는 작업을 완료하도록 이 알림을 트리거하여 push-to-pull(밀어넣기-끌어오기) 패턴을 활성화할 수 있습니다.
  - 예약된 푸시: 언제든지 알림을 보내도록 예약할 수 있습니다.
  - 직접 푸시: Notification Hubs 서비스를 통해 디바이스 등록을 건너뛰고 직접 디바이스 핸들 목록에 일괄적으로 푸시할 수 있습니다.
  - 맞춤형 푸시: 디바이스 푸시 변수를 사용하면 사용자 지정 키-값 쌍이 있는 디바이스별 맞춤형 푸시 알림을 보낼 수 있습니다.
- **다양한 원격 분석**
  - 일반적인 푸시, 디바이스, 오류 및 작업 원격 분석은 Azure Portal에서 프로그래밍 방식으로 사용할 수 있습니다.
  - 메시지별 원격 분석은 초기 요청 호출에서 푸시 아웃 일괄 처리를 성공적으로 수행한 Notification Hubs 서비스에 대한 각 푸시를 추적합니다.
  - 플랫폼 알림 시스템 피드백은 디버깅을 지원하기 위해 플랫폼 알림 시스템의 모든 피드백을 통신합니다.
- **확장성**
  - 다시 설계하거나 디바이스를 분할하지 않고도 수백만 개의 디바이스로 빠른 메시지를 보냅니다.
- **보안**
  - SAS(Shared Access Secret) 또는 페더레이션 인증입니다.

## <a name="next-steps"></a>다음 단계

[자습서: 모바일 애플리케이션에 알림 푸시](notification-hubs-android-push-notification-google-fcm-get-started.md)를 따라 알림 허브 만들기 및 사용을 시작합니다.

[0]: ./media/notification-hubs-overview/registration-diagram.png
[1]: ./media/notification-hubs-overview/notification-hub-diagram.png

[How customers are using Notification Hubs]: https://azure.microsoft.com/services/notification-hubs
[Notification Hubs tutorials and guides]: https://azure.microsoft.com/documentation/services/notification-hubs
[iOS]: https://azure.microsoft.com/documentation/articles/notification-hubs-ios-get-started
[Android]: https://azure.microsoft.com/documentation/articles/notification-hubs-android-get-started
[Windows Universal]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-store-dotnet-get-started
[Windows Phone]: https://azure.microsoft.com/documentation/articles/notification-hubs-windows-phone-get-started
[Kindle]: https://azure.microsoft.com/documentation/articles/notification-hubs-kindle-get-started
[Xamarin.iOS]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-ios-get-started
[Xamarin.Android]: https://azure.microsoft.com/documentation/articles/partner-xamarin-notification-hubs-android-get-started
[Microsoft.WindowsAzure.Messaging.NotificationHub]: https://msdn.microsoft.com/library/microsoft.windowsazure.messaging.notificationhub.aspx
[Microsoft.ServiceBus.Notifications]: https://msdn.microsoft.com/library/microsoft.servicebus.notifications.aspx
[App Service Mobile Apps]: https://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop/
[templates]: notification-hubs-templates-cross-platform-push-messages.md
[Azure portal]: https://portal.azure.com
[tags]: (https://msdn.microsoft.com/library/azure/dn530749.aspx)
