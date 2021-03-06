---
title: '자습서: LinkedIn Sales Navigator와 Azure Active Directory 통합 | Microsoft Docs'
description: Azure Active Directory와 LinkedInSalesNavigator 간에 Single Sign-On을 구성하는 방법에 대해 알아봅니다.
services: active-directory
documentationCenter: na
author: jeevansd
manager: daveba
ms.assetid: 7a9fa8f3-d611-4ffe-8d50-04e9586b24da
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/12/2018
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 63e3e98c2c3dc8f99e733174c86965304fe483ce
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60257649"
---
# <a name="tutorial-azure-active-directory-integration-with-linkedin-sales-navigator"></a>자습서: LinkedIn Sales Navigator와 Azure Active Directory 통합

이 자습서에서는 LinkedIn Sales Navigator를 Azure AD(Azure Active Directory)와 통합하는 방법에 대해 알아봅니다.

LinkedIn Sales Navigator를 Azure AD와 통합하면 다음과 같은 이점이 제공됩니다.

- LinkedIn Sales Navigator에 대한 액세스 권한이 있는 사용자를 Azure AD에서 제어할 수 있습니다.
- 사용자의 Azure AD 계정으로 LinkedIn Sales Navigator에 자동으로 로그온(Single Sign-On)되도록 설정할 수 있습니다.
- 단일 중앙 위치인 Azure Portal에서 계정을 관리할 수 있습니다.

Azure AD와의 SaaS 앱 통합에 대한 자세한 내용은 [Azure Active Directory를 사용한 애플리케이션 액세스 및 Single Sign-On이란 무엇인가요?](../manage-apps/what-is-single-sign-on.md)를 참조하세요.

## <a name="prerequisites"></a>필수 조건

LinkedIn Sales Navigator와 Azure AD 통합을 구성하려면 다음 항목이 필요합니다.

- Azure AD 구독
- LinkedIn Sales Navigator Single Sign-On이 설정된 구독

> [!NOTE]
> 이 자습서의 단계를 테스트하기 위해 프로덕션 환경을 사용하는 것은 바람직하지 않습니다.

이 자습서의 단계를 테스트하려면 다음 권장 사항을 준수해야 합니다.

