# MonoPatchNeRF: Improving Neural Radiance Fields with Patch-based Monocular Guidance

[https://arxiv.org/abs/2404.08252](https://arxiv.org/abs/2404.08252)

[https://yuqunw.github.io/MonoPatchNeRF/](https://yuqunw.github.io/MonoPatchNeRF/)

- Apr 2024

![희소한 시점의 장면 파사드에서 MonoPatchNeRF를 제시합니다. 우리의 방법은 테스트 뷰에서 실감 나는 이미지와 정확한 법선을 렌더링하고, 완전한 메쉬를 재구성합니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled.png)

희소한 시점의 장면 파사드에서 MonoPatchNeRF를 제시합니다. 우리의 방법은 테스트 뷰에서 실감 나는 이미지와 정확한 법선을 렌더링하고, 완전한 메쉬를 재구성합니다.

### 1 Introduction

이 논문에서는 이미지를 통해 3D 장면을 모델링하는 것을 컴퓨터 비전의 핵심 문제로 다루고 있습니다. 특히, 매핑, 시설 평가, 로보틱스, 건설 모니터링 등 다양한 응용 분야에서 유용하게 사용될 수 있는데, 이러한 응용 분야들은 정확한 기하학적 구조와 새로운 시점에서의 실감 나는 시각화를 필요로 합니다. 대규모 장면이나 희소한 뷰를 다룰 때, 고품질의 새로운 시점 합성을 제공하는 Neural Radiance Field (NeRF) 방식이 특히 유망하다고 소개되고 있습니다. 그러나 이 방법은 아직 완전한 해결책이 되지 못하고 있으며, 최신의 규제화된 접근법조차도 다중뷰 스테레오(MVS) 벤치마크에서 기하학적 정확성이나 시점 외관을 만족스럽게 생성하지 못하는 문제점을 가지고 있습니다. 이러한 배경을 바탕으로, 이 논문은 패치 기반 접근법을 통해 단안 깊이와 법선 예측을 활용하고 가상 시점의 일관성을 높이는 새로운 기법을 제안하여, NeRF 기반 모델의 기하학적 정확성과 시점 합성 성능을 향상시키는 것을 목표로 합니다.

### 2 Related Works

이 장에서는 3D 장면 재구성에 관한 기존의 연구들과 최근 접근 방식들을 소개하고 있습니다. 특히, Multi-view Stereo (MVS)는 이미 알려진 포즈를 가진 이미지를 사용하여 기하학적 구조를 재구성하는 잘 알려진 분야입니다. MVS는 초기 연구부터 깊이와 기하학적 일관성을 평가하는 다양한 방법을 통해 발전해 왔으며, 최근에는 딥러닝 기술을 통합한 새로운 방법들이 제안되고 있습니다.

![우리의 아키텍처 개요. 우리의 MonoPatchNeRF는 세 가지 주요 손실 유형을 포함합니다: 1) RGB 이미지의 색상 감독, 2) 단안 깊이와 법선 맵의 기하학적 감독, 3) 무작위로 샘플링된 패치와 해당 지상 진실 RGB 픽셀 사이의 가상 뷰 패치 정규화. 우리는 훈련 뷰 카메라 중심에서 무작위 변환을 통해 가상 뷰 자세를 샘플링하고, 훈련 뷰에서 렌더링된 깊이로 역투영된 광선을 따라 렌더링하여 해당 가상 뷰 패치를 얻습니다 (그림 3 참조). 추가적으로, 단안 기하학을 사용하여 영역을 제거함으로써 밀도 검색 공간을 제한합니다 (그림 4 참조).](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%201.png)

