# Responsive real-time grass rendering for general 3D scenes

[https://www.cg.tuwien.ac.at/research/publications/2017/JAHRMANN-2017-RRTG/JAHRMANN-2017-RRTG-draft.pdf](https://www.cg.tuwien.ac.at/research/publications/2017/JAHRMANN-2017-RRTG/JAHRMANN-2017-RRTG-draft.pdf)

[https://github.com/LanLou123/Real-Time-Grass-Rendering](https://github.com/LanLou123/Real-Time-Grass-Rendering)

[https://github.com/klejah/ResponsiveGrassDemo](https://github.com/klejah/ResponsiveGrassDemo)

[https://gpuopen.com/learn/mesh_shaders/mesh_shaders-procedural_grass_rendering/](https://gpuopen.com/learn/mesh_shaders/mesh_shaders-procedural_grass_rendering/)

- 2017 i3D

![이 그림은 우리의 렌더링 기법의 예시를 보여줍니다. 볼링공이 지나간 흔적에서 충돌 반응이 보입니다. 오른쪽은 우리의 차폐 컬링 방법의 정확성을 보여주기 위해 와이어프레임 모드로 렌더링되었습니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled.png)

이 그림은 우리의 렌더링 기법의 예시를 보여줍니다. 볼링공이 지나간 흔적에서 충돌 반응이 보입니다. 오른쪽은 우리의 차폐 컬링 방법의 정확성을 보여주기 위해 와이어프레임 모드로 렌더링되었습니다.

## 1 Introduction

야외 장면 렌더링은 많은 상호작용 애플리케이션에서 중요한 작업입니다. 거의 모든 야외 장면에는 풀 또는 풀과 같은 식물이 포함되어 있습니다. 이러한 풀밭은 높은 기하학적 복잡성 때문에 종종 빌보드나 기타 이미지 기반 기법을 사용하여 렌더링됩니다. 그러나 이미지 기반 기법은 카메라의 위치와 시점에 따라 사실성이 달라지는 단점이 있습니다. 이를 해결하기 위해, 현대 풀 렌더링 기법은 각 풀잎을 기하학적 객체로 그립니다. 이렇게 하면 각 풀잎을 주변 환경에 따라 애니메이션할 수 있지만, 동시에 많은 기하학적 객체를 처리하기 위한 가속 구조가 필요합니다. 대부분의 기법은 하드웨어 인스턴싱을 사용하여 격자 기반 데이터 구조에서 풀 패치를 그리므로, 풀밭의 모양이 높이 필드로 제한되는 문제가 있습니다. 이는 많은 지형이 높이 맵과 일치하지 않기 때문에 문제가 됩니다.

이 논문에서는 각 풀잎을 기하학적 객체로 렌더링하고, 기하학적 구조에 구애받지 않는 가속 구조를 통해 임의의 3D 모델에서 풀밭을 렌더링할 수 있는 기법을 제안합니다. 각 풀잎의 렌더링을 위해 하드웨어 테셀레이션을 사용하여 동적인 디테일 수준을 적용하고, 풀잎의 모양은 분석 함수로 정의됩니다. 각 풀잎은 중력, 바람, 충돌과 같은 환경적 힘의 영향을 받습니다. 또한, 여러 컬링 방법을 통해 시각적 외형에 영향을 미치는 풀잎만 렌더링합니다. 이러한 컬링 기준에는 표준 차폐 컬링 외에도 카메라와의 방향 및 거리가 포함됩니다. 모든 계산은 GPU에서 간접 렌더링을 통해 수행되어, CPU와 GPU 간의 비용이 많이 드는 왕복을 피할 수 있습니다.

## 2 Previous Work

현재의 풀 렌더링 기법은 이미지 기반, 기하학적, 하이브리드 접근 방식으로 나눌 수 있습니다. 이미지 기반 렌더링 기법은 빠르기 때문에 상호작용 애플리케이션에서 가장 많이 사용됩니다. 대부분의 이미지 기반 기법은 반투명 풀 텍스처를 사용한 빌보드를 그립니다. 빌보드는 카메라를 향하게 하거나 별 모양의 클러스터로 배열될 수 있습니다. Orthmann 등(2009)은 복잡한 객체와의 충돌에 반응할 수 있는 빌보드 기법을 소개하였습니다. 다른 이미지 기반 기법으로는 그리드에 투명한 텍스처 슬라이스를 배치하는 방법이 있습니다. 모든 이미지 기반 기법의 주요 단점은 다른 각도에서 볼 때 시각적 품질이 다르다는 것입니다. 또한 바람 애니메이션과 충돌 반응으로 인해 텍스처가 심하게 왜곡되어 렌더링 아티팩트가 발생하고 사실성이 떨어집니다.

우리의 렌더링 기법과 유사하게 각 풀잎을 기하학적 객체로 그리는 여러 방법도 있습니다. 대부분의 기법은 하드웨어 인스턴싱을 사용하여 여러 번 풀 패치를 그리지만, 이는 풀밭이 높이 맵에 배치되어야 한다는 제한이 있습니다. 기하학적 방법의 장점은 각 풀잎이 환경에 따라 개별적으로 영향을 받을 수 있다는 것입니다. 예를 들어, Wang 등(2005)은 각 풀잎에 바람 효과를 시뮬레이션하기 위해 골격을 추가하고, Chen과 Johan(2010)은 충돌을 시뮬레이션하기 위해 파동 계산을 사용합니다. Jahrmann 등(2013)은 바람 애니메이션에 따라 풀잎의 끝을 이동시키고, 이미지 기반 방법을 사용하여 충돌을 근사합니다. Fan 등(2015)은 풀잎과 구체 간의 충돌을 평가하는 더 정교한 충돌 방식을 도입하였지만, 바람은 별도의 분석 함수를 사용하여 계산됩니다. 이러한 방법들과 달리, 우리의 렌더링 기법은 높이 맵에 제한되지 않습니다. 또한, 모든 물리적 모델을 일관된 방식으로 평가하여 중력, 바람 및 복잡한 객체와의 충돌과 같은 자연적인 힘을 계산합니다. 이전 방법들은 이러한 모든 효과를 결합하지 않았습니다.

순수 기하학적 또는 이미지 기반 렌더링의 대안으로, 빌보드를 프록시 기하학으로 그리고, 프래그먼트 셰이더에서 정확한 곡선 기하학을 평가하는 방법이 있습니다. 그러나 이 방법은 아직 풀 렌더링에 구현되지 않았습니다. Boulanger 등(2009)은 하이브리드 풀 렌더링 기법을 제안하였으며, 이는 서로 다른 정적 레벨의 디테일 단계를 사용합니다. 카메라에 가까운 풀은 기하학적 객체로, 먼 풀은 여러 수평 및 수직 텍스처 슬라이스로 그립니다. 이 접근 방식은 실시간으로 현실적인 이미지를 렌더링할 수 있으며, Madden NFL 25와 같은 상용 비디오 게임에 사용되었습니다. 그러나 풀잎은 고정되어 충돌이나 자연적인 힘에 반응할 수 없습니다. 여러 디테일 단계를 추가하여 렌더링 성능을 더욱 향상시킬 수 있습니다.

## 3 Overview

사전 처리 단계에서, 풀잎은 3D 모델의 표면에 분포되고 여러 패치로 나뉩니다. 각 패치는 대략 동일한 수의 풀잎을 포함하며, 임의의 형태와 정렬을 가질 수 있습니다. 이후 이미지 렌더링 과정은 세 단계로 진행됩니다:

1. **물리적 모델 평가**: 각 풀잎에 대한 물리적 모델이 평가됩니다. 이 모델은 중력, 바람, 충돌 등의 자연적 힘을 고려합니다.
2. **컬링**: 컬링 방법을 사용하여 최종 렌더링에 중요하지 않은 풀잎을 제거합니다. 이는 차폐, 카메라와의 방향 및 거리와 같은 기준을 기반으로 합니다.
3. **간접 렌더링**: 각 풀잎을 테셀레이션된 기하학적 객체로 간접 렌더링 방식을 사용하여 렌더링합니다. 이는 CPU와 GPU 간의 비용이 많이 드는 데이터 전송을 피하고, 모든 계산을 GPU에서 수행할 수 있게 합니다.

이 렌더링 기법은 각 풀잎을 개별적으로 그리고, 복잡한 환경 상호작용을 실시간으로 처리할 수 있도록 설계되었습니다. 이를 통해 임의의 3D 모델에서 높은 사실성을 유지하며 풀밭을 렌더링할 수 있습니다.

## 4 Preprocessing

사전 처리 단계에서는 3D 모델 표면에 풀잎을 생성하고 이를 여러 패치로 나누는 과정을 거칩니다. 먼저, 개별 풀잎의 모델을 소개합니다.

### **풀잎 모델**

풀잎은 세 개의 제어점 𝑣0,𝑣1,𝑣2 으로 구성된 2차 베지에 곡선으로 모델링됩니다. 첫 번째 제어점 𝑣0는 풀잎의 고정된 위치를 나타내고, 두 번째 제어점 𝑣2는 물리적 모델에 따라 움직입니다. 중간 제어점 𝑣1는 𝑣2의 위치에 따라 조정됩니다. 풀잎은 높이, 너비, 강성 계수, 업 벡터, 방향 각도와 같은 속성도 가집니다. 이를 통해 풀잎은 네 개의 4차원 벡터로 완전히 설명될 수 있습니다.

![풀잎의 정의를 설명하는 그림입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%201.png)

풀잎의 정의를 설명하는 그림입니다.

### **풀 분포**

풀잎은 개별 풀잎 또는 전체 다발 형태로 생성될 수 있습니다. 생성되는 풀잎의 수는 사용자 정의 밀도 값과 3D 모델의 총 면적에 따라 결정됩니다. 다발 형태의 풀은 Poisson-disk 샘플링을 사용하여 다발이 겹치지 않도록 분포시키며, 다발 중심 주변에 무작위로 배치됩니다. 개별 풀잎은 3D 모델 표면에 무작위로 분포되며, 이는 자연스러운 풀밭 분포에 유리합니다. 실제적인 초원은 두 가지 분포 방법을 조합하여 생성할 수 있습니다.

### **패치 생성**

풀잎이 생성된 후, 이들을 여러 패치로 나눕니다. 패치의 수는 렌더링 알고리즘의 성능에 중요한 영향을 미치며, 최적의 패치 수는 그래픽 하드웨어에 따라 달라집니다. 물리적 모델 평가와 컬링은 컴퓨팅 셰이더를 사용하여 수행되므로, 패치 내의 풀잎 수는 모든 패치에서 동일하게 하고, 최대 병렬 처리를 위해 하드웨어가 보고하는 작업 그룹 호출 수의 배수를 사용합니다. 패치는 가능한 한 콤팩트하고 직사각형 형태로 만들어야 하며, 이는 컬링의 효율성을 높입니다.

패치를 생성하는 과정은 균형 잡힌 클러스터링 문제로 볼 수 있으며, 각 클러스터는 동일한 수의 요소를 포함해야 합니다. 이 문제는 선형 프로그래밍이나 그래프 이론적 접근 방식을 통해 효율적으로 해결할 수 있습니다. 패치로 나뉜 후, 각 패치의 풀잎은 인덱스를 기준으로 정렬됩니다. 현재는 좌표에 따른 사전식 정렬이 효율적인 것으로 나타났지만, 보다 정교한 정렬 알고리즘도 검토할 수 있습니다.

## 5 Physical Model

우리의 물리적 모델은 각 풀잎에 대해 자연적인 힘과 충돌을 시뮬레이션하며, 높은 사실성을 위해 독립적으로 평가됩니다. 이 계산은 전적으로 그래픽 카드의 컴퓨트 셰이더에서 수행됩니다. 풀잎의 자유로운 움직임을 허용하기 위해, 먼저 힘은 풀잎 끝점인 𝑣2만을 조작하며, 세 가지 보정 단계를 통해 풀잎의 유효한 상태를 유지합니다.

![ 물리적 모델에서 고려되는 다양한 영향을 설명하는 그림입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%202.png)

 물리적 모델에서 고려되는 다양한 영향을 설명하는 그림입니다.

### **힘 계산**

𝑣2의 변위 𝛿는 세 가지 자연적인 힘(복원력 𝑟, 중력 𝑔, 바람 𝑤)과 충돌에 의한 변위 𝑑로 계산됩니다. 힘은 시간 간격 Δ𝑡로 정규화된 후 적용됩니다. 이 변위는 풀잎의 고유한 텍셀에 저장되며, 충돌 강도는 시간이 지나면서 감소하여 풀잎이 서서히 원래 위치로 돌아오도록 합니다.

### **자연적인 힘**

### **복원력**

복원력은 이전에 가해진 힘에 대한 반발력으로, 후크의 법칙을 따릅니다. 풀잎의 초기 위치 𝐼𝑣2로 향하는 힘이며, 강성 계수 𝑠*s*와 충돌 강도 𝜂에 따라 달라집니다.

𝑟=(𝐼𝑣2−𝑣2)𝑠 max⁡(1−𝜂,0.1)

### **중력**

중력은 두 가지 추가적인 힘으로 구성됩니다. 하나는 환경 중력 𝑔𝐸*gE*로, 이는 전체 장면의 중력을 나타내며, 전역 중력 방향과 중력 중심 두 가지 방식으로 나타낼 수 있습니다. 다른 하나는 풀잎의 너비에 수직인 앞면 중력 𝑔𝐹입니다.

![Untitled](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%203.png)

### **바람**

바람은 3D 공간을 이동하는 바람 파동을 나타내는 분석 함수로 계산됩니다. 바람의 세기는 풀잎의 높이와 방향에 따라 달라집니다.

𝑤=𝑤𝑖(𝑣0)𝜃(𝑤𝑖(𝑣0),ℎ)

![2D 공간에서 두 가지 다른 바람 함수의 결과를 보여줍니다. 빨간 표면의 높이는 해당 위치에서 바람의 강도를 나타내며, 검은 화살표는 바람 파동의 방향과 움직임을 설명합니다. 위쪽 함수는 특정 방향에서 불어오는 일반적인 바람을 시뮬레이션하고, 아래쪽 함수는 특정 바람 원천의 영향을 보여줍니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%204.png)

2D 공간에서 두 가지 다른 바람 함수의 결과를 보여줍니다. 빨간 표면의 높이는 해당 위치에서 바람의 강도를 나타내며, 검은 화살표는 바람 파동의 방향과 움직임을 설명합니다. 위쪽 함수는 특정 방향에서 불어오는 일반적인 바람을 시뮬레이션하고, 아래쪽 함수는 특정 바람 원천의 영향을 보여줍니다.

### **상태 검증**

풀잎의 유효한 상태는 세 가지 조건으로 정의됩니다: 𝑣2가 지면 아래로 밀리지 않아야 하며, 𝑣1의 위치는 𝑣2의 위치에 따라 설정되고, 곡선의 길이는 풀잎의 높이와 동일해야 합니다.

![v1 과 v2사이의 관계를 설명하는 그림입니다. 다른 색상은 풀잎의 다른 상태를 상징합니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%205.png)

v1 과 v2사이의 관계를 설명하는 그림입니다. 다른 색상은 풀잎의 다른 상태를 상징합니다.

### **충돌**

풀잎은 충돌 시 환경에 반응해야 하므로, 각 풀잎에 대해 충돌을 감지하고 반응합니다. 충돌 검출은 곡선과 구체 사이의 교차점을 측정하는 대신, 𝑣2와 곡선의 중심점 𝑚을 사용합니다.

![Untitled](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%206.png)

충돌이 감지되면, 해당 점을 구의 표면으로 이동시킵니다.

![Untitled](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%207.png)

충돌이 감지될 때마다 변위의 제곱 길이가 충돌 강도 𝜂에 추가됩니다.

𝜂=𝜂+𝑑⋅𝑑

이를 통해 각 풀잎은 환경과 상호작용하며, 자연스러운 행동을 시뮬레이션할 수 있습니다.

![풀잎과 구체 사이의 두 가지 가능한 충돌을 설명하는 그림입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%208.png)

풀잎과 구체 사이의 두 가지 가능한 충돌을 설명하는 그림입니다.

## 6 Rendering

풀밭을 렌더링하기 위해, 각 풀잎은 테셀레이션된 2D 객체로 그려집니다. Jahrmann 등(2013)의 방법과 유사하게, 우리는 테셀레이션 파이프라인을 사용하여 풀잎의 모양에 동적인 레벨의 디테일을 제공합니다. 하지만 알파 텍스처를 사용하는 대신, 분석 함수를 사용하여 직접 기하학을 수정합니다. 각 풀잎이 개별적인 상태와 위치를 가지고 있어 여러 패치를 동시에 렌더링할 수 없기 때문에, 우리는 싱글 풀잎 단위의 컬링을 사용하여 필드의 시각적 외형에 영향을 미치는 풀잎만 렌더링합니다. 이를 위해 간접 렌더링 접근 방식을 사용합니다.

### **간접 렌더링**

직접 렌더링과 달리, 간접 렌더링 호출은 드로우 명령의 매개변수를 포함하지 않습니다. 대신, 매개변수는 GPU 메모리의 버퍼에서 읽습니다. 이를 통해 컴퓨트 셰이더 내에서 매개변수 버퍼를 수정할 수 있으며, CPU와의 동기화가 필요 없습니다. 우리 기술에서는 컴퓨트 셰이더를 사용하여 원치 않는 풀잎을 제거합니다. 컬링되지 않은 각 풀잎은 매개변수 버퍼의 객체 수를 증가시키고, 인덱스 버퍼에 인덱스를 기록합니다.

![와이어프레임 모드에서 차폐 테스트의 효과를 설명하는 그림입니다. 왼쪽 이미지는 차폐 테스트가 적용된 경우이고, 오른쪽 이미지는 적용되지 않은 경우입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%209.png)

와이어프레임 모드에서 차폐 테스트의 효과를 설명하는 그림입니다. 왼쪽 이미지는 차폐 테스트가 적용된 경우이고, 오른쪽 이미지는 적용되지 않은 경우입니다.

### **컬링**

컬링은 두 단계로 수행됩니다. 첫 번째 단계에서는 패치의 경계 상자를 카메라의 뷰 프러스텀과 테스트합니다. 사전 처리에서 경계 상자는 잠재적인 풀잎의 움직임을 고려하여 계산됩니다. 두 번째 단계에서는 보이는 패치의 각 풀잎을 다른 객체에 의한 차폐, 카메라와의 방향 및 거리 기준으로 테스트합니다. 이 테스트를 통과한 풀잎만 렌더링됩니다.

### **방향 테스트**

이 테스트는 카메라를 향한 풀잎의 방향에 따라 풀잎을 제거합니다. 풀잎은 두께가 없기 때문에, 뷰잉 방향과 평행한 풀잎은 원치 않는 앨리어싱 아티팩트를 유발할 수 있습니다. 따라서, 뷰잉 방향 𝑑𝑖𝑟𝑐*dirc*과 풀잎의 너비 벡터 𝑑𝑖𝑟𝑏*dirb* 사이의 각도의 코사인 값이 0.9를 초과하면 풀잎을 제거합니다.

0.9>∣𝑑𝑖𝑟𝑐⋅𝑑𝑖𝑟𝑏∣→풀잎제거

### **뷰 프러스텀 테스트**

두 번째 테스트는 풀잎이 카메라의 뷰 프러스텀 내에 있는지 확인합니다. 풀잎의 모든 점을 테스트하는 대신, 𝑣0, 곡선의 중간 점 𝑚, 𝑣2의 세 점만 고려합니다. 이 점들을 뷰 프로젝션 행렬 𝑉𝑃를 사용하여 정규화된 장치 좌표로 투영한 후, 각 좌표를 비교하여 테스트합니다.

### **거리 테스트**

세 번째 테스트는 풀잎과 카메라 간의 거리에 따라 풀잎을 제거합니다. 풀밭은 수평선 근처에서 더 밀집되어 보이므로, 이 높은 밀도로 인해 깊이 값의 낮은 정밀도와 앨리어싱 아티팩트가 발생할 수 있습니다. 따라서, 카메라와 풀잎 사이의 거리를 로컬 평면에 투영한 후 이 거리를 사용하여 풀잎을 제거합니다.

𝑑𝑝𝑟𝑜𝑗=∥𝑣0−𝑐−𝑢𝑝((𝑣0−𝑐)⋅𝑢𝑝)∥

### **차폐 테스트**

마지막 테스트는 풀잎이 다른 객체에 의해 차폐되는지 확인합니다. 이 테스트는 곡선의 세 점을 화면 좌표로 투영하여 이전에 생성된 불투명 장면 객체의 선형 깊이 값을 나타내는 텍스처를 샘플링합니다. 샘플링된 깊이 값이 카메라와 풀잎 간의 거리보다 작으면 풀잎을 제거합니다.

### **풀잎 기하학**

렌더링 과정에서 각 풀잎은 3D 공간에 위치한 2D 객체로 그려집니다. 풀잎의 모양 생성은 테셀레이션 평가 셰이더에서 수행되며, 이는 하드웨어 테셀레이션 유닛의 정보를 사용하여 생성된 정점의 위치를 결정합니다. 초기 풀잎 기하학은 𝑢와 𝑣라는 보간 파라미터로 정의된 평면 사각형입니다. 𝑢는 풀잎의 너비를 따라, 𝑣는 풀잎의 높이를 따라 보간합니다. 각 정점에 대해 제어점의 곡선 보간을 평가하여 사각형을 베지에 곡선에 맞춥니다.

풀잎의 더 정교한 모양을 적용하기 위해, 우리는 분석 함수를 사용하여 생성된 정점의 최종 위치를 계산합니다. 기본 모양으로는 사각형, 삼각형, 포물선, 삼각형 팁 등이 있으며, 이 외에도 민들레 잎과 같은 복잡한 모양도 생성할 수 있습니다. 추가 기능으로는 3D 변위와 폭 보정이 있습니다. 3D 변위는 풀잎의 중간 축을 정상 벡터를 따라 변위시켜 'V'자 모양을 만듭니다. 폭 보정은 멀리 있는 풀잎이 픽셀 크기보다 얇아져 앨리어싱 아티팩트가 발생하는 것을 방지합니다.

![네 가지 기본 모양인 사각형, 삼각형, 포물선형, 삼각형 팁을 설명하는 그림입니다. 빨간색과 녹색 점선은 𝑐 0 와 𝑐 1 의 위치를 나타냅니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2010.png)

네 가지 기본 모양인 사각형, 삼각형, 포물선형, 삼각형 팁을 설명하는 그림입니다. 빨간색과 녹색 점선은 𝑐 0 와 𝑐 1 의 위치를 나타냅니다.

### **기본 모양**

기본 모양의 정점 위치 𝑝*p*는 보간 파라미터 𝑡*t*를 사용하여 두 곡선 점 𝑐0*c*0와 𝑐1*c*1 사이를 보간하여 계산됩니다.

𝑝=(1−𝑡)𝑐0+𝑡𝑐1

각 모양의 보간 파라미터는 각기 다릅니다. 예를 들어, 사각형 모양은 𝑡=𝑢를 사용하고, 삼각형 모양은 𝑡=𝑢+0.5𝑣−𝑢𝑣를 사용합니다.

### **민들레**

민들레 모양은 복잡한 삼각 함수 방정식을 사용하여 두 곡선 점 사이를 보간합니다. 이 함수는 테셀레이션 수준도 포함하여 저수준의 테셀레이션에서도 스파이크를 잃지 않도록 합니다.

![민들레 모양을 설명하는 그림입니다. 왼쪽 이미지는 분석적 민들레 함수의 그래프를 나타내며, x축은 𝑣 v, y축은 𝑢 u를 나타냅니다. 다른 색상은 서로 다른 테셀레이션 수준에 해당합니다. 오른쪽 이미지는 민들레 다발의 렌더링을 보여줍니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2011.png)

민들레 모양을 설명하는 그림입니다. 왼쪽 이미지는 분석적 민들레 함수의 그래프를 나타내며, x축은 𝑣 v, y축은 𝑢 u를 나타냅니다. 다른 색상은 서로 다른 테셀레이션 수준에 해당합니다. 오른쪽 이미지는 민들레 다발의 렌더링을 보여줍니다.

### **3D 변위**

3D 변위는 풀잎의 중간 축을 정상 벡터를 따라 변위시켜 'V'자 모양을 만듭니다. 변위 벡터 𝑑*d*는 다음과 같이 계산됩니다.

𝑑=𝑤𝑛(0.5−∣𝑢−0.5∣(1−𝑣))

### **폭 보정**

폭 보정은 멀리 있는 풀잎이 픽셀 크기보다 얇아져 앨리어싱 아티팩트가 발생하는 것을 방지합니다. 폭이 최소 폭 𝑤𝑚𝑖𝑛*wmin* 이하인 경우 보정 값 ΦΦ가 1이 되어 풀잎을 사각형으로 만듭니다.

![Untitled](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2012.png)

이러한 방법을 통해 풀밭을 보다 사실적으로 렌더링하고, 실시간 성능을 유지할 수 있습니다.

## 7 Results

이 장에서는 제안된 렌더링 기법의 결과를 관련 알고리즘과 비교하여 제시합니다. 우리의 결과 평가는 시각적 외형, 그래픽 카드에서 소요된 시간, 프레임당 총 소요 시간을 기준으로 이루어집니다. 결과는 C++와 OpenGL 4.5를 사용하여 구현된 테스트 프레임워크에서 렌더링되었으며, 이 프레임워크는 필드의 기하학과 애니메이션에 중점을 두지만, 현대 엔진에서 일반적으로 사용되는 그림자, 앰비언트 오클루전, 대기 효과 등의 추가적인 사실적 렌더링 기법은 포함되지 않습니다. 하지만 이는 방법의 한계가 아닙니다. 풀잎이 기하학적 객체로 그려지기 때문에, 이러한 기법을 지원하는 엔진에 우리의 방법을 통합하는 것은 간단합니다. 테스트는 NVIDIA GeForce GTX 780M 그래픽 카드와 Intel Core i7-4800 @ 2.7 GHz CPU, 32GB 램을 사용하는 머신에서 수행되었으며, 렌더링 해상도는 1024x768 픽셀입니다. 앨리어싱 아티팩트를 줄이기 위해 8배의 MSAA가 사용되었습니다. 제안된 풀 렌더링 기법의 대표적인 오픈 소스 데모 애플리케이션은 [GitHub](https://github.com/klejah/ResponsiveGrassDemo)에서 제공됩니다.

다음 두 장면은 각각 다른 측정 기준으로 평가되고 논의됩니다. 측정 기준에는 초당 렌더링 프레임 수, 프레임 렌더링 시간, 렌더링된 풀잎 수, 컬링된 풀잎 수, 물리적 모델 평가에 사용된 시간, 가시성 계산 및 간접 렌더링 설정에 사용된 시간, 렌더링에 사용된 시간, 힘 업데이트에 고려된 충돌 구체 수가 포함됩니다. 시간 값은 밀리초 단위로 측정됩니다. 측정은 모든 기능이 활성화된 경우, 충돌 감지가 비활성화된 경우, 컬링이 비활성화된 경우 등 세 가지 상황에서 수행됩니다. 공정한 비교를 위해 각 장면의 모든 측정은 고정된 참조 시점에서 동일한 입력 데이터를 사용하여 수행됩니다.

![왼쪽 이미지는 평가된 자연 장면의 렌더링을 보여줍니다. 오른쪽 이미지는 토끼 모델의 구체 표현을 시각화한 것입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2013.png)

왼쪽 이미지는 평가된 자연 장면의 렌더링을 보여줍니다. 오른쪽 이미지는 토끼 모델의 구체 표현을 시각화한 것입니다.

### **7.1 자연 장면**

자연 장면은 여러 3D 객체로 구성되며, 야외 시나리오를 재현합니다. 이 장면의 렌더링 결과는 그림 10에 제시되어 있습니다. 풀밭은 부드러운 언덕이 있는 지형에 생성되었으며, 397,881개의 풀잎으로 구성됩니다. 각 풀잎은 적당한 너비를 가지고 있어 높은 밀도를 나타냅니다. 장면에는 총 1000개의 충돌 구체로 표현된 토끼 모델이 포함되어 있으며, 두 개의 구체가 굴러가면서 흔적을 남기도록 설정되어 물리적 모델의 효과를 보여줍니다. 또한 시각적 표현을 개선하기 위해 여러 객체가 추가되었습니다.

### **7.2 헬리콥터 장면**

헬리콥터 장면은 바람 효과와 매우 높은 밀도의 풀밭 렌더링을 보여줍니다. 헬리콥터가 지면 위를 날고 있어 풀잎이 다른 객체에 의해 차폐되지 않기 때문에, 이는 우리의 알고리즘에 대한 최악의 시나리오를 나타냅니다. 풀밭은 900,000개의 풀잎으로 구성되어 있으며, 헬리콥터의 바람 효과는 헬리콥터를 바람의 원천으로 하는 점 기반 바람으로 시뮬레이션됩니다. 그림 11에 이 장면의 렌더링이 제시되어 있으며, 테이블 3에는 측정 결과가 나와 있습니다.

![헬리콥터 장면의 렌더링을 보여줍니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2014.png)

헬리콥터 장면의 렌더링을 보여줍니다.

### **7.3 관련 연구와의 비교**

우리의 렌더링 기법은 다른 많은 풀 렌더링 기법, 특히 기하학적 접근 방식과 비교하여 임의의 모양과 공간 정렬을 가진 풀밭을 처리할 수 있습니다. 이는 높이 맵으로 모델링할 수 없는 다양한 장면을 가능하게 합니다. 또한, 3D 모델 위에 자랄 수 있는 풀은 털이나 머리카락을 시뮬레이션할 수도 있습니다.

우리의 기법은 물리적 상호작용에 중점을 둡니다. Orthmann 등(2009)과 Fan 등(2015)은 풀과 환경 충돌체 간의 상호작용에 중점을 둡니다. Orthmann 등은 복잡한 객체와의 충돌에 반응할 수 있는 빌보드를 사용하며, 충돌이 감지되면 빌보드의 정점이 이동되고 일정 시간이 지나면 빌보드가 원래 상태로 돌아갑니다. Fan 등의 알고리즘은 3D 객체로 표현된 풀잎과 구체 간의 충돌을 제한합니다. 충돌 반응으로, 해당 풀잎의 정점이 이동되고 일정 시간이 지나면 원래 상태로 리셋됩니다.

![복잡한 3D 모델 두 개에 풀잎이 자라는 모습을 보여줍니다. 각각 다른 색상 텍스처를 사용했습니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2015.png)

복잡한 3D 모델 두 개에 풀잎이 자라는 모습을 보여줍니다. 각각 다른 색상 텍스처를 사용했습니다.

![뫼비우스 띠 모델에 풀잎이 자라는 모습을 보여줍니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2016.png)

뫼비우스 띠 모델에 풀잎이 자라는 모습을 보여줍니다.

이와 달리, 우리의 기법은 각 풀잎을 개별적으로 처리하고, 구체와 복잡한 객체와의 충돌에 반응할 수 있습니다. 또한, 각 풀잎은 개별 애니메이션 상태를 저장하여, 충돌 후 풀잎이 원래 상태로 돌아가는 시간이 충돌의 세기에 따라 달라지도록 합니다. Orthmann 등의 기법과 비교하여, 손이 풀밭 위를 움직이는 장면을 모델링했습니다. 그림 14에 나타난 것처럼 손가락이 지나간 자리가 분명히 보이며, 풀잎이 눌려졌습니다. Fan 등의 기법과 비교하여, 그림 15에 나타난 것처럼 많은 공이 풀밭 위로 던져지는 장면을 생성했습니다. 우리 렌더링은 더 밀도가 높아 충돌 반응이 더 잘 보입니다.

![Orthmann 등(2009)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 장면 모두 복잡한 객체가 초원을 통과하는 모습을 보여줍니다. 이는 빌보드를 사용하는 대신 각 풀잎을 기하학적 객체로 그리는 장점이 잘 드러납니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2017.png)

Orthmann 등(2009)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 장면 모두 복잡한 객체가 초원을 통과하는 모습을 보여줍니다. 이는 빌보드를 사용하는 대신 각 풀잎을 기하학적 객체로 그리는 장점이 잘 드러납니다.

![Fan 등(2015)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 장면 모두 수백 개의 공이 던져지는 풀밭을 보여줍니다. 풀밭의 밀도가 높아 충돌 효과가 오른쪽 이미지에서 더 잘 보입니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2018.png)

Fan 등(2015)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 장면 모두 수백 개의 공이 던져지는 풀밭을 보여줍니다. 풀밭의 밀도가 높아 충돌 효과가 오른쪽 이미지에서 더 잘 보입니다.

Wang 등(2005)의 연구는 각 풀잎에 적용되는 현실적인 자연력을 나타냅니다. 이 기법은 헬리콥터 착륙이나 토네이도와 같은 특별한 바람 효과를 시뮬레이션할 수 있습니다. 바람 영향을 계산하기 위해, 저자는 풀잎이 직립된 위치에 있다고 가정하고 바람 효과로 인한 변위를 계산합니다. 우리의 물리적 모델은 프레임 간의 지속적인 상태를 가지며, 이를 통해 자연력과 충돌을 하나의 물리적 모델로 구현할 수 있습니다. 그림 16은 헬리콥터와 토네이도를 시뮬레이션한 두 장면을 나타냅니다.

![Wang 등(2005)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 기법 모두 삼각 함수로 계산된 영향보다 더 복잡한 특별한 바람 효과를 생성할 수 있습니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2019.png)

Wang 등(2005)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 기법 모두 삼각 함수로 계산된 영향보다 더 복잡한 특별한 바람 효과를 생성할 수 있습니다.

Jahrmann 등(2013)은 부드러운 모양의 풀잎을 렌더링하기 위해 테셀레이션 파이프라인을 사용하는 유사한 렌더링 접근 방식을 사용합니다. 풀잎의 모양은 알파 텍스처로 생성되며, 보이지 않는 프래그먼트는 폐기됩니다. 이는 다양한 모양을 쉽게 생성할 수 있지만, 사용된 텍스처의 해상도가 시각적 외형에 중요합니다. 텍스처 샘플링 아티팩트가 발생할 수 있으며, 높은 해상도의 알파 텍스처는 높은 메모리 사용량과 느린 처리 속도를 초래합니다. 이와 달리, 우리는 분석 함수를 사용하여 풀잎의 기하학을 직접 수정하여 모양을 생성합니다. 이를 통해 처리해야 할 프래그먼트 수를 줄이고, 거리와 관계없이 모양의 가장자리를 부드럽게 유지할 수 있습니다. 그림 17은 두 기법의 풀잎을 근접 촬영한 모습입니다.

![Wang 등(2005)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 렌더링 모두 풀잎의 근접 뷰를 보여줍니다. 알파 텍스처로 생성된 모양은 텍스처 샘플링 아티팩트를 보여주지만, 분석 함수는 매끄러운 가장자리를 생성합니다.](Responsive%20real-time%20grass%20rendering%20for%20general%203%2088f14889ac0a4f588fc409876a656779/Untitled%2020.png)

Wang 등(2005)의 기법(왼쪽)과 우리의 기법(오른쪽)을 비교한 그림입니다. 두 렌더링 모두 풀잎의 근접 뷰를 보여줍니다. 알파 텍스처로 생성된 모양은 텍스처 샘플링 아티팩트를 보여주지만, 분석 함수는 매끄러운 가장자리를 생성합니다.

## 8 Conclusion and Future Work

이 논문에서는 밀도가 높은 풀밭을 실시간으로 렌더링할 수 있는 새로운 풀 렌더링 기법을 제안했습니다. 관련 연구와 비교하여, 우리는 임의의 모양과 공간 정렬을 가진 풀밭을 처리할 수 있습니다. 또한, 각 풀잎을 기하학적 객체로 렌더링하여 환경에 반응할 수 있게 합니다. 우리의 물리적 모델은 중력, 바람 및 복잡한 객체와의 충돌을 포함한 자연력을 평가하여 각 풀잎에 적용합니다. 복잡한 객체의 충돌 검출에는 구체 팩킹 접근 방식을 사용합니다. 실시간 성능을 달성하기 위해, 우리는 차폐 및 카메라와의 방향 및 거리 기준을 기반으로 단일 풀잎을 컬링하는 방법을 도입했습니다. 이러한 컬링 방법은 표준 프레임에서 전체 풀잎의 최대 75%를 제거할 수 있습니다. 그러나 여전히 각 풀잎의 렌더링이 성능의 병목 현상입니다. 렌더링 시간을 줄이기 위해 Boulanger 등(2009)의 연구처럼 다양한 디테일 수준의 표현을 도입하는 것이 향후 작업으로 고려될 수 있습니다.