# InternImage: Exploring Large-Scale Vision Foundation Models with Deformable Convolutions

*이 논문에서는 이미지 인식 작업을 위해 CNN과 트랜스포머의 강점을 효과적으로 결합한 새로운 모델인 InternImage를 소개합니다. 이 모델은 이중 경로 구조를 사용하여 로컬 상호 작용을 위한 CNN 경로와 글로벌 컨텍스트 모델링을 위한 트랜스포머 경로를 병합하여 표현 능력을 향상시킵니다.*

*두 경로를 결합하기 위해 InternImage의 기본 단위인 새로운 이미지 분해 블록(IDB)과 컨볼루션 라우팅 메커니즘을 제안하여 모델의 성능을 향상시켰습니다.*

*또한 저자들은 모델의 효율성을 높이는 새로운 계산 리소스 할당 전략도 소개했습니다. 인턴이미지 모델은 다른 모델과 비교하여 테스트한 결과, 다양한 이미지 변환에서 우수한 매개변수 효율성, 빠른 추론 속도, 더 나은 견고성을 보여주었습니다. 또한 다양한 데이터 규모에서 강력한 견고성을 보여주었습니다.*

*저자들은 제안된 모델이 다양한 이미지 처리 작업에 적합하며 정확도와 효율성 면에서 기존 모델보다 뛰어나다고 결론지었습니다. 또한 고효율이 필요한 다운스트림 작업에 대한 모델의 적합성을 더욱 향상시키기 위해 효율적인 DCN을 개발하는 데 관심을 표명했습니다.*

