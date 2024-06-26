# What You See is What You GAN: Rendering Every Pixel for High-Fidelity Geometry in 3D GANs

[https://arxiv.org/abs/2401.02411](https://arxiv.org/abs/2401.02411)

- Jan 2024

![왼쪽: 결과. 가운데의 분할 보기는 2D 렌더링과 해당 3D 지오메트리 간의 높은 일치도를 보여줍니다. 이 방법은 멀티뷰나 3D 스캔 데이터 없이도 2D 이미지에 기하학적으로 잘 정렬된 세밀한 3D 디테일(예: 안경테, 고양이 털)을 학습할 수 있습니다. 오른쪽: EG3D [7]와 비교. 유니티의 타이트한 SDF 선행은 얼굴과 모자의 부드럽고 디테일한 표면을 제공하는 반면, EG3D는 지오메트리 아티팩트와 지오메트리와 렌더링 간의 불일치를 나타냅니다. 더 많은 예시는 그림 5와 첨부된 비디오를, 다른 기준선과의 비교는 그림 6을 참조하세요.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled.png)

왼쪽: 결과. 가운데의 분할 보기는 2D 렌더링과 해당 3D 지오메트리 간의 높은 일치도를 보여줍니다. 이 방법은 멀티뷰나 3D 스캔 데이터 없이도 2D 이미지에 기하학적으로 잘 정렬된 세밀한 3D 디테일(예: 안경테, 고양이 털)을 학습할 수 있습니다. 오른쪽: EG3D [7]와 비교. 유니티의 타이트한 SDF 선행은 얼굴과 모자의 부드럽고 디테일한 표면을 제공하는 반면, EG3D는 지오메트리 아티팩트와 지오메트리와 렌더링 간의 불일치를 나타냅니다. 더 많은 예시는 그림 5와 첨부된 비디오를, 다른 기준선과의 비교는 그림 6을 참조하세요.

### 1 Introduction

최근에는 2차원 이미지의 풍부한 자원을 활용하여 3차원 객체의 표현을 생성하는 3D 생성 모델 트레이닝이 주목받고 있습니다. 이러한 방법 중, 3D 인식 생성적 적대 신경망, 즉 3D GANs는 2차원 이미지 집합으로부터 감독 없이 3차원 표현을 학습하는 강력한 방법으로 부상하고 있습니다. 이러한 방법들은 적대적 훈련을 통해 렌더링된 3D 장면과 2D 데이터를 비교하는 차별화된 렌더링을 사용합니다.

특히, 신경 복사 분야(Neural Radiance Fields, NeRF)는 최근 성공적인 3D GANs 사이에서 인기 있는 선택이 되었습니다. 그러나 체적 렌더링의 상당한 계산 및 메모리 비용 때문에, 3D GANs는 고해상도 출력으로 확장하는 데 어려움을 겪고 있습니다. 예를 들어, 512x512 이미지 한 장을 생성하기 위해 체적 렌더링을 사용하면, 중요 샘플링을 사용할 때 광선 당 96개의 깊이 샘플을 사용하여 최대 2천5백만 개의 깊이 샘플을 평가해야 할 수도 있습니다.

훈련 과정에서는 모든 중간 작업을 GPU 메모리에 저장해야 하며, 이는 역방향 패스에 대해 각 깊이 샘플마다 필요합니다. 따라서 기존 방법들은 패치 작업이나 저해상도 신경 렌더링과 2D 초고해상도 처리의 조합을 사용하고 있습니다. 그러나 패치 기반 방법은 장면에 대한 제한된 수용 필드를 가지고 있어 만족스럽지 못한 결과를 초래하며, 저해상도 렌더링과 SR 스킴의 조합은 다시 보기 일관성과 3D 기하학의 정확성을 희생합니다.

본 연구에서는 각 픽셀을 명시적으로 렌더링함으로써 신경 체적 렌더링을 고해상도로 확장하는 도전을 해결합니다. 즉, "2D에서 보는 것이 3D에서 얻는 것"이라는 원칙을 확립하며, 전례 없는 수준의 기하학적 세부 사항과 엄격한 다시 보기 일관성 있는 이미지를 생성합니다. 이를 위해 다음과 같은 기여를 제공합니다:

1. 고주파수 기하학을 표현하기 위한 SDF 기반 3D GAN을 도입합니다. 이는 훈련 과정에서 점차적으로 증가하는 공간적으로 변화하는 표면 긴밀도를 통해 적은 샘플 렌더링을 용이하게 합니다.
2. 훈련 중 전체 해상도 렌더링을 처음으로 가능하게 하는 일반화된 학습 샘플러를 제안합니다.
3. 상당히 적은 깊이 샘플을 사용하여 안정적인 신경 렌더링을 생성하는 견고한 샘플링 전략을 보여줍니다. 우리의 샘플러는 기존 3D GANs가 적어도 96개의 샘플을 사용해야 하는 것과 달리 광선당 단 20개의 샘플로 운영될 수 있습니다.

이러한 기여를 통해, 본 연구는 SR 기준과 동등한 품질로 렌더링하면서 3D GANs의 최첨단 기하학을 달성합니다. 더 많은 결과는 그림 5를, 다른 기준과의 비교는 그림 6을 참조해 주십시오.

### 2. Related Work

우리는 먼저 3D 생성 모델의 선행 연구들과 그들의 현재 한계점들을 검토합니다. 그 후, 3D 기하학 표현과 신경 렌더링을 위한 기초 기술들을 살펴보며, 이러한 기술들로부터 영감을 받습니다. 그리고 신경 체적 렌더링의 가속화를 위한 기존 방법들을 논의합니다.

![EG3D 256 모델의 샘플. 오른쪽: 투패스 중요도 샘플링[38]을 사용하여 광선당 48개의 거친 샘플과 48개의 미세 샘플로 볼륨 렌더링하면 언더샘플링이 발생하여 눈에 띄게 노이즈가 많은 아티팩트가 발생합니다. 왼쪽: 이러한 아티팩트는 초해상도(SR)로 복구됩니다. 프레젠테이션 목적으로 확대된 뷰에 선명하지 않은 마스크가 적용되었습니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%201.png)

EG3D 256 모델의 샘플. 오른쪽: 투패스 중요도 샘플링[38]을 사용하여 광선당 48개의 거친 샘플과 48개의 미세 샘플로 볼륨 렌더링하면 언더샘플링이 발생하여 눈에 띄게 노이즈가 많은 아티팩트가 발생합니다. 왼쪽: 이러한 아티팩트는 초해상도(SR)로 복구됩니다. 프레젠테이션 목적으로 확대된 뷰에 선명하지 않은 마스크가 적용되었습니다.

2.1 3D 생성 모델

2D GAN과 마찬가지로, 3D 인식 GAN은 2D 이미지의 집합으로부터 학습하지만, 3D 표현과 차별화된 렌더링을 사용하여 다시 보기 이미지나 실제 3D 스캔 없이 3D 장면을 학습합니다. 가장 성공적인 연구들 중 일부는 신경 필드와 특성 그리드를 조합하여 3D 표현으로 사용하고, 신경 체적 렌더링을 차별화된 렌더러로 사용합니다. 그러나 신경 체적 렌더링의 상당한 메모리 및 계산 비용 때문에, 많은 이전 연구들은 저해상도에서 렌더링을 수행하고 2D 후처리 CNN에 의존하여 뷰 불일치 방식으로 고주파수 세부 사항을 창출합니다.

엄격한 3D 일관성을 보장하기 위해, 다른 이전 연구들은 고해상도에서 렌더링을 시도하고 높은 계산 비용을 해결하기 위한 기술들을 제안합니다. 한 방향의 연구는 3D 장면의 희소한 특성을 활용하여 렌더링을 가속화합니다. 하지만 이러한 희소 표현은 제한된 다양성과 시야 각도를 가진 생성 장면에 대한 제약을 가집니다. 반면, 우리의 샘플 기반 방법은 새로운 장면마다 일반화되고 광선별로 렌더링을 가속화합니다.

최근에는 확산 모델을 사용하는 새로운 생성 접근법이 제안되었습니다. 이러한 3D 인식 확산 모델은 3D 장면 생성을 학습하기 위해 신경 필드 표현과 2D 이미지 제거 목표를 결합합니다. 하지만 확산 모델은 반복적인 특성과 장면별 최적화가 필요한 비용 때문에 상당한 계산 비용을 발생시킵니다.

2.2 고신뢰도 기하학 학습

3D GAN에 관한 이전 작업들은 일반적으로 방사선 필드를 기하학적 표현으로 사용했으며, 이는 필드 내에서 표면 기하학이 어디에 위치하는지 명확하지 않아 울퉁불퉁한 표면을 만듭니다. 다른 작업들은 신경 체적 렌더링과 함께 사용될 수 있는 암시적 표면을 기반으로 한 대체 표현을 제안했습니다. 우리는 VolSDF를 기반으로 한 암시적 표면 표현을 사용하고, Adaptive Shells와 유사한 공간적으로 변화하는 매개변수를 활용하여 표면의 부드러움을 제어합니다.

2.3 신경 체적 렌더링 가속화

섹션 2.1에서 언급했듯이, 3D GAN의 가속화는 일반적으로 가속 구조를 사용하여 이루어집니다. 하지만 우리는 가속 구조에 의존하지 않는 방법 클래스를 살펴봅니다. 우리는 밀도 추정기에 대한 연구에서 영감을 받아 장면 조건부 제안 네트워크를 학습하여 장면별로 일반화하는 방법을 제안합니다. 또한 더 효율적인 표현을 사용하여 렌더링을 가속화하는 다른 방법들도 있습니다. 우리의 샘플링 전략은 어떠한 NeRF 표현에도 사용될 수 있습니다.

### 3. Background

우리 연구는 최신의 3D 인식 GAN 방법론에 대한 배경 지식으로 시작합니다. 우리의 방법은 유사한 기본 구조를 활용하여 3D 표현으로 매핑하는 데 중점을 둡니다. 3D GAN은 일반적으로 StyleGAN과 유사한 구조를 사용하여, 단순한 가우시안 사전으로부터 NeRF의 조건을 매핑합니다. 우리는 이들 중 특히 고 표현력과 효율성을 자랑하는 삼축 평면(triplane) 조건을 상속받습니다. 이는 세 축에 정렬된 2D 특징 그리드를 통해 NeRF 조건을 직교 투영 및 보간으로 제공합니다.

![여기에서는 제안된 파이프라인과 그 중간 결과물을 보여줍니다. 삼면체 T에서 시작하여 균일한 샘플을 추적하여 장면을 프로빙하여 저해상도 I128과 가중치 P128을 산출합니다. 이를 고해상도 제안 가중치 Pˆ512를 생성하는 CNN에 공급합니다(가중치는 균일한 레벨 세트로 시각화됨). 강력한 샘플링과 볼륨 렌더링을 수행하여 최종 이미지 I512와 표면 분산 B를 얻습니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%202.png)

여기에서는 제안된 파이프라인과 그 중간 결과물을 보여줍니다. 삼면체 T에서 시작하여 균일한 샘플을 추적하여 장면을 프로빙하여 저해상도 I128과 가중치 P128을 산출합니다. 이를 고해상도 제안 가중치 Pˆ512를 생성하는 CNN에 공급합니다(가중치는 균일한 레벨 세트로 시각화됨). 강력한 샘플링과 볼륨 렌더링을 수행하여 최종 이미지 I512와 표면 분산 B를 얻습니다.

고신뢰도 기하학을 생성하기 위해, 우리 방법은 VolSDF에 기반을 두고 있습니다. 이는 점 x의 불투명도 σ를 직접 출력하는 대신, SDF 값 s를 출력하고 이를 라플라스 CDF를 사용하여 σ로 변환합니다. SDF 기반 표현의 하나의 분명한 장점은 표면 추출의 용이함입니다.

삼축 평면 조건과 방정식 1을 사용하여, 우리는 각 볼륨 내의 점에 불투명도 σ와 복사도 c를 할당할 수 있습니다. 주어진 카메라 광선에 대해, 우리는 볼륨 렌더링 적분을 근사화합니다. 이는 컴퓨터 그래픽스의 중요 샘플링 기술을 사용하여 적은 샘플로도 더 효율적인 추정자를 개발하는 것이 가능하게 합니다.

토론: 우리는 훈련 중에 무작위 숫자를 층화하여 이전 작업들, 예를 들어 NeRF와 EG3D를 개선합니다. 이는 특히 샘플 수가 적을 때 렌더링 분산을 상당히 줄이는 데 도움이 됩니다. 또한, 고해상도에서 중요 샘플링을 위한 좋은 분포를 예측하는 신경 방법을 개발했습니다. 이는 광선을 철저히 살펴봐야 할 필요 없이 중요한 샘플링을 위한 좋은 분포를 예측하는 신경 방법을 개발했습니다.

### 4. Method

이 섹션에서는 SDF 기반의 NeRF 파라미터화(4.1절)부터 시작하여, 우리의 방법론을 설명합니다. 우리는 먼저 저해상도에서 3D 장면을 탐색하고(4.2절), 그 후 고해상도 CNN 제안 네트워크(4.3절)를 사용하며, 마지막으로 결과 제안에 대한 견고한 샘플링 방법(4.4절)을 소개합니다. 그 다음으로는 안정적인 훈련을 위한 규제 방법들(4.5절)을 설명하고, 마지막으로 전체 훈련 파이프라인(4.6절)을 논의합니다.

4.1. 3D 표현으로의 매핑

잡음 벡터 z에서 시작하여, 우리는 초기 트리플레인 T'을 StyleGAN 계층을 사용하여 합성합니다. 우리는 추가 합성 블록을 사용하여 더 표현력 있는 트리플레인 특성 T를 생성합니다. 이러한 설계 선택은 각각의 평면에 대한 특성에 주목할 수 있도록 합니다.

4.2. 고해상도 제안 네트워크

우리는 이제 잠재 코드 z에서 3D NeRF 표현으로의 매핑을 가지고 있습니다. 하지만 NeRF의 고해상도 체적 렌더링은 많은 샘플과 많은 메모리 및 시간을 요구합니다. 우리는 고해상도에서의 무지개 밀도 샘플링 대신, 저해상도 렌더링을 사용하여 3D 표현을 저렴하게 탐색하고 고해상도에서 제안 분포를 생성합니다.

4.3. 제안 네트워크 감독

제안 네트워크의 입력과 출력을 설명한 후, 이제 그 감독 방법을 보여줍니다. 목표 카메라에서, 우리는 고해상도에서 작은 64×64 패치에 대해 192개의 거친 샘플을 추적하여 체적 렌더링 가중치의 실제 텐서를 생성합니다. 우리는 이 텐서를 교차 엔트로피 손실로 비교하여 감독합니다.

![오른쪽 이미지의 녹색 픽셀에 대한 볼륨 렌더링 PDF를 샘플링 방법과 함께 시각화합니다. 파란색의 기준 실측 데이터 분포는 불연속적인 깊이로 인해 바이모달입니다. 계층화를 적용하지 않으면 예측된 노란색 PDF의 샘플이 두 번째 모드를 완전히 놓칩니다. 계층화는 분산을 줄이지만 두 번째 모드도 놓칩니다. 강력한 계층화 샘플은 부정확한 예측에도 불구하고 두 가지 모드를 모두 맞춥니다. 감독 PDF도 보라색으로 시각화됩니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%203.png)

오른쪽 이미지의 녹색 픽셀에 대한 볼륨 렌더링 PDF를 샘플링 방법과 함께 시각화합니다. 파란색의 기준 실측 데이터 분포는 불연속적인 깊이로 인해 바이모달입니다. 계층화를 적용하지 않으면 예측된 노란색 PDF의 샘플이 두 번째 모드를 완전히 놓칩니다. 계층화는 분산을 줄이지만 두 번째 모드도 놓칩니다. 강력한 계층화 샘플은 부정확한 예측에도 불구하고 두 가지 모드를 모두 맞춥니다. 감독 PDF도 보라색으로 시각화됩니다.

4.4. 제안 네트워크에서의 샘플링

고해상도 분포를 예측하고 훈련하는 방법을 보여준 후, 이제 결과 제안을 샘플링하는 방법을 개요합니다. 이 과정은 언어처리에서 사용되는 핵심 샘플링과 유사합니다. 우리는 각각의 이산 예측 PDF에 대해 가장 작은 확률 집합을 계산합니다.

4.5. 고해상도 훈련을 위한 규제

우리는 광선당 낮은 샘플 예산으로 정확하게 렌더링하기 위해 표면이 타이트해야 한다는 것을 원합니다. 이를 위해, 우리는 공간적으로 변화하는 β 값에 대한 규제를 도입합니다.

4.6. 훈련 파이프라인

학습된 샘플러를 사용하여 고해상도 렌더링을 시작하기 위해, 우리는 좋은 NeRF 표현을 훈련하는 것과 동시에 좋은 샘플러가 필요합니다. 초기 훈련 단계에서, 우리는 표준 NeRF 샘플링 기법을 통해 저해상도 3D GAN을 먼저 학습합니다. 그 후, 샘플러 훈련을 도입하고, 더 높은 해상도로 넘어가면서 모든 손실을 도입한 후, 생성기 G의 매개변수를 최적화합니다.

### 5. Results

우리는 표준 3D GAN 데이터셋인 FFHQ와 AFHQv2 Cats를 사용하여 벤치마킹을 진행합니다. 두 데이터셋 모두 512×512 해상도에서 사용되며, EG3D에서 추출된 카메라 파라미터를 조건부 렌더링에 사용합니다. 우리의 AFHQ 모델은 적응형 데이터 증강을 사용하여 FFHQ 모델에서 미세 조정되었습니다.

5.1 비교 연구

우리는 EG3D, MVCGAN, StyleSDF와 같은 최신 3D GAN 방법들과 비교합니다. 이들은 저해상도 신경 렌더링과 2D 후처리 CNN 초고해상도를 사용합니다. 그리고 Mimic3D, Epigraf, GMPI, GramHD와 같이 전적으로 신경 렌더링에 기반한 방법들과도 비교합니다.

![FFHQ와 AFHQ에서 선별된 샘플. 이 방법은 지오메트리와 노멀 맵에서 볼 수 있듯이 충실도가 높은 지오메트리(예: 안경)와 세밀한 디테일(예: 수염, 고양이의 털)을 해결할 수 있습니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%204.png)

FFHQ와 AFHQ에서 선별된 샘플. 이 방법은 지오메트리와 노멀 맵에서 볼 수 있듯이 충실도가 높은 지오메트리(예: 안경)와 세밀한 디테일(예: 수염, 고양이의 털)을 해결할 수 있습니다.

**질적 결과**: 우리 방법으로 생성된 샘플들은 사실적 렌더링과 고해상도 세부 사항을 보여줍니다. 특히, EG3D는 중요한 아티팩트를 보여주며 고해상도 기하학을 해결하지 못합니다. 반면, 우리 방법은 고신뢰도 3D 형상과 고해상도 세부 사항을 제공합니다.

![EG3D [7], Mimic3D [10], Epigraf [53]를 사용한 FFHQ의 질적 비교. EG3D는 128×128 해상도로 뉴럴 렌더링을 수행하며 4배의 초해상도를 사용하여 이미지를 생성합니다. 오른쪽의 Mimic3D와 Epigraf는 뉴럴 렌더링을 통해 이미지를 직접 생성합니다. 다른 모든 기준선은 광선당 최대 192개의 고밀도 깊이 샘플을 사용하는 반면, 우리의 방법은 광선당 30개의 샘플로 작동할 수 있습니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%205.png)

EG3D [7], Mimic3D [10], Epigraf [53]를 사용한 FFHQ의 질적 비교. EG3D는 128×128 해상도로 뉴럴 렌더링을 수행하며 4배의 초해상도를 사용하여 이미지를 생성합니다. 오른쪽의 Mimic3D와 Epigraf는 뉴럴 렌더링을 통해 이미지를 직접 생성합니다. 다른 모든 기준선은 광선당 최대 192개의 고밀도 깊이 샘플을 사용하는 반면, 우리의 방법은 광선당 30개의 샘플로 작동할 수 있습니다.

**양적 결과**: FID와 특정 얼굴에 대한 정규 FID(FID-N)을 사용하여 이미지 품질을 측정합니다. 우리의 결과는 신경 렌더링만을 사용하는 방법들 중 최고의 이미지 품질을 보여주며, EG3D와 같은 최신 SR 기반 방법과 비교할 수 있는 FID를 달성합니다.

5.2 소거 연구

우리의 표면 긴밀도 정규화 없이는 SDF 표면이 흐려지며, 이는 더 나쁜 기하학적 점수를 초래합니다. 또한, 샘플러나 샘플링 중 계층화 없이는 모델이 제한된 깊이 예산으로 의미 있는 3D 기하학을 학습할 수 없으며, 퇴화된 3D 기하학을 생성합니다. 우리의 견고한 샘플링 전략 없이는 샘플링이 더 취약해지며, 이는 FID 저하와 가끔 부유하는 기하학적 아티팩트를 생성합니다.

![베타 정규화의 효과에 대한 제거 연구](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%206.png)

베타 정규화의 효과에 대한 제거 연구

![제거 연구를 위한 정성적 비교](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%207.png)

제거 연구를 위한 정성적 비교

### 6. Discussion

**한계점 및 향후 연구**. 우리의 방법은 3D 기하학 생성에서 상당한 개선을 보여주지만, 아직도 반사 표면이 있는 경우 찌그러짐과 같은 아티팩트가 발생할 수 있으며, 렌즈와 같은 투명한 객체를 잘 처리하지 못합니다. 향후 연구에서는 더 발전된 재료 공식화와 표면 정규화를 통합할 수 있습니다. 3D GAN은 단일 시점 이미지 컬렉션에서 3D 표현을 학습할 수 있지만, 정면 편향과 부정확한 라벨은 특히 얼굴의 옆면에서 기하학적 아티팩트를 초래할 수 있습니다. 향후의 유망한 방향은 인터넷 대규모 데이터를 사용한 3D GAN 훈련과 더 발전된 정규화 및 자동 카메라 보정을 통해 생성물을 360도로 확장하는 것입니다. 마지막으로, 우리의 샘플 기반 가속 방법은 다른 NeRF에도 적용될 수 있습니다.

**윤리적 고려사항**. 기존 방법들은 보이지 않는 GAN을 효과적으로 탐지하는 능력을 보여주었지만, 우리의 기여는 생성된 이미지에서 특정 특성을 제거할 수 있으며, 이는 탐지 작업을 더 어렵게 만들 수 있습니다. 가능한 해결책으로는 합성 미디어의 인증이 있습니다.

**결론**. 우리는 3D GAN을 가속화하여 2D 데이터의 원래 해상도에서 3D 표현을 해결하는 샘플러 기반 방법을 제안했습니다. 이는 엄격한 다시 보기 일관성 있는 이미지뿐만 아니라, 야생의 2D 이미지 컬렉션에서 학습된 상세한 3D 기하학을 생성합니다. 우리는 우리의 연구가 고품질 3D 모델과 야생 변형을 포착하는 합성 데이터를 생성하는 새로운 가능성을 열고, 조건부 뷰 합성과 같은 새로운 애플리케이션을 가능하게 할 것이라고 믿습니다.

### Supplementary Material

이 부록에서는 추가적인 시각적 결과(섹션 A1)와 평가(섹션 A2)를 제공하고 있습니다. 또한, 우리의 구현 세부 사항(섹션 A3)과 적응형 샘플링 접근 방법(소절 A3.4)에 대한 세부 사항을 설명합니다. 실험 세부 사항(섹션 A4)과 우리 연구의 한계점 및 향후 연구 방향에 대한 토론(섹션 A5)도 포함하고 있습니다. 추가적인 시각적 결과와 비교를 위해 동반되는 비디오도 참조하시기 바랍니다.

A1. 추가적인 질적 결과

![FFHQ 모델에서 선별한 샘플입니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%208.png)

FFHQ 모델에서 선별한 샘플입니다.

우리는 FFHQ와 AFHQ 모델 모두에 대해 큐레이션된 결과와 큐레이션되지 않은 결과를 보여줍니다. 큐레이션된 FFHQ 결과는 다양하고 세부적인 표정과 잘 정의된 3D 액세서리들을 보여줍니다. 큐레이션된 AFHQ 결과는 고양이의 상세한 텍스처와 잘 정의된 코와 귀를 강조합니다. 객관적인 제시를 위해, 큐레이션되지 않은 결과도 보여줍니다.

A2. 추가적인 평가

다양한 샘플링 수에 대한 샘플링 방법 비교

이 섹션에서는 매우 낮은 샘플 수에서 예측된 Pˆ 512를 사용한 우리의 제안된 샘플링 전략의 강인성을, 비계층화 및 계층화 샘플링 방법과 비교하여 보여줍니다. 우리 방법은 표준 샘플링 기술보다 우수한 성능을 보여줍니다.

A3. 구현 세부 사항

A3.1. 추론 세부 사항 및 시간

추론 시, 우리의 방법은 기본적으로 고해상도에서 17.6개의 깊이 샘플을 사용합니다. 우리 모델은 훈련 중 시점 의존적 효과를 학습하지만, 추론 시에는 정면 시점 조건을 사용합니다.

A3.2. 훈련 세부 사항

우리는 16개의 80GB NVIDIA A100 GPU에서 28.2백만 이미지에 대해 FFHQ 모델을 훈련시킵니다. AFHQ 모델은 이 모델을 기반으로 조정하여 추가적으로 훈련시킵니다.

A3.3. 네트워크 세부 사항

생성기의 아키텍처는 EG3D를 따르지만, 용량을 두 배로 늘립니다. 최종 삼면체를 얻기 위해 StyleGAN2에서 추가적인 Synthesis Blocks를 적용합니다.

![이 방법을 사용한 단일 뷰 3D 재구성의 예를 보여줍니다. 왼쪽 열은 테스트 이미지 입력을, 오른쪽 열은 오른쪽 세 열은 세 가지 새로운 뷰에서 얻은 반전 샘플을 보여줍니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%209.png)

이 방법을 사용한 단일 뷰 3D 재구성의 예를 보여줍니다. 왼쪽 열은 테스트 이미지 입력을, 오른쪽 열은 오른쪽 세 열은 세 가지 새로운 뷰에서 얻은 반전 샘플을 보여줍니다.

A3.4. 적응형 샘플링 세부 사항

우리는 예측된 고해상도 분포 Pˆ 512를 고려하여 더 어려운 픽셀에 더 많은 샘플을 할당합니다. 우리는 90%의 픽셀에 16개의 샘플을 할당하고 나머지 10%에 32개를 할당하는 간소화된 가정하에 운영합니다.

A3.5. 계층화 샘플링 세부 사항

우리는 예측된 분포 pˆ에서 견고한 분포 q를 계산합니다. 우리는 샘플 예산 s > 1이 주어졌을 때, 각 c개의 계층에 ⌊ s/c ⌋개의 샘플을 할당합니다.

A4. 실험 세부 사항

A4.1. 기하학 시각화

기하학 시각화를 위해, 우리는 March Cubes를 사용하여 등고면 기하학을 추출합니다. SDF 기반 방법의 경우, SDF 필드에서 0단계 셋을 추출합니다.

A4.2. Normal-FID

Normal-FID 계산을 위해, 우리는 NPHM 데이터셋의 메시에서 추출된 정상 맵을 사용합니다. PyFacer를 사용하여 배경 픽셀을 검은색으로 마스킹합니다.

A4.3. 기준 방법 세부 사항

모든 기준 방법에 대해, 공개된 사전 훈련된 모델을 사용하거나, 이전 작업에서 FID 숫자를 인용합니다.

A5. 토론

A5.1. 한계 및 향후 연구

우리는 우리 방법의 실패 사례 세 가지를 보여줍니다. 향후 연구는 더 균형 잡힌 데이터셋을 사용하고, 인간의 몸이나 더 일반적인 클래스로 확장하는 것이 매우 흥미로울 것입니다.

![3D 제너레이티브 모델의 세 가지 실패 사례. 첫 번째 줄은 얼굴 측면의 이음새를, 두 번째 줄은 표면 거칠기를 표시하여 스페큘러를 시뮬레이션하고, 세 번째 줄은 카메라 가까이에서 밀도가 나타나 피사체를 가리는 드문 현상을 보여줍니다.](What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3/Untitled%2010.png)

3D 제너레이티브 모델의 세 가지 실패 사례. 첫 번째 줄은 얼굴 측면의 이음새를, 두 번째 줄은 표면 거칠기를 표시하여 스페큘러를 시뮬레이션하고, 세 번째 줄은 카메라 가까이에서 밀도가 나타나 피사체를 가리는 드문 현상을 보여줍니다.