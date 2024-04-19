# Motion

[BlazePose: On-device Real-time Body Pose tracking](3/BlazePose%20On-device%20Real-time%20Body%20Pose%20tracking%20b1e82940fa68452fa7bd7bdb4ec5d176)

이미지와 동영상에서 인체 포즈를 실시간으로 추정하는 새로운 모델 'BlazePose'를 제시합니다. 이 모델은 주로 건강 추적, 수화 인식, 제스처 제어와 같은 애플리케이션을 위해 개발되었습니다. BlazePose는 가벼운 신체 포즈 감지기를 사용하여 먼저 얼굴과 같은 신체의 단단한 부위를 감지한 다음 33개의 키포인트 위치를 예측합니다. 이 모델은 휴대폰에서 거의 실시간으로 작동할 수 있도록 설계되었으며, 기존 데이터 세트 및 네트워크와 호환됩니다. 성능 면에서는 일반적인 사용 사례에서 OpenPose보다 약간 떨어지지만, 요가/피트니스 추적 시나리오에서는 우수하며 속도가 훨씬 빠릅니다. 이는 모바일 장치에서 실시간으로 포즈 추정을 수행하는 데 적합한 솔루션입니다.

[OpenPose: Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields](3/OpenPose%20Realtime%20Multi-Person%202D%20Pose%20Estimation%20%20dba9ea5dddb64bbe9dfac1554725d62d)

OpenPose라는 실시간 다중 인원 2D 포즈 추정 시스템을 소개합니다. OpenPose는 사람의 팔다리 위치와 방향을 모두 인코딩하는 비파라메트릭 표현을 활용합니다. 이 시스템은 신체 부위 감지와 연관성을 동시에 학습하고, 고품질의 신체 포즈 파싱을 효율적으로 생성하기 위해 욕심 많은 파싱 알고리즘을 사용합니다. 저자들은 또한 파트 선호도 필드(PAF)를 개선함으로써 런타임 성능과 정확도가 크게 향상된다는 점을 강조합니다. OpenPose는 오픈소스로 공개되었으며, 다양한 연구 분야에서 널리 사용되고 있습니다.

[DensePose: Dense Human Pose Estimation In The Wild](3/DensePose%20Dense%20Human%20Pose%20Estimation%20In%20The%20Wild%209bc47a7138814f909e6a6e046b07aff8)

고밀도 인체 포즈 추정을 위한 새로운 접근 방식을 제시합니다. 이들은 COCO-DensePose라는 대규모 데이터 세트를 생성하고, 이를 활용하여 2D 이미지의 모든 픽셀을 인체 3D 표면에 정확하게 매핑하는 모델을 개발했습니다. 이 방법은 다양한 환경에서 인체 포즈를 정확하게 추정하는 데 성공하며, 이전 방법보다 뛰어난 성능을 보입니다. 연구는 증강 현실, 컴퓨터 그래픽스, 그리고 이미지를 3D 객체와 연결하는 분야에서의 응용 가능성을 탐색합니다. 이 모델은 특히 복잡한 환경에서도 인체의 포즈를 정확하고 신속하게 추정할 수 있음을 보여주며, 고밀도 포즈 추정의 정확도와 견고성을 크게 향상시키는 것으로 나타났습니다.

[Comparing the Quality of Human Pose Estimation with BlazePose or OpenPose](3/Comparing%20the%20Quality%20of%20Human%20Pose%20Estimation%20wit%20dfe903d4931643269046f986477a8ad3)

BlazePose와 OpenPose라는 두 가지 인공 지능 기반 인간 포즈 추정 기술을 비교합니다. BlazePose는 모바일 최적화 모델이고, OpenPose는 널리 인정받는 기술입니다. 연구진은 두 기술로 신체 위치를 추정하고 그 결과를 비교했습니다. BlazePose는 특히 무릎과 팔꿈치 같은 관절에서 OpenPose와 다르게 위치를 추정하는 경향이 있었으나, 그 속도와 모바일 호환성 때문에 환자의 움직임을 빠르게 확인하고 활동을 파악하는 데 유용할 수 있습니다. 그러나 정확한 신체 위치 추정이 필요한 경우 OpenPose 사용이 권장됩니다.

