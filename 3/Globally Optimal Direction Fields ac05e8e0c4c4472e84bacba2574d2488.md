# Globally Optimal Direction Fields

[https://www.youtube.com/watch?v=uU0iZSN4Hgo](https://www.youtube.com/watch?v=uU0iZSN4Hgo)

[https://www.cs.cmu.edu/~kmcrane/Projects/GloballyOptimalDirectionFields/paper.pdf](https://www.cs.cmu.edu/~kmcrane/Projects/GloballyOptimalDirectionFields/paper.pdf)

- Globally Optimal Direction Fields
    
    "전역 최적 방향 필드"는 표면에서 가능한 가장 낮은 에너지(즉, 부드러움 및/또는 유도 필드와의 정렬)를 갖는 최상의 n 방향 필드를 찾는 프로세스를 의미합니다. "전역"이라는 용어는 최적의 솔루션이 아닐 수 있는 국부적인 최소값이 아니라 가능한 모든 구성 중에서 찾은 솔루션이 가장 좋다는 것을 나타냅니다.
    
    n 방향 필드는 서페이스의 각 점에 균등한 간격의 단위 탄젠트 벡터 집합을 할당합니다. 필드의 평활도는 필드가 서피스 전체에서 갑작스러운 변화 없이 얼마나 잘 변화하는지를 나타내며, 정렬은 필드가 주어진 안내 필드(예: 주 곡률 방향)에 얼마나 잘 정렬되는지를 나타냅니다. 전역 최적 방향 필드는 평활도, 정렬 및 기타 기준 간의 균형을 맞추는 동시에 특이점(필드가 원활하게 변화하지 않는 고립된 지점)의 최적 수, 배치 및 지수를 자동으로 결정하는 것을 목표로 합니다.
    
    이러한 맥락에서 전 세계적으로 최적의 방향 필드를 생성하는 방법에는 일반적으로 평활도, 정렬 및 기타 원하는 속성 간의 균형을 고려하여 표면에서 가능한 최상의 n 방향 필드를 쉽게 찾을 수 있는 효율적인 수치 알고리즘과 에너지 공식이 포함됩니다.
    
    ![스탠포드 토끼 위에서 가장 매끄러운 단위 벡터 필드 단일 고유 벡터 문제를 풀어서 계산된 모든 가능한 특이점 구성의 단일 고유 벡터 문제. 빨간색과 파란색 구체는 각각 양의 및 음의 특이점을 각각 나타냅니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled.png)
    
    스탠포드 토끼 위에서 가장 매끄러운 단위 벡터 필드 단일 고유 벡터 문제를 풀어서 계산된 모든 가능한 특이점 구성의 단일 고유 벡터 문제. 빨간색과 파란색 구체는 각각 양의 및 음의 특이점을 각각 나타냅니다.
    

우리는 표면에서 부드러운 n-방향 필드(선 필드, 십자 필드 등)를 구성하는 방법을 제시합니다. 이 방법은 최첨단 기법보다 한 차원 빠르면서도 동일하거나 더 나은 품질의 필드를 생성합니다. 이 방법으로 생성된 필드는 가능한 모든 특이점 구성(개수, 위치 및 지수)에 대한 간단하고 명확한 2차 부드러움 에너지를 최소화함으로써 전역적으로 최적입니다. 이 방법은 완전히 자동이며 주요 곡률 방향과 같은 주어진 가이던스 필드와 정렬된 필드를 선택적으로 생성할 수 있습니다. 계산적으로 가장 부드러운 필드는 코탄 라플라시안과 유사한 행렬을 포함하는 희소 고유값 문제를 통해 찾아집니다. 가이던스 필드가 있는 경우, 최적의 필드를 찾는 것은 단일 선형 시스템을 푸는 것으로 이루어집니다.

![왼쪽에서 오른쪽으로: 각각 인덱스 +1, +1/2 , +1/4 의 특이점 근처에서 n = 1(방향), n = 2(선), n = 4(십자)에 대한 n 방향 필드의 예입니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%201.png)

왼쪽에서 오른쪽으로: 각각 인덱스 +1, +1/2 , +1/4 의 특이점 근처에서 n = 1(방향), n = 2(선), n = 4(십자)에 대한 n 방향 필드의 예입니다.

![정수 변수를 포함하는 평활화 에너지는 공식화하기는 쉽지만 공식화하기는 쉽지만 최소화하기는 어렵습니다. 왼쪽: Bommes 등[2009]의 혼합 정수 방법으로 Bommes 등[2009]의 혼합 정수 방법에 의해 생성된 솔루션. 가운데: s = 0인 우리의 방법으로 생성된 해. 오른쪽: s = 1인 우리의 방법으로 생성된 해 s = 1로 우리의 방법으로 생성된 해법. 두 솔루션 모두 글로벌 최소화자이며, 매개변수 s는 평활도와 특이점 수 사이에서 와 특이점 수 사이의 절충점을 제공합니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%202.png)

정수 변수를 포함하는 평활화 에너지는 공식화하기는 쉽지만 공식화하기는 쉽지만 최소화하기는 어렵습니다. 왼쪽: Bommes 등[2009]의 혼합 정수 방법으로 Bommes 등[2009]의 혼합 정수 방법에 의해 생성된 솔루션. 가운데: s = 0인 우리의 방법으로 생성된 해. 오른쪽: s = 1인 우리의 방법으로 생성된 해 s = 1로 우리의 방법으로 생성된 해법. 두 솔루션 모두 글로벌 최소화자이며, 매개변수 s는 평활도와 특이점 수 사이에서 와 특이점 수 사이의 절충점을 제공합니다.

### 1 Introduction

저자는 표면에서 가장 매끄러운 n 방향 필드를 계산하는 방법을 제안합니다. n 방향 필드는 균등한 간격의 단위 탄젠트 벡터 집합을 표면의 각 점에 연결합니다. 이러한 필드에는 특이점, 즉 필드가 매끄럽게 변화하지 않는 고립된 점이 있어야 합니다. 목표는 2π/n의 정수 배수만큼 각도가 다른 방향을 식별하는 것과 함께 최적의 특이점의 수, 배치 및 인덱스를 결정하는 것입니다.

이 방법에는 두 가지 핵심 요소가 있습니다:

- n 방향 필드는 임의의(그러나 고정된) 접선 기저 방향과 함께 각 정점에 복소수의 n 번째 거듭제곱을 저장하여 표현됩니다. 이 접근 방식은 이전 방법에서 사용된 주기 점프나 삼각 함수가 필요하지 않습니다.
    
    표면에서 가장 매끄러운 n 방향 필드를 계산하기 위해 제안된 방법에서 저자는 각 정점에 복소수의 n 번째 거듭제곱을 임의의(그러나 고정된) 접선 기저 방향과 함께 저장하여 n 방향 필드를 표현합니다.
    
    n 방향 필드는 균등한 간격의 단위 탄젠트 벡터 집합을 서페이스의 각 점과 연결합니다. 이러한 필드를 n의 거듭제곱으로 올린 복소수를 사용하여 표현하면 이전 방법에서 필요했던 주기 점프나 삼각 함수를 사용하지 않고도 필드의 기본 지오메트리를 캡처할 수 있습니다.
    
    주기 점프와 삼각 함수는 이전 방법을 더 복잡하고 최적화하기 어렵게 만드는 경우가 많았습니다. 저자는 이 새로운 n 방향 필드 표현을 사용하여 문제를 단순화하고 필드의 평활성을 더 쉽게 최적화할 수 있도록 했습니다. 주기 점프와 삼각 함수를 제거함으로써 표면에서 매끄러운 n 방향 필드를 계산하는 더 효율적이고 간단한 알고리즘을 만들 수 있습니다.
    
- n 방향 필드의 평활도는 적절한 슈뢰딩거 연산자의 기저 상태 에너지를 사용하여 측정됩니다. 이 공식은 각 벡터에 비볼록 단위 규범 제약 조건이 필요하지 않으며, 단일 n 방향 필드에 대해서도 잘 정의되어 있습니다.
    
    n 방향 필드의 맥락에서 평활도는 필드가 표면에서 어떻게 변하는지를 결정하는 중요한 속성입니다. 슈뢰딩거 연산자의 기저 상태 에너지는 n 방향 필드의 평활도를 측정하는 데 사용됩니다. 이 접근 방식은 다른 방법에 비해 몇 가지 장점이 있습니다:
    
    비볼록 단위 규범 제약이 없습니다: 방향 필드의 평활도를 측정하는 기존 방법에는 각 벡터에 비볼록 단위 규범 제약 조건이 포함되는 경우가 많아 최적화 문제가 더 까다로워질 수 있습니다. 슈뢰딩거 연산자의 기저 상태 에너지를 사용하면 이러한 제약 조건이 필요하지 않으므로 최적화 프로세스가 간소화됩니다.
    
    특이 n 방향 필드에 대해 잘 정의되어 있습니다: 특이점은 표면에서 필드가 원활하게 변화하지 않는 지점을 말합니다. 슈뢰딩거 연산자 기반 공식은 n방향 필드에 특이점이 있는 경우에도 잘 정의되어 있어 일관된 평활도 측정을 보장합니다.
    
    요약하면, 적절한 슈뢰딩거 연산자의 접지 상태 에너지를 사용하여 n 방향 필드의 평활도를 측정하는 것은 기존 방법에 비해 더 편리하고 잘 정의된 접근 방식을 제공하므로 필드를 더 쉽게 최적화하고 분석할 수 있습니다.
    

또한 저자는 필드 라인의 직진도와 총 특이점 수 사이의 균형을 제공하는 "기하학 인식" 평활도 에너지의 연속체를 소개합니다. 또한 평활도와 안내 필드와의 정렬 간에 균형을 맞출 수 있어 파라미터를 세심하게 조정할 필요 없이 간단한 자동 체계를 구현할 수 있습니다.

삼각형 메시의 경우 이 알고리즘은 코탄-라플라시안과 유사하지만 실제 변수 대신 복잡한 변수가 포함된 행렬을 설정해야 합니다. 이 행렬의 가장 작은 고유값에 해당하는 고유 벡터를 찾으면 평활도 에너지의 전역 최소값을 구할 수 있습니다. 주어진 필드와의 정렬이 필요한 경우 단일 선형 시스템을 풀면 가중 평활도 및 정렬 에너지의 전역 최소값을 구할 수 있습니다.

### 1.1 Related Work

이 연구는 표면 상의 부드러운 n-방향 필드 계산을 탐구하며, 이러한 계산은 비 사실감 렌더링, 텍스처 합성, 매개변수화 및 리메시 등의 응용 분야에서 중요한 구성 요소입니다. 이러한 응용 프로그램에서는 종종 주요 곡률 방향을 근사하는 부드러운 필드에 관심이 있습니다. 이러한 에너지를 공식화하는 것은 상대적으로 간단하지만, 일반적으로 비볼록하고 최소화하기가 NP-하드입니다. 지금까지 전역적으로 가장 부드러운 n-방향 필드를 찾는 간단한 볼록 공식화를 제시하는 것은 연구자들에게는 어려운 일이었습니다.

- 과거의 접근법에서는 사용자가 특이점(임의의 위상을 가진 표면에서 존재해야 함)의 위치를 제공할 필요가 없었습니다. 미리 지정된 특이점이 있는 경우, Fisher 등[2007]의 벡터 필드와 Crane 등[2010]의 n-방향 필드에 의해 간단한 이차 공식화가 가능합니다.
    
    앞서 언급한 접근 방식에서는 사용자가 임의의 토폴로지를 가진 서피스에서 불가피하게 발생하는 특이점의 위치를 제공할 필요가 없습니다. 특이점은 필드가 원활하게 변화하지 않는 고립된 지점입니다. 이러한 접근 방식에 제시된 알고리즘과 공식은 최적의 특이점의 수, 위치, 인덱스를 자동으로 결정할 수 있어 사용자의 전반적인 프로세스를 간소화합니다.
    
    특이점을 미리 지정하면 벡터 필드와 n 방향 필드에 대한 공식이 훨씬 더 간단해질 수 있습니다. 피셔 등[2007]은 벡터 필드에 대해 이를 입증했으며, 크레인 등[2010]은 n 방향 필드에 대해서도 동일한 결과를 보여주었습니다. 이러한 이차 공식은 최적화와 계산을 더 쉽게 할 수 있게 해주며, 특이점 위치가 이미 알려진 경우 프로세스를 더 효율적으로 만듭니다.
    
    요약하면, 앞서 언급한 접근 방식을 통해 사용자는 특이점 위치를 수동으로 지정할 필요 없이 n방향 필드로 작업할 수 있습니다. 그러나 이러한 위치가 제공되면 알고리즘을 단순화하고 최적화할 수 있어 보다 효율적인 계산과 결과를 얻을 수 있습니다.
    

이 연구의 표현은 Kass와 Witkin[1987]의 연구와 가장 유사하며, 이미지에서 방향을 고려하지 않는 선 필드(n=2)를 추출하기 위해 제곱된 복소수를 사용했습니다. Palacios와 Zhang[2007]은 임의의 n에 대한 유사한 아이디어를 제안하지만, 실제 표현 벡터(Rcos(n), Rsin(n))를 사용하고 접선 공간 간의 변환을 행렬을 통해 인코딩합니다. 이러한 표현은 매력적이지만, 어느 연구에서도 표면에서 최적으로 부드러운 필드를 찾는 문제를 다루지 않습니다.

### 2 n-Vector Fields on Surfaces

이 섹션에서는 실평면과 복소선의 개념을 사용하여 연속 및 이산 설정 모두에서 서페이스에서 n-벡터 필드의 표현에 대해 설명합니다. 이 섹션에서는 곡면 M의 한 점 p에서의 접선 공간 TpM을 복소수 C의 복사본으로 볼 수 있다고 제안합니다. 이러한 맥락에서 접선 다발 TM을 복소선 다발이라고 하며, 각 점 p에서의 TpM에서 단위 기저 벡터 Xp의 선택을 기저 구간이라고 합니다.

n-벡터 필드는 단일 복소 함수 u := zn을 사용하여 간결하게 나타낼 수 있습니다. 개별 벡터 필드를 복구하기 위해 이러한 함수의 n번째 근을 계산합니다. 이러한 함수의 의미는 선택한 기저 섹션에 상대적이므로 서로 다른 접하는 공간의 벡터를 비교할 때는 병렬 전송을 고려해야 합니다.

병렬 전송은 평면 내 회전 없이 한 탄젠트 공간에서 다른 탄젠트 공간으로 벡터를 매핑하는 프로세스입니다. 이 작업은 병렬 전송 맵 Ppq를 사용하여 수행되며, 이 맵은 Xp에 대한 계수 zp를 Xq에 대한 계수 e^(iθpq) zp에 매핑합니다. 두 벡터 간의 차이는 |e^(iθpq) zp - zq|^2로 정의되고, 두 n-벡터 간의 차이는 |rpq up - uq|^2로 표현되며, 여기서 ρpq := nθpq 및 해당 전송 계수 rpq := e^(iρpq)로 표현됩니다.

중요한 점은 계수 r이 일정하고 M의 기하학적 구조, 기저 단면 X의 선택, 필드의 차수 n에만 의존하기 때문에 이 표현을 사용하면 z에서 이차식으로 표현할 수 있다는 것입니다.

![한 탄젠트 공간에서 다른 탄젠트 공간으로 벡터를 전송하려면 벡터가 두 접하는 두 공간을 연결하는 측지선 에 대한 각도를 측정하는 것으로 충분합니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%203.png)

한 탄젠트 공간에서 다른 탄젠트 공간으로 벡터를 전송하려면 벡터가 두 접하는 두 공간을 연결하는 측지선 에 대한 각도를 측정하는 것으로 충분합니다.

- 접선 공간을 복소수로 표현한다는 것은 접선 공간을 2차원 실수 벡터 공간이 아닌 복소선이라고도 하는 1차원 복소 벡터 공간으로 본다는 의미입니다.
    
    전통적으로 곡면 M의 한 점 p에서 접하는 접선 공간 TpM은 실제 유클리드 평면 R²의 복사본으로 간주됩니다. 이 표현에서 모든 탄젠트 벡터는 두 기저 벡터인 e1과 e2의 실제 선형 조합, 즉 x * e1 + y * e2로 표현할 수 있으며, 여기서 x와 y는 실제 계수입니다.
    
    다른 접근 방식은 탄젠트 공간 TpM을 복소수 C의 복사본으로 간주하는 것입니다. 이 경우 모든 벡터는 단일 기저 벡터 e1의 복소배수로 표현할 수 있습니다. 즉, 탄젠트 벡터는 z * e1로 표현할 수 있으며, 여기서 z는 복소수입니다.
    
    접선 공간을 복소선으로 취급하면 특정 수학적 연산과 조작을 단순화할 수 있으며 공간의 기하학적 특성을 더 쉽게 분석할 수 있습니다. 이 표현은 미분 기하학 및 관련 분야에서 n 방향 필드와 복소선 다발로 작업할 때 특히 유용합니다.
    

### 3 Smooth n-Direction Fields

이 단원에서는 디리클레 에너지를 사용하여 n 방향 필드의 평활성을 측정하는 데 따르는 어려움에 대해 논의하고, 가능한 모든 특이점 구성 중에서 가장 평활한 n 방향 필드를 찾기 위한 대체 방법인 Eˆ를 제안합니다.

디리클레 에너지에는 최적화를 어렵게 만드는 점 단위 제약 조건과 특이 n 방향 필드에 대해 무한한 경향이 있다는 두 가지 주요 문제가 있습니다.

대체 에너지 측정값인 Eˆ는 주어진 방향 필드에 평행한 모든 n-벡터 필드 중에서 가장 낮은 디리클레 에너지로 정의됩니다. 이 에너지를 최소화하려면 물리학의 시간 독립적 슈뢰딩거 방정식과 유사한 고유값 문제를 풀어야 하며, 이를 통해 특이 n 방향 필드에 대해서도 에너지 측정값을 잘 정의할 수 있습니다.

![우리의 알고리즘은 모든 정도의 필드를 생성할 수 있습니다. 3(왼쪽) 및 n = 5(오른쪽)의 홀로모픽 에너지를 사용하여 필드를 생성할 수 있습니다](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%204.png)

우리의 알고리즘은 모든 정도의 필드를 생성할 수 있습니다. 3(왼쪽) 및 n = 5(오른쪽)의 홀로모픽 에너지를 사용하여 필드를 생성할 수 있습니다

모든 단위 필드에서 Eˆ(')를 최소화하면 전 세계적으로 가장 매끄러운 n 방향 필드를 찾을 수 있습니다. 여기에는 가장 매끄러운 n-벡터 필드를 찾고 결과 벡터를 정규화하는 작업이 포함됩니다.

Eˆ(')의 물리적 해석은 전 세계적으로 가장 매끄러운 n 방향 필드가 특이점이 적은 이유에 대한 통찰력을 제공합니다. 회전 속도에 극이 너무 많으면 위치 에너지가 증가하여 해당 파동 함수의 기저 상태 에너지가 높아집니다.

![홀로모픽 에너지로 측정한 다양한 n에 대한 가장 매끄러운 방향 필드입니다. n이 클수록 특이점이 작은 n에서 분리되는 것처럼 그룹으로 발생합니다](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%205.png)

홀로모픽 에너지로 측정한 다양한 n에 대한 가장 매끄러운 방향 필드입니다. n이 클수록 특이점이 작은 n에서 분리되는 것처럼 그룹으로 발생합니다

요약하면, 이 접근법은 디리클레 에너지의 문제를 극복하는 대체 에너지 측정값인 Eˆ를 최소화하여 표면에서 가장 매끄러운 n 방향 필드를 찾는 방법을 제공합니다. 이 방법은 고유값 문제를 해결해야 하며, 가장 매끄러운 n 방향 필드의 동작을 설명하는 데 도움이 되는 물리적 해석을 제공합니다.

### 4 Quadratic Smoothness Energies

이 글에서는 표면의 기하학적 구조를 고려하면서 표면에 부드러운 벡터 필드를 생성하는 새로운 접근 방식에 대해 설명합니다. 일반적으로 디리클레 에너지가 이러한 목적으로 사용되지만, 저자는 홀로모픽 및 반홀모픽 용어(EH 및 EA)를 결합하여 보다 다양한 방법을 제안합니다.

평활도 에너지(Es)는 매개변수 's'로 제어되는 이러한 용어의 혼합으로 정의됩니다. 이 파라미터를 사용하면 반동형(s = -1)에서 디리클레(s = 0), 홀로모픽(s = 1) 에너지에 이르는 연속적인 범위의 에너지를 구현할 수 있습니다. 's'를 조정하면 평활도 에너지가 표면의 지오메트리를 더 잘 설명할 수 있습니다.

![부드러운 벡터 필드를 찾으려고 할 때, 특이점(필드가 잘 정의되지 않은 위치)이 적은 것과 직선 필드 라인을 갖는 것 사이에서 종종 상충하는 경우가 있습니다. 이 글에서 설명하는 방법은 지속적으로 변화하는 매개변수 's'를 사용하여 이 두 가지 목표의 균형을 맞추는 방법을 제공합니다. 's'를 1(홀로모픽 에너지)로 설정하면 이 방법은 일반적으로 더 적은 수의 특이점을 생성합니다. 's'를 0(디리클레 에너지)으로 설정하면 더 곧은 필드 라인을 생성합니다. 's'를 -1(반홀로모픽 에너지)로 설정하면 이 두 가지를 절충하여 특이점 최소화와 직선 필드 라인 유지 사이의 균형을 맞출 수 있습니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%206.png)

부드러운 벡터 필드를 찾으려고 할 때, 특이점(필드가 잘 정의되지 않은 위치)이 적은 것과 직선 필드 라인을 갖는 것 사이에서 종종 상충하는 경우가 있습니다. 이 글에서 설명하는 방법은 지속적으로 변화하는 매개변수 's'를 사용하여 이 두 가지 목표의 균형을 맞추는 방법을 제공합니다. 's'를 1(홀로모픽 에너지)로 설정하면 이 방법은 일반적으로 더 적은 수의 특이점을 생성합니다. 's'를 0(디리클레 에너지)으로 설정하면 더 곧은 필드 라인을 생성합니다. 's'를 -1(반홀로모픽 에너지)로 설정하면 이 두 가지를 절충하여 특이점 최소화와 직선 필드 라인 유지 사이의 균형을 맞출 수 있습니다.

단위 규범을 가진 최적의 평활 벡터 필드를 찾으려면 Es에 의해 결정된 이차식(A)을 사용하여 고유값 문제를 푸는 것이 좋습니다.

또한 이 단원에서는 홀로모픽 필드와 반홀로모픽 필드, 조화 형태, 표면 파라미터화 및 가장 평활한 1-벡터 필드 계산과 같은 기존 컴퓨터 그래픽 기술 간의 연관성을 살펴봅니다.

요약하면, 이 단원에서는 표면의 벡터 필드 선의 특이점 수와 직진도(또는 측지 곡률) 사이의 균형을 제공하는 보다 기하학적 인식 평활도 에너지를 개발하는 것을 목표로 합니다.

### 5 Alignment Energies

주어진 필드에서 평활도와 정렬의 균형을 맞추기 위해 정렬에 함수 El이 사용됩니다. 결합 에너지 함수 Es,t는 평활도 에너지 Es와 정렬 에너지 El을 결합하여 생성되며, 여기서 t(0과 1 사이)는 정렬의 강도를 제어합니다. Es,t를 최소화하기 위해 (A - tI)e = λ 방정식을 풀면, 여기서 A는 Es,t에 대응하는 이차 방정식입니다. 그런 다음 해를 정규화합니다. 매개변수 t는 정렬과 평활성 사이의 균형을 제어합니다. 필드의 점 방향 크기에 따라 정렬과 평활성 항 사이의 국부 가중치가 결정됩니다. 일부 영역에서 크기를 0으로 설정하면 크기가 0보다 큰 영역에서 데이터가 부드럽게 보간됩니다.

![정렬 항을 추가하면, 곡률 정렬 필드와 곡률 정렬 필드(왼쪽 위)와 평활 필드(오른쪽 아래)를 보간할 수 있습니다. 여기서 s = 0이고 1은 가장 작은 고유값입니다. 곡률을 안내 필드로 사용합니다](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%207.png)

정렬 항을 추가하면, 곡률 정렬 필드와 곡률 정렬 필드(왼쪽 위)와 평활 필드(오른쪽 아래)를 보간할 수 있습니다. 여기서 s = 0이고 1은 가장 작은 고유값입니다. 곡률을 안내 필드로 사용합니다

섹션 5에서 저자는 주어진 방정식에 대한 특정 해법이 모든 단위 규범 필드에 대한 평활화 에너지(Es,t)의 전역 최소값을 제공한다고 주장합니다. 이 설명은 n 방향 필드의 정렬과 평활화 사이의 트레이드오프에 영향을 미치는 해와 매개변수 t 사이의 관계를 보여줍니다.

저자는 그들이 찾은 해가 제약 최소값에 필요한 조건을 충족한다는 것을 증명합니다. 그들은 t -> 0의 경우 평활화 에너지가 가장 작은 고유값을 가진 고유 벡터에 접근한다는 것을 보여줍니다(우리는 양의 반정밀 허미티안 연산자로 작업하고 있습니다). t -> 1로 갈수록 평활 에너지는 1에 가까워집니다.

그런 다음 저자는 (1,1)에서 (0,1)로 매핑하는 엄격하게 단조로운 분석 함수 t()가 있음을 보여줍니다. 이 함수는 평활 에너지 파라미터를 정렬과 평활 사이의 트레이드오프에 영향을 미치는 파라미터 t와 연관시킵니다.

요약하면, 저자들은 그들의 솔루션이 평활도 에너지(Es,t)에 대한 전역 최소값을 매개변수 t와 특정 관계로 제공한다는 것을 증명합니다. 평활도 에너지 매개변수가 변함에 따라 정렬과 평활화 사이의 트레이드오프에 영향을 미치므로 다양한 특성을 가진 n 방향 필드의 연속 범위가 허용됩니다.

### 6 Computation on Triangle Meshes

지금까지 부드러운 설정에 대해 설명했으며, 이제 조각 선형(PL) 기준 섹션을 사용하여 트라이앵글 메시의 수치 계산을 다뤄보겠습니다. 입력이 경계가 있든 없든 방향성이 있는 2다양체 트라이앵글 메시라고 가정합니다. 정점은 R3 좌표를 가지며, 나머지 표면은 선형 보간을 통해 정의됩니다.

각 정점은 단위 기저 벡터와 이 기저에 대해 연관된 n-벡터를 나타내는 계수를 전달합니다. 탄젠트 공간에서 각도를 측정하기 위해 각 정점에서 유클리드 각을 재조정합니다. 한 정점에서 다른 정점으로의 병렬 전송을 위해 재조정된 유클리드 각을 기반으로 한 전송 계수를 사용합니다.

곡률은 각 삼각형에 대한 선 다발의 곡률을 나타내는 홀로노미로 계산됩니다. 각 삼각형에 대해 매트릭스 조립에 필요한 이 값을 계산합니다.

유한 요소법의 경우, 정점의 단위 기저 벡터는 입사 삼각형으로 병렬로 전송되고 표준 모자 함수로 선형 감쇠됩니다. 이렇게 하면 각 정점에 입사하는 삼각형에 PL 기저 섹션이 지원됩니다. 이제 모든 필드는 기저 섹션의 복잡한 선형 조합으로 주어지는 조각 단위 선형입니다.

### 6.1 Algorithms

Smoothest Field 연속 설정에서 가장 평활한 필드 문제(방정식 8)는 이산 설정에서 일반화된 행렬 고유 벡터 문제가 됩니다:

Au = Mu. (15)

여기서 A는 조각별 선형 기저 섹션에 대한 에너지 Es를 나타내는 |V| x |V| 헤르미시안 행렬이고, M은 헤르미시안 질량 행렬입니다. 가장 작은 고유값에 해당하는 고유 벡터를 찾기 위해 역전력 반복을 사용하여 방정식 (15)를 풉니다. 자세한 내용은 섹션 7에서 확인할 수 있습니다.

Aligned Field 정렬된 필드에 대한 방정식 (10)의 연속 문제는 이산 설정에서 행렬 문제로 바뀝니다:

(A - tM)u˜ = Mq. (16)

이 방정식에서 A와 M은 이전과 동일한 에르미트 행렬이고, q는 유도 필드의 부분 선형 버전에 대한 계수 벡터입니다. Es,t의 이산화된 버전을 최소화하는 단위 벡터 u는 t = (1 + ||u˜|)^(-1)에 대해 u = u˜/||u˜|로 주어집니다. t와 t 사이의 관계는 단조롭지만 이에 대한 폐쇄형 식은 없습니다. 실제로는 t = 0으로 설정하는 것이 문제 해결을 위한 좋은 시작 값인 경우가 많습니다.

### 6.1.1 Matrix Entries

A와 M의 항목은 적분으로 주어지며, 모든 삼각형에 대한 루프를 통해 조합할 수 있습니다. 즉, t_ijk에 대한 적분의 합은 로컬 3x3 행렬을 계산하는 데 사용되며, 이 행렬의 항목은 닫힌 형태로 유도할 수 있습니다.

질량 행렬: 삼각형 t_ijk에서 L2 내적 곱에 의해 유도된 국부 질량 행렬은 방정식 (17)로 주어집니다. 오른쪽 분수는 곡률이 없는 한계(Ω_ijk → 0)에서 제거 가능한 특이점을 가지며, 예상대로 1/12 값을 갖습니다.

에너지 행렬: 라플라시안(디리클레 에너지 항)으로 시작하여 국부 에너지 행렬은 방정식 (18)로 주어집니다. f1과 f2의 특이점은 제거할 수 있으며, Ω_ijk → 0이 되면 평평한 설정에서 표준 코탄-라플라시안으로 복구됩니다.

경계: 에너지 행렬을 삼각형 단위로 조립하면 메시의 경계가 자동으로 계산됩니다. 디리클레 에너지(s = 0)의 경우 이는 제로 노이만 조건에 해당하며, s ≠ 0의 경우 EA 및 EH로 인해 경계 항이 존재합니다.

![삼각형별 로컬 행렬을 전역 행렬로 조립하여 행렬로 조립하면 경계 조건이 자동으로 고려됩니다. 여기 반동형 에너지 및 곡률 정렬( = 0)을 최소(왼쪽, n = 2) 및 양쪽(오른쪽, n = 4) 주 곡률 방향으로 사용하는 예제입니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%208.png)

삼각형별 로컬 행렬을 전역 행렬로 조립하여 행렬로 조립하면 경계 조건이 자동으로 고려됩니다. 여기 반동형 에너지 및 곡률 정렬( = 0)을 최소(왼쪽, n = 2) 및 양쪽(오른쪽, n = 4) 주 곡률 방향으로 사용하는 예제입니다.

수치 평가: 질량 및 디리클레 행렬의 대각선 밖 항목에 대한 식은 Ω_ijk → 0으로 제거 가능한 특이점을 갖습니다. 수학적 어려움을 피하고 이러한 식을 효율적이고 신뢰할 수 있으며 정확하게 평가하기 위해 체비셰프 확장을 사용합니다. 체비셰프 다항식은 전체 구간에서 배정밀도 최하위 비트 단위의 오류를 보장하는 등가 속성을 가지고 있습니다. M_jk 및 _jk에 대한 식은 Ω_ijk ∈ (π,π]에 대해서만 평가되고 실수(각각 허수) 부분의 대칭(각각 반대칭)으로 인해 [0,π]의 인자에 대한 체비셰프 확장을 구하는 것으로 충분합니다. 확장은 Mathematica의 도움으로 계산되며 구현 코드는 보충 자료에 포함되어 있습니다.

### 6.1.2 Curvature Alignment

이 텍스트에서 정렬 에너지 Es,t는 곡률선(n = 2)과 십자선(n = 4)의 평활화된 버전을 계산하는 데 사용됩니다. 정렬 필드는 모양 연산자의 트레이스가 없는 부분인 호프 미분의 조각별 선형 근사치입니다. 이산 설정에서는 가장자리를 따라 집중된 분포로만 액세스할 수 있습니다.

정렬 필드의 계수는 기준 섹션과 쌍을 이루고 삼각형을 통해 통합하여 계산됩니다. 이 계수는 주 곡률의 제곱 차이에 비례하므로, 주 방향이 잘못 정의된 배꼽 영역에서는 정렬보다 평활도가 선호됩니다.

삽입된 이미지는 불규칙한 테셀레이션으로 인해 정렬 필드에 노이즈가 발생할 수 있지만 평활화 항이 여전히 좋은 결과를 생성하는 방법을 보여줍니다. 텍스트에는 정렬 필드를 사용하여 최소 곡률 방향에 정렬하는 방법도 언급되어 있으며, 그림에는 스퓨리어스 특이점을 제어하는 평활도와 정렬 간의 트레이드 오프의 예가 나와 있습니다.

![Untitled](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%209.png)

### 6.1.3 Index Computation

이 단원에서는 n방향 필드에서 각 삼각형에 특이점(지수 ±1)의 존재를 나타내는 정수 인덱스를 사용하여 레이블을 지정하는 방법을 설명합니다. 각 꼭지점에 대해 규범이 1인 복소수로 표현되는 n 방향 필드가 주어지면 각 가장자리에 대해 회전 각도가 계산됩니다. 이 각도는 인접한 정점 간의 관계를 정의하는 데 도움이 되는 고유 번호입니다.

그런 다음 각 삼각형의 인덱스는 삼각형의 회전각과 홀로노미 값(⌦ijk)을 사용하여 계산됩니다. 이 인덱스는 -1, 0 또는 1의 값을 취할 수 있습니다. 이산 푸앵카레-호프 정리는 n 방향 필드에서 특이점의 동작을 이해하는 데 사용됩니다.

![4방향 필드가 (= 0)으로 정렬된 토끼는 s = 1을 사용하여 (qi^2 )와 정렬됩니다. s = 1을 사용하여 (qi^2)에 정렬된 두 구멍이 있는 원환입니다. 개구리는 가장 매끄러운 (s = 1) 4방향 필드를 보여줍니다. 코끼리는 s = 1을 사용하고 최소 곡률 방향 q와 정렬 ( = 0)을 사용합니다.](Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488/Untitled%2010.png)

4방향 필드가 (= 0)으로 정렬된 토끼는 s = 1을 사용하여 (qi^2 )와 정렬됩니다. s = 1을 사용하여 (qi^2)에 정렬된 두 구멍이 있는 원환입니다. 개구리는 가장 매끄러운 (s = 1) 4방향 필드를 보여줍니다. 코끼리는 s = 1을 사용하고 최소 곡률 방향 q와 정렬 ( = 0)을 사용합니다.

제공된 이미지에서 인덱스가 +1인 특이점은 빨간색 구체로 표시되고 인덱스가 -1인 특이점은 파란색 구체로 표시됩니다.

### 7 Evaluation

이 논문의 저자들은 자신들의 알고리즘을 Bommes 등[2009]의 최신 혼합 정수 방법과 비교했습니다. 두 알고리즘 모두 일반적인 C++ 프레임워크에서 구현되었으며, 혼합 정수 문제에는 CoMISo [Bommes 외. 2012]를, 제안된 방법에 필요한 선형 시스템에는 CHOLMOD [Chen 외. 2009]를 사용했습니다. A가 계급이 부족한 경우의 인수 분해 문제를 피하기 위해 인수 분해 전에 M의 작은 배수(ε = 10^(-8))를 A에 추가했으며, 이러한 이동은 고유 벡터에 영향을 미치지 않습니다. 모든 타이밍은 동일한 2.4GHz 인텔 코어 2 듀오 컴퓨터에서 측정되었습니다.

고유벡터 문제(방정식 (15))의 경우, 저자들은 A를 인수분해하고 고정된 횟수(논문의 모든 예제에서 20회)의 파워 반복을 적용했습니다. 가장 작은 고유값만 필요하기 때문에 ARPACK과 같은 정교한 고유해석기가 필요하지 않으므로 이 방법을 더 쉽게 구현할 수 있습니다. 정렬 에너지를 최소화하기 위해 단일 인수분해와 역치환을 수행했습니다. 35만 개 이상의 삼각형이 있는 메시에서 제안한 방법은 평활화에 11초, 정렬에 7초가 소요된 반면, 혼합 정수 방법은 평활화와 정렬에 모두 3분 이상이 소요되었습니다. 전반적으로 제안된 방법은 평균 약 19배의 속도 향상을 보였습니다.

결과의 품질을 평가하기 위해 저자들은 방정식 (1)에 설명된 에너지를 사용하여 두 가지 방법으로 생성된 필드를 비교했습니다. 제안한 방법은 이 에너지를 명시적으로 최소화하지 않아도 더 부드러운 필드를 생성했습니다. 곡률 정렬 필드의 경우, Bommes 등의 방법에는 많은 매개변수가 포함되어 있기 때문에 에너지의 직접적인 비교는 시도하지 않았습니다. 대신 저자들은 특이점(S)의 수와 시간(t)을 보고했습니다.

그림 14, 15, 16은 제안된 방법이 에너지 최소화, 특이점 배치, 견고성 측면에서 혼합 정수 방법보다 우수한 성능을 보이는 다양한 예를 보여줍니다. 제안한 방법은 더 복잡한 형상에서도 유사한 특이점 구성을 달성할 수 있었으며, 훨씬 짧은 시간 내에 혼합정수 방법과 비교하여 효율성과 효과성을 입증했습니다.

### 8 Conclusion

저자들은 효율적인 수치 알고리즘으로 이어지는 표면의 n 방향 필드에 대한 원칙적인 공식을 제시했습니다. 주요 초점은 자동 특이점 배치에 맞춰져 있지만, 이 공식은 특히 기존 연구와의 잘 정립된 연관성을 고려할 때 다양한 응용 분야에 적용할 수 있는 유용한 이론을 제공합니다.

이러한 연관성은 향후 연구를 위한 몇 가지 흥미로운 길을 열어줍니다:

- 수동 필드 편집: 일부 애플리케이션에서는 심미성과 같은 다른 기준을 충족하기 위해 수동 필드 편집이 바람직할 수 있습니다. 이는 Crane 등[2010]의 사소한 연결 알고리즘의 이중화된 버전을 사용하여 제안된 프레임워크에서 달성할 수 있습니다.
- 표면 매개변수화: 홀로모픽 및/또는 반홀로모픽 n-벡터 필드를 표면 매개변수화의 기초로 사용하여 쿼드 메시로 직접 경로를 생성할 수 있습니다(예: [Kälberer et al. 2007]).
- 다른 기준 통합: 필드의 크기에 대한 다른 가중치 체계를 사용하면 정렬 용어 El을 통해 다른 흥미로운 기준을 통합할 수 있습니다.

이러한 미래 방향을 탐구함으로써 저자의 공식은 더 광범위한 응용 프로그램과 시나리오에 확장 및 적용될 수 있으며, 컴퓨터 그래픽 및 지오메트리 처리 분야의 다양한 사용 사례에 더욱 다양하고 강력하게 적용될 수 있습니다.

- 샘플 프로젝트
    
    [http://www.cs.cmu.edu/~kmcrane/](http://www.cs.cmu.edu/~kmcrane/)
    
    [http://www.cs.cmu.edu/~kmcrane/Projects/StripePatterns/code.zip](http://www.cs.cmu.edu/~kmcrane/Projects/StripePatterns/code.zip) stripe
    
    [https://github.com/GeometryCollective/fieldgen](https://github.com/GeometryCollective/fieldgen)
    
    - const Mesh& Mesh :: operator=( const Mesh& mesh )
    메시 클래스에 대한 복사 할당 연산자의 구현입니다. 복사 할당 연산자는 기존 메시 개체의 내용을 다른 메시 개체에 복사하는 데 사용됩니다.
        
        이 함수는 하프 에지, 버텍스, 에지, 페이스에 대해 각각 하나씩 4개의 맵을 생성하여 원본 메시의 포인터를 새 메시의 해당 포인터에 매핑합니다. 그런 다음 원본 메시에서 새 메시로 지오메트리를 복사하고 앞서 생성한 맵을 사용하여 새 메시의 포인터를 업데이트합니다.
        
        마지막으로 새 메시 객체를 반환합니다.
        
        맵은 커스텀 비교 함수(HalfEdgeCIterCompare, VertexCIterCompare, EdgeCIterCompare, FaceCIterCompare)를 사용하여 맵의 포인터를 비교한다는 점에 유의하세요. 이는 맵의 포인터를 메모리 주소가 아닌 포인터가 가리키는 객체를 기준으로 비교해야 하기 때문에 필요합니다.
        
    - void Mesh::normalize( void )
        
        이 코드는 메시 오브젝트에 대한 정규화() 함수를 정의합니다. 이 함수의 목적은 메시가 원점의 중심에 있고 단위 공 안에 포함되도록 메시를 변환하고 크기를 조정하는 것입니다.
        
        이 함수는 먼저 모든 정점의 위치를 합산하고 총 정점 수로 나누어 메시의 질량 중심을 계산합니다. 그런 다음 각 정점의 위치에서 질량 중심을 빼서 메시를 원점으로 변환합니다.
        
        그런 다음 원점으로부터 모든 정점의 최대 거리(경계 구의 반지름)를 계산하고 각 정점 위치를 이 값으로 나누어 단위 공 안에 포함되도록 메시의 스케일을 다시 조정합니다.
        
        이 함수는 메시 내 각 버텍스의 위치 속성을 수정합니다.
        
    - void Mesh :: initializeSmoothStructure( void )
        
        이 방법은 각 버텍스에서 나가는 각 하프 에지의 각도 좌표를 계산하여 메시의 부드러운 구조를 초기화합니다. 이 단계의 목적은 각도 좌표를 사용하여 이산 라플라시안 연산자를 계산하는 후속 평활화 작업을 위해 메시를 준비하는 것입니다.
        이 방법은 메시의 각 버텍스를 반복하고 초기 하프 에지를 기준으로 각 나가는 하프 에지에서 누적 각도를 계산합니다. 이는 버텍스의 원링 이웃에 있는 각 하프 에지를 반복하고 그 각도를 누적 각도에 더함으로써 이루어집니다.
        각 하프 에지에 대한 누적 각도가 계산되면 각도 좌표가 2π가 되도록 정규화됩니다. 이 단계는 이산 라플라스 연산자가 각도 좌표의 차이로 정의되므로 적절하게 정규화되도록 합니다.
        경계 정점은 나가는 반쪽 모서리가 하나뿐이므로 정규화할 필요가 없으므로 정규화 단계는 내부 정점에 대해서만 수행됩니다.
        
    - int Mesh :: eulerCharacteristic( void ) const
        
        오일러 특성은 다면체 표면의 정점, 가장자리, 면의 수 사이의 관계를 설명하는 위상 불변수입니다. 공식은 다음과 같습니다:
        
        χ = V - E + F
        
        여기서 χ는 오일러 특성, V는 정점 수, E는 에지 수, F는 면 수입니다.
        구와 같이 닫힌 표면의 경우 오일러 특성은 항상 2이고, 평면과 같이 열린 표면의 경우 오일러 특성은 0입니다. 오일러 특성은 표면을 찢거나 붙이지 않고 늘이거나 구부리는 것과 같은 특정 위상 변환에서도 변하지 않습니다.
        코드의 Mesh::eulerCharacteristic() 메서드는 정점, 에지, 면의 수를 세고 위의 공식을 적용하여 메시의 오일러 특성을 계산합니다.
        
    - void Mesh :: buildMassMatrix( void )
        
        이 메서드는 메시의 질량 행렬을 빌드합니다. 질량 행렬은 대각선 항목이 정점의 이중 영역인 대각선 행렬입니다. 정점의 이중 면적은 정점에 인접한 삼각형 면적의 1/3을 합한 값입니다.
        
        이 방법은 먼저 메시의 정점 수에 해당하는 올바른 크기로 질량 행렬을 초기화합니다. 그런 다음 메시의 모든 정점을 반복하여 각 정점에 대한 이중 면적을 계산합니다. 이중 면적은 정점에 입사하는 각 삼각형 면적의 1/3을 합산하여 얻습니다. 그런 다음 정점에 해당하는 질량 행렬의 대각선 항목이 이중 영역으로 설정됩니다.
        
        이 방법은 또한 블록이 2x2 행렬이고 대각선 항목이 이중 영역인 블록 대각선 행렬인 실제 질량 행렬을 구축합니다. 실제 질량 행렬은 복소 행렬이 필요한 고유값 계산에 사용되며, 여기서 실제 질량 행렬은 해당 헤르미티안 행렬을 구축하는 데 사용됩니다.
        
        전반적으로 질량 행렬은 라플라스-벨트라미 연산자에 가중치를 부여하는 데 사용되며, 이 가중치는 고유 벡터와 고유값 계산에 사용됩니다.
        
    - void Mesh :: buildFieldEnergy( void )
        
        이 코드는 에너지 함수의 파라미터인 필드 차수 k를 사용하여 메시의 면당 에너지 행렬 A를 빌드합니다. 먼저 A의 크기를 메시의 정점 수에 맞게 조정합니다. 그런 다음 메시의 각 비경계 면에 대해 반쪽 가장자리를 반복하고 반쪽 가장자리의 코탄젠트 가중치와 각도 좌표를 사용하여 A의 해당 항목을 계산합니다. 메시의 이산 라플라스-벨트라미 연산자에 대한 공식을 사용하여 이러한 항목을 계산합니다. 그에 따라 A의 대각선 및 대각선을 벗어난 항목을 업데이트합니다.
        
        마지막으로, 이 코드는 행렬을 작은 상수만큼 이동시켜 콜레스키 솔버에 대해 엄격하게 양의 정의가 되도록 합니다. 이는 일부 도메인에서 평평한 디스크와 같은 사소한 구간이 허용되어 행렬이 특이하게 될 수 있기 때문입니다. 시프트는 행렬의 고유 벡터를 변경하지 않으며, 결국에는 사용되지 않는 고유값만 변경합니다.
        
    - void Mesh :: buildDualLaplacian( void )
        
        이 코드는 메시의 이중 라플라시안 행렬 L을 구축합니다. 이중 라플라시안 행렬은 메시에서 면의 연결성에 대한 정보를 인코딩하는 항목이 있는 행렬로, 메시의 조화 기준을 계산하는 데 사용됩니다.
        
        이 코드는 먼저 행렬 L을 메시의 면 수와 같은 크기의 스파스 행렬로 초기화합니다. 그런 다음 메시의 각 면을 반복하고 각 면 f에 대해 인접한 면의 연결 정보를 사용하여 L의 해당 항목을 계산합니다. 구체적으로는 f의 반쪽 가장자리를 반복하고 인접한 각 면에 대해 가중치 wij를 계산합니다. 그런 다음 L의 대각선 및 대각선을 벗어난 항목을 적절히 업데이트합니다.
        
        마지막으로, 이 코드는 행렬을 작은 상수만큼 이동시켜 콜레스키 솔버에 대해 엄격하게 양의 정의가 되도록 합니다. 이는 일부 도메인에서 사소한 고조파 기저를 허용하여 행렬을 특이 행렬로 만들 수 있기 때문입니다. 시프트는 행렬의 고유 벡터를 변경하지 않으며, 고조파 기저를 계산하는 데 사용되는 고유값만 변경합니다.
        
    - void Mesh :: computeTrivialSection( void )
        
        이 코드는 나중에 방향 필드의 정렬에 사용되는 메시의 사소한 섹션을 계산하는 메시 클래스의 메서드의 일부입니다. 이 메서드는 먼저 이전 섹션을 각 버텍스의 oldDirectionField 어트리뷰트에 저장합니다. 그런 다음 buildDualLaplacian() 메서드를 사용하여 이중 메시에서 스칼라 포텐셜을 구성하고 연결 1-폼을 추출합니다. solvePositiveDefinite() 메서드는 이중 라플라시안과 연결 1-폼을 포함하는 선형 시스템을 푸는 데 사용됩니다.
        
        그런 다음 이 코드는 첫 번째 정점의 directionField 속성을 각각 실수 부분과 허수 부분으로 cos(fieldOffsetAngle) 및 sin(fieldOffsetAngle)을 갖는 복소수로 설정하여 평행 섹션을 구성합니다. 그런 다음 메시에서 넓이 우선 검색을 수행하여 각 정점의 directionField 속성을 이웃의 directionField 속성, 가장자리 사이의 각도, 가장자리를 따라 연결되는 1-폼의 차이에 따라 새로운 복소수로 업데이트합니다. 마지막으로 각 정점의 directionField 속성을 제곱합니다.
        
        전체적으로 이 방법은 이중 라플라시안과 연결 1-폼을 포함하는 선형 시스템을 풀어서 메시에서 사소한 섹션을 계산하고, 이웃의 속성과 연결 1-폼을 기반으로 각 정점의 directionField 속성을 업데이트하여 평행 섹션을 구성합니다.
        
    - void Mesh :: alignTrivialSection( void )
        
        이 코드 조각은 트라이앵글 메시의 전역 컨포멀 파라미터화를 계산하는 메서드 구현의 일부입니다. 이 방법은 이산 컨포멀 매핑 이론을 기반으로 하며, 이산 컨포멀 매핑은 복소 분석에서 이산 표면 설정에 이르기까지 컨포멀 맵의 개념을 이산화하고자 하는 이론입니다.
        
        이 코드는 특히 표면에서 복잡한 방향의 병렬 이동을 정의하는 데 사용되는 메시의 사소한 연결의 계산 및 정렬을 다룹니다. 삼항 연결은 메시 위에 복소 선 다발의 전역적인 매끄러운 섹션을 유도하는 고유한 평면 연결로 정의되며, 섹션의 특이점은 표면 곡률의 특이점에 의해 규정됩니다.
        
        computeTrivialSection() 함수는 메시의 이중 라플라시안 행렬을 포함하는 선형 시스템을 풀어서 삼항 연결을 계산하고 복소선 다발의 평행 단면을 구성합니다. 그런 다음 alignTrivialSection() 함수는 계산된 평행 섹션을 각 접하는 공간에서 일정한 회전까지 전 세계적으로 가장 매끄러운 섹션으로 선택된 참조 섹션과 정렬합니다.
        
        이 코드는 스파스 및 고밀도 행렬, 복소수, 넓이 우선 검색을 위한 큐와 같은 여러 도우미 함수와 데이터 구조를 사용합니다. 전체 알고리즘은 표면의 컨포멀 구조를 인코딩하기 위해 호프 미분 개념을 사용하는 홀로모픽 1-폼 방법을 기반으로 합니다.
        
    - void Mesh :: computeSmoothestSection( void )
        
        이 메서드는 메시에서 전역적으로 가장 매끄러운 방향 필드를 계산합니다. 먼저 buildFieldEnergy() 메서드를 호출하여 에너지 함수의 행렬 표현을 구성하고, 에너지 행렬의 기저 상태(즉, 가장 작은 고유값에 해당하는 고유 벡터)를 계산하는 데 사용됩니다. 결과 고유 벡터는 메시의 각 버텍스에 방향 필드로 저장됩니다.
        
        방향 필드는 메시의 각 지점에서 선호하는 방향을 나타내며, 메시의 모양을 따르는 패턴을 디자인하거나 컴퓨터 그래픽의 이방성 필터링과 같은 다양한 용도로 사용할 수 있습니다.
        
    - void Mesh :: computeCurvatureAlignedSection( void )
        
        이 코드에서 computeCurvatureAlignedSection 메서드는 메시의 주요 곡률 방향과 정렬된 방향 필드를 계산합니다.
        
        먼저, 이 메서드는 buildFieldEnergy를 사용하여 에너지 행렬과 질량 행렬을 구축합니다.
        
        다음으로, 단위 길이를 갖도록 정규화된 각 버텍스에서 주 방향을 유지하도록 복소수 값 행렬인 principalField를 초기화합니다. 주 방향은 각 버텍스에서 principalDirection 메서드를 사용하여 계산되며, 이 메서드는 해당 버텍스에서 최대 곡률 방향을 나타내는 3D 공간의 벡터를 반환합니다.
        
        그런 다음 이 메서드는 에너지 행렬과 질량 행렬에 곱한 복소 스칼라 t의 합으로 스파스 행렬 A를 설정합니다. 스칼라 t는 결과 방향 필드의 부드러움을 제어하기 위해 조정할 수 있는 파라미터입니다.
        
        마지막으로, 이 방법은 solvePositiveDefinite 메서드를 사용하여 x는 미지의 평활화된 방향 필드이고 b는 초기 주 필드인 양의 정의 선형 시스템 A x = b를 풉니다. 그런 다음 결과 평활화된 필드는 각 정점에서 단위 길이를 갖도록 정규화되고 각 정점의 directionField 멤버 변수에 할당됩니다.
        
    - void Mesh :: parameterize( void )
        
        이 코드는 메시 데이터 구조의 파라미터화 함수를 구현하고 있습니다. 이 함수는 메시의 각 정점을 복소수에 매핑하는 하나 또는 두 개의 스칼라 함수를 사용하여 메시의 1D 또는 2D 파라미터화를 계산하는 역할을 합니다. 이 파라미터화는 텍스처 매핑, 모양 분석, 표면 재구성 등 다양한 용도로 사용할 수 있습니다.
        
        이 함수는 인수가 0인 computeParameterization 함수를 호출하여 첫 번째 좌표 함수에 대한 파라미터화를 계산하는 것으로 시작합니다. 그런 다음 사용자가 두 번째 좌표 함수(선택 사항)를 요청한 경우 1 벡터 필드를 90도 회전하여(즉, 각 벡터에 -1을 곱하여) 직교 방향 필드를 얻습니다. 그런 다음 인수를 1로 하여 computeParameterization을 호출하여 두 번째 좌표 함수에 대한 매개변수화를 계산합니다. 마지막으로 방향 필드를 원래 방향으로 다시 회전합니다. 유효한 좌표 함수의 개수는 nComputedCoordinateFunctions 변수에 저장됩니다.
        
    - void Mesh :: buildDirichletEnergy( SparseMatrix<Real>& A )
        
        이 코드의 buildDirichletEnergy 메서드는 메시에서 2벡터 필드의 디리클레 에너지를 계산합니다. 디리클레 에너지는 함수 또는 필드의 평활성을 측정하는 척도로, 도메인의 필드 또는 함수와 관련된 최적화 문제에서 자주 사용됩니다. 이 경우 2벡터 필드는 메시의 정점에 정의되고 디리클레 에너지는 메시의 각 에지 끝점에 있는 필드 벡터 간의 각도 차이로 정의됩니다.
        
        이 방법은 먼저 치수가 2*nV x 2*nV인 스파스 행렬 A를 초기화합니다. 여기서 nV는 메시의 정점 수입니다. 그런 다음 메시의 가장자리를 반복하여 행렬을 채웁니다. 각 에지에 대해 이 방법은 엔드포인트 정점의 정식 기저에 대한 에지의 각도를 계산하고 한 엔드포인트에서 다른 엔드포인트로의 병렬 전송 계수를 계산합니다. 그런 다음 에지의 코탄젠트 가중치(에지에 접하는 두 삼각형에서 에지 반대쪽 각도의 코탄젠트에 따라 달라짐)를 계산하고 각 엔드포인트에서 2-벡터 필드의 임의의 근을 선택합니다.
        
        두 근이 같은 방향을 가리키면 복소 곱셈을 나타내는 2x2 블록을 행렬에 추가합니다. 두 근이 서로 반대 방향을 가리키면 복소수 접합과 곱셈을 모두 나타내는 2x2 블록을 추가합니다. 두 경우 모두 블록에 에지의 코탄젠트 가중치가 곱해지고 에지의 끝점에 해당하는 행렬의 대각선 항이 코탄젠트 가중치만큼 증가합니다.
        
        행렬을 채운 후, 이 방법은 행렬을 작은 상수만큼 이동시켜 행렬을 양의 확실성으로 만드는데, 이는 행렬을 포함하는 선형 시스템을 푸는 데 필요합니다. 결과 행렬은 메시에서 2벡터 필드의 디리클레 에너지를 나타냅니다.
        
    - void Mesh :: buildEnergy( SparseMatrix<Real>& A, int coordinate )
        
        이 코드는 표면 메시에서 벡터 필드의 호지 분해에 대한 에너지 행렬 계산을 구현하고 있습니다.
        
        에너지 행렬은 직교 고유 벡터 세트와 그에 해당하는 고유값을 계산하는 데 사용됩니다. 그런 다음 고유 벡터를 사용하여 벡터 필드를 컬이 없는 구성 요소와 발산이 없는 구성 요소로 분해합니다.
        
        빌드필드에너지 함수는 컬이 없는 성분에 해당하는 에너지 행렬을 구축하고, 빌드듀얼라플라시안 함수는 발산이 없는 성분에 해당하는 에너지 행렬을 구축합니다.
        
        computeTrivialSection 함수는 서피스 메시에서 트리비얼 연결을 계산하고 트리비얼 연결을 사용하여 평행 섹션을 구성합니다. 이 평행 단면은 호지 분해에 대한 벡터 필드를 초기화하는 데 사용됩니다.
        
        alignTrivialSection 함수는 각 접하는 공간에서 일정한 회전까지 평행 섹션을 전역적으로 가장 매끄러운 섹션에 정렬합니다. 이 정렬은 호지 분해가 가능한 한 매끄러운 벡터 필드를 생성하도록 보장합니다.
        
        마지막으로 buildDirichletEnergy 및 buildEnergy 함수는 각각 디리클레 문제와 노이만 문제에 대한 에너지 행렬을 구성합니다. 이 함수는 서피스 메시에서 고조파 및 벡터 라플라스 연산자를 계산하는 데 사용됩니다.
        
    - void Mesh :: computeParameterization( int coordinate )
        
        이 코드는 메시의 3D 지오메트리를 평면이나 구와 같은 2D 도메인에 매핑하는 방법인 메시의 파라미터화를 계산하는 방법을 구현한 것으로 보입니다. 특히 '컨포멀 맵'으로 알려진 보다 일반적인 파라미터화 클래스의 특수한 경우인 '스트라이프 패턴' 파라미터화를 계산하는 것으로 보입니다. 매개변수화는 특정 경계 조건에 따라 에지 길이와 인접한 에지 사이의 각도에 따라 달라지는 에너지 함수를 최소화하여 계산됩니다. 해는 에너지 최소화 문제에서 발생하는 선형 시스템을 나타내는 희소 양정확 행렬의 고유 벡터를 계산하여 얻습니다. 결과 고유 벡터는 복소수로 표시되는 메시의 각 정점의 파라미터화를 계산하는 데 사용됩니다. 마지막으로 파라미터화에 따라 각 정점에 텍스처 좌표가 할당됩니다.
        
    - double Mesh :: energy( const SparseMatrix<Real>& A, const DenseMatrix<Real>& x, double eps )
        
        이 메서드는 희소 행렬 A와 고밀도 행렬 x, 그리고 매개 변수 eps가 주어지면 시스템의 에너지를 계산하는 Mesh 클래스의 메서드입니다.
        
        이 메서드는 먼저 A와 x를 곱한 다음 결과의 내적 곱을 x로 취하여 에너지의 이차 부분을 계산합니다. 이는 미지수의 이차 형태인 수송 계수의 에너지에 해당합니다.
        
        그런 다음 메시의 모든 정점을 반복하고 각 정점에 대한 매개변수화의 제곱을 계산하여 에너지의 이차 부분을 계산합니다. 정점이 메시의 마지막 정점이 아닌 경우(즉, 경계 정점이 아닌 경우) 매개변수화의 제곱과 1 사이의 차이의 제곱에 비례하는 에너지에 대한 기여도를 계산하며, 비례 상수는 eps/4로 합니다. 이 항은 매개변수화가 단위 길이를 유지하도록 하기 위한 페널티 항입니다.
        
        마지막으로 이 메서드는 에너지의 이차 부분과 이차 부분의 합을 반환합니다.
        
    - void Mesh :: assignTextureCoordinates( int p )
        
        이 코드는 스트라이프 패턴을 사용하여 메시의 파라미터화를 계산하는 메서드를 구현하고 있습니다. 매개변수화는 메시 표면의 복소수 값 함수로 정의됩니다. 이 함수는 특정 평활도 조건을 만족해야 하며, 이는 고유값 문제를 해결하여 최소화되는 에너지 함수로 인코딩됩니다.
        
        Mesh 클래스에는 고유값 문제를 빌드하고 해결하는 데 사용되는 여러 메서드가 포함되어 있습니다. buildEnergy 메서드는 에너지 함수의 이차 항과 이차 항을 나타내는 스파스 행렬 A를 구축합니다. 가장 작은EigPositiveDefinite 메서드는 고유값 문제를 해결하여 에너지 함수의 기저 상태를 얻습니다. computeParameterization 메서드는 고유값 문제의 해를 메시의 정점에 할당하여 최종 텍스처 좌표를 계산합니다. 마지막으로 assignTextureCoordinates 메서드는 메시의 각 트라이앵글 주변의 복잡한 이동 계수와 각도를 계산하여 텍스처 좌표를 구성합니다.
        
        에너지 메서드는 고유값 문제에 대한 주어진 해에 대한 에너지 함수의 값을 계산합니다.
        
    - void Mesh :: glueParameterization( void )
        
        이 코드는 스트라이프 패턴을 사용하여 메시의 파라미터화를 구현합니다. 매개변수화는 computeParameterization 메서드에서 계산되는데, 이 메서드에서는 먼저 buildEnergy를 사용하여 에너지 행렬 A를 구축한 다음, smallestEigPositiveDefinite를 사용하여 시스템의 접지 상태를 계산하여 메시 정점의 매개변수화를 얻습니다.
        
        assignTextureCoordinates 메서드는 계산된 파라미터화를 기반으로 메시의 트라이앵글에 대한 최종 텍스처 좌표를 계산합니다. 이 메서드는 가장자리를 따라 이동 계수를 사용하여 파라미터화 영역에서 각 트라이앵글 모서리의 각도를 결정합니다.
        
        glueParameterization 메서드는 선택 사항이며 매개변수화 도메인의 삼각형을 서로 접착하여 연속적인 매개변수화를 형성하는 데 사용할 수 있습니다.
        
        이 구현에서는 복소수를 광범위하게 사용하여 매개변수화와 가장자리를 따라 이동 계수를 표현합니다. 매개변수화는 크기가 1과 같은 복소수로 표현되고, 수송 계수는 크기가 1과 같고 인수가 회전 각도와 같은 복소수로 표현됩니다. 전송 계수는 가장자리를 따라 평행 전송을 사용하여 계산되며, 가장자리의 끝점에서 표준 벡터 사이의 각도에 따라 결정됩니다.
        
    - void Mesh :: extractSingularities( void )
        
        이 메서드는 메시에서 특이점을 추출하는데, 이는 필드에 fieldDegree 차수의 특이점이 있는 위치입니다. 이 방법은 각 버텍스 주변의 필드 권선 수를 계산하여 해당 버텍스의 특이점 인덱스와 같은 값을 구합니다. 이는 정점 주변의 인접한 면 사이의 필드 인덱스 차이를 필드 차수로 나눈 값을 합산하는 방식으로 수행됩니다. 그런 다음 권선 수를 2 * 파이로 나누어 특이 인덱스를 계산합니다. 결과는 각 면의 singularIndex 멤버 변수에 저장됩니다.
        
        특이점은 메시의 텍스처 파라미터화에서 "핀칭 포인트"로 시각화할 수 있으며, 이 점을 중심으로 메시가 방사형 방식으로 늘어나거나 압축됩니다. 특이점은 텍스처 매핑 시 메시의 시각적 모양에 영향을 미치고 흥미로운 시각 효과를 만드는 데 사용할 수 있기 때문에 중요합니다. 예를 들어 특이점이 정사각형의 모서리에 있는 경우 메시의 스트라이프 패턴이 모서리에 가까워질수록 비틀어지고 변형되는 것처럼 보입니다.
        
        fieldDegree 멤버 변수는 추출할 특이점의 순서를 지정하는 데 사용됩니다. 예를 들어 fieldDegree를 2로 설정하면 이 메서드는 필드에 제곱근 특이점이 있는 분기점인 차수 2의 특이점을 추출합니다.
        
- 영상정리
    
    이 중 첫 번째 문제인 곡면의 방향 필드를 계산하는 문제를 해결해 보겠습니다. 처음에는 복잡한 조합 최적화 작업으로 여겨졌던 이 문제는 수년에 걸쳐 전역 최소값을 쉽게 찾을 수 있는 간단한 이차 에너지 최소화 문제로 발전했습니다.
    
    서페이스에 방향 필드가 왜 필요한지 궁금하실 것입니다. 최근에는 사변형 리메싱을 위해 관심을 끌고 있지만, 다른 용도로도 사용할 수 있습니다. 동물의 털을 관리하고, 비사실적인 렌더링을 수행하고, 이방성 셰이딩을 제어하는 등의 용도로 사용할 수 있습니다. 오늘은 특정 애플리케이션에 초점을 맞추지 않겠습니다. 대신 이러한 모든 애플리케이션이 활용할 수 있는 수학적 및 계산적 기반을 구축하는 데 집중하겠습니다.
    
    토끼와 같은 표면이 있고 적절한 방향 필드를 자동으로 찾고 싶다고 가정해 보겠습니다. 이 필드는 크로스 필드, 라인 필드 또는 기존의 벡터 필드일 수 있습니다. 하지만 어떤 경우든 목표는 최적의 솔루션을 찾는 것입니다. 최적이란 무엇을 의미할까요? 이는 애플리케이션에 따라 크게 달라집니다. 예를 들어, 동물의 털을 디자인하는 경우 필드에서 특이점(또는 카우클릭)이 어디에 있어야 하는지 이미 알고 있고 다른 곳에서 가장 매끄러운 솔루션을 원할 수 있습니다. 몇 년 전, 우리는 매우 효율적인 단일 선형 시스템을 풀면 이를 달성할 수 있다는 것을 보여주었습니다.
    
    이보다 더 어려운 문제는 알고리즘이 얼마나 많은 특이점이 존재해야 하는지, 어디에 배치해야 하는지, 그 유형은 무엇인지 결정하는 이 필드를 자동으로 생성하는 것입니다. 이것은 어려운 조합 문제처럼 들리지만, 단일 행렬 인수분해로 귀결되므로 빠르고 효율적으로 처리할 수 있습니다.
    
    일반적으로 에너지 최소화 방법을 사용하여 표면에서 필드를 계산하는 방법을 공식화하는데, 여기서 에너지 메트릭 E를 사용하여 필드 ψ의 평활도를 측정합니다. 이 메트릭(종종 디리클레 에너지)은 가장자리의 차이를 제곱하고 이를 전체 표면에 걸쳐 합산합니다. 그러나 이 접근 방식에는 문제가 있습니다. 대부분의 애플리케이션에서는 벡터의 크기보다는 벡터의 방향에 더 신경을 쓰므로 처리해야 할 단위 규범 제약 조건이 생깁니다. 또한, 특히 90도 회전을 무시하려는 크로스 필드의 경우 회전을 고려해야 합니다.
    
    시간이 지남에 따라 이 문제를 해결하기 위해 다양한 에너지 지표가 제안되었지만 최소화하기는 쉽지 않습니다. 초기 접근 방식에서는 코사인과 같은 삼각 함수를 사용하여 필드의 각도를 계산했습니다. 하지만 코사인은 수많은 국부 최소값을 가지므로 매우 비볼록한 문제가 발생합니다. 다른 방법은 n에 대해 2파이를 모듈로 하는 정수를 사용했지만, 이 경우 문제가 복잡한 혼합 정수 문제로 바뀌었습니다. 또 다른 접근 방식은 각도로 직접 작업하는 대신 표현 벡터를 사용하는 것이었지만, 각 벡터가 단위 규범을 가져야 한다는 또 다른 비볼록 제약 조건이 생겼습니다.
    
    이러한 공식의 비볼록한 특성은 최적의 해를 찾는다는 보장이 없다는 것을 의미합니다. 또한 근사 해를 찾는 데 사용되는 방법은 속도가 느릴 수 있으며 때로는 복잡한 혼합 정수 솔버가 필요할 수 있습니다. 종종 간과되는 또 다른 문제는 최소화하려는 디리클레 에너지가 단위 방향 필드에 대한 의미 있는 척도를 제공하지 못한다는 것입니다. 예를 들어, 특이점에 가까워질수록 디리클레 에너지는 무한대에 가까워지는 경향이 있어 표면에서 가장 매끄러운 단위 벡터 필드를 식별하는 데 도움이 되지 않습니다. 이는 추상적인 수학적 문제처럼 보일 수 있지만, 실제적인 문제를 야기할 수도 있습니다.
    
    이러한 문제를 해결하기 위해 특이점이 있는 솔루션에 대해서도 잘 정의되어 있으며, 중요한 것은 모두 볼록하고 이차적인 새로운 에너지 메트릭을 도입했습니다. 이는 전 세계적으로 최적의 솔루션을 쉽게 찾을 수 있음을 의미합니다. 또한 이러한 메트릭은 표면의 기하학적 구조에 민감하여 토끼 귀 끝과 같은 특징을 포착하고 선택적으로 주요 곡률에 맞춰 정렬할 수 있습니다.
    
    실제로 이것은 하나의 스파스 행렬을 인수 분해하는 것으로 요약되며, 이 알고리즘은 최신 기술보다 약 20배 더 빠릅니다. 더 빠를 뿐만 아니라 생성되는 솔루션도 기존 방법보다 더 부드럽습니다.
    
    다음으로 서페이스에 n개의 벡터 필드를 표현하는 것을 고려합니다. 일반적으로 각 벡터는 표면의 접하는 평면에서 두 기저 벡터의 선형 조합으로 표현하며, 이를 실제 평면 R2의 복사본으로 간주합니다. 또는 이 평면을 복소수의 관점에서 볼 수 있는데, 각 벡터는 단일 복소수 계수에 단일 기저 벡터를 곱한 값입니다.
    
    벡터의 회전된 복사본을 식별하는 문제를 처리하기 위해 회전을 각도 파이의 i5에 대한 e로 나타내는 복소수의 속성을 활용합니다. 이러한 값 중 하나를 n의 거듭제곱으로 올리면 동일한 벡터 u를 얻을 수 있으며, 이를 표현 벡터라고 부릅니다.
    
    표면에서는 모든 점에서 기저 방향을 선택하고 이 기저를 기준으로 벡터 필드를 계수 함수로 표현합니다. 중요한 것은 표면의 다른 점 사이의 회전을 고려하기 위해 공통 측지법으로 기저가 이루는 각도를 측정해야 한다는 점입니다. 이러한 계수 z에서 표현 벡터 u로 변수를 변경하면 전체 도메인에 걸쳐 합산할 수 있는 이차 에너지 메트릭을 얻을 수 있습니다.
    
    단위 노름 제약 조건을 처리하기 위해 단위 벡터 필드의 에너지를 해당 필드의 가능한 모든 리스케일링 중에서 최소 디리클레 에너지로 재정의합니다. 이렇게 하면 벡터가 특이점에서 0이 되어 무한 에너지를 피할 수 있습니다. 또한 전체 솔루션이 단위 L2 노름을 갖는다는 조건을 부과합니다. 이는 고유값 문제 또는 각속도가 포텐셜로 사용되는 시간 독립적인 슈뢰딩거 방정식을 푸는 것으로 해석할 수 있습니다.
    
    전 세계적으로 최적의 단위 벡터 필드를 얻기 위해 가능한 모든 단위 방향 필드에 대해 이 에너지를 최소화합니다. 최소 고유값 문제 해결에는 단일 행렬 인수분해와 반복적인 역치환이 필요하기 때문에 결과 솔루션은 빠르고 효율적입니다. 각 지점에서 벡터를 점 단위로 정규화하는 것도 계산적으로 간단합니다. 예를 들어, 개구리에서 가장 매끄러운 크로스 필드를 계산하면 결과 필드가 표면의 복잡한 기하학적 구조와 세부 사항을 효과적으로 포착합니다.
    
    이 알고리즘은 눈과 발가락과 같은 논리적인 위치에 특이점이 나타나는 매우 부드러운 방향 필드를 제공합니다. 필드의 모양을 더 잘 제어하기 위해 이 디리클레 에너지를 홀로모픽 부분과 반홀로모픽 부분으로 분해할 수 있습니다. 그러면 평활도 에너지는 이 두 에너지 사이의 보간입니다. 실제로 이것은 특이점의 수와 필드 라인의 직진도 사이의 균형을 맞출 수 있음을 의미합니다.
    
    말과 관련된 더 어려운 문제를 생각해 봅시다. 가장 매끄러운 선 필드를 계산하면 귀 끝과 발바닥과 같은 논리적인 위치에 특이점이 나타납니다. 그러나 다리를 확대하면 필드 선이 모든 경우에 다리의 자연스러운 방향을 따르지 않는다는 것을 알 수 있습니다. 더 많은 제어를 위해 솔루션이 규정된 필드에 얼마나 잘 정렬되는지 측정하는 추가 정렬 항을 도입했습니다. 이렇게 하면 고유값 문제가 단일 스파스 선형 시스템으로 변경되어 빠르게 해결할 수 있습니다.
    
    정렬을 위한 자연스러운 선택 중 하나는 주 곡률의 방향입니다. 말 예제로 돌아가서 곡률 정렬 필드를 계산하면 이제 필드 선이 예상대로 다리를 따르는 것을 볼 수 있습니다. 또 다른 예는 스무딩과 정렬 사이의 상충 관계를 보여줍니다. 스무딩을 하지 않으면 노이즈가 발생하고, 지나치게 스무딩하면 기하학적 디테일이 손실됩니다. 그러나 중간에서 균형을 맞추면 바람직한 해결책을 찾을 수 있습니다.
    
    이 문제를 이산화하는 방식은 표면의 벡터 필드 작업에 유용한 새로운 유한 요소 기반을 생성합니다. 이 방법은 정점에서 기준 방향을 선택하고 모든 각도를 각도 합으로 정규화하는 것입니다. 이를 삼각형에 곡률을 균일하게 분배하는 것으로 상상할 수 있습니다. 이 개념을 사용하면 정점의 기저 벡터를 삼각형 내의 모든 점으로 옮길 수 있습니다. 그런 다음 적절한 내적 곱을 계산하여 질량 행렬과 강성 행렬을 구축할 수 있습니다. 결과 문제에는 양의 정적 행렬만 포함되며, 이러한 행렬의 희소성은 각 정점의 고리 하나만 포함합니다.
    
    기존의 혼합 정수 접근 방식과 비교했을 때, 우리의 솔루션은 대체로 유사하지만 때로는 더 자연스러운 위치에 특이점을 배치합니다. 계산적으로 우리의 방법은 약 20배 더 빠릅니다. 또한 열악한 메시, 노이즈, 평활화를 처리할 수 있을 만큼 강력하며, 특이점은 대부분 같은 위치에 위치합니다.
    
    결론적으로, 최적의 솔루션을 보장하는 강력한 자동 방향 필드 생성 방법을 도입했습니다. 이 방법은 최신 접근 방식보다 약 20배 빠르며 표준 수치 선형 대수학에만 의존합니다. 이론적으로는 특이점에서 우수한 성능을 발휘하고, 필드 직진도와 특이점 수 사이의 균형을 제공하며, 단일 파라미터를 사용하여 주요 곡률과 정렬할 수 있는 새로운 에너지 메트릭을 도입했습니다. 또한 향후 서페이스의 방향 필드 문제에 유용한 새로운 유한 요소 기반을 도입했습니다. 향후 출시될 코드에는 간소화된 시각화가 포함되며 최적의 솔루션을 수동으로 조정할 수 있습니다.