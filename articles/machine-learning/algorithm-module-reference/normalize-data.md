---
title: 데이터를 정규화 합니다. 모듈 참조
titleSuffix: Azure Machine Learning service
description: Azure Machine Learning 서비스에서를 통해 데이터 집합을 변환할 데이터 정규화 모듈을 사용 하는 방법을 알아봅니다 *정규화*...
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 95069bafa94770511c7ee40e82068960298fd6c5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029447"
---
# <a name="normalize-data-module"></a>정규화 데이터 모듈

이 문서에서는 Azure Machine Learning 서비스에 대 한 시각적 인터페이스 (미리 보기)의 모듈을 설명 합니다.

이 모듈을 사용 하 여을 통해 데이터 집합을 변환할 *정규화*합니다.

정규화는 machine learning 위한 데이터 준비의 일환으로 자주 적용 되는 기술입니다. 손실 정보나 값의 범위에서 차이 왜곡 하지 않고 공용 확장을 사용 하 여 데이터 집합의 숫자 열의 값을 변경 하려면 정규화의 목표가입니다. 정규화는 데이터를 올바르게 모델링 하는 일부 알고리즘에도 필요 합니다.

예를 들어 입력된 데이터 집합에 0에서 1 사이의 값을 사용 하 여 하나의 열과 100,000에서 10000 까지의 값을 사용 하 여 다른 열을 가정 합니다. 유용한의 차이점은 *확장* 숫자의 문제를 일으킬 수 값을 모델링 하는 동안 기능으로 결합 하려는 경우.

*정규화* 모델에 사용 되는 모든 숫자 열에 걸쳐 적용 확장 내에서 값을 유지 하면서 일반 분포와 원본 데이터의 비율을 유지 관리 하는 새 값을 만들어 이러한 문제를 방지 합니다.

이 모듈에는 숫자 데이터를 변환 하기 위한 몇 가지 옵션이 있습니다.

- 0-1을 모든 값을 변경할 수 크기 조정 또는으로 백분위 수 순위 절대값 대신를 지정 하는 대로를 나타내는 값을 변환 합니다.
- 단일 열 또는 동일한 데이터 집합의 여러 열에 정규화를 적용할 수 있습니다.
- 실험을 반복 또는 다른 데이터에 동일한 정규화 단계를 적용 해야 할 경우에 정규화 변환 단계를 저장 하 고 동일한 스키마가 있는 다른 데이터 집합에 적용할 수 있습니다.

> [!WARNING]
> 일부 알고리즘에서는 모델을 학습 하기 전에 데이터를 정규화 해야 합니다. 다른 알고리즘 자신의 데이터 크기 조정 또는 정규화를 수행합니다. 따라서 기계 학습 예측 모델을 만드는 데 사용할 알고리즘을 선택 하면 해야 정규화 학습 데이터에 적용 하기 전에 알고리즘의 데이터 요구 사항을 검토 합니다.

##  <a name="configure-normalize-data"></a>구성 데이터 표준화

이 모듈을 사용 하 여 한 번에 하나만 정규화 메서드를 적용할 수 있습니다. 따라서 동일한 정규화 방법은 선택 하는 모든 열에 적용 됩니다. 두 번째 인스턴스를 사용 하 여 다른 정규화 메서드를 사용 하려면 **Normalize-data**합니다.

1. 추가 된 **Normalize-data** 모듈을 실험 합니다. 아래에 있는 Azure Machine Learning에서 모듈을 찾을 수 있습니다 **데이터 변환**를 **크기 조정 및 감소** 범주입니다.

2. 모든 숫자 열을 하나 이상 포함 된 데이터 집합을 연결 합니다.

3. 열 선택기를 사용 하 여 정규화 할 숫자 열을 선택 합니다. 기본적으로 개별 열을 선택 하지 않더라도 **모든** 입력의 숫자 형식 열 포함 되며 동일한 정규화 프로세스는 선택한 모든 열에 적용 됩니다. 

    이 정규화 하지 않아야 하는 숫자 열을 포함 하는 경우 이상한 결과가 발생할 수 있습니다. 항상 열을 신중 하 게 확인 합니다.

    숫자 열이 없으면 감지 되 면 열의 데이터 형식을 지원 되는 숫자 형식 인지 확인 하는 열 메타 데이터를 확인 합니다.

    > [!TIP]
    > 을 보장 하기 위해 특정 유형의 열이 입력으로 제공 되는 사용해 합니다 [Select Columns in Dataset](./select-columns-in-dataset.md) 하기 전에 모듈 **Normalize-data**합니다.