[Comparative Analysis of OpenPose, PoseNet, and MoveNet Models for Pose Estimation in Mobile Devices](3/Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca)

모바일 장치에서 사용되는 포즈 추정 모델인 OpenPose, PoseNet, MoveNet을 비교 분석합니다. OpenPose는 상향식 접근 방식을 사용해 여러 인원의 포즈를 추정하지만 처리 속도가 느립니다. PoseNet은 하향식 방식을 채택하여 한 명 또는 여러 명의 포즈를 안정적으로 추정하며, MoveNet은 두 가지 버전으로 빠른 속도와 높은 정확도를 제공합니다. 연구 결과는 각 모델의 장단점을 밝혀내며, 특정 애플리케이션의 요구 사항에 따라 적합한 모델을 선택하는 데 도움을 줍니다. 이 연구는 모바일 디바이스에서의 포즈 추정에 대한 포괄적인 평가를 제공하며, 이 분야의 향후 연구에 중요한 기여를 할 것으로 기대됩니다.

[A comparative analysis of pose estimation models as enablers for a smart-mirror physical rehabilitation system](3/A%20comparative%20analysis%20of%20pose%20estimation%20models%20a%209251e926c5af471e91f929a539974563)

SHAPES 프로젝트의 일환으로, 고령자를 위한 스마트 거울 기반의 신체 재활 시스템 개발을 목표로 합니다. 스마트 거울은 사용자의 자세를 모니터링하고 재활 루틴을 지원하기 위해 내장된 컴퓨터와 디스플레이를 갖춘 거울입니다. 이 연구에서는 다양한 비전 기반 신체 자세 추정 모델(MoveNet, BlazePose, PoseNet)의 성능을 비교 분석했습니다. 평가는 정확도, 속도, 전반적인 성능을 고려하여 수행되었으며, 이러한 모델들은 스마트 미러 장치와의 호환성을 위해 테스트되었습니다. 연구 결과, PoseNet이 가장 높은 정확도를 보였으나, MoveNet이 속도와 정확도의 균형이 가장 적절하다고 결론지어 스마트 미러 시스템에 가장 적합한 모델로 선택되었습니다. BlazePose는 3D 포즈 추정 기능을 제공하지만 정확도가 제한적이어서 향후 발전이 필요한 것으로 나타났습니다. 이 연구는 신체 재활을 위한 스마트 미러 시스템의 효율성을 높이고 향후 발전 가능성을 탐색합니다.

[Physics-based Motion Retargeting from Sparse Inputs](3/Physics-based%20Motion%20Retargeting%20from%20Sparse%20Input%2095ee423a1cc04e35a7c7dd77b359e80a)

가상현실(VR)과 증강현실(AR) 환경에서 사용자의 모션을 다양한 캐릭터에 실시간으로 리타기팅하는 새로운 방법을 제시합니다. 이 방법은 강화학습을 활용하여 사용자의 헤드셋과 컨트롤러 데이터만으로 다양한 캐릭터의 물리적으로 타당한 포즈를 생성합니다. 이 시스템은 사용자의 제한된 센서 데이터를 바탕으로 실시간으로 인간이 아닌 캐릭터의 고품질 모션을 생성할 수 있으며, 이를 위해 대규모 모션 캡처 데이터 세트를 활용합니다. 특히, 이 연구는 캐릭터의 물리적 속성과 비례를 고려하여, 예를 들어 공룡의 무거운 꼬리나 쥐의 짧은 다리 등을 모방합니다. 전반적으로 이 시스템은 VR과 AR에서 사용자의 동작을 다양한 캐릭터에 실시간으로 효과적으로 리타기팅하는 새로운 방법을 제공합니다.

