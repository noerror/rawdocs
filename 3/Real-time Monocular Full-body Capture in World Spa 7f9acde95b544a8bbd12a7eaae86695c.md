# Real-time Monocular Full-body Capture in World Space via Sequential Proxy-to-Motion Learning

[https://liuyebin.com/proxycap/](https://liuyebin.com/proxycap/)

[https://arxiv.org/abs/2307.01200](https://arxiv.org/abs/2307.01200)

### 1. Introduction

![우리의 실시간 단안 전신 캡처 시스템이 세계 공간에서 타당한 발-땅 접촉을 포함한 정확한 인간의 움직임을 만들어낼 수 있음을 보여줍니다.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled.png)

우리의 실시간 단안 전신 캡처 시스템이 세계 공간에서 타당한 발-땅 접촉을 포함한 정확한 인간의 움직임을 만들어낼 수 있음을 보여줍니다.

이 연구는 학습 기반 접근 방식을 사용하여 실시간으로 전신 동작을 캡처하는 방법을 소개합니다. 이 기술은 게임, VR/AR, 스포츠 분석 등 다양한 분야에 활용될 수 있는 중요한 기술이며, 정확하고 물리적으로 그럴듯한 동작을 구현하는 것을 목표로 합니다.

현재 3D 신체 모션 캡처를 위한 학습 기반 방식은 학습 데이터와 모션 주석의 품질에 크게 의존합니다. 그러나 마커 기반 또는 멀티뷰 시스템을 사용하여 생성된 기존 모션 캡처 데이터 세트는 다양성이 제한적입니다. 자연 상태의 데이터 세트에 대한 의사 레이블을 생성하기 위해 일부 노력을 기울였지만, 단순화된 카메라 가정과 깊이 모호성으로 인해 정확한 글로벌 모션이 부족합니다.

이 문제를 해결하기 위해 저자는 대규모 모션 시퀀스 데이터세트(AMASS)를 활용하여 가상 카메라 설정으로 순차적인 프록시-모션 쌍을 합성하는 새로운 접근 방식을 제안합니다. 이 데이터세트 생성 전략은 고품질의 대규모 데이터를 정확한 신체 움직임과 가상 카메라 설정으로 합성하여 이전 데이터세트의 한계를 극복하는 데 도움이 됩니다.

새로운 접근 방식은 2D 스켈레톤 시퀀스를 프록시 표현으로 사용하여 월드 스페이스의 3D 회전 모션 시퀀스와 연결합니다. 이렇게 하면 데이터를 개별적으로 합성한 다음 통합하여 전신 프록시 데이터 세트를 구성할 수 있습니다. 이 접근 방식은 이전의 합성 데이터 세트에 비해 깊이 모호성을 처리하고 일반화를 개선하는 데 도움이 됩니다.

프록시 데이터에서 물리적으로 그럴듯한 동작을 구현하기 위해 프록시 시퀀스를 입력으로 받아 월드 스페이스에서 3D 동작을 생성하는 네트워크 아키텍처가 제안됩니다. 이 네트워크는 먼저 거친 3D 동작을 예측한 다음 반복적인 동작 개선을 위해 접촉 인식 신경 모션 하강 모듈을 사용하여 동작을 개선합니다.

이 솔루션은 전신 프록시 데이터 세트를 활용하여 전신 모션 캡처에 적합합니다. 또한 손 부분의 컨텍스트 정보를 통합하여 전신 모델에서 더 적합한 손목 포즈를 예측합니다. 그 결과 월드 스페이스에서 발과 지면의 접촉이 그럴듯한 실시간 단안 전신 캡처가 가능합니다.

### 2. Related Work

이 논문은 단안 모션 캡처의 영역을 심층적으로 다루며 이 활발한 연구 분야가 어떻게 발전해 왔는지를 강조합니다. 저자는 단안 모션 캡처와 관련된 다양한 측면의 한계와 진전을 다룹니다.

모션 캡처 데이터 세트: 마커 기반 또는 마커가 없는 시스템을 활용하는 기존의 모션 캡처 데이터 세트는 다양성이 제한적입니다. 이러한 데이터 세트를 보강하려는 시도로 인해 이미지 공간에서는 더 잘 정렬되지만 월드 공간에서의 모션을 무시하는 의사 기준 레이블이 만들어졌습니다. 또한 인간 모델에 합성 데이터를 사용하면 실제 이미지와 영역 차이가 발생하고 생성 비용이 많이 들 수 있습니다.

휴먼 메시 복구를 위한 프록시 표현: 주석이 달린 데이터와 다양성의 부족으로 인해 원시 RGB 이미지에서 정확한 3D 동작을 학습하는 데 따르는 어려움을 완화하기 위해 실루엣, 랜드마크, 분할, IUV와 같은 프록시 표현이 사용되어 왔습니다. 이러한 프록시 표현은 관찰을 단순화하고 신경망의 학습을 더 쉽게 만들어 줍니다. 그러나 이러한 프록시 표현은 특히 단일 프레임 프록시 표현을 사용할 때 깊이와 스케일 모호성을 유발합니다. 이 연구에서 저자는 2D 스켈레톤 시퀀스를 프록시 표현으로 사용하고 정확한 월드 스페이스 모션으로 프록시 데이터를 생성합니다.

전신 모션 캡처: 최근 여러 연구에서 신체, 손, 얼굴과 같은 개별 부위를 추정하는 데 상당한 진전이 있었습니다. 일부 접근 방식은 신체 부위 모델을 회귀하고 통합하여 전신 모션 캡처를 위해 이러한 노력을 결합합니다. 하지만 이러한 방법은 월드 스페이스에서 실시간 정확도를 제공하는 데 어려움을 겪습니다. 저자들의 연구는 새로운 데이터 생성 전략과 새로운 네트워크 아키텍처를 통해 이 문제를 해결하여 발과 지면이 접촉하는 것처럼 보이는 실시간 전신 캡처를 구현합니다.

모션 캡처를 위한 뉴럴 하강: 3D 파라메트릭 모델을 위한 기존의 최적화 기반 방법은 초기화 민감도가 높고 까다로운 포즈를 처리하지 못하는 문제가 있습니다. 최근 연구에서는 반복적인 개선을 위해 신경망을 활용하여 보다 효율적이고 강력한 모션 예측을 제공합니다. 저자는 접촉 인식 신경 하강 모듈을 도입하고 보다 효과적인 모션 업데이트를 위해 교차주의를 사용합니다.

모션 캡처의 물리적 타당성: 기존의 단안 모션 캡처 방식은 종종 지면 침투나 풋 스케이팅과 같은 아티팩트가 있는 결과를 생성합니다. 이전 연구에서는 보다 정확한 카메라 모델을 활용하거나 신체 접촉 정보를 명시적으로 학습하여 이 문제를 해결하려고 시도했습니다. 그러나 이러한 접근 방식은 종종 높은 계산 비용이 필요하며 실시간 애플리케이션에는 적합하지 않습니다. 저자는 새로운 네트워크 설계와 프록시 데이터의 정확한 모션 감독을 통해 이러한 문제를 극복하고 월드 스페이스에서 발과 지면이 접촉하는 것처럼 보이는 실시간 캡처를 제공합니다.

### 3. Proxy Data Generation

저자는 2D 스켈레톤 시퀀스를 프록시 표현으로 활용하고 순차적인 프록시-모션 데이터를 합성하여 기존 데이터 세트의 정밀도 및 일반화와 관련된 문제를 해결합니다. 이 프로세스는 카메라 및 모션 디커플링과 합성 데이터 생성의 두 부분으로 나뉩니다.

![제안된 방법의 설명. 우리의 방법은 몸과 손의 예측된 2D 골격 포즈를 입력으로 받고, 세계 공간에서 타당한 발-땅 접촉을 가진 3D 움직임을 복원합니다.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled%201.png)

제안된 방법의 설명. 우리의 방법은 몸과 손의 예측된 2D 골격 포즈를 입력으로 받고, 세계 공간에서 타당한 발-땅 접촉을 가진 3D 움직임을 복원합니다.

카메라 및 모션 디커플링

기존 방식은 카메라 좌표계에서 사람의 포즈를 직접 회귀시키기 때문에 동일한 프로젝션 함수를 다른 장면에 적용할 때 모호한 부분이 생깁니다. 이 문제를 완화하기 위해 저자는 카메라 포즈와 사람의 움직임을 분해하여 월드 스페이스에서 카메라 포즈와 사람의 움직임을 독립적으로 예측할 수 있도록 합니다. 저자들은 카메라가 지면 위 높이 h에서 z축을 따라 고정된 고전적인 핀홀 카메라 모델을 사용합니다. 카메라 방향은 정면을 향하므로 피치 α와 롤 ϕ의 두 가지 파라미터만으로 카메라 포즈를 단순화할 수 있습니다. 또한 회전 파라미터 θw와 글로벌 트랜지션 tw로 월드 스페이스에서 신체 움직임을 표현하여 발과 지면의 접촉을 더욱 그럴듯하게 표현할 수 있습니다.

![물리적 사전을 얻기 위해 세계 공간에서 카메라 포즈와 인간의 움직임을 분리하는 설명.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled%202.png)

물리적 사전을 얻기 위해 세계 공간에서 카메라 포즈와 인간의 움직임을 분리하는 설명.

데이터 합성 및 통합

저자는 2D 스켈레톤 시퀀스를 프록시 표현으로 활용하고 가상 카메라로 기존 모션 데이터 세트를 기반으로 프록시 시퀀스를 합성합니다. 신체 동작, 손 제스처, 접촉 레이블 등 다양한 유형의 레이블을 프록시 데이터에 합성하고 통합하는 방법을 자세히 설명합니다.

신체 프록시 데이터: 3772분 분량의 다양하고 복잡한 신체 동작 시퀀스 데이터가 포함된 AMASS 데이터 세트는 신체 부위에 대한 프록시-모션 쌍을 생성하는 데 사용됩니다. 데이터는 초당 60프레임으로 다운샘플링됩니다.

손 제스처와 통합: 1361K 프레임의 제스처 데이터가 포함된 InterHand 데이터 세트는 손 포즈로 프록시 데이터를 보강하는 데 사용됩니다. 데이터는 40, 50, 60fps로 업샘플링되어 AMASS 모션 시퀀스의 신체 포즈와 무작위로 통합됩니다.

접촉 라벨과 통합: 접촉 레이블을 생성하기 위해 저자는 LEMO 방법론을 따르고 SMPL 모델 하단에 있는 6개의 정점을 선택하여 연속 접촉 지표를 계산합니다.

사용된 카메라 설정은 기존 공개 데이터 세트에 사용된 것과 유사합니다. 카메라 시점은 가변성을 보장하기 위해 무작위로 지정되며, 카메라로부터의 다양한 거리를 시뮬레이션하기 위해 사람 모델에 무작위 회전과 변위를 적용합니다. 의사 2D 스켈레톤 주석은 신체 모델의 3D 관절에 가우시안 노이즈를 추가하고 이를 이미지 평면에 투영하여 생성됩니다.

### 4. Method

이 섹션에서는 2D 스켈레톤 시퀀스를 월드 스페이스에서 3D 인체 모션으로 변환하는 제안된 방법을 설명합니다. 이 방법은 순차적 전신 모션 복구, 접촉 인식 신경 모션 하강, 손실 함수 계산의 세 가지 주요 단계로 구성됩니다.

![신경 움직임 하강을 위한 제안된 교차 주의 설명.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled%203.png)

신경 움직임 하강을 위한 제안된 교차 주의 설명.

4.1 순차적 전신 모션 복구
감지된 2D 골격 시퀀스가 네트워크에 공급됩니다. 템포럴 인코더는 시퀀스에서 특징을 추출한 다음 회귀기에 의해 처리하여 월드 스페이스에서 전신 모션을 생성합니다. 이 인코더는 시간 확장 컨볼루션 네트워크를 기반으로 합니다. 정확한 예측을 위해 글로벌 궤적과 신체 모션은 별도로 학습됩니다. 또한 교차 주의 메커니즘을 사용하여 몸과 손 복구 간의 동작 컨텍스트를 공유하므로 손목 포즈가 더 비슷해집니다.

4.2 접촉 인식 신경 모션 하강
이 단계에서는 초기 모션 예측을 보다 정확하고 물리적으로 그럴듯하게 만들기 위해 개선합니다. 신경망인 하강 모듈은 오정렬 및 발과 지면 접촉 상태를 입력으로 받아 업데이트된 인간 파라미터를 출력합니다. 이 모듈은 교차주의를 사용하여 상태와 편차 그룹 간의 관계를 효과적으로 학습하여 보다 정확한 모션 업데이트를 수행합니다. 또한 이 접근 방식은 순차적 모션 컨텍스트를 사용하여 모션 예측을 개선합니다.

4.3 손실 함수
전신 모션 복구 모듈과 접촉 인식 신경 모션 하강 모듈은 개별적으로 훈련됩니다. 손실 함수는 예측된 모션과 실제 모션의 차이를 계산합니다. 모션 복구 손실 함수는 3D MPJPE 손실, 3D 궤적 L1 손실, 투영된 2D MPJPE 손실, 추정된 사람의 포즈, 모양, 카메라 포즈와 기준점 사이의 손실 등 여러 구성 요소로 이루어져 있습니다. 신경 하강 모듈의 객관적 손실에는 궤적 드리프트, 발 부유 또는 지면 관통의 오차, 예측된 접촉 레이블과 지상 실측 사이의 손실도 포함됩니다.

### 5. Experiments

5.1. 최신 기술과의 비교

이 방법은 Human3.6M, 3DPW, EHF 및 합성 데이터 세트와 같은 공개 데이터 세트에서 평가되었습니다. 이 방법은 스켈레톤 기반 방법 및 메시 기반 방법과 비교되었습니다. 스켈레톤 기반 방법은 MPJPE/P-MPJPE에서 더 높은 점수를 받았지만, 정점 대 지면 접촉 정보와 인체 운동학을 캡처하는 데 어려움을 겪었습니다. 개발된 방법은 메시 기반 방법과 비슷한 결과를 제공하며 실시간 효율성, 전신 모션 캡처, 그럴듯한 지면 접촉 제약 조건 등 여러 가지 이점을 제공합니다.

![(a) 3DPW [58], (b) EHF [40], 그리고 (c) Human3.6M [14] 데이터셋과 (d,e,f) 우리의 캡처된 시퀀스에서 다양한 케이스를 가로질러 복구 결과의 시각화.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled%204.png)

(a) 3DPW [58], (b) EHF [40], 그리고 (c) Human3.6M [14] 데이터셋과 (d,e,f) 우리의 캡처된 시퀀스에서 다양한 케이스를 가로질러 복구 결과의 시각화.

5.2. 절제 연구

방법의 각 구성 요소를 검증하기 위해 다양한 설정을 사용하여 제거 연구를 수행했습니다. 네 가지 설정으로 실험을 진행했습니다: 실내, 신디사이저, 퓨전, 퓨전+ND의 네 가지 설정으로 실험을 진행했습니다.

퓨전 훈련 스케줄은 CPN이 감지한 2D 입력과 기준선(GT) 2D 입력 모두에서 Human3.6M 또는 합성 데이터세트만을 사용한 훈련보다 우수한 성능을 보였습니다.

모션 컨텍스트 공유 방식(Temp)과 접촉 인식 신경 모션 하강 모듈(Desc)에 대한 제거 연구에서는 신체 MPJPE, 손 MPJPE, 글로벌 MPJPE, 지면 침투(GP), 발 부동(FF) 메트릭 측면에서 성능이 개선된 것으로 나타났습니다.

또한 기존 신경 하강 방식과 대표적인 전신 동작 회복 방식인 PyMAF-X와 비교했을 때 부자연스러운 몸 기울임과 무릎 굽힘을 방지하고 발 스케이팅과 지면 침투를 방지하는 성능이 개선된 것으로 나타났습니다.

5.3. 계산 복잡도

개발된 실시간 단안 전신 캡처 시스템은 노트북 한 대에서 구현할 수 있습니다. 2D 포즈 추정기는 미디어파이프 프레임워크를 활용하며, 엔비디아 텐서RT 플랫폼에서 반정밀 연산으로 재구현되었습니다. 각 모듈의 추론 시간은 보고되었지만 제공하신 텍스트에는 언급되어 있지 않습니다.

![정성적인 비교. (a) 왼쪽: LearnedGD [51], 오른쪽: 우리의 방법, (b) 위: PyMAF-X [68], 아래: 우리의 방법.](Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c/Untitled%205.png)

정성적인 비교. (a) 왼쪽: LearnedGD [51], 오른쪽: 우리의 방법, (b) 위: PyMAF-X [68], 아래: 우리의 방법.

### 6. Conclusion

이 논문에서는 월드 스페이스에서 신뢰할 수 있는 발과 지면 접촉을 이용한 실시간 전신 모션 캡처 방법을 소개합니다. 이 방법은 순차적 프록시-모션 학습 체계를 통합하고 월드 스페이스에서 정확한 3D 회전 동작을 가진 2D 골격 시퀀스의 프록시 데이터 집합을 합성합니다.

보다 정확하고 물리적으로 그럴듯한 결과를 얻기 위해 접촉 인식 신경 모션 하강 모듈이 제안되어 네트워크에 통합됩니다. 또한 이 네트워크는 몸과 손의 컨텍스트 정보를 활용하여 손목 포즈가 전신 모델과 더 잘 호환되도록 합니다.

이 연구는 특히 월드 스페이스에서 발과 지면의 접촉을 보다 사실적으로 구현하는 측면에서 기존 솔루션에 비해 우수한 결과를 제공하는 실시간 단안 전신 캡처 시스템을 시연합니다.

하지만 이 방법에는 몇 가지 한계가 있습니다. 이전 연구와 마찬가지로, 제안된 방법은 희박한 골격에서 신체 형태를 캡처하는 것이 어렵습니다. 또한 이 방법은 월드 스페이스에서 더 신뢰할 수 있는 결과를 보여주지만, 특히 카메라 설정이 훈련 데이터에서 벗어날 때 메시와 이미지의 정렬이 최신 이미지 기반 솔루션보다 약간 떨어집니다.