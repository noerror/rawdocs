# Least Squares Conformal Maps for Automatic Texture Atlas Generation

![Untitled](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled.png)

[https://inria.hal.science/inria-00100754/file/lscm.pdf](https://inria.hal.science/inria-00100754/file/lscm.pdf)

[https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf](https://members.loria.fr/Bruno.Levy/papers/LSCM_SIGGRAPH_2002.pdf)

[https://github.com/kyle-gh/LeastSquaresConformalMaps](https://github.com/kyle-gh/LeastSquaresConformalMaps)

### 1 INTRODUCTION

이 논문에서는 색상, 텍스처 등을 추가하여 3D 모델을 세부적으로 수정할 수 있는 3D 페인트 시스템에 대해 설명합니다. 이 프로세스는 모델의 표면 표현에 따라 크게 달라집니다. 특히 이 백서에서는 3D 모델을 차트라고 하는 여러 부분으로 분할하여 각각 고유한 파라미터를 사용하여 표현하는 텍스처 아틀라스라는 기법에 중점을 둡니다. 이 기법은 아티팩트를 줄이고 텍스처 공간을 최적으로 사용하며 텍스처 공간의 균일한 샘플링을 유지하기 위한 것입니다.

텍스처 아틀라스 생성에는 세분화, 파라미터화, 패킹의 세 단계가 포함됩니다. 세분화는 모델을 차트로 분할합니다. 파라미터화는 각 차트를 2D 공간의 하위 집합에 대응시킵니다. 패킹은 차트를 텍스처 공간으로 모읍니다. 이 백서에서는 자연스러운 모양으로 차트를 생성하여 텍스처 아티팩트를 줄이기 위한 새로운 텍스처 아틀라스 생성 방법도 소개합니다.

이러한 각 단계에서는 이전 작업이 강조 표시됩니다. 예를 들어, 이전의 여러 세분화 방법은 텍스처 아틀라스에서 불연속성을 유발하는 많은 수의 차트를 생성합니다. 또한 이 논문에서는 수많은 매개변수화 기법에 대해 언급하고 있는데, 그 중 상당수는 고정 경계 조건, 복잡한 알고리즘이 필요하거나 최적화 중에 로컬 최소값에 갇히는 것에 민감합니다.

최소 제곱 컨포멀 맵(LSCM)이라는 새로운 최적화 기반 매개변수화 방법이 소개됩니다. LSCM은 각도 변형 및 불균일한 스케일링 최소화, 삼각형 방향 유지, 고정된 차트 테두리 필요 없음 등 이전 방법에 비해 여러 가지 장점이 있습니다.

저자는 LSCM과 함께 중요한 기하학적 엔티티에 해당하는 더 큰 차트를 만드는 새로운 세분화 방법을 제시합니다. 이 방법을 사용하면 차트 및 관련 아티팩트 수를 줄일 수 있을 뿐만 아니라 규칙적인 패턴 사용을 간소화할 수 있습니다.

이 논문에서는 차트의 복잡한 모양을 고려하여 이전 접근 방식보다 더 정확하게 텍스처 공간에 차트를 패킹하는 방법도 소개합니다. 이 방법은 게임 '테트리스'의 플레이어가 사용하는 전략에 비유할 수 있습니다. 이 논문은 스캔한 메시와 모델링한 메시를 모두 기반으로 한 결과로 마무리합니다.

## 2 LEAST SQUARES CONFORMAL MAPS

LSCM은 3D 표면을 2D 도메인에 파라미터화하면서 원래 모양을 최대한 유지하려고 노력하는 방법입니다. 이는 텍스처 매핑을 위한 3D 모델링, 즉 2D 이미지를 3D 모델에 적용하는 데 자주 사용됩니다.

![스캔한 말의 몸통은 이 방법의 견고성을 테스트하는 사례로, 복잡한 테두리가 있는 72,438개의 삼각형으로 구성된 매우 큰 단일 차트입니다. A: 결과 아이소 파라미터 곡선, B: 경계가 자동으로 외삽된 해당 펼쳐진 표면, C: 이러한 절단으로 인해 표면이 원판과 동일하게 됨, D: 파라미터화 매개변수화가 견고하며, 동그라미 영역의 큰 삼각형(스캐닝 프로세스 중에 나타나는 그림자 영역으로 인해 발생)의 영향을 받지 않습니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%201.png)

스캔한 말의 몸통은 이 방법의 견고성을 테스트하는 사례로, 복잡한 테두리가 있는 72,438개의 삼각형으로 구성된 매우 큰 단일 차트입니다. A: 결과 아이소 파라미터 곡선, B: 경계가 자동으로 외삽된 해당 펼쳐진 표면, C: 이러한 절단으로 인해 표면이 원판과 동일하게 됨, D: 파라미터화 매개변수화가 견고하며, 동그라미 영역의 큰 삼각형(스캐닝 프로세스 중에 나타나는 그림자 영역으로 인해 발생)의 영향을 받지 않습니다.

목표는 각도를 국부적으로 보존하여 3D 표면의 작은 원을 2D 도메인의 작은 원에 매핑하는 '컨포멀' 매핑을 찾는 것입니다. 이는 매핑이 각도를 얼마나 잘 보존하는지 측정하는 수학적 기준을 통해 공식화되며, 이는 코시-리만 방정식으로 알려진 미분 방정식으로 작성할 수 있습니다.

![컨포멀 맵에서 아이소-U 곡선과 아이소-V 곡선에 대한 접선 벡터는 에 대한 접선 벡터는 직교하며 길이가 동일합니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%202.png)

컨포멀 맵에서 아이소-U 곡선과 아이소-V 곡선에 대한 접선 벡터는 에 대한 접선 벡터는 직교하며 길이가 동일합니다.

일반적으로 전체 3D 표면에 대해 완벽하게 작동하는 컨포멀 맵을 찾는 것은 불가능하기 때문에 저자는 최소 제곱 접근법을 제안합니다. 즉, 코시-리만 방정식을 정확히 만족하는 대신 방정식 위반의 제곱의 합을 최소화하는 맵을 찾는 것입니다.

그런 다음 저자는 이 최적의 맵을 실제로 계산하는 방법을 설명합니다. 저자는 3D 표면을 삼각형(3D 그래픽에서 흔히 볼 수 있는 표현)으로 구성된 것으로 간주하고 이러한 삼각형의 꼭지점으로 적합성 기준을 표현합니다. 이들은 이 문제가 선형 대수 기법을 사용하여 효율적으로 해결할 수 있는 이차 최소화 문제로 축소된다는 것을 보여줍니다.

![우리의 LSCM 파라미터화는 메시의 해상도에 민감하지 않습니다. 거친 메시(그림 A)와 미세한 메시(그림 B)에서 얻은 아이소 파라미터 곡선 (그림 B)에서 얻은 이소 파라미터 곡선은 동일하며, 메시 내에서 해상도가 달라져도 안정적으로 유지됩니다. (그림 C와 D의 동그라미 영역)](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%203.png)

우리의 LSCM 파라미터화는 메시의 해상도에 민감하지 않습니다. 거친 메시(그림 A)와 미세한 메시(그림 B)에서 얻은 아이소 파라미터 곡선 (그림 B)에서 얻은 이소 파라미터 곡선은 동일하며, 메시 내에서 해상도가 달라져도 안정적으로 유지됩니다. (그림 C와 D의 동그라미 영역)

최소화 문제의 몇 가지 특성, 즉 최소 두 개의 정점이 고정되어 있을 때 해의 고유성, 유사도 변환 하에서의 불변성, 메시 해상도의 독립성 등에 대해 설명합니다.

결론적으로 이 섹션에서는 컴퓨터 그래픽의 텍스처 매핑과 같은 애플리케이션에 유용한 각도를 최대한 보존하면서 3D 표면을 2D 도메인에 파라미터화하는 실용적이고 효율적인 방법을 간략하게 설명합니다.

### 3 SEGMENTATION

이는 3D 그래픽 또는 컴퓨터 비전에 관한 텍스트에서 발췌한 것으로 보이며, 여기서 모델은 컴퓨터로 표현된 3차원 객체를 의미하며 모델 분할 알고리즘을 설명합니다.

3D 그래픽의 맥락에서 세분화란 3D 모델을 관리하거나 처리하기 쉬운 '차트'라고 하는 부분으로 나누는 과정을 말합니다.

![A: 특징 감지 알고리즘의 결과, B: 거리_투_시드 함수는 차트 성장 프로세스를 구동하는 데 최적의 선택이 아님, C: 거리_투_피처 함수는 D: 더 자연스러운 모양의 등고선 윤곽을 보여주는 거리_투_피처 함수 D: distance_to_f eatures 함수에 의해 구동되는 세분화 알고리즘의 결과입니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%204.png)

A: 특징 감지 알고리즘의 결과, B: 거리_투_시드 함수는 차트 성장 프로세스를 구동하는 데 최적의 선택이 아님, C: 거리_투_피처 함수는 D: 더 자연스러운 모양의 등고선 윤곽을 보여주는 거리_투_피처 함수 D: distance_to_f eatures 함수에 의해 구동되는 세분화 알고리즘의 결과입니다.

논의된 알고리즘은 두 가지 요구 사항을 충족하는 것을 목표로 합니다:

차트의 경계는 텍스처 아티팩트(3D 모델에 적용된 텍스처의 연속성이 원치 않게 끊어지는 현상)를 최소화하도록 배치되어야 합니다. 이러한 경계는 음영의 변화로 인해 텍스처 아티팩트가 덜 눈에 띄는 높은 곡률 영역에 배치되어야 합니다.
차트는 원반 모양(원반과 동형)이어야 하며, 너무 많은 변형을 일으키지 않고 파라미터화가 가능해야 합니다.
알고리즘은 세 가지 주요 단계로 설명됩니다:

3.1 특징 감지
이 섹션에서는 3D 모델에서 특징(곡률이 높은 영역 또는 기타 중요한 특성)을 감지하는 방법에 대해 설명합니다. 여기에는 선명도 기준(법선 사이의 각도 사용)을 계산하고, 이 기준에 따라 가장자리의 일부를 필터링한 다음 지정된 알고리즘을 사용하여 나머지 가장자리를 따라 "특징 곡선"을 증가시키는 작업이 포함됩니다.

![Untitled](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%205.png)

![Untitled](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%206.png)

3.2 차트 확장
선명한 특징이 감지되면 차트는 반복적으로 최상의 다음 작업을 선택하는 "탐욕스러운" 알고리즘을 사용하여 만들어집니다. 이 방법은 초기 '시드'(시작점) 집합에서 모든 차트를 동시에 확장하여 차트 경계가 이전에 감지된 특징에서 만나도록 합니다. 그런 다음 이 문서에서는 차트 확장 알고리즘과 이 알고리즘이 사용하는 데이터의 세부 사항에 대해 설명합니다.

![A: 세분화 알고리즘이 원통형 모양을 감지하고, B: '양말 모양'의 극단적인 원통에 추가 컷이 추가됩니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%207.png)

A: 세분화 알고리즘이 원통형 모양을 감지하고, B: '양말 모양'의 극단적인 원통에 추가 컷이 추가됩니다.

3.3 차트 유효성 검사
차트를 구성하고 매개변수를 설정한 후에는 테두리의 자체 교차로 인한 중첩이 없는지, 모델 영역과 텍스처 영역의 비율을 특정 임계값 이내로 유지하여 극단적인 영역 변화를 방지하는지 등 두 가지 기준에 따라 차트의 유효성을 검사합니다. 차트가 이러한 기준을 충족하지 못하면 차트는 더 세분화됩니다.

![A: 공룡의 머리는 하나의 차트로 만들어졌으며, B: 공룡의 머리에 삼각형 뒤집기, 테두리의 자체 교차로 인해 겹침이 발생할 수 있음; C: 차트를 세분화하여 차트를 세분화하여 제거할 수 있습니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%208.png)

A: 공룡의 머리는 하나의 차트로 만들어졌으며, B: 공룡의 머리에 삼각형 뒤집기, 테두리의 자체 교차로 인해 겹침이 발생할 수 있음; C: 차트를 세분화하여 차트를 세분화하여 제거할 수 있습니다.

설명 전반에 걸쳐 세분화 알고리즘의 다양한 측면은 특징 감지에 사용되는 임계값 τ, 특징 증가 알고리즘에 사용되는 최대 문자열 길이 및 최소 특징 길이, 영역 변화 검증에 사용되는 최대/최소 비율 임계값과 같은 매개 변수에 의해 제어됩니다. 또한 전체 프로세스 내에서 사용되는 특정 방법이나 알고리즘에 대해서는 다른 연구 논문([11], [15], [29] 등의 숫자로 인용)을 참조합니다.

### 4 PACKING

이 섹션에서는 분해된 3D 모델의 구성 요소인 차트를 텍스처 아틀라스에 패킹하는 알고리즘에 대해 설명합니다. 텍스처 아틀라스는 모델에 사용된 모든 작은 텍스처를 포함하는 큰 이미지입니다. 패킹 프로세스에는 텍스처 아틀라스 내에서 겹치지 않고 사용되지 않는 공간을 최소화하도록 차트를 배열하는 작업이 포함됩니다.

![A: 당사의 패킹 알고리즘은 차트를 하나씩 삽입하고 그 과정에서 '수평선'(파란색)을 유지합니다. 각 차트(녹색)는 아래쪽 수평선(검은색) 사이의 '낭비 공간'을 최소화하는 위치에 하단 수평선(분홍색)과 현재 수평선 사이의 '낭비되는 공간'(검은색)을 최소화합니다. 현재 수평선. 그런 다음 현재 차트의 위쪽 수평선(빨간색)을 사용하여 수평선을 업데이트합니다. B: 공룡 데이터 집합의 결과입니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%209.png)

A: 당사의 패킹 알고리즘은 차트를 하나씩 삽입하고 그 과정에서 '수평선'(파란색)을 유지합니다. 각 차트(녹색)는 아래쪽 수평선(검은색) 사이의 '낭비 공간'을 최소화하는 위치에 하단 수평선(분홍색)과 현재 수평선 사이의 '낭비되는 공간'(검은색)을 최소화합니다. 현재 수평선. 그런 다음 현재 차트의 위쪽 수평선(빨간색)을 사용하여 수평선을 업데이트합니다. B: 공룡 데이터 집합의 결과입니다.

패킹 문제는 NP 완전성으로 알려져 있으며, 이는 계산적으로 풀기가 어렵다는 것을 의미합니다. 이 문제를 해결하기 위해 다양한 휴리스틱이 제안되었습니다. 이 글의 저자들은 '테트리스' 게임에서 영감을 얻은 새로운 접근법을 제안합니다.

그 접근 방식은 다음과 같습니다:

차트의 배율을 조정합니다: 각 차트의 텍스처 아틀라스(u,v) 공간의 면적이 실제 3D 영역(x,y,z) 공간과 같도록 차트의 배율을 재조정합니다.

차트를 정렬합니다: 차트는 최대 직경이 작은 순서로 정렬되며 이러한 직경은 세로 방향입니다.

각 차트의 위쪽 및 아래쪽 수평선을 계산합니다: 수평선은 차트의 대략적인 면적을 나타냅니다. 밉 매핑(텍스처 해상도를 관리하기 위한 컴퓨터 그래픽의 기술)으로 인한 원치 않는 혼합을 방지하기 위해 수평선에 추가 여백이 추가됩니다.

차트를 삽입합니다: 차트가 텍스처 아틀라스에 하나씩 삽입됩니다. 각 차트의 배치는 차트의 현재 수평선과 아래쪽 수평선 사이의 손실된 공간(비어 있는 영역)이 최소화되도록 선택됩니다. 차트를 삽입하면 현재 차트의 상단 수평선을 사용하여 수평선이 업데이트됩니다.

수평선은 이산화된 값의 배열로 표시되며, 이는 텍스처 아틀라스에서 텍스처가 픽셀(텍셀)로 이산화되는 방식과 잘 일치합니다. 따라서 조각 단위의 선형 함수를 사용하는 것보다 알고리즘이 더 간단합니다.

저자들은 이 알고리즘이 테스트한 모든 데이터 세트를 1초 이내에 처리할 수 있을 정도로 성능이 우수하다고 주장합니다.

### 5 RESULTS

이 섹션에서는 제안한 다각형 모델용 텍스처 아틀라스 자동 생성 방법을 다양한 데이터 세트에 적용한 후 얻은 결과를 간략하게 설명합니다. 이러한 데이터 세트에는 3D 모델링된 메쉬와 스캔된 메쉬가 모두 포함되었습니다.

저자들은 이전 방법인 MIPS로 얻은 결과와 동등한 결과를 얻었지만, 이 방법이 수학적 보증을 제공하고 계산적으로 더 효율적이라는 점에 주목했습니다. 3D 모델에서 2D 텍스처 아틀라스로의 매핑 왜곡을 측정하는 '스트레치' 메트릭은 다른 표준 방법으로 얻은 결과와 비슷했습니다.

샌더 등이 제안한 알고리즘으로 이 방법의 결과를 후처리하면 매핑을 더욱 최적화할 수 있습니다. 차트의 테두리가 임의로 고정되지 않고 자연스럽게 배치되기 때문에 더 나은 결과를 얻을 수 있습니다.

또한 텍스처 아틀라스에는 치아나 발굽과 같은 미세한 기하학적 디테일을 나타내는 작은 차트가 종종 존재한다는 점에 주목합니다. 이러한 차트는 대부분의 페인트 시스템에서 올바르게 처리할 수 있으므로 문제가 되지 않습니다. 텍스처 모델의 몇 가지 예는 그림 8과 10에 나와 있습니다.

저자들은 모델을 차트로 분할하고 파라미터화하는 데 걸린 시간, 알고리즘을 사용하여 달성한 패킹 비율과 다른 방법, 후처리 최적화 적용 전후에 측정한 스트레치 등 테스트에서 얻은 몇 가지 통계 데이터를 제공합니다. 저자는 매핑이 각 삼각형에서 거의 등각에 가깝고, LSCM 기준이 특별히 영역 변형을 처벌하는 것을 목표로 하지 않음에도 불구하고 왜곡된 면이 거의 없다는 점에 주목합니다.

결론적으로, 이들은 합성 데이터 세트와 스캔 데이터 세트 모두에 자신들의 기술을 성공적으로 적용했으며 기존 방법보다 효율성과 견고성이 입증되었다고 강조합니다. 이 방법은 3D 모델을 의미 있는 기하학적 엔티티로 분해하여 보다 효율적이고 실제 프로덕션 환경에 적합하게 만듭니다.

이들은 리메싱 및 데이터 압축과 같은 다른 분야에서도 알고리즘의 잠재력을 보고 있습니다. 향후에는 아웃오브코어 알고리즘을 사용하여 더 큰 모델에서 이 방법을 테스트하고 LSCM 기준을 최소화하기 위한 다른 수치적 방법을 모색할 계획입니다. 또한 향후 연구 분야로 스트레치 기준을 LSCM 기준에 직접 포함시킬 수 있는 가능성을 제시하고 있습니다.

### A PROPERTIES OF LSCMS

3D 공간에서 삼각형화된 표면을 매개변수화하는 기술인 최소 제곱 컨포멀 맵(LSCM)의 속성에 대해 논의하고 있습니다. 삼각형의 모양을 최대한 보존하면서 3D 표면을 2D 평면에 매핑하는 것이 목표입니다. 이러한 속성에는 전체 순위, 단일 최소값, 유사도에 따른 불변성, 해상도에 대한 독립성, 방향 보존 등이 포함됩니다. 요약하면 다음과 같습니다:

![위상적으로 원판인 삼각형을 점진적으로 구성합니다. 왼쪽: 가장자리를 따라 두 개의 삼각형을 붙입니다. 오른쪽: 두 개의 기존 꼭지점을 결합하여 새 삼각형을 만듭니다.](Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204/Untitled%2010.png)

위상적으로 원판인 삼각형을 점진적으로 구성합니다. 왼쪽: 가장자리를 따라 두 개의 삼각형을 붙입니다. 오른쪽: 두 개의 기존 꼭지점을 결합하여 새 삼각형을 만듭니다.

A.1 전체 순위
행렬 Mf와 A는 p가 2를 초과할 때 전체 순위를 갖습니다. 이는 행렬이 반전 가능하다는 것을 의미하며, 이는 최소화 문제를 푸는 데 중요합니다. 전체 순위는 삼각 측량의 증분 구성에 기반한 귀납법으로 증명됩니다.

A.2 단일 최소값
p가 2를 초과할 때 함수 C(U)는 고유한 최소값을 갖는다는 것을 알 수 있습니다. 이는 A의 그램 행렬이 반전 가능하여 최소화 문제에 대한 고유한 해를 유도한다는 것을 보여줌으로써 증명됩니다. 이 결과는 A의 전체 순위 속성에 따라 달라집니다.

A.3 유사성에 의한 불변성
최적의 지도 U를 찾는 문제는 유사도 변환에 불변합니다. 즉, U의 크기를 조정하거나 회전하거나 변환해도 함수 C(U)의 값은 동일하게 유지됩니다. 이 특성은 U를 zU + T로 대체할 때 C(U)가 변하지 않는다는 것을 증명함으로써 알 수 있습니다. 여기서 z는 복소수이고 T는 상수 벡터입니다.

A.4 해상도에 대한 독립성
메쉬를 조밀화하면(즉, 삼각형을 더 추가하면) 초기 메쉬의 정점에서의 최소화 문제에 대한 해는 변하지 않습니다. 이는 하나의 삼각형이 세 개의 삼각형으로 분할되는 경우를 고려하면 증명됩니다. 이 경우 이전 정점의 새로운 매개변수화는 이전과 동일하며, 새 정점의 매개변수화는 이전 정점의 매개변수화의 평균이 됩니다.

A.5 방향 보존
마지막으로 최소 제곱 등각 컨포멀 맵은 방향을 보존하므로 삼각형을 뒤집지 않는다는 것을 알 수 있습니다. 이 속성은 삼각 측량이 점진적으로 구성된다고 가정하고 각 단계에서 매개변수 공간의 삼각형의 방향이 원래 3D 공간의 방향과 일관되게 유지됨을 보여줌으로써 증명됩니다.