우리의 아키텍처 개요. 우리의 MonoPatchNeRF는 세 가지 주요 손실 유형을 포함합니다: 1) RGB 이미지의 색상 감독, 2) 단안 깊이와 법선 맵의 기하학적 감독, 3) 무작위로 샘플링된 패치와 해당 지상 진실 RGB 픽셀 사이의 가상 뷰 패치 정규화. 우리는 훈련 뷰 카메라 중심에서 무작위 변환을 통해 가상 뷰 자세를 샘플링하고, 훈련 뷰에서 렌더링된 깊이로 역투영된 광선을 따라 렌더링하여 해당 가상 뷰 패치를 얻습니다 (그림 3 참조). 추가적으로, 단안 기하학을 사용하여 영역을 제거함으로써 밀도 검색 공간을 제한합니다 (그림 4 참조).

Neural Radiance Field (NeRF)는 3D 재구성의 대안으로 등장했으며, 특히 새로운 시점 합성에 효과적인 솔루션을 제공합니다. NeRF는 이미지 집합에서 실시간으로 사실적인 뷰를 렌더링할 수 있으나, 희소한 입력이나 넓은 기저선에서는 정확하게 수렴하지 못하는 문제가 있습니다. 이러한 문제를 해결하기 위해, PixelNeRF와 DietNeRF 같은 학습 기반 방법들이 제안되었고, 이들은 사전 지식을 활용하여 희소한 입력 상황에서도 부드러운 노벨 뷰를 렌더링할 수 있도록 설계되었습니다.

또한, RegNeRF와 같은 방법들은 무작위로 샘플링된 패치에 대해 기하학적 부드러움 손실과 적대적 사진 손실을 적용하여, 입력 이미지가 희소할 때 성능을 향상시키는 접근 방식을 제안합니다. 이 외에도 SPARF와 FreeNeRF는 추가적인 정규화를 통해 NeRF 모델의 학습을 개선하고, 더 나은 기하학적 일관성을 추구하려고 시도합니다. 이러한 연구들은 NeRF와 MVS의 강점을 결합하거나 새로운 정규화 기법을 통해 성능을 개선하려는 다양한 시도를 보여주고 있습니다.

### 3 Method

이 장에서는 제안된 방법론의 세부사항을 설명하고, 3D 뉴럴 레디언스 필드를 구축하는 데 사용된 새로운 접근 방식을 자세히 소개합니다.

3.1 배경: Neural Radiance Fields (NeRF)
NeRF는 주어진 이미지 컬렉션에서 새로운 시점을 사실적으로 렌더링할 수 있는 장면의 매개변수적 표현을 제공합니다. NeRF는 3D 점의 위치와 관측 방향에 따라 색상과 투명도를 출력하는 함수로 정의됩니다. 픽셀 색상은 볼륨 렌더링을 통해 계산되며, NeRF는 렌더링된 픽셀 값과 관찰된 픽셀 값 사이의 차이를 최소화하면서 학습됩니다.

3.2 패치 기반 단안 단서의 정제
단일 이미지로부터의 법선 및 깊이 예측을 제공하는 학습 기반 네트워크를 활용하여 장면의 기하학적 구조에 대한 견고한 단서를 제공합니다. 단안 방법은 지역적으로만 정확할 수 있기 때문에, 이를 감안하여 더 나은 접근 방식을 모색합니다. 패치 기반의 단안 감독 손실을 통해 렌더링된 깊이와 법선 간의 차이를 최소화하며, 특히 깊이의 스케일과 이동을 정규화하여 최적화합니다.

3.3 가상 시점에 대한 패치 기반 사진 일관성
입력 이미지가 희소할 때 NeRF 모델이 과적합하는 문제를 해결하기 위해, 훈련 세트의 패치와 일치하는 가상 패치를 샘플링하고, 이들 사이의 구조적 유사성(SSIM)과 정규화된 교차 상관(NCC)을 최대화하는 손실 함수를 사용합니다. 가상 패치 샘플링은 훈련 패치의 깊이를 기반으로 하여 이루어지며, 가상 패치와 훈련 패치 간의 색상과 깊이를 비교하여 일관성을 평가합니다.

