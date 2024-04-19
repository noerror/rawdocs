# Instant Field-Aligned Meshes

[https://igl.ethz.ch/projects/instant-meshes/instant-meshes-SA-2015-jakob-et-al.pdf](https://igl.ethz.ch/projects/instant-meshes/instant-meshes-SA-2015-jakob-et-al.pdf)

[https://github.com/wjakob/instant-meshes](https://github.com/wjakob/instant-meshes)

![1,300만 개의 정점을 가진 스캔한 드래곤을 특징 정렬 등방성 삼각형과 약 8만 개의 정점을 가진 쿼드 메시로 리메시징. 왼쪽에서 오른쪽으로, 두 경우 모두: 방향 필드, 위치 필드 및 출력 메시의 시각화(각각 71.1초 및 67.2초 만에 계산됨). 쿼드의 경우, 쿼드 도미넌트 메시를 1/4 해상도로 최적화하고 한 번 세분화하여 순수 쿼드 메시를 얻습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled.png)

1,300만 개의 정점을 가진 스캔한 드래곤을 특징 정렬 등방성 삼각형과 약 8만 개의 정점을 가진 쿼드 메시로 리메시징. 왼쪽에서 오른쪽으로, 두 경우 모두: 방향 필드, 위치 필드 및 출력 메시의 시각화(각각 71.1초 및 67.2초 만에 계산됨). 쿼드의 경우, 쿼드 도미넌트 메시를 1/4 해상도로 최적화하고 한 번 세분화하여 순수 쿼드 메시를 얻습니다.

### 1 Introduction

컴퓨터 그래픽스와 CAD 애플리케이션에서는 삼각형 및 사각형 지배 메시가 표면을 표현하기 위해 널리 사용됩니다. T-스플라인과 Dyadic T-Mesh Subdivision의 도입으로, T-joint가 있는 사각형 지배 메시(T-메시)는 순수한 사각형 메시의 특성과 응용을 갖게 되었으며, 더 유연하게 사용할 수 있고, CAD 응용 프로그램에서 종종 원하는 유연한 지역 세분을 자연스럽게 지원합니다.

메시 생성 알고리즘은 지역 방법과 전역 방법으로 분류될 수 있습니다. 전자는 일반적으로 간단하고 견고하며 확장성이 있지만, 그들의 지역성 때문에 정규 격자와 연결성이 다른 많은 특이점을 도입하는 경향이 있습니다. 전역 알고리즘은 전체 데이터셋에 따라 크기가 결정되는 최적화 문제를 해결하며, 품질은 크게 향상시키지만 확장성, 효율성, 구현의 단순성을 희생합니다.

우리는 지역 스무딩 연산자에 기반한 새로운 접근법인 instant field-aligned meshing을 제안합니다. 이것은 구현이 간단하고 견고하며, 제어 가능하고 효율적입니다. 새 메시의 가장자리 정렬 추정 및 요소 배치와 같은 두 가지 전역 문제를 해결합니다. 이 알고리즘은 근접성의 느슨한 정의만을 요구하므로, 점군을 처리하고, 구조화되지 않은 다중 해상도 계층을 채택함으로써 수백만 개의 샘플을 초과하는 데이터셋에 확장할 수 있습니다.

우리의 알고리즘은 접선 필드의 부드러움 에너지에 의존하며, 이는 내재적 또는 외재적일 수 있습니다. 기계적인 모델에 특히 중요한 형상 특징에 대한 자연스러우면서 매개변수가 없는 정렬을 가져옵니다. 또한 어떤 매개변수 조정 없이 출력 메시의 가장자리를 날카로운 특징에 자연스럽게 붙입니다.

우리의 알고리즘이 메시의 크기와 선형적으로 비례하는 실행 시간을 가집니다: 중간 크기의 데이터셋(< 100k 삼각형)은 1초 미만의 시간 안에 메시화될 수 있고, 우리의 실험에서 가장 큰 데이터셋인 372백만 개의 삼각형을 10분 미만으로 처리했습니다.

우리는 이 효율성을 활용하고, 최종 메시의 가장자리 정렬, 표면에서의 정확한 위치, 특이점의 위치와 수를 제어할 수 있는 대화식 브러시 도구 세트를 도입했습니다. 모든 스트로크의 효과는 실시간으로 시각화되어 빠른 디자인 반복을 가능하게 합니다. 이러한 도구들을 이용해 우리의 시스템은 자동 및 수동 메시화 방법을 결합합니다: 사용자가 중요한 영역에서 요소의 위치를 제어할 수 있도록 하면서, 표면의 나머지 부분은 자동으로 세분화합니다.

![이 백서의 시각화 유형: 우리 방법의 핵심 단계는 불연속 방향 및 위치 필드를 해결합니다(왼쪽). 백서의 나머지 부분에서는 보다 직관적인 시각화인 부드러운 시각화(가운데) 또는 추출된 메시(오른쪽)를 사용하여 표시합니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%201.png)

이 백서의 시각화 유형: 우리 방법의 핵심 단계는 불연속 방향 및 위치 필드를 해결합니다(왼쪽). 백서의 나머지 부분에서는 보다 직관적인 시각화인 부드러운 시각화(가운데) 또는 추출된 메시(오른쪽)를 사용하여 표시합니다.

우리의 알고리즘을 검증하기 위해 삼각형 및 사각형 메시화 방법과 비교하며, 우리의 방법이 보다 단순하고, 견고하며, 제어 가능하고, 빠르며, 큰 데이터셋에 확장 가능하면서도 기존의 기술보다 높은 등방성을 가진 메시를 생성함을 보여줍니다. 이러한 장점을 얻기 위해 지불하는 대가는 전역 매개화에 기반한 최신 재메시화 알고리즘에 비해 특이점의 수가 증가한다는 것입니다. 우리는 추가 자료로 우리의 대화식 참조 구현의 전체 소스 코드와 바이너리, 그리고 우리의 알고리즘으로 생성된 232개의 메시 컬렉션 및 그것들을 재현하는 방법을 제공합니다.

### 2 Related work

이 문서에서는 3D 컴퓨터 그래픽에서 메시를 재배열하는 프로세스인 리메싱의 다양한 기법에 대해 설명합니다.

1. 표면 재구성: 3D 스캐닝 또는 스테레오 재구성과 같은 기법은 범위 맵 또는 포인트 클라우드의 형태로 표면 샘플을 추출한 다음 불연속적인 표면으로 변환합니다. 저자가 제안한 알고리즘은 빠르고 확장 가능하며 대규모 데이터 세트를 처리할 수 있지만, 다른 대안과 달리 노이즈에 대한 저항력이 떨어집니다.
2. 로컬 메시: 이 접근 방식은 토폴로지 연산을 적용하여 기존 메시를 수정합니다. 로컬 방법은 확장 가능하고 견고하지만 메시 가장자리를 표면 모양에 정확하게 정렬하고 특이점의 위치를 제어하는 데 어려움이 있습니다. 저자의 방법은 형상 특징에 대한 특이점 및 가장자리 정렬을 제어하여 이러한 문제를 극복합니다.
3. 스케치 기반 리메싱: 이 방법을 사용하면 벡터 필드를 인터랙티브하게 디자인할 수 있으므로 생성된 결과를 완벽하게 제어하려는 아티스트에게 이상적입니다. 저자의 접근 방식은 다른 작품의 툴을 결합하여 주요 영역에서 정밀한 제어를 용이하게 하는 동시에 나머지 표면의 테셀레이션을 자동화합니다.
4. 글로벌 파라미터화: 전역 방법은 전체 데이터 세트를 고려하는 최적화를 포함합니다. 이러한 방법은 메시 품질과 특이점을 정밀하게 제어할 수 있지만 구현이 복잡하고 대규모 데이터 세트에 대한 확장성에 문제가 있는 경우가 많습니다. 저자의 알고리즘은 에지 정렬과 특이점 배치를 유지하면서 전역 매개변수화를 통해 메시를 생성하는 다른 접근 방식을 취합니다.
5. 통합: 쿼드 도미넌트 메쉬는 유연성 때문에 애니메이션 용도로 선호됩니다. 곡률선을 통합하여 만들 수 있으며 순수한 사변형 메시로 쉽게 변환할 수 있습니다. 저자의 알고리즘은 인터랙티브 특이점 이동 및 축소 기능을 갖춘 사변형 메시를 생성하여 최종 결과에 대한 높은 수준의 사용자 제어 기능을 제공합니다.

이 문서는 다양한 리메싱 기법에 대한 포괄적인 개요를 제공하며, 확장성, 견고성, 정밀도 측면에서 저자의 알고리즘이 가진 강점을 강조합니다.

### 3 Method

이 단원에서는 포인트 클라우드 또는 기타 형식의 기하학적 데이터에서 메시 구조를 만드는 알고리즘에 대해 설명합니다. 간단히 요약하면 다음과 같습니다:

![당사의 방식은 저품질 또는 비매니폴드 입력에도 견고합니다. 입력에 강합니다. 왼쪽: 545개의 비다양체 정점이 있는 까다로운 입력 메시, 특히 왼쪽 눈 영역 주변. 오른쪽: 우리의 방법은 눈의 메시를 생성할 수 없지만 품질이 서서히 저하됩니다. 매니폴드 입력을 가정하는 전역 방법 매니폴드 입력을 가정하는 글로벌 메서드[Ray et al. 2006; Bommes et al. 2009]는 이러한 데이터를 처리할 수 없습니다. 이러한 데이터를 처리할 수 없습니다. 우리가 시도한 구현은 단순히 충돌했습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%202.png)

당사의 방식은 저품질 또는 비매니폴드 입력에도 견고합니다. 입력에 강합니다. 왼쪽: 545개의 비다양체 정점이 있는 까다로운 입력 메시, 특히 왼쪽 눈 영역 주변. 오른쪽: 우리의 방법은 눈의 메시를 생성할 수 없지만 품질이 서서히 저하됩니다. 매니폴드 입력을 가정하는 전역 방법 매니폴드 입력을 가정하는 글로벌 메서드[Ray et al. 2006; Bommes et al. 2009]는 이러한 데이터를 처리할 수 없습니다. 이러한 데이터를 처리할 수 없습니다. 우리가 시도한 구현은 단순히 충돌했습니다.

방향 필드: 알고리즘은 먼저 입력 서페이스에 '방향 필드'를 설정합니다. 이는 서페이스의 모든 지점에 있는 방향 집합으로, 최종 메시의 가장자리가 어떻게 정렬되어야 하는지를 나타냅니다. 이 알고리즘은 서페이스의 특징과 자연스럽게 정렬되는 이 필드를 생성하는 데 매개변수 없이 자동화된 고유한 접근 방식을 사용합니다. 이는 특정 제약 조건이나 파라미터가 필요한 다른 방법과 다릅니다.

위치 필드: 오리엔테이션 필드 다음에 알고리즘은 각 버텍스에 대한 로컬 파라미터화를 계산합니다. 이 필드의 그라데이션은 1단계의 방향 필드에 맞춰 정렬됩니다. 좌표를 할당하는 데 비용이 많이 드는 전역 최적화가 필요하지 않으므로 더 빠르고 간단하게 최적화할 수 있습니다. 또한 로컬 접근 방식을 통해 알고리즘이 불규칙한 입력을 더 잘 처리할 수 있습니다.

통합 알고리즘: 방향과 위치 필드가 함께 작동합니다. 이 알고리즘은 비선형 가우스-사이델(GS) 반복과 무차별 대입 검색을 사용하여 에너지를 최소화하여 로컬 방식으로 부드러워지며, 무작위화 또는 다중 해상도 계층구조를 사용하여 로컬 최소값이 나빠지는 것을 방지합니다.

- 가우스-사이델
    
    가우스-사이델 방법은 선형 방정식 시스템을 풀기 위해 수치 해석에서 널리 사용되는 기법입니다. 기본 아이디어는 시스템의 모든 변수를 반복하고 각 반복마다 다른 변수의 이전 값을 기반으로 현재 변수를 업데이트하는 것입니다.
    
    가우스-사이델 방법은 일반적으로 선형 방정식에 사용됩니다. 그러나 비선형 방정식 시스템을 풀기 위해 확장할 수도 있습니다. 이러한 확장을 흔히 비선형 가우스-사이델(NGS) 방법 또는 반복 가우스-사이델 방법이라고 합니다.
    
    비선형 가우스-사이델(GS) 반복법은 선형 GS 방법과 비슷한 원리로 작동하지만, 선형 방정식 대신 비선형 방정식을 사용합니다.
    
    다음은 작동 원리에 대한 간단한 설명입니다:
    
    1. 초기화: 솔루션에 대한 초기 추측을 선택합니다.
    2. 반복: 미리 정해진 순서로 각 변수를 반복하고 다른 변수의 현재 값을 사용하여 해당 비선형 방정식을 풀어서 현재 변수를 업데이트합니다.
    3. 수렴 확인: 변수를 완전히 통과할 때마다(한 번의 스윕) 솔루션이 수렴했는지 확인합니다. 일반적으로 변수의 변화가 특정 임계값 이하인지 또는 잔차(방정식의 왼쪽과 오른쪽의 차이)가 충분히 작은지 확인하는 방식으로 이 작업을 수행합니다.
    4. 반복: 솔루션이 수렴하지 않으면 반복 단계로 돌아갑니다. 수렴하면 방법이 완료된 것입니다.
    
    특히 방정식이 강하게 결합된 경우(즉, 각 변수의 값이 다른 변수에 큰 영향을 미치는 경우) NGS 방법의 수렴이 다른 반복 방법보다 빠를 수 있습니다. 그러나 모든 반복 방법과 마찬가지로 NGS는 모든 비선형 방정식 시스템에 대해 수렴이 보장되는 것은 아니며, 초기 추측과 변수의 순서에 따라 성능이 달라질 수 있습니다.
    

입력 표현: 이 알고리즘은 입력 표면을 위치와 법선 방향에 연결된 정점이 있는 그래프로 표현합니다. 가우스-사이델 방법과 유사한 로컬 반복을 사용하여 부드러운 방향 및 위치 필드를 계산합니다.

최적화: 알고리즘에는 내재적 접근 방식과 외재적 접근 방식의 두 가지 최적화 접근 방식이 있습니다. 내재적 접근 방식은 인접한 벡터를 공통 평면으로 펼친 후 벡터 간의 각도 차이를 측정하는 것을 기반으로 합니다. 외재적 접근 방식은 3D 주변 공간에서 직접 거리를 측정하므로 형상 특징에 더 잘 정렬할 수 있습니다.

이 알고리즘은 표면에서 특이점이라고 하는 특수한 점을 감지하여 출력 메시의 테셀레이션 밀도를 제어합니다.

이 글에서는 3D 표면에서 주어진 방향 필드에 정렬하는 필드의 일종인 위치 필드를 최적화하는 새로운 방법에 대해 설명합니다. 기존 방법에는 너무 복잡하고, 대규모 데이터 세트에 확장할 수 없으며, 토폴로지 노이즈에 민감하다는 한계가 있습니다. 또한 방향 필드에 대한 일관된 단일 파라미터화를 생성해야 하며 표면 절단이 필요합니다. 이 프로세스는 최소 제곱 방식으로 기울기를 방향 필드에 정렬하며 복잡한 비선형 최적화를 수반합니다.

![내재적(위) 및 외재적(아래) 방향 RoSy 필드의 평활도 측정. 이웃하는 정점 vi와 vj는 각각 법선 ni, nj 를 갖는 접선면을 갖습니다. 필드 평활도를 내재적으로 측정하기 위해서는 먼저 정점 vj의 탄젠트 평면을 회전 회전(nj × ni, ](nj , ni))으로 변환하여 정점 vi의 탄젠트 평면과 평행이 되도록 한 다음, 공통 2D 공간에서 oi와 가장 작은 각을 갖는 j의 대표 벡터의 정수 회전을 구하여 관련 RoSy의 차이를 측정합니다. 외재적 평활도 측정은 공통 2D 공간으로의 변환을 생략하고 3D에서 직접 차이를 측정하여 두 RoSy 대표 벡터의 각도를 최소화하는 정수 회전을 찾습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%203.png)

내재적(위) 및 외재적(아래) 방향 RoSy 필드의 평활도 측정. 이웃하는 정점 vi와 vj는 각각 법선 ni, nj 를 갖는 접선면을 갖습니다. 필드 평활도를 내재적으로 측정하기 위해서는 먼저 정점 vj의 탄젠트 평면을 회전 회전(nj × ni, ](nj , ni))으로 변환하여 정점 vi의 탄젠트 평면과 평행이 되도록 한 다음, 공통 2D 공간에서 oi와 가장 작은 각을 갖는 j의 대표 벡터의 정수 회전을 구하여 관련 RoSy의 차이를 측정합니다. 외재적 평활도 측정은 공통 2D 공간으로의 변환을 생략하고 3D에서 직접 차이를 측정하여 두 RoSy 대표 벡터의 각도를 최소화하는 정수 회전을 찾습니다.

