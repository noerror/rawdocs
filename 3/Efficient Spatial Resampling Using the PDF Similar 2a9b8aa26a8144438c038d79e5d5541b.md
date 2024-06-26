# Efficient Spatial Resampling Using the PDF Similarity

- 용어) Re-sampled Importance Sampling with Temporal and Spatial Reservoirs
    
    ReSTIR(시간 및 공간 저장소를 사용한 재샘플링 임포턴스 샘플링)은 실시간 전역 조명을 위한 렌더링 기법입니다. 특히 복잡한 조명 시나리오나 작은 광원을 다룰 때 레이 트레이스 장면에서 노이즈를 줄이고 이미지 품질을 개선하는 데 사용되는 고급 광원 샘플링 기법입니다.
    
    ReSTIR은 2020년에 Bitterli 등이 도입했으며, 시간적 및 공간적 저장소를 모두 활용하여 씬의 특정 지점에 대한 조명을 효율적으로 샘플링하는 것을 목표로 합니다. 이 방법은 두 가지 주요 단계로 구성됩니다:
    
    1. 저장소 샘플링: ReSTIR은 씬의 각 픽셀에 대한 후보 광원 샘플 저장소를 유지합니다. 새로운 프레임마다 현재 리저버와 새로운 샘플을 결합하여 이상적인 배광 분포를 더 잘 추정합니다. 저장소 샘플링은 렌더링된 이미지의 분산과 노이즈를 줄이는 데 도움이 됩니다.
    2. 시공간 리샘플링: ReSTIR은 공간적 및 시간적 일관성을 모두 활용하여 광원 샘플링 효율성을 개선합니다. 공간적 일관성은 동일한 프레임에서 인접한 픽셀 간의 광 분포의 유사성을 의미하며, 시간적 일관성은 동일한 픽셀에 대한 연속된 프레임 간의 광 분포의 유사성을 의미합니다. 인접 픽셀과 이전 프레임의 광원 샘플을 리샘플링함으로써 ReSTIR은 필요한 광원 샘플의 수를 줄이고 렌더링 성능을 향상시킬 수 있습니다.
    
    특히 광원이 작거나 많은 복잡한 조명 시나리오에서 ReSTIR은 기존의 중요도 샘플링 방식에 비해 이미지 품질과 렌더링 성능이 크게 향상됩니다. 이 방법은 비디오 게임, 가상 현실 등 다양한 실시간 렌더링 애플리케이션에 성공적으로 적용되었습니다.
    
- 용어) Probability Density Function
    
    렌더링 및 컴퓨터 그래픽의 맥락에서 PDF는 확률 밀도 함수를 의미합니다. 확률 밀도 함수는 연속적인 무작위 변수에 대한 다양한 결과의 가능성을 설명하는 수학적 표현입니다. PDF는 조명, 머티리얼 또는 카메라 속성과 같이 본질적으로 어느 정도의 무작위성이 있는 씬의 다양한 측면을 모델링하는 데 사용됩니다.
    
    렌더링에서 PDF는 사실적인 조명과 전역 조명을 시뮬레이션하는 데 널리 사용되는 몬테카를로 방식에서 중요한 역할을 합니다. 몬테카를로 방법은 함수의 적분을 추정하기 위해 무작위 샘플링에 의존하며, PDF는 더 적은 수의 샘플로 더 나은 결과를 얻을 수 있도록 샘플링 프로세스를 안내하는 데 도움이 됩니다.
    
    씬을 렌더링할 때 주요 목표 중 하나는 각 픽셀에 도달하는 빛의 양을 추정하는 것입니다. 이 추정에는 광원의 기여도와 빛과 씬의 머티리얼 간의 상호 작용을 계산하는 작업이 포함됩니다. PDF는 씬의 빛 분포와 반사율 또는 투과율과 같은 머티리얼의 속성을 설명하는 데 사용됩니다.
    
    임포턴스 샘플링은 최종 결과에 더 큰 영향을 미치는 영역에서 더 자주 샘플링하여 몬테카를로 방법의 효율성을 개선하는 데 사용되는 기법입니다. 임포턴스 샘플링은 통합되는 함수와 거의 일치하는 PDF를 생성하여 렌더링된 이미지의 분산과 노이즈를 줄이는 데 도움이 됩니다.
    
    요약하면, PDF는 컴퓨터 그래픽 및 렌더링에서 무작위성을 모델링하고, 샘플링을 안내하며, 몬테카를로 방법 및 임포턴스 샘플링과 같은 렌더링 알고리즘의 효율성과 정확성을 개선하는 데 필수적인 툴입니다.
    

