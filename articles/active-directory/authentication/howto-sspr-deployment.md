---
title: 셀프 서비스 암호 재설정 배포 가이드 - Azure Active Directory
description: Azure AD 셀프 서비스 암호 재설정을 성공적으로 롤아웃하기 위한 팁
services: active-directory
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sahenry
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9c254ef3a71e95b33df2a779c728d47fff3c3759
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190359"
---
# <a name="how-to-successfully-roll-out-self-service-password-reset"></a>셀프 서비스 암호 재설정을 성공적으로 롤아웃하는 방법

Azure AD(Azure Active Directory) SSPR(셀프 서비스 암호 재설정) 기능을 자연스럽게 롤아웃하기 위해 대부분의 고객이 다음 단계를 완료해야 합니다.

> [!VIDEO https://www.youtube.com/embed/OZn5btP6ZXw]

1. 조직의 일부에 대해 파일럿 롤아웃을 완료합니다.
   * 파일럿 실행 방법에 대한 내용은 [자습서: Azure AD 셀프 서비스 암호 재설정 파일럿 배포 완료](tutorial-sspr-pilot.md)를 참조하세요.
1. 기술 지원팀을 교육합니다.
   * 이 지원팀이 사용자에게 어떻게 도움을 주게 되나요?
   * 사용자가 강제로 SSPR을 사용하도록 하고 기술 지원팀에서 지원하지 못하게 할 예정인가요?
   * 등록 및 재설정을 위한 URL을 제공했나요?
      * 등록: https://aka.ms/ssprsetup
      * 재설정: https://aka.ms/sspr

   > [!WARNING]
   > 확인란의 "반드시 암호 변경 다음 로그온 할 때" Active Directory 사용자 및 컴퓨터 또는 Active Directory 관리 센터와 같은 온-프레미스 Active Directory 관리 도구에서 사용 하 여 지원 되지 않습니다. 암호를 변경 하는 경우 온-프레미스가이 옵션을 선택 하지 마십시오. 

1. 사용자를 교육합니다.
   * 이 문서의 다음 섹션에서는 암호 포털로 가서 등록을 적용하고 인증 데이터를 채우는 샘플 커뮤니케이션을 진행합니다.
   * Azure Active Directory 제품 그룹에서는 조직에서 이 사이트의 설명서와 함께 비즈니스 사례를 만들고 셀프 서비스 암호 재설정 배포를 계획하는 데 사용할 수 있는 [단계별 배포 계획](https://aka.ms/SSPRDeploymentPlan)을 만들었습니다.
1. 전체 조직에 대해 셀프 서비스 암호 재설정을 사용하도록 설정합니다.
   * 준비가 되면 **셀프 서비스 암호 재설정 사용**을 **전체**로 설정하여 모든 사용자가 암호 재설정을 사용하도록 설정합니다.

## <a name="sample-communication"></a>샘플 커뮤니케이션

많은 고객은 사용자에게 SSPR을 사용하도록 하는 가장 쉬운 방법으로 사용하기 간단한 지침이 포함된 전자 메일 캠페인을 사용하는 것이라고 생각합니다. [Microsoft는 롤아웃에 도움이 되도록 템플릿으로 사용할 수 있는 간단한 전자 메일 및 기타 참고 자료를 만들었습니다.](https://www.microsoft.com/download/details.aspx?id=56768)

* **출시 예정**: 롤아웃 몇 주 또는 며칠 전에 사용자에게 어떤 작업을 수행해야 한다는 사실을 알리기 위해 사용하는 전자 메일 템플릿입니다.
* **현재 사용 가능**: 프로그램 출시 당일에 사용자가 인증 데이터를 등록하고 확인하도록 하기 위해 사용하는 전자 메일 템플릿입니다. 사용자가 지금 등록하는 경우 필요할 때 SSPR을 사용할 수 있습니다.
* **등록 미리 알림**: 배포 며칠 후 또는 몇 주 후에 사용자에게 인증 데이터를 등록하고 확인하라고 알려주는 전자 메일 템플릿입니다.
* **SSPR 포스터**: 롤아웃하기 며칠 및 몇 주 전과 롤아웃한 후에 조직에서 사용자 지정하고 표시할 수 있는 포스터입니다.
* **SSPR 테이블 텐트**: 사용자가 등록을 완료하도록 하기 위해 간이 식당, 회의실 또는 책상에 놓을 수 있는 테이블 카드입니다.
* **SSPR 스티커**: 사용자 지정하고 인쇄할 수 있는 스티커 템플릿으로, SSPR에 액세스하는 방법을 상기시키기 위해 랩톱, 모니터, 키보드 또는 휴대폰에 배치할 수 있습니다.

![사용자에 게 출시에 대 한 SSPR 전자 메일 샘플][Email]

## <a name="create-your-own-password-portal"></a>고유 암호 포털 만들기

대부분의 고객은 웹 페이지를 호스팅하고 https://passwords.contoso.com과 같은 루트 DNS 항목을 만드는 방법을 선택합니다. 이 페이지를 다음 정보에 대한 링크로 채웁니다.

* [Azure AD 암호 재설정 포털 - https://aka.ms/sspr](https://aka.ms/sspr)
* [Azure AD 암호 재설정 등록 포털 - https://aka.ms/ssprsetup](https://aka.ms/ssprsetup)
* [Azure AD 암호 변경 포털 - https://account.activedirectory.windowsazure.com/ChangePassword.aspx](https://account.activedirectory.windowsazure.com/ChangePassword.aspx)
* 다른 조직 관련 정보

보낸 전자 메일 통신 또는 전단지에서 사용자가 서비스를 사용해야 할 때 이동할 수 있는 유명하고 기억하기 쉬운 URL을 포함할 수 있습니다. 사용자에게 도움이 되도록 조직의 필요에 맞게 사용하고 사용자 지정할 수 있는 [샘플 암호 재설정 페이지](https://github.com/ajamess/password-reset-page)를 만들었습니다.

## <a name="use-enforced-registration"></a>강제 등록 사용

사용자를 암호 재설정에 등록하려면 사용자가 Azure AD를 통해 로그인할 때 등록하도록 할 수 있습니다. **등록** 탭의 **로그인할 때 사용자가 등록해야 함** 옵션을 사용하여 디렉터리의 **암호 재설정** 창에서 이 옵션을 사용할 수 있습니다.

관리자는 특정 기간 이후에 사용자가 다시 등록하도록 요구할 수 있습니다. **사용자가 인증 정보를 다시 확인하도록 요청받을 때까지의 기간(일) 설정** 옵션을 0에서 730 사이로 설정할 수 있습니다.

이 옵션을 설정한 후에 사용자가 로그인할 경우 해당 관리자의 요청에 따라 인증 정보를 확인해야 한다는 내용의 메시지가 표시됩니다.

## <a name="populate-authentication-data"></a>인증 데이터 채우기

[사용자에 대한 몇 가지 인증 데이터를 미리 채우는](howto-sspr-authenticationdata.md) 것이 좋습니다. 이러한 방식으로 사용자는 SSPR을 사용하기 전에 암호 재설정에 등록할 필요가 없습니다. 사용자가 정의된 암호 재설정 정책을 충족하는 인증 데이터를 제공하면 해당 암호를 재설정할 수 있습니다.

## <a name="disable-self-service-password-reset"></a>셀프 서비스 암호 재설정 사용 안 함

조직이 셀프 서비스 암호 재설정을 비활성화하기로 결정한 경우 프로세스는 간단합니다. Azure AD 테넌트를 열고 **암호 재설정** > **속성**으로 이동한 다음 **셀프 서비스 암호 재설정이 사용하도록 설정됨** 아래에서 **없음**을 선택합니다. 사용자는 등록된 인증 방법을 나중에 사용할 수 있도록 계속 유지합니다.

## <a name="next-steps"></a>다음 단계

* [암호 재설정 또는 변경](../user-help/active-directory-passwords-update-your-own-password.md)
* [셀프 서비스 암호 재설정 등록](../user-help/active-directory-passwords-reset-register.md)
* [Azure Multi-Factor Authentication 및 Azure AD 셀프 서비스 암호 재설정에 융합 등록 사용](concept-registration-mfa-sspr-converged.md)
* [라이선스 관련 질문이 있습니까?](concept-sspr-licensing.md)
* [SSPR에서 사용하는 데이터는 무엇이며, 사용자에 대해 어떤 데이터를 채워야 합니까?](howto-sspr-authenticationdata.md)
* [SSPR에서 사용하는 정책 옵션은 무엇입니까?](concept-sspr-policy.md)
* [비밀번호 쓰기 저장은 무엇이며, 왜 관심을 가져야 합니까?](howto-sspr-writeback.md)
* [SSPR 작업은 어떻게 보고 합니까?](howto-sspr-reporting.md)
* [모든 SSPR 옵션과 그 의미는 무엇입니까?](concept-sspr-howitworks.md)
* [무엇인가 손상된 문제가 있습니다. SSPR 문제는 어떻게 해결합니까?](active-directory-passwords-troubleshoot.md)
* [다른 곳에서 다루지 않았던 질문이 있습니다.](active-directory-passwords-faq.md)

[Email]: ./media/howto-sspr-deployment/sspr-emailtemplates.png "조직의 요구 사항에 맞게 이러한 전자 메일 템플릿을 사용자 지정"