[Real-time Monocular Full-body Capture in World Space via Sequential Proxy-to-Motion Learning](3/Real-time%20Monocular%20Full-body%20Capture%20in%20World%20Spa%207f9acde95b544a8bbd12a7eaae86695c)

실시간 단안 전신 캡처 시스템을 소개합니다, 이는 세계 공간에서 발-땅 접촉을 포함한 정확한 인간의 움직임을 생성할 수 있습니다. 이 방법은 학습 데이터에 의존하는 기존의 3D 신체 모션 캡처 방식과 다르게, 대규모 모션 시퀀스 데이터세트를 사용하여 순차적인 프록시-모션 쌍을 합성하는 새로운 접근 방식을 채택합니다. 이 연구는 2D 스켈레톤 시퀀스를 프록시 표현으로 사용하여 월드 스페이스의 3D 회전 모션 시퀀스와 연결하고, 이를 통해 깊이 모호성을 처리하고 일반화를 개선합니다. 네트워크 아키텍처는 프록시 시퀀스를 입력으로 받아 월드 스페이스에서 3D 동작을 생성하며, 이 과정에는 반복적인 동작 개선을 위한 접촉 인식 신경 모션 하강 모듈이 포함됩니다. 결과적으로, 이 시스템은 월드 스페이스에서 발과 지면의 접촉이 그럴듯한 실시간 단안 전신 캡처를 가능하게 합니다.

[BEDLAM: A Synthetic Dataset ofBodies Exhibiting Detailed Lifelike Animated Motion](3/BEDLAM%20A%20Synthetic%20Dataset%20ofBodies%20Exhibiting%20Det%207b79657d90c34f49bdb4ec57c363cbca)

BEDLAM은 3D 인간 자세와 모양 추정(HPS) 알고리즘을 훈련 및 테스트하기 위한 대규모 합성 비디오 데이터 세트입니다. 이 데이터 세트는 다양한 체형, 피부색, 동작을 포함하며, 머리카락과 현실적인 옷을 입은 SMPL-X 모델로 구성되어 있습니다. 이를 통해 실제 훈련 이미지 없이도 HPS의 최첨단 정확도를 달성할 수 있었습니다. BEDLAM은 다양한 신체 형태, 피부 질감, 의상 스타일, 애니메이션을 포함하여 가장 사실적이고 다양한 합성 데이터세트 중 하나입니다. 이 연구는 기본적인 HPS 추정 방법도 BEDLAM 데이터세트로 훈련하면 최첨단 성능을 달성할 수 있음을 보여줍니다. 이는 합성 데이터가 HPS 추정 방법을 훈련하는 데 매우 효과적임을 의미하며, 종종 실제 데이터를 사용한 훈련보다 성능이 뛰어나다는 것을 시사합니다. BEDLAM은 또한 3D 의류 모델링이나 동작 인식 같은 다른 연구 분야에도 활용될 수 있습니다.

[Humans in 4D:Reconstructing and Tracking Humans with Transformers](3/Humans%20in%204D%20Reconstructing%20and%20Tracking%20Humans%20wi%2000cb19ea5d0148c29dd637b3d9b04e48)

HMR 2.0, 단일 이미지에서 3D 인간 자세와 형상을 재구성하는 트랜스포머 기반 접근법을 소개합니다. 이 기술은 다양한 자세와 시점에서 뛰어난 성능을 제공하며, 4DHumans 시스템의 핵심으로, 비디오에서 시간에 따른 인간의 공동 재구성과 추적을 가능하게 합니다. HMR 2.0은 CNN이나 LSTM에서 트랜스포머 아키텍처로 변환하는 '트랜스포머화' 개념을 채택하여, 비디오 내에서 인물을 동시에 재구성하고 추적할 수 있도록 합니다. 이 시스템은 포즈트랙 데이터셋에서 추적 부문의 최첨단 결과를 달성하고, AVA v2.2 데이터셋에서 동작 인식을 크게 향상시킨 것으로 나타났습니다. HMR 2.0은 컴퓨터 비전, 로봇 공학, 컴퓨터 그래픽, 생체 역학 등 다양한 분야에서 응용될 수 있으며, 특히 인간의 형태와 움직임을 분석하는 데 유용할 것으로 기대됩니다.