- 참고 : 내재적, 외재적 방향 필드
    
    오리엔테이션 필드 최적화 및 지오메트리 처리의 맥락에서 내재적 및 외재적이라는 용어는 서로 다른 유형의 속성 또는 측정값을 나타냅니다.
    
    외적 속성은 공간에서 오브젝트의 위치 또는 방향에 따라 달라집니다. 이러한 속성은 외부 참조 프레임 또는 좌표계를 기준으로 측정됩니다. 예를 들어, 3D 공간에서 표면의 점의 위치는 전역 좌표계에 따라 달라지므로 외재적 속성이라고 할 수 있습니다.
    
    반면에 내재적 속성은 공간에서 오브젝트의 위치나 방향과 무관합니다. 이러한 속성은 전역 좌표계와의 관계가 아니라 객체 자체에만 의존합니다. 이러한 속성은 오브젝트가 찢어지거나 접착되지 않는 변형에서도 보존됩니다. 고유 속성의 예로는 서페이스의 두 점 사이의 측지 거리(서페이스를 따라 최단 경로)를 들 수 있습니다.
    
    방향 필드와 관련하여 내재적 최적화에 대해 이야기할 때는 외부 참조 프레임과의 관계를 무시하고 필드 자체의 속성을 기반으로 한 계산 및 조정을 의미합니다. 반대로 외재적 최적화는 글로벌 기준 프레임에서 필드의 방향 또는 위치를 고려합니다.
    
    대부분의 경우 내재적 속성은 3D 공간에서 오브젝트의 특정 위치나 임베딩에 덜 민감하고 더 안정적이기 때문에 메시와 같은 작업에 선호됩니다.
    