![가상 뷰 패치 샘플링 및 가림 표시 시각화. (a) 먼저 훈련 뷰 근처의 가상 뷰를 샘플링합니다. 그런 다음 훈련 뷰에서 패치를 렌더링하여 깊이를 추정합니다 (빨간 선), 이는 가상 뷰로 투영됩니다 (녹색 선). 마지막으로 가상 패치를 렌더링하고 (파란 선), 렌더링된 RGB를 훈련 패치의 지상 진실 RGB와 비교합니다. (b) 렌더링된 가상 뷰 깊이의 투영이 훈련 뷰 깊이와 각도 차이에 따라 일치하지 않는 경우 픽셀은 가려진 것으로 표시되고 NCC 및 SSIM 손실에서 제외됩니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%202.png)

가상 뷰 패치 샘플링 및 가림 표시 시각화. (a) 먼저 훈련 뷰 근처의 가상 뷰를 샘플링합니다. 그런 다음 훈련 뷰에서 패치를 렌더링하여 깊이를 추정합니다 (빨간 선), 이는 가상 뷰로 투영됩니다 (녹색 선). 마지막으로 가상 패치를 렌더링하고 (파란 선), 렌더링된 RGB를 훈련 패치의 지상 진실 RGB와 비교합니다. (b) 렌더링된 가상 뷰 깊이의 투영이 훈련 뷰 깊이와 각도 차이에 따라 일치하지 않는 경우 픽셀은 가려진 것으로 표시되고 NCC 및 SSIM 손실에서 제외됩니다.

3.4 희소한 SfM 기하학을 통한 밀도 제한
NeRF가 낮은 텍스처 또는 뷰 의존적 효과로 인해 올바른 기하학을 예측하지 못하는 문제를 해결하기 위해, 단안 기하학적 사전 지식과 희소한 다중 뷰 사전 지식을 사용하여 밀도 분포의 범위를 제한합니다. 이를 통해 불필요한 공간을 제거하고 전반적인 기하학적 추정을 개선합니다.

![밀도 제한 시각화. 왼쪽에는 밀도 제한 하에 훈련된 모델의 포인트 클라우드 재구성을 보여줍니다. 오른쪽에는 밀도 제한이 기하학을 개선하는 방법을 보여주기 위해 장면의 수직 슬라이스를 캡처합니다. 파란색 영역은 밀도 제한된 영역을 나타내고, 색칠된 영역은 밀도 제한 하에 훈련된 우리 방법의 포인트 클라우드 재구성을 나타냅니다. 녹색 및 빨간색 영역은 밀도 제한 없이 훈련된 우리 모델의 포인트 클라우드를 나타내며, 녹색은 밀도 제한 영역 내부에 있는 영역을, 빨간색은 밀도 제한 영역 밖에 있는 영역을 나타냅니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%203.png)

밀도 제한 시각화. 왼쪽에는 밀도 제한 하에 훈련된 모델의 포인트 클라우드 재구성을 보여줍니다. 오른쪽에는 밀도 제한이 기하학을 개선하는 방법을 보여주기 위해 장면의 수직 슬라이스를 캡처합니다. 파란색 영역은 밀도 제한된 영역을 나타내고, 색칠된 영역은 밀도 제한 하에 훈련된 우리 방법의 포인트 클라우드 재구성을 나타냅니다. 녹색 및 빨간색 영역은 밀도 제한 없이 훈련된 우리 모델의 포인트 클라우드를 나타내며, 녹색은 밀도 제한 영역 내부에 있는 영역을, 빨간색은 밀도 제한 영역 밖에 있는 영역을 나타냅니다.

3.5 훈련
사전 훈련된 Omnidata 모델을 사용하여 이미지로부터 단안 기하학적 단서를 추정하고, 이를 기반으로 밀도 제한을 초기화합니다. 훈련 과정에서는 각 참조 이미지마다 다수의 패치를 샘플링하고, 이를 가상 패치와 비교하여 여러 손실 항목을 평가합니다. NeRF 모델은 다양한 손실 함수를 사용하여 최적화되며, 이를 통해 더 정확하고 완전한 3D 표현을 구축합니다.