[Expressive Body Capture: 3D Hands, Face, and Body from a Single Image](3/Expressive%20Body%20Capture%203D%20Hands,%20Face,%20and%20Body%20f%208612eca963db4c9cae7532c3eb8ab346)

단일 이미지로부터 전체 인체의 3D 핸드, 페이스, 및 바디 모델을 포착하는 새로운 방법인 SMPL-X와 SMPLify-X를 소개합니다. SMPL-X는 기존 모델들이 놓친 신체, 손, 얼굴의 상세한 표현을 잡아낼 수 있도록 설계되었으며, 이를 위해 대량의 3D 스캔 데이터를 학습했습니다. SMPLify-X는 이 모델을 단일 RGB 이미지에 적용하는 방법으로, OpenPose를 사용하여 이미지에서 2D 특징을 추정한 다음 이를 기반으로 3D 모델을 맞춥니다. 연구진은 새로운 평가 데이터 세트를 통해 이 방법의 유효성을 검증했으며, 결과는 SMPL-X와 SMPLify-X가 단일 이미지에서 인체의 포괄적이고 표현력 있는 3D 모델을 생성하는 데 중요한 발전을 나타냅니다.

[ViTPose: Simple Vision Transformer Baselines for Human Pose Estimation](3/ViTPose%20Simple%20Vision%20Transformer%20Baselines%20for%20Hu%2062e93bb6eb8048b2a3b56556a1f569d3)

사람의 포즈를 추정하는 비전 트랜스포머 기반 모델인 'ViTPose'를 소개합니다. ViTPose는 비계층적인 트랜스포머 백본과 간단한 디코더를 사용하여 이미지에서 인체의 키포인트를 식별합니다. 본 모델의 핵심 강점은 단순함, 확장성, 유연성, 그리고 전송성에 있습니다. ViTPose는 복잡한 구조를 사용하지 않으면서도 MS COCO 키포인트 데이터세트에서 뛰어난 성능을 보여줍니다. 모델의 단순성은 다양한 트랜스포머 레이어와 특징 차원을 조정함으로써 다양한 요구에 맞출 수 있음을 의미합니다. 또한, ViTPose는 다양한 입력 및 특징 해상도에서 효과적이며, 필요한 경우 다양한 포즈 데이터 세트에 적응할 수 있습니다. 실험 결과는 ViTPose가 여러 데이터 세트에서 우수한 성능을 보임을 입증하며, 더 큰 ViTPose 모델로부터 학습을 통해 성능을 향상시킬 수 있음을 보여줍니다. 연구자들은 ViTPose가 향후 비전 트랜스포머 기반 포즈 추정의 발전에 중요한 기준이 될 것으로 기대합니다.

[ProHMR - Probabilistic Modeling for Human Mesh Recovery](3/ProHMR%20-%20Probabilistic%20Modeling%20for%20Human%20Mesh%20Rec%202fd515001d7048f1b356c117fde3df60)

2D 이미지에서 3D 인간 형상 복구를 위한 새로운 확률론적 접근 방법을 제시합니다. 이 연구는 기존의 결정론적 모델과 달리, 입력 이미지에 기반하여 다양한 3D 포즈의 분포를 예측하는 ProHMR 모델을 개발합니다. 이 모델은 정규화 흐름을 활용하여 가능한 3D 포즈 분포를 생성하고, 각 샘플의 가능성을 계산합니다. ProHMR은 여러 데이터 세트에서 강력한 성능을 보이며, 특히 불완전하거나 신뢰도가 낮은 키포인트에도 현실적인 3D 형상 재구성을 가능하게 합니다. 이 연구는 3D 인간 형상 복구 분야에 중요한 기여를 하며, 다양한 애플리케이션에 응용될 잠재력을 가지고 있습니다.