- 꼭 필요한 경우가 아니면 프로덕션 환경을 사용하지 마세요.
- Azure AD 평가판 환경이 없으면 [여기](https://azure.microsoft.com/pricing/free-trial/)에서 1개월 평가판을 얻을 수 있습니다.

## <a name="scenario-description"></a>시나리오 설명
이 자습서에서는 테스트 환경에서 Azure AD Single Sign-On을 테스트 합니다. 이 자습서에 설명된 시나리오는 다음 두 가지 주요 구성 요소로 이루어져 있습니다.

1. 갤러리에서 LinkedIn Sales Navigator 추가
1. Azure AD Single Sign-on 구성 및 테스트

## <a name="adding-linkedin-sales-navigator-from-the-gallery"></a>갤러리에서 LinkedIn Sales Navigator 추가
LinkedIn Sales Navigator가 Azure AD에 통합되도록 구성하려면 LinkedIn Sales Navigator를 갤러리에서 관리되는 SaaS 앱 목록에 추가해야 합니다.

**갤러리에서 LinkedIn Sales Navigator를 추가하려면 다음 단계를 수행합니다.**

1. **[Azure Portal](https://portal.azure.com)** 의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다. 

    ![Active Directory][1]

1. **엔터프라이즈 응용 프로그램**으로 이동합니다. 그런 후 **모든 애플리케이션**으로 이동합니다.

    ![애플리케이션][2]
    
1. 대화 상자 맨 위 있는 **새 애플리케이션** 단추를 클릭합니다.

    ![애플리케이션][3]

1. 검색 상자에서 **LinkedIn Sales Navigator**를 입력합니다.

    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_search.png)

1. 결과 패널에서 **LinkedIn Sales Navigator**를 선택하고 **추가** 단추를 클릭하여 애플리케이션을 추가합니다.

    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Azure AD Single Sign-on 구성 및 테스트
이 섹션에서는 "Britta Simon"이라는 테스트 사용자를 기반으로 LinkedIn Sales Navigator에서 Azure AD Single Sign-On을 구성하고 테스트합니다.

Single Sign-On이 작동하려면 Azure AD에서 Azure AD 사용자에 해당하는 LinkedIn Sales Navigator 사용자가 누구인지 알고 있어야 합니다. 즉, Azure AD 사용자와 LinkedIn Sales Navigator의 관련 사용자 간에 연결 관계가 형성되어야 합니다.

이 연결 관계는 Azure AD의 **사용자 이름** 값을 LinkedIn Sales Navigator의 **Username** 값으로 할당하여 설정합니다.

LinkedIn Sales Navigator에서 Azure AD Single Sign-On을 구성하고 테스트하려면 다음 구성 요소를 완료해야 합니다.

1. **[Configuring Azure AD Single Sign-On](#configuring-azure-ad-single-sign-on)** - 사용자가 이 기능을 사용할 수 있도록 합니다.
1. **[Azure AD 테스트 사용자 만들기](#creating-an-azure-ad-test-user)** - Britta Simon으로 Azure AD Single Sign-On 테스트하는 데 사용합니다.
1. **[LinkedIn Sales Navigator 테스트 사용자 만들기](#creating-a-linkedin-sales-navigator-test-user)** - Britta Simon의 Azure AD 표현과 연결된 해당 사용자를 LinkedIn Sales Navigator에 만듭니다.
1. **[Azure AD 테스트 사용자 할당](#assigning-the-azure-ad-test-user)** - Britta Simon이 Azure AD Single Sign-on을 사용할 수 있도록 합니다.
1. **[Single Sign-On 테스트](#testing-single-sign-on)** - 구성이 작동하는지 확인합니다.

### <a name="configuring-azure-ad-single-sign-on"></a>Azure AD Single Sign-On 구성

이 섹션에서는 Azure Portal에서 Azure AD Single Sign-On을 사용하도록 설정하고 LinkedIn Sales Navigator 애플리케이션에서 Single Sign-On을 구성합니다.

**LinkedIn Sales Navigator에서 Azure AD Single Sign-On을 구성하려면 다음 단계를 수행합니다.**

1. Azure Portal의 **LinkedIn Sales Navigator** 애플리케이션 통합 페이지에서 **Single Sign-On**을 클릭합니다.

    ![Configure Single Sign-On][4]

1. **Single Sign-On** 대화 상자의 **모드**에서 **SAML 기반 로그온**을 선택하여 Single Sign-On을 사용하도록 설정합니다.
 
    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_samlbase.png)

1. 다른 웹 브라우저 창에서 **LinkedIn Sales Navigator** 웹 사이트에 관리자로 로그온합니다.

1. **계정 센터**의 **설정** 아래에서 **전역 설정**을 클릭합니다. 또한 드롭다운 목록에서 **Sales Navigator**를 선택합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_01.png)

1. **OR Click Here to load and copy individual fields from the form**(또는 폼에서 개별 필드를 로드하여 복사하려면 여기를 클릭)을 클릭하고 **엔터티 ID** 및 **ACS(Assertion Consumer Access) URL**을 복사합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_031.png)

1. Azure Portal의 **LinkedIn Sales Navigator 도메인 및 URL** 섹션에서 **IDP** 시작 모드로 애플리케이션을 구성하려는 경우 다음 단계를 수행합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url1.png)

    a. **식별자** 텍스트 상자에 LinkedIn 포털에서 복사한 **엔티티 ID**를 입력합니다. 

    b. **회신 URL** 텍스트 상자에 LinkedIn 포털에서 복사한 **ACS(Assertion Consumer Access) URL**을 입력합니다.

1. **SP** 시작 모드에서 애플리케이션을 구성하려면 **고급 URL 설정 표시**를 선택하세요.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_url2.png)

    **로그온 URL** 텍스트 상자에 다음 패턴으로 값을 입력합니다. `https://www.linkedin.com/checkpoint/enterprise/login/<account id>?application=salesNavigator` 

1. **LinkedIn Sales Navigator** 애플리케이션에는 특정 형식의 SAML 어설션이 필요하기 때문에 SAML 토큰 특성 구성에 사용자 지정 특성 매핑을 추가해야 합니다. 다음 스크린샷은 예제를 보여 줍니다. **사용자 ID**의 기본값은 **user.userprincipalname**이지만 LinkedIn Sales Navigator에는 이것이 사용자의 메일 주소와 매핑되어야 합니다. 목록에서 **user.mail** 특성을 사용하거나 조직 구성을 기반으로 적절한 특성 값을 사용할 수 있습니다. 

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/updateusermail.png)
    