![ETH3D [29]에서의 질적 결과. 우리 방법과 기준선 [16, 24, 34, 38]에 대해 ETH3D [29]의 파사드에서 테스트 뷰의 렌더링된 RGB, 깊이 및 법선 맵과 완전한 기하학 재구성을 시각화합니다. 우리는 램프와 계단과 같은 도전적인 영역을 확대하여 차이를 강조합니다. 시각화 목적으로 패치의 깊이를 재정규화합니다. MonoSDF 및 Neuralangelo의 기하학은 메쉬이며, 다른 방법들의 기하학은 투영된 포인트 클라우드입니다. 확대하여 보는 것이 좋습니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%204.png)

ETH3D [29]에서의 질적 결과. 우리 방법과 기준선 [16, 24, 34, 38]에 대해 ETH3D [29]의 파사드에서 테스트 뷰의 렌더링된 RGB, 깊이 및 법선 맵과 완전한 기하학 재구성을 시각화합니다. 우리는 램프와 계단과 같은 도전적인 영역을 확대하여 차이를 강조합니다. 시각화 목적으로 패치의 깊이를 재정규화합니다. MonoSDF 및 Neuralangelo의 기하학은 메쉬이며, 다른 방법들의 기하학은 투영된 포인트 클라우드입니다. 확대하여 보는 것이 좋습니다.

이러한 방법들을 통해 제안된 접근 방식은 NeRF 기반의 3D 재구성에서 기하학적 정확성과 새로운 시점의 품질을 향상시키는 것을 목표로 합니다.

### 4 Experiments

이 장에서는 제안된 방법론을 검증하기 위한 실험적 접근법과 결과를 상세히 설명합니다. 다양한 벤치마크 데이터셋을 사용하여 기존의 NeRF 방법들과 비교 분석하였습니다.

![새로운 시점 이미지 및 메쉬의 질적 비교. 우리는 ETH3D 데이터셋 [29]에서 테스트 뷰 렌더링 이미지 및 메쉬를 제공합니다. 우리, RegNeRF [24], FreeNeRF [34]의 메쉬는 예측된 RGBD 시퀀스를 통해 TSDF 융합을 통해 생성됩니다. 확대하여 보는 것이 좋습니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%205.png)

새로운 시점 이미지 및 메쉬의 질적 비교. 우리는 ETH3D 데이터셋 [29]에서 테스트 뷰 렌더링 이미지 및 메쉬를 제공합니다. 우리, RegNeRF [24], FreeNeRF [34]의 메쉬는 예측된 RGBD 시퀀스를 통해 TSDF 융합을 통해 생성됩니다. 확대하여 보는 것이 좋습니다.

**데이터셋**: 실험은 주로 ETH3D 고해상도 데이터셋과 TanksAndTemples (TnT) 데이터셋을 사용했습니다. ETH3D는 큰 실내 및 실외 장면을 포함하며, 평균적으로 각 장면마다 약 35개의 이미지가 희소하게 촬영되어 있습니다. 이 데이터셋은 NeRF에게 특히 도전적인 환경을 제공합니다. TnT 데이터셋에서는 더 많이 캡처된 장면들을 선택하여 검증하였습니다.

**평가 프로토콜**: 새로운 시점 합성을 평가하기 위해 이미지의 90%를 사용하여 모델을 훈련하고 나머지 10%를 테스트 뷰로 사용하여 평가했습니다. 기하학적 추론은 모든 이미지를 사용하여 훈련하고 제공된 3D 기하학 평가 파이프라인을 통해 포인트 클라우드를 평가했습니다.

**성능 지표**: NeRF 방법의 기하학적 정확성 및 시점 외삽의 성능을 평가하기 위해 점 구름 지표와 시점 외삽을 측정했습니다. 노벨 뷰 합성의 경우 Peak Signal-to-Noise Ratio (PSNR), Structural Similarity Index Measure (SSIM), 그리고 Learned Perceptual Image Patch Similarity (LPIPS) 등의 메트릭을 보고했습니다.