[MobileHumanPose: Toward Real-Time 3D Human Pose Estimation in Mobile Devices](3/MobileHumanPose%20Toward%20Real-Time%203D%20Human%20Pose%20Est%20e1d7ca7e56874757baa916fd40d71c9a)

실시간 3D 인체 포즈 추정을 가능하게 하는 새로운 방법론 'MobileHumanPose'를 제시합니다. 기존 딥러닝 기반 3D 인체 자세 추정 모델들은 고성능을 제공하지만, 높은 연산 요구로 인해 모바일 기기에서의 사용이 제한적이었습니다. 이 연구는 이러한 문제를 해결하기 위해 MobileNetV2를 백본으로 하는 경량화된 모델을 사용합니다. 모델은 채널 수 조정, 파라메트릭 ReLU 활성화 함수 사용, 스킵 연결 추가 등의 수정을 거쳐 계산 효율성을 높이고 성능을 최적화합니다. 실험 결과는 이 모델이 작은 크기에도 불구하고 인상적인 정확도와 속도를 제공함을 보여줍니다. 저자들은 이 모델을 통해 모바일 애플리케이션에서 다양한 3D 가상 효과를 구현할 수 있음을 시사하며, 이 연구가 모바일 기기에서의 3D 인체 자세 추정 발전에 기여할 것으로 기대합니다.

[STCFormer: 3D Human Pose Estimation with Spatio-Temporal Criss-cross Attention](3/STCFormer%203D%20Human%20Pose%20Estimation%20with%20Spatio-Tem%20e6b8c3e46987464db5826f11727bfe79)

비디오에서 3D 인체 포즈를 추정하기 위한 새로운 접근 방식인 시공간 교차 트랜스포머(STCFormer)를 제시합니다. 이 모델은 공간적 및 시간적 상호작용을 병렬로 처리하는 새로운 메커니즘을 사용하여 3D 인체 포즈를 추정합니다. 이는 공간적 및 시간적 상관관계를 효과적으로 포착하고 계산 비용을 줄이는 데 중점을 둡니다. STCFormer는 인체의 동적 연쇄 구조를 활용하여 새로운 위치 임베딩 기능을 제공합니다. 이 기능은 관절의 국소 구조를 강조하여 보다 정확한 포즈 추정을 가능하게 합니다. 실험 결과, STCFormer는 표준 데이터 세트에서 최첨단 방법보다 우수한 성능을 보여주었으며, 이는 3D 인체 포즈 추정을 위한 강력하고 혁신적인 방법임을 입증합니다.

[RTMPose: Real-Time Multi-Person Pose Estimation based on MMPose](3/RTMPose%20Real-Time%20Multi-Person%20Pose%20Estimation%20bas%202285612a57664e31b82c29e18bca28e2)

RTMPose라는 실시간 다중 인물 포즈 추정 프레임워크를 소개합니다. RTMPose는 효율적인 모델 설계와 최적화 전략을 통해 모델 성능과 복잡성 사이의 균형을 이루는데 초점을 맞추었습니다. 이 프레임워크는 2D 포즈 추정을 두 개의 분류 작업으로 처리하고, 다양한 학습 전략과 최적화 기법을 적용하여 성능을 높였습니다. RTMPose는 다양한 장치(CPU, GPU, 모바일 장치)에서 실시간으로 작동할 수 있으며, COCO, AP-10K, CrowdPose, MPII 등의 여러 데이터셋에서 평가되었습니다. 이 프레임워크는 모델 성능과 복잡성 사이의 우수한 균형을 달성함으로써 산업 분야에서의 포즈 추정 요구를 충족하고 향후 인간 포즈 추정 분야의 연구에 기여할 수 있습니다.