- 방n-RoSy(회전 대칭) 필드
    
    n-RoSy(회전 대칭) 필드는 컴퓨터 그래픽 및 지오메트리 처리에 사용되는 기하학적 개념입니다. 모든 점과 관련된 벡터 또는 방향이 등각 간격으로 n개 있는 벡터 필드를 말합니다. 이러한 벡터는 장미의 꽃잎처럼 공통의 중심(점)에서 대칭적인 패턴으로 퍼져 나가기 때문에 '장미'를 형성한다고 합니다.
    
    예를 들어, 2-RoSy 필드는 각 점이 벡터와 그 반대 방향이라는 두 가지 방향과 연관되어 있기 때문에 기존의 벡터 필드와 동일합니다. 각 지점의 4-RoSy 필드는 나침반의 기본 방향과 유사하게 등각으로 간격을 둔 4개의 방향으로 구성됩니다.
    
    이러한 필드는 텍스처 합성, 리메싱, 비실사 렌더링과 같은 다양한 지오메트리 처리 작업에 유용합니다. 이러한 프로세스를 표면 전체에 걸쳐 방향이 일관된 방식으로 안내하는 데 도움이 됩니다.
    
    예를 들어 메시에서 n-RoSy 필드는 방향 지침을 제공하여 메시가 주어진 표면을 가로질러 어떻게 정렬되거나 흘러야 하는지 알려줍니다. n-RoSy의 "n"은 결과 패턴 또는 메시의 복잡도와 대칭을 제어하기 위해 선택할 수도 있습니다. n이 높을수록 복잡도와 회전 대칭이 높아집니다.
    