![Untitled](Efficient%20Spatial%20Resampling%20Using%20the%20PDF%20Similar%202a9b8aa26a8144438c038d79e5d5541b/Untitled.png)

그림 1은 바이어스드 ReSTIR 기법을 사용하여 복잡한 장면에서 다양한 렌더링 방법을 비교한 것입니다. 위쪽 이미지는 픽셀당 1.25개의 섀도우 광선(rpp)과 새로운 제거 방법을 사용하여 더 나은 그림자를 생성합니다. 아래 이미지는 광선 수와 제거 방법이 다른 렌더링 결과를 클로즈업하여 새로운 방법으로 개선된 품질을 강조합니다.

ReSTIR은 실시간 렌더링에서 리샘플링된 중요도 샘플링을 위한 후보 샘플의 수를 늘리기 위해 사용되는 기법입니다. 그러나 목표 확률 밀도 함수(PDF)가 적분과 크게 다를 경우 샘플을 재사용하는 것은 비효율적일 수 있습니다. 특히 지오메트리, 재질, 그림자가 다양한 세부 장면에서는 더욱 그렇습니다.

이 백서에서는 직접 조명과 같은 단일 바운스 경로 연결에 대한 PDF 모양의 유사성에 기반한 새로운 제거 방법을 소개합니다. 이 방법은 기존의 제거 방법과 달리 임의의 재질과 그림자 가장자리를 고려합니다. 이 방법은 폰 미제스-피셔 분포와 시간적 리샘플링으로 PDF 모양을 추정함으로써 그림자 가장자리 주변의 오류를 효율적으로 줄일 수 있습니다.

섀도 레이 가시성을 재사용하는 ReSTIR 변형과 함께 새로운 제거 방법을 사용하면 섀도 레이의 수를 줄이면서도 섀도 에지를 보존할 수 있습니다. 그 결과 기존 방식에 비해 더 적은 수의 광선으로 더 높은 품질의 섀도를 구현할 수 있습니다.

1 INTRODUCTION

하드웨어 레이 트레이싱과 몬테카를로 통합은 실시간 애플리케이션과 오프라인 렌더러 모두에서 사용됩니다. 하지만 실시간 프레임 속도를 달성하려면 광선 수를 픽셀당 몇 개로 제한해야 합니다. 이렇게 제한된 광선 수로 고품질 이미지를 렌더링하려면 임포턴스 샘플링이 중요합니다. 시공간 저장소 리샘플링(ReSTIR)은 과거 프레임과 인접 픽셀의 샘플을 재사용하여 후보 샘플을 늘리는 강력한 기술입니다.

지오메트리 에지 및 그림자 에지와 같은 다양한 요인으로 인해 각 픽셀의 타깃 분포가 다르기 때문에 매우 세밀한 장면에서는 ReSTIR의 공간 재사용이 항상 효율적인 것은 아닙니다. 기존의 ReSTIR 방법은 지오메트리 유사성을 기반으로 샘플을 거부했지만 그림자 가장자리와 임의의 재질을 지원하지 않아 편향이 어두워지고 그림자 가장자리가 손실되는 문제가 있었습니다.

이 백서에서는 타깃 확률 밀도 함수(PDF)의 모양 유사성을 기반으로 단일 바운스 경로 연결(예: 직접 조명)에서 공간 재사용을 위한 새로운 제거 방법을 소개합니다. 이 방법은 PDF 유사성을 사용하여 섀도 에지와 머티리얼 경계를 감지할 수 있습니다. 실시간 프레임 속도를 달성하기 위해 이 방법은 폰 미제스-피셔 분포와 템포 리샘플링을 사용하여 PDF 모양을 대략적으로 추정합니다.