1. **사용자 특성** 섹션에서 **기타 모든 사용자 특성 보기 및 편집**을 클릭하고 특성을 설정합니다. 사용자는 **전자 메일**, **부서**, **이름** 및 **성**이라는 이름의 4개 클레임을 추가해야 하며, 값은 **user.mail**, **user.department**, **user.givenname** 및 **user.surname**으로 각각 매핑됩니다.

    | 특성 이름 | 특성 값 |
    | --- | --- |    
    | 이메일| user.mail |
    | department| user.department |
    | firstname| user.givenname |
    | lastname| user.surname |
    
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/userattribute.png)
    
    a. **특성 추가**를 클릭하여 특성 대화 상자를 엽니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_04.png)
    
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/tutorial_attribute_05.png)
   
    b. **이름** 텍스트 상자에서 해당 행에 표시된 특성 이름을 입력합니다.
    
    다. **값** 목록에서 해당 행에 대해 표시된 특성을 입력합니다.
    
    d.  **확인**을 클릭합니다.

1. **이름** 특성에서 다음 단계를 수행합니다.

    a. 특성을 클릭하여 **특성 편집** 창을 엽니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/url_update.png)

    b. **네임스페이스**에서 URL 값을 삭제합니다.
    
    다. **확인**을 클릭하여 설정을 저장합니다.

1. **SAML 서명 인증서** 섹션에서 **메타데이터 XML**을 클릭한 후 컴퓨터에 XML 파일을 저장합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_certificate.png) 

1. **저장** 단추를 클릭합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_general_400.png)

1. **LinkedIn 관리 설정** 섹션으로 이동합니다. **XML 파일 업로드**를 클릭하여 Azure Portal에서 다운로드한 메타데이터 XML을 업로드합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_metadata_03.png)

1. **설정**을 클릭하여 SSO를 사용하도록 설정합니다. SSO 상태가 **연결 안 됨**에서 **연결됨**으로 변경됩니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedin_admin_05.png)


> [!TIP]
> 이제 앱을 설정하는 동안 [Azure Portal ](https://portal.azure.com) 내에서 이러한 지침의 간결한 버전을 읽을 수 있습니다.  **Active Directory &gt; 엔터프라이즈 애플리케이션** 섹션에서 이 앱을 추가한 후에는 **Single Sign-On** 탭을 클릭하고 맨 아래에 있는 **구성** 섹션을 통해 포함된 설명서에 액세스하면 됩니다. 포함된 설명서 기능에 대한 자세한 내용은 [Azure AD 포함된 설명서]( https://go.microsoft.com/fwlink/?linkid=845985)에서 확인할 수 있습니다.
> 

### <a name="creating-an-azure-ad-test-user"></a>Azure AD 테스트 사용자 만들기
이 섹션의 목적은 Azure Portal에서 Britta Simon이라는 테스트 사용자를 만드는 것입니다.

![Azure AD 사용자 만들기][100]

**Azure AD에서 테스트 사용자를 만들려면 다음 단계를 수행하세요.**

1. **Azure Portal**의 왼쪽 탐색 창에서 **Azure Active Directory** 아이콘을 클릭합니다.

    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/create_aaduser_01.png) 

1. **사용자 및 그룹**으로 이동하여 **모든 사용자**를 클릭합니다.
    
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/create_aaduser_02.png) 