[GHUM: Implicit Generative Models of 3D Human Shape and Articulated Pose](3/GHUM%20Implicit%20Generative%20Models%20of%203D%20Human%20Shape%20%20257c9f5a7ad740c99a468568d12272b0)

GHUM과 GHUML이라는 새로운 3D 인간 형태 및 포즈 모델을 개발하고 있습니다. 이 모델들은 더 정확하고 상세한 인체 모델링을 위해 고해상도 데이터를 활용하는 엔드-투-엔드 학습 파이프라인에 기반을 두고 있습니다. 이러한 접근 방식은 전체 신체를 포괄적으로 모델링하는데 초점을 맞추고 있으며, 이를 통해 얼굴과 손의 디테일과 같은 세밀한 부분까지 재현할 수 있습니다. GHUM과 GHUML 모델은 다양한 신체 형태와 포즈를 정확하게 표현할 수 있으며, 특히 GHUM은 더 높은 디테일을 제공합니다. 이 모델들은 의류 가상 착용, 피트니스, AR/VR, 특수 효과, 게임 개발 등 다양한 분야에 활용될 잠재력을 가지고 있습니다. 연구팀은 이 모델들을 추가 연구와 발전을 위해 공개할 계획임을 밝혔습니다

[CMX: Cross-Modal Fusion for RGB-X Semantic Segmentation with Transformers](3/CMX%20Cross-Modal%20Fusion%20for%20RGB-X%20Semantic%20Segmenta%20bf20235c149246c3b9e44f44f2ea6f58)

자율주행차량의 환경 인식을 개선하기 위해 CMX라는 새로운 크로스 모달 융합 모델을 제안합니다. CMX는 RGB 이미지와 추가 센서 데이터(깊이, 열, 편광, 이벤트, LiDAR)를 통합하여 복잡한 환경에서 물체를 더 정확하게 구별할 수 있도록 설계되었습니다. 이 모델은 크로스 모달 특징 정류 모듈(CM-FRM)과 기능 융합 모듈(FFM)을 사용하여 다양한 센서 데이터 간의 상호 작용을 강화하고, 이를 통해 보다 정확한 의미 예측을 가능하게 합니다. 다양한 조건과 데이터 조합에서의 테스트를 통해 CMX는 현재 모델보다 우수한 성능을 보였으며, 이는 자율주행차량의 환경 인식과 시맨틱 분할 능력을 향상시키는 데 기여할 것으로 기대됩니다.

[PersonLab: Person Pose Estimation and Instance Segmentation with a Bottom-Up, Part-Based, Geometric Embedding Model](3/PersonLab%20Person%20Pose%20Estimation%20and%20Instance%20Segm%20c7e9e314100a43d78b10ea22716af985)

다중 인물 감지, 2D 포즈 추정, 인스턴스 분할을 처리하는 상향식 방법을 제안합니다. 이 시스템은 주어진 이미지에서 모든 사람의 위치를 식별하고, 주요 특징을 매핑하며, 각 인물에 대한 분할 마스크를 생성합니다. 연구는 키포인트 감지와 키포인트를 사람 인스턴스로 그룹화하는 두 단계로 구성되며, COCO 키포인트 데이터셋을 기반으로 학습되었습니다. 이 방법은 인스턴스 수준의 분할에 키포인트 주석을 활용하며, 더 높은 키포인트 평균 정밀도를 달성합니다. 결과적으로, 이 시스템은 다양한 실제 환경에서 사람을 효과적으로 감지하고 포즈를 추정할 수 있는 능력을 입증하였습니다.

[TEDi: Temporally-Entangled Diffusion for Long-Term Motion Synthesis](3/TEDi%20Temporally-Entangled%20Diffusion%20for%20Long-Term%20%2019f29fc8bd8e482a9197e1122961e3f4)