저자는 전체 메시에서 연속적인 파라미터화 함수를 찾는 대신 각 버텍스에 대해 로컬 파라미터화를 계산하는 로컬 접근 방식을 제안합니다. 이렇게 하면 가장자리의 불연속성이 발생하지만 방향 필드와 정확하게 정렬됩니다.

버텍스의 로컬 파라미터화를 단순화하기 위해 그라데이션이 방향 필드에 정확하게 정렬되도록 인코딩하는 방법을 제안합니다. 이렇게 하면 자유도가 크게 줄어듭니다. 이 파라미터화는 정점의 탄젠트 평면에 있는 2D 점으로, 가장 가까운 격자 점을 참조하여 전체 탄젠트 평면에 대한 파라미터화를 효과적으로 정의합니다.

![(a) 내재 평활 에너지에 기반한 접근법 에 기반한 접근법[Ray et al. 2008; Bommes et al. 2009]은 기본적으로 다음과 같습니다. 기하학적 특징에 정렬되지 않은 평활 필드를 계산합니다. 이 "외적 지식"을 최적화에 통합하려면 추가 제약 조건이 필요합니다. (b) 우리의 새로운 외적 에너지는 제약 조건을 지정하지 않아도 자연스럽게 동일한 효과를 얻을 수 있습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%204.png)

