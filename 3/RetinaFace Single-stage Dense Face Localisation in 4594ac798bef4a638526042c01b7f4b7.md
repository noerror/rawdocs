# RetinaFace: Single-stage Dense Face Localisation in the Wild

*이 논문에서는 다양한 스케일의 이미지에서 얼굴을 조밀하게 로컬라이즈하고 정렬하는 새로운 1단계 솔루션인 RetinaFace를 소개합니다. 저자는 광범위한 실험을 통해 RetinaFace가 까다로운 얼굴 인식 벤치마크에서 다른 최신 방법보다 뛰어난 성능을 발휘한다는 것을 보여줍니다. 또한 선도적인 얼굴 인식 방식과 결합하면 정확도가 크게 향상됩니다. 연구진은 이 분야의 추가 연구를 지원하기 위해 데이터와 모델을 공개했습니다.*

[https://arxiv.org/pdf/1905.00641.pdf](https://arxiv.org/pdf/1905.00641.pdf)

[https://arxiv.org/abs/1905.00641](https://arxiv.org/abs/1905.00641)

- May 2019

### 1. Introduction

이 백서에서는 자동 얼굴 로컬라이제이션에 대한 고급 접근 방식인 RetinaFace를 소개합니다. 얼굴의 경계 상자만 추정하는 기존의 얼굴 감지와 달리 RetinaFace는 다양한 스케일에 걸쳐 얼굴 위치에 대한 보다 정확한 정보를 제공하도록 설계되었습니다. 이는 일반적인 객체 감지 방법에 사용되는 딥 러닝의 최근 발전을 수용하여 달성합니다.

![제안된 단일 단계 픽셀 단위 얼굴 위치 지정 방법은 기존 박스 분류 및 회귀 분기와 병렬로 과도한 감독 및 자기 감독 멀티태스크 학습을 사용합니다. 각 양성 앵커는 (1) 얼굴 점수, (2) 얼굴 상자, (3) 다섯 개의 얼굴 랜드마크, 및 (4) 이미지 평면에 투영된 밀집 3D 얼굴 버텍스를 출력합니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled.png)

제안된 단일 단계 픽셀 단위 얼굴 위치 지정 방법은 기존 박스 분류 및 회귀 분기와 병렬로 과도한 감독 및 자기 감독 멀티태스크 학습을 사용합니다. 각 양성 앵커는 (1) 얼굴 점수, (2) 얼굴 상자, (3) 다섯 개의 얼굴 랜드마크, 및 (4) 이미지 평면에 투영된 밀집 3D 얼굴 버텍스를 출력합니다.

이 방법은 얼굴 감지를 넘어 얼굴 정렬, 픽셀 단위 얼굴 파싱, 3D 고밀도 대응 회귀와 같은 추가 기능을 포함합니다. 이러한 고밀도 얼굴 로컬라이제이션 기술은 다양한 규모에 걸쳐 정확한 얼굴 위치 데이터를 제공합니다.

이 시스템은 멀티태스크 학습 전략을 사용하여 이전의 단일 단계 얼굴 감지 프레임워크를 개선합니다. 얼굴 점수, 바운딩 박스, 5개의 얼굴 랜드마크, 각 얼굴 픽셀의 3D 위치 및 대응을 동시에 예측합니다.

레티나페이스는 주목할 만한 성공을 거두었습니다. 예를 들어, 넓은 얼굴 하드 서브셋에서 최첨단 방법보다 뛰어난 성능을 발휘하여 평균 정밀도(AP) 점수를 1.1% 개선하여 91.4%에 도달했습니다. IJB-C 데이터 세트에서는 ArcFace의 검증 정확도를 개선하여 더 나은 얼굴 로컬라이제이션이 얼굴 인식을 크게 향상시킬 수 있음을 시사했습니다.

고급 기능에도 불구하고 RetinaFace는 단일 CPU 코어에서 VGA 해상도 이미지에 대해 실시간으로 작동할 수 있어 효율적입니다. 저자들은 향후 연구를 지원하기 위해 추가 주석과 코드를 공개했습니다.

### 2. Related Work

이 논문에서는 얼굴 인식 및 로컬라이제이션과 관련된 여러 선행 연구와 접근 방식에 대해 설명합니다. 다음은 몇 가지 핵심 사항입니다:

이미지 피라미드 대 특징 피라미드: 조밀한 이미지 그리드에서 분류기를 사용하는 슬라이딩 윈도우 패러다임은 수십 년 동안 널리 사용되어 왔습니다. 이 분야에서 중요한 발전은 캐스케이드 체인을 사용하여 이미지 피라미드에서 잘못된 얼굴 영역을 빠르게 거부하는 Viola-Jones 방식이었습니다. 그러나 피처 피라미드와 슬라이딩 앵커 기법이 도입되면서 얼굴 감지에 선호되는 접근 방식으로 빠르게 자리 잡았습니다.

2단계 방식과 단일 단계 방식: 현재 얼굴 인식 방식은 2단계 또는 단일 단계로 분류되는 경우가 많으며, 이 두 가지 방식은 일반적인 객체 인식의 발전에 영향을 받았습니다. 2단계 방법은 "제안 및 개선" 메커니즘을 사용하여 높은 위치 정확도를 제공합니다. 반면, 단일 단계 방식은 얼굴 위치와 스케일을 조밀하게 샘플링하기 때문에 훈련 중에 양성 및 음성 샘플의 불균형이 발생합니다. 일반적으로 더 효율적이고 리콜률이 높지만 오탐률이 높고 로컬라이제이션 정확도가 떨어질 위험이 있습니다.

컨텍스트 모델링: 모델의 문맥 추론 능력을 향상시키는 것은 특히 작은 얼굴을 캡처하는 데 중요합니다. SSH 및 PyramidBox와 같은 기술은 특징 피라미드에 컨텍스트 모듈을 사용하여 수용 영역을 늘립니다. 변형 컨볼루션 네트워크(DCN)는 고유한 변형 가능한 레이어를 사용하여 기하학적 변형을 모델링합니다.

멀티태스크 학습: 정렬된 얼굴 모양이 얼굴 분류에 더 나은 특징을 제공하기 때문에 얼굴 감지와 정렬을 결합하는 접근 방식이 널리 사용되어 왔습니다. Mask R-CNN 및 DensePose와 같은 기술은 기존 가지에 더해 객체 마스크 또는 밀집된 부분 레이블과 좌표를 예측하는 가지를 추가하여 감지 성능을 향상시켰습니다. 이러한 분기는 일반적으로 지도 학습에 의해 학습되며 픽셀 대 픽셀의 고밀도 매핑을 예측하는 작업을 포함합니다.

### 3. RetinaFace

RetinaFace는 정교한 얼굴 인식 시스템입니다. 시스템의 훈련 과정에는 얼굴 분류 손실, 얼굴 상자 회귀 손실, 얼굴 랜드마크 회귀 손실, 고밀도 회귀 손실의 네 가지 주요 구성 요소로 나뉘는 다중 작업 손실 함수를 최소화하는 작업이 포함됩니다.

![제안된 단일 단계 밀집 얼굴 위치 지정 접근법 개요. RetinaFace는 독립적인 컨텍스트 모듈을 기반으로 설계되었습니다. 컨텍스트 모듈을 따라 각 앵커에 대한 멀티태스크 손실을 계산합니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%201.png)

제안된 단일 단계 밀집 얼굴 위치 지정 접근법 개요. RetinaFace는 독립적인 컨텍스트 모듈을 기반으로 설계되었습니다. 컨텍스트 모듈을 따라 각 앵커에 대한 멀티태스크 손실을 계산합니다.

얼굴 분류 손실(Lcls): 손실 함수의 이 구성 요소는 주어진 앵커가 얼굴인지 아닌지를 결정하는 이진 분류 작업과 관련이 있습니다. 앵커가 얼굴일 것으로 예측되는 확률은 pi로 표시되며, 기준 진실(실제로 얼굴인지 아닌지 여부)은 p∗i로 표시됩니다. 분류 손실은 이러한 이진 클래스에 대해 소프트맥스 손실 함수를 사용하여 계산됩니다.

얼굴 상자 회귀 손실(Lbox): 이 손실은 이미지에서 얼굴의 경계 상자를 식별하는 회귀 작업과 관련이 있습니다. 이 손실은 예측된 박스 좌표(ti)와 양수 앵커와 연관된 지상 실측 박스 좌표(t∗i)를 사용합니다. 이 손실은 강력한 손실 함수(smooth-L1)를 사용하여 중심 위치, 너비, 높이에 대해 정규화된 이러한 좌표를 비교합니다.

![(a) 2D 합성곱은 유클리드 그리드 수용 영역 내에서 커널 가중치 이웃 합입니다. 각 합성곱 레이어에는 KernelH × KernelW × Channelin × Channelout 매개 변수가 있습니다. (b) 그래프 합성곱도 커널 가중치 이웃 합의 형태이지만, 이웃 거리는 두 개의 버텍스를 연결하는 최소 에지 수를 그래프에서 계산합니다. 각 합성곱 레이어에는 K ×Channelin × Channelout 매개 변수가 있으며, 체비셰프 계수 θi,j ∈ R K는 K 순서에서 잘려냅니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%202.png)

(a) 2D 합성곱은 유클리드 그리드 수용 영역 내에서 커널 가중치 이웃 합입니다. 각 합성곱 레이어에는 KernelH × KernelW × Channelin × Channelout 매개 변수가 있습니다. (b) 그래프 합성곱도 커널 가중치 이웃 합의 형태이지만, 이웃 거리는 두 개의 버텍스를 연결하는 최소 에지 수를 그래프에서 계산합니다. 각 합성곱 레이어에는 K ×Channelin × Channelout 매개 변수가 있으며, 체비셰프 계수 θi,j ∈ R K는 K 순서에서 잘려냅니다.

얼굴 랜드마크 회귀 손실(Lpts): 이 손실은 이미지에서 얼굴 랜드마크를 식별하는 회귀 작업과 관련이 있습니다. 예측된 얼굴 랜드마크(li)와 양수 앵커와 관련된 지상 실측 랜드마크(l∗i)를 사용합니다. 손실은 박스 회귀와 유사하게 계산되며, 앵커 중심을 기준으로 목표 정규화를 사용합니다.

고밀도 회귀 손실(L픽셀): 이 손실은 고밀도 픽셀 예측의 회귀 작업과 관련이 있습니다. 렌더링된 2D 얼굴과 원본 2D 얼굴을 픽셀 단위로 비교합니다.

RetinaFace에는 메시 디코더와 차별 렌더러로 구성된 고밀도 회귀 브랜치도 포함되어 있습니다.

메시 디코더: 메시 디코더는 빠른 국소 스펙트럼 필터링을 기반으로 하는 그래프 컨볼루션 방식으로, 조인트 모양 및 텍스처 디코더를 사용하여 가속을 달성합니다.

차별화 렌더러: 모양과 텍스처 파라미터를 예측한 후 효율적인 차등 3D 메시 렌더러를 사용하여 주어진 카메라 및 조명 파라미터를 사용하여 컬러 메시를 2D 이미지 평면에 투사합니다.

마지막으로 이러한 손실의 가중치는 각각 0.25, 0.1, 0.01로 설정된 파라미터 λ1, λ2, λ3에 의해 균형을 맞춰 박스 및 랜드마크 위치를 개선하는 데 더 큰 의미를 부여합니다.

### 4. Experiments

연구진은 규모, 포즈, 표정, 오클루전, 조명 등 얼굴 특징에 상당한 수준의 가변성이 있는 WIDER FACE 데이터 세트를 사용하여 실험을 진행했습니다. 이 데이터 세트는 훈련, 검증, 테스트 하위 집합으로 나뉘며, 난이도가 높은 샘플이 점점 더 많이 포함됨에 따라 세 가지 난이도(쉬움, 중간, 어려움)로 나뉘어져 있습니다. 또한 연구진은 이미지의 얼굴 랜드마크에 대한 추가 주석을 제공했습니다.

구현 세부 사항을 살펴보면, RetinaFace 시스템은 다양한 스케일의 물체를 감지하는 데 사용되는 피처 피라미드로 구축되었습니다. 또한 이 네트워크는 초기 특징 추출을 위해 ImageNet-11k 데이터세트에서 훈련된 사전 훈련된 ResNet-152 네트워크를 사용합니다. 컨텍스트 모듈 및 손실 헤드와 같은 추가 모듈도 컨텍스트 모델링을 개선하고 멀티태스크 손실을 계산하는 데 사용되었습니다.

앵커 설정의 경우, 연구원들은 다양한 피처 피라미드 레벨에 걸쳐 규모별 앵커를 사용했으며, 다양한 크기의 얼굴에 대한 감지를 최적화하도록 설계된 설정을 사용했습니다. 훈련 중에 앵커는 IoU(Intersection-over-Union) 방법을 사용하여 기준값 상자에 매칭되었으며, 양성 훈련 예제와 음성 훈련 예제 간의 심각한 불균형을 처리하기 위해 온라인 하드 예제 마이닝(OHEM) 전략이 사용되었습니다.

![WIDER FACE 교육 및 검증 세트에서 주석을 추가할 수 있는 얼굴(우리는 이를 "주석 가능"이라고 함)에 다섯 개의 얼굴 랜드마크의 추가 주석을 추가합니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%203.png)

WIDER FACE 교육 및 검증 세트에서 주석을 추가할 수 있는 얼굴(우리는 이를 "주석 가능"이라고 함)에 다섯 개의 얼굴 랜드마크의 추가 주석을 추가합니다.

무작위 자르기, 수평 뒤집기, 광도계 색상 왜곡 등 데이터 증강 기법을 사용하여 모델의 견고성을 더욱 강화했습니다. 훈련은 모멘텀이 있는 확률적 경사 하강(SGD)을 사용하여 수행되었으며, 훈련 과정의 특정 지점에서 학습 속도를 조정했습니다.

테스트 과정에서 연구원들은 다중 스케일 전략과 박스 투표를 포함한 표준 관행을 따랐으며, 예측을 결합하여 최종 얼굴 감지 세트를 생성했습니다.

절제 연구에서 연구진은 제안된 RetinaFace 모델의 성능과 얼굴 랜드마크 및 고밀도 회귀 분기가 얼굴 감지 성능에 어떤 영향을 미치는지 이해하기 위해 광범위한 실험을 수행했습니다. 연구팀은 현재 최신 기술(FPN, 컨텍스트 모듈, 변형 가능한 컨볼루션)을 사용하여 91.286%의 정확도를 달성하는 기준 시스템을 설정했습니다. 얼굴 랜드마크 회귀를 추가했을 때 하드 하위 집합의 얼굴 상자 AP 및 mAP 점수가 향상되어 정확한 얼굴 감지를 위한 랜드마크 로컬라이제이션의 중요성이 강조되었습니다. 그러나 고밀도 회귀 분기를 추가하면 Easy 및 Medium 하위 집합의 결과는 개선되었지만 Hard 하위 집합의 점수는 약간 감소하여 어려운 시나리오에서 고밀도 회귀가 어렵다는 것을 보여주었습니다.

![RetinaFace는 제안된 공동의 추가 감독 및 자기 감독 멀티태스크 학습의 이점을 활용하여 보고된 1151명의 사람들 중 약 900명의 얼굴(임계값 0.5에서)을 찾을 수 있습니다. 탐지기의 신뢰도는 오른쪽의 컬러 바에 의해 제공됩니다. 밀집 위치 지정 마스크는 파란색으로 그려집니다. 작은 얼굴들에 대한 상세한 탐지, 정렬 및 밀집 회귀 결과를 확인하기 위해 확대하십시오.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%204.png)

RetinaFace는 제안된 공동의 추가 감독 및 자기 감독 멀티태스크 학습의 이점을 활용하여 보고된 1151명의 사람들 중 약 900명의 얼굴(임계값 0.5에서)을 찾을 수 있습니다. 탐지기의 신뢰도는 오른쪽의 컬러 바에 의해 제공됩니다. 밀집 위치 지정 마스크는 파란색으로 그려집니다. 작은 얼굴들에 대한 상세한 탐지, 정렬 및 밀집 회귀 결과를 확인하기 위해 확대하십시오.

얼굴 상자 정확도 측면에서 RetinaFace는 24개의 최신 얼굴 감지 알고리즘과 비교한 결과, AP 측면에서 이들 알고리즘을 능가하는 성능을 보였습니다. RetinaFace는 검증 및 테스트 세트의 모든 하위 집합에서 최고의 AP를 달성했으며, 특히 작은 얼굴이 많이 포함된 하드 하위 집합에서 신기록을 세웠습니다.

얼굴 랜드마크 로컬라이제이션의 정확도를 평가하기 위해 AFLW 데이터 세트와 WIDER FACE 검증 세트에서 RetinaFace를 MTCNN과 비교했습니다. 레티나페이스는 정규화된 평균 오류를 2.72%에서 2.21%로 크게 줄였으며, 이는 MTCNN과 비교했을 때도 마찬가지였습니다. 또한 RetinaFace는 실패율을 26.31%에서 9.37%로 크게 줄였습니다(NME 임계값 10%).

이러한 실험은 까다로운 시나리오에서도 높은 얼굴 감지 및 랜드마크 로컬라이제이션 정확도를 달성하는 데 있어 RetinaFace 접근 방식의 효과를 입증합니다.

상자 및 5개의 얼굴 랜드마크 외에도 RetinaFace는 자체 지도 학습을 통해 조밀한 얼굴 대응을 출력합니다. 고밀도 얼굴 랜드마크 로컬라이제이션의 정확도는 AFLW2000-3D 데이터 세트에서 평가되었습니다. 지도 방식과 자가 지도 방식 간에는 성능 차이가 존재했지만 RetinaFace의 고밀도 회귀 결과는 최첨단 방식과 비슷했습니다. 그 결과 5개의 얼굴 랜드마크 회귀가 고밀도 회귀 분기의 훈련 난이도를 완화하고 고밀도 회귀 결과를 크게 개선할 수 있음을 보여주었습니다. 그러나 단일 단계 특징을 사용하여 고밀도 대응 매개 변수를 예측하는 것은 RoI 특징을 사용하는 것보다 훨씬 어렵고, 이는 단일 단계 프레임워크로 높은 정확도의 고밀도 회귀 결과를 얻는 것이 어렵다는 것을 나타냅니다.

![AFLW2000-3D에서 CED 곡선. (a) 2D 좌표와 함께 68개의 랜드마크에 대해 평가를 수행하고 (b) 3D 좌표를 가진 모든 랜드마크에 대해 평가합니다. (c)에서는 RetinaFace와 Mesh Decoder [70]의 밀집 회귀 결과를 비교합니다. RetinaFace는 자세 변화가 있는 얼굴을 쉽게 처리할 수 있지만 복잡한 시나리오에서 정확한 밀집 대응을 예측하는 데 어려움이 있습니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%205.png)

AFLW2000-3D에서 CED 곡선. (a) 2D 좌표와 함께 68개의 랜드마크에 대해 평가를 수행하고 (b) 3D 좌표를 가진 모든 랜드마크에 대해 평가합니다. (c)에서는 RetinaFace와 Mesh Decoder [70]의 밀집 회귀 결과를 비교합니다. RetinaFace는 자세 변화가 있는 얼굴을 쉽게 처리할 수 있지만 복잡한 시나리오에서 정확한 밀집 대응을 예측하는 데 어려움이 있습니다.

다음으로, 얼굴 감지가 심층 얼굴 인식에 미치는 영향을 조사하기 위해 ArcFace 시스템에서 감지 및 정렬을 위해 MTCNN을 대체하는 RetinaFace를 사용했습니다. 그 결과 CFP-FP 데이터 세트에서 ArcFace의 검증 정확도가 98.37%에서 99.49%로 증가했으며, 이는 얼굴 감지와 정렬이 얼굴 인식 성능에 상당한 영향을 미친다는 것을 나타냅니다. 또한, 얼굴 인식 애플리케이션에서 RetinaFace는 MTCNN보다 훨씬 더 강력한 기준이 되는 것으로 입증되었습니다.

![IJB-C 데이터셋의 1:1 검증 프로토콜의 ROC 곡선. "+F"는 피처 임베딩 동안의 플립 테스트를 참조하고 "+S"는 템플릿 내의 샘플을 가중하는 데 사용되는 얼굴 감지 점수를 나타냅니다. 각 범례의 끝에는 TAR를 FAR= 1e − 6으로 제공합니다.](RetinaFace%20Single-stage%20Dense%20Face%20Localisation%20in%204594ac798bef4a638526042c01b7f4b7/Untitled%206.png)

IJB-C 데이터셋의 1:1 검증 프로토콜의 ROC 곡선. "+F"는 피처 임베딩 동안의 플립 테스트를 참조하고 "+S"는 템플릿 내의 샘플을 가중하는 데 사용되는 얼굴 감지 점수를 나타냅니다. 각 범례의 끝에는 TAR를 FAR= 1e − 6으로 제공합니다.

마지막으로 RetinaFace의 추론 효율성을 평가했습니다. RetinaFace는 단일 단계로 얼굴 로컬라이제이션을 수행하므로 유연하고 효율적입니다. 테스트에는 무거운 모델(ResNet-152)과 가벼운 모델(MobileNet-0.25)이 모두 사용되었으며, 후자가 상당한 실시간 속도를 보여주었습니다. 경량 모델인 RetinaFace-MobileNet-0.25는 인상적인 실시간 속도를 보여줬으며, 모바일 디바이스에서 빠른 시스템을 구현할 수 있도록 ARM에서 VGA 이미지에 대해 16FPS를 달성했습니다.

### 5. Conclusions

결론적으로, 이 연구는 이미지에서 다양한 스케일의 얼굴에 대한 고밀도 로컬라이제이션과 정렬을 동시에 수행해야 하는 까다로운 과제를 해결했습니다. 이 연구에서는 가장 까다로운 얼굴 인식 벤치마크에서 기존 최첨단 방법의 성능을 능가하는 최초의 1단계 솔루션인 RetinaFace를 소개했습니다. 특히 RetinaFace를 최첨단 얼굴 인식 방식과 통합하면 정확도가 크게 향상됩니다. 이 주제에 대한 추가 연구를 촉진하기 위해 이 연구의 데이터와 모델은 공개적으로 사용 가능합니다.

- 모델의 입력과 출력
    
    RetinaFace 모델은 이미지를 입력으로 받습니다. 이 이미지는 얼굴이 포함되어 있을 수 있으며, 얼굴의 위치, 크기, 각도 등에 제한이 없습니다. 모델은 이 이미지에서 얼굴의 위치를 찾고, 얼굴의 특징을 감지하며, 이미지 플레인에 투영된 3D 얼굴의 상세한 표현을 생성하는 것이 목표입니다.
    
    이 모델의 출력은 다음과 같은 여러 요소를 포함합니다:
    
    1. Face Score: 각각의 얼굴에 대한 점수로, 얼굴이 존재할 가능성을 나타냅니다.
    2. Face Box: 얼굴의 위치와 크기를 나타내는 경계 상자(bounding box)입니다.
    3. Facial Landmarks: 얼굴의 중요한 특징점을 나타내는 랜드마크(눈, 코, 입 등)입니다.
    4. Dense 3D Face Vertices: 이미지 플레인에 투영된 3D 얼굴의 상세한 표현으로, 얼굴의 모양과 구조를 더 잘 이해하게 도와줍니다.
    
    이들 출력은 이미지 내의 모든 얼굴에 대해 생성되며, 이는 이미지 내에서 여러 얼굴을 동시에 감지하고 분석할 수 있음을 의미합니다. 이러한 정보들은 얼굴 인식, 감정 분석, 나이 예측 등의 후속 작업을 위한 기초로 사용될 수 있습니다.
    
- 의의
    
    RetinaFace는 여러 면에서 이전의 얼굴 감지 방법들과 구별됩니다:
    
    1. **단일 단계 솔루션**: RetinaFace는 얼굴 탐지, 얼굴 특징점(localization) 및 밀도 높은 3D 얼굴 표현 학습을 동시에 수행하는 단일 단계 솔루션입니다. 이는 효율성을 크게 향상시키며, 얼굴 탐지 및 분석의 정확도를 높입니다.
    2. **감독 및 자기 감독 학습의 조합**: RetinaFace는 추가적으로 감독된 학습(supervised learning)과 자기 감독 학습(self-supervised learning)을 결합합니다. 이는 모델이 얼굴 감지 작업에 필요한 다양한 종류의 정보를 동시에 학습하도록 합니다.
    3. **3D 정보 사용**: RetinaFace는 이미지 평면에 투영된 밀도 높은 3D 얼굴의 상세한 표현을 생성합니다. 이는 모델이 얼굴의 3D 구조를 이해하고 이를 이용해 얼굴을 더 정확하게 감지하고 분석하는 능력을 강화합니다.
    4. **정확성과 효율성**: RetinaFace는 최첨단 성능을 달성하면서도, 그 과정에서 생기는 계산 부하를 줄이는 데 초점을 맞추었습니다. 이를 위해, RetinaFace는 무거운 모델(ResNet-152)과 가벼운 모델(MobileNet-0.25) 두 가지를 제공하며, 각각의 모델은 정확도와 효율성 사이의 다른 균형을 제공합니다.
    
    이러한 특징들 덕분에, RetinaFace는 이전의 얼굴 감지 방법들보다 더 뛰어난 성능을 제공하며, 더 다양한 상황에서 얼굴을 감지하고 분석하는 능력을 가집니다.