새로운 모션 합성 방법을 소개합니다. TEDi는 동작 시퀀스의 시간 축을 확산 시간 축과 결합하여 긴 시퀀스의 동작을 생성하는 기술입니다. 이 방법은 U-Net 아키텍처를 활용한 자동회귀 방식을 통해 동작을 생성하며, '정지 모션 버퍼'라는 핵심 요소를 사용하여 지속적으로 새로운 프레임을 생성합니다. 이는 확산 시간 축을 따라 진행하면서 실제로 확산 시간을 증가시키지 않는 과정입니다. TEDi는 가이드 생성, 경로 제어 등의 기능을 제공하며 다른 모델과 비교했을 때 우수한 성능을 보여줍니다. 이 연구는 오디오, 비디오, 이미지 패치 순서 등 다양한 순차 데이터에 대한 확산 모델의 적용 가능성을 시사하며, 향후 더 빠른 노이즈 제거 방법을 탐색할 계획입니다.

[DeepCut: Joint Subset Partition and Labeling for Multi Person Pose Estimation](3/DeepCut%20Joint%20Subset%20Partition%20and%20Labeling%20for%20Mu%20741871d2f317455a91dbfce7ecc41653)

이미지 내 여러 사람의 포즈를 동시에 추정하는 새로운 방법, 'DeepCut'을 소개합니다. 이 방식은 정수 선형 프로그래밍을 이용해 개별 신체 부위를 식별하고 분류하며, 컨볼루션 신경망(CNN) 변형을 사용하여 신체 부위 후보를 생성합니다. DeepCut은 기존 방법들이 한계를 보였던 여러 사람이 가까이 있거나 부분적으로 가려진 복잡한 시나리오에서 각 개인의 포즈를 효과적으로 추정할 수 있는 능력을 가지고 있으며, 이는 다양한 데이터 세트에서의 실험을 통해 입증되었습니다. 이 연구는 특히 다인 포즈 추정 분야에서 혁신적인 접근법을 제시하며, 기존 방법보다 우수한 성능을 보여줍니다.

[Generating Fine-Grained Human Motions Using ChatGPT-Refined Descriptions](3/Generating%20Fine-Grained%20Human%20Motions%20Using%20ChatGP%20e60962cbca4e43c3960c42b26e1ccad4)

인공지능을 활용해 사실적인 인간 모션을 생성하는 새로운 방법을 제시합니다. 이 연구에서는 ChatGPT를 사용하여 텍스트 설명을 세분화된 모션 지침으로 변환하고, 이를 바탕으로 트랜스포머 기반 확산 모델을 통해 세밀하고 스타일화된 인간 동작을 생성합니다. CLIP을 통해 모션 정보를 글로벌 및 부분 토큰으로 인코딩하는 이 방식은 다양한 평가 지표와 사용자 연구를 통해 실제 동작에 가까운 모션 생성 능력이 뛰어남을 입증하였으며, 이는 가상 현실, 애니메이션, 인터랙티브 시스템 등 여러 분야에 응용될 수 있는 중요한 발전으로 평가됩니다.

[3D-LFM: Lifting Foundation Model](3/3D-LFM%20Lifting%20Foundation%20Model%20258b795c743847649f7f8fdc10e5ce57)

2D 키포인트를 3D 모델로 변환하는 새로운 접근 방식을 제공합니다, 다양한 객체 유형에 걸쳐 뛰어난 적응성과 범용성을 보여주며, 특히 순열 등가성을 활용하여 다른 카테고리의 객체를 효과적으로 처리합니다. 이 모델은 직교-N-포인트 및 원근-N-포인트 방식과 결합된 그래프 기반 트랜스포머 아키텍처를 사용하여, 보이지 않는 물체와 구성에 대한 강력한 일반화 능력을 입증합니다. 비록 극단적인 원근 왜곡과 깊이 모호성에 대한 도전이 존재하지만, 3D-LFM은 2D 데이터로부터 3D 구조를 효율적으로 재구성하는 강력한 도구로서, 이 분야의 지속적인 발전을 위한 유망한 기반을 제공합니다.