이 새로운 방법은 시간적 연속성이 있는 섀도 에지 주변의 오류를 줄이면서 거의 감지할 수 없는 편향성을 도입합니다. 이 방법을 적극적인 가시성 재사용과 함께 편향된 ReSTIR에 적용하면 더 적은 섀도 레이로 고품질 섀도 에지를 렌더링할 수 있습니다.

이 백서의 주요 기여 사항은 다음과 같습니다:

1. ReSTIR의 PDF 모양 유사성에 기반한 제거 방법.
2. 폰 미제스-피셔 분포와 시간적 추정을 사용한 PDF 모양의 대략적인 근사치.
3. 기존 거부 휴리스틱과 프레임 간 시간적 연속성에 기반한 PDF 유사성을 결합한 방식입니다.
4. 가시성 계산 방법이 다른 다양한 ReSTIR 변형에 대한 이 방법의 효과 입증.

2 BACKGROUND

2.1 Related Work

최근의 리샘플링 기법은 2패스 샘플링 알고리즘을 사용하여 목표 분포에 따라 샘플을 생성하는 샘플링 중요도 리샘플링(SIR)을 기반으로 합니다. 재샘플링된 중요도 샘플링(RIS)은 몬테카를로 적분을 위해 SIR에 의해 선택된 샘플의 분포를 정규화합니다. 시공간 저장소 리샘플링(ReSTIR)은 RIS를 기반으로 픽셀과 프레임에 걸쳐 샘플을 재사용합니다.

ReSTIR은 직접 조명, 전역 조명 및 경로 추적을 위해 다양한 개선과 확장을 거쳤습니다. 그러나 실시간 렌더링에서 분산과 편향을 줄이려면 유사한 픽셀만 재사용하는 것이 필수적입니다. 이전에는 지오메트리, 러프니스 파라미터 또는 경로 연결성의 유사성을 기반으로 유사하지 않은 픽셀을 거부하는 접근 방식을 사용했지만 이러한 접근 방식은 단일 바운스 경로 연결에서 섀도 에지 및 임의의 머티리얼을 고려하지 않습니다.

이 백서에서는 단일 바운스 경로 연결에서 그림자 가장자리와 임의의 머티리얼을 처리하기 위해 타깃 확률 밀도 함수(PDF) 모양의 유사성을 사용하는 새로운 제거 방법을 소개합니다. 이 방법은 PDF 모양의 유사성을 고려함으로써 고품질의 그림자와 머티리얼 경계를 유지하면서 향상된 실시간 렌더링 성능을 제공할 수 있습니다.

2.2 Algorithm of ReSTIR

시공간 저장소 리샘플링(ReSTIR)은 2패스 샘플링 알고리즘을 사용하여 소스 확률 밀도 함수(PDF)에 따라 후보 샘플을 생성하는 재샘플링된 중요도 샘플링(RIS)을 기반으로 합니다. RIS는 몬테카를로 적분을 위해 이 방법으로 선택한 샘플의 분포를 정규화합니다. ReSTIR은 각 픽셀에 저장된 시공간적 인접 샘플을 재사용하여 후보 샘플 수를 늘리고 효율성을 개선함으로써 RIS를 확장합니다.

효율성을 더욱 향상시키기 위해 ReSTIR은 지오메트리 유사성과 같은 휴리스틱을 사용하여 서로 다른 픽셀을 재사용하지 못하도록 거부합니다. 인접한 픽셀은 세부적인 지오메트리, 다른 재질, 그림자 가장자리를 가질 수 있으므로 유사하지 않은 픽셀을 거부하는 것은 특히 공간 재사용에서 오류를 줄이는 데 매우 중요합니다. 조명이 지속적으로 변경될 때는 프레임 간 차이보다 픽셀 간 차이가 더 클 수 있습니다.

이 백서에서는 단일 바운스 경로 연결에서 그림자 가장자리와 임의의 머티리얼을 처리하기 위해 대상 PDF 모양의 유사성을 고려하는 공간 재사용을 위한 새로운 거부 휴리스틱을 소개합니다. 이 방법은 고품질 섀도와 머티리얼 경계를 유지하면서 실시간 렌더링 성능을 향상시키는 데 도움이 됩니다.

3 OUR REJECTION METHOD FOR RESTIR