(a) 내재 평활 에너지에 기반한 접근법 에 기반한 접근법[Ray et al. 2008; Bommes et al. 2009]은 기본적으로 다음과 같습니다. 기하학적 특징에 정렬되지 않은 평활 필드를 계산합니다. 이 "외적 지식"을 최적화에 통합하려면 추가 제약 조건이 필요합니다. (b) 우리의 새로운 외적 에너지는 제약 조건을 지정하지 않아도 자연스럽게 동일한 효과를 얻을 수 있습니다.

이러한 로컬 파라미터화가 전체 서페이스에 걸쳐 정의되면 각 정점의 파라미터 위치의 분수 좌표를 나타내는 위치 필드가 형성됩니다. 이 방법의 핵심은 두 개의 로컬 파라미터화를 비교하고 서로 얼마나 유사한지 정량화하는 평활 에너지 함수를 생성하는 것입니다.

![3.2절에서 소개한 기본 비선형 가우스-사이델 반복은 그 자체로 방향 및 위치 필드를 국부적으로 평활화할 수 있지만, 곧 원하는 해와 거리가 먼 국부 최소값에 갇히게 됩니다. 반복을 간단한 다중 해상도 계층 구조와 결합하면 이 문제를 피할 수 있습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%205.png)

3.2절에서 소개한 기본 비선형 가우스-사이델 반복은 그 자체로 방향 및 위치 필드를 국부적으로 평활화할 수 있지만, 곧 원하는 해와 거리가 먼 국부 최소값에 갇히게 됩니다. 반복을 간단한 다중 해상도 계층 구조와 결합하면 이 문제를 피할 수 있습니다.

