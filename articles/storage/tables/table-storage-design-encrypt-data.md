---
title: Azure Storage 테이블 데이터 암호화 | Microsoft Docs
description: Azure Storage에서 테이블 데이터 암호화에 대해 알아봅니다.
services: storage
author: MarkMcGeeAtAquent
ms.service: storage
ms.topic: article
ms.date: 04/11/2018
ms.author: sngun
ms.subservice: tables
ms.openlocfilehash: f56946702011968a0fcb31f6fbecbaacdc89ea42
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60326006"
---
# <a name="encrypt-table-data"></a>테이블 데이터 암호화
.NET Azure Storage 클라이언트 라이브러리는 작업 삽입 및 삭제의 문자열 엔터티 속성 암호화를 지원합니다. 암호화된 문자열은 서비스에 이진 속성으로 저장되고 암호 해독 후에는 다시 문자열로 변환됩니다.    

테이블의 경우, 암호화 정책 외에도 사용자가 암호화할 속성을 지정해야 합니다. 이것은 특성(TableEntity에서 파생 되는 POCO 엔터티)을 지정[EncryptProperty]하거나 암호화 해결 프로그램 요청 옵션에서 수행할 수 있습니다.  암호화 해결 프로그램은 파티션 키, 행 키, 그리고 속성 이름 및 암호화 여부 속성을 나타내는  Bool방식을 반환하는 대표자입니다. 암호화 중에 클라이언트 라이브러리는 이 정보를 사용하여 연결에 쓰는 동안 속성을 암호화할지 여부를 결정합니다. 대리자 속성은 암호화 하는 방법 논리의 가능성도 제공 합니다. (예를 들어 X의 경우, A 속성을 암호화하고 그렇지 않은 경우 A와 B 속성을 암호화) 엔터티를 일거나 쿼리하는 동안 이 정보를 제공할 필요가 없습니다.

## <a name="merge-support"></a>병합 지원

병합은 현재 지원 되지 않습니다. 속성의 하위 집합은 이전에 다른 키를 사용하여 암호화되었을 수 있기 때문에 단순히 새로운 속성을 병합하는 작업과 메타데이터를 업데이트하는 작업은 데이터 손실을 일으킬 수 있습니다. 서비스에서 기존 엔터티를 읽을 수 있는 추가 서비스 호출을 수행 하거나 속성 당 새 키를 사용하는 것 모두에 성능상의 이유로 적합하지 않습니다.     

테이블 데이터를 암호화에 대한 정보는 [클라이언트측 암호화 및 Microsoft Azure Storage에 대한 Azure Key Vault](../common/storage-client-side-encryption.md)를 참조하십시오.  

## <a name="next-steps"></a>다음 단계

- [테이블 디자인 패턴](table-storage-design-patterns.md)
- [관계 모델링](table-storage-design-modeling.md)
- [관계 모델링](table-storage-design-modeling.md)
- [데이터 수정을 위한 디자인](table-storage-design-for-modification.md)
