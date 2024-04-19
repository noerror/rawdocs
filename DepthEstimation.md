# DepthEstimation

[Depth-Aware Video Frame Interpolation](3/Depth-Aware%20Video%20Frame%20Interpolation%2013f0deb9050643b3894760c7398562ed)

비디오 프레임 보간을 위한 깊이 인식 알고리즘을 제시합니다. 이 기술은 깊이 정보를 활용하여 비디오에서 객체의 상대적 거리를 감지하고, 이를 통해 더 부드러운 중간 프레임을 생성합니다. 이 연구는 비디오 시퀀스에서 기존 프레임 사이에 새로운 프레임을 생성하는 과정에 초점을 맞추며, 특히 오클루전(시야 차단) 상황에서도 효과적입니다. 연구진은 깊이 정보를 사용하여 오클루전을 명시적으로 감지하고, 이를 통해 중간 프레임 생성 시 가까운 객체에 우선순위를 부여합니다. 이 알고리즘은 계층적 특징을 사용해 주변 픽셀에서 컨텍스트를 수집하고, 이를 통해 고품질의 비디오 프레임을 생성합니다. 실험 결과, 이 새로운 모델은 다양한 데이터 세트에서 기존 방법보다 우수한 성능을 보여줍니다.

[Towards Robust Monocular Depth Estimation: Mixing Datasets for Zero-shot Cross-dataset Transfer](3/Towards%20Robust%20Monocular%20Depth%20Estimation%20Mixing%20D%200d93469a68bf4460898b11b588ad25ed)

