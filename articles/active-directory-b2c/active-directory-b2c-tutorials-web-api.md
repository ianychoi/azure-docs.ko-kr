---
title: 자습서 - ASP.NET 웹 API로의 액세스 권한 부여 - Azure Active Directory B2C | Microsoft Docs
description: Active Directory B2C를 사용하여 ASP.NET 웹 API를 보호하고 ASP.NET 웹 애플리케이션에서 호출하는 방법에 대한 자습서입니다.
services: active-directory-b2c
author: davidmu1
manager: celestedg
ms.author: davidmu
ms.date: 02/04/2019
ms.custom: mvc
ms.topic: tutorial
ms.service: active-directory
ms.subservice: B2C
ms.openlocfilehash: 77e3eaeffba862c727e021427e5f27967fcf35bd
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687992"
---
# <a name="tutorial-grant-access-to-an-aspnet-web-api-using-azure-active-directory-b2c"></a>자습서: Azure Active Directory B2C를 사용하여 ASP.NET Web API로의 액세스 권한 부여

이 자습서에서는 ASP.NET 웹 애플리케이션에서 Azure AD(Azure Active Directory) B2C로 보호된 웹 API 리소스를 호출하는 방법을 보여 줍니다.

이 자습서에서는 다음 방법에 대해 알아봅니다.

> [!div class="checklist"]
> * 웹 API 애플리케이션 추가
> * 웹 API에 대한 범위 구성
> * 웹 API에 사용 권한 부여
> * 애플리케이션을 사용하도록 샘플 구성

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>필수 조건

[자습서: Azure Active Directory B2C를 사용하여 웹 애플리케이션에서 인증을 사용하도록 설정](active-directory-b2c-tutorials-web-app.md)의 단계 및 필수 조건을 완료합니다.

## <a name="add-a-web-api-application"></a>웹 API 애플리케이션 추가

액세스 토큰을 제공하는 클라이언트 애플리케이션을 통해 보호된 리소스 요청을 수락하고 이에 응답하려면, 먼저 웹 API 리소스를 테넌트에 등록해야 합니다.

