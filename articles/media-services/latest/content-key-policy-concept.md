---
title: Media Services의 콘텐츠 키 정책 - Azure | Microsoft Docs
description: 이 문서에서는 콘텐츠 키 정책의 개념과 Azure Media Services에서 이러한 정책을 사용하는 방법을 설명합니다.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 05/22/2019
ms.author: juliako
ms.custom: seodec18
ms.openlocfilehash: 68778cea51144ec33efd4d5843a51b489ea17ca4
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155734"
---
# <a name="content-key-policies"></a>콘텐츠 키 정책

Media Services를 사용하면 Advanced Encryption Standard(AES-128) 또는 Microsoft PlayReady, Google Widevine 및 Apple FairPlay 등 세 가지 주요 DRM(디지털 권한 관리) 시스템 중 하나로 동적 암호화된 라이브 콘텐츠 및 주문형 콘텐츠를 제공할 수 있습니다. 또한 Media Services는 인증된 클라이언트에게 AES 키 및DRM(PlayReady, Widevine 및 FairPlay) 라이선스를 배달하는 서비스를 제공합니다.

만들려는 프로그램 스트림에서 암호화 옵션을 지정 하려면를 [스트리밍 정책](streaming-policy-concept.md) 와 연결 프로그램 [스트리밍 로케이터](streaming-locators-concept.md)합니다. 만들어야를 [콘텐츠 키 정책](https://docs.microsoft.com/rest/api/media/contentkeypolicies) 구성 하려면 어떻게 콘텐츠 키 (에 대 한 보안 액세스를 제공 하는 프로그램 [자산](assets-concept.md)) 최종 클라이언트에 배달 됩니다. **콘텐츠 키 정책** 도 연관 하 **스트리밍 로케이터**합니다. 클라이언트에 전달 되도록 지정 된 구성을 사용 하 여 키를 위해 충족 해야 하는 콘텐츠 키 정책에서 요구 사항 (제한)를 설정 하려면 필요 합니다. 

Media Services를 통해 콘텐츠 키 자동 생성 하는 것이 좋습니다. 일반적으로 장기적인된 키를 사용 하는 사용 하 여 정책 있는지 확인할 **가져올**합니다. 키를 가져오려면 별도의 동작 메서드를 호출하여 비밀 또는 자격 증명을 가져와야 합니다. 아래 예제를 참조하세요.

**콘텐츠 키 정책**은 업데이트할 수 있습니다. 예를 들어 키 순환을 수행해야 하는 경우 정책을 업데이트하는 것이 좋습니다. 기본 확인 키 및 기존 정책의 대체 확인 키 목록을 업데이트할 수 있습니다. 키 배달 캐시를 업데이트하고 업데이트된 정책을 선택하는 데 최대 15분까지 걸릴 수 있습니다. 

> [!IMPORTANT]
> * 날짜/시간 형식의 **콘텐츠 키 정책** 속성은 언제나 UTC 형식입니다.
> * Media Services 계정에 대한 제한된 정책 세트를 설계하고 동일한 옵션이 필요할 때마다 스트리밍 로케이터에 해당 세트를 다시 사용하는 것이 좋습니다. 자세한 내용은 [할당량 및 제한 사항](limits-quotas-constraints.md)을 참조하세요.

## <a name="example"></a>예

키로 이동하려면 아래 예제와 같이 **GetPolicyPropertiesWithSecretsAsync**를 사용합니다.

[!code-csharp[Main](../../../media-services-v3-dotnet-tutorials/AMSV3Tutorials/EncryptWithDRM/Program.cs#GetOrCreateContentKeyPolicy)]

## <a name="filtering-ordering-paging"></a>필터링, 정렬, 페이징

[Media Services 엔터티 필터링, 순서 지정, 페이징](entities-overview.md)을 참조하세요.

## <a name="next-steps"></a>다음 단계

* [AES-128 동적 암호화 및 키 전달 서비스 사용](protect-with-aes128.md)
* [DRM 동적 암호화 및 라이선스 배달 서비스 사용](protect-with-drm.md)
* [EncodeHTTPAndPublishAESEncrypted](https://github.com/Azure-Samples/media-services-v3-dotnet-core-tutorials/tree/master/NETCore/EncodeHTTPAndPublishAESEncrypted)