시공간 저장소 리샘플링(ReSTIR)은 2패스 샘플링 알고리즘을 사용하여 소스 확률 밀도 함수(PDF)에 따라 후보 샘플을 생성하는 재샘플링된 중요도 샘플링(RIS)을 기반으로 합니다. RIS는 몬테카를로 적분을 위해 이 방법으로 선택한 샘플의 분포를 정규화합니다. ReSTIR은 각 픽셀에 저장된 시공간적 인접 샘플을 재사용하여 후보 샘플 수를 늘리고 효율성을 개선함으로써 RIS를 확장합니다.

효율성을 더욱 향상시키기 위해 ReSTIR은 지오메트리 유사성과 같은 휴리스틱을 사용하여 서로 다른 픽셀을 재사용하지 못하도록 거부합니다. 인접한 픽셀은 세부적인 지오메트리, 다른 재질, 그림자 가장자리를 가질 수 있으므로 유사하지 않은 픽셀을 거부하는 것은 특히 공간 재사용에서 오류를 줄이는 데 매우 중요합니다. 조명이 지속적으로 변경될 때는 프레임 간 차이보다 픽셀 간 차이가 더 클 수 있습니다.

이 백서에서는 단일 바운스 경로 연결에서 그림자 가장자리와 임의의 머티리얼을 처리하기 위해 대상 PDF 모양의 유사성을 고려하는 공간 재사용을 위한 새로운 거부 휴리스틱을 소개합니다. 이 방법은 고품질 섀도와 머티리얼 경계를 유지하면서 실시간 렌더링 성능을 향상시키는 데 도움이 됩니다.

3.1 Resampling with Similar PDFs

ReSTIR은 많은 후보 샘플에 대해 기여 가중치 𝑊𝑋를 1/𝑝¯𝑠(𝑋)로 수렴하여 오류를 줄이는 것을 목표로 합니다. ReSTIR이 수렴할 때 정규화된 대상 PDF의 비율이 대략 1과 같으면 작은 오류가 발생할 수 있습니다. 이 논문에서는 그림자와 임의의 재질을 고려하는 거부 휴리스틱에 정규화된 PDF의 유사성을 사용할 것을 제안합니다.

단일 바운스 경로 연결의 경우 이 방법은 광원 방향 영역을 사용하여 표현됩니다. 음영 포인트가 가깝고 조명이 정적인 경우 그림자 가장자리와 머티리얼 경계로 인해 구형 PDF 𝑝¯𝑖(𝛚)와 𝑝¯𝑠(𝛚)의 모양이 서로 다를 수 있습니다. 저자는 광선 방향 영역에서 PDF 𝑝¯𝑖 (𝛚)와 𝑝¯𝑠 (𝛚) 사이의 모양 유사성을 평가하여 유사한 픽셀이 재사용되도록 하여 ReSTIR 알고리즘의 효율성을 개선하고 오류를 줄였습니다.

4절에 언급된 실험 결과는 이 방법이 효율성과 오류를 개선한다는 것을 보여줍니다. 4에 언급된 실험 결과는 이 방법이 성능과 품질 간의 균형을 유지하면서 효과적으로 오류를 줄인다는 것을 보여줍니다.

3.2 Similarity Computation for Spherical PDFs

3.2.1 vMF 근사치. 실제로 목표 PDF 𝑝¯𝑠(𝛚)의 정확한 모양을 얻는 것은 어렵습니다. 따라서 정규화된 구형 가우시안이라고도 하는 S²의 폰 미제스-피셔(vMF) 분포를 사용하여 PDF를 근사화합니다. vMF 분포는 로브 축 𝛍𝑠와 선명도 𝜅𝑠로 특징지어집니다. vMF 근사치를 얻기 위해 실시간 프레임 속도에서 평균 방향 v´𝑠를 추정합니다.