**정량적 결과**: 실험 결과, 제안된 방법은 기하학적 정확성 및 노벨 뷰 합성에서 다른 NeRF 기반 방법들에 비해 뛰어난 성능을 보였습니다. 특히, ETH3D 벤치마크에서 F1@2cm, SSIM, LPIPS 지표에 있어 상당한 향상을 보여주었으며, 이는 제안된 방법이 기하학적 정확성을 크게 개선할 수 있는 가능성을 시사합니다.

![포인트 클라우드 시각화. 우리는 ACMMP [33]와 우리 방법을 ETH3D [29]에서 MVS 깊이 감독하에 시각화합니다. 우리 방법은 질감이 없고 반사하는 표면을 완성할 수 있습니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%206.png)

포인트 클라우드 시각화. 우리는 ACMMP [33]와 우리 방법을 ETH3D [29]에서 MVS 깊이 감독하에 시각화합니다. 우리 방법은 질감이 없고 반사하는 표면을 완성할 수 있습니다.

**질적 결과**: ETH3D 데이터셋에서 얻은 결과는 제안된 방법이 색상, 깊이, 법선 맵에서 노이즈가 적고 정확하며, 다른 기존 방법들에 비해 고주파 세부사항(예: 계단, 램프)을 더 잘 재현하는 것을 보여주었습니다. 또한, 제안된 방법의 포인트 클라우드는 다중 뷰에 걸쳐 일관되고 상세한 기하학을 제공함을 확인할 수 있었습니다.

**한계 및 개선점**: 제안된 방법은 NeRF/SDF 기반 방법들에 비해 더 높은 기하학적 정확성을 제공하지만, 여전히 MVS 시스템에는 미치지 못합니다. 또한, NeRF 기반 방법들은 MVS 시스템에 비해 계산 속도가 느려, 연구 및 실용적인 측면에서의 장벽이 될 수 있습니다. 이러한 문제를 해결하기 위해 가상 시점 패치의 샘플링 가이드, 단일 시점 예측과 기하학적 추론의 공동 추론, 재질 모델 확장 등을 통한 개선 방안이 제시되었습니다.

### 5 Conclusion

이 논문에서는 MonoPatchNeRF라는 패치 기반 정규화된 NeRF 모델을 제안합니다. 이 모델은 단안 기하학적 추정과 패치 기반 광선 샘플링 최적화, 밀도 제한을 효과적으로 활용하여 NeRF 기반 방법들의 기하학적 정확성을 크게 향상시키는 데 초점을 맞춥니다. 특히, NCC와 SSIM 사진 일관성 손실을 가상 및 훈련 뷰의 패치들 사이에 적용하여, 기존의 정규화된 NeRF 방법들과 비교했을 때 ETH3D MVS 벤치마크에서 뛰어난 성능을 보여줍니다.

제안된 방법은 기하학적 정확성, 노벨 뷰 합성 능력 모두에서 현저한 개선을 달성하였으며, 이는 NeRF 기반 모델의 발전 가능성을 입증합니다. 또한, 이 연구는 NeRF 기반 최적화가 전통적인 MVS 접근법을 궁극적으로 능가할 수 있는 잠재력을 지닌다고 믿습니다.

마지막으로, 논문은 여러 개선 방향을 제시하며, 특히 가상 시점 패치의 샘플링, 단일 시점 예측과 기하학적 추론의 통합, 더 다양한 재질 모델의 적용, 의미론적 분할의 포함 등을 통해 성능을 더욱 향상시킬 수 있는 방법을 모색할 것을 권장합니다. 이러한 연구 방향은 NeRF 기반 방법의 기하학적 정확성과 뷰 합성 품질을 더욱 향상시키는 데 기여할 것입니다.

![다른 매개변수에서 MonoSDF의 시각화. Relief_2에서 MonoSDF1.0은 저자들이 제공한 코드에서 기본 바이어스 매개변수(1.0)로 훈련된 MonoSDF를 나타내고, MonoSDF0.1은 저자들이 특히 대규모 장면을 위해 제안한 매개변수(0.1)로 훈련된 MonoSDF를 나타냅니다. 작은 바이어스로 MonoSDF [38]은 더 나은 메쉬를 재구성하며, 원래의 바이어스로는 로컬 최소점에 빠집니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%207.png)