[WHENet: Real-time Fine-Grained Estimation for Wide Range Head Pose](3/WHENet%20Real-time%20Fine-Grained%20Estimation%20for%20Wide%20%2049af1aa3c15044ce93ea4d3f5e9e988c)

헤드 포즈 추정(HPE) 기술에 대한 혁신을 다룹니다. 이 연구에서는 WHENet, 즉 Wide Headpose Estimation Network를 소개합니다. 이 네트워크는 헤드 포즈 추정을 위한 모바일 친화적 모델로, 특히 광범위한 요 각도를 포괄합니다. WHENet는 기존의 앞면 뷰 중심 방식을 넘어서서 불리한 조명 조건, 가림막, 패션 액세서리가 있는 환경에서도 효과적으로 작동합니다. 이 네트워크는 효율적인 네트워크 구조를 사용하고, 새로운 래핑 손실 기법을 도입하여 큰 요 좌표에서의 성능을 향상시킵니다. WHENet은 높은 정확도를 유지하면서도 경량화된 구조로 실시간 애플리케이션에 적합하며, 전면 뷰뿐만 아니라 전체 범위의 헤드 포즈 추정에서 뛰어난 성능을 보여줍니다.

[SLoMo: A General System for Legged Robot Motion Imitation from Casual Videos](3/SLoMo%20A%20General%20System%20for%20Legged%20Robot%20Motion%20Imi%20f2856bf1f2c74924a7321139bb1a7af6)

이 논문은 단일 이동 카메라로 촬영된 인간 및 동물의 움직임을 로봇 하드웨어로 전송할 수 있는 최초의 일반적인 프레임워크를 제시합니다. 이 접근법은 로봇과 환경 간의 접촉 상호작용을 명시적으로 고려하며, 다양한 로봇 형태학에 적용 가능한 3D 재구성, 궤적 최적화, 모델 예측 제어 기술을 통합하여 인간 및 동물의 자연스러운 동작을 로봇이 모방할 수 있도록 합니다.

[Motion Mamba: Efficient and Long Sequence Motion Generation with Hierarchical and Bidirectional Selective SSM](3/Motion%20Mamba%20Efficient%20and%20Long%20Sequence%20Motion%20Ge%209364f0d269b74f11bd53425833eaa448)

본 연구에서 제안된 'Motion Mamba'는 계층적 시간 Mamba(Hierarchical Temporal Mamba, HTM) 블록과 양방향 공간 Mamba(Bidirectional Spatial Mamba, BSM) 블록을 통해 인간 모션 생성의 시간적 정렬과 공간적 정보 교환을 강화하는 새로운 접근 방식을 도입합니다. 이를 통해, 기존 확산 기반 모델의 한계를 극복하고, Fréchet Inception Distance(FID) 점수 개선 및 추론 속도의 현저한 향상을 달성하여 모션 생성 작업에서의 정확성과 효율성을 동시에 제공합니다.

[VLOGGER: Multimodal Diffusion for Embodied Avatar Synthesis](3/VLOGGER%20Multimodal%20Diffusion%20for%20Embodied%20Avatar%20S%20157b3778cb434c1380ad6906fea0e388)

VLOGGER는 오디오 또는 텍스트 입력만을 사용하여 단일 이미지에서 말하고 생생하게 움직이는 사람의 광사실적이고 시간적으로 일관된 비디오를 생성하는 첫 번째 기술입니다. 이는 머리 움직임, 시선, 눈 깜박임, 입 모양 움직임뿐만 아니라 상체와 손짓을 포함하여 오디오 주도 비디오 합성을 한 단계 더 발전시킨 점에서 차별화됩니다.