3.2.2 평균 방향의 시간적 추정. 단일 바운스 경로 연결의 경우 구형 적분 v´𝑠을 경로 공간 적분으로 다시 작성할 수 있습니다. 목표 분포 𝑝ˆ𝑠(𝑋)에 따라 빛의 방향 𝛚𝑋을 리샘플링하여 평균 방향 v´𝑠을 추정하기 위해 ReSTIR의 바이어스된 변형을 사용합니다. 평균 방향 추정에서는 기존의 편향된 ReSTIR 방법과 유사하게 시간에 따른 가시성을 재사용합니다. 섀도 에지와 머티리얼 경계를 보존하기 위해 시간적으로 인접한 픽셀만 재사용하고 공간적으로 인접한 픽셀은 재사용하지 않습니다.

3.2.3 vMF의 유사성. 각 픽셀에 대한 vMF 분포를 추정하고 나면 유사도를 계산합니다. PDF의 중첩을 평가하기 위해 곱 적분 기반 유사도를 사용합니다. PDF가 델타 함수인 고주파 PDF의 경우 스무딩 커널 𝑔(𝛚′; 𝛚, 𝛼)를 사용하여 각 PDF를 스무딩합니다. 그런 다음 Tokuyoshi [2015]에서 도출된 분석 곱 적분 기반 유사도를 사용하여 픽셀 간 평활화된 vMF의 유사도를 계산합니다.

이 PDF 유사도(ℎour ∈ [0, 1])를 사용하면 추정된 vMF의 오차가 작은 경우 섀도 에지 및 머티리얼 경계에서 공간 재사용을 방지할 수 있습니다. 이 접근 방식을 사용하면 최종 렌더링된 이미지에서 그림자 가장자리와 머티리얼 경계의 품질을 유지하면서 평균 방향을 효율적으로 근사하고 추정할 수 있습니다.

3.3 Combination with Existing Heuristics

이 섹션에서는 평균 방향의 시간적 추정으로 인한 분산 문제와 PDF 유사도 분산과 조명 간의 상관관계에 대해 설명합니다. 이러한 상관관계는 공간 재사용을 거부하는 편향성을 초래할 수 있으며, 이는 시간적 불일치 및 빠르게 움직이는 조명으로 인해 후보 샘플 수가 적을 때 두드러질 수 있습니다.

이 문제를 해결하기 위해 저자는 후보 샘플 수가 충분한 경우에만 PDF 유사성 휴리스틱을 사용할 것을 제안합니다. 이들은 시간적으로 누적된 후보 개수를 사용하여 휴리스틱(ℎour)과 기존 휴리스틱(ℎprev)을 보간합니다. 이렇게 하면 시간적으로 연속적인 픽셀과 정적이거나 천천히 움직이는 조명에 대해서만 휴리스틱이 효과적입니다.

저자들은 방정식 (15)와 (16)을 도입하여 평균 방향 추정, 초기 후보 수, 시간적 재사용을 위한 최대 후보 수에 대해 누적된 후보 수를 기반으로 결합된 휴리스틱(ℎ)과 보간 계수(𝑡)를 계산합니다. 평균 방향 추정에 대한 기여 가중치가 0이면 빛의 방향이 무한정이며, 이 경우 기존 휴리스틱만 사용합니다.

이 결합된 휴리스틱은 PDF 유사도와 조명 간의 분산 및 상관관계의 영향을 줄여 결과의 전반적인 품질을 개선하는 데 도움이 됩니다.

4 EXPERIMENTAL RESULTS

이 섹션에서는 몇 가지 ReSTIR 변형에 대해 이전 지오메트리 기반 제거 방법[Bitterli 외. 2020]과 비교한 제거 방법의 결과를 제시합니다. 이 방법들은 DirectX 레이트레이싱을 사용하여 Microsoft MiniEngine에서 구현하고 AMD Radeon™ RX 6900 XT GPU에서 1920x1080 해상도로 이미지를 렌더링합니다. 이미지 품질은 대칭 평균 절대 백분율 오류(SMAPE) 메트릭을 사용하여 평가합니다.

저자는 다양한 시나리오에서 이 방법을 테스트합니다:

픽셀당 두 개의 광선을 사용하는 ReSTIR: 이 방법은 시간적으로 연속된 픽셀의 그림자 가장자리에 대한 편향과 편차를 모두 줄이고 시간적으로 연속된 광택 하이라이트 주변의 편차를 줄입니다. 이 방법의 총 오버헤드는 약 0.2밀리초입니다.

