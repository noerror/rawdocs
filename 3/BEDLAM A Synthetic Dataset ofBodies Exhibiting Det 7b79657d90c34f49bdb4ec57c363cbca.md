# BEDLAM: A Synthetic Dataset ofBodies Exhibiting Detailed Lifelike Animated Motion

[https://bedlam.is.tuebingen.mpg.de/media/upload/BEDLAM_CVPR2023.pdf](https://bedlam.is.tuebingen.mpg.de/media/upload/BEDLAM_CVPR2023.pdf)

[https://bedlam.is.tue.mpg.de/](https://bedlam.is.tue.mpg.de/)

![BEDLAM은 3D 인간 자세와 모양 추정(HPS) 작업에 대한 알고리즘을 훈련시키고 테스트하기 위해 설계된 대규모 합성 비디오 데이터 세트입니다. BEDLAM은 다양한 체형, 피부색, 그리고 동작들을 포함하고 있습니다. 이전 데이터 세트들을 넘어서, BEDLAM은 머리카락과 현실적인 옷을 가진 SMPL-X 바디를 포함하고 있으며, 물리 시뮬레이션을 이용해 애니메이션 처리되었습니다. BEDLAM의 현실감과 규모 덕분에, 우리는 실제 훈련 이미지를 사용하지 않고도 실제 이미지 데이터 세트에서 최첨단 HPS 정확도를 달성하기 위해 회귀 분석기를 훈련시키는 데 충분한 합성 데이터를 찾을 수 있었습니다.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled.png)

BEDLAM은 3D 인간 자세와 모양 추정(HPS) 작업에 대한 알고리즘을 훈련시키고 테스트하기 위해 설계된 대규모 합성 비디오 데이터 세트입니다. BEDLAM은 다양한 체형, 피부색, 그리고 동작들을 포함하고 있습니다. 이전 데이터 세트들을 넘어서, BEDLAM은 머리카락과 현실적인 옷을 가진 SMPL-X 바디를 포함하고 있으며, 물리 시뮬레이션을 이용해 애니메이션 처리되었습니다. BEDLAM의 현실감과 규모 덕분에, 우리는 실제 훈련 이미지를 사용하지 않고도 실제 이미지 데이터 세트에서 최첨단 HPS 정확도를 달성하기 위해 회귀 분석기를 훈련시키는 데 충분한 합성 데이터를 찾을 수 있었습니다.

### 1. Introduction

이 논문에서는 3D 인체 포즈 및 모양(HPS) 추정 방법의 개선점을 평가하기 위해 만들어진 새로운 사실적인 합성 데이터 세트인 BEDLAM(Bodies Exhibiting Detailed Lifelike Animated Motion)을 소개합니다. 이러한 방법은 이미지에서 HPS를 추정하는 신경망 모델인 HMR이 처음 등장한 이후 상당한 발전을 이루었습니다. 그러나 이러한 발전이 더 나은 아키텍처 때문인지 아니면 훈련 데이터의 개선 때문인지는 불분명했습니다.

베드람은 2D 관절 위치 대신 정확한 3D 신체가 포함된 데이터 세트를 제공함으로써 해결책을 제시합니다. 합성 데이터 세트에는 다양한 피부색, 체형, 연령 및 기타 요인이 포함되어 있어 HPS 방법을 더욱 포괄적으로 사용할 수 있습니다. 이 데이터 세트는 다양한 카메라, 장면, 센서에 맞게 조정할 수 있어 기존 합성 데이터 세트의 한계를 극복합니다.

BEDLAM 데이터 세트는 SMPL-X 형식의 해당 3D 신체 지상 실측 데이터와 함께 RGB 비디오로 구성됩니다. 다양한 신체 형태, 피부 질감, 의상 스타일, 애니메이션 모션이 포함되어 있어 가장 사실적이고 다양한 합성 데이터세트 중 하나입니다.

이 연구는 HMR과 같은 기본적인 HPS 추정 방법도 BEDLAM 데이터세트로 훈련하면 최첨단 성능을 달성할 수 있음을 보여줍니다. 연구진은 CLIFF와 같이 널리 사용되는 다른 모델의 성능도 평가한 결과 비슷한 결과를 발견했습니다.

이 논문은 합성 데이터가 HPS 추정 방법을 훈련하는 데 매우 효과적이며, 종종 실제 데이터를 사용한 훈련보다 성능이 뛰어나다는 결론을 내립니다. BEDLAM은 3D 의류 모델이나 동작 인식과 같은 다른 측면을 연구하는 데에도 사용할 수 있습니다. 이제 이 데이터 세트는 새로운 데이터 세트를 생성하는 데 필요한 리소스와 함께 연구 목적으로 사용할 수 있습니다.

### 2. Related work

이전 연구에서 확인된 한계를 해결하기 위해, 실제 세계의 가변성을 최대한 가깝게 에뮬레이션하는 것을 목표로 인간 포즈 및 형태(HPS) 회귀분석을 훈련할 수 있는 합성 데이터 세트를 생성하는 방법론을 제안합니다. 이 방법은 컴퓨터 그래픽과 머신 러닝의 발전을 통합하여 합성 이미지를 생성할 때 제어와 다양성을 모두 제공하는 파이프라인을 제공합니다.

![데이터 세트 구성. 각 단계의 과정을 설명하고 있으며, 한 명의 캐릭터에 대해 보여줍니다. 왼쪽에서 오른쪽으로: (a) 샘플링된 체형. (b) 피부 질감. (c) 옷 시뮬레이션. (d) 옷 텍스처. (e) 머리카락. (f) 자세. (g) 장면과 조명. (h) 모션 블러.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%201.png)

데이터 세트 구성. 각 단계의 과정을 설명하고 있으며, 한 명의 캐릭터에 대해 보여줍니다. 왼쪽에서 오른쪽으로: (a) 샘플링된 체형. (b) 피부 질감. (c) 옷 시뮬레이션. (d) 옷 텍스처. (e) 머리카락. (f) 자세. (g) 장면과 조명. (h) 모션 블러.

우리의 프레임워크는 크게 네 가지 핵심 요소로 구성됩니다:

![피부색 다양성. 50명의 남성과 50명의 여성 텍스처로부터 나온 예시 바디 텍스처들이 다양한 피부색을 보여줍니다.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%202.png)

피부색 다양성. 50명의 남성과 50명의 여성 텍스처로부터 나온 예시 바디 텍스처들이 다양한 피부색을 보여줍니다.

![옷과 텍스처의 다양성. 상단: BEDLAM의 실제 세계 복잡도를 가진 111가지 의상의 샘플. 하단: 각 의상은 여러 옷 텍스처를 가지고 있습니다. 총합: 1691가지.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%203.png)

옷과 텍스처의 다양성. 상단: BEDLAM의 실제 세계 복잡도를 가진 111가지 의상의 샘플. 하단: 각 의상은 여러 옷 텍스처를 가지고 있습니다. 총합: 1691가지.

신체 모델 및 포즈: SMPL[45]과 같은 파라메트릭 인체 모델을 활용하여 크고 다양한 신체 형태와 포즈를 생성합니다. 실제 세계의 가변성을 에뮬레이트하기 위해 다양한 인체 측정 데이터 세트에서 신체 파라미터를 가져와 다양한 인구 통계를 포괄할 수 있도록 합니다. 포즈 생성을 위해 우리는 데이터 기반 방법과 운동학 모델을 조합하여 활용하며, 다양성을 높이기 위해 무작위 섭동을 통합하기도 합니다.

의상 생성: 우리는 다양한 의상 스타일을 생성하기 위해 물리 기반 클로스 시뮬레이션과 데이터 기반 모델을 조합하여 사용합니다. 이 접근 방식을 통해 다양한 의상을 생성할 수 있을 뿐만 아니라 의상의 변형이 물리적으로 사실적이어서 강력한 HPS 회귀자를 훈련하는 데 매우 중요합니다.

씬 구성 및 렌더링: 우리는 사실적이고 다양한 씬을 제작하기 위해 두 가지 접근 방식을 채택합니다. 우리는 절차적으로 생성된 3D 환경과 사전 렌더링된 3D 환경을 혼합하여 배경으로 사용합니다. 생성된 의상으로 장식된 사람 모델을 이러한 환경 내에 배치하여 그럴듯한 상호 작용을 보장합니다. 그런 다음 패스 트레이싱과 같은 고급 기술을 사용하여 씬을 렌더링하여 시각적 사실감을 더욱 향상시킵니다.

지상 실사 생성: 우리는 각 합성 이미지에 대해 이미지 생성 프로세스에 사용된 파라미터를 기반으로 3D 인체 형상, 포즈, 2D 키포인트에 대한 고품질의 기준 데이터를 생성합니다. 또한 실루엣 및 심도 맵과 같은 추가 주석을 제공하여 다른 관련 작업에 유용하게 사용할 수 있습니다.

이 파이프라인에서 다양한 요소를 신중하게 제어하고 무작위화함으로써 HPS 회귀 모델의 훈련 데이터로 사용할 수 있는 매우 다양한 대규모 합성 데이터 세트를 생성할 수 있습니다. 이미지와 해당 실측 데이터는 합성이지만 실제와 유사한 가변성과 충실도를 보여줌으로써 이전 합성 데이터 세트의 한계를 극복할 수 있습니다.

사실적인 사람 이미지를 합성하는 데는 고유한 어려움이 있지만, 이 접근 방식은 실제 데이터 수집 방법에 비해 효과적으로 캡처할 수 있는 시나리오의 범위를 크게 확장한다는 점에 주목할 필요가 있습니다. 따라서 우리의 합성 데이터 생성 방법론을 통해 보다 일반적이고 강력한 HPS 회귀자를 훈련할 수 있으며, 3D 인체 포즈 및 형태 추정 분야에서 최첨단 기술을 한 단계 더 발전시킬 수 있습니다.

### 3. Dataset

이 글에서는 인체 자세 추정(HPS) 모델 훈련에 사용되는 풍부하고 다양한 합성 데이터 세트인 BEDLAM을 생성하는 과정을 설명합니다. 이 데이터 세트는 고품질의 사실적인 그래픽을 생성하는 것으로 유명한 인기 게임 엔진인 언리얼 엔진 5(UE5)를 사용하여 생성되었습니다.

![고-BMI 체형에 대한 옷 텍스처 맵. 왼쪽: 시뮬레이션된 옷의 예시. 오른쪽: BMI가 30, 40, 50인 체형에 맵핑된 옷 텍스처.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%204.png)

고-BMI 체형에 대한 옷 텍스처 맵. 왼쪽: 시뮬레이션된 옷의 예시. 오른쪽: BMI가 30, 40, 50인 체형에 맵핑된 옷 텍스처.

![모든 테스트 데이터 세트에서 BEDLAM-CLIFF 결과의 예시. 왼쪽에서 오른쪽으로: SSP-3D × 2, HBW × 3, RICH, 3DPW.](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%205.png)

모든 테스트 데이터 세트에서 BEDLAM-CLIFF 결과의 예시. 왼쪽에서 오른쪽으로: SSP-3D × 2, HBW × 3, RICH, 3DPW.

데이터 세트 생성 프로세스에는 여러 단계가 포함됩니다:

체형: 데이터셋에는 날씬한 체형부터 비만 체형까지 총 271가지의 다양한 체형이 포함되어 있으며, AGORA 및 CAESAR 데이터셋에서 가져온 것입니다. 체형은 SMPL-X 성 중립적 모양 공간을 사용하여 표현됩니다.

피부 톤의 다양성: 제작자는 피부 톤의 다양성을 보장하기 위해 메시카페이드의 상용 피부 알베도 텍스처 100개를 사용했습니다. 텍스처는 다양한 변형이 가능한 7가지 인종 그룹을 포함합니다.

3D 의류 및 텍스처: 의상의 다양성을 보장하기 위해 3D 의상 디자이너가 111개의 독특한 실제 의상을 제작했습니다. 각 의상마다 5~27개의 서로 다른 의상 텍스처가 디자인되어 총 1691개의 고유한 의상 텍스처가 만들어졌습니다.

헤어: 캐릭터 크리에이터 소프트웨어를 사용하여 27가지 헤어스타일을 생성했습니다.

사람의 모션: AMASS 데이터세트에서 샘플링한 모션으로, 자주 사용하는 모션에 편향되지 않도록 신중하게 선택했습니다. 데이터 세트에는 총 2311개의 고유 모션이 포함되어 있습니다.

장면 및 조명: 환경은 실내 및 실외 모두에서 95개의 HDRI 이미지 또는 8개의 3D 장면을 통해 표현되었습니다.

카메라: 사용된 카메라 모델은 일반적인 지상용 핸드헬드 카메라를 시뮬레이션합니다.

렌더링: 이미지 시퀀스는 시네마틱 카메라 모델을 사용하여 UE5 게임 엔진으로 렌더링했습니다.

뎁스 맵 및 세분화: RGB 프레임과 함께 뎁스 맵과 세분화 마스크도 생성했습니다.

BEDLAM 데이터 세트에는 총 38만 개의 RGB 프레임과 개별 인물이 포함된 100만 개의 고유 바운딩 박스가 훈련, 검증, 테스트 세트로 나뉘어 포함되어 있습니다. 그 크기와 다양성은 기존의 실제 및 합성 데이터 세트와 비교할 때 독보적입니다.

### 4. Experiments

이 섹션에서는 BEDLAM 프로젝트의 실험 설정에 대해 자세히 설명합니다. HMR과 CLIFF 훈련 모델은 합성 데이터(BEDLAM과 AGORA 데이터 세트 모두에서)에 적용되었으며, 이러한 구현을 BEDLAM-HMR 및 BEDLAM-CLIFF라고 부릅니다. 백본의 가중치에 대한 다양한 초기화가 테스트되었는데, 여기에는 처음부터 시작하거나 ImageNet을 사용하거나 COCO에서 훈련된 포즈 추정 네트워크를 사용하는 방법이 포함되었습니다. 모든 실측 신체는 훈련 감독을 위해 성 중립적인 형상 공간에 표현되었으며 성별 레이블은 사용되지 않았습니다. 훈련 과정에서 몇 가지 데이터 증강이 적용되었습니다.

또한, 실제 이미지 데이터에 대해서만 BEDLAM-CLIFF와 동일한 설정을 사용하여 훈련한 CLIFF(CLIFF†라고 함)의 재구현도 수행되었습니다. CLIFF† 훈련에는 Human3.6M, MPI-INF-3DHP, COCO, MPII와 같은 데이터 세트와 CLIFF 어노테이터에서 제공하는 의사 GT가 사용되었습니다. 실제 이미지로 훈련하고 3DPW 훈련 데이터로 미세 조정한 결과, 이전 연구에서 보고된 정확도와 일치했습니다.

손 네트워크 훈련의 경우, 전신 네트워크인 BEDLAM-CLIFF-X를 훈련하여 몸과 손의 포즈를 회귀하도록 했습니다. 지상 실측 손 키포인트를 사용하여 BEDLAM 훈련 이미지에서 손 자르기 데이터 세트를 생성했습니다. 미디어파이프를 사용하여 크롭에서 손을 감지하고, 높은 신뢰도로 손이 감지된 크롭만 훈련에 사용했습니다.

훈련에는 BEDLAM의 약 75만 개의 작물과 AGORA의 8만 5천 개의 작물이 활용되었습니다. 또한 3DPW 훈련 데이터에서 모델을 미세 조정했습니다. 평가는 다양한 카메라 각도에 대해 3DPW 및 리치 데이터 세트에서 수행되었습니다. 체형 평가에는 SSP-3D 및 HBW 데이터 세트가 사용되었습니다.

신체 포즈와 형태 정확도를 평가하기 위해 표준 메트릭을 사용했습니다. 이러한 메트릭에는 버텍스와 관절 위치의 평균 오차를 측정하는 PVE, MPJPE, PA-MPJPE, PVE-T-SC, P2P20k가 포함됩니다. 모든 오차는 mm 단위로 보고됩니다. 버텍스 매핑 절차를 사용하여 SMPL-X와 SMPL 형식 간의 지상 실측 데이터 변환을 수행했습니다.

BEDLAM에 대한 사전 학습과 3DPW 및 BEDLAM 학습 데이터를 혼합하여 미세 조정한 결과, 3DPW와 RICH에서 가장 정확한 결과를 얻을 수 있었습니다. 수정된 CLIFF 모델(BEDLAM-CLIFF*)은 CLIFF†* 또는 이전 연구에서 보고된 결과보다 더 나은 성능을 보였습니다.

![Untitled](BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca/Untitled%206.png)

동일한 데이터로 훈련된 수정된 HMR 모델(BEDLAM-HMR*)은 3DPW에서 거의 비슷한 성능을 보였으며, RICH에서는 CLIFF†*보다 더 나은 성능을 보였습니다. 이는 고품질 데이터로 학습할 경우 간단한 방법이라도 좋은 결과를 얻을 수 있음을 시사합니다.

3DPW 미세 조정이 없는 BEDLAM-CLIFF는 미세 조정된 버전과 거의 비슷한 성능을 보였으며, 3DPW 미세 조정이 있든 없든 CLIFF보다 RICH에 더 잘 일반화되었습니다.

합성 데이터로만 학습한 CLIFF와 HMR 모두 이 분야의 최신 방법보다 우수한 성능을 보였으며, 이는 고품질 데이터를 확보하기 위해 더 많은 노력을 기울여야 함을 시사합니다.

표 2는 베드램-클리프가 옷을 입은 상태에서 체형을 추정할 수 있음을 보여줍니다. 다른 방법들은 특정 데이터 세트에서는 최고의 성능을 보였지만 다른 데이터 세트에서는 성능이 떨어졌습니다. 반면, BEDLAM-CLIFF는 다양한 데이터 세트에서 일관되게 2위를 차지하여 강력한 일반화 능력을 보여주었습니다.

절제 연구 결과는 다양한 데이터 세트, 백본 가중치, 훈련에 사용된 데이터의 양이 미치는 영향을 보여주었습니다. 이 연구에서 주목할 만한 몇 가지 결과는 이미지 데이터에 대해 백본을 사전 훈련하는 것이 표준 관행이지만, BEDLAM에서 백본을 처음부터 훈련하면 결과가 더 나빠진다는 것입니다. COCO와 같은 2D 포즈 추정 작업으로 백본을 훈련하는 것이 중요한 것으로 밝혀졌습니다. 또한, 5%에 불과한 소량의 BEDLAM 크롭을 사용해도 3DPW에서 적절한 성능을 얻을 수 있는 것으로 나타나 BEDLAM 데이터 세트의 다양성을 알 수 있었습니다. 또한 훈련에 사실적인 의상 시뮬레이션을 통합하면 텍스처 바디를 사용한 훈련보다 훨씬 더 나은 결과를 얻을 수 있었습니다.

### 5. Limitations and Future Work

오픈소스 자산: 라이선스 제약으로 인해 고품질의 상업용 자산을 사용할 수 없다는 점이 연구 진행에 큰 걸림돌로 작용했습니다. 이 분야에서는 더 많은 오픈 소스 에셋이 필요하다는 것이 분명합니다.

모션과 장면: 이 연구에서는 AMASS에서 무작위로 샘플링한 사람 모션을 사용했습니다. 실제 생활에서 의상과 동작, 장면과 동작은 서로 연관되어 있습니다. 또한, 사람들은 서로 및 사물과 상호 작용하기 때문에 이상적으로는 사실적으로 합성되어야 합니다. 현재 데이터 세트에는 앉거나 누워 있는 포즈, 복잡한 스포츠 포즈와 같은 포즈도 부족합니다.

헤어: 현재 데이터세트에는 사실적인 헤어 피직스, 긴 헤어스타일, 다양한 헤어 컬러가 부족합니다. 기존 솔루션은 특정 조명 조건에서 아티팩트가 발생합니다. 스트랜드 기반 헤어 솔루션은 보다 사실적인 결과를 제공할 수 있습니다.

체형의 다양성: 현재 연구에서 체형 분포가 균일하지 않았습니다. 향후 연구에서는 보다 균형 잡힌 분포를 사용하고 어린이, 척추측만증 환자, 절단 환자 등 더 다양한 체형을 포함해야 합니다. 체질량 지수가 높은 피험자의 옷감 시뮬레이션 및 모션 리타겟팅과 관련된 문제가 발견되었습니다.

바디 텍스처: 사용된 피부 텍스처는 다양했지만 디테일과 사실적인 리플렉션 속성이 부족했습니다. 적절한 라이선스를 보유한 고품질 텍스처를 찾기가 어려웠습니다.

신발: BEDLAM 데이터 세트의 시체는 맨발이었습니다. 기본적인 신발을 추가하는 것은 매우 간단하지만 하이힐로 인한 신체 자세 및 걸음걸이 변화와 같은 보다 복잡한 문제를 해결하려면 보다 고급 솔루션이 필요합니다.

손과 얼굴: 전신과 손의 움직임을 포함하는 모션캡처 데이터는 제한적이며, 물체와의 상호작용을 포함하는 모션캡처 데이터는 더 적습니다. 얼굴 움직임은 이 연구에 포함되지 않았습니다. 전신, 손, 얼굴 동작을 평가하는 데이터 세트가 생성되면 도움이 될 것입니다.

### 6. Discussion and Conclusions

결론 섹션에서 저자들은 처음에 제기했던 질문, "합성 데이터만으로 충분할까요?"에 대한 답을 제시합니다. 저자들은 SSP-3D, HBW, 3DPW, RICH와 같은 데이터 세트에서 입증된 바와 같이 BEDLAM 데이터 세트가 충분히 현실적이기 때문에 이를 통해 훈련된 메서드를 다양한 실제 장면에 일반화할 수 있다고 제안합니다. BEDLAM이 특정 실제 이미지 영역을 정확하게 표현하지 못하는 경우 카메라 뷰, 이미징 모델, 모션 등을 변경하여 조정할 수 있습니다.

또한 저자들은 더 정교한 방법과 비교한 BEDLAM-HMR의 성능을 통해 아키텍처가 일반적으로 생각하는 것보다 덜 중요할 수 있음을 시사하며 아키텍처의 중요성에 의문을 제기합니다. 그러나 한 가지 주의할 점은 HPS 정확도는 백본의 사전 학습에 따라 달라지는 것 같습니다. 실제 이미지 가변성에 많이 노출되는 COCO에서 2D 포즈 추정을 위해 백본을 사전 학습하면 일반화에 도움이 되는 것으로 보입니다.

합성 데이터의 사실성은 시간이 지남에 따라 개선되지만, 저자들은 결국 사전 훈련이 불필요해질 것이라고 생각합니다. 또한 저자들은 3D 의상 모델링, 암시적 형상 방법을 사용한 3D 아바타 학습, 세계 좌표에서 사람 추정 등의 추가 연구에 BEDLAM을 사용할 수 있다고 제안합니다. 이 데이터 세트는 시간적 정보나 동작 의미를 활용하는 방법도 지원할 수 있습니다.

저자들은 3D 의상을 제작한 스튜디오 루파스(STUDIO LUPAS GbR), 피부 텍스처를 제공한 메시카페이드(Meshcapade GmbH), 프로젝트의 여러 측면에 도움을 준 여러 개인 등 다양한 개인과 조직의 공헌에 감사를 표합니다.