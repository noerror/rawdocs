# Zero-Shot Metric Depth with a Field-of-View Conditioned Diffusion Model

[https://arxiv.org/abs/2312.13252](https://arxiv.org/abs/2312.13252)

- Dec 2023

### 1. Introduction

단안 깊이 추정의 과제는 모바일 로봇 공학 및 자율 주행과 같은 다양한 환경에 적용하는 데 있습니다. 이 분야의 발전을 가로막는 두 가지 주요 장벽이 있습니다:

- RGB와 깊이 분포의 차이: 실내와 실외 환경은 RGB 및 심도 데이터 특성이 크게 다릅니다. 이러한 차이는 두 가지 설정을 모두 정확하게 평가할 수 있는 범용 모델을 개발하는 것을 복잡하게 만듭니다.
    
    ![8개의 제로샷 벤치마크와 2개의 인-디스트리뷰션(∗) 벤치마크에서 DMD(당사)의 상대적 깊이 오차, ZoeDepth(SOTA)와 비교. DMD는 모든 벤치마크에서 ZoeDepth를 큰 차이로 앞섰습니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled.png)
    
    8개의 제로샷 벤치마크와 2개의 인-디스트리뷰션(∗) 벤치마크에서 DMD(당사)의 상대적 깊이 오차, ZoeDepth(SOTA)와 비교. DMD는 모든 벤치마크에서 ZoeDepth를 큰 차이로 앞섰습니다.
    
- 카메라 고유 특성 부족으로 인한 스케일 모호성: 사용되는 카메라의 특정 특성(본질)을 알지 못하면 이미지에서 물체의 실제 크기를 결정하는 것이 모호해집니다.

일반적으로 깊이 추정 모델은 실내 또는 실외 장면에 초점을 맞추거나, 둘 다에 맞게 설계된 경우 메트릭 깊이 대신 스케일 불변 깊이를 추정합니다. 이러한 모델의 학습은 일반적으로 카메라 내재가 고정된 단일 데이터 세트(예: 실내 장면의 경우 RGBD 카메라, 실외 환경의 경우 RGB+LIDAR)에 국한됩니다. 이러한 접근 방식은 모델의 일반성을 제한하고 학습 데이터의 특정 카메라 고유 특성에 과적합하게 되어 학습되지 않은 다른 데이터에서 성능이 저하됩니다.

실내 및 실외 데이터를 모두 처리하는 일반적인 방법은 스케일 및 시프트 불변의 깊이를 추정하는 것입니다. MiDAS와 같은 모델에서 볼 수 있는 이 접근 방식은 깊이 분포를 정규화하여 다양한 카메라 특성으로 인한 실내-외 차이와 스케일 모호성을 완화합니다. 그러나 최근의 추세는 실내 및 실외 장면 모두에 대해 메트릭 심도를 추정하는 모델을 개발하는 데 관심을 보이고 있습니다. 예를 들어, ZoeDepth는 각 도메인에 대해 두 개의 개별 구성 요소를 추가하여 스케일 불변에서 메트릭 심도로 변환할 수 있도록 함으로써 MiDaS를 조정합니다.

이 논문에서는 제로 샷 메트릭 뎁스 추정을 위해 노이즈 제거 확산 모델을 활용하는 새로운 모델인 DMD(디퓨전 포 메트릭 뎁스)를 소개합니다. DMD는 몇 가지 혁신적인 기능을 통합하여 최첨단 성능을 구현합니다:

- 시야각(FOV) 증강: 훈련 중에 사용되는 이 기능은 모델이 다양한 카메라 속성으로 일반화하는 능력을 향상시킵니다.
- FOV 컨디셔닝: 트레이닝과 추론 모두에서 구현되며 이미징 프로세스에 내재된 스케일 모호성을 해결하는 데 도움이 됩니다.
- 로그 도메인의 깊이 표현: 이 접근 방식은 실내 및 실외 장면에서 모델 용량의 균형을 보다 효과적으로 조정하여 실내 환경에서의 성능을 향상시킵니다.
- 신경망 노이즈 제거의 v-파라미터화: 추론 속도를 크게 향상시킵니다.

DMD는 특히 동일한 데이터 세트에서 미세 조정할 때 기존 메트릭 깊이 모델인 ZoeDepth보다 성능이 뛰어난 것으로 나타났습니다. 다양한 분포에서 벗어난 데이터 세트에서 상대적 깊이 오차가 현저히 낮습니다. 또한 훈련 데이터 세트를 확장하면 성능이 더욱 향상됩니다.

이 연구의 공헌은 다음과 같이 요약할 수 있습니다:

- 일반 장면에서 제로 샷 메트릭 깊이 추정을 위한 새롭고 효과적인 방법인 DMD를 소개합니다.
모델 일반화를 개선하고 깊이 스케일 모호성을 해결하기 위한 방법(예: FOV 증강, FOV 컨디셔닝, 로그 스케일 깊이 표현)을 제안했습니다.
- 실내 및 실외 환경 모두에서 기존 모델보다 훨씬 낮은 상대적 오류를 달성하는 동시에 확산의 v-파라미터화로 인해 더 효율적인 제로 샷 메트릭 깊이 추정에서 새로운 표준을 확립했습니다.

### 2. Related Work

이 논문은 단안 심도 추정에 관한 기존 연구의 맥락에서 이 분야의 주요 영역과 발전 사항을 강조하면서 그 공헌을 설명합니다.

특정 도메인에 대한 단안 심도 추정

- 도메인 내 깊이 추정: 대부분의 기존 모델은 실내 또는 실외 환경 중 하나에 맞춰져 있으며, 두 환경 모두에 적용되지는 않습니다. 이러한 모델은 특수한 아키텍처와 손실 함수를 통해 상당한 발전을 이루었습니다. 일부 연구자들은 이미지를 공통의 내재적 표준으로 정규화하여 다양한 카메라 내재적 특성을 가진 데이터 세트를 결합하려고 시도했습니다. 그러나 이러한 시도는 주로 실내 또는 실외 장면에 각각 초점을 맞춘 것이었습니다.

실내-외 공동 모델

- 스케일 및 시프트 불변 깊이 추정: 실내 및 실외 장면을 모두 처리하기 위해 일부 모델은 스케일 및 시프트 불변 깊이를 추정합니다. 예를 들어, 다양한 데이터 세트에 대해 학습된 MiDaS는 일반화는 잘 이루어졌지만 메트릭 심도는 제공하지 못했습니다. DPT와 ZoeDepth는 이 접근 방식을 확장했습니다. 이들은 유사한 다양한 데이터 세트를 사전 학습한 다음 메트릭 심도를 미세 조정했으며, ZoeDepth는 다양한 장면 유형을 처리하기 위해 전문가 혼합 모델을 도입했습니다. 하지만 이러한 접근 방식은 여전히 아키텍처에 도메인별 컴포넌트를 포함해야 했습니다.

인트릭스-컨디셔닝된 단안 뎁스

- 심도 추정의 카메라 본질: 일부 연구에서는 카메라 내재성을 깊이 추정 모델에 통합하는 방법을 모색했습니다. 이러한 모델은 다양한 내재성을 가진 여러 데이터 세트에서 훈련할 수 있는 잠재력을 보여주었습니다. 이 백서에서 소개하는 접근 방식인 DMD는 이러한 아이디어를 바탕으로 새로운 시야각(FOV) 증강 기법을 도입합니다. 이 기법은 다양한 실내 및 실외 장면과 다양한 카메라 내재적 특성에서 모델의 일반화를 개선하기 위해 다양한 FOV를 시뮬레이션합니다.

비전에서의 확산 모델

- 노이즈 제거 확산 모델: 최근 노이즈 제거 확산 모델은 처음에는 이미지 생성에, 나중에는 의미적 분할 및 광학 흐름과 같은 다양한 컴퓨터 비전 작업에 사용되는 강력한 생성 도구로 각광받고 있습니다. 이 논문은 확산 모델이 실내와 실외의 일반적인 장면에서 최첨단 제로샷 메트릭 깊이 추정을 효과적으로 지원할 수 있음을 입증한 최초의 논문이라고 주장합니다.

요약하자면, 이 논문은 전문화된 도메인 내 모델, 실내외 공동 모델, 내재적 조건부 접근법, 컴퓨터 비전 분야에서 노이즈 제거 확산 모델의 진화하는 사용을 배경으로 그 공헌도를 평가합니다. DMD 모델은 도메인에 국한되지 않는 보다 일반적인 프레임워크를 제공하며, 다양한 환경에서 향상된 메트릭 깊이 추정을 위한 확산 모델의 적용과 함께 FOV 확대 및 조절을 새롭게 사용한다는 점에서 차별화됩니다.

### 3. Diffusion for Metric Depth (DMD)

이 섹션에서는 노이즈 제거 확산을 사용한 단안 심도 추정 모델인 DMD(디노이즈 확산)의 개발 및 설계 결정에 대해 간략하게 설명합니다. 제로 샷, 메트릭 깊이 추정을 용이하게 하기 위해 기존 확산 모델에 통합된 기술 혁신에 대해 자세히 설명합니다.

3.1 확산 모델

확산 모델은 목표 분포를 노이즈 분포로 변환한 다음 신경 노이즈 제거기를 사용하여 이 과정을 역전시키는 확률론적 모델입니다. 이러한 모델은 세분화 및 깊이 추정과 같은 작업을 포함하여 이미지 및 비디오 처리에서 성공적으로 사용되어 왔습니다. DMD는 이러한 모델을 기반으로 구축되었으며, 특히 DDVM의 효율적인 U-Net 아키텍처를 사용합니다. DDVM과 달리 DMD는 최소한의 정제 단계로 효율적인 추론이 가능한 v-파라미터화를 사용합니다. 이 접근 방식은 노이즈 제거 네트워크에 노이즈가 있는 대상 이미지(뎁스 맵)를 공급하고 특정 매개변수(v)를 예측하는 것입니다. 모델은 더 나은 결과를 위해 L1 손실 함수를 사용하여 '절단된 SNR 가중치' 손실로 훈련됩니다.

3.2 실내-외 공동 모델링

실내와 실외 장면의 심도 분포 차이를 해결하기 위해 DMD는 몇 가지 혁신적인 기능을 통합했습니다:

- 로그 깊이: DMD는 선형 스케일링 대신 로그 스케일링 깊이를 사용하여 일반적으로 깊이가 10미터 미만인 실내 장면에 더 많은 표현 용량을 할당합니다. 이 로그 스케일링은 상당한 이점이 있는 것으로 입증되었습니다.
- 시야각 증강: 시야각의 변화가 거의 없는 데이터 세트에 모델이 과적합되는 것을 방지하기 위해 DMD는 자르기 및 자르기 해제를 통해 다양한 시야각을 시뮬레이션하여 훈련 데이터를 보강합니다. 자르지 않은 이미지에는 단순성과 효율성을 위해 가우시안 노이즈가 추가됩니다. 이러한 증강은 모델이 다양한 카메라 특성에 적응하는 데 도움이 됩니다.
- 시야각 컨디셔닝: 알 수 없는 카메라 고유 특성으로 메트릭 심도를 추정하는 것이 어렵다는 점을 감안하여 DMD는 수직 시야의 절반 탄젠트를 컨디셔닝 신호로 사용합니다. 이 기술은 뎁스 스케일을 명확히 하고 다양한 카메라 설정에서 일반화를 개선하는 데 도움이 됩니다.

요약하면, DMD는 확산 모델을 사용하고 로그 깊이 표현, 시야 확대, 컨디셔닝과 같은 새로운 기술을 도입함으로써 단안 깊이 추정에서 상당한 발전을 이루었습니다. 이러한 혁신을 통해 이 모델은 실내 및 실외 환경의 복잡성과 다양한 카메라 특성을 효과적으로 처리할 수 있습니다.

### 4. Experiments

이 섹션에서는 훈련 데이터, 디자인 선택, 결과 및 다양한 절제를 포함하여 DMD(디퓨전 포 메트릭 뎁스)를 평가하기 위해 수행한 실험에 대해 설명합니다.

![실내 장면에서 우리 방법과 ZoeDepth [5]의 질적 비교. ZoeDepth와 달리, 우리의 방법은 다양한 데이터 세트에서 더 정확한 규모로 깊이를 추정합니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled%201.png)

실내 장면에서 우리 방법과 ZoeDepth [5]의 질적 비교. ZoeDepth와 달리, 우리의 방법은 다양한 데이터 세트에서 더 정확한 규모로 깊이를 추정합니다.

![실외 장면에서 DMD와 ZoeDepth [5]의 질적 비교. ZoeDepth [5]와 비교했을 때, 우리의 방법은 더 정확한 깊이 스케일을 추정할 수 있습니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled%202.png)

실외 장면에서 DMD와 ZoeDepth [5]의 질적 비교. ZoeDepth [5]와 비교했을 때, 우리의 방법은 더 정확한 깊이 스케일을 추정할 수 있습니다.

4.1 훈련 데이터

DMD를 훈련하려면 크고 다양한 데이터 세트가 중요합니다. 모델은 처음에 ImageNet 및 Places365 데이터 세트에 대한 비지도 사전 학습을 거칩니다. 그런 다음 ScanNet, SceneNet-RGBD, Waymo 및 DIML Indoor 데이터 세트에 대한 지도 사전 학습으로 이동하여 다양성을 더합니다. 최종 훈련 단계에는 NYU, Taskonomy, KITTI, nuScenes와 같은 데이터 세트가 포함되며, 일부 데이터 세트에는 시야각(FOV) 증강이 적용되지만 모든 데이터 세트에는 적용되지는 않습니다. 또한 이 모델의 변형은 ZoeDepth 모델과의 공정한 비교를 위해 NYU와 KITTI에 대해서만 훈련되었습니다.

4.2 설계 선택

- 디노이저 아키텍처: 효율적인 U-Net 모델은 FOV 컨디셔닝을 위해 수정되어 적용됩니다. 이 아키텍처는 FOV 임베딩을 위해 사인-코스 위치 임베딩과 선형 투영의 조합을 사용합니다.
- 증강: FOV 증강 외에도 무작위 수평 플립 증강이 사용됩니다.
- 샘플러: 실내 및 실외 데이터 세트에 대해 다양한 수의 노이즈 제거 단계가 있는 DDPM 샘플러가 사용되며, 여러 샘플을 평균화하여 성능을 향상시킵니다.

4.3 결과

- 제로 샷 성능: DMD는 대부분의 분포 외(OOD) 데이터 세트에서 ZoeDepth보다 우수한 성능을 보여줍니다. 더 큰 데이터 세트에서 미세 조정을 하면 특히 깊이 스케일과 세밀한 디테일 측면에서 DMD의 성능이 더욱 향상됩니다.
- 배포 내 성능: KITTI 및 NYU 데이터 세트에서 DMD는 ZoeDepth에 비해 경쟁력이 있거나 우수한 성능을 보여줍니다.

4.4 절제

몇 가지 절제를 통해 DMD의 다양한 구성 요소를 테스트합니다:

- 로그와 선형 스케일 뎁스: 로그 스케일링은 특히 실내 데이터 세트나 깊이가 얕은 실외 데이터 세트의 경우 정량적 개선을 보여줍니다. 또한 선형 스케일링에서 나타나는 노이즈 아티팩트도 해결합니다.
    
    ![깊이를 선형적으로 스케일링하면 깊이가 얕은 이미지에 대해 노이즈가 많은 예측을 하게 됩니다. 자세한 내용은 섹션 3.2를 참조하십시오. 로그 스케일로 깊이를 예측하면 이 문제가 해결됩니다. 여기서는 더 나은 시각화를 위해 최대 5m의 깊이를 사용했습니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled%203.png)
    
    깊이를 선형적으로 스케일링하면 깊이가 얕은 이미지에 대해 노이즈가 많은 예측을 하게 됩니다. 자세한 내용은 섹션 3.2를 참조하십시오. 로그 스케일로 깊이를 예측하면 이 문제가 해결됩니다. 여기서는 더 나은 시각화를 위해 최대 5m의 깊이를 사용했습니다.
    
- 시야각 조절: 이 방법은 실제 시야각에 가까운 최적의 결과를 얻을 수 있는 최고의 성능을 구현합니다.

![DMD-NK(NYU 및 KITTI에서 미세 조정)와 DMD-MIX(KITTI, NYU, nuScenes 및 Taskonomy에서 미세 조정)의 질적 비교. DMD-MIX는 뎁스 스케일 추정과 뎁스 경계에 대한 세밀한 디테일을 더욱 향상시킵니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled%204.png)

DMD-NK(NYU 및 KITTI에서 미세 조정)와 DMD-MIX(KITTI, NYU, nuScenes 및 Taskonomy에서 미세 조정)의 질적 비교. DMD-MIX는 뎁스 스케일 추정과 뎁스 경계에 대한 세밀한 디테일을 더욱 향상시킵니다.

- 시야각 확대 또는 컨디셔닝이 없습니다: 이러한 기능이 없으면 성능이 저하되므로 그 중요성이 강조됩니다.
- ϵ 대 v 확산 파라미터화: v 매개변수화는 우수한 성능을 위해 필요한 노이즈 제거 단계의 수를 줄여주므로 ϵ 매개변수화보다 효율적입니다.
    
    ![추론 중 FOV 교란의 효과를 보여주는 플롯. 최적의 성능은 실제 FOV 또는 그 근처에서 나타납니다. 섭동이 클수록 성능이 저하됩니다.](Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633/Untitled%205.png)
    
    추론 중 FOV 교란의 효과를 보여주는 플롯. 최적의 성능은 실제 FOV 또는 그 근처에서 나타납니다. 섭동이 클수록 성능이 저하됩니다.
    

요약하면, 실험을 통해 DMD가 제로 샷과 배포 중 시나리오 모두에서 탁월한 성능을 발휘하며 ZoeDepth와 같은 기존 모델보다 성능이 뛰어나다는 것을 알 수 있습니다. 훈련 데이터, 아키텍처, 제거 연구의 혁신은 모두 다양한 깊이 추정 작업을 처리할 수 있는 견고성과 다용도성에 기여합니다.

### 5 Conclusion

일반적이고 다양한 접근 방식: DMD 모델은 단안 메트릭 깊이 추정을 위한 확산 기반 생성기입니다. 이 모델의 강점은 작업별 귀납적 편향이나 특수한 아키텍처 없이도 작동하는 범용성에 있습니다. 이러한 범용성 덕분에 실내 및 실외의 다양한 장면을 능숙하게 처리할 수 있습니다.

혁신적인 로그 스케일 심도 파라미터화: 로그 스케일 깊이 매개변수화는 모델의 중요한 측면입니다. 이 접근 방식은 모델이 다양한 깊이 범위에 적절한 표현 용량을 할당하도록 보장하여 이전 모델의 일반적인 한계를 해결합니다.

- 일반화를 위한 FOV 증강: 자르기 및 자르기 해제를 통해 훈련 데이터의 시야각(FOV)을 증강하는 새로운 방법이 도입되었습니다. 이 기법은 모델이 학습 데이터 세트에 존재하는 시야각 이상의 시야각으로 일반화할 수 있도록 하는 데 특히 효과적입니다. 노이즈 패딩을 사용한 언크롭이라는 간단하면서도 효과적인 전략이 더 큰 FOV를 시뮬레이션하는 수단으로 강조됩니다.
- FOV 컨디셔닝의 핵심: FOV에 대한 모델 컨디셔닝은 뎁스 스케일을 명확히 하는 데 매우 중요한 것으로 확인되었습니다. 이 인사이트는 심도 추정 시 카메라별 변수를 고려하는 것이 얼마나 중요한지를 강조합니다.
- 새로운 데이터 세트 혼합으로 향상된 성능: 새로운 미세 조정 데이터 세트 혼합물의 도입으로 모델의 성능이 크게 향상되었습니다. 이 데이터 세트 혼합은 모델의 훈련에 맞게 조정되어 모델의 성공에 중추적인 역할을 합니다.
- 새로운 표준 설정: 이러한 혁신을 통해 DMD는 단안 메트릭 깊이 추정 영역에서 새로운 기준을 제시합니다. 모든 제로샷 및 도메인 내 데이터 세트에서 이전 모델, 특히 ZoeDepth를 상당한 차이로 능가합니다.

요약하자면, 이 논문은 DMD를 단안 깊이 추정 분야의 획기적인 발전으로 소개하며 다양한 설정에서 뛰어난 성능과 적응성을 보여줍니다. 로그 스케일 깊이 매개변수화 및 FOV 증강과 같은 새로운 기술과 전략적 데이터 세트 활용의 결합으로 DMD는 해당 분야에서 선도적인 모델로 자리매김하고 있습니다.

### Appendix

추가 인사이트 및 훈련 세부 정보

이 백서에서는 추가적인 정성적 예시를 제공하고 알 수 없는 카메라 시야각(FOV) 처리에 대해 설명하며 DMD 모델에 대한 추가 학습 세부 정보를 제공합니다.

A. 추가 정 성적 예제

- 비교 분석: 그림 7과 그림 8에는 각각 실내 및 실외 장면에 대한 DMD와 ZoeDepth의 추가적인 정성적 비교가 나와 있습니다. 이러한 예는 실내와 실외의 다양한 장면에서 DMD가 일관되게 더 정확한 메트릭 스케일 깊이 추정을 달성한다는 관찰을 뒷받침합니다.

B. 알 수 없는 시야 처리

- 현실적인 과제: 실제 애플리케이션에서, 특히 휴대폰, 로봇 플랫폼 또는 자율 주행 자동차와 같은 장치에서 FOV를 포함한 카메라의 본질적인 특성은 일반적으로 알려져 있습니다. 그러나 인터넷 이미지나 생성 이미지와 같은 특정 경우에는 이러한 특성을 알 수 없습니다.
- FOV 추정 솔루션: 이 문제를 해결하기 위해 수직 FOV를 추정하는 간단한 모델이 개발되었습니다. 이 모델은 사전 학습된 팔레트 모델의 인코더를 기반으로 수직 FOV 각도의 절반의 탄젠트로 회귀합니다. 이 모델은 FOV 증강 기능이 있는 NYU 및 KITTI 데이터 세트를 사용하여 훈련되었습니다.
- 성능 비교: 추정 FOV와 실제 FOV를 사용한 깊이 추정 성능을 비교한 결과, DIML 실외 데이터 세트를 제외하고는 경쟁력이 있는 결과를 보였습니다. DIML 실외의 FOV가 클수록 단순 FOV 추정기에 문제가 발생하여 깊이 추정 성능이 저하되었습니다.
- 향후 개선 사항: 카메라 내재적 예측을 위해 보다 정교한 모델을 통합하면 FOV 추정 정확도와 결과적으로 깊이 추정이 향상될 수 있습니다. 이 영역은 향후 연구를 위해 제안됩니다.

C. 추가 훈련 세부 사항

- 훈련 단계: 이 모델은 두 가지 주요 훈련 단계를 거쳤습니다. 첫 번째 지도 훈련 단계는 1 × 10^-4의 학습률로 150만 걸음 동안 진행되었습니다. 두 번째(최종) 단계는 3 × 10^-5의 학습률로 50,000개의 스텝에 걸쳐 진행되었습니다.
- FOV 증강 전략: 두 번째 훈련 단계에서는 이미지와 뎁스 맵을 자르거나 자르지 않기 위해 0.8에서 1.5 사이의 배율을 무작위로 샘플링하여 FOV를 확대했습니다. 이 전략은 다양한 FOV 조건에 적응하는 모델의 능력을 향상시키는 데 필수적인 요소였습니다.

요약하면, 이 논문은 DMD의 우수한 성능에 대한 추가적인 증거를 제시하고 깊이 추정에서 알려지지 않은 FOV의 실질적인 문제를 해결합니다. 간단한 FOV 추정 모델과 훈련 프로세스에 대한 세부 사항이 포함되어 있어 DMD 모델을 개발할 때 취한 포괄적인 접근 방식을 더욱 잘 보여줍니다. 이러한 측면은 다양한 시나리오에서 모델의 견고성과 적응성을 강조하여 향후 이 분야의 발전을 위한 발판을 마련합니다.