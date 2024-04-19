# ProHMR - Probabilistic Modeling for Human Mesh Recovery

[https://arxiv.org/abs/2108.11944](https://arxiv.org/abs/2108.11944)

[https://github.com/nkolot/ProHMR](https://github.com/nkolot/ProHMR)

### 1. Introduction

이 연구는 2D 관찰에서 3D 인체 포즈를 재현하는 문제에 초점을 맞춥니다. 이 문제는 오랜 문제이지만, 대부분의 최신 방법은 결정론적인 방식으로 단일 3D 추정치만 제공합니다. 이 논문에서는 단일 출력이 아닌 입력 데이터를 기반으로 한 3D 포즈 분포를 고려하는 것이 중요하다고 주장합니다.

![3D 인간 메시 복구를 위한 확률론적 모델링. 우리는 3D 인간 복구 문제를 입력으로부터 3D 포즈의 분포로의 매핑을 학습하는 문제로 재구성하는 것을 제안합니다. 출력 분포는 2D 증거와 일치하는 다양한 포즈에 높은 확률 질량을 가집니다.](ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60/Untitled.png)

3D 인간 메시 복구를 위한 확률론적 모델링. 우리는 3D 인간 복구 문제를 입력으로부터 3D 포즈의 분포로의 매핑을 학습하는 문제로 재구성하는 것을 제안합니다. 출력 분포는 2D 증거와 일치하는 다양한 포즈에 높은 확률 질량을 가집니다.

현재 기법들은 다양한 애플리케이션에서 단순성과 유용성 때문에 결정론적 시스템에 의존하는 경우가 많습니다. 일부 기존 기법은 입력당 여러 예측을 제공하지만, 복잡한 앙상블형 예측 모델에 의존하는 경우가 많아 확장성 문제와 출력 표현력의 제한을 초래합니다.

이러한 문제를 해결하기 위해 저자들은 제공된 2D 입력을 기반으로 3D 포즈 분포를 예측하는 새로운 방법을 제안합니다. 이 모델은 정규화 흐름을 사용하여 그럴듯한 포즈 분포를 회귀시켜 네트워크가 입력(예: 이미지 또는 2D 키포인트)을 기반으로 3D 포즈의 조건부 분포를 반환할 수 있도록 합니다. 이 모델은 다양한 출력을 샘플링하고, 각 샘플의 가능성을 효율적으로 계산하고, 분포의 모드를 계산하는 빠른 방법을 제공합니다.

![3D 인간 메시 추정을 위한 확률론적 모델링의 가치. 우리는 3D 인간 메시 추정의 경우에 확률론적 모델링이 그 우아하고 유연한 형태 때문에 특히 유용할 수 있다는 것을 보여줍니다. 이는 다양한 하위 어플리케이션을 가능하게 합니다. 첫 번째 행: 일반적인 3D 메시 회귀의 경우, 우리는 분포의 모드를 자연스럽게 사용하고 단일 3D 메시를 회귀하는 접근법과 비슷한 성능을 낼 수 있습니다. 두 번째 행: 키포인트(또는 다른 종류의 2D 증거)가 있을 때 우리는 우리의 모델을 이미지 기반의 사전 조건으로 취급하고 2D 재투영 항을 결합함으로써 키포인트에 인간 몸 모델을 맞춤화할 수 있습니다. 세 번째 행: 여러 뷰가 있을 때, 우리는 모든 단일 프레임 예측을 교차 뷰 일관성 항을 추가함으로써 자연스럽게 합칠 수 있습니다. 우리는 이 모든 응용 프로그램들이 테스트 시간 동작을 참조하고 같은 학습된 확률 모델을 사용한다는 것을 강조합니다(태스크별 훈련은 필요하지 않습니다).](ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60/Untitled%201.png)

3D 인간 메시 추정을 위한 확률론적 모델링의 가치. 우리는 3D 인간 메시 추정의 경우에 확률론적 모델링이 그 우아하고 유연한 형태 때문에 특히 유용할 수 있다는 것을 보여줍니다. 이는 다양한 하위 어플리케이션을 가능하게 합니다. 첫 번째 행: 일반적인 3D 메시 회귀의 경우, 우리는 분포의 모드를 자연스럽게 사용하고 단일 3D 메시를 회귀하는 접근법과 비슷한 성능을 낼 수 있습니다. 두 번째 행: 키포인트(또는 다른 종류의 2D 증거)가 있을 때 우리는 우리의 모델을 이미지 기반의 사전 조건으로 취급하고 2D 재투영 항을 결합함으로써 키포인트에 인간 몸 모델을 맞춤화할 수 있습니다. 세 번째 행: 여러 뷰가 있을 때, 우리는 모든 단일 프레임 예측을 교차 뷰 일관성 항을 추가함으로써 자연스럽게 합칠 수 있습니다. 우리는 이 모든 응용 프로그램들이 테스트 시간 동작을 참조하고 같은 학습된 확률 모델을 사용한다는 것을 강조합니다(태스크별 훈련은 필요하지 않습니다).

이 방법의 가장 큰 장점은 주어진 입력에 대해 가장 가능성이 높은 3D 포즈를 반환할 수 있기 때문에 단일 추정이 필요한 애플리케이션에서 다용도로 사용할 수 있다는 점입니다. 인체 모델을 2D 위치에 맞추는 최적화 접근 방식에서 강력한 이미지 기반 선행으로 사용할 수 있습니다. 여러 뷰를 사용할 수 있는 경우 모든 조건부 분포의 정보를 통합하여 뷰 간 일관성을 최적화하고 사용 가능한 관찰과 일치하는 3D 결과를 제공할 수 있습니다.

저자는 확률론적 모델의 가치를 보여주기 위해 광범위한 실험을 수행합니다. 다양한 작업과 평가 설정에서 강력한 성능을 발휘하는 휴먼 메시 복구를 위한 확률론적 모델인 ProHMR을 소개합니다. 그 결과, 이 모델은 여러 소스의 정보를 효과적으로 통합하고 강력한 이미지 기반 선행으로 작동하여 이전 기준선보다 크게 개선할 수 있음을 보여줍니다.

### 2. Related work

본 논문에서 수행한 연구는 다양한 선행 연구와 연관되어 있으며, 다양한 입/출력에 적용 가능하지만 단일 이미지에서 휴먼 메쉬 복원에 중점을 두고 있습니다. 관련 연구는 크게 세 가지로 구분됩니다:

단일 이미지에서 휴먼 메시 복구:

회귀 접근법: 딥 네트워크가 단일 이미지에서 모델의 파라미터를 회귀하는 방식입니다. 대표적인 예로 많은 후속 연구에 영향을 준 HMR이 있습니다. 저자의 접근 방식은 이와 유사하지만 단일 포즈가 아닌 3D 포즈 분포를 회귀합니다.
2D 단서에 따라 모델 파라미터를 반복적으로 추정하는 최적화 방법. 2D 키포인트를 사용하여 SMPL 파라미터를 최적화하는 SMPLify가 대표적인 예입니다. 저자들은 이 모델이 이미지 기반 정보를 사용하여 키포인트 기반 최적화를 안내할 수 있음을 보여줍니다.
최적화-회귀 하이브리드는 두 가지 패러다임을 결합한 것입니다. 예를 들면 HMR, HUND, SPIN 등이 있습니다. 저자의 확률적 모델도 피팅의 사전 조건으로 사용할 수 있는 포즈 분포를 회귀하는 방식으로 유사한 접근 방식을 취합니다.
3D 인체 포즈에 대한 여러 가설: 이러한 방법은 3D 인체 포즈 재구성의 모호함을 해결하는 데 사용되었습니다. 기술에는 구성 모델, 혼합 밀도 네트워크 또는 조건부 VAE를 사용하여 여러 가설을 생성하는 방법이 포함됩니다. 저자의 접근 방식은 이산적인 가설 집합이 아니라 포즈에 대한 전체 확률을 제공한다는 점에서 차별화됩니다.

흐름 정규화: 단순한 기본 분포에서 일련의 역변환을 통해 복잡한 분포를 표현하는 데 사용되었습니다. 3D 인체 포즈 추정의 맥락에서 사용되어 그럴듯한 포즈 분포에 대한 사전 학습을 위해 사용되었습니다. 저자들의 연구는 3D 포즈 공간에 대한 일반적인 포즈 사전이 아니라 2D 이미지 증거에 조건부 포즈 사전을 학습하는 데 중점을 둠으로써 차별화됩니다.

### 3. Method

이 섹션에서는 ProHMR(확률론적 휴먼 메시 복구)이라는 이름의 제안된 접근법의 방법론에 대해 설명합니다. 이 접근 방식은 주어진 입력 이미지에서 그럴듯한 포즈 분포를 학습하는 것을 궁극적인 목표로 하여 플로우 정규화 및 SMPL 바디 모델을 활용합니다.

![인간 메시 복구를 위한 제안된 확률론적 모델, ProHMR의 아키텍처. 왼쪽: 우리의 이미지 인코더는 숨겨진 벡터 c를 회귀하며, 이는 플로우 모델에게 조건화 입력으로 사용됩니다. 동시에, 이것은 형상 매개 변수 β와 카메라 π로 디코딩됩니다. 오른쪽: 우리의 플로우 모델은 두 가지 처리 방향을 허용하는 가역적인 매핑을 학습합니다; 원하는 기능에 따라 샘플링 및 빠른 우도 계산을 모두 수행할 수 있습니다.](ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60/Untitled%202.png)

인간 메시 복구를 위한 제안된 확률론적 모델, ProHMR의 아키텍처. 왼쪽: 우리의 이미지 인코더는 숨겨진 벡터 c를 회귀하며, 이는 플로우 모델에게 조건화 입력으로 사용됩니다. 동시에, 이것은 형상 매개 변수 β와 카메라 π로 디코딩됩니다. 오른쪽: 우리의 플로우 모델은 두 가지 처리 방향을 허용하는 가역적인 매핑을 학습합니다; 원하는 기능에 따라 샘플링 및 빠른 우도 계산을 모두 수행할 수 있습니다.

3.1. 플로우 정규화: 이 방법은 복잡한 분포를 단순한 기본 분포의 일련의 역변환으로 모델링합니다. 일반적으로 표준 다변량 가우스 분포가 기본 분포로 선택됩니다. 그런 다음 변환된 변수의 로그 확률 밀도를 계산할 수 있습니다. 조건부 분포는 흐름 정규화를 사용하여 모델링할 수도 있습니다.

3.2. SMPL 모델: SMPL(Skinned Multi-Person Linear model)은 포즈 파라미터와 모양 파라미터를 사용하여 바디 메시를 출력하는 인체 모델입니다. 그런 다음 신체 관절을 메시 정점의 선형 조합으로 표현할 수 있습니다.

3.3. 모델 디자인: ProHMR 모델은 사람의 이미지를 입력으로 사용하고 SMPL 신체 모델 파라미터 세트를 출력합니다. 이 모델은 입력 이미지에 따라 사람의 그럴듯한 포즈 분포를 학습합니다. 네트워크의 출력은 포즈 매개변수의 조건부 확률 분포와 모양 및 카메라 매개변수에 대한 포인트 추정치라는 점에서 HMR 패러다임을 밀접하게 따릅니다.

이 아키텍처는 CNN을 사용하여 입력 이미지를 컨텍스트 벡터로 인코딩하고 조건부 정규화 흐름을 사용하여 포즈 파라미터의 조건부 분포를 모델링합니다. 이 네트워크는 표현력과 복잡한 분포를 모델링할 수 있는 능력, 주어진 출력 샘플의 가능성을 계산할 수 있는 능력 때문에 혼합 밀도 네트워크(MDN)나 VAE가 아닌 노멀라이징 플로우를 사용합니다.

사용된 정규화 흐름 모델은 Glow 아키텍처를 기반으로 하며, 빠른 확률 계산과 분포에서 빠른 샘플링을 모두 지원합니다. 추가 부수 정보가 없는 경우 출력 분포의 모드를 사용하여 예측이 이루어집니다.

마지막으로, 컨텍스트 벡터를 입력으로 받아 단일 포인트 추정치를 출력하는 작은 MLP를 사용하여 카메라와 SMPL 모양 파라미터를 회귀합니다.

저자들은 이미지 데이터로부터 3D 포즈 추정을 위한 새로운 훈련 목표를 제안합니다. 3D 포즈 주석은 일반적으로 몇 가지 제한된 데이터 세트 외에는 사용할 수 없기 때문에 이는 특히 중요합니다. 모델을 훈련하기 위해 학습된 분포 측면에서 기준 데이터 예제의 음의 로그 확률에 대한 기대치를 최소화합니다. 이 손실은 무의식적 통계학자의 법칙을 사용하여 기대치를 다시 작성하여 차별화할 수 있습니다.

저자들은 생성 모델과 예측 모델을 모두 목표로 합니다. 저자들은 각 이미지에 대해 출력 분포의 모드가 특정 변환에 해당한다는 속성을 활용합니다. 그런 다음 표준 회귀 프레임워크 내에서 사용 가능한 모든 주석을 사용하여 이 모드를 감독하여 성능을 향상시킵니다.

6D 표현으로 회전을 모델링할 때 발생하는 문제점은 고유성이 부족하다는 것입니다. 이를 완화하기 위해 6D 표현이 직교 6D 표현에 가까워지도록 강제하는 손실 함수를 도입합니다. 최종 훈련 목표는 정의된 모든 손실의 가중치 합계입니다.

저자는 훈련된 모델의 다운스트림 애플리케이션도 시연합니다. 여기에는 단일 이미지에서 3D 포즈 회귀, SMPL 신체 모델 피팅, 보정되지 않은 여러 뷰에서 포즈 추정 등이 포함됩니다.

마지막으로, ResNet-50을 인코더로 사용하고 모델의 흐름을 정규화하는 등 ProHMR 구현에 대한 자세한 내용을 제공합니다. 또한 2D 포즈를 3D 스켈레톤으로 리프팅하는 데 이 접근 방식을 어떻게 사용할 수 있는지 보여줍니다. 다운스트림 작업의 경우 포즈 공간에서 직접 최적화하는 대신 잠재 공간에서 최적화할 것을 제안합니다.

### 4. Experimental evaluation

실험 평가는 정량적 평가와 정성적 결과의 두 부분으로 나누어 진행되었습니다.

정량적 평가에는 Human3.6M, MPI-INF3DHP, 3DPW, 마네킹 챌린지 등 4개의 데이터 세트가 훈련과 평가에 사용되었습니다. 이 모델을 표준 회귀 방법과 비교한 결과, 휴먼 메시 복구 측면에서 비슷한 성능을 보였습니다. 여러 가설 기준선을 두고 표현력을 비교했을 때, 제안된 모델이 더 나은 성능을 보였습니다.

모델 피팅 정확도는 SMPL 신체 모델을 2D 키포인트 세트에 피팅하는 다양한 방법을 비교하여 평가했습니다. 저자가 제안한 학습된 이미지 컨디셔닝을 사용한 피팅은 SMPLify 및 EFT와 같은 기존의 다른 방법보다 정확도가 눈에 띄게 향상되었습니다.

![학습된 분포에서의 샘플. 분홍색 메시는 모드에 해당합니다.](ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60/Untitled%203.png)

학습된 분포에서의 샘플. 분홍색 메시는 모드에 해당합니다.

또한 보정되지 않은 멀티뷰 시나리오에서 포즈 예측을 개선하는 데 있어 모델의 효과를 평가했습니다. 저자들의 방법은 개별 뷰별 예측 및 회전 평균을 수행하는 기준선과 비교했을 때 결과가 개선된 것으로 나타났습니다.

그림 4와 그림 5에 제시된 정성적 결과는 특히 키포인트 감지가 누락되거나 신뢰도가 매우 낮은 경우 이 모델이 보다 현실적인 재구성을 생성할 수 있음을 보여주었습니다.

![모델 맞춤화 결과. 분홍색: 회귀. 녹색: ProHMR + 맞춤화. 회색: 회귀 + SMPLify](ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60/Untitled%204.png)

모델 맞춤화 결과. 분홍색: 회귀. 녹색: ProHMR + 맞춤화. 회색: 회귀 + SMPLify

요약하자면, 저자들이 제안한 모델은 정량적 평가와 정성적 결과 모두에서 가능성을 보였으며, 현장에서의 추가 적용 및 개발 가능성을 보여주었습니다.

### 5. Summary

이 연구에서는 2D 증거로부터 3D 인체 메시를 복구하기 위한 확률론적 모델을 소개합니다. 3D 포즈에 대한 단일 점 추정치를 생성하는 기존 접근 방식과 달리, 이 연구에서는 이 분포 모델링에 조건부 정규화 흐름을 활용하여 입력에서 그럴듯한 포즈 분포에 대한 매핑을 학습할 것을 제안합니다.

그 결과 확률적 모델을 통해 다양한 출력의 샘플링이 가능하고, 각 샘플의 가능성을 효율적으로 계산할 수 있으며, 모드에 대한 빠르고 닫힌 형태의 솔루션을 얻을 수 있습니다. 이 방법의 효과는 여러 벤치마크에 대한 경험적 결과를 통해 확인되었습니다.

향후 이 접근 방식은 다른 범주의 관절형 또는 비관절형 객체로 확장될 수 있습니다. 또한 깊이-크기 트레이드오프와 같은 다른 불확실성을 모델링하는 것도 가능할 수 있습니다.