픽셀당 하나의 광선을 사용하는 ReSTIR: 이 제거 방법은 픽셀당 두 개의 광선을 추적하지 않고 시간적으로 연속된 픽셀에 대한 하드 컨택트 섀도를 얻습니다. 오버헤드는 약 0.3밀리초입니다.

적응형 광선 추적을 사용한 ReSTIR: 시간적으로 연속적인 픽셀에 대해 이전 방법보다 하드 컨택 섀도를 더 잘 보존하는 제거 방법입니다. 또한 거리 기반 접근 방식에 더해 확률 1-𝑡(여기서 𝑡는 식 16에서 주어진 시간적 연속성)에 따라 광선을 확률적으로 추적하므로 더욱 세밀하고 시간적으로 일관된 섀도를 생성합니다.

정확한 가시성 테스트가 포함된 ReSTIR: 이 제거 방법은 실험에서 0.5밀리초의 오버헤드로 편차를 줄였습니다. 유사도 계산과 조명 추정 간의 분산 상관관계로 인한 편향은 눈에 띄지 않을 정도로 작습니다.

동적 간접 조명: 이 방법은 VPL을 사용하는 직접 조명뿐만 아니라 간접 조명에도 적용할 수 있습니다. 제거 휴리스틱은 정적이거나 천천히 움직이는 조명에만 효과적입니다.

요약하면, 저자들은 그들의 제거 방법이 다양한 ReSTIR 시나리오에서 렌더링된 이미지의 품질을 개선하여 편향과 편차를 줄이고 더 높은 품질의 그림자와 광택 있는 하이라이트를 제공한다는 것을 보여줍니다. 이 방법의 오버헤드는 상대적으로 적기 때문에 실시간 애플리케이션에 사용하기에 적합합니다.

5 LIMITATIONS

이 섹션에서는 저자들이 거부 방법의 한계와 향후 작업 가능성에 대해 논의합니다:

편향 Bias: 거부 휴리스틱은 샘플의 상관 관계로 인해 편향이 발생할 수 있습니다. 시간적으로 연속적인 픽셀의 경우 편향이 무시할 수 있을 정도로 작지만, 이 방법은 여전히 일관되지 않은 추정 방법입니다. 다른 초기 샘플을 사용하여 샘플을 연관시키는 것이 도움이 될 수 있지만, 이 경우 계산 오버헤드가 증가합니다.

시간적 불연속성 Temporal discontinuities: 거부 휴리스틱은 시간적 연속성에 대해서만 작동하므로 움직임이 있는 동안에는 그다지 효과적이지 않을 수 있습니다. 저자는 애니메이션 장면에 시공간 적응형 광선 추적을 사용하여 픽셀당 더 적은 광선을 사용하면서 그림자 가장자리를 보존할 것을 제안합니다.

오탐 False positives: 이 방법은 서로 다른 PDF를 동일한 vMF 로브로 근사화할 수 있지만 이 문제는 실험 결과에서 문제가 될 만큼 자주 발생하지는 않습니다.

다중 바운스: 이 방법은 PDF가 구형 PDF 시퀀스의 곱인 다중 바운스를 지원하지 않습니다. 다중 바운스 조명을 위해 이를 확장하는 것은 향후 작업으로 남겨져 있습니다.

메모리 오버헤드: 이 방식은 저장소를 메모리에 저장하기 때문에 메모리 전송 비용이 발생합니다. 저장소 데이터 크기를 줄이는 것은 향후 작업의 잠재적인 영역입니다.

고광택 표면: PDF 유사도 추정은 보기 방향이 변경될 때 광택이 높은 표면에 대해 시간적 지연이 발생합니다. 이 지연을 줄이기 위해 저자는 분석적 광택 로브 유사성을 추가하거나 들어오는 복사광과 BSDF를 PDF에서 분리할 것을 제안합니다. 이러한 접근 방식의 효율성을 조사하는 것은 향후 연구의 잠재적 영역입니다.

저자는 이러한 한계를 인정하고 이를 해결하기 위한 잠재적인 해결책과 향후 작업을 제안하여 궁극적으로 다양한 시나리오에서 방법의 성능과 적용 가능성을 향상시킵니다.