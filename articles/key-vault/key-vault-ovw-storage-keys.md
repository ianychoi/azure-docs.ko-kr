---
title: Azure Key Vault 관리 스토리지 계정 - CLI
description: 스토리지 계정 키는 Azure Key Vault 간의 원활한 통합과 Azure Storage 계정에 대한 키 기반 액세스를 제공합니다.
ms.topic: conceptual
services: key-vault
ms.service: key-vault
author: msmbaldwin
ms.author: mbaldwin
manager: barbkess
ms.date: 03/01/2019
ms.openlocfilehash: 190375700f65cf2d3ea47335a646562eb46b2d49
ms.sourcegitcommit: 2ce4f275bc45ef1fb061932634ac0cf04183f181
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/07/2019
ms.locfileid: "65232569"
---
# <a name="azure-key-vault-managed-storage-account---cli"></a>Azure Key Vault 관리 스토리지 계정 - CLI

> [!NOTE]
> [Azure Active Directory (Azure AD)를 사용 하 여 azure storage 통합]은 Microsoft의 클라우드 기반 id 및 액세스 관리 서비스입니다. Azure AD 통합은 Blob 및 큐 서비스에 대해 사용할 수 있습니다. (https://docs.microsoft.com/azure/storage/common/storage-auth-aad). 인증 및 권한 부여에 Azure Key Vault뿐만 아니라, Azure Storage에 대한 OAuth2 토큰 기반 액세스를 제공하는 Azure AD를 사용하는 것이 좋습니다. 이를 통해 다음을 수행할 수 있습니다.
> - 스토리지 계정 자격 증명 대신, 애플리케이션 또는 사용자 ID를 사용하여 클라이언트 애플리케이션을 인증합니다. 
> - Azure 기반 실행 시 [Azure AD 관리 ID](/azure/active-directory/managed-identities-azure-resources/)를 사용합니다. 관리 ID를 통해 클라이언트 인증은 물론, 애플리케이션에 또는 애플리케이션을 통해 자격 증명을 저장할 필요성이 없어집니다.
> - 권한 부여 관리에 Key Vault에서도 지원되는 역할 기반 제어(RBAC)를 사용합니다.

[Azure Storage 계정](/azure/storage/storage-create-storage-account)은 계정 이름과 키로 구성된 자격 증명을 사용합니다. 키는 자동 생성되며 암호화 키와는 더 반대되는 "암호" 역할을 합니다. Key Vault는 이러한 키를 [Key Vault 비밀](/azure/key-vault/about-keys-secrets-and-certificates#key-vault-secrets)로 저장하여 해당 스토리지 계정 키를 관리할 수 있습니다. 

## <a name="overview"></a>개요

Key Vault 관리 스토리지 계정 기능은 사용자를 대신하여 다양한 관리 기능을 수행합니다.

- Azure Storage 계정과 함께 키를 나열(동기화)합니다.
- 키를 정기적으로 다시 생성(회전)합니다.
- 스토리지 계정과 클래식 스토리지 계정 모두의 키를 관리합니다.
- 키 값은 호출자에게 응답으로 반환되지 않습니다.

관리 스토리지 계정 키 기능을 사용할 경우:

- **Key Vault에서만 스토리지 계정 키를 관리하도록 허용합니다.** 직접 관리할 경우 Key Vault 프로세스를 방해할 수 있으므로 직접 관리하려고 하지 마세요.
- **둘 이상의 Key Vault 개체가 스토리지 계정 키를 관리하도록 허용하지 않습니다**.
- **스토리지 계정 키를 수동으로 다시 생성하지 않습니다**. Key Vault를 통해 다시 생성하는 것이 좋습니다.
- 이제 사용자 보안 주체 및 서비스 주체 하지 여 라는 Key Vault 저장소 계정 관리를 수행할 수 있습니다.

다음 예에서는 Key Vault에서 스토리지 계정 키를 관리하도록 허용하는 방법을 보여줍니다.

> [!IMPORTANT]
> Azure AD 테넌트는 등록된 각 애플리케이션에 애플리케이션의 ID 역할을 하는 **[서비스 주체](/azure/active-directory/develop/developer-glossary#service-principal-object)** 를 제공합니다. 서비스 주체의 애플리케이션 ID는 RBAC(역할 기반 액세스 제어)를 통해 다른 Azure 리소스에 액세스할 수 있는 권한을 부여할 때 사용됩니다. Key Vault는 Microsoft 애플리케이션이므로 각 Azure 클라우드 내에서 동일한 애플리케이션 ID에 속한 모든 Azure AD 테넌트에 사전 등록되어 있습니다.
> - Azure Government 클라우드의 Azure AD 테넌트는 애플리케이션 ID `7e7c393b-45d0-48b1-a35e-2905ddf8183c`를 사용합니다.
> - Azure 공용 클라우드 및 그 외 모든 클라우드의 Azure AD 테넌트는 애플리케이션 ID `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`을 사용합니다.

<a name="prerequisites"></a>필수 조건
--------------
1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) Azure CLI 설치   
2. [저장소 계정 만들기](https://azure.microsoft.com/services/storage/)
    - 이 [문서](https://docs.microsoft.com/azure/storage/)의 단계에 따라 저장소 계정을 만듭니다.  
    - **명명 지침:** Storage 계정 이름은 3자에서 24자 사이여야 하고 숫자 및 소문자만 포함할 수 있습니다.        
      
<a name="step-by-step-instructions-on-how-to-use-key-vault-to-manage-storage-account-keys"></a>Key Vault를 사용하여 스토리지 계정 키를 관리하는 방법에 대한 단계별 지침
--------------------------------------------------------------------------------
개념상, 수행되는 단계 목록은 다음과 같습니다.
- 먼저 (기존) 스토리지 계정을 가져옵니다.
- 그런 다음, (기존) 키 자격 증명 모음을 페치합니다.
- 자격 증명 모음에 KeyVault 관리형 스토리지 계정을 추가하고 Key1을 활성 키로, 다시 생성 기간을 180일로 설정합니다.
- 마지막으로, Key1을 사용하여 지정된 스토리지 계정에 대한 스토리지 컨텍스트를 설정합니다.

아래 지침에서는 저장소 계정에 대한 운영자 권한이 있는 서비스로 Key Vault를 할당합니다.

> [!NOTE]
> Azure Key Vault 관리 스토리지 계정 키를 설정했으면 Key Vault를 통하는 경우를 제외하고 더 이상 변경하지 **않아야 합니다**. 관리 스토리지 계정 키는 Key Vault가 스토리지 계정 키 회전을 관리한다는 것을 의미합니다.

> [!IMPORTANT]
> Azure AD 테넌트는 등록된 각 애플리케이션에 애플리케이션의 ID 역할을 하는 **[서비스 주체](/azure/active-directory/develop/developer-glossary#service-principal-object)** 를 제공합니다. 서비스 주체의 애플리케이션 ID는 RBAC(역할 기반 액세스 제어)를 통해 다른 Azure 리소스에 액세스할 수 있는 권한을 부여할 때 사용됩니다. Key Vault는 Microsoft 애플리케이션이므로 각 Azure 클라우드 내에서 동일한 애플리케이션 ID에 속한 모든 Azure AD 테넌트에 사전 등록되어 있습니다.
> - Azure Government 클라우드의 Azure AD 테넌트는 애플리케이션 ID `7e7c393b-45d0-48b1-a35e-2905ddf8183c`를 사용합니다.
> - Azure 공용 클라우드 및 그 외 모든 클라우드의 Azure AD 테넌트는 애플리케이션 ID `cfa8b339-82a2-471a-a3c9-0fc0be7a4093`을 사용합니다.

> - 현재 저장소 계정을 관리 하는 Key Vault에 게 사용자 계정 및 서비스 주체 하지을 사용할 수 있습니다.


1. 저장소 계정을 만든 후 다음 명령을 실행하여 관리하려는 저장소 계정의 리소스 ID를 가져옵니다.

    ```
    az storage account show -n storageaccountname 
    ```
    결과 아래와 같이 위의 명령에서 ID 필드 복사
    ```
    /subscriptions/0xxxxxx-4310-48d9-b5ca-0xxxxxxxxxx/resourceGroups/ResourceGroup/providers/Microsoft.Storage/storageAccounts/StorageAccountName
    ```
            "objectId": "93c27d83-f79b-4cb2-8dd4-4aa716542e74"
    
2. 저장소 계정에 대 한 액세스 범위를 제한, Key Vault에 "저장소 계정 키 운영자 서비스 역할" RBAC 역할을 할당 합니다. 클래식 저장소 계정에 대 한 "클래식 Storage 계정 키 운영자 서비스 역할입니다." 사용
    ```
    az role assignment create --role "Storage Account Key Operator Service Role"  --assignee-object-id <ObjectIdOfKeyVault> --scope 93c27d83-f79b-4cb2-8dd4-4aa716542e74
    ```
    
    ' 93c27d83-f79b-4cb2-8dd4-4aa716542e74'에 공용 클라우드에서 Key Vault에 대 한 개체 ID입니다. National clouds에서 Key Vault에 대 한 개체 ID를 가져오려면 위의 중요 섹션을 참조
    
3. Key Vault 관리 저장소 계정을 만듭니다.     <br /><br />
   아래에서 다시 생성 기간을 90일로 설정합니다. 90일 후에 Key Vault는 ‘key1’을 다시 생성하고 활성 키를 ‘key1’에서 ‘key2’로 교체합니다. Key Vault는 이제 Key1을 활성 키로 표시합니다. 
   
    ```
    az keyvault storage add --vault-name <YourVaultName> -n <StorageAccountName> --active-key-name key1 --auto-regenerate-key --regeneration-period P90D --resource-id <Id-of-storage-account>
    ```

<a name="step-by-step-instructions-on-how-to-use-key-vault-to-create-and-generate-sas-tokens"></a>Key Vault를 사용하여 SAS 토큰을 만들고 생성하는 방법에 대한 단계별 지침
--------------------------------------------------------------------------------
Key Vault에 SAS(공유 액세스 서명) 토큰을 생성하도록 요청할 수도 있습니다. 공유 액세스 서명은 저장소 계정의 리소스에 대한 위임된 권한을 제공합니다. SAS로 계정 키를 공유하지 않고 저장 계정의 리소스에 대한 클라이언트의 액세스를 승인할 수 있습니다. 이는 애플리케이션에서 공유 액세스 서명을 사용하는 중요한 점입니다. SAS는 계정 키를 손상시키지 않고 스토리지 리소스를 공유할 수 있는 보안 방법입니다.

위에 나열된 단계를 완료하면 다음 명령을 실행하여 Key Vault에 사용자의 SAS 토큰을 생성하도록 요청할 수 있습니다. 

아래 단계에서 수행되는 작업 목록은 다음과 같습니다.
- SAS 정의 라는 계정을 설정 합니다 `<YourSASDefinitionName>` KeyVault 관리 되는 저장소 계정에 `<YourStorageAccountName>` 자격 증명 모음에 `<VaultName>`입니다. 
- 서비스, 컨테이너 및 개체 리소스 종류에 대해 Blob, 파일, 테이블 및 큐 서비스의 계정 SAS 토큰을 만듭니다. 이 토큰은 모든 권한이 있으며 https를 사용하고 시작 및 종료 날짜가 지정됩니다.
- 자격 증명 모음에 KeyVault 관리형 스토리지 SAS 정의를 설정합니다. 템플릿 URI는 위에서 만든 SAS 토큰으로, SAS 유형은 ‘계정’이고 N일간 유효합니다.
- SAS 정의에 해당하는 KeyVault 비밀에서 실제 액세스 토큰을 검색합니다.

1. 이 단계에서는 SAS 정의를 만듭니다. 이 SAS 정의가 생성되고 나면 Key Vault에 추가 SAS 토큰을 생성하도록 요청할 수 있습니다. 이 작업에는 스토리지/setsas 권한이 필요합니다.

```
$sastoken = az storage account generate-sas --expiry 2020-01-01 --permissions rw --resource-types sco --services bfqt --https-only --account-name storageacct --account-key 00000000
```
위의 작업에 대한 자세한 도움말은 [여기](https://docs.microsoft.com/cli/azure/storage/account?view=azure-cli-latest#az-storage-account-generate-sas)에서 확인할 수 있습니다.

이 작업이 성공적으로 실행되면 아래와 비슷한 출력이 표시됩니다. 확인했습니다.

```console
   "se=2020-01-01&sp=***"
```

1. 이 단계에서는 위에서 생성된 출력($sasToken)을 사용하여 SAS 정의를 만듭니다. 자세한 문서를 보려면 [여기](https://docs.microsoft.com/cli/azure/keyvault/storage/sas-definition?view=azure-cli-latest#required-parameters)를 참조하세요.   

```
az keyvault storage sas-definition create --vault-name <YourVaultName> --account-name <YourStorageAccountName> -n <NameOfSasDefinitionYouWantToGive> --validity-period P2D --sas-type account --template-uri $sastoken
```
                        

 > [!NOTE] 
 > 사용자에게 저장소 계정에 대한 권한이 없는 경우 먼저 사용자의 개체 ID를 가져옵니다.

 ```
 az ad user show --upn-or-object-id "developer@contoso.com"

 az keyvault set-policy --name <YourVaultName> --object-id <ObjectId> --storage-permissions backup delete list regeneratekey recover     purge restore set setsas update
 ```
    
## <a name="fetch-sas-tokens-in-code"></a>코드에서 SAS 토큰 페치

이 섹션에서는 Key Vault에서 [SAS 토큰](https://docs.microsoft.com/azure/storage/common/storage-dotnet-shared-access-signature-part-1)을 가져와서 스토리지 계정에 대한 작업을 수행하는 방법을 설명합니다.

아래 섹션에서는 위에 표시된 대로 SAS 정의가 생성된 후 SAS 토큰을 페치하는 방법을 보여 줍니다.

> [!NOTE]
>   [기본 개념](key-vault-whatis.md#basic-concepts)에 설명된 것처럼 3가지 방법으로 Key Vault에 인증할 수 있습니다.
> - 관리 서비스 ID 사용(강력 권장)
> - 서비스 주체 및 인증서 사용 
> - 서비스 주체 및 암호 사용(권장하지 않음)

```cs
// Once you have a security token from one of the above methods, then create KeyVaultClient with vault credentials
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(securityToken));

// Get a SAS token for our storage from Key Vault. SecretUri is of the format https://<VaultName>.vault.azure.net/secrets/<ExamplePassword>
var sasToken = await kv.GetSecretAsync("SecretUri");

// Create new storage credentials using the SAS token.
var accountSasCredential = new StorageCredentials(sasToken.Value);

// Use the storage credentials and the Blob storage endpoint to create a new Blob service client.
var accountWithSas = new CloudStorageAccount(accountSasCredential, new Uri ("https://myaccount.blob.core.windows.net/"), null, null, null);

var blobClientWithSas = accountWithSas.CreateCloudBlobClient();
```

SAS 토큰이 곧 만료될 예정이면 Key Vault에서 SAS 토큰을 다시 가져와서 코드를 업데이트합니다.

```cs
// If your SAS token is about to expire, get the SAS Token again from Key Vault and update it.
sasToken = await kv.GetSecretAsync("SecretUri");
accountSasCredential.UpdateSASToken(sasToken);
```

### <a name="relevant-azure-cli-commands"></a>관련 Azure CLI 명령

[Azure CLI Storage 명령](https://docs.microsoft.com/cli/azure/keyvault/storage?view=azure-cli-latest)

## <a name="see-also"></a>참고 항목

- [키, 비밀 및 인증서에 대한 정보](https://docs.microsoft.com/rest/api/keyvault/)
- [Key Vault 팀 블로그](https://blogs.technet.microsoft.com/kv/)