1. 대화 상자 위쪽에서 **추가**를 클릭하여 **사용자** 대화 상자를 엽니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/create_aaduser_03.png) 

1. **사용자** 대화 상자 페이지에서 다음 단계를 수행합니다.
 
    ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/create_aaduser_04.png) 

    a. **이름** 텍스트 상자에 **BrittaSimon**을 입력합니다.

    b. **사용자 이름** 텍스트 상자에 BrittaSimon의 **전자 메일 주소**를 입력합니다.

    다. **암호 표시**를 선택하고 **암호** 값을 적어둡니다.

    d. **만들기**를 클릭합니다.
 
### <a name="creating-a-linkedin-sales-navigator-test-user"></a>LinkedIn Sales Navigator 테스트 사용자 만들기

LinkedIn Sales Navigator 애플리케이션이 JIT(Just-in-time) 사용자 프로비전을 지원하며 인증 후에 애플리케이션에서 사용자가 자동으로 만들어집니다. **Automatically assign licenses**(라이선스 자동 할당)를 활성화하여 사용자에게 라이선스를 할당합니다.
   
   ![Azure AD 테스트 사용자 만들기](./media/linkedinsalesnavigator-tutorial/LinkedinUserprovswitch.png)

### <a name="assigning-the-azure-ad-test-user"></a>Azure AD 테스트 사용자 할당

이 섹션에서는 Azure Single Sign-On을 사용할 수 있도록 Britta Simon에게 LinkedIn Sales Navigator에 대한 액세스 권한을 부여합니다.

![사용자 할당][200] 

**Britta Simon을 LinkedIn Sales Navigator에 할당하려면 다음 단계를 수행합니다.**

1. Azure Portal에서 애플리케이션 보기를 연 다음 디렉터리 보기로 이동하고 **엔터프라이즈 애플리케이션**으로 이동한 후 **모든 애플리케이션**을 클릭합니다.

    ![사용자 할당][201] 

1. 애플리케이션 목록에서 **LinkedIn Sales Navigator**를 선택합니다.

    ![Configure Single Sign-On](./media/linkedinsalesnavigator-tutorial/tutorial_linkedinsalesnavigator_app.png) 

1. 왼쪽 메뉴에서 **사용자 및 그룹**을 클릭합니다.

    ![사용자 할당][202] 

1. **추가** 단추를 클릭합니다. 그런 후 **할당 추가** 대화 상자에서 **사용자 및 그룹**을 선택합니다.

    ![사용자 할당][203]

1. **사용자 및 그룹** 대화 상자의 사용자 목록에서 **Britta Simon**을 선택합니다.

1. **사용자 및 그룹** 대화 상자에서 **선택** 단추를 클릭합니다.

1. **할당 추가** 대화 상자에서 **할당** 단추를 클릭합니다.
    
### <a name="testing-single-sign-on"></a>Single Sign-On 테스트

이 섹션에서는 액세스 패널을 사용하여 Azure AD Single Sign-On 구성을 테스트합니다.

액세스 패널에서 LinkedIn Sales Navigator 타일을 클릭하면 개인 LinkedIn 계정 세부 정보를 제공해야 하는 조직 페이지로 리디렉션됩니다. 개인 계정이 LinkedIn 비즈니스 계정과 연결됩니다. 액세스 패널에 대한 자세한 내용은 [액세스 패널 소개](../user-help/active-directory-saas-access-panel-introduction.md)를 참조하세요. 

## <a name="additional-resources"></a>추가 리소스

* [Azure Active Directory와 SaaS Apps를 통합하는 방법에 대한 자습서 목록](tutorial-list.md)
* [Azure Active Directory로 애플리케이션 액세스 및 Single Sign-On을 구현하는 방법](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_01.png
[2]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_02.png
[3]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_03.png
[4]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_04.png

[100]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_100.png

[200]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_200.png
[201]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_201.png
[202]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_202.png
[203]: ./media/linkedinsalesnavigator-tutorial/tutorial_general_203.png