또한 저자는 매번 무차별 지역 검색을 사용하여 이 평활성 에너지 함수를 반복적으로 최소화하는 프로세스에 대해서도 설명합니다. 저자는 정수 변환이 가능한 공간이 무한하기 때문에 최적의 정수 변수를 찾는 것이 어려워 보이지만, 특정 문제를 통해 매우 국지적인 검색이 가능하기 때문에 관리가 더 쉬워진다고 말합니다.

마지막으로, 다중 해상도 계층 구조를 구현하여 수렴을 개선하고 알고리즘이 로컬 최소값에서 벗어날 수 있도록 합니다. 더 거친 그래프를 반복적으로 구성하고 각 반복마다 정점 수를 대략 절반으로 줄입니다. 또한 메시의 경계 상자 내에서 균일하게 분포된 임의의 접선 벡터와 위치를 사용하여 필드를 초기화하는 방법을 제안합니다.

전체 프로세스는 인접한 두 계층 레벨의 전역 최소값이 밀접하게 관련되어 있으므로 한 레벨에서 찾은 솔루션이 다음 레벨의 좋은 출발점이 된다는 가설에 기반합니다. 이 가설에 대한 공식적인 증거는 없지만, 실제 실험에서 이 가설이 성공적이라는 것을 발견했습니다.

이 단원에서는 방향과 위치라는 두 가지 정보 필드를 기반으로 3D 메쉬 또는 도형을 생성하고 편집하는 방법에 대해 설명합니다.

정수 변수인 kij와 tij는 두 필드의 대칭을 국부적으로 인수 분해하는 데 필요한 변환을 나타냅니다. 필드의 특이점 또는 불규칙성은 출력 메시에 영향을 줄 수 있는 방향 또는 위치의 변화를 나타냅니다.

방향 필드에서 특이점은 에지가 정상보다 많거나 적은 버텍스를 나타냅니다. 위치 필드에서 특이점은 주기에 따른 정수 변환의 변화에 해당하며, 방향에는 영향을 미치지 않지만 출력 메시의 규칙성을 변경할 수 있습니다.

![노란색 마커(왼쪽)와 같은 위치 특이점은 와 같은 위치 특이점은 위치 필드 최적화에서 자연스럽게 발생하며, 이를 통해 우리의 메서드가 사용 가능한 공간에 적응하고 출력 메시에서 T 접합을 생성하여 출력 메시를 생성하여 등방성을 극대화합니다. 특이점은 출력 메시(가운데)에 오각형 또는 삼각형을 생성하며, 세분화 단계(오른쪽)를 거쳐 원자가 3과 5의 정점 쌍으로 변환할 수 있습니다.](Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8/Untitled%206.png)