단일 카메라 깊이 추정을 강화하기 위한 새로운 접근법을 탐구합니다. 이 연구는 다양한 데이터 소스에서 훈련 데이터를 결합하고, 데이터 세트 간 일관성을 유지하기 위한 새로운 손실 함수를 개발합니다. 연구진은 다양한 환경에서 깊이 추정의 정확도를 향상시키기 위해 대규모 인코더와 보조 작업에 대한 사전 훈련의 중요성을 강조합니다. 실험 결과, 이 방법은 제로 샷 교차 데이터 세트 전송을 통해 평가되었으며, 보이지 않는 데이터 세트에서 테스트하여 모델의 실제 성능을 더 정확하게 측정합니다. 이 연구는 단안 깊이 추정에 대한 새로운 표준을 제시하며, [https://github.com/intel-isl/MiDaS](https://github.com/intel-isl/MiDaS) 에서 무료로 이용할 수 있습니다.

[Boosting Monocular Depth Estimation Models to High-Resolution via Content-Adaptive Multi-Resolution Merging](3/Boosting%20Monocular%20Depth%20Estimation%20Models%20to%20High%20f8799b5dfe374f7583324a0ba79c1101)

심도 추정 모델의 한계를 해결하는 새로운 접근법을 제시합니다. 이 연구는 단일 이미지에서 고해상도 심도 맵을 생성하기 위해 다양한 해상도에서 추정치를 병합하는 방법을 개발했습니다. 저자들은 저해상도 추정이 일관된 구조를 제공하지만 디테일이 부족하고, 반대로 고해상도 추정은 디테일이 풍부하지만 구조적 일관성이 떨어진다는 것을 발견했습니다. 이에 따라, 이미지의 각 부분에 적합한 해상도를 선택하고, 이를 기본 추정치에 병합하여 고해상도의 상세한 심도 맵을 생성하는 방법을 제안합니다. 이 방법은 기존 심도 추정 모델의 성능을 상당히 향상시키며, 병합된 심도 맵은 높은 해상도와 경계 정확도를 제공합니다.

[IM2HEIGHT: Height Estimation from Single Monocular Imagery via Fully Residual Convolutional-Deconvolutional Network](3/IM2HEIGHT%20Height%20Estimation%20from%20Single%20Monocular%20%2086332c9e6cb2414ab8dec4954f1cfacf)

단일 단안 원격 감지 이미지에서 높이를 추정하는 문제에 대한 새로운 접근 방식을 제시합니다. 이 연구에서는 완전한 컨볼루션-디컨볼루션 네트워크 아키텍처를 사용하여 단안 이미지와 높이 맵 사이의 매핑을 모델링합니다. 네트워크는 두 부분으로 구성되어 있으며, 컨볼루션 하위 네트워크는 원격 감지 이미지를 다차원 특징으로 변환하고, 디컨볼루션 하위 네트워크는 이 특징을 바탕으로 높이 맵을 생성합니다. 연구는 베를린 지역의 고해상도 항공 이미지 데이터 세트를 사용하여 네트워크를 검증하고, 높이 맵 추정의 시각적 및 정량적 분석을 통해 이 방법의 효과를 입증합니다.

[Enforcing Temporal Consistency in Video Depth Estimation](3/Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17)

비디오에서 3D 정보를 복구하는 데 있어 깊이 추정의 시간적 일관성에 중점을 둡니다. 이 연구는 단일 이미지 깊이 방법을 기반으로 하되, 시간적 일관성을 강화함으로써 보다 안정적인 깊이 추정을 달성하는 새로운 접근법을 제시합니다. 저자들은 이 방법이 깊이 추정 품질을 저하시키지 않으면서 시간적 일관성을 개선한다는 것을 입증하며, 깊이 기준이 없는 실제 장면에도 이 방법을 적용할 수 있음을 보여줍니다. 이 연구는 단일 프레임 추론에서 비디오 깊이 추정의 시간적 일관성을 향상시키는 중요한 발전을 나타냅니다.

[ZoeDepth: Zero-shot Transfer by Combining Relative and Metric Depth](3/ZoeDepth%20Zero-shot%20Transfer%20by%20Combining%20Relative%20%20d443ea562ca54766889cfbf116a31b65)

단일 이미지 깊이 추정(SIDE) 분야에서 메트릭 깊이 추정(MDE)과 상대적 깊이 추정(RDE)을 결합한 새로운 방법론을 제안합니다. 이 논문은 MDE의 정확도와 RDE의 일반화 능력을 통합하여, 다양한 도메인의 이미지에 대해 효과적으로 깊이를 추정할 수 있는 모델을 개발합니다. 이 방법론은 기존의 단일 도메인에 국한된 깊이 추정 방식과 달리 다양한 환경에 적용 가능하며, 실내외 장면에 대해 높은 정확도를 보이는 동시에, 학습되지 않은 데이터셋에 대해서도 뛰어난 일반화 능력을 보여줍니다. 이는 깊이 추정 분야에서의 중요한 발전으로, 향후 실용적인 응용에 큰 영향을 미칠 수 있습니다.

[Vision Transformers for Dense Prediction](3/Vision%20Transformers%20for%20Dense%20Prediction%2010797514596c475fa59fbfdd5cb78e40)

고밀도 예측 트랜스포머(DPT)를 소개합니다. 이는 깊이 추정 및 의미 분할과 같은 고밀도 예측 작업에 적합한 새로운 아키텍처입니다. 전통적인 컨볼루션 기반 네트워크와 달리, DPT는 비전 트랜스포머(ViT)를 인코더의 기본 구성 요소로 사용하여 모든 단계에서 일정한 차원의 표현을 유지합니다. 이를 통해 보다 정확하고 전역적으로 일관된 예측이 가능해집니다. 실험 결과, DPT는 기존 최신 모델을 상당히 능가하며, 특히 대규모 데이터 세트에서 탁월한 성능을 보여줍니다. 이러한 결과는 DPT가 고밀도 예측 작업에 효과적인 접근 방식임을 시사합니다.

[MiDaS v3.1 – A Model Zoo for Robust Monocular Relative Depth Estimation](3/MiDaS%20v3%201%20%E2%80%93%20A%20Model%20Zoo%20for%20Robust%20Monocular%20Rela%203fedd3daf3974d96acb2d0d542959877)

단안 깊이 추정 기술에 대한 혁신적인 연구로, 단일 이미지에서 물체의 거리를 예측하는 방법을 제공합니다. 이 기술은 자율 주행, 3D 재구성 등에 응용될 수 있으며, MiDaS는 정확한 깊이 추정을 위해 학습 기반 접근 방식을 채택합니다. 이 최신 버전에서는 다양한 최첨단 백본과 새로운 깊이 추정 모델이 통합되어 있으며, 연구의 초점은 이 백본들을 MiDaS에 통합하는 방법과 다양한 모델을 비교 및 분석하는 데 있습니다. 이를 통해 더욱 정확하고 일반화된 깊이 추정 결과를 도출할 수 있으며, MiDaS는 이 분야에서 중요한 발전을 이루고 있습니다.

[Zero-Shot Metric Depth with a Field-of-View Conditioned Diffusion Model](3/Zero-Shot%20Metric%20Depth%20with%20a%20Field-of-View%20Condit%201150761a6e134402a570ec44c6b73633)

DMD(De-noising Diffusion for Metric Depth)라는 새로운 기술을 소개합니다. 이 기술은 단안 깊이 추정 문제에 대한 혁신적인 접근 방식으로, 특히 다양한 카메라 속성에 대한 일반화와 스케일 모호성 해결에 중점을 둡니다. DMD는 시야각 증강, FOV 컨디셔닝, 로그 도메인 깊이 표현, 노이즈 제거의 v-파라미터화 같은 기술을 통합하여 실내외 환경에서 뛰어난 메트릭 깊이 추정 성능을 제공합니다. 이 모델은 여러 벤치마크에서 기존 메트릭 깊이 모델인 ZoeDepth보다 우수한 성능을 보여, 이 분야에서의 새로운 표준을 제시합니다.

[Depth Anything: Unleashing the Power of Large-Scale Unlabeled Data](3/Depth%20Anything%20Unleashing%20the%20Power%20of%20Large-Scale%203bd6dbacc1644cae86d71a065b9a3e9c)

대규모의 레이블이 없는 데이터를 활용하여 단일 카메라 심도 추정(Monocular Depth Estimation, MDE) 분야에 새로운 접근 방식을 제시함으로써, 기존 방법들과 비교하여 뛰어난 일반화 능력과 견고성을 보여줍니다. 이는 레이블이 없는 이미지에서 자동으로 심도 레이블을 생성하고, 의미론적 정보를 통합하여 모델의 성능을 크게 향상시키는 두 가지 주요 전략에 기반합니다.

[DepthFM: Fast Monocular Depth Estimation with Flow Matching](3/DepthFM%20Fast%20Monocular%20Depth%20Estimation%20with%20Flow%20%2016ada3dba3264b37bffc52c4c58c1e58)

DepthFM은 단일 이미지로부터 세밀한 깊이 정보를 정확하게 추정할 수 있는 효율적인 흐름 매칭 기반 모델을 제안합니다. 이 모델은 전적으로 합성 데이터에 기반하여 훈련되었음에도 불구하고, 실제 이미지에 대해 뛰어난 일반화 능력을 보여줍니다.