다른 매개변수에서 MonoSDF의 시각화. Relief_2에서 MonoSDF1.0은 저자들이 제공한 코드에서 기본 바이어스 매개변수(1.0)로 훈련된 MonoSDF를 나타내고, MonoSDF0.1은 저자들이 특히 대규모 장면을 위해 제안한 매개변수(0.1)로 훈련된 MonoSDF를 나타냅니다. 작은 바이어스로 MonoSDF [38]은 더 나은 메쉬를 재구성하며, 원래의 바이어스로는 로컬 최소점에 빠집니다.

![ Facade에서 다른 배치 크기의 Neuralangelo 시각화.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%208.png)

 Facade에서 다른 배치 크기의 Neuralangelo 시각화.

![이미지 및 깊이 오버레이 시각화. 왼쪽에서 오른쪽으로, 우리는 RGB 이미지와 렌더링된 깊이 맵을 다양한 페이드 임계값과 오버레이합니다. 우리 방법은 이미지와 깊이가 정확하게 겹쳐지면서 전경이 두꺼워지지 않음을 보여줍니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%209.png)

이미지 및 깊이 오버레이 시각화. 왼쪽에서 오른쪽으로, 우리는 RGB 이미지와 렌더링된 깊이 맵을 다양한 페이드 임계값과 오버레이합니다. 우리 방법은 이미지와 깊이가 정확하게 겹쳐지면서 전경이 두꺼워지지 않음을 보여줍니다.

![ETH3D에서 새로운 시점 이미지 및 메쉬의 추가 비교. 우리는 기준선 [16, 24, 34, 38]과 추가 비교를 제공합니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%2010.png)

ETH3D에서 새로운 시점 이미지 및 메쉬의 추가 비교. 우리는 기준선 [16, 24, 34, 38]과 추가 비교를 제공합니다.

![TnT 고급 장면 [11]의 포인트 클라우드 시각화. 우리는 포인트 클라우드의 내부 및 멀리 떨어진 시점을 시각화하여 재구성된 기하학을 더 잘 보여줍니다. 우리 방법은 완전하고 정확한 포인트 클라우드를 재구성합니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%2011.png)

TnT 고급 장면 [11]의 포인트 클라우드 시각화. 우리는 포인트 클라우드의 내부 및 멀리 떨어진 시점을 시각화하여 재구성된 기하학을 더 잘 보여줍니다. 우리 방법은 완전하고 정확한 포인트 클라우드를 재구성합니다.

![ TnT 고급 장면 [11]의 자유형 뷰 렌더링.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%2012.png)

 TnT 고급 장면 [11]의 자유형 뷰 렌더링.

![ETH3D의 포인트 클라우드 시각화. 우리는 Facade를 제외한 ETH3D [29]의 모든 장면의 포인트 클라우드를 시각화합니다. 우리 방법은 완전하고 정확한 포인트 클라우드를 재구성합니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%2013.png)

ETH3D의 포인트 클라우드 시각화. 우리는 Facade를 제외한 ETH3D [29]의 모든 장면의 포인트 클라우드를 시각화합니다. 우리 방법은 완전하고 정확한 포인트 클라우드를 재구성합니다.

![ ETH3D [29]의 자유형 뷰 렌더링. 우리는 Facade를 제외한 ETH3D의 각 장면에 대해 우리의 자유형 뷰어를 사용하여 새로운 시점을 렌더링합니다. 우리의 새로운 시점 렌더링은 새로운 시점에서 고품질의 세부 질감을 유지함을 보여줍니다.](MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195/Untitled%2014.png)

 ETH3D [29]의 자유형 뷰 렌더링. 우리는 Facade를 제외한 ETH3D의 각 장면에 대해 우리의 자유형 뷰어를 사용하여 새로운 시점을 렌더링합니다. 우리의 새로운 시점 렌더링은 새로운 시점에서 고품질의 세부 질감을 유지함을 보여줍니다.