[https://arxiv.org/abs/2211.05778](https://arxiv.org/abs/2211.05778)

### 1. Introduction

이 논문에서는 컴퓨터 비전 작업에서 비전 트랜스포머(ViT)와 경쟁할 수 있도록 설계된 새로운 컨볼루션 신경망(CNN) 기반 모델인 InternImage의 개발과 적용에 대해 설명합니다. ViT는 수십억 개의 매개변수와 방대한 데이터가 포함된 대규모 모델을 처리하는 데 탁월한 성능을 보여 다양한 작업에서 CNN을 능가하는 성능을 보여 왔습니다. 그러나 저자들은 올바른 아키텍처와 설계를 통해 CNN이 ViT와 비슷하거나 심지어 능가할 수 있다고 주장합니다.

![다른 핵심 연산자들의 비교. (a)는 다중 헤드 자기 주의(MHSA) [1]의 글로벌 집합을 보여주는데, 이는 고해상도 입력이 필요한 하류 작업에서의 계산 및 메모리 비용이 많이 든다. (b)는 비용을 줄이기 위해 MHSA의 범위를 지역 창 [2]으로 제한한다. (c)는 장거리 의존성을 모델링하기 위해 아주 큰 커널을 가진 깊이별 합성곱이다. (d)는 변형 가능한 합성곱으로, MHSA와 비슷한 좋은 특성을 공유하며 대규모 모델에 충분히 효율적이다. 우리는 이를 시작으로 대규모 CNN을 구축한다.](InternImage%20Exploring%20Large-Scale%20Vision%20Foundatio%20d71e02986f3148da846c7a3a961fc52b/Untitled.png)

다른 핵심 연산자들의 비교. (a)는 다중 헤드 자기 주의(MHSA) [1]의 글로벌 집합을 보여주는데, 이는 고해상도 입력이 필요한 하류 작업에서의 계산 및 메모리 비용이 많이 든다. (b)는 비용을 줄이기 위해 MHSA의 범위를 지역 창 [2]으로 제한한다. (c)는 장거리 의존성을 모델링하기 위해 아주 큰 커널을 가진 깊이별 합성곱이다. (d)는 변형 가능한 합성곱으로, MHSA와 비슷한 좋은 특성을 공유하며 대규모 모델에 충분히 효율적이다. 우리는 이를 시작으로 대규모 CNN을 구축한다.

이 논문은 CNN과 ViT의 두 가지 주요 차이점을 인정합니다. 첫째, ViT의 다중 헤드 자기 주의(MHSA) 메커니즘은 장거리 종속성과 적응형 공간 집계를 허용하여 대규모 데이터 세트에서 보다 강력한 표현을 학습할 수 있게 해줍니다. 둘째, ViT에는 레이어 정규화(LN), 피드 포워드 네트워크(FFN), GELU와 같이 표준 CNN에는 없는 고급 구성 요소가 포함되어 있습니다.

이러한 격차를 해소하기 위해 저자는 컨볼루션의 변형인 변형 컨볼루션(DCN)을 트랜스포머와 유사한 다른 블록 수준 및 아키텍처 수준 설계와 결합하여 새로운 CNN 기반 모델인 InternImage를 구축합니다. 핵심 연산자는 3x3의 공통 창 크기를 가진 동적 스파스 컨볼루션으로, 입력 데이터에 적응하여 적절한 수신 필드를 동적으로 학습할 수 있습니다.

제안된 InternImage는 대규모 매개변수 크기로 효율적으로 확장하고 대규모 훈련 데이터에서 더 강력한 표현을 학습하여 다양한 비전 작업에서 대규모 ViT의 성능과 일치하거나 능가할 수 있습니다. 이 모델은 10억 개 이상의 매개변수와 4억 개의 훈련 이미지로 효과적으로 확장할 수 있는 최초의 CNN입니다.

저자는 이 모델을 사용하여 CNN이 대규모 모델 연구에서도 실행 가능한 방향임을 보여줍니다. 또한 CNN 연산자를 사용하여 장거리 종속성과 적응적 공간 집계를 도입하고 맞춤형 기본 블록, 스태킹 규칙 및 확장 전략을 조사합니다.

제안한 모델은 이미지 분류, 객체 검출, 인스턴스 및 의미적 분할과 같은 대표적인 비전 작업에 대해 평가하고 모델 크기와 데이터를 확장하여 최첨단 CNN 및 대규모 ViT와 비교했습니다. 이 모델은 ImageNet 및 기타 벤치마크에서 이전 모델보다 지속적으로 우수한 성능을 보였습니다.

### 2. Related Work

이 백서에서는 대규모 데이터 세트와 계산 리소스를 사용할 수 있게 되면서 컨볼루션 신경망(CNN)이 시각 인식에 널리 사용되었다는 점에 주목하여 비전 기반 모델의 진화에 대해 살펴봅니다. 지난 몇 년 동안 AlexNet, VGG, GoogleNet, ResNet, ResNeXt, EfficientNet과 같은 여러 가지 효과적인 신경망 아키텍처가 제안되었습니다. 또한 깊이별 컨볼루션 및 변형 가능한 컨볼루션과 같은 보다 정교한 컨볼루션 연산이 공식화되어 개선된 설계로 인해 비전 작업에서 우수한 성능을 발휘하는 최신 CNN으로 이어졌습니다.

이 백서에서는 비전 기반 모델을 위한 트랜스포머 기반 아키텍처로의 최근 전환에 대해서도 강조합니다. 비전 트랜스포머(ViT)는 글로벌 수용 필드와 동적 공간 집계로 인해 비전 작업에서 큰 성공을 거두었습니다. 그러나 전 세계적으로 주목받고 있는 ViT는 특히 대규모 피처 맵의 경우 계산 비용이 많이 들고 메모리 집약적입니다. 이 문제를 해결하기 위해 PVT, Linformer, DAT, HaloNet, Swin 트랜스포머와 같은 새로운 모델에서는 다운샘플링, 변형 가능한 주의, 로컬 주의 메커니즘과 같은 솔루션을 제안했습니다.

이 논문에서는 자연어 처리(NLP) 분야에서 잘 연구된 개념인 특징 표현 품질 향상을 위한 모델 확장의 중요성을 강조합니다. 연구자들은 NLP의 성공을 바탕으로 ViT를 수십억 개의 매개변수로 확장하고 다양한 수준에서 ViT와 CNN의 장점을 결합하여 대규모 하이브리드 ViT를 개발했습니다. 그러나 저자들은 CNN 기반 대규모 모델이 총 파라미터와 성능 면에서 트랜스포머 기반 아키텍처에 비해 뒤처진다는 점에 주목하고, 새로운 CNN 기반 기초 모델인 InternImage를 통해 이 격차를 해소하고자 합니다.

### 3. Proposed Method

이 백서에서는 유연한 컨볼루션 변형인 변형 컨볼루션 v2(DCNv2)로 시작하여 대규모 모델의 요구 사항에 맞게 수정하는 대규모 CNN 기반 기초 모델을 설계하는 방법을 소개합니다. 핵심 빌딩 블록은 조정된 컨볼루션 연산자와 최신 백본의 고급 블록 설계를 결합합니다.

![InternImage의 전체 구조, 여기서 핵심 연산자는 DCNv3이고, 기본 블록은 트랜스포머로서의 레이어 정규화(LN) [24]와 피드-포워드 네트워크(FFN) [1]로 구성되며, 줄기 및 다운샘플링 계층은 전통적인 CNN의 디자인을 따른다. "s2"와 "p1"은 각각 stride 2와 padding 1을 의미한다. 쌓기 규칙에 의해 제한되어, 오직 4개의 하이퍼파라미터 (C1, C0, L1, L3) 만이 모델 변형을 결정할 수 있다.](InternImage%20Exploring%20Large-Scale%20Vision%20Foundatio%20d71e02986f3148da846c7a3a961fc52b/Untitled%201.png)

InternImage의 전체 구조, 여기서 핵심 연산자는 DCNv3이고, 기본 블록은 트랜스포머로서의 레이어 정규화(LN) [24]와 피드-포워드 네트워크(FFN) [1]로 구성되며, 줄기 및 다운샘플링 계층은 전통적인 CNN의 디자인을 따른다. "s2"와 "p1"은 각각 stride 2와 padding 1을 의미한다. 쌓기 규칙에 의해 제한되어, 오직 4개의 하이퍼파라미터 (C1, C0, L1, L3) 만이 모델 변형을 결정할 수 있다.

새로운 버전인 변형 컨볼루션 v3(DCNv3)는 기존 컨볼루션과 다중 헤드 셀프 어텐션(MHSA) 간의 격차를 해소하기 위해 개발되었습니다. 저자들은 일반적으로 큰 수용 필드(장거리 종속성)를 가진 모델이 비전 작업에서 더 나은 성능을 보인다는 사실을 인지했습니다. 그러나 일반 3x3 컨볼루션으로 스택된 CNN은 유효 수용 필드가 상대적으로 작습니다. 또 다른 차이점은 공간 집계의 적응성입니다. MHSA 가중치는 입력에 따라 적응하는 반면, 일반 컨볼루션은 정적 가중치를 유지합니다. CNN이 더 빠르게 수렴하고 더 적은 학습 데이터를 필요로 할 수 있지만, 이로 인해 웹 규모 데이터에서 학습할 수 있는 패턴이 제한됩니다.

![다양한 백본들의 COCO에서의 성능 비교. 제안된 InternImage-H는 COCO test-dev에서 65.4 박스 AP라는 새로운 기록을 달성하여, 최신 CNNs 및 대규모 ViTs를 크게 앞섭니다.](InternImage%20Exploring%20Large-Scale%20Vision%20Foundatio%20d71e02986f3148da846c7a3a961fc52b/Untitled%202.png)

다양한 백본들의 COCO에서의 성능 비교. 제안된 InternImage-H는 COCO test-dev에서 65.4 박스 AP라는 새로운 기록을 달성하여, 최신 CNNs 및 대규모 ViTs를 크게 앞섭니다.

DCNv2는 일반 컨볼루션에 장거리 종속성과 적응형 공간 집계를 도입했습니다. 이 논문에서는 세 가지 방법으로 비전 기반 모델용 DCNv2를 확장할 것을 제안했습니다:

컨볼루션 뉴런 간 가중치 공유: 일반 컨볼루션과 마찬가지로 DCNv2는 서로 다른 컨볼루션 뉴런에 대해 독립적인 선형 투영 가중치를 사용합니다. 이렇게 하면 메모리와 매개변수 복잡성이 증가하여 대규모 모델의 효율성이 제한될 수 있습니다. 이를 극복하기 위해 저자는 분리형 컨볼루션이라는 아이디어를 적용하여 원래의 컨볼루션 가중치를 깊이와 점 단위로 나누었습니다.

다중 그룹 메커니즘 도입: 트랜스포머의 그룹 컨볼루션과 MHSA의 다중 그룹 설계에서 영감을 얻어 공간 집계 프로세스를 각각 개별 샘플링 오프셋과 변조 스케일을 가진 여러 그룹으로 분할했습니다. 이를 통해 단일 컨볼루션 레이어의 여러 그룹이 서로 다른 공간 집계 패턴을 가질 수 있으므로 다운스트림 작업에서 더 강력한 피처를 얻을 수 있습니다.

샘플링 지점을 따라 변조 스칼라를 정규화합니다: 기존 DCNv2에서는 변조 스칼라가 시그모이드 함수에 의해 요소별로 정규화되어 대규모 파라미터와 데이터로 훈련할 때 그라데이션이 불안정해집니다. 이 문제를 해결하기 위해 연구진은 샘플 포인트를 따라 소프트맥스 정규화를 사용하여 다양한 규모의 모델 훈련 과정을 안정화했습니다.

새롭게 제안된 DCNv3 연산자는 일반 컨볼루션의 단점을 해결하고, 귀납적 편향으로 인해 더 적은 훈련 데이터와 더 짧은 훈련 시간으로 더 효율적이며, 희소 샘플링으로 인해 계산 및 메모리 효율이 더 높습니다.

이 섹션에서는 핵심 구성 요소로 변형 컨볼루션 v3(DCNv3)를 활용하는 향상된 딥러닝 모델인 InternImage를 제안합니다. 이 모델의 구성은 여러 단계로 나뉩니다:

기본 블록: 제안된 모델에서 기본 블록은 비전 트랜스포머(ViT)와 유사하며, 레이어 정규화(LN), 피드 포워드 네트워크(FFN), GELU와 같은 고급 요소를 포함합니다. 이 블록의 핵심 연산자는 DCNv3입니다. 오프셋과 변조 스케일은 입력 피처를 분리 가능한 컨볼루션을 통해 전달하여 예측됩니다.

스템 및 다운샘플링 레이어: 이 레이어는 피처 맵의 크기를 다른 스케일로 조정하는 데 사용됩니다. 스템 레이어는 입력 해상도를 4배 감소시키며 두 개의 컨볼루션, 두 개의 LN 레이어, 하나의 GELU 레이어로 구성된 첫 번째 단계 앞에 배치됩니다. 두 단계 사이에 배치된 다운샘플링 레이어는 3×3 컨볼루션을 사용하여 입력 특징 맵 크기를 절반으로 줄입니다.

스태킹 규칙: 저자는 모델의 스태킹 프로세스를 안내하기 위해 특정 규칙을 제시합니다. 여기에는 단계의 채널 번호와 그룹 번호를 조정하고 각 단계의 기본 블록 수를 지정하는 것이 포함됩니다. 여러 단계의 스택 블록 수에는 단순화된 "AABA" 패턴이 사용됩니다.

스케일링 규칙: 저자들은 깊이(D)와 너비(C1)를 기준으로 모델을 확장하는 규칙도 제안합니다. 두 가지 스케일링 차원을 고려하고 α, β 및 복합 계수 φ를 사용하여 스케일링합니다. 실험적으로 찾은 최상의 스케일링 설정은 α = 1.09 및 β = 1.36입니다. 저자는 이러한 규칙을 사용하여 InternImage 모델의 다양한 변형을 만들었습니다.

결론적으로, 저자들은 DCNv3를 핵심 연산자로 효과적으로 활용하는 새로운 딥러닝 모델인 InternImage를 제안했습니다. 이 모델은 컨볼루션 기반 모델의 장거리 종속성 및 적응형 공간 집계 문제를 해결하도록 설계되었습니다. 또한 이 모델과 다양한 변형 모델의 개발을 안내하기 위해 스태킹 및 확장에 대한 특정 규칙이 공식화되었습니다.

### 4. Experiment

이 섹션에서는 이미지 분류, 객체 감지, 인스턴스 및 시맨틱 분할을 포함한 다양한 비전 작업에서 InternImage 모델의 성능을 평가합니다.

이미지 분류

이미지 분류의 경우, 이 모델은 ImageNet에서 테스트됩니다. 저자는 작은 InternImage 변형(T/S/B)은 ImageNet-1K에서 300회에 걸쳐 훈련하고, 큰 변형(L/XL)은 먼저 ImageNet-22K에서 90회에 걸쳐 훈련한 다음 ImageNet-1K에서 20회에 걸쳐 미세 조정하여 공정한 비교를 수행합니다. 인턴이미지-H는 4억 2,700만 개의 대규모 이미지 데이터 세트에 대해 사전 학습됩니다. 인턴이미지 모델은 정확도와 계산 효율성 측면에서 최첨단 CNN 및 트랜스포머 기반 모델보다 성능이 뛰어나거나 동등한 것으로 나타났습니다.

예를 들어, InternImage-T는 83.5%의 상위 1퍼센트 정확도를 달성하여 ConvNext-T 모델을 1.4포인트 차이로 능가합니다. 더 큰 데이터 세트에 대해 사전 학습한 경우 InternImage-XL과 -H의 상위 1% 정확도는 각각 88.0%와 89.6%에 달해 다른 주요 대규모 비전 트랜스포머와의 격차가 약 1% 포인트까지 좁혀집니다.

물체 감지

다음으로 두 가지 프레임워크를 사용하여 COCO 벤치마크에서 물체 감지 모델을 테스트합니다: 마스크 R-CNN과 캐스케이드 마스크 R-CNN입니다. 역시 결과는 긍정적이었으며, InternImage 모델이 다른 모델을 크게 능가했습니다. 예를 들어, 1배 훈련 스케줄에서 InternImage-T 모델의 박스 AP(평균 정밀도)는 Swin-T보다 4.5포인트, ConvNeXt-T보다 3.0포인트 더 높습니다.

더 복잡한 3배 멀티스케일 훈련 일정과 더 많은 파라미터, 고급 캐스케이드 마스크 R-CNN을 사용하는 InternImage-XL은 56.2의 박스 AP를 달성하여 ConvNeXt-XL을 1.0포인트 능가합니다. 인스턴스 세분화 실험에서도 비슷한 결과가 관찰됩니다. 최고의 마스크 AP는 InternImage-XL이 달성했으며, 이는 다른 마스크보다 최소 1.1포인트 더 높습니다.

마지막으로, 고급 설정을 사용하여 Objects365 및 COCO 데이터 세트에서 DINO 검출기와 함께 모델을 미세 조정하여 COCO val2017 및 test-dev에서 각각 65.0 APb 및 65.4 APb의 최상의 결과를 얻었습니다. 이는 FD-SwinV2-G와 같은 이전의 최첨단 모델을 능가하는 것으로, 물체 검출 작업에서 InternImage의 효율성을 입증한 것입니다.

![다른 그룹에서 다른 단계에서의 샘플링 위치의 시각화. 파란색 별은 쿼리 포인트를 나타내며(왼쪽 양), 다른 색깔의 점들은 다른 그룹의 샘플링 위치를 나타낸다.](InternImage%20Exploring%20Large-Scale%20Vision%20Foundatio%20d71e02986f3148da846c7a3a961fc52b/Untitled%203.png)

다른 그룹에서 다른 단계에서의 샘플링 위치의 시각화. 파란색 별은 쿼리 포인트를 나타내며(왼쪽 양), 다른 색깔의 점들은 다른 그룹의 샘플링 위치를 나타낸다.

이 섹션에서는 의미론적 세분화 작업에서 인턴이미지 모델의 성능을 평가하고 모델의 일부 설계 선택을 설명하기 위해 몇 가지 제거 연구를 제시합니다.

시맨틱 세분화

시맨틱 세그먼트 성능을 평가하기 위해 InternImage 모델을 ADE20K의 UperNet으로 훈련하고 이전의 CNN 기반 및 트랜스포머 기반 모델과 비교합니다. 거의 동일한 수의 파라미터와 FLOP을 사용하는 InternImage-B는 ADE20K 검증 데이터세트에서 50.8mIoU를 달성하여 ConvNeXt-B 및 RepLKNet-31B와 같은 강력한 대응 모델을 능가합니다. Mask2Former로 무장할 경우, InternImage-H는 심지어 ADE20K 벤치마크에서 현재 최고 수준인 BEiT-3보다 높은 62.9의 mIoU를 달성했습니다. 이는 CNN 기반 파운데이션 모델인 InternImage가 대규모 데이터 세트의 이점을 활용하고 선도적인 트랜스포머 기반 모델과 경쟁할 수 있음을 시사합니다.

절제 연구

그런 다음 저자들은 설계 선택을 검증하기 위해 두 가지 제거 연구를 수행했습니다:

컨볼루션 뉴런 간 가중치 공유: 이 연구에서 저자는 DCNv3의 컨볼루션 뉴런 간에 가중치를 공유할 때의 이점을 설명합니다. 가중치를 공유하지 않는 모델은 훨씬 더 많은 메모리가 필요하고 특히 -H 스케일에서 더 많은 매개변수를 포함한다는 것을 보여줍니다. 예를 들어, 공유 가중치를 사용하면 -H 스케일에서 파라미터와 GPU 메모리가 각각 42.0%와 84.2% 감소합니다. 또한 공유 가중치를 사용한 모델과 공유되지 않은 가중치를 사용한 모델의 성능은 ImageNet과 COCO에서 비슷하지만, 공유되지 않은 가중치를 사용한 모델의 파라미터가 66.1% 더 많습니다.

다중 그룹 공간 집계: 저자들은 모델이 서로 다른 표현 하위 공간에서 정보를 학습할 수 있도록 집계 그룹을 도입하는 것이 중요하다고 강조합니다. 다중 그룹이 있는 모델과 없는 모델의 성능을 비교하여 다중 그룹을 제거하면 ImageNet과 COCO val2017 모두에서 상당한 성능 저하가 발생한다는 것을 보여줍니다. 또한 모델의 유효 수용 필드(ERF)가 모델이 깊어질수록 증가하는 것을 관찰했는데, 이는 일반적으로 ERF가 전역적인 ViT와는 다른 점입니다.

### 5. Conclusion & Limitations

이들은 이미지 분류, 물체 감지, 의미적 분할과 같은 다양한 비전 작업에 강력한 표현을 제공할 수 있는 새로운 대규모 컨볼루션 신경망(CNN) 기반 기초 모델인 InternImage를 소개합니다. 연구진은 기본 모델의 요구 사항을 충족하기 위해 변형 컨볼루션 네트워크(DCNv2) 연산자를 조정하고 이 핵심 연산자를 기반으로 블록, 스태킹 규칙 및 확장 전략 세트를 개발했습니다. 객체 감지 및 시맨틱 분할 벤치마크에 대한 광범위한 실험을 통해 InternImage가 상당한 데이터로 훈련된 대규모 비전 트랜스포머와 비슷하거나 더 우수한 성능을 달성할 수 있음을 입증했습니다. 이는 CNN이 대규모 비전 기초 모델 연구에도 실행 가능한 옵션이 될 수 있음을 보여줍니다.

그러나 연구자들은 작업의 한계도 인정합니다. 첫째, CNN 기반 운영자는 고속 처리가 필요한 다운스트림 작업에 적응할 때 여전히 지연 문제가 있습니다. 또한 대규모 CNN은 아직 개발 초기 단계에 있어 개선과 추가 탐색의 여지가 많다는 것을 나타냅니다. 저자는 인턴이미지가 현재 진행 중인 이 연구 분야에서 좋은 출발점이 될 수 있기를 희망한다고 밝혔습니다.

### Appendix

저자들은 부록을 통해 훈련 설정과 하이퍼파라미터 탐색에 대해 자세히 설명합니다.

A. 자세한 훈련 설정: 이 섹션에서는 이미지 분류, 객체 감지, 시맨틱 분할 등 세 가지 비전 작업에 대한 훈련 설정을 자세히 설명합니다. 이미지 분류 작업은 이미지넷을 사용하며 라벨이 없는 데이터와 약한 라벨이 있는 데이터를 모두 허용하는 M3I 사전 훈련 방식을 채택합니다. 객체 감지의 경우, 일반적인 관행에 따라 마스크 R-CNN과 캐스케이드 마스크 R-CNN 위에 COCO 벤치마크를 사용합니다. 시맨틱 세분화의 경우, 사전 학습된 분류 가중치로 초기화된 모델과 함께 ADE20K 데이터 세트를 사용합니다. 또한 시스템 수준 비교를 위해 다양한 설정 세트를 자세히 설명합니다.

B. 하이퍼파라미터 탐색: 이 섹션에서는 모델 스태킹 및 확장에 중점을 둔 하이퍼파라미터 탐색에 대해 자세히 설명합니다. 다양한 하이퍼파라미터 조합으로 실험을 수행하고 그 결과를 표와 그림으로 표시합니다. 그 결과 최적의 스태킹 하이퍼파라미터는 (C1, C0, L1, L3) = (64, 16, 4, 18)로 나타났습니다. 모델 스케일링의 경우 (α, β)를 (1.09, 1.36)으로 설정할 때 가장 좋은 성능을 얻을 수 있음을 발견했습니다.

또한 DCNv3 연산자에서 커널 크기의 영향도 조사합니다. 이들은 DCNv3 연산자의 3 × 3 커널을 5 × 5 또는 7 × 7 커널로 대체하여 컨볼루션 커널을 확대해도 정확도가 크게 향상되지 않고 오히려 감소할 수 있음을 발견했습니다. 인턴이미지의 핵심 연산자로 단순하면서도 효과적인 3 × 3 DCNv3를 채택하는 것이 더 나은 전략이라는 결론을 내립니다.

마지막으로 모델이 효과적으로 인식할 수 있는 이미지 영역을 이해하는 데 도움이 되는 유효 수용 필드(ERF)에 대한 분석을 제공합니다. 이 분석은 충분한 학습을 거친 후 InternImage가 전체 이미지의 정보를 효과적으로 인식할 수 있음을 보여줍니다.

![다른 백본들의 효과적인 수용 필드(ERF) 시각화. 활성화된 픽셀은 강아지의 눈에 있다. (a)와 (b)는 각각 ImageNet-1K [31]에서의 훈련 유/무에 따른 ResNet-101 [36]의 ERF를 보여준다. (c)와 (d)는 ImageNet-1K에서의 훈련 유/무에 따른 InternImage-B의 ERF이다.](InternImage%20Exploring%20Large-Scale%20Vision%20Foundatio%20d71e02986f3148da846c7a3a961fc52b/Untitled%204.png)

다른 백본들의 효과적인 수용 필드(ERF) 시각화. 활성화된 픽셀은 강아지의 눈에 있다. (a)와 (b)는 각각 ImageNet-1K [31]에서의 훈련 유/무에 따른 ResNet-101 [36]의 ERF를 보여준다. (c)와 (d)는 ImageNet-1K에서의 훈련 유/무에 따른 InternImage-B의 ERF이다.

C. 추가 다운스트림 작업 섹션에서는 분류, 객체 감지, 의미적 세분화 등 다양한 다운스트림 작업에서 InternImage-H 모델의 성능을 평가하기 위해 몇 가지 실험을 수행했습니다.

분류 작업에는 iNaturalist 2018, Places205, Places365 데이터 세트가 사용되었으며, 이 모델은 모든 데이터 세트에서 최첨단 정확도를 달성했습니다. 이 실험은 모델이 다양한 실내 및 실외 장면을 나타내는 데이터뿐만 아니라 불균형하고 세분화된 데이터에서도 학습할 수 있음을 보여줍니다.

객체 감지 측면에서 모델은 LVIS v1.0, 파스칼 VOC, OpenImages v6, CrowdHuman 및 BDD100K 데이터 세트를 사용하여 평가됩니다. 모든 경우에서 이 모델은 새로운 성능 기록을 세우며 붐비는 사람 장면부터 복잡한 운전 상황까지 다양한 시나리오에서 다양한 객체 클래스를 감지할 수 있는 능력을 보여줍니다.

마지막으로 이 모델은 COCO-Stuff, Pascal Context, Cityscapes, NYU Depth V2 데이터 세트를 사용하여 시맨틱 세분화 작업에 대해 테스트됩니다. 이 모델은 모든 테스트에서 인상적인 결과를 얻었으며, 이전 최고 모델의 성능을 개선했습니다. 이는 이 모델이 거리 뷰부터 깊이 정보가 있는 실내 환경까지 복잡한 장면의 의미론적 이해와 관련된 작업을 처리할 수 있음을 보여줍니다.

이 실험은 다양한 작업과 데이터 세트에서 우수한 성능을 발휘하는 모델의 능력을 강조하며, 다양한 컴퓨터 비전 과제를 처리할 수 있는 유연성과 능력을 보여줍니다.

처리량 분석에서 저자는 DCNv2, ConvNext, RepLKNet, 변형 가능한 주의(DAT)가 있는 비전 트랜스포머를 포함한 여러 다른 모델과 InternImage의 성능을 비교합니다. 인턴이미지 모델은 DCNv2 모델에 비해 우수한 파라미터 효율성과 빠른 추론 속도를 보여주었습니다. DCN 기반 연산자를 사용했기 때문에 ConvNeXt 모델과 처리량 차이가 있었지만, InternImage 모델은 여전히 정확도 면에서 우위를 보였습니다.

그런 다음 저자들은 번역, 회전, 크기 조정 등 다양한 이미지 변환에서 모델의 견고성을 테스트했습니다. 각각의 경우에서 모델은 강력한 견고성을 보여주었습니다. 변환 불변성의 경우, 이 모델이 가장 우수한 성능을 보였으며, 컨볼루션 기반 ConvNeXt, 글로벌 관심도 기반 PVTv2, 로컬 관심도 기반 Swin 트랜스포머가 그 뒤를 이었습니다. 회전 불변성 측면에서는 모든 모델이 작은 각도에서는 비슷한 성능을 보이지만, 큰 각도에서는 InternImage 모델이 다른 모델보다 우수한 성능을 보였습니다. 스케일링 불변성 평가에서도 이 모델은 특히 이미지를 스케일업할 때 다른 모델보다 성능이 뛰어났습니다.

저자들은 또한 데이터 규모에 대한 모델의 견고성을 테스트했습니다. 이 모델은 다양한 크기(1%, 10%, 100%)의 ImageNet-1K 데이터에 대해 학습되었습니다. 이 모델은 모든 데이터 세트에서 강력한 견고성을 보였으며, 데이터 규모에 대한 모델의 견고성을 나타내는 변형 가능한 주의(dattn) 및 ConvNeXt를 사용한 InternImage-T 변형보다 더 나은 성능을 보였습니다.

- 모델의 입력과 출력
    
    알려진 내용에 따르면, 모델의 입력은 일반적으로 이미지 또는 이미지 시퀀스입니다. 이 이미지는 모델이 처리할 수 있는 적절한 형식과 해상도로 변환되어야 합니다. 여기서 해상도는 종종 모델에 따라 다르며, 224×224나 800×800과 같은 크기가 널리 사용됩니다.
    
    모델의 출력은 문맥에 따라 다르지만, 이미지 분류 작업의 경우 출력은 각 클래스에 대한 확률 분포일 수 있습니다. 이 확률 분포는 모델이 이미지에 대해 예측하는 각 클래스의 확신도를 나타냅니다. 가장 높은 확률 값을 가진 클래스는 모델이 예측한 클래스가 됩니다.
    
    객체 감지 작업의 경우, 모델의 출력은 이미지 내에서 감지된 객체의 경계 상자 (bounding boxes) 및 각 경계 상자에 대한 클래스 라벨일 수 있습니다. 각 경계 상자는 일반적으로 이미지 내에서 객체의 위치와 크기를 정의하는 네 개의 좌표로 표시됩니다.
    
    주어진 문맥에 따라 모델의 입력과 출력은 달라질 수 있으므로, 위의 내용은 일반적인 경우를 설명한 것입니다. 특정 모델의 입력과 출력을 더 구체적으로 알려주시면 더욱 정확한 설명을 제공할 수 있습니다.
    
- 의의
    
    이 텍스트에서 제시된 InternImage 모델은 기존의 이미지 인식 기술과 여러 가지 방면에서 차별화됩니다. 여기에는 아래의 주요 포인트들이 포함됩니다:
    
    1. **용량 효율성과 추론 속도:** InternImage는 DCNv2 [28]와 같은 다른 모델과 비교했을 때 더 나은 매개 변수 효율성과 훨씬 빠른 추론 속도를 가집니다. 이는 224x224와 800x800 등의 다양한 입력 해상도에서도 동일합니다.
    2. **파라미터 제어:** InternImage는 모델 변형을 결정하는 데 4개의 하이퍼파라미터(C1, C0, L1, L3)만 사용하므로 모델을 구성하고 튜닝하는 데 더 많은 유연성을 제공합니다.
    3. **변형에 대한 강인성:** 모델은 이미지 변형에 대해 강인성을 보여주며, 특히 이미지가 평행 이동, 회전, 또는 크기 조정되었을 때 동일한 출력을 유지하는 능력을 평가합니다. 이러한 변환에 대한 강인성은 InternImage가 다른 모델들보다 더 뛰어납니다.
    4. **데이터 규모에 대한 강인성:** InternImage 모델은 데이터 규모에 대해서도 강인하며, 특히 작은 데이터 세트(예: ImageNet의 1% 및 10%)에서도 높은 성능을 보입니다. 이는 모델이 많은 양의 데이터가 필요하지 않음을 나타내고, 제한된 데이터에서도 효과적으로 학습할 수 있음을 의미합니다.
    
    이러한 차별점들은 InternImage를 다른 이미지 인식 기술에 비해 고유하게 만들며, 일부 작업에서는 향상된 성능을 제공합니다.