1. [Azure Portal](https://portal.azure.com)에 로그인합니다.
2. Azure AD B2C 테넌트를 포함하는 디렉터리를 사용하려면 위쪽 메뉴에서 **디렉터리 및 구독 필터**를 클릭하고 테넌트가 포함된 디렉터리를 선택합니다.
3. Azure Portal의 왼쪽 위에서 **모든 서비스**를 선택하고 **Azure AD B2C**를 검색하여 선택합니다.
4. **응용 프로그램**을 선택하고 **추가**를 선택합니다.
5. 애플리케이션의 이름을 입력합니다. 예를 들어 *webapi1*과 같습니다.
6. **웹앱/웹 API 포함** 및 **암시적 흐름 허용**에 **예**를 선택합니다.
7. **회신 URL**에는 Azure AD B2C에서 애플리케이션이 요청한 토큰을 반환하는 엔드포인트를 입력합니다. 이 자습서의 샘플은 로컬에서 실행되고 `https://localhost:44332`에서 수신 대기합니다.
8. **앱 ID URI**에 웹 API에 사용하는 식별자를 입력합니다. 도메인을 포함하는 전체 식별자 URI가 생성됩니다. 예: `https://contosotenant.onmicrosoft.com/api`
9. **만들기**를 클릭합니다.
10. 속성 페이지에서 웹 애플리케이션을 구성할 때 사용할 애플리케이션 ID를 기록합니다.

## <a name="configure-scopes"></a>범위 구성

범위는 보호된 리소스에 대한 액세스를 제어하는 방법을 제공합니다. 범위는 웹 API에서 범위 기반 액세스 제어를 구현하는 데 사용됩니다. 예를 들어 웹 API 사용자가 읽기 및 쓰기 액세스 권한을 모두 갖고 있을 수도 있고, 읽기 권한만 갖고 있을 수도 있습니다. 이 자습서에서는 범위를 사용하여 웹 API에 대한 읽기 및 쓰기 권한을 정의합니다.

1. **애플리케이션**을 선택한 다음, *webapi1*을 선택합니다.
2. **게시된 범위**를 선택합니다.
3. **범위**에 `Hello.Read`를 입력하고, 설명에 `Read access to hello`를 입력합니다.
4. **범위**에 `Hello.Write`를 입력하고, 설명에 `Write access to hello`를 입력합니다.
5. **저장**을 클릭합니다.

게시된 범위는 클라이언트 애플리케이션 사용 권한을 웹 API에 부여하는 데 사용할 수 있습니다.

## <a name="grant-permissions"></a>권한 부여

애플리케이션에서 보호된 웹 API를 호출하려면 애플리케이션 사용 권한을 API에 부여해야 합니다. 필수 조건 자습서에서 *webapp1*이라는 웹 애플리케이션을 Azure AD B2C에 만들었습니다. 이 애플리케이션은 웹 API를 호출하는 데 사용합니다.

1. **애플리케이션**을 선택한 후 *webapp1*을 선택합니다.
2. **API 액세스**를 선택한 후 **추가**를 선택합니다.
3. **API 선택** 드롭다운에서 *webapi1*을 선택합니다.
4. **범위 선택** 드롭다운에서 이전에 정의한 **Hello.Read** 및 **Hello.Write**를 선택합니다.
5. **확인**을 클릭합니다.

애플리케이션이 보호된 웹 API를 호출하도록 등록됩니다. 사용자가 Azure AD B2C로 인증하여 애플리케이션을 사용합니다. 애플리케이션은 Azure AD B2C에서 권한 부여를 받아 보호되는 웹 API에 액세스합니다.

## <a name="configure-the-sample"></a>샘플 구성

이제 웹 API가 등록되었고 범위가 정의되었으므로 웹 API에서 Azure AD B2C 테넌트를 사용하도록 구성합니다. 이 자습서에서는 샘플 웹 API를 구성합니다. 샘플 웹 API는 필수 구성 요소 자습서에서 다운로드한 프로젝트에 포함됩니다.

샘플 솔루션에는 두 개의 프로젝트가 있습니다.

다음의 두 프로젝트는 샘플 솔루션에 있습니다.

- **TaskWebApp** - 작업 목록을 만들고 편집합니다. 이 샘플은 **등록 또는 로그인** 사용자 흐름을 사용하여 사용자를 등록하거나 로그인시킵니다.
- **TaskService** - 작업 목록 만들기, 읽기, 업데이트 및 삭제 기능을 지원합니다. API는 Azure AD B2C를 통해 보호되고 TaskWebApp에서 호출됩니다.

### <a name="configure-the-web-application"></a>웹 애플리케이션 구성

1. Visual Studio에서 **B2C-WebAPI-DotNet** 솔루션을 엽니다.
2. **TaskWebApp** 프로젝트에서 **Web.config**를 엽니다.
3. API를 로컬로 실행하려면 **api:TaskServiceUrl**에 대한 localhost 설정을 사용합니다. Web.config를 다음과 같이 변경합니다. 

    ```C#
    <add key="api:TaskServiceUrl" value="https://localhost:44332/"/>
    ```

3. API의 URI를 구성합니다. 이는 웹 애플리케이션에서 API를 요청하는 데 사용하는 URI입니다. 또한 요청된 권한도 구성합니다.

    ```C#
    <add key="api:ApiIdentifier" value="https://<Your tenant name>.onmicrosoft.com/api/" />
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

### <a name="configure-the-web-api"></a>Web API 구성

1. **TaskService** 프로젝트에서 **Web.config**를 엽니다.
2. 테넌트를 사용하도록 API를 구성합니다.

    ```C#
    <add key="ida:Tenant" value="<Your tenant name>.onmicrosoft.com" />
    ```

3. 클라이언트 ID를 API에 대해 등록된 애플리케이션 ID로 설정합니다.

    ```C#
    <add key="ida:ClientId" value="<application-ID>"/>
    ```

4. 등록 및 로그인 사용자 흐름의 이름으로 사용자 흐름 설정을 업데이트합니다.

    ```C#
    <add key="ida:SignUpSignInUserFlowId" value="B2C_1_signupsignin1" />
    ```

5. 포털에서 만든 범위와 일치하도록 범위 설정을 구성합니다.

    ```C#
    <add key="api:ReadScope" value="Hello.Read" />
    <add key="api:WriteScope" value="Hello.Write" />
    ```

## <a name="run-the-sample"></a>샘플 실행

**TaskWebApp** 및 **TaskService** 프로젝트를 모두 실행해야 합니다. 

1. 솔루션 탐색기에서 솔루션을 마우스 오른쪽 단추로 클릭하고 **시작 프로젝트 설정**을 선택합니다. 
2. **여러 시작 프로젝트**를 선택합니다.
3. **시작**할 두 프로젝트에 대한 **작업**을 변경합니다.
4. **확인**을 클릭하여 구성을 저장합니다.
5. **F5** 키를 눌러 두 응용 프로그램을 모두 실행합니다. 각 애플리케이션은 자체 브라우저 탭에서 열립니다. `https://localhost:44316/`은 웹 애플리케이션이고,
    `https://localhost:44332/`는 웹 API입니다.

6. 웹 애플리케이션에서 **등록/로그인**을 클릭하여 웹 애플리케이션에 로그인합니다. 이전에 만든 계정을 사용합니다. 
7. 로그인한 후 **할 일 목록**을 클릭하고 할 일 목록 항목을 만듭니다.

할 일 목록 항목을 만들면 웹 애플리케이션에서 할 일 목록 항목을 생성하는 웹 API를 요청합니다. 보호되는 웹 애플리케이션이 Azure AD B2C 테넌트에서 보호되는 웹 API를 호출합니다.

## <a name="next-steps"></a>다음 단계

이 자습서에서는 다음 방법에 대해 알아보았습니다.

> [!div class="checklist"]
> * 웹 API 애플리케이션 추가
> * 웹 API에 대한 범위 구성
> * 웹 API에 사용 권한 부여
> * 애플리케이션을 사용하도록 샘플 구성

> [!div class="nextstepaction"]
> [자습서: Azure Active Directory B2C의 애플리케이션에 ID 공급자 추가](tutorial-add-identity-providers.md)
