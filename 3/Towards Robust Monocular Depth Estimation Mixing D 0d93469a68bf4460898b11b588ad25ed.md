# Towards Robust Monocular Depth Estimation: Mixing Datasets for Zero-shot Cross-dataset Transfer

[https://arxiv.org/abs/1907.01341](https://arxiv.org/abs/1907.01341)

[https://pytorch.org/hub/intelisl_midas_v2/](https://pytorch.org/hub/intelisl_midas_v2/)

[https://github.com/isl-org/MiDaS](https://github.com/isl-org/MiDaS)

- 2 Jul 2019

### 1 INTRODUCTION

깊이는 물리적 환경에서의 동작을 표현하는 데 유용한 중간 표현입니다. 그러나 단일 카메라에서 깊이를 추정하는 것(단안 깊이 추정)은 어렵고 학습 기반 기술이 필요합니다. 효과적인 모델을 생성하려면 다양하고 대규모의 훈련 데이터가 필요합니다. 구조광, ToF, 레이저 스캐너, 스테레오 카메라 등 다양한 센서를 통해 수집된 현재의 데이터 세트는 편향되고 불완전합니다.

![우리는 다양하고 알 수 없는 깊이 범위 및 스케일에도 불구하고, 단일 뷰 깊이 추정을 위해 여러 보완적인 소스로부터의 훈련 데이터를 어떻게 활용하는지 보여줍니다. 우리의 접근 방식은 데이터셋 간의 강력한 일반화를 가능하게 합니다. 상단: 입력 이미지. 중간: 제시된 방법에 의해 예측된 역 깊이 맵. 하단: 새로운 시점에서 렌더링된 해당 포인트 클라우드. 포인트 클라우드는 Open3D [4]를 통해 렌더링됩니다. 입력 이미지는 훈련 중에 보지 못한 Microsoft COCO 데이터셋 [5]에서 가져옴.](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled.png)

우리는 다양하고 알 수 없는 깊이 범위 및 스케일에도 불구하고, 단일 뷰 깊이 추정을 위해 여러 보완적인 소스로부터의 훈련 데이터를 어떻게 활용하는지 보여줍니다. 우리의 접근 방식은 데이터셋 간의 강력한 일반화를 가능하게 합니다. 상단: 입력 이미지. 중간: 제시된 방법에 의해 예측된 역 깊이 맵. 하단: 새로운 시점에서 렌더링된 해당 포인트 클라우드. 포인트 클라우드는 Open3D [4]를 통해 렌더링됩니다. 입력 이미지는 훈련 중에 보지 못한 Microsoft COCO 데이터셋 [5]에서 가져옴.

이 논문은 다양한 환경에서 잘 작동하는 강력한 단안 깊이 추정 모델을 개발하는 데 중점을 둡니다. 연구진은 데이터 세트 간의 불일치에 영향을 받지 않는 새로운 손실 함수를 도입하여 다양한 감지 양식의 데이터로 학습할 수 있도록 합니다. 또한 기존 데이터 세트의 가치를 조사하고 훈련 중에 데이터 세트를 혼합하는 최적의 전략을 모색합니다. 이 연구는 대용량 인코더의 중요성과 대규모 보조 작업에 대한 인코더 사전 훈련의 중요성을 강조합니다.

실험을 통해 적절한 훈련 절차를 사용하여 다양한 소스의 다양한 이미지로 훈련된 모델이 다양한 환경에서 최첨단 결과를 얻을 수 있음을 보여줍니다. 성능은 제로 샷 교차 데이터 세트 전송을 사용하여 평가되는데, 일부 데이터 세트에서 모델을 학습시키고 보이지 않는 데이터 세트에서 테스트합니다. 이 방법은 실제 성능을 보다 정확하게 측정할 수 있습니다. 이 방법은 이전 방법보다 성능이 뛰어나며 단안 깊이 추정에 대한 새로운 표준을 제시합니다.

### 2 RELATED WORK

초기의 단안 심도 추정 접근 방식은 MRF 기반 공식, 간단한 기하학적 가정 또는 비모수적 방법과 같은 기술을 사용했습니다. 최근의 발전은 컨볼루션 네트워크를 사용하여 입력 이미지에서 장면 깊이를 직접 추정하는 데 중점을 두었습니다. 이러한 방법은 정확도가 향상되었지만, 여전히 실측 깊이 데이터에 의존하고 훈련 데이터의 다양성이 제한되어 있어 제약이 없는 장면으로 일반화하기에는 어려움이 있습니다.

![3D 영화 데이터셋에서 샘플 이미지. 훈련 세트에 있는 일부 영화에서의 이미지와 그들의 역 깊이 맵을 보여줍니다. 하늘 영역과 유효하지 않은 픽셀은 마스킹됩니다. 각 이미지는 다른 영화에서 가져옵니다. 3D 영화는 다양한 데이터의 대량 소스를 제공합니다.](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%201.png)

3D 영화 데이터셋에서 샘플 이미지. 훈련 세트에 있는 일부 영화에서의 이미지와 그들의 역 깊이 맵을 보여줍니다. 하늘 영역과 유효하지 않은 픽셀은 마스킹됩니다. 각 이미지는 다른 영화에서 가져옵니다. 3D 영화는 다양한 데이터의 대량 소스를 제공합니다.

보정된 스테레오 카메라를 사용한 자체 감독은 데이터 수집을 간소화했지만 여전히 한계에 직면해 있습니다. 자체 감독 방식은 종종 스테레오 이미지나 명백한 움직임이 필요하기 때문에 동적인 장면에 적용하기 어렵습니다.

딥 모노안 깊이 추정 모델의 주요 과제는 대규모의 다양한 실사 데이터가 부족하다는 점입니다. 기존 데이터 세트에는 균일한 장면 레이아웃과 적은 수의 동적 오브젝트만 포함되어 있어 제약이 적은 환경에서는 모델이 실패하는 경우가 많습니다.

연구자들은 크라우드 소싱, SfM, MVS 재구성과 같은 기술을 사용하여 보다 다양한 데이터 세트를 만들려고 시도해 왔습니다. 그러나 이러한 데이터 세트는 상당한 사전 및 사후 처리가 필요한 경우가 많으며 저작권 제한으로 인해 복제가 어려울 수 있습니다.

이러한 맥락에서 여러 데이터 소스의 제어된 혼합은 널리 연구되지 않았습니다. 이전 연구에서는 구조 및 움직임 추정을 위해 더 작은 데이터 세트를 결합하거나 단안 심도 학습을 위해 순진한 혼합 전략을 사용했지만, 최적의 혼합 전략이나 여러 데이터 세트 결합의 영향에 대해서는 조사하지 않았습니다.

### 3 EXISTING DATASETS

이 연구에서 연구진은 단일 이미지에서 깊이를 예측하는 단안 깊이 추정을 연구하고 있습니다. 연구진은 컬러 이미지와 그에 해당하는 깊이 정보를 제공하는 다양한 기존 데이터 세트를 사용했습니다. 이러한 데이터 세트는 각각 캡처된 환경과 물체의 유형, 깊이 주석 세부 정보, 데이터 정확도 방법, 이미지 품질, 카메라 설정, 데이터 세트의 크기 등 고유한 특징을 가지고 있습니다. 그러나 이러한 특성에는 고유한 편견과 문제도 있습니다.

일반적으로 정확도가 높은 데이터 세트는 확장하기가 어렵고 동적 오브젝트에는 문제가 있습니다. 반면에 인터넷에서 가져온 대규모 데이터 세트는 이미지 품질이 낮고, 깊이 데이터가 정확하지 않으며, 카메라 매개변수를 알 수 없는 경우가 많습니다.

연구자들이 단일 데이터 세트에 대해 알고리즘을 학습시킬 때, 유사한 데이터에서는 잘 작동하지만 새로운 다른 데이터에는 잘 일반화되지 않는 경향이 있습니다. 따라서 연구진은 여러 데이터 세트를 혼합하여 훈련하는 방법을 제안했습니다. 이 방법은 훈련 데이터에 포함되지 않은 다양한 데이터 세트에서 테스트했을 때 더 나은 일반화 성능을 보였습니다.

연구진은 훈련을 위해 다섯 가지 데이터 세트를 사용했습니다: ReDWeb, MegaDepth, WSVD, DIML Indoor입니다. 이러한 데이터 세트는 각각 고유한 속성과 특성을 가지고 있습니다.

학습된 모델을 테스트하기 위해 6개의 다른 데이터세트를 사용했습니다: DIW, ETH3D, Sintel, KITTI, NYU 및 TUM 데이터 세트입니다. 이들은 실측 데이터의 다양성과 정확성을 고려하여 이러한 데이터 세트를 선택했습니다.

이들은 이러한 데이터세트에서 모델을 미세 조정하지 않고 있는 그대로 테스트하는 방법을 제로샷 교차 데이터세트 전송이라고 합니다. 이를 통해 다양한 데이터 소스에서 모델의 성능을 측정할 수 있습니다.

### 4 3D MOVIES

연구진은 단안 깊이 추정 작업을 개선하기 위한 새로운 데이터 소스를 제안했습니다: 바로 3D 영화입니다. 이러한 영화는 다양한 동적 환경에서 고품질 비디오 프레임을 제공합니다. 절대 깊이 데이터는 제공하지 않지만 스테레오 이미지에서 상대 깊이를 유추할 수 있습니다. 3D 영화를 사용하는 주된 동기는 통제된 조건에서 캡처한 수백만 개의 고품질 이미지를 제공하는 규모와 다양성입니다.

하지만 3D 영화를 사용하는 데는 어려움이 따릅니다. 각 장면의 디스턴스 범위 또는 뎁스 예산은 예술적 및 시청자 편의성 고려 사항으로 인해 제한됩니다. 따라서 초점 거리, 기준선 및 수렴 각도와 같은 카메라 특성은 단일 영화 내에서 달라질 수 있습니다. 또한 영화 프레임에는 양수 및 음수 차이가 있어 물체가 화면의 앞이나 뒤에 나타날 수 있습니다.

이러한 문제를 해결하기 위해 연구진은 실제 스테레오 카메라로 촬영하고 고해상도 블루레이 형식으로 제공되는 23개의 다양한 영화를 선택했습니다. 연구진은 스테레오 이미지 쌍을 추출하여 적절한 포맷으로 처리했습니다. 장면 감지 도구를 사용하여 개별 클립을 추출하고 다양성을 보장하기 위해 프레임을 샘플링했습니다.

그리고 광학 흐름 알고리즘을 사용하여 추출된 이미지 쌍으로부터 디스패리티 맵을 추정했습니다. 광학 흐름 방법은 포지티브 및 네거티브 디퍼런스를 모두 처리하고 중간 정도의 변위에 대해 우수한 성능을 발휘하기 때문에 선택되었습니다. 또한 좌우 일관성 검사를 수행하여 디스패리티 품질이 좋지 않은 프레임을 걸러냈습니다.

마지막으로 연구진은 깊이 추정 모델을 훈련하기 위해 19개 영화의 프레임을 사용했으며, 검증과 테스트를 위해 각각 두 개의 영화를 확보했습니다. 각 영화에서 추출한 프레임 수는 영화 런타임과 디스패리티 품질에 따라 달라졌습니다.

### 5 TRAINING ON DIVERSE DATA

이 글에서는 다양한 데이터 세트를 사용하여 단안 수심 추정을 위한 훈련 모델과 그에 따른 고유한 과제에 대해 설명합니다. 절대 깊이, 알 수 없는 축척까지의 깊이, 격차 맵 등 다양한 데이터 세트에서 깊이 정보를 표현할 수 있는 다양한 방법을 고려할 때, 이러한 변화를 처리할 수 있는 일관된 계산 방법이 필요합니다.

세 가지 중요한 과제가 강조됩니다:

- 본질적으로 다른 깊이 표현(예: 직접 깊이 표현과 역 깊이 표현)
- 일부 데이터 집합의 깊이가 알 수 없는 스케일로 제공되는 스케일 모호성.
- 시프트 모호성: 일부 데이터 세트에서 알 수 없는 스케일과 글로벌 디스턴스 시프트까지 디스턴스를 제공하는 경우.

이러한 문제를 극복하기 위해 이 글에서는 앞서 언급한 모호성을 처리할 수 있도록 디스패리티 공간에서 작동하는 스케일 및 시프트 불변 손실 함수를 제안합니다. 이 손실 함수는 최적의 정렬을 위해 값을 스케일링하고 시프트한 후 예측된 불일치와 실측 불일치 간의 차이를 최소화하도록 설계되었습니다.

이 문서에서는 이러한 정렬을 달성하기 위한 두 가지 전략, 즉 최소제곱 기준과 강력한 스케일 및 이동 추정치를 기반으로 하는 전략에 대해 설명합니다. 이러한 전략은 실측 데이터의 이상값과 불일치를 처리하는 데 특히 유용합니다.

이 텍스트에서는 또한 단안 깊이 추정 모델을 훈련할 때 알려지지 않았거나 다양한 스케일을 설명하기 위해 시도하는 다양한 기존 손실 함수에 대해 설명합니다. 예를 들어, Eigen 등이 제안한 로그 깊이 공간에서의 스케일 불변 손실, Chen 등이 제안한 서수 손실, Wang 등이 제안한 정규화된 다중 스케일 기울기(NMG) 손실 등이 있습니다. 이러한 각 방법에는 고유한 장단점이 있으며, 손실 함수의 선택은 애플리케이션의 특정 요구 사항에 따라 달라집니다.

훈련에 사용되는 최종 손실 함수에는 예측된 격차 맵에서 깊이가 다른 영역 간의 경계가 실측 데이터의 경계와 일치하도록 하기 위한 기울기 매칭 용어가 포함됩니다. 훈련 중에 데이터 세트를 통합하는 데는 두 가지 전략이 제안되는데, 하나는 각 데이터 세트를 동등하게 취급하는 것이고 다른 하나는 데이터 세트에 대해 근사 파레토 최적을 찾는 것입니다.

이 접근 방식은 특히 단안 심도 추정 작업에서 다양한 데이터 세트에 대한 딥러닝 모델 훈련의 과제와 잠재적 해결책을 강조하지만, 이러한 원칙 중 상당수는 훈련 데이터가 다양한 형식 또는 다양한 소스에서 제공될 수 있는 다른 머신러닝 영역에도 적용될 수 있습니다.

### 6 Expreriments

연구원들은 단일 이미지 깊이 예측을 위해 ResNet 기반 멀티스케일 아키텍처를 사용한 Xian 등의 실험 설정을 사용했습니다. 연구진은 이미지넷의 사전 학습된 가중치로 인코더를 초기화하고 사전 학습된 레이어와 무작위로 초기화된 레이어에 대해 서로 다른 학습 속도를 가진 아담 옵티마이저를 사용했습니다. 데이터를 보강하기 위해 이미지를 뒤집고, 자르고, 크기를 조정했습니다.

![Pareto-optimal 믹싱을 사용해 다양한 데이터셋 조합으로 훈련된 모델의 비교. Microsoft COCO [5]에서의 이미지.](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%202.png)

Pareto-optimal 믹싱을 사용해 다양한 데이터셋 조합으로 훈련된 모델의 비교. Microsoft COCO [5]에서의 이미지.

또한 ImageNet 사전 학습이 성능에 영향을 미친다고 판단하여 손실 함수 및 인코더 아키텍처에 대한 제거 연구를 수행했습니다. 가장 성능이 좋은 사전 학습 모델을 데이터 세트 혼합 실험의 시작점으로 사용했습니다. 배치 크기는 8L(예: 3개 데이터 세트의 경우 24개)였습니다. 데이터 세트를 비교할 때 에포크를 72,000개의 이미지를 처리하는 것으로 정의하고 60개의 에포크에 대해 학습했습니다. 모든 데이터 세트에 대해 실측값 불일치를 [0, 1] 범위로 조정했습니다.

연구원들은 제거 연구와 데이터 세트 혼합 실험에 다양한 테스트 데이터 세트와 메트릭을 사용했습니다. 제거 연구에는 RW, MD, MV의 홀드아웃 검증 세트를 사용했으며, 데이터 세트 혼합 실험과 최신 기술과의 비교를 위해 DIW, ETH3D, Sintel, KITTI, NYU, TUM과 같은 보이지 않는 데이터 세트에 대해 테스트했습니다.

각 데이터세트에는 실측 데이터에 맞는 특정 메트릭이 있었습니다. DIW의 경우, 가중 인간 불일치율(WHDR)을 사용했습니다. 상대적 깊이 데이터세트(MV, RW, MD)의 경우, 격차 공간에서 평균제곱근오차를 측정했습니다. 정확한 절대 깊이를 가진 데이터 세트(ETH3D, Sintel)의 경우, 깊이 공간에서 상대 오차의 평균 절대값(AbsRel)을 측정했습니다. 마지막으로 특정 비율을 가진 픽셀의 비율을 사용하여 KITTI, NYU, TUM의 모델을 평가했습니다.

연구진은 심도 공간에서 평가된 데이터 세트에 대해 적절한 최대값으로 예측을 제한했습니다. 또한 오차를 측정하기 전에 각 이미지의 축척과 시프트에서 예측과 실측 데이터를 정렬했습니다. 여러 데이터 세트에서 평가할 때 결과를 더 쉽게 해석할 수 있도록 기준 방법과 비교하여 성능의 상대적인 변화를 제시했습니다.

입력 해상도를 평가하기 위해 연구원들은 테스트 이미지의 크기를 조정하여 큰 축은 384픽셀로, 작은 축은 32픽셀의 배수로 조정하여 화면비를 원본에 최대한 가깝게 유지했습니다. KITTI 데이터 세트의 경우 가로 세로 비율이 넓기 때문에 작은 축의 크기를 384픽셀과 동일하게 조정했습니다.

연구진이 비교한 최신 방법은 특정 데이터 세트에 특화되어 있으며 다양한 이미지 크기와 종횡비를 처리하는 방법을 지정하지 않았습니다. 연구진은 평가 스크립트와 훈련 차원에 따라 모든 방법에서 가장 성능이 좋은 설정을 찾으려고 노력했습니다.

정사각형 패치로 훈련된 접근법의 경우, 큰 축을 훈련 이미지 축 길이로 설정하고 작은 축을 조정했습니다. 정사각형 패치가 아닌 접근법의 경우, 작은 축을 더 작은 훈련 이미지 축 차원으로 고정했습니다. DORN, 모노뎁스2, Struct2Depth와 같은 다른 방법의 경우 특정 크기 조정 및 패딩 프로토콜을 따랐습니다.

마지막으로 모든 예측은 평가를 위해 실측 자료의 해상도에 맞게 재조정되었습니다.

연구진은 검증 성능에 대한 다양한 손실 함수를 비교한 결과, 제안한 트림된 MAE 손실이 모든 데이터 세트에서 가장 낮은 검증 오류를 생성한다는 사실을 발견했습니다. 따라서 연구진은 Lssitrim + Lreg를 사용하여 다음 실험을 모두 수행했습니다.

또한 인코더 아키텍처의 영향도 평가했습니다. 연구팀은 원래 Xian 등이 사용했던 ResNet-50 인코더를 기준선으로 삼아 ResNet-101, ResNeXt-101, DenseNet-161 인코더와 비교했습니다. 모든 인코더는 ImageNet에서 사전 학습되었습니다. 그 결과 용량이 큰 엔코더가 더 나은 성능을 보였으며, 약하게 감독된 데이터로 사전 학습된 ResNeXt-101 엔코더는 ImageNet에서만 학습된 동일한 엔코더보다 훨씬 더 나은 성능을 보였습니다. 사전 학습을 하지 않은 ResNet-50 인코더는 평균 35% 더 나쁜 성능을 보였기 때문에 사전 학습은 매우 중요했습니다. 인코더의 ImageNet 성능은 단안 깊이 추정 성능에 대한 강력한 예측 인자였으며, 이는 이미지 분류의 개선이 강력한 단안 깊이 추정에 직접적인 도움이 될 수 있음을 시사합니다. 연구팀은 기준선 대비 최대 15%의 상대적 개선을 관찰했으며, 이후 모든 실험에 ResNeXt-101-WSL을 사용했습니다.

![Microsoft COCO 데이터셋 [5]의 이미지에서 우리의 접근법과 네 가지 최고 경쟁자들의 질적 비교.](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%203.png)

Microsoft COCO 데이터셋 [5]의 이미지에서 우리의 접근법과 네 가지 최고 경쟁자들의 질적 비교.

연구진은 일반화를 위해 다양한 훈련 데이터 세트의 유용성을 평가했습니다. 그 결과, 보다 전문화된 데이터 세트는 유사한 테스트 세트에서 더 나은 성능을 보였지만 다른 데이터 세트에서는 성능이 저하되는 것을 발견했습니다. 흥미롭게도 단일 데이터셋을 단독으로 사용하면 규모는 작지만 선별된 RW 데이터셋을 사용하는 것보다 일반화 성능이 더 나빴습니다.

비슷한 특성을 가진 데이터셋을 비교한 결과, MV와 WS가 RW보다 더 크지만 둘 다 개별 성능이 더 나쁘다는 것을 발견했습니다. 이는 이러한 데이터 세트의 비디오 특성으로 인한 중복 데이터와 RW의 더 엄격한 필터링 때문일 수 있습니다.

혼합 실험에서는 여러 훈련 세트를 혼합하면 기준선에 비해 일관되게 성능이 향상되는 것을 발견했습니다. 하지만 데이터 세트를 추가한다고 해서 항상 성능이 향상되는 것은 아니었습니다. 파레토 최적 데이터 세트 혼합은 나이브 혼합 전략보다 성능이 향상되었으며, 추가 데이터 세트를 더 일관되게 활용할 수 있었습니다. 5개의 데이터셋을 모두 파레토 최적 혼합으로 결합하면 가장 성능이 좋은 모델을 얻을 수 있었습니다.

연구진은 최고 성능의 모델을 다양한 최신 접근 방식과 비교했습니다. 연구진의 모델은 제로 샷 성능 측면에서 기준선을 크게 앞섰습니다. ResNet-50을 기반으로 한 더 작은 변형 모델도 최신 기술을 능가했으며, 이러한 강력한 성능은 네트워크 용량이 증가했을 뿐만 아니라 제안된 훈련 체계 때문이기도 했습니다.

특정 데이터 세트에 대해 학습된 일부 모델은 해당 개별 데이터 세트에서는 매우 우수한 성능을 보였지만 다른 모든 테스트 세트에서는 성능이 현저히 떨어졌습니다. 개별 데이터 세트에 대한 미세 조정은 특정 환경에 대한 강력한 사전 지식으로 이어지며, 이는 일부 애플리케이션에서는 바람직할 수 있지만 모델을 일반화해야 하는 경우에는 적합하지 않습니다.

연구원들은 모델의 성능을 보여주기 위해 다양한 데이터 세트에 대한 추가적인 정성적 결과를 발표했습니다.

DIW 테스트 세트에는 다양한 물체, 장면, 조명 조건, 카메라 각도 등 다양한 입력 이미지 세트가 포함되었습니다. 이 이미지들은 클로즈업부터 원거리 촬영까지 다양하여 모델의 다재다능함을 보여주었습니다.

또한 연구진은 시간 정보를 사용하지 않고 모든 프레임을 개별적으로 처리하여 DAVIS 비디오 데이터 세트에 대한 정성적 결과를 보여주었습니다. 이 데이터 세트에는 사람, 동물, 자동차가 움직이는 다양한 동영상이 포함되어 있었습니다. 그러나 이 데이터 세트에는 실측 깊이 정보를 사용할 수 없었습니다.

![DIW 테스트 세트에서의 질적 결과](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%204.png)

DIW 테스트 세트에서의 질적 결과

마지막으로, 연구진은 음영이나 소실점과 같은 대략적인 깊이 단서만 있으면 공개적으로 사용 가능한 모델이 추상적인 선화와 다양한 수준의 추상화가 있는 그림에서도 그럴듯한 결과를 제공한다는 것을 확인했습니다. 이는 비교적 추상적인 입력에서도 그럴듯한 상대적 깊이를 추정할 수 있는 모델의 놀라운 능력을 보여줍니다.

![그림과 드로잉의 결과. 상단 행: 친구가 필요해, 카시우스 마르셀루스 쿨리지, 그리고 아스니에르의 목욕객, 조르주 피에르 스뤼라. 하단 행: 미티아가스타트, 빈센트 반 고흐, 그리고 오래된 유럽 도시의 중앙 거리의 벡터 그리기, 빌니우스, @Misha](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%205.png)

그림과 드로잉의 결과. 상단 행: 친구가 필요해, 카시우스 마르셀루스 쿨리지, 그리고 아스니에르의 목욕객, 조르주 피에르 스뤼라. 하단 행: 미티아가스타트, 빈센트 반 고흐, 그리고 오래된 유럽 도시의 중앙 거리의 벡터 그리기, 빌니우스, @Misha

연구원들은 모델에서 몇 가지 일반적인 실패 사례와 편견을 확인했습니다:

- 이미지의 자연스러운 편향: 일반적으로 이미지의 낮은 부분이 높은 부분보다 카메라에 더 가깝습니다. 이러한 편향은 네트워크에 의해 학습되어 회전된 이미지나 같은 거리에 있는 물체가 이미지 아래쪽의 카메라에 더 가깝게 재구성되는 경우와 같은 일부 극단적인 경우 오류가 발생할 수 있습니다.
- 그림, 사진, 거울: 모델이 이러한 물체를 인식하지 못하고 물체 자체의 깊이를 예측하는 대신 물체에 묘사된 콘텐츠를 기반으로 깊이를 추정하는 경우가 많습니다.
- 강한 가장자리: 가장자리가 강하면 깊이 불연속성이 발생하여 깊이 예측에 오류가 발생할 수 있습니다.
- 얇은 구조물: 모델이 얇은 구조물을 놓칠 수 있으며 때때로 단절된 물체 간의 상대적 깊이 배열을 정확하게 결정하지 못할 수 있습니다.
- 흐릿한 배경 영역: 제한된 입력 이미지 해상도와 원거리의 불완전한 지상 실측 자료로 인해 배경 영역에서 깊이 추정 결과가 흐려지는 경향이 있습니다.

이러한 실패 사례를 해결하려면 학습 데이터를 보강하거나 모델 아키텍처를 개선해야 할 수 있습니다. 그러나 이러한 잠재적 개선 사항 중 일부가 현재 작업에 바람직한 속성인지 고려하는 것이 중요합니다.

![실패 사례. 상대적인 깊이 배열이나 누락된 세부사항에서의 미묘한 실패가 초록색으로 강조됩니다.](Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed/Untitled%206.png)

실패 사례. 상대적인 깊이 배열이나 누락된 세부사항에서의 미묘한 실패가 초록색으로 강조됩니다.

### 7 Conclusion

저자들은 단안 깊이 추정을 위한 기존 데이터 세트가 여전히 불충분하며, 이는 앞으로의 발전을 제한하는 요인이 될 수 있다고 생각합니다. 이 문제를 해결하기 위해 보완적인 데이터 소스를 결합하는 도구, 유연한 손실 함수, 원칙에 입각한 데이터 세트 혼합 전략을 소개합니다. 또한 다양한 동적 장면에 대한 밀도 높은 지상 실측 데이터를 제공하는 3D 영화 기반 데이터 세트를 제시합니다.

연구진은 제로 샷 교차 데이터 세트 전송을 사용하여 모델의 견고성과 일반성을 평가하고, 이 방법이 사용 가능한 데이터 세트의 일부만을 대상으로 테스트하는 것보다 실제 성능에 대한 더 나은 프록시라는 것을 발견했습니다.

이 연구는 일반적인 단안 깊이 추정의 최신 기술을 발전시켰으며, 제안된 아이디어가 다양한 환경에서 성능을 크게 향상시킨다는 것을 보여줍니다. 저자들은 이 연구가 실제 애플리케이션의 요구 사항을 충족하는 단안 심도 모델을 배포하는 데 기여할 수 있기를 희망합니다. 이 모델은 [https://github.com/intel-isl/MiDaS](https://github.com/intel-isl/MiDaS) 에서 무료로 사용할 수 있습니다.