노란색 마커(왼쪽)와 같은 위치 특이점은 와 같은 위치 특이점은 위치 필드 최적화에서 자연스럽게 발생하며, 이를 통해 우리의 메서드가 사용 가능한 공간에 적응하고 출력 메시에서 T 접합을 생성하여 출력 메시를 생성하여 등방성을 극대화합니다. 특이점은 출력 메시(가운데)에 오각형 또는 삼각형을 생성하며, 세분화 단계(오른쪽)를 거쳐 원자가 3과 5의 정점 쌍으로 변환할 수 있습니다.

이 방법은 방향과 위치 값에 영향을 주는 편집 작업도 지원합니다. 정수 변수를 사용하여 특이점을 이동하고 조작할 수 있습니다. 가우스-사이델(GS) 반복 중 제약 조건은 필드 제약 조건을 적용할 수 있으며, 이 정보는 멀티레졸루션 계층 구조로 전달될 수 있습니다.

이 방법에는 각 삼각형의 가장자리에 정수 변수를 추가하고 특정 값을 확인하여 특이점을 감지하는 방법도 포함되어 있습니다. 포인트 클라우드의 특이점 편집 및 조작은 향후 연구 분야로 제안되었습니다.

출력은 캣멀-클락 세분화를 사용하여 순수한 쿼드 메시로 변환할 수 있는 쿼드 지배 메시입니다. 위치 특이점의 수는 출력 메시의 해상도에 따라 달라집니다.

이 방법은 마지막으로 계산된 필드에서 메시를 추출하는 프로세스를 설명합니다. 이 프로세스에는 입력 메시의 정점과 가장자리에서 방향이 없는 그래프를 생성하고 가장자리가 남지 않을 때까지 정점을 축소하는 과정이 포함됩니다. 그 결과 원하는 대로 조작하고 편집할 수 있는 3D 모양이 생성됩니다.

### 4 Results

이 단원에서는 3D 모델에 최적화된 메시를 생성하기 위한 새로운 알고리즘의 개발과 성능에 대해 설명합니다. 이 알고리즘은 C++로 작성되었으며 선형 대수 연산을 위한 Eigen 라이브러리와 병렬화를 위한 인텔 스레딩 빌딩 블록을 사용합니다. 이 알고리즘은 비선형 가우스-사이델 반복을 병렬화하는 표준 접근 방식을 사용하며 단정밀도 산술에서 잘 작동합니다.

메시 애플리케이션은 인터랙티브한 방식으로 OpenGL을 사용하여 방향 및 위치 필드를 실시간으로 시각화합니다. 사용자 인터페이스를 통해 다양한 수준의 디테일로 포인트 클라우드와 트라이앵글 메시를 렌더링할 수 있습니다.

이 알고리즘은 특별한 수치적 주의가 필요하지 않으며, 12코어 2.7GHz 인텔 제온 E5 워크스테이션(64GB 메모리)과 16코어 인텔 제온 머신(256GB 메모리) 등 다양한 컴퓨터에서 테스트되었습니다.

개발된 방법은 기존 기법에 비해 여러 테스트에서 우수한 것으로 입증되었습니다. 이 방법은 더 높은 등방성, 규칙성 및 형상 특징에 대한 정렬을 보여주었습니다. 또한 다양한 필드 대칭에 쉽게 적응할 수 있습니다. 하지만 특이점 수가 많아 글로벌 파라미터화 알고리즘과 비교했을 때 결과가 약간 덜 규칙적입니다.

이 접근 방식은 확장 가능하고 견고하며 매우 큰 3D 스캔을 빠르게 메싱할 수 있습니다. 이 알고리즘은 글로벌 파라메트리라이제이션 벤치마크에서 1초 이내에 완료되는 등 우수한 성능을 보였으며, 까다로운 데이터 세트의 대규모 데이터베이스를 처리할 수 있음을 입증했습니다.

결론적으로 이 알고리즘은 속도, 확장성, 유연성, 기존 방법과 비교했을 때 여러 면에서 뛰어난 성능을 보여주기 때문에 최적화된 3D 메시를 생성하는 데 유망한 도구로 보입니다.

### 5 Concluding remarks

