# LGM: Large Multi-View Gaussian Model for High-Resolution 3D Content Creation

[https://arxiv.org/abs/2402.05054](https://arxiv.org/abs/2402.05054)

[https://me.kiui.moe/lgm/](https://me.kiui.moe/lgm/)

- Feb 2024

![단일 뷰 이미지 또는 텍스트로부터 5초 만에 고해상도 3D 가우시안 이미지를 생성하는 방법.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled.png)

단일 뷰 이미지 또는 텍스트로부터 5초 만에 고해상도 3D 가우시안 이미지를 생성하는 방법.

### 1 Introduction

자동 3D 콘텐츠 생성 기술은 디지털 게임, 가상 현실, 영화 제작 등 다양한 분야에서 중요한 역할을 하고 있습니다. 이 기술은 전문 3D 아티스트들 사이에서 수작업의 필요성을 크게 줄여주며, 3D 자산 생성에 있어서 전문 지식이 없는 사람들도 참여할 수 있는 기회를 제공합니다.

이전의 3D 생성 연구는 대부분 2D 확산 모델을 3D로 변환하는 점수 증류 샘플링(Score Distillation Sampling, SDS)에 초점을 맞추어 왔습니다. 이러한 최적화 기반 방법들은 텍스트 또는 단일 뷰 이미지 입력으로부터 고도로 상세한 3D 객체를 생성할 수 있는 잠재력을 가지고 있지만, 생성 속도가 느리고 생성된 객체의 다양성이 제한적이라는 문제가 있습니다.

최근의 발전에 따라 단일 뷰 또는 소수 샷 이미지를 사용하여 3D 객체를 생성하는 데 필요한 시간이 크게 단축되었습니다. 이러한 방법들은 대부분 트랜스포머를 사용하여 직접적으로 3D 뉴럴 레이디언스 필드(NeRF)를 회귀하는 방식을 취합니다. 그러나 이러한 접근법들은 저해상도 훈련으로 인해 세부적인 질감과 복잡한 기하학적 형태를 생성하는 데 한계가 있습니다. 이러한 한계는 비효율적인 3D 표현 방식과 과도하게 많은 매개변수를 가진 3D 백본 때문에 발생합니다. 예를 들어, 고정된 계산 예산 하에서 LRM의 트라이플레인 표현은 32의 해상도로 제한되며, 온라인 볼륨 렌더링으로 인해 렌더링된 이미지의 해상도는 최대 128로 제한됩니다. 또한, 계산 집약적인 트랜스포머 기반 백본은 훈련 해상도를 제한합니다.

이러한 도전을 해결하기 위해, 본 연구에서는 트라이플레인 기반 볼륨 렌더링이나 트랜스포머에 의존하지 않고 소수 샷 3D 재구성 모델을 훈련하는 새로운 방법을 제안합니다. 우리의 접근법은 비대칭 U-Net을 사용하여 3D 가우시안 스플래팅의 특징을 예측하는 고성능 백본을 사용합니다. 이 설계의 동기는 고해상도 3D 생성을 달성하기 위해 표현력 있는 3D 표현과 고해상도에서 훈련할 수 있는 능력이 필요하기 때문입니다. 가우시안 스플래팅은 단일 트라이플레인에 비해 장면을 효율적으로 표현하는 능력과, 무거운 볼륨 렌더링에 비해 렌더링 효율성이 뛰어나다는 장점이 있습니다. 그러나 이 방법은 상세한 3D 정보를 정확하게 표현하기 위해 충분한 수의 3D 가우시안이 필요합니다. Splatter Image에서 영감을 받아, 우리는 U-Net이 다중 뷰 픽셀에서 충분한 수의 가우시안을 생성하는 데 효과적임을 발견했습니다. 이는 동시에 고해상도 훈련의 용량을 유지합니다. 우리의 기본 모델은 이전 방법들보다 더 높은 512의 해상도로 훈련되었으며, 피드포워드 회귀 모델의 빠른 생성 속도를 유지하면서도 이미지-3D 및 텍스트-3D 작업 모두를 지원합니다.

### 2 Related Work

**고해상도 3D 생성에 대한 관련 작업**

고해상도 3D 모델 생성에 관한 연구는 대부분 SDS(점수 증류 샘플링) 기반 최적화 기술에 의존하고 있습니다. 이 기술들은 2D 확산 모델로부터 3D로 상세한 정보를 효과적으로 추출하기 위해 표현력 있는 3D 표현과 고해상도 감독을 필요로 합니다. 예를 들어, NeRF를 DMTet로 변환하고 이를 더 세밀한 해상도로 정제하기 위한 두 단계 접근법을 사용하는 Magic3D가 있습니다. DMTet 기하학과 해시 그리드 질감의 혼합 표현은 고품질 3D 정보를 효율적으로 렌더링할 수 있게 합니다. 이와 유사하게, Fantasia3D는 DMTet를 직접 훈련하여 기하학과 외관 생성을 분리합니다. 이후 연구들은 메시 기반 단계를 사용하여 고해상도 감독을 가능하게 하고, 더 상세한 정보를 포착합니다. 또 다른 유망한 3D 표현인 가우시안 스플래팅은 그 표현력과 효율적인 렌더링 능력 때문에 주목받고 있으나, 이 방법으로 풍부한 세부 사항을 달성하기 위해서는 적절한 초기화와 주의 깊은 밀도화가 필요합니다.

**효율적인 3D 생성에 대한 접근**

SDS 기반 최적화 방법과 달리, 피드포워드 3D 네이티브 방법은 훈련 후 몇 초 안에 3D 자산을 생성할 수 있는 능력을 가지고 있습니다. 일부 연구는 3D 표현(예: 포인트 클라우드, 볼륨)에 대한 텍스트 조건부 확산 모델을 훈련시키려고 시도했습니다. 그러나 이러한 방법들은 대규모 데이터셋에 잘 일반화되지 않거나, 단순한 질감을 가진 저품질 3D 자산만을 생성할 수 있습니다. 최근에는 LRM이 단일 뷰 이미지로부터 NeRF를 견고하게 예측하고 5초 만에 메시로 내보낼 수 있는 회귀 모델을 훈련시킨 첫 사례입니다. Instant3D는 텍스트를 멀티뷰 이미지로 변환하는 확산 모델과 멀티뷰 LRM을 훈련하여 빠르고 다양한 텍스트-3D 생성을 수행합니다. 이러한 피드포워드 모델은 간단한 회귀 목표를 사용하여 훈련될 수 있으며, 3D 객체 생성 속도를 현저하게 가속화할 수 있습니다. 그러나 이들의 트라이플레인 NeRF 기반 표현은 상대적으로 낮은 해상도로 제한되어 최종 생성 품질에 제약을 줍니다.

**가우시안 스플래팅을 이용한 생성 작업**

가우시안 스플래팅을 사용하는 최근 방법들은 텍스트에서 3D 가우시안 생성까지의 시간을 줄이기 위해 SDS 기반 최적화 접근 방식과 결합되었습니다. 이러한 방법들은 다양한 밀도화 및 초기화 전략을 탐색합니다. 그러나 이러한 최적화 기반 방법으로 고품질 3D 가우시안을 생성하는 데에는 여전히 몇 분이 소요됩니다. TriplaneGaussian은 LRM 프레임워크에 가우시안 스플래팅을 도입하여, 가우시안 중심을 포인트 클라우드로 예측한 다음 이를 트라이플레인에 투영하여 다른 특징을 생성합니다. 그러나 가우시안의 수와 트라이플레인의 해상도는 여전히 제한되어 생성된 가우시안의 품질에 영향을 줍니다. Splatter Image는 단일 뷰 이미지에서 U-Net을 사용하여 출력 특징 맵에 있는 픽셀로 3D 가우시안을 예측하는 접근 방식을 제안합니다. 이 방법은 주로 단일 뷰 또는 두 뷰 시나리오에 초점을 맞추며, 대규모 데이터셋에 대한 일반화에는 제한이 있습니다. PixelSplat은 두 포즈 이미지의 각 픽셀에 대한 가우시안 매개변수를 예측하여 장면 데이터셋에서 사용됩니다. 본 연구에서는 기존 멀티뷰 확산 모델과 결합된 4-뷰 재구성 모델을 설계하여 일반 텍스트 또는 이미지에서 고품질 3D 객체 생성을 가능하게 합니다.

### 3 Large Multi-View Gaussian Model

이 연구는 고해상도 3D 콘텐츠 생성을 위한 새로운 프레임워크를 제시합니다. 이 프레임워크의 핵심은 멀티뷰 이미지로부터 3D 가우시안을 예측하고 융합하는 비대칭 U-Net 아키텍처를 사용하는 것입니다. 연구의 목적은 3D 표현의 효율성을 높이고 고해상도에서의 훈련 가능성을 개선하여, 텍스트나 단일 뷰 이미지로부터 상세하고 복잡한 3D 객체를 신속하게 생성할 수 있는 능력을 향상시키는 것입니다.

![파이프라인. 우리의 모델은 다중 뷰 이미지에서 3D 가우시안으로 재구성하도록 훈련되며, 추론 시 텍스트만, 이미지만 또는 두 입력 모두에서 기성 모델[44,51]로 합성할 수 있습니다. 다각형 메쉬는 선택적으로 추출할 수 있습니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%201.png)

파이프라인. 우리의 모델은 다중 뷰 이미지에서 3D 가우시안으로 재구성하도록 훈련되며, 추론 시 텍스트만, 이미지만 또는 두 입력 모두에서 기성 모델[44,51]로 합성할 수 있습니다. 다각형 메쉬는 선택적으로 추출할 수 있습니다.

### 3.1 기본 개념

- **가우시안 스플래팅**: 3D 데이터를 표현하기 위해 3D 가우시안의 집합을 사용합니다. 각 가우시안은 중심, 스케일링 요소, 회전 쿼터니언, 불투명도 값, 색상 특징으로 정의됩니다. 이러한 매개변수들은 3D 장면을 효율적으로 표현하며, 렌더링 시에는 2D 가우시안으로 이미지 평면에 투영되어 최종 색상과 불투명도를 결정합니다.
- **멀티뷰 확산 모델**: 3D 데이터셋에 대해 미세 조정된 멀티뷰 확산 모델을 사용하여, 동일한 객체의 여러 뷰를 생성할 수 있습니다. 이는 텍스트 프롬프트나 단일 뷰 이미지로부터 시작할 수 있으며, 실제 3D 모델 없이도 여러 관점에서 일관성 있는 이미지를 생성할 수 있습니다.

### 3.2 프레임워크 구조

![LGM의 아키텍처. 우리 네트워크는 크로스 뷰 자체 주의를 기울이는 비대칭 U-Net 기반 아키텍처를 채택합니다. 카메라 광선이 임베딩된 4개의 이미지를 입력으로 가져와서 3D 가우시안으로 해석 및 융합된 4개의 특징 맵을 출력합니다. 그런 다음 가우시안 이미지를 새로운 뷰에서 렌더링하고 지상 실측 이미지로 감독합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%202.png)

LGM의 아키텍처. 우리 네트워크는 크로스 뷰 자체 주의를 기울이는 비대칭 U-Net 기반 아키텍처를 채택합니다. 카메라 광선이 임베딩된 4개의 이미지를 입력으로 가져와서 3D 가우시안으로 해석 및 융합된 4개의 특징 맵을 출력합니다. 그런 다음 가우시안 이미지를 새로운 뷰에서 렌더링하고 지상 실측 이미지로 감독합니다.

- **인퍼런스 파이프라인**: 이 프레임워크는 두 단계의 3D 생성 파이프라인을 사용합니다. 첫 번째 단계에서는 멀티뷰 확산 모델을 사용하여 멀티뷰 이미지를 생성합니다. 두 번째 단계에서는 U-Net 기반 모델을 사용하여 이러한 스파스 뷰 이미지로부터 3D 가우시안을 예측합니다.
- **비대칭 U-Net 아키텍처**: U-Net은 멀티뷰 이미지와 해당 카메라 포즈 임베딩을 입력으로 받아, 각 이미지로부터 가우시안 세트를 예측하고 이를 융합하여 최종 3D 가우시안을 형성합니다. 이 아키텍처는 메모리 절약을 위해 더 깊은 레이어에서만 셀프 어텐션을 적용하고, 멀티뷰 정보를 통합하기 위해 이미지 특징을 평탄화하고 연결합니다.

### 3.3 로버스트 훈련

- **데이터 증강**: 실제 3D 객체로부터 렌더링된 멀티뷰 이미지와 확산 모델을 사용해 생성된 이미지 간의 도메인 갭을 완화하기 위해 두 가지 데이터 증강 방법을 사용합니다. 이는 모델이 다양한 멀티뷰 입력에 더 강건하게 대응할 수 있도록 합니다.
- **손실 함수**: RGB 이미지와 알파 이미지 모두를 렌더링하여, 생성된 가우시안을 감독합니다. 평균 제곱 오차 손실과 VGG 기반의 LPIPS 손실을 적용하여, RGB 이미지의 품질을 개선합니다.

### 3.4 메시 추출

- **메시 변환 알고리즘**: 생성된 3D 가우시안에서 다운스트림 작업에 더 적합한 다각형 메시를 추출하기 위한 일반적인 알고리즘을 설계합니다. 이 과정은 NeRF를 사용하여 3D 가우시안으로부터 렌더링된 이미지를 효율적으로 다각형 메시로 변환합니다.

![메시 추출 파이프라인. 유니티는 효율적인 파이프라인을 구현하여 3D 가우시안 모델을 매끄럽고 질감이 있는 메시로 변환합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%203.png)

메시 추출 파이프라인. 유니티는 효율적인 파이프라인을 구현하여 3D 가우시안 모델을 매끄럽고 질감이 있는 메시로 변환합니다.

대규모 멀티뷰 가우시안 모델은 고해상도 3D 콘텐츠 생성을 위한 혁신적인 접근 방식을 제공합니다. 비대칭 U-Net 아키텍처와 고도로 최적화된 데이터 증강 및 훈련 전략을 통해, 텍스트나 이미지 입력으로부터 복잡하고 상세한 3D 객체를 신속하게 생성할 수 있는 능력을 실현합니다. 이러한 접근 방식은 3D 콘텐츠 생성의 효율성과 접근성을 크게 향상시키며, 연구와 산업 양쪽 모두에서의 응용 가능성을 열어줍니다.

### 4 Experiments

이 연구에서는 대규모 멀티뷰 가우시안 모델의 효과성을 입증하기 위해 다양한 실험을 수행했습니다. 실험의 목적은 제안된 모델의 이미지-3D 및 텍스트-3D 생성 성능을 평가하고, 기존 방법과의 비교를 통해 우수성을 입증하는 것입니다.

![이미지 대 3D를 위해 생성된 3D 가우시안 비교. 우리의 방법은 다양한 까다로운 이미지에서 더 나은 시각적 품질로 가우시안 스플래팅을 생성합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%204.png)

이미지 대 3D를 위해 생성된 3D 가우시안 비교. 우리의 방법은 다양한 까다로운 이미지에서 더 나은 시각적 품질로 가우시안 스플래팅을 생성합니다.

![이미지에서 3D로 변환하기 위한 LRM과의 비교. 우리의 방법을 LRM [15]의 사용 가능한 결과와 비교합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%205.png)

이미지에서 3D로 변환하기 위한 LRM과의 비교. 우리의 방법을 LRM [15]의 사용 가능한 결과와 비교합니다.

4.1 구현 세부 사항

- **데이터셋**: Objaverse 데이터셋의 필터링된 부분집합을 사용하여 모델을 훈련시켰습니다. 원본 데이터셋에서 품질이 낮은 3D 모델들을 제거하는 두 가지 경험적 규칙을 적용했습니다. 이 과정을 통해 대략 80K개의 3D 객체로 구성된 최종 데이터셋을 확보했습니다.
- **네트워크 아키텍처**: 비대칭 U-Net 모델은 6개의 다운샘플링 블록, 1개의 중간 블록, 5개의 업샘플링 블록으로 구성되었습니다. 입력 이미지는 256×256 해상도이며, 출력 가우시안 특성 맵은 128×128 해상도입니다.
- **훈련**: 모델은 32개의 NVIDIA A100 GPU에서 약 4일 동안 훈련되었습니다. 각 GPU당 배치 크기는 8로 설정되었으며, 총 효과적인 배치 크기는 256이었습니다.
- **추론**: 추론 과정은 멀티뷰 확산 모델을 포함하여 대략 10GB의 GPU 메모리만 사용합니다. 이는 모델을 배포하기에 친화적인 환경을 제공합니다.

4.2 질적 비교

![텍스트 대 3D를 위해 생성된 3D 모델 비교. 우리의 방법은 더 나은 텍스트 정렬과 시각적 품질을 달성합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%206.png)

텍스트 대 3D를 위해 생성된 3D 모델 비교. 우리의 방법은 더 나은 텍스트 정렬과 시각적 품질을 달성합니다.

- **이미지-3D 변환**: 제안된 모델은 기존 방법들과 비교하여 생성된 3D 가우시안의 시각적 품질이 뛰어나고 입력 뷰의 콘텐츠를 효과적으로 보존하는 것으로 나타났습니다.
- **텍스트-3D 변환**: 제안된 모델은 텍스트-3D 작업에서도 우수한 성능을 보여주며, 현실적인 3D 객체를 더 효율적으로 생성할 수 있음을 입증했습니다.

![3D 생성의 다양성. 모호한 텍스트 설명이나 단일 뷰 이미지가 주어졌을 때 다양한 3D 모델을 생성할 수 있습니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%207.png)

3D 생성의 다양성. 모호한 텍스트 설명이나 단일 뷰 이미지가 주어졌을 때 다양한 3D 모델을 생성할 수 있습니다.

4.3 양적 비교

- 사용자 연구를 통해 이미지-3D 가우시안 생성 성능을 양적으로 평가했습니다. 총 90개의 비디오를 사용하여 30개의 이미지로부터 생성된 3D 가우시안의 360도 회전 비디오를 평가했습니다.
- 각 참가자는 무작위로 섞인 방법에서 선택된 30개의 샘플을 보고 이미지 일관성과 전반적인 모델 품질에 대해 평가했습니다. 총 20명의 자원봉사자로부터 600개의 유효한 점수를 수집했습니다.
- 결과적으로, 제안된 모델이 원본 이미지 콘텐츠와의 일관성과 전반적인 품질 측면에서 선호되었습니다.

4.4 소거 연구

![제거 연구. 우리 방법의 디자인에 대한 제거 연구를 수행합니다.](LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212/Untitled%208.png)

제거 연구. 우리 방법의 디자인에 대한 제거 연구를 수행합니다.

- **뷰의 수**: 단일 뷰 모델과 비교하여, 멀티뷰 설정은 뒷면의 흐릿함과 평평한 기하학적 형태 문제를 완화하고 보이지 않는 뷰에서도 세부 사항을 향상시키는 데 성공했습니다.
- **데이터 증강**: 데이터 증강을 적용하지 않은 모델과 비교하여, 데이터 증강 전략을 적용한 모델이 3D 불일치와 부정확한 카메라 포즈를 더 잘 교정했습니다.
- **훈련 해상도**: 더 적은 수의 가우시안과 더 작은 렌더링 해상도를 사용한 모델은 여전히 성공적으로 수렴했지만, 제안된 대규모 해상도 모델이 더 나은 세부 사항과 더 높은 해상도의 가우시안을 생성했습니다.

이 실험들은 제안된 대규모 멀티뷰 가우시안 모델이 이미지-3D 및 텍스트-3D 변환 작업에서 뛰어난 성능을 보임을 입증합니다. 질적 및 양적 비교 모두에서 기존 방법들을 능가하는 결과를 보여주며, 3D 콘텐츠 생성의 새로운 기준을 설정합니다. 이러한 결과는 3D 생성 기술의 발전뿐만 아니라, 다양한 응용 분야에서의 활용 가능성을 시사합니다.

### 5 Conclusion

이 연구에서는 고해상도 3D 콘텐츠 생성을 위한 새로운 프레임워크, 대규모 멀티뷰 가우시안 모델을 제안합니다. 이 모델은 기존의 NeRF 기반 방법과 트랜스포머에 의존하지 않고, 대신 가우시안 스플래팅과 U-Net을 활용하여 고메모리 요구 사항과 저해상도 훈련의 문제를 해결합니다. 제안된 방법은 멀티뷰 이미지에서 고해상도 3D 가우시안을 생성하고, 이를 다양한 방식으로 융합하여 복잡한 3D 객체를 신속하게 생성할 수 있는 능력을 입증합니다.

데이터 증강과 훈련 전략을 통해 모델의 강인성을 높이고, 생성된 3D 가우시안에서 다운스트림 작업에 적합한 다각형 메시를 추출하는 일반적인 알고리즘을 설계함으로써, 텍스트 또는 이미지 입력에서 시작하여 고해상도의 상세하고 현실적인 3D 객체를 생성할 수 있습니다.

실험을 통해 제안된 모델이 기존 방법들을 능가하는 성능을 보임을 확인했습니다. 질적 및 양적 비교 모두에서 제안된 모델은 높은 해상도와 세부 사항을 가진 3D 생성물을 신속하게 생성할 수 있는 능력을 보여줍니다. 이러한 결과는 제안된 모델이 3D 콘텐츠 생성 분야에서 중요한 발전을 나타내며, 디지털 게임, 가상 현실, 영화 제작 등 다양한 응용 분야에서의 활용 가능성을 시사합니다.

이 연구는 3D 콘텐츠 생성 기술의 발전에 기여함과 동시에, 고해상도 3D 객체 생성을 위한 새로운 접근 방식을 제공합니다. 미래 연구에서는 현재 모델의 한계를 극복하고, 더 다양한 입력과 응용 분야에서의 활용을 탐구할 수 있습니다. 이러한 진전은 3D 콘텐츠 생성의 효율성과 접근성을 더욱 향상시킬 것으로 기대됩니다.