4. **선택 열 상수에 대 한 0 사용**:  모든 숫자 열에는 단일 변경 되지 않는 값을 포함 하는 경우이 옵션을 선택 합니다. 이렇게 하면 이러한 열 정규화 작업에서 사용 되지 않습니다.

5. **변환 메서드** 드롭다운 목록에서 선택한 모든 열에 적용 하는 단일 수학 함수를 선택 합니다. 
  
    - **Zscore**: 모든 값을 z-점수로 변환합니다.
    
      열의 값은 다음 수식을 사용 하 여 변환 됩니다.  
  
      ![z를 사용 하 여 정규화&#45;점수](media/module/aml-normalization-z-score.png)
  
      평균 및 표준 편차는 개별적으로 각 열에 대해 계산 됩니다. 모집단 표준 편차 사용 됩니다.
  
    - **MinMax**: 최소-최대 노 멀 라이저는 [0, 1] 간격으로 모든 기능에 선형 크기 다시 조정 합니다.
    
      [0, 1] 간격으로 크기 조정 최소 값이 0이 되도록 각 기능의 값을 이동 하 고 새 최대값 (즉, 원래 최대 및 최소 값 간의 차이)으로 나눈 다음으로 수행 됩니다.
      
      열의 값은 다음 수식을 사용 하 여 변환 됩니다.  
  
      ![최소값을 사용 하 여 정규화&#45;함수 max](media/module/aml-normalization-minmax.png "AML_normalization minmax")  
  
    - **Logistic**: 열의 값은 다음 수식을 사용 하 여 변환 됩니다.

      ![로지스틱 함수에 의해 정규화에 대 한 수식](media/module/aml-normalization-logistic.png "AML_normalization 로지스틱")  
  
    - **LogNormal**: 이 옵션은 로그 정규 배율로 모든 값으로 변환 합니다.
  
      열의 값은 다음 수식을 사용 하 여 변환 됩니다.
  
      ![수식 로그&#45;정규 분포](media/module/aml-normalization-lognormal.png "AML_normalization 정규")
    
      여기서 μ σ와 계산 데이터에서 최대 예상 유사도로, 각 열에 대해 개별적으로 분포의 매개 변수입니다.  
  
    - **TanH**: 모든 값은 쌍 곡 탄젠트로 변환 됩니다.
    
      열의 값은 다음 수식을 사용 하 여 변환 됩니다.
    
      ![tanh 함수를 사용 하 여 정규화](media/module/aml-normalization-tanh.png "AML_normalization tanh")

6. 실험을 실행 하거나 두 번 클릭 합니다 **Normalize-data** 모듈과 선택 **선택 항목 실행**합니다. 

## <a name="results"></a>결과

합니다 **Normalize-data** 모듈에서는 두 개의 출력을 생성 합니다.

- 변환 된 값을 보려면 모듈을 마우스 오른쪽 단추로 클릭, 선택 **데이터 집합을 변환**, 클릭 **시각화**합니다.

    기본적으로 값은 현재 위치에서 변환 됩니다. 원래 값으로 변환 된 값을 비교 하려는 경우 사용 합니다 [열 추가](./add-columns.md) 모듈을 데이터 집합을 다시 결합 하 고 열을 나란히 표시 합니다.

- 변환에 다른 유사한 데이터 집합에 동일한 정규화 메서드를 적용할 수 있도록 저장 하려면 모듈을 마우스 오른쪽 단추로 클릭, 선택 **변환 함수**, 클릭 **변환으로 저장**합니다.

    저장 된 변환을 로드할 수 있습니다 합니다 **변환** 그룹의 왼쪽된 탐색 창 사용 하 여 동일한 스키마를 사용 하 여 데이터 집합에 적용 [. / 변환 적용](apply-transformation.md)합니다.  


## <a name="next-steps"></a>다음 단계

참조를 [사용할 수 있는 모듈 집합](module-reference.md) Azure Machine Learning 서비스입니다. 