연구진은 수억 개의 요소가 포함된 다양한 3D 모델에 적용할 수 있는 새롭고 강력하며 확장 가능한 반정형 메시를 생성하는 알고리즘을 개발했습니다. 이 알고리즘은 방향 필드 최적화, 위치 필드 최적화, 메시 추출의 세 가지 주요 단계로 구성됩니다. 이러한 단계는 함께 또는 독립적으로 작동할 수 있어 프로세스에 유연성을 제공합니다.

이 알고리즘은 포인트 클라우드, 범위 스캔, 삼각형 메시와 같은 다양한 입력 데이터를 처리하는 데 탁월합니다. 또한 매니폴드가 아닌 입력에 대해서도 탄력적입니다. 하지만 글로벌 파라미터화 방법보다 특이점이 더 많이 발생한다는 것이 주요 한계 중 하나입니다. 연구진은 인터랙티브 브러시를 사용하여 특이점의 수를 줄일 수 있는 가능성을 제시하고 이를 더 연구할 계획입니다.

이 알고리즘의 고유한 로컬 최적화 접근 방식은 기존 기법과는 대조적이지만 입력 크기에 비해 선형적인 실행 시간과 같은 이점을 제공합니다. 이러한 장점은 연구자와 실무자 모두에게 매력적으로 다가올 것으로 예상됩니다.

향후 작업에는 알고리즘을 반규칙적인 볼류메트릭 메시로 확장하고 새로운 유형의 인터랙티브 스트로크를 개발하는 것이 포함될 수 있습니다. 연구진은 이 분야의 연구를 촉진하기 위해 최적화된 병렬 구현, 데이터 세트, 비교 및 결과를 대중에게 공개할 계획입니다.

- 요약
    
    이 연구 논문은 3D 모델에서 반정형 메쉬를 생성하는 다단계 알고리즘에 대해 설명하는 것 같습니다. 다음은 그 과정을 단계별로 분석한 내용입니다:
    
    1단계: 오리엔테이션 필드 최적화
    알고리즘의 첫 번째 단계는 오리엔테이션 필드를 생성하는 것입니다. 오리엔테이션 필드는 기본적으로 3D 모델의 전체 구조 또는 "흐름"을 나타내는 것으로, 메시의 각 지점에서의 방향성을 자세히 설명합니다. 여기서 최적화는 필드가 모델의 모양과 잘 정렬되도록 하는 것입니다. 이 단계는 다른 단계와 독립적으로 사용할 수 있으며, 연구원들은 Python과 C++로 독립적으로 구현할 수 있는 방법을 제공했습니다.
    
    2단계: 포지션 필드 최적화
    다음 단계는 위치 필드를 생성하는 것입니다. 이 필드는 메시 요소의 정점(모서리)의 위치를 나타냅니다. 방향 필드와 마찬가지로 3D 모델의 모양에 최대한 가깝게 일치하도록 최적화됩니다. 이 단계도 다른 단계와 별도로 사용할 수 있으며 Python 및 C++ 구현이 가능합니다.
    
    3단계: 메시 추출
    방향 및 위치 필드가 최적화되면 다음 단계는 실제로 메시를 생성하는 것입니다. 이 단계에서는 두 필드를 사용하여 메시의 정점을 어디에 배치하고 어떻게 연결할지 결정해야 합니다. 이 단계는 전역 파라미터화 작업에 적용하여 이 복잡한 작업에 대한 간단한 솔루션을 제공할 수 있습니다.
    
    이 알고리즘의 주요 강점 중 하나는 로컬리티로, 3D 모델 전체를 한 번에 처리하지 않고 관리하기 쉬운 작은 부분으로 나누어 작동한다는 점입니다. 따라서 수억 개의 요소가 포함된 모델을 처리할 수 있는 보다 견고하고 확장성이 뛰어납니다.
    
    하지만 이 접근 방식에는 몇 가지 한계가 있는데, 특히 글로벌 파라미터화 방법보다 특이점(메시의 흐름이 갑자기 변하는 지점)이 더 많이 발생하는 경향이 있다는 점이 있습니다. 저자들은 향후 연구에서 이러한 특이점을 자동으로 줄이는 방법을 연구할 수 있다고 제안합니다.
    
    이러한 한계에도 불구하고 저자들은 선형적인 실행 시간과 유연성 등 알고리즘의 장점을 고려할 때 3D 모델링 분야에 기여할 수 있는 가치가 있다고 생각합니다. 이들은 향후 이 분야의 연구를 지원하기 위해 모든 데이터 및 결과와 함께 알고리즘 구현을 공개할 계획입니다.