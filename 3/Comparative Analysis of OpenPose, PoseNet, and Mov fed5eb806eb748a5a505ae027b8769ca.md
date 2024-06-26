# Comparative Analysis of OpenPose, PoseNet, and MoveNet Models for Pose Estimation in Mobile Devices

[https://www.iieta.org/journals/ts/paper/10.18280/ts.390111](https://www.iieta.org/journals/ts/paper/10.18280/ts.390111)

### 1. INTRODUCTION

이 문서는 악수나 춤과 같은 인체의 움직임과 제스처를 의미하는 포즈의 개념에 초점을 맞추고 있습니다. 이러한 포즈는 비언어적 커뮤니케이션의 중요한 형태이며 데스크톱, 노트북, 스마트폰과 같은 기기는 물론 VR, AR, MR 환경과 상호 작용하는 데 활용할 수 있습니다. 이 접근 방식은 보다 직접적이고 확장된 사용자 경험을 제공합니다.

이를 위해서는 인체 위치를 결정하는 프로세스인 포즈 추정이 매우 중요합니다. 포즈 추정은 모션 캡처 기술을 통해 영화나 비디오 게임과 같은 분야에서 일반적으로 사용됩니다. 예를 들어, 배우가 센서가 부착된 수트를 입거나 몸에 마커를 부착하여 움직임을 기록하고 이를 컴퓨터 그래픽으로 오버레이할 수 있습니다. 이러한 예는 인터랙티브 게임 플레이를 위해 컨트롤러에 적외선 센서를 사용하는 닌텐도 Wii와 같은 게임 콘솔에서 볼 수 있습니다.

또 다른 예로는 카메라 기반 포즈 추정과 음성 인식을 결합하여 컨트롤러가 필요 없는 게임 환경을 제공하는 Microsoft의 Kinect가 있습니다. PlayStation의 카메라 및 이동 컨트롤러와 VIVE 트래커에도 유사한 기술이 사용됩니다.

스마트폰에서도 기기의 물리적 한계를 넘어 인터랙티브 기능을 확장하기 위해 포즈 추정을 사용하기 시작했습니다. 삼성 헬스나 애플 피트니스+와 같은 모바일 헬스 및 피트니스 앱은 사용자의 운동 자세를 안내하거나 AR 콘텐츠와 상호작용하는 데 포즈 추정을 사용할 수 있습니다.

모바일 디바이스에서 포즈 추정을 구현하기 위한 세 가지 주목할 만한 도구로는 OpenPose, PoseNet, MoveNet이 있습니다. 이러한 도구는 카메라 입력에 딥러닝 알고리즘을 사용하여 포즈를 추정하며, 추가 센서가 필요하지 않아 활용도가 매우 높습니다.

이 백서는 이 세 가지 포즈 추정 툴을 비교하고 논의하는 방식으로 구성되어 있습니다. 먼저 OpenPose, PoseNet, MoveNet의 기능을 소개한 다음 동일한 환경에서 각 도구의 특징과 성능을 평가하고, 마지막으로 결과를 바탕으로 결론을 내립니다.

### 2. MODELS

인체 움직임을 해석하는 데 필수적인 포즈 추정은 일반적으로 하향식 또는 상향식 방법을 통해 이루어집니다. 하향식 접근 방식은 먼저 사람을 식별한 다음 경계 상자 내에서 포즈를 결정하는 반면, 상향식 접근 방식은 모든 주요 포인트와 그 관계를 기반으로 포즈를 추론합니다.

2017년 카네기멜론 대학교에서 개발한 OpenPose는 실시간 다중 인원 포즈 추정을 위한 최초의 오픈소스 모델입니다. 이 모델은 상향식 접근 방식을 사용하여 포즈를 추정합니다. 프로세스는 2D 이미지/비디오를 가져와 신뢰도 맵을 생성하는 것으로 시작됩니다. 그런 다음 비최대 억제(NMS)를 사용하여 가능한 신체 부위를 식별하고, 중복된 바운딩 박스를 제거하며, 여러 신체 부위 간의 관계를 표시하는 부위 선호도 필드(PAF)를 생성합니다. 이 데이터를 사용하여 전신 포즈를 생성합니다.

![OpenPose의 전체 프로세스](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled.png)

OpenPose의 전체 프로세스

포즈넷은 구글 크리에이티브 랩에서 개발한 머신러닝 모델로, 사람의 포즈를 실시간으로 추정할 수 있습니다. 이 모델은 입력 이미지, 이미지 배율, 수평 뒤집기, 출력 보폭을 사용하여 1인 포즈를 추정합니다. 여러 사람의 포즈를 추정하려면 감지된 최대 포즈 수, 포즈 신뢰도 점수 임계값, NMS 반경이라는 세 가지 요소가 추가로 필요합니다. 포즈넷은 텐서플로우 라이트를 사용하는 안드로이드와 iOS 기기에서 작동합니다.

![PoseNet의 흐름](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%201.png)

PoseNet의 흐름

디지털 헬스 회사인 IncludeHealth가 Google의 지원을 받아 개발한 MoveNet은 두 가지 버전이 있습니다: 라이트닝과 썬더입니다. Lightning은 성능 지향적이며 192x192 고정 크기 입력을 수신하고 깊이 승수는 1.0이며, Thunder는 정확도 지향적이며 더 큰 입력(256x256)을 수신하고 깊이 승수 1.75를 사용합니다. 이러한 뎁스 배율은 입력 비디오/이미지 채널의 수를 변경합니다. 무브넷은 텐서플로우의 객체 감지 API와 모바일넷 V2를 특징 추출기로 사용해 작동하며, 센터넷 감지 API에 이어서 작동합니다. 표준 모델과 달리 무브넷은 중심점을 사용하여 객체를 식별하고 NMS를 피하기 때문에 성능이 향상됩니다.

![MoveNet의 흐름](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%202.png)

MoveNet의 흐름

세 가지 포즈 추정 모델인 OpenPose, PoseNet, MoveNet은 모두 서로 다른 방법을 사용하여 포즈 감지를 제공하며 성능과 정확도를 향상시키기 위해 지속적으로 개발되고 있습니다.

### 3. RESULTS AND DISCUSSION

세 가지 포즈 추정 모델인 OpenPose, PoseNet, MoveNet을 비교 분석한 결과, 각 모델의 성능에 대한 흥미로운 인사이트를 얻을 수 있었습니다. OpenPose는 다른 두 모델의 17개에 비해 137개로 더 많은 키포인트를 추적할 수 있어 보다 세밀한 신체 추적이 가능했습니다. 그러나 사람을 감지한 후 경계 상자 내에서 포즈를 추정하는 모바일 애플리케이션에서는 PoseNet이 더 효율적인 하향식 방법을 사용하는 반면, OpenPose와 MoveNet은 느린 상향식 방법을 사용하는 것으로 나타났습니다.

![이미지 그룹의 예시들 (a) 한 명의 사람이 있는 첫 번째 그룹 (b) 여러 명의 사람이 있는 두 번째 그룹 (c) 사람이 없는 세 번째 그룹](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%203.png)

이미지 그룹의 예시들 (a) 한 명의 사람이 있는 첫 번째 그룹 (b) 여러 명의 사람이 있는 두 번째 그룹 (c) 사람이 없는 세 번째 그룹

테스트는 한 사람이 포함된 이미지, 여러 사람이 포함된 이미지, 사람이 없는 이미지의 세 가지 이미지 그룹을 사용하여 수행되었습니다. 일반적으로 모든 모델이 사람이 없는 이미지에서 가장 좋은 성능을 보였으며, 이는 포즈 추정 작업이 없기 때문에 모델의 성능이 최적화되었음을 시사합니다. 포즈넷은 가장 작은 성능 변동을 보였는데, 이는 세 모델 중 가장 안정적인 모델이라는 것을 의미합니다.

흥미롭게도 상향식 방법을 사용하는 OpenPose는 모든 이미지 그룹을 고려했을 때 다른 모델보다 성능이 좋지 않았습니다. 이는 하향식 접근 방식에 비해 이 방법의 복잡성과 시간 집약적인 특성 때문일 수 있습니다.

![OpenPose의 결과들](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%204.png)

OpenPose의 결과들

포즈 추정 정확도는 모델마다 차이가 있었습니다. PoseNet이 가장 높은 정확도를 보였고, OpenPose, MoveNet이 그 뒤를 이었습니다. 그러나 이미지에서 여러 사람의 포즈를 성공적으로 식별하고 추정하는 모델은 OpenPose가 유일했기 때문에 여러 사람의 포즈를 감지해야 하는 사용 사례에서 선호되는 모델이 될 수 있습니다.

![행 결과의 일부 (a) 원본 이미지 (b) OpenPose의 결과들 (c) PoseNet의 결과들 (d) MoveNet Lightning의 결과들 (e) MoveNet Thunder의 결과들](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%205.png)

행 결과의 일부 (a) 원본 이미지 (b) OpenPose의 결과들 (c) PoseNet의 결과들 (d) MoveNet Lightning의 결과들 (e) MoveNet Thunder의 결과들

실행 결과를 분석한 결과, 세 번째 그룹에 포즈가 포함되지 않았을 때 불필요한 연산이 발생하여 오탐지 결과를 초래할 수 있는 경우에도 PoseNet과 MoveNet이 사람의 포즈를 출력할 수 있는 것으로 나타났습니다. 또 다른 중요한 발견은 오픈포즈가 실패한 경우에도 포즈넷과 무브넷이 성공적으로 포즈를 추론했다는 점으로, 하향식 방법의 성능 우월성을 강조합니다. 반대로 OpenPose는 인형과 같이 사람이 아닌 물체의 포즈를 인식하고 추정할 수 있었는데, 이는 항상 바람직하지 않을 수 있으며 잘못된 감지로 이어질 수 있습니다.

![첫 번째 그룹에서의 잘못된 출력들 (a)(b) PoseNet의 결과들 (c)(d) OpenPose의 결과들](Comparative%20Analysis%20of%20OpenPose,%20PoseNet,%20and%20Mov%20fed5eb806eb748a5a505ae027b8769ca/Untitled%206.png)

첫 번째 그룹에서의 잘못된 출력들 (a)(b) PoseNet의 결과들 (c)(d) OpenPose의 결과들

이러한 결과는 각 모델의 장단점과 이러한 특성이 다양한 시나리오에서 성능에 미치는 영향을 강조합니다. 따라서 가장 적합한 모델은 당면한 작업의 특정 요구 사항에 따라 달라집니다. 예를 들어, 여러 사람을 감지해야 하는 애플리케이션에서는 OpenPose가 더 적합할 수 있고, 속도와 안정성이 요구되는 애플리케이션에서는 PoseNet과 MoveNet이 선호될 수 있습니다. 추가 작업에는 특정 사용 사례에서 성능을 개선하기 위해 모델을 미세 조정하거나 기존 모델의 장점을 결합한 하이브리드 모델을 개발하는 것이 포함될 수 있습니다.

### 4. CONCLUSION

포즈 추정은 모바일 디바이스에서 점점 더 중요해지고 있으며, 기존의 터치 인터페이스를 뛰어넘는 기능과 애플리케이션을 구현할 수 있게 해줍니다. 스마트 스테이, 스마트 스크롤, 모션 센스와 같은 기술은 모두 포즈 추정으로 구동되는 기능의 예입니다. OpenPose, PoseNet, MoveNet과 같은 모델은 이러한 애플리케이션에서 상당한 잠재력을 가지고 있으며, 각각 고유한 장단점을 가지고 있습니다.

이 연구에서는 사전 분류된 이미지에 대한 기능과 성능에 초점을 맞춰 OpenPose, PoseNet, MoveNet의 모바일 버전을 평가하고 비교했습니다. 연구 결과 각 모델이 서로 다른 영역에서 특정 강점을 보이는 것으로 나타났습니다. MoveNet Lightning이 가장 빠른 모델로 확인되어 속도가 필요한 애플리케이션에 적합한 것으로 나타났습니다. OpenPose는 무브넷 라이트닝보다 12배 느리지만 여러 사람의 포즈를 추정할 수 있어 특정 시나리오에서 유용하게 사용될 수 있습니다. 포즈넷은 정확도가 가장 높았기 때문에 정밀도가 가장 중요한 경우에 선호되는 모델일 수 있습니다.

그럼에도 불구하고 이 연구에는 몇 가지 한계가 있습니다. 예를 들어, 카메라 입력의 잠재적 영향을 고려하지 않고 이미지에 대한 모델의 성능만 비교했습니다. 향후 연구에서는 라이브 카메라 피드에서 이러한 모델의 성능을 평가해야 합니다. 또한 최근에 개발된 MoveNet 멀티포즈는 이번 연구에 포함되지 않았으며, 이는 후속 연구에서 다룰 예정입니다. 이는 모바일 디바이스에서의 포즈 추정 모델과 그 적용에 대한 보다 포괄적인 평가를 제공하는 데 도움이 될 것입니다.

결론적으로, 포즈 추정 기술의 성장은 인터랙티브 모바일 애플리케이션 개발에 대한 흥미로운 전망을 제공합니다. 다양한 포즈 추정 모델의 상대적인 강점과 약점을 이해하는 것은 효과적인 구현을 위해 매우 중요하며, 이 연구가 이러한 이해에 기여하기를 바랍니다.