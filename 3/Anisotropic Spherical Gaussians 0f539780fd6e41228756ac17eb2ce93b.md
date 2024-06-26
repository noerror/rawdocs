# Anisotropic Spherical Gaussians

[https://cg.cs.tsinghua.edu.cn/people/~kun/asg/](https://cg.cs.tsinghua.edu.cn/people/~kun/asg/)

[https://cg.cs.tsinghua.edu.cn/people/~kun/asg/paper_asg.pdf](https://cg.cs.tsinghua.edu.cn/people/~kun/asg/paper_asg.pdf)

- 2013

![고도로 이방성인 금속 접시의 렌더링에서 SG (구형 가우시안) 기반 근사와 ASG (이방성 구형 가우시안) 기반 근사의 비교. 금속 접시의 BRDF는 다른 이미지에서 ASG 또는 SG의 다른 수로 근사화됩니다. ASG와 SG의 우수한 특성에 주목하십시오. 1 ASG로 생성된 결과는 경로 추적 참조와 이미 잘 일치합니다 (L² 오류는 0.10 %), 프레임 속도는 125 fps로 매우 높으며, 유사한 품질을 얻으려면 10 개 이상의 SG가 필요하지만 프레임 속도는 훨씬 낮습니다 (13개 SG의 경우 19fps, 15개 SG의 경우 17fps). L² 오류와 각 설정의 프레임 속도도 해당 부제에서 주어집니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled.png)

고도로 이방성인 금속 접시의 렌더링에서 SG (구형 가우시안) 기반 근사와 ASG (이방성 구형 가우시안) 기반 근사의 비교. 금속 접시의 BRDF는 다른 이미지에서 ASG 또는 SG의 다른 수로 근사화됩니다. ASG와 SG의 우수한 특성에 주목하십시오. 1 ASG로 생성된 결과는 경로 추적 참조와 이미 잘 일치합니다 (L² 오류는 0.10 %), 프레임 속도는 125 fps로 매우 높으며, 유사한 품질을 얻으려면 10 개 이상의 SG가 필요하지만 프레임 속도는 훨씬 낮습니다 (13개 SG의 경우 19fps, 15개 SG의 경우 17fps). L² 오류와 각 설정의 프레임 속도도 해당 부제에서 주어집니다.

### 1 Introduction

컴퓨터 그래픽, 특히 렌더링의 경우 구형 함수를 표현하는 간결한 방법이 있으면 유용합니다. 현재 구형 가우시안(SG)은 조명과 반사 등을 표현하는 데 사용됩니다. SG는 렌더링에 필수적인 회전 불변성 및 특정 수학 연산에 대한 폐쇄형 솔루션과 같은 바람직한 속성을 가지고 있기 때문에 선택됩니다. 하지만 SG는 이방성(방향에 따라 달라짐)이 있는 실제 조명이나 반사를 표현하는 데는 한계가 있습니다. 이 문제를 해결하기 위해 여러 SG를 혼합한 SG 믹스처가 사용됩니다. 하지만 이렇게 하면 복잡성이 증가하고 정확도와 속도 사이에서 타협해야 합니다.

이러한 한계를 극복하기 위해 이방성 구형 가우시안(ASG)이라는 새로운 함수가 도입되었습니다. 이 비등방성 구형 함수를 보다 효과적으로 표현할 수 있습니다. ASG를 사용하면 동일한 함수를 표현하는 데 필요한 숫자가 줄어들어 정확도와 속도가 모두 향상됩니다. 예를 들어 금속 접시를 렌더링할 때 하나의 ASG로 렌더링할 수 있지만 비슷한 효과를 내기 위해서는 15개의 SG가 필요하지만 속도는 느립니다.

ASG는 SG의 우수한 특성을 그대로 유지하며 SG처럼 정확한 수학적 해법은 없지만 거의 정확한 해법을 제공합니다. 따라서 ASG는 다용도로 사용할 수 있어 기존 SG 애플리케이션을 개선할 수 있습니다. ASG의 효과는 렌더링 및 편집 애플리케이션에서 그 잠재력을 보여주는 새로운 렌더링 프레임워크를 통해 입증되었습니다.

### 2 Related Works

그래픽의 구형 가우시안(SG):

- SG는 컴퓨터 그래픽에서 구형 함수를 나타내며 환경 조명, 빛의 이동 및 BRDF를 묘사하는 데 도움이 됩니다.
- Tsai와 Green 등의 과거 연구에서는 다양한 목적으로 SG를 사용했지만 동적 반사율에 문제가 있었으며 렌더링 시 유연성 측면에서 제한이 있었습니다.
- Wang 등은 SG를 사용하여 동적 반사율 문제를 해결했지만 등방성 특성으로 인해 이방성 함수를 근사화하기 어려웠습니다.
- SG의 다른 구현으로는 노멀 맵 필터링, 실시간 렌더링, 간접 하이라이트의 효율적인 렌더링 등의 기술이 있습니다.

PRT에 사용되는 기타 구형 함수:

- SG 외에도 구형 하모닉스(SH), 웨이블릿 및 기타 함수가 사전 계산된 방사 전송(PRT)에서 구형 함수를 표현하는 데 사용되었습니다.
- SH는 저주파수 신호에는 탁월하지만 고주파수 함수에는 어려움을 겪습니다. 마찬가지로 웨이블릿과 다른 방법도 각자의 장단점이 있습니다.
- 이에 비해 ASG는 SG의 장점은 유지하면서 이방성 함수를 표현하는 데 더 효율적입니다.

비등방성 외형:

- 이방성 외관은 브러시드 메탈이나 머리카락과 같은 실제 표면에서 흔히 볼 수 있습니다.
- 이러한 외관을 재현하기 위해 카지야, 워드, 아시크민과 같은 많은 모델이 도입되었습니다. 그러나 이러한 모델은 특히 복잡한 이방성 효과를 표현하는 데 있어 ASG에 비해 특정 한계가 있습니다.
- 마이크로 패싯 BRDF 모델은 반사 마이크로 패싯이 있는 표면을 가정하며 실제 반사를 묘사하는 데 효과적인 것으로 입증되었습니다.

방향 통계:

- 새로운 ASG 함수는 빙엄 분포(Bingham distribution)를 기반으로 합니다. ASG를 공식화하는 동안 피셔-빙햄 분포와 켄트 분포와 같은 다른 분포도 탐색했습니다.
- 피셔-빙햄 분포는 복잡성과 계산 비용으로 인해 어려움이 있었습니다. 더 간단한 버전인 켄트 분포는 두 분포를 곱해도 그 형태를 유지하지 못합니다.
- 이에 비해 빙엄 기반 ASG는 수학적 연산 측면에서 더 나은 솔루션을 제공하므로 의도한 애플리케이션에 더 적합합니다.

### 3 Definition of an ASG

"ASG의 정의" 섹션에서 저자는 이방성 구형 가우시안(ASG)의 개념을 소개합니다. 이 새로운 개념은 등방성인 기존 구형 가우시안(SG)의 한계를 극복하기 위해 개발되었습니다. 그 동기는 이방성 또는 방향에 따른 변화를 설명할 수 있는 표현을 갖기 위해서입니다.

![이방성 구형 가우시안 (ASGs)의 정의. (a) 매개 변수 (λ = 4,µ = 1)를 가진 ASG. (b) 매개 변수 (λ = 1, µ = 1), (λ = 10, µ = 1), (λ = 100, µ = 10), (λ = 300, µ = 3)를 가진 여러 ASG 예. 각 ASG는 Lambert 동일 면적 매개 변수화를 사용하여 위쪽 반구를 디스크에 투영하여 설명됩니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%201.png)

이방성 구형 가우시안 (ASGs)의 정의. (a) 매개 변수 (λ = 4,µ = 1)를 가진 ASG. (b) 매개 변수 (λ = 1, µ = 1), (λ = 10, µ = 1), (λ = 100, µ = 10), (λ = 300, µ = 3)를 가진 여러 ASG 예. 각 ASG는 Lambert 동일 면적 매개 변수화를 사용하여 위쪽 반구를 디스크에 투영하여 설명됩니다.

- ASG의 정의: ASG는 다음 방정식으로 수학적으로 정의됩니다.
G(v;[x,y,z],[λ,µ],c). ASG는 단위 방향 v의 함수로, 직교 프레임을 형성하는 로브 z, 탄젠트 x축 및 이탄젠트 y축에 의해 결정됩니다. 대역폭 λ 및 µ는 각각 x축과 y축을 따라 확산을 제어합니다. 로브 진폭은 c로 표시되며, 평활 항은 다음과 같습니다. S(v;z)는 ASG를 상반구로 제한하여 평활성을 보장하는 데 사용됩니다.
- ASG의 기원: ASG 함수의 생성은 방향 통계의 빙햄 분포에서 영감을 얻었습니다. 빙엄 분포는 3D 방향의 분포를 설명하며, 이 맥락에서 함수가 상반구에 한정되도록 하기 위해 평활 항과 결합하는 데 활용됩니다.
- 대수 형식: 저자들은 대수 형식인 G(v;A)로 ASG를 대체할 수 있는 표현을 제시합니다. ASG의 기하학적 형태와 대수적 형태 사이의 동등성은 행렬 A의 특이값 분해(SVD)를 사용하여 설정됩니다.
- 기존 SG와의 관계: 기존 구형 가우시안(SG)과 새로 도입된 ASG 사이에는 중요한 차이가 있습니다. ASG는 대역폭 λ과 µ가 같을 때 등방성(SG처럼 동작)이 됩니다. 그러나 이 경우에도 등방성 ASG는 기존 SG와 엄격하게 동일하지는 않지만 특정 조건에서는 거의 동일합니다.

본질적으로 저자들은 방향에 따른 변화를 표현하는 데 있어 기존 SG보다 더 유연한 도구로서 ASG를 제안하며, 이러한 표현이 중요한 컴퓨터 그래픽과 같은 애플리케이션에 특히 유용하다고 말합니다.

### 4 Supported Operators

이 글은 그래픽 렌더링의 맥락에서 구형 가우시안(SG) 및 이방성 구형 가우시안(ASG)과 관련된 특정 수학적 개념 및 연산에 대한 자세한 설명으로 보입니다. 주요 요점을 요약해 보겠습니다:

소개:

- 구형 가우시안(SG)은 회전 불변이므로 조명과 같은 구형 함수를 표현하는 데 적합합니다.

![ASG 적분 근사의 평가. 빨간색 곡선은 Eq. 11을 사용하여 근사화되며 파란색 곡선은 Eq. 10을 사용하여 근사화됩니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%202.png)

ASG 적분 근사의 평가. 빨간색 곡선은 Eq. 11을 사용하여 근사화되며 파란색 곡선은 Eq. 10을 사용하여 근사화됩니다.

- SG는 모든 주파수 신호를 나타낼 수 있습니다.
- SG는 적분, 곱셈, 컨볼루션 연산을 위한 폐쇄형 솔루션을 제공합니다. 이러한 특성 덕분에 SG는 특히 그래픽 렌더링에 적합합니다.
- 렌더링 프로세스에는 기본적으로 적분 계산이 포함됩니다.
- 렌더링의 곱셈에는 조명, BRDF, 가시성 등 다양한 함수의 곱셈이 포함됩니다.
- ASG는 SG의 확장으로 이방성 함수를 나타낼 수 있습니다. 앞서 언급한 연산에 대한 정확한 폐쇄형 솔루션은 없지만 그래픽에 적합한 대략적인 솔루션이 있습니다.

ASG의 적분:

- 이 함수의 목적은 단위 구에 대한 ASG의 적분에 대한 해석적 근사치를 유도하는 것입니다.

![두 ASG의 제품. 모든 예제에 대한 첫 번째 ASG (λ1 및 µ1) 및 두 번째 ASG (λ2 및 µ2)의 대역폭 매개 변수는 아래에 나열됩니다. 예제 1: λ1 = µ1 = 3, λ2 = 100, µ2 = 5. 예제 2: λ1 = 10, µ1 = 1, λ2 = 100, µ2 = 10. 예제 3: λ1 = 10, µ1 = 1, λ2 = 1000, µ2 = 10.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%203.png)

두 ASG의 제품. 모든 예제에 대한 첫 번째 ASG (λ1 및 µ1) 및 두 번째 ASG (λ2 및 µ2)의 대역폭 매개 변수는 아래에 나열됩니다. 예제 1: λ1 = µ1 = 3, λ2 = 100, µ2 = 5. 예제 2: λ1 = 10, µ1 = 1, λ2 = 100, µ2 = 10. 예제 3: λ1 = 10, µ1 = 1, λ2 = 1000, µ2 = 10.

![ASG 제품 및 컨볼루션 근사의 L² 오류. 오류는 제한을 통해 측정됩니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%204.png)

ASG 제품 및 컨볼루션 근사의 L² 오류. 오류는 제한을 통해 측정됩니다.

- 일련의 유도 과정을 거친 후, 방정식 10과 방정식 11의 두 가지 방정식을 사용하여 ASG의 적분을 근사화할 수 있습니다. 둘 중 어떤 것을 선택할지는 대역폭 λ과 μ의 값에 따라 달라집니다. 도출된 공식을 검증한 결과 근사 오차가 상당히 작다는 것을 알 수 있습니다.

두 ASG의 곱입니다:

- 목표는 두 ASG의 곱에 대한 좋은 근사치를 찾는 것입니다.
- 두 ASG를 곱한 결과는 다른 ASG로 표현할 수 있습니다. 이는 방정식 13으로 표현할 수 있습니다.
검증 결과는 이 근사치가 실측 데이터와 시각적으로 구별할 수 없는 결과를 제공한다는 것을 보여주며, 이는 그래픽 작업에서 이 근사치가 유용하다는 것을 의미합니다.

ASG와 SG의 컨볼루션:

- 이 컨볼루션을 계산하기 위한 공식이 도출되었습니다. 여기에는 테일러 확장을 사용하고 적분에서 상수를 꺼내는 것이 포함됩니다. 결과는 ASG로 근사화할 수 있습니다.
- 특히 값이 극단적이지 않은 경우 결과의 정확도에 큰 영향을 미치지 않음을 시사합니다. 근사치를 검증하기 위한 시각적 및 정량적 증거를 제공합니다.
- ASG와 SG의 컨볼루션은 유도된 방정식을 사용하여 단일 ASG로 잘 근사화할 수 있습니다.

![ASG와 SG의 컨볼루션. 모든 예제에서 ASG (λ 및 µ) 및 SG (ν)의 대역폭 매개 변수는 아래에 나열됩니다. 예제 1: λ = µ = 5, ν = 1. 예제 2: λ = 40, µ = 1, ν = 10. Example 3: λ = 100, µ = 10, ν = 100.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%205.png)

ASG와 SG의 컨볼루션. 모든 예제에서 ASG (λ 및 µ) 및 SG (ν)의 대역폭 매개 변수는 아래에 나열됩니다. 예제 1: λ = µ = 5, ν = 1. 예제 2: λ = 40, µ = 1, ν = 10. Example 3: λ = 100, µ = 10, ν = 100.

### 5 ASG-based Rendering Framework

렌더링에서 ASG(이방성 구면 가우시안)의 사용, 특히 SG(구면 가우시안)와의 비교에 대해 설명합니다. ASG는 머티리얼과 라이팅의 이방성 동작을 보다 정확하게 포착하는 메커니즘을 제공합니다. 이는 컴퓨터 그래픽에서 특히 브러시드 메탈이나 반사 패턴이 뚜렷한 패브릭과 같은 이방성 머티리얼을 정확하게 렌더링할 때 유용합니다.

언급된 주요 측면을 세분화해 보겠습니다:

- 렌더링 공식: 이 섹션에서는 조명 방향 i, 뷰 방향 o 및 표면 법선( n. BRDF(양방향 반사율 분포 함수)는 확산 및 스페큘러 구성 요소로 세분화됩니다.
- 라이트 근사치: 환경 광원은 ASG 믹스를 사용하여 표현됩니다. 그림 7의 ASG와 SG 비교에서 볼 수 있듯이 ASG가 환경의 이방성 특징을 더 효과적으로 포착할 수 있다는 것이 그 아이디어입니다.

![성 베드로 대성당 및 그레이스 대성당의 환경 조명에 대한 ASGs 및 SGs의 비교.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%206.png)

성 베드로 대성당 및 그레이스 대성당의 환경 조명에 대한 ASGs 및 SGs의 비교.

- BRDF 근사치: 여기에서는 마이크로 패싯 모델에 기반한 BRDF의 스페큘러 구성 요소에 대해 설명합니다. 이 텍스트에서는 ASG를 사용하여 특정 BRDF 모델을 근사화하는 방법을 설명하고 특히 이방성 BRDF의 경우 ASG의 효율성과 정확성을 보여줍니다.
- 파라메트릭 BRDF: 이 문서에서는 ASG를 사용한 다양한 파라메트릭 BRDF 모델의 근사치를 SG와 비교하여 설명합니다. 이 논의에서는 ASG가 이방성이 높은 BRDF를 표현할 때에도 경로 추적 참조와 시각적으로 유사한 결과를 얻을 수 있다는 점을 지적합니다.
- 측정된 BRDF: 이 논문에서는 ASG를 사용하여 실제 측정된 BRDF를 근사화하는 방법에 대해 자세히 설명합니다. 일부 파라메트릭 모델은 특정 소재에는 적합할 수 있지만, 다른 소재, 특히 고유한 반사 특성을 가진 소재에는 적합하지 않다는 점을 지적합니다. 그러나 ASG는 더 나은 다용도성을 보여줍니다.
    
    ![다른 매개 변수로 Ashikmin BRDFs에 ASGs 및 SGs를 맞추는 비교. 첫 번째 행: nu = 8,nv = 800 (이방성 비율: 10); 두 번째 행: nu = 200,nv = 22 (이방성 비율: 3); 세 번째 행: nu = 20000,nv = 200 (이방성 비율: 10). 각 구성에 대한 L² 오류도 해당 부제에서 주어집니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%207.png)
    
    다른 매개 변수로 Ashikmin BRDFs에 ASGs 및 SGs를 맞추는 비교. 첫 번째 행: nu = 8,nv = 800 (이방성 비율: 10); 두 번째 행: nu = 200,nv = 22 (이방성 비율: 3); 세 번째 행: nu = 20000,nv = 200 (이방성 비율: 10). 각 구성에 대한 L² 오류도 해당 부제에서 주어집니다.
    
- ASG 기반 BRDF의 구형 워핑: 하프 벡터 ℎ와 빛의 방향 측면에서 NDF의 다양한 파라미터화가 주어졌을 때 i에 대한 다양한 매개변수화가 주어지면 BRDF와 입사광의 효율적인 곱셈을 위해 구면 워핑 기법이 도입됩니다. 이는 렌더링 중에 BRDF를 효율적으로 적용하는 데 필수적입니다.

![구형 왜곡](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%208.png)

구형 왜곡

- 가시성 근사치: 가시성, 즉 씬의 오브젝트가 빛을 가리는 방식은 SSDF(Spherical Signed Distance Function)를 사용하여 근사화됩니다. ASG의 경우 가시성 계산에서 이방성을 고려하기 위해 특히 '유효 대역폭'이라는 개념으로 수정 사항이 도입되었습니다.

![[Ngan et al. 2005]에서 실제로 캡처 된 두 개의 BRDF (빗질린 알루미늄 및 보라색 사틴)를 맞춥니다. 우리는 또한 Ashikmin 및 Ward의 매개 변수화 된 이방성 모델을 사용한 적합 결과도 제공합니다. 매개 변수화 된 모델은 로브가 투영에서 벗어난 하단 데이터 (보라색 사틴)를 잘 맞출 수 없습니다. 각 구성에 대한 L² 오류도 해당 부제에서 주어집니다.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%209.png)

[Ngan et al. 2005]에서 실제로 캡처 된 두 개의 BRDF (빗질린 알루미늄 및 보라색 사틴)를 맞춥니다. 우리는 또한 Ashikmin 및 Ward의 매개 변수화 된 이방성 모델을 사용한 적합 결과도 제공합니다. 매개 변수화 된 모델은 로브가 투영에서 벗어난 하단 데이터 (보라색 사틴)를 잘 맞출 수 없습니다. 각 구성에 대한 L² 오류도 해당 부제에서 주어집니다.

이방성 머티리얼은 다양한 각도에서 볼 때 다른 속성을 나타내므로 이를 정확하게 표현하는 것은 사실적인 렌더링에 필수적입니다. ASG는 이를 달성하기 위해 수학적으로 간결하고 효율적인 표현을 제공합니다.

### 6 Applications and Results

이 섹션에서는 두 가지 주요 SG 기반 렌더링 애플리케이션을 구현하여 ASG의 장점을 살펴봅니다:

![가시성 근사의 평가.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%2010.png)

가시성 근사의 평가.

동적 양방향 반사율 분포 함수(BRDF)를 사용한 전 주파수 렌더링 [Wang et al. 2009].
이중 스케일 BRDF 편집 [이와사키 외. 2012a].
이러한 구현은 특정 하드웨어 및 소프트웨어 구성이 있는 PC 설정을 사용하여 테스트되었습니다.

동적 BRDF를 사용한 전 주파수 렌더링:

- 전 주파수 렌더링을 수행하기 위해 확산 용어(Ld)와 정반사 용어(Ls(o))가 모두 평가됩니다. 입사광 L(i)는 ASG 혼합을 사용하여 근사화하며, 확산 항 Ld는 특정 방정식을 사용하여 근사화할 수 있습니다. 스페큘러 항의 경우 ASG 기반 표현을 얻은 다음 그래픽 하드웨어를 사용하여 효율적으로 평가합니다.
- 그 결과는 그림 12와 13에 나와 있습니다:
    
    ![Dish Ball](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%2011.png)
    
    Dish Ball
    
    ![측정 및 공간적으로 변하는 BRDF의 결과.](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%2012.png)
    
    측정 및 공간적으로 변하는 BRDF의 결과.
    
    - 다양한 수준의 비등방성 및 다양한 로컬 프레임이 있는 장면.
    - 측정된 비등방성 BRDF와 공간적으로 변화하는 BRDF가 있는 장면.

바이 스케일 BRDF 편집:

- 이 하위 섹션에서는 SG 기반 이중 스케일 BRDF 편집 방법을 검토합니다[이와사키 외. 2012a]. 대규모 유효 BRDF는 소규모 등방성 BRDF와 효과적인 양방향 가시 정규 분포 함수(BVNDF)의 컨볼루션을 통해 달성됩니다. 이 방법은 ASG 혼합물과 SG 혼합물을 모두 사용하여 표현합니다. 그림 14의 결과는 SG와 비교하여 이방성이 높은 BVNDF를 표현하는 데 있어 ASG가 효과적이라는 것을 보여줍니다.

SG와의 비교:

- 그림 1과 같이 단일 ASG와 다양한 수의 SG를 사용한 렌더링 결과를 비교했습니다. 테스트한 장면은 이방성이 매우 높은 금속 접시로 구성되어 있습니다. 결과는 이러한 애플리케이션에서 SG보다 ASG가 더 우수하다는 것을 보여줍니다.

![천 장면의 이중 규모 BRDF 편집](Anisotropic%20Spherical%20Gaussians%200f539780fd6e41228756ac17eb2ce93b/Untitled%2013.png)

천 장면의 이중 규모 BRDF 편집

이 글에서는 컴퓨터 그래픽 렌더링, 특히 이방성 반사 특성이 있는 장면을 처리할 때 기존 SG에 비해 ASG가 갖는 실질적인 이점을 강조합니다. 이 결과는 특정 애플리케이션에서 ASG가 SG보다 더 효과적이고 효율적일 수 있으며, 더 적은 컴퓨팅 리소스로 더 높은 품질의 렌더링을 구현할 수 있다는 강력한 증거를 제공합니다.

### 7 Discussions and Conclusion

이 글에서는 두 가지 중요한 렌더링 애플리케이션에서 이방성 구면 가우시안(ASG)의 다양한 활용성을 보여줍니다.
새로 파생된 연산자 덕분에 ASG를 기존의 다른 SG 기반 애플리케이션에 쉽게 통합하여 특히 이방성 효과를 처리할 때 확장성을 향상시킬 수 있습니다.
ASG의 실제 구현은 다음과 같이 다양합니다:

- 노멀 맵 필터링을 개선하여 비등방성 NDF를 더 잘 표현합니다.
- 간접 하이라이트 렌더링에서 비등방성 BRDF 처리.
- 실시간 러프 리프랙션에서 이방성 굴절 효과 렌더링.
- 렌더링 외에도 ASG는 함수 근사치 및 비선형 회귀 추정에 일반적으로 사용되는 머신 러닝과 같은 다른 영역으로 적용 범위가 확장됩니다.

결론:

- 이 연구에서는 고유한 이방성 구형 가우시안(ASG) 함수를 소개하고 주요 연산자(적분, 곱셈, 컨볼루션)에 대한 근사 폐쇄형 솔루션을 제공합니다. 이러한 솔루션의 정확도는 정량적으로 검증되었습니다.
- ASG는 SG의 바람직한 특성을 이어받으면서도 고유한 이방성 특성이라는 추가적인 이점을 제공합니다. 이를 통해 이방성 구형 함수를 기존 SG보다 더 효과적이고 효율적으로 표현할 수 있습니다.
- ASG 기반 표현을 사용하여 저자들은 ASG 기반 렌더링 프레임워크를 고안했습니다. 그리고 실제 시나리오에서 ASG의 유용성과 효율성을 강조하기 위해 두 가지 중요한 렌더링 애플리케이션을 구현했습니다.

향후 방향:

- 현재의 연구는 ASG와 SG 사이의 컨볼루션 연산자만 살펴봤습니다. ASG에 대한 포괄적인 이해와 활용성을 확보하기 위해서는 두 ASG를 어떻게 컨볼루션할 수 있는지 파악하는 것이 필수적입니다.
- ASG 분포에서 중요도 샘플링을 달성하는 효율적인 방법을 밝혀내는 것은 오프라인 렌더링 연구에 도움이 될 수 있습니다.

이 논문은 이방성 구형 함수를 효율적으로 표현하여 컴퓨터 그래픽 렌더링을 향상시키는 데 있어 ASG의 잠재력을 강조합니다. 연구 결과는 유망하지만, 특히 두 ASG의 컨볼루션에 대한 추가 탐색을 위한 길을 제시합니다. ASG의 적용 범위가 렌더링을 넘어 머신 러닝과 같은 분야로 확대됨에 따라 그 중요성은 더욱 커질 것으로 예상됩니다.