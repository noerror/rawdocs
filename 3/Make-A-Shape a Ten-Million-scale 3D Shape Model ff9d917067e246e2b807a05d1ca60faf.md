# Make-A-Shape: a Ten-Million-scale 3D Shape Model

[https://arxiv.org/abs/2401.11067](https://arxiv.org/abs/2401.11067)

- Jan 2024

![Make-A-Shape는 천만 개 이상의 다양한 3D 형상으로 훈련된 대규모 3D 생성 모델입니다. 위의 그림은 Make-A-Shape가 무조건적으로 다양한 객체 카테고리의 3D 형상을 생성할 수 있는 능력을 보여줍니다. 생성된 형상은 복잡한 기하학적 세부 사항, 그럴듯한 구조, 비평범한 위상 구조, 깨끗한 표면을 특징으로 합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled.png)

Make-A-Shape는 천만 개 이상의 다양한 3D 형상으로 훈련된 대규모 3D 생성 모델입니다. 위의 그림은 Make-A-Shape가 무조건적으로 다양한 객체 카테고리의 3D 형상을 생성할 수 있는 능력을 보여줍니다. 생성된 형상은 복잡한 기하학적 세부 사항, 그럴듯한 구조, 비평범한 위상 구조, 깨끗한 표면을 특징으로 합니다.

## 1. Introduction

대규모 생성 모델은 점점 더 사실적인 출력을 생성할 수 있는 능력을 갖추게 되면서 디자인, 마케팅, 전자상거래 등 다양한 상업적 응용 분야에서 크게 성공하고 있습니다. 이러한 성공은 대규모 3D 생성 모델의 개발 가능성에 대한 질문을 불러일으켰으며, 이러한 모델은 흥미로운 새로운 특성을 나타내고 다양한 응용 프로그램을 지원할 수 있을 것입니다. 그러나 현재의 3D 생성 모델은 품질 면에서 뒤처지거나, 작은 3D 데이터셋에 집중하거나, 단일 조건만 허용하는 경우가 많습니다.

2D 이미지에 비해 3D 데이터는 추가적인 공간 차원으로 인해 입력 변수의 수가 크게 증가하여 신경망의 네트워크 파라미터가 많아지고, 이는 특히 U-Net 기반 디퓨전 모델에서 메모리 집약적인 특징 맵을 생성하여 GPU 처리에 부담을 주고 훈련 시간을 연장시킵니다. 또한, 3D 데이터를 처리하는 데는 2D 이미지에 비해 저장 및 데이터 처리의 복잡성이 증가하여 훈련 시간과 비용이 상승합니다. 마지막으로, 3D 형상을 표현하는 여러 방법이 있으며, 효율적인 훈련을 위해 높은 표현 품질을 유지하면서도 압축성이 좋은 표현 방식을 선택하는 것이 중요합니다.

최근의 대규모 3D 생성 모델은 이러한 문제를 해결하기 위해 두 가지 주요 전략을 사용합니다. 첫 번째는 손실 입력 표현을 사용하여 모델이 처리해야 하는 입력 변수의 수를 줄이는 것이지만, 이는 중요한 세부 사항을 생략하여 형상을 충실하게 캡처하지 못하는 단점이 있습니다. 예를 들어, Point-E는 포인트 클라우드를 입력으로 사용하고, Shap-E는 잠재 벡터를 사용합니다. 두 번째 전략은 여러 뷰 이미지를 사용하여 형상을 표현하는 방법입니다. 이 접근 방식에서는 생성된 형상의 이미지를 생성하고 이를 실제 다중 뷰 이미지와 비교하여 학습합니다. 그러나 이러한 방법은 손실 계산을 위해 느리고 완전한 형상을 캡처하지 못하는 경우가 많습니다.

이러한 문제를 해결하기 위해, 우리는 대규모 모델 훈련을 위해 3D 형상을 인코딩하는 새로운 3D 표현 방식인 웨이블릿 트리 표현을 소개합니다. 이 표현 방식은 고해상도 서명 거리 함수(SDF) 그리드에 웨이블릿 분해를 적용하여 다양한 스케일의 세부 정보를 포함합니다. 이를 통해 큰 모델 훈련을 위한 효과적인 데이터 표현 방식을 제공합니다.

웨이블릿 트리 표현은 정보가 풍부한 세부 계수를 포함하여 형상의 세부 사항을 보다 압축적으로 포함할 수 있도록 설계되었습니다. 또한, 이 표현 방식은 효율적인 스트리밍과 훈련을 가능하게 하여 대규모 모델 훈련에 적합합니다. 예를 들어, 복잡하게 압축된 256^3 SDF 그리드를 스트리밍하고 로드하는 데 걸리는 시간은 266 밀리초인 반면, 우리의 표현 방식은 동일한 프로세스에 184 밀리초만 소요됩니다.

결론적으로, 우리의 생성 모델은 효율적으로 훈련될 수 있으며, 기존 방법에 비해 몇 초 만에 고품질의 형상을 생성할 수 있습니다. 우리는 우리의 생성 프레임워크를 Make-A-Shape라고 명명하였으며, 이 프레임워크는 다양한 객체 카테고리를 포함한 1천만 개 이상의 3D 형상 데이터셋에서 무조건적인 생성 모델과 다양한 입력 조건에서 확장 모델을 효과적으로 훈련할 수 있습니다.

![Make-A-Shape는 다양한 입력 모달리티에 대해 다양한 형상을 생성할 수 있습니다: 단일 뷰 이미지(행 1 및 2), 다중 뷰 이미지(행 3 및 4), 포인트 클라우드(행 5 및 6), 복셀(행 7 및 8), 불완전한 입력(마지막 행). 행 7 및 8의 복셀 해상도는 각각 16^3과 32^3입니다. 상위 8개 행에서 홀수 열은 입력을, 짝수 열은 생성된 형상을 보여줍니다. 마지막 행에서 열 1 및 4는 부분 입력을, 나머지 열은 다양한 완성된 형상을 보여줍니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%201.png)

Make-A-Shape는 다양한 입력 모달리티에 대해 다양한 형상을 생성할 수 있습니다: 단일 뷰 이미지(행 1 및 2), 다중 뷰 이미지(행 3 및 4), 포인트 클라우드(행 5 및 6), 복셀(행 7 및 8), 불완전한 입력(마지막 행). 행 7 및 8의 복셀 해상도는 각각 16^3과 32^3입니다. 상위 8개 행에서 홀수 열은 입력을, 짝수 열은 생성된 형상을 보여줍니다. 마지막 행에서 열 1 및 4는 부분 입력을, 나머지 열은 다양한 완성된 형상을 보여줍니다.

![형상의 SDF를 다양한 방법으로 재구성: (a) 원본 형상, (b) Point-E [62], (c) Shap-E [33], (d) 거친 계수 C0 [28], (e) 우리의 웨이블릿 트리 표현. 우리의 접근 방식(e)은 형상의 구조와 세부 사항을 더 충실하게 재구성할 수 있습니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%202.png)

형상의 SDF를 다양한 방법으로 재구성: (a) 원본 형상, (b) Point-E [62], (c) Shap-E [33], (d) 거친 계수 C0 [28], (e) 우리의 웨이블릿 트리 표현. 우리의 접근 방식(e)은 형상의 구조와 세부 사항을 더 충실하게 재구성할 수 있습니다.

## 2. Related Work

![우리의 생성 접근 방식 개요. (a) 형상은 먼저 절단 서명 거리 필드(TSDF)로 인코딩되고, 웨이블릿 트리 구조에서 다중 스케일 웨이블릿 계수로 분해됩니다. 우리는 계수 간의 관계를 활용하여 정보가 풍부한 계수를 추출하고 웨이블릿 트리 표현을 구성하는 서브밴드 계수 필터링 절차를 설계했습니다. (b) 웨이블릿 트리 표현을 관리 가능한 공간 해상도의 규칙적인 그리드 구조로 재배열하는 서브밴드 계수 패킹 방식을 제안하여 디노이징 디퓨전 모델을 통해 효과적으로 표현을 생성할 수 있습니다. (c) 다양한 서브밴드의 형상 정보를 균형 있게 유지하고 세부 계수의 희소성을 해결하기 위해 서브밴드 적응 훈련 전략을 공식화했습니다. 이를 통해 수백만 개의 3D 형상을 효율적으로 훈련할 수 있습니다. (d) 우리의 프레임워크는 다양한 모달리티를 조건으로 확장할 수 있습니다](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%203.png)

우리의 생성 접근 방식 개요. (a) 형상은 먼저 절단 서명 거리 필드(TSDF)로 인코딩되고, 웨이블릿 트리 구조에서 다중 스케일 웨이블릿 계수로 분해됩니다. 우리는 계수 간의 관계를 활용하여 정보가 풍부한 계수를 추출하고 웨이블릿 트리 표현을 구성하는 서브밴드 계수 필터링 절차를 설계했습니다. (b) 웨이블릿 트리 표현을 관리 가능한 공간 해상도의 규칙적인 그리드 구조로 재배열하는 서브밴드 계수 패킹 방식을 제안하여 디노이징 디퓨전 모델을 통해 효과적으로 표현을 생성할 수 있습니다. (c) 다양한 서브밴드의 형상 정보를 균형 있게 유지하고 세부 계수의 희소성을 해결하기 위해 서브밴드 적응 훈련 전략을 공식화했습니다. 이를 통해 수백만 개의 3D 형상을 효율적으로 훈련할 수 있습니다. (d) 우리의 프레임워크는 다양한 모달리티를 조건으로 확장할 수 있습니다

최근 몇 년간 다양한 3D 표현 방식과 딥러닝의 통합에 관한 연구가 활발히 진행되었습니다. 초기 연구는 주로 3D 컨볼루션 네트워크를 사용하여 볼륨 표현을 탐구했습니다. 예를 들어, 몇몇 연구는 멀티뷰 CNN을 사용하여 3D 형상을 여러 각도에서 렌더링한 이미지를 기반으로 처리하는 방법을 제안했습니다.

이후, 포인트 클라우드를 활용한 딥러닝 방법이 소개되었습니다. PointNet은 포인트 클라우드를 직접 처리할 수 있는 첫 번째 신경망 모델로, 이후 추가적인 귀납적 바이어스를 채택하여 포인트 클라우드를 처리하는 방법들이 등장했습니다. 또한, 신경망을 사용하여 서명 거리 함수(SDF)나 점유 필드를 예측하는 방법도 연구되었습니다. 이러한 신경 암묵적 표현 방식은 많은 주목을 받았습니다. 그 외에도, 메시(mesh)나 경계 표현(BREP)과 같은 명시적 표현 방식이 탐구되었으며, 이는 주로 분류 및 생성 응용 프로그램에 사용되었습니다.

최근에는 웨이블릿을 사용하여 SDF 신호를 다중 스케일 웨이블릿 계수로 분해하는 연구가 진행되었습니다. 그러나 이러한 방법들은 학습 효율성을 높이기 위해 고주파 세부 정보를 필터링하지만, 이는 형상의 충실도를 희생시키는 결과를 낳습니다. 반면, 본 연구에서는 고주파 세부 정보를 포함하는 새로운 웨이블릿 트리 표현 방식을 제안하여 3D 형상을 거의 손실 없이 압축적으로 인코딩할 수 있도록 하였습니다.

3D 생성 모델 분야의 초기 연구는 주로 생성적 적대 신경망(GAN)에 집중되었습니다. 이후, 오토인코더와 GAN을 결합한 방법이 사용되었으며, 이는 포인트 클라우드나 암묵적 표현을 처리하는 생성 모델을 가능하게 했습니다. 최근 연구들은 차별화 가능한 렌더링을 GAN과 결합하여 여러 뷰를 손실 신호로 사용하였습니다. 또한, 정규화 흐름이나 변분 오토인코더(VAE)를 기반으로 한 생성 모델도 탐구되었습니다. 자가 회귀 모델은 3D 생성 모델링에서 인기를 끌며 폭넓게 연구되었습니다.

고품질 이미지 생성을 위한 디퓨전 모델의 최근 발전으로 인해 3D 컨텍스트에서도 많은 관심을 받고 있습니다. 대부분의 접근 방식은 먼저 3D 표현을 벡터 양자화 VAE(VQ-VAE)로 학습한 후, 디퓨전 모델을 잠재 공간에 적용합니다. 본 연구에서는 VQ-VAE를 사용하지 않고 직접 3D 표현에서 디퓨전 모델을 훈련함으로써 정보 손실을 피하고자 합니다.

조건부 3D 생성 모델은 크게 두 가지로 나눌 수 있습니다. 첫 번째 그룹은 큰 2D 조건부 이미지 생성 모델을 활용하여 3D 장면이나 객체를 최적화하는 방법입니다. 이 접근 방식은 텍스트-3D, 이미지, 멀티뷰 이미지, 스케치 등을 조건으로 사용합니다. 그러나 이러한 방법은 비용이 많이 들고 실제 응용에 제한이 있습니다. 두 번째 그룹은 조건이 있는 데이터를 사용하여 조건부 생성 모델을 훈련하는 방법입니다. 이 모델들은 포인트 클라우드, 이미지, 저해상도 복셀, 스케치, 텍스트 등을 조건으로 사용하여 훈련됩니다.

본 연구에서는 큰 조건부 생성 모델을 훈련하여 빠른 생성을 가능하게 하고, 다양한 조건을 쉽게 통합할 수 있도록 합니다. 이를 통해 무조건적인 응용과 제로 샷 과제(예: 형상 완성)도 가능하게 합니다.

## 3. Overview

![입력 형상을 TSDF로 표현하여 웨이블릿 분해를 통해 재귀적으로 거친 계수(Ci)와 세부 계수({DLHi, DHLi, DHH_i})로 나눕니다. 3D의 경우, 각 분해 단계에서 7개의 서브밴드 세부 계수가 생성됩니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%204.png)

입력 형상을 TSDF로 표현하여 웨이블릿 분해를 통해 재귀적으로 거친 계수(Ci)와 세부 계수({DLHi, DHLi, DHH_i})로 나눕니다. 3D의 경우, 각 분해 단계에서 7개의 서브밴드 세부 계수가 생성됩니다.

이 논문에서는 수백만 개의 3D 형상에 대해 효율적으로 훈련할 수 있는 대규모 3D 생성 모델을 설계하기 위한 프레임워크를 제안합니다. 3D 데이터의 복잡성으로 인해 대규모로 효율적인 훈련을 수행하는 것은 매우 도전적인 작업입니다. 특히, 형상 생성의 품질과 훈련 속도를 최적화할 필요가 있습니다. 우리의 접근 방식은 다음 네 가지 주요 구성 요소로 구성됩니다:

1. **웨이블릿 트리 표현**:
먼저, 3D 형상을 대규모 훈련을 지원하기 위해 압축적이고 효율적이며 표현력이 높은 3D 표현으로 변환합니다. 이를 위해 각 형상을 고해상도 절단 서명 거리 함수(TSDF)로 인코딩하고, TSDF를 다중 스케일 웨이블릿 계수로 분해합니다. 계수 간의 관계를 활용하는 서브밴드 계수 필터링 절차를 설계하여, 정보가 풍부한 웨이블릿 구성 요소(거친 계수와 세부 계수)를 유지함으로써 효율적인 저장 및 스트리밍이 가능한 충실하면서도 압축적인 3D 형상 표현을 만듭니다.
2. **디퓨전 가능한 웨이블릿 트리 표현**:
다음으로, 웨이블릿 트리 표현을 디퓨전 모델과 호환되도록 변환합니다. 비록 우리의 표현 방식이 압축적이고 효율적이지만, 그 불규칙한 형식은 효과적인 형상 학습 및 생성을 방해할 수 있습니다. 이를 해결하기 위해, 서브밴드 계수를 관리 가능한 공간 해상도의 규칙적인 그리드로 재배열하는 서브밴드 계수 패킹 방식을 설계하여 형상 생성을 지원합니다.
3. **서브밴드 적응 훈련 전략**:
디퓨전 웨이블릿 트리 표현을 효과적으로 훈련하기 위한 방법을 개발합니다. 일반적으로 형상 정보는 서브밴드와 스케일에 따라 다양하며, 세부 계수는 매우 희소하지만 중요한 형상 세부 정보를 포함합니다. 표준 평균 제곱 오차(MSE) 손실을 적용할 경우, 모델이 균형을 잃고 세부 정보를 학습하는 데 비효율적일 수 있습니다. 이를 해결하기 위해, 다양한 서브밴드에서 중요한 계수에 집중하는 적응 훈련 전략을 도입합니다. 이를 통해 훈련 동안 형상 정보의 균형을 효과적으로 유지하고, 모델이 형상의 구조적 및 세부적 측면을 모두 학습하도록 합니다.
4. **조건부 생성을 위한 확장**:
마지막으로, 무조건적인 생성 외에도, 단일/다중 뷰 이미지, 복셀, 포인트 클라우드와 같은 다양한 조건에 따라 형상을 생성할 수 있도록 방법을 확장합니다. 본질적으로, 지정된 조건을 잠재 벡터로 인코딩한 후, 이 벡터들을 생성 네트워크에 주입하기 위해 여러 메커니즘을 활용합니다. 이로 인해, 우리의 새로운 표현 방식은 압축적이면서도 대부분의 형상 정보를 충실하게 유지할 수 있으며, 수백만 개의 3D 형상에서 대규모 생성 모델의 효과적인 훈련을 촉진할 수 있습니다.

종합적으로, 우리의 접근 방식은 효율적인 훈련과 고품질 형상 생성을 가능하게 하여 기존 방법에 비해 우수한 성능을 보입니다. 이 프레임워크를 Make-A-Shape라고 명명하였으며, 다양한 객체 카테고리를 포함한 광범위한 데이터셋에서 무조건적인 생성 모델과 다양한 입력 조건에서의 확장 모델을 효과적으로 훈련할 수 있습니다.

## 4. Wavelet-tree Representation

우리의 웨이블릿 트리 표현은 3D 형상을 고해상도 절단 서명 거리 함수(TSDF)로 변환한 후, 이를 웨이블릿 변환을 통해 다중 스케일 계수로 분해함으로써 시작됩니다. TSDF는 256^3 해상도로 설정되며, 웨이블릿 변환을 통해 거친 계수와 세부 계수 집합으로 나뉩니다.

![부모-자식 관계 개요. C0의 각 계수에 대해 웨이블릿 트리가 형성되며, 상위 수준의 계수가 부모로, 하위 수준의 계수가 자식으로 설정됩니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%205.png)

부모-자식 관계 개요. C0의 각 계수에 대해 웨이블릿 트리가 형성되며, 상위 수준의 계수가 부모로, 하위 수준의 계수가 자식으로 설정됩니다.

**웨이블릿 변환 과정**:
먼저 TSDF를 웨이블릿 변환하여 거친 계수(C0)와 세부 계수 집합(D0, D1, D2)을 얻습니다. 이 변환 과정은 다중 스케일로 진행되며, 각 단계에서 TSDF는 더 작은 거친 계수와 관련된 세부 계수들로 분해됩니다. 이 과정은 2D 예시로 설명되지만 실제 계산은 3D에서 수행됩니다. 세부 계수는 고주파 정보를 포함하고 있으며, 이 표현은 손실 없이 역 웨이블릿 변환을 통해 TSDF로 변환될 수 있습니다.

**웨이블릿 트리 및 계수 관계**:
우리는 각 거친 계수를 부모로, 관련된 세부 계수를 자식으로 간주하여 웨이블릿 트리로 구성합니다. 이 부모-자식 관계는 계층적으로 확장되며, 각 부모 계수는 자식 계수 집합과 함께 상위 계수로부터 재구성될 수 있습니다. 또한, 같은 부모를 공유하는 계수들은 형제 계수로 정의됩니다.

**관찰**:
우리는 웨이블릿 계수에 대해 네 가지 중요한 관찰을 했습니다:

1. 계수의 크기가 작을수록 해당 계수의 자식도 작은 크기를 가질 가능성이 높으며, 이는 형상에 미치는 영향이 적습니다.
2. 형제 계수들 사이에는 양의 상관관계가 있습니다.
3. 거친 계수(C0)는 대부분 0이 아니며, 형상 정보의 대부분을 포함합니다.
4. D2의 대부분의 계수는 중요하지 않으며, 이를 0으로 설정해도 TSDF의 충실도를 유지할 수 있습니다.

**서브밴드 계수 필터링**:
이 관찰을 바탕으로, 우리는 정보가 풍부한 계수를 선택적으로 필터링하여 압축적이면서도 충실한 표현을 만듭니다. C0의 모든 계수를 유지하고 D2의 모든 계수를 제외한 후, D0와 D1의 계수는 중요도에 따라 선택됩니다. 계수 위치를 평가하여 가장 정보가 풍부한 K개의 계수를 선택하고, 이들의 위치와 값을 저장하여 세부 구성 요소를 형성합니다.

이 과정을 통해, 우리는 256^3 해상도의 TSDF를 약 99.56%의 IOU로 효과적으로 압축할 수 있습니다. 이 표현은 백만 개 이상의 3D 형상을 효율적으로 처리할 수 있도록 데이터 스트리밍 및 로딩 시간을 44.5% 감소시킵니다.

![우리 표현의 세부 구성 요소. D0와 D1에서 정보가 풍부한 계수를 추출하고, 이들의 공간 위치와 함께 포장하여 세부 구성 요소를 형성합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%206.png)

우리 표현의 세부 구성 요소. D0와 D1에서 정보가 풍부한 계수를 추출하고, 이들의 공간 위치와 함께 포장하여 세부 구성 요소를 형성합니다.

**효율적인 처리**:
백만 개 이상의 3D 형상을 효율적으로 처리하기 위해, 우리는 웨이블릿 트리 표현을 사용하여 각 형상을 인코딩한 후 클라우드에 저장합니다. 이는 대규모 훈련을 위한 일회성 전처리 단계로, 데이터 스트리밍과 로딩 시간을 크게 줄여줍니다. 복잡한 압축 기술을 사용하지 않고도, 우리의 표현 방식은 훈련 효율성을 높일 수 있습니다.

## 5. Diffusible Wavelet-tree Representation

웨이블릿 트리 표현을 디퓨전 모델로 효과적으로 학습하고 생성할 수 있는 형식으로 변환하는 것이 목표입니다. 우리의 디퓨전 모델은 DDPM 프레임워크를 기반으로 하며, 생성 과정을 마코프 체인으로 공식화합니다. 이 체인은 두 가지 주요 과정으로 구성됩니다: (i) 전방 과정, 이는 데이터 샘플 𝑥0*x*0에 점진적으로 노이즈를 추가하여 단위 가우시안 분포로 변환하는 과정, (ii) 역방향 과정, 이는 노이즈가 있는 샘플에서 점진적으로 노이즈를 제거하여 원래의 샘플 𝑥0*x*0을 예측하는 생성 네트워크 𝜃*θ*를 포함합니다.

**문제점**:
웨이블릿 트리 표현은 데이터 스트리밍에 있어서는 압축적이고 효율적이지만, 훈련 중 몇 가지 도전에 직면합니다. 이 표현은 그리드 구조의 거친 구성 요소(C0)와 세 개의 불규칙한 배열로 구성된 세부 구성 요소를 포함합니다. 이러한 세부 구성 요소는 D0와 D1에서 파생됩니다. 단순히 이 표현을 디퓨전 타겟으로 처리하고 두 분기 네트워크를 사용하여 거친 구성 요소와 세부 구성 요소를 예측하는 접근 방식은 훈련의 수렴 문제와 모델 붕괴를 초래할 수 있습니다.

또 다른 접근 방식은 표현에서 추출한 계수를 평탄화하여 불규칙성을 피하는 것입니다. 이 방법은 거친 구성 요소 C0를 좌상단에 배치하고, D0와 D1의 추출된 세부 계수를 각각의 서브밴드 위치에 배열하는 방식으로 이루어집니다. 그러나 이 표현은 공간적으로 매우 커지며, 메모리 집약적인 특징 맵을 생성하여 GPU 메모리 사용 문제를 초래하고 계산 효율성을 저하시킵니다.

![디퓨전 가능한 웨이블릿 표현. 먼저, 우리의 웨이블릿 트리 표현에서 계수를 풀어 평탄화합니다(왼쪽). 관찰(iii)에 따라 형제 계수를 채널별로 연결하여 공간 해상도를 줄입니다(오른쪽). 여기서 C0의 각 계수를 D0의 세 자식 및 D1의 재구성된 후손(각각 1×1×4 크기)과 연결합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%207.png)

디퓨전 가능한 웨이블릿 표현. 먼저, 우리의 웨이블릿 트리 표현에서 계수를 풀어 평탄화합니다(왼쪽). 관찰(iii)에 따라 형제 계수를 채널별로 연결하여 공간 해상도를 줄입니다(오른쪽). 여기서 C0의 각 계수를 D0의 세 자식 및 D1의 재구성된 후손(각각 1×1×4 크기)과 연결합니다.

**서브밴드 계수 패킹**:
이러한 문제를 해결하기 위해, 우리는 효율적인 2D 이미지 생성을 위한 최근 연구에서 영감을 받아, 서브밴드 계수의 유사한 구조를 활용하여 형제 서브밴드 계수와 자식 계수를 효과적으로 패킹합니다. 이를 통해 계수를 관리 가능한 해상도의 규칙적인 그리드로 재배열하여 디퓨전 모델 타겟으로 사용합니다. 이 방식은 공간 해상도를 줄이고 채널 수를 증가시켜 메모리 집약적인 특징 맵의 사용을 피하고, 3D 형상 훈련 시 더 효율적인 계산을 가능하게 합니다. 이 전략은 네트워크 아키텍처에 적용될 때 GPU 메모리 사용을 약 64배 줄이고 훈련 속도를 크게 향상시킬 수 있습니다.

결론적으로, 우리의 디퓨전 가능한 웨이블릿 트리 표현은 대규모 3D 데이터셋에서 효율적으로 모델을 훈련할 수 있도록 하여, 형상 생성의 품질과 속도를 최적화합니다.

## 6. Subband Adaptive Training Strategy

우리의 목표는 디퓨전 가능한 웨이블릿 트리 표현을 효과적으로 훈련하여 고품질의 3D 형상을 생성하는 것입니다. 이 과정에서 주요 과제는 모델이 표현의 거친 구성 요소와 세부 구성 요소 모두를 잘 생성할 수 있도록 하는 것입니다. 이를 위해 우리는 서브밴드 적응 훈련 전략을 개발했습니다.

**기본 MSE 손실의 문제점**:
표준 평균 제곱 오차(MSE) 손실을 모든 계수에 동일하게 적용하는 경우, 훈련 중 품질 저하가 발생할 수 있습니다. 이는 두 가지 주요 요인 때문입니다. 첫째, 다양한 스케일에서 계수의 수가 불균형합니다. 예를 들어, 3D의 경우 |C0| : |D0| : |D1|의 비율은 1 : 7 : 56입니다. 따라서, 균일한 MSE 손실은 중요한 세부 계수를 과소평가할 수 있습니다. 둘째, 대부분의 세부 계수는 거의 0에 가까운 값으로, 중요한 고주파 정보를 포함하는 고비용 계수는 매우 적습니다. 이러한 불균형은 훈련 효율성을 떨어뜨립니다.

**초기 접근 방법**:
이 문제를 해결하기 위해, 우리는 C0, D0, D1에 대해 각각의 MSE 손실을 정의하고 이를 동일한 가중치로 결합하는 방법을 시도했습니다. 그러나 이 접근 방식은 세부 계수의 희소성 문제를 해결하지 못하며, 훈련 성능이 개선되지 않았습니다.

**서브밴드 적응 훈련 전략**:
우리는 세부 계수의 고비용 계수를 효과적으로 강조하면서도 나머지 계수도 균형 있게 고려하는 서브밴드 적응 훈련 전략을 개발했습니다. 구체적으로, 각 서브밴드에서 가장 큰 계수의 크기를 기준으로 중요한 계수를 선택하고, 이들의 공간 위치를 기록합니다. 이 과정에서 형제 계수들 간의 양의 상관관계를 활용하여 정보가 풍부한 계수 위치를 선택합니다.

훈련 손실은 다음과 같이 계산됩니다:
LMSE(𝐶0)+12(∑𝐷LMSE(𝐷[𝑃0])+∑𝐷LMSE(𝑅(𝐷[𝑃0′])))LMSE(*C*0)+21(∑*D*LMSE(*D*[*P*0])+∑*D*LMSE(*R*(*D*[*P*0′])))

여기서 𝐷*D*는 서브밴드, 𝑃0*P*0는 중요한 계수의 위치 집합, 𝑃0′*P*0′는 중요하지 않은 계수의 위치 집합, 𝑅*R*은 무작위로 선택된 일부 계수를 나타냅니다. 이러한 접근 방식은 모델이 중요한 정보에 집중하면서도 세부 정보를 학습할 수 있도록 합니다.

**효율적인 손실 계산**:
효율적인 훈련을 위해, 우리는 고정 크기의 이진 마스크를 사용하여 선택된 계수 위치를 나타내고, 이를 통해 생성 타겟과 네트워크 예측을 마스킹하여 MSE 손실을 계산합니다. 이 방법은 불규칙한 작업을 피하고, 훈련 속도를 높이는 데 도움이 됩니다.

결론적으로, 우리의 서브밴드 적응 훈련 전략은 중요한 고비용 계수를 효과적으로 학습하고, 모델이 형상의 구조적 및 세부적 측면을 모두 잘 학습할 수 있도록 합니다. 이를 통해 디퓨전 가능한 웨이블릿 트리 표현의 훈련 성능을 크게 향상시킵니다.

## 7. Extension for Conditional Generation

우리의 프레임워크는 무조건적인 생성뿐만 아니라 다양한 조건을 사용한 조건부 생성도 지원하도록 설계되었습니다. 이를 위해 각 조건을 잠재 벡터로 변환하고, 이 벡터를 생성 네트워크에 주입하여 조건부 생성이 가능하도록 합니다.

**조건 잠재 벡터**:
모든 입력 조건을 일련의 잠재 벡터로 변환하여 조건부 생성에 필요한 일반성을 유지합니다. 이러한 접근 방식은 각 조건에 대해 새로운 조건 메커니즘을 개발할 필요 없이 다양한 조건을 지원할 수 있게 합니다. 각 조건에 대한 인코더는 다음과 같습니다:

1. **단일 뷰 이미지**:
단일 뷰 이미지가 주어지면, CLIP L-14 이미지 인코더를 사용하여 이미지를 처리합니다. 인코더의 풀링 층 이전의 잠재 벡터를 조건 잠재 벡터로 사용합니다.
2. **다중 뷰 이미지**:
3D 모델의 네 개의 이미지를 55개의 미리 정의된 카메라 각도 중 하나에서 렌더링하여 제공받습니다. 각 이미지에 대해 CLIP L-14 이미지 인코더를 사용하여 이미지 잠재 벡터를 생성하고, 각 이미지의 카메라 각도에 해당하는 학습 가능한 카메라 잠재 벡터와 결합합니다. 이를 통해 조건 잠재 벡터를 형성합니다.
3. **3D 포인트 클라우드**:
주어진 포인트 클라우드를 PointNet과 유사하게 세 개의 MLP 층을 사용하여 피처 벡터로 변환하고, 이를 Set Transformer의 PMA 블록을 사용하여 잠재 벡터로 집계합니다.
4. **복셀**:
입력 3D 복셀을 두 개의 3D 컨볼루션 층을 통해 점진적으로 다운샘플링하여 3D 피처 볼륨으로 변환하고, 이를 잠재 벡터로 평탄화합니다.

**네트워크 아키텍처**:
생성 네트워크는 U-ViT 아키텍처를 따릅니다. 주 분기(노란색 상자)는 다중 ResNet 컨볼루션 층을 사용하여 노이즈가 있는 계수를 피처 병목 볼륨으로 다운샘플링하고, 이어서 여러 주의층을 적용합니다. 이후, 디컨볼루션 층을 사용하여 노이즈를 제거한 계수를 생성합니다. 중요한 특징은 컨볼루션 및 디컨볼루션 블록 간의 학습 가능한 스킵 연결을 포함하여 안정성과 정보 공유를 향상시킨다는 점입니다.

![우리의 생성 네트워크는 입력 계수를 점진적으로 병목 피처 볼륨으로 다운샘플링합니다(중간). 이 볼륨은 주의층과 디컨볼루션을 거쳐 업샘플링되어 디노이즈된 계수를 예측합니다. 조건 잠재 벡터가 있을 경우, 이 벡터들을 세 위치에서 동시에 변환하여 아키텍처에 도입합니다: (i) 입력 노이즈 계수와 결합(녹색 화살표), (ii) 컨볼루션 및 디컨볼루션 블록 조건화(파란색 화살표), (iii) 병목 볼륨과의 교차 주의(빨간색 화살표).](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%208.png)

우리의 생성 네트워크는 입력 계수를 점진적으로 병목 피처 볼륨으로 다운샘플링합니다(중간). 이 볼륨은 주의층과 디컨볼루션을 거쳐 업샘플링되어 디노이즈된 계수를 예측합니다. 조건 잠재 벡터가 있을 경우, 이 벡터들을 세 위치에서 동시에 변환하여 아키텍처에 도입합니다: (i) 입력 노이즈 계수와 결합(녹색 화살표), (ii) 컨볼루션 및 디컨볼루션 블록 조건화(파란색 화살표), (iii) 병목 볼륨과의 교차 주의(빨간색 화살표).

조건 잠재 벡터가 있을 경우, 이를 생성 네트워크에 세 가지 위치에서 통합합니다:

1. 초기 잠재 벡터를 MLP 층과 풀링 층을 통해 단일 잠재 벡터로 변환한 후, 입력 노이즈 계수에 추가 채널로 결합합니다.
2. 잠재 벡터를 변환하여 그룹 정규화 층의 어파인 파라미터를 조정하여 컨볼루션 및 디컨볼루션 층을 조건화합니다.
3. 병목 볼륨을 조건화하기 위해 추가적인 위치 인코딩을 조건 잠재 벡터에 적용하고, 이를 병목 볼륨과의 교차 주의 연산에 사용합니다.

결론적으로, 우리의 프레임워크는 다양한 조건을 활용하여 3D 형상을 생성할 수 있도록 확장되어, 이미지, 복셀, 포인트 클라우드와 같은 다양한 입력 조건에 대해 높은 품질의 형상을 빠르게 생성할 수 있습니다. 이러한 접근 방식은 무조건적 생성 및 조건부 생성 모두에서 뛰어난 성능을 발휘하며, 다양한 응용 프로그램에 적용될 수 있습니다.

## 8. Results

이 섹션에서는 실험 설정, 정량적 및 정성적 결과, 그리고 다양한 입력 조건에 대한 성능을 제시합니다. 또한, 우리의 생성 모델이 추가 훈련 없이 형상 완성 작업에 적응할 수 있음을 보여주고, 프레임워크에 대한 종합적인 분석 및 어블레이션 연구를 제공합니다.

**8.1. 실험 설정**

**데이터셋**:
우리는 18개의 공개된 서브 데이터셋에서 1천만 개 이상의 3D 형상을 포함하는 대규모 데이터셋을 구성했습니다. 각 서브 데이터셋은 특정 객체 클래스(예: CAD 모델, 가구, 인간, 동물 등)를 포함하며, Objaverse와 ObjaverseXL은 인터넷에서 수집된 일반적인 객체들을 포함합니다. 각 서브 데이터셋을 98%의 훈련 세트와 2%의 테스트 세트로 나누어 최종 훈련 및 테스트 세트를 구성했습니다. 각 형상에 대해 TSDF와 웨이블릿 트리 표현을 생성하여 모델 훈련에 사용했습니다.

**훈련 세부 사항**:
Adam Optimizer를 사용하여 학습률 1e-4와 배치 크기 96으로 모델을 훈련했습니다. 모델 업데이트에는 0.9999의 감쇠율을 가진 지수 이동 평균을 사용했습니다. 모델은 48개의 A10G GPU에서 2백만에서 4백만 회 반복하여 훈련되었습니다.

**평가 데이터셋**:
정량적 평가를 위해, 각 서브 데이터셋의 테스트 세트에서 임의로 선택된 50개의 형상을 포함한 평가 세트를 준비했습니다. 이 평가 세트를 "Our Val" 데이터셋으로 지칭합니다. 추가적으로, Google Scanned Objects (GSO) 데이터셋을 사용하여 교차 도메인 일반화 성능을 평가했습니다.

**평가 지표**:
조건부 생성 작업에서는 생성된 형상과 실제 형상 간의 유사성을 평가하기 위해 IoU(Intersection over Union)와 LFD(Light Field Distance)를 사용했습니다. 무조건부 생성 작업에서는 Frechet Inception Distance (FID)를 사용하여 생성 성능을 평가했습니다.

**8.2. 다른 대규모 생성 모델과의 정량적 비교**:
단일 뷰 및 다중 뷰 설정에서 다른 대규모 이미지-3D 생성 모델과 비교한 결과, 우리의 모델이 IoU와 LFD 지표에서 모든 기준 모델을 상당히 능가함을 확인했습니다. 단일 뷰 모델은 단일 이미지를 입력으로 사용하고, 다중 뷰 모델은 네 개의 이미지를 추가 정보로 사용합니다. 다중 뷰 모델의 결과는 더 나은 전반적인 형상 포착을 보여주었으며, 이는 네 개의 뷰가 더 많은 정보를 제공하기 때문입니다.

![이미지-3D 생성 작업에 대한 시각적 비교는 우리의 방법이 Point-E [62], Shap-E [33], One-2-3-45 [43] 세 가지 주요 생성 모델보다 우수함을 보여줍니다. 우리의 단일 뷰 모델은 이 기준 모델들보다 더 정확한 형상을 생성하며, 다중 뷰 모델은 추가 뷰 정보를 통해 형상 충실도를 더욱 향상시킵니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%209.png)

이미지-3D 생성 작업에 대한 시각적 비교는 우리의 방법이 Point-E [62], Shap-E [33], One-2-3-45 [43] 세 가지 주요 생성 모델보다 우수함을 보여줍니다. 우리의 단일 뷰 모델은 이 기준 모델들보다 더 정확한 형상을 생성하며, 다중 뷰 모델은 추가 뷰 정보를 통해 형상 충실도를 더욱 향상시킵니다.

**8.3. 이미지-3D 생성**:
추가적인 단일 뷰 및 다중 뷰 조건의 결과는 우리의 방법이 다양한 객체 카테고리의 형상을 생성할 수 있음을 보여주었습니다. 우리의 모델은 입력 이미지의 가시적인 부분을 충실히 재현할 뿐만 아니라, 보이지 않는 부분도 창의적으로 재구성할 수 있습니다.

![우리 모델은 단일 입력 이미지로부터 다양한 결과를 생성할 수 있으며, 보이는 부분을 정확하게 재현하면서 보이지 않는 영역에 대해 다양한 형태를 제공합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2010.png)

우리 모델은 단일 입력 이미지로부터 다양한 결과를 생성할 수 있으며, 보이는 부분을 정확하게 재현하면서 보이지 않는 영역에 대해 다양한 형태를 제공합니다.

![우리의 단일 뷰 조건부 생성 모델은 다양한 형상을 생성합니다. 우리의 모델은 스크류, 의자, 자동차와 같은 CAD 객체뿐만 아니라 인간, 동물, 식물과 같은 유기적 형상도 능숙하게 생성합니다.우리의 단일 뷰 조건부 생성 모델은 다양한 형상을 생성합니다. 우리의 모델은 스크류, 의자, 자동차와 같은 CAD 객체뿐만 아니라 인간, 동물, 식물과 같은 유기적 형상도 능숙하게 생성합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2011.png)

우리의 단일 뷰 조건부 생성 모델은 다양한 형상을 생성합니다. 우리의 모델은 스크류, 의자, 자동차와 같은 CAD 객체뿐만 아니라 인간, 동물, 식물과 같은 유기적 형상도 능숙하게 생성합니다.우리의 단일 뷰 조건부 생성 모델은 다양한 형상을 생성합니다. 우리의 모델은 스크류, 의자, 자동차와 같은 CAD 객체뿐만 아니라 인간, 동물, 식물과 같은 유기적 형상도 능숙하게 생성합니다.

![우리의 다중 뷰 조건부 생성 모델은 다중 뷰 정보를 활용하여 복잡한 위상을 가진 다양한 형상을 생성할 수 있으며, 이는 첫 줄의 CAD 객체 예시로 잘 나타납니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2012.png)

우리의 다중 뷰 조건부 생성 모델은 다중 뷰 정보를 활용하여 복잡한 위상을 가진 다양한 형상을 생성할 수 있으며, 이는 첫 줄의 CAD 객체 예시로 잘 나타납니다.

**8.4. 포인트-3D 생성**:
포인트 클라우드를 입력으로 사용하여 3D 형상을 생성한 결과, 포인트 수에 따라 성능이 향상됨을 확인했습니다. 적은 수의 포인트로도 우리의 방법은 합리적인 IoU를 달성했습니다.

![입력 포인트 수에 따른 시각적 비교는 우리의 모델이 최소 5000개의 포인트로도 사슴 뿔이나 의자 다리와 같은 얇은 구조를 강력하게 생성할 수 있음을 강조합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2013.png)

입력 포인트 수에 따른 시각적 비교는 우리의 모델이 최소 5000개의 포인트로도 사슴 뿔이나 의자 다리와 같은 얇은 구조를 강력하게 생성할 수 있음을 강조합니다.

![우리의 포인트 클라우드 조건부 생성 결과는 모델이 복잡한 기하학적 세부 사항을 유지하면서 입력 포인트 클라우드의 복잡한 위상을 보존하는 형상을 생성할 수 있음을 보여줍니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2014.png)

우리의 포인트 클라우드 조건부 생성 결과는 모델이 복잡한 기하학적 세부 사항을 유지하면서 입력 포인트 클라우드의 복잡한 위상을 보존하는 형상을 생성할 수 있음을 보여줍니다.

**8.5. 복셀-3D 생성**:
저해상도 복셀을 입력으로 사용하여 3D 형상을 생성한 결과, 우리의 모델이 매끄럽고 깨끗한 표면을 생성할 수 있음을 확인했습니다. 또한, 전통적인 인터폴레이션 방법과 비교하여 우리의 방법이 월등히 우수한 성능을 보였습니다.

![우리의 복셀 조건부 생성 모델은 저해상도 입력으로부터 높은 품질의 출력을 생성하는 데 뛰어나며, 다양한 그럴듯한 기하학적 패턴을 도입합니다. 이는 입력에 없었던 크라운의 구멍을 생성한 예시로 잘 나타납니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2015.png)

우리의 복셀 조건부 생성 모델은 저해상도 입력으로부터 높은 품질의 출력을 생성하는 데 뛰어나며, 다양한 그럴듯한 기하학적 패턴을 도입합니다. 이는 입력에 없었던 크라운의 구멍을 생성한 예시로 잘 나타납니다.

![최근접 이웃 업샘플링과 삼차 보간법을 사용한 인터폴레이션으로 생성된 메시와 비교하여, 우리의 생성 결과는 현저히 매끄러운 표면을 보여줍니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2016.png)

최근접 이웃 업샘플링과 삼차 보간법을 사용한 인터폴레이션으로 생성된 메시와 비교하여, 우리의 생성 결과는 현저히 매끄러운 표면을 보여줍니다.

![불완전한 입력(왼쪽)이 주어졌을 때, 우리의 무조건 생성 모델은 누락된 영역을 일관되고 의미 있는 방식으로 완성합니다. 또한, 완성된 형상의 다양한 변형을 생성할 수 있으며, 이는 학습된 다양한 형상 분포를 강조합니다.](Make-A-Shape%20a%20Ten-Million-scale%203D%20Shape%20Model%20ff9d917067e246e2b807a05d1ca60faf/Untitled%2017.png)

불완전한 입력(왼쪽)이 주어졌을 때, 우리의 무조건 생성 모델은 누락된 영역을 일관되고 의미 있는 방식으로 완성합니다. 또한, 완성된 형상의 다양한 변형을 생성할 수 있으며, 이는 학습된 다양한 형상 분포를 강조합니다.

**8.6. 3D 형상 완성**:
훈련된 무조건부 생성 모델을 사용하여 부분적인 입력 형상을 기반으로 다양한 변형을 생성할 수 있음을 확인했습니다. 이는 모델이 부분 형상에 대한 다양한 변형을 생성할 수 있는 능력을 보여줍니다.

**8.7. 어블레이션 연구**:
분류기-없는 가이던스와 추론 시간 단계 분석을 통해, 분류기-없는 가이던스 가중치와 추론 시간 단계가 생성 품질에 미치는 영향을 분석했습니다. 또한, 서브밴드 적응 훈련 전략의 유효성을 검증하여, 이 전략이 모델 성능을 크게 향상시킴을 확인했습니다.

**결론**:
우리의 생성 모델은 다양한 조건에서 높은 품질의 3D 형상을 효율적으로 생성할 수 있으며, 이는 기존의 방법들보다 우수한 성능을 보여줍니다. 다양한 조건을 지원하는 능력과 함께, 우리의 접근 방식은 3D 형상 생성 및 완성 작업에서 뛰어난 성능을 발휘합니다.

## 9. Limitations and Future Works

우리의 접근 방식은 몇 가지 한계를 가지고 있으며, 향후 연구 방향으로 확장될 수 있는 부분이 있습니다:

1. **불균형 데이터 분포**:
무조건 모델은 다양한 객체를 생성할 수 있지만, 학습된 3D 형상 분포가 객체 카테고리별로 균형 잡히지 않을 수 있습니다. 이는 CAD 모델이 과도하게 대표되는 결과를 초래할 수 있습니다. 이를 해결하기 위해 대규모 제로 샷 언어 모델(예: ChatGPT)을 사용하여 객체 카테고리를 주석 달고, 데이터 증강 기법을 통해 학습 데이터를 균형 있게 만드는 방법을 고려할 수 있습니다.
2. **카테고리 레이블 사용 부족**:
우리의 생성 네트워크는 다양한 데이터셋을 혼합하여 훈련되지만, 객체 카테고리 레이블을 추가 조건으로 사용하지 않습니다. 이로 인해 때때로 불가능한 형상을 생성하거나 출력에 노이즈가 포함될 수 있습니다. 이러한 이상 현상을 식별하고 완화하는 연구가 필요합니다. 특히, 대규모 3D 데이터셋을 사용할 때 생성된 3D 형상의 시각적 타당성을 평가하기 위한 데이터 중심의 메트릭 개발이 흥미로운 방향이 될 것입니다.
3. **텍스처 생성**:
현재 우리는 주로 3D 형상의 직접 생성에 초점을 맞추고 있으며, 텍스처 생성은 다루지 않았습니다. 향후 연구에서는 기하학적 형상과 함께 텍스처를 생성하는 방법을 탐구할 필요가 있습니다. 이를 위해 계산적으로 비용이 많이 드는 최적화를 피하면서 텍스처와 형상을 함께 생성할 수 있는 방법을 모색할 수 있습니다.

## 10. Conclusion

이 논문에서는 Make-A-Shape라는 새로운 3D 생성 프레임워크를 제안했습니다. 이 프레임워크는 1천만 개 이상의 공개된 3D 형상 데이터셋을 기반으로 훈련되었으며, 2초 이내에 고품질의 3D 형상을 생성할 수 있습니다. 우리의 접근 방식은 다음과 같은 새로운 기술을 도입하여 이루어졌습니다:

1. **서브밴드 계수 필터링**:
압축적이고 표현력이 뛰어나며 효율적인 웨이블릿 트리 표현을 구성하여, 최소한의 정보 손실로 256^3 SDF를 효과적으로 인코딩할 수 있습니다.
2. **디퓨전 모델을 위한 서브밴드 계수 패킹**:
웨이블릿 트리 표현을 디퓨전 모델로 훈련할 수 있도록 변환하여, 고해상도에서 효율적인 생성을 가능하게 합니다.
3. **서브밴드 적응 훈련 전략**:
중요한 정보가 포함된 계수를 효과적으로 학습할 수 있도록 하여, 모델이 거친 형상 정보와 세부 형상 정보를 모두 잘 학습할 수 있도록 합니다.

또한, 우리는 Make-A-Shape를 다양한 조건부 입력(단일/다중 뷰 이미지, 포인트 클라우드, 저해상도 복셀)으로 확장하여 조건부 생성이 가능하도록 했습니다. 우리의 광범위한 실험 결과, 제안된 모델은 다양한 조건에서 고품질의 3D 형상을 생성할 수 있으며, 기존의 기준 모델들을 능가하는 성능을 보여주었습니다. 또한, 제로 샷 응용 프로그램(예: 형상 완성)에서도 우수한 성능을 입증했습니다.

우리의 연구는 다른 3D 표현 방식에서도 대규모 3D 모델 훈련을 가능하게 하는 길을 열어줄 것으로 기대됩니다.