# A Reconstruction Filter for Plausible Motion Blur

[https://dl.acm.org/doi/10.1145/2159616.2159639](https://dl.acm.org/doi/10.1145/2159616.2159639)

[https://casual-effects.com/research/McGuire2012Blur/McGuire12Blur.pdf](https://casual-effects.com/research/McGuire2012Blur/McGuire12Blur.pdf)

- 2012

![돌리 카메라로 추적하는 드래곤이 불타는 도시 위를 지나가며, GeForce 480에서 1280×720 해상도로 3ms 내에 모션 블러 결과가 재구성되었습니다. 이 이미지는 줌, 팬, 이동, 변형, 회전을 포함하는 캐릭터와 카메라 움직임을 모두 포함합니다. 전체 해상도의 보조 깊이 및 속도 입력 버퍼가 축소된 크기로 삽입되어 있습니다. 속도는 알고리즘을 구현할 때 관찰된 세밀한 속도 버퍼 위에 거친 벡터 필드로 시각화되었으며, (r, g, b) = (dx/dt, dy/dt, 0)/k + 0.5로 표현됩니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled.png)

돌리 카메라로 추적하는 드래곤이 불타는 도시 위를 지나가며, GeForce 480에서 1280×720 해상도로 3ms 내에 모션 블러 결과가 재구성되었습니다. 이 이미지는 줌, 팬, 이동, 변형, 회전을 포함하는 캐릭터와 카메라 움직임을 모두 포함합니다. 전체 해상도의 보조 깊이 및 속도 입력 버퍼가 축소된 크기로 삽입되어 있습니다. 속도는 알고리즘을 구현할 때 관찰된 세밀한 속도 버퍼 위에 거친 벡터 필드로 시각화되었으며, (r, g, b) = (dx/dt, dy/dt, 0)/k + 0.5로 표현됩니다.

## 1 Introduction and Related Work

실제 카메라는 노출 시간 동안 들어오는 빛을 각 픽셀에 통합하여, 움직이는 물체의 이미지를 흐리게 만듭니다. 이러한 모션 블러 효과를 컴퓨터 생성 이미지에서 구현하기 위한 두 가지 주요 접근 방식이 있습니다. 첫 번째는 즉시성에 기반한 방법으로, 단일 시간에 촬영된 이미지를 모션 블러와 유사한 현상으로 향상시키는 것입니다. 이러한 방법은 실시간 애플리케이션에 적합하지만, 입력이 노출을 아우르는 정보가 부족하기 때문에 이상적인 품질을 제공하지 못합니다. 두 번째는 재구성 방법으로, 노출 기간에 걸쳐 샘플을 필터링하고 감소시켜, 샘플링 연산자를 역으로 적용하여 움직이는 장면의 물리적으로 정확한 이미지를 복구하려고 합니다. 전통적인 재구성 필터는 픽셀 내의 모든 샘플을 단순히 평균화하거나 좁은 커널을 고정하여 적용합니다. 이러한 필터는 빠르지만, 입력이 조밀하게 샘플링되어야 하며, 이는 주로 레이 트레이싱이나 마이크로폴리곤 래스터화와 같은 확률적 프로세스에 의해 이루어집니다.

### 1. Introduction and Related Work

최근에는 공간-시간 이미지 처리의 비등방성을 고려한 물리 기반의 스마트 재구성 방법이 개발되었습니다. 이러한 방법은 고전적인 재구성 방법보다 훨씬 낮은 입력 샘플 밀도에서 높은 품질의 이미지를 생성할 수 있지만, 스마트 재구성 자체가 프레임당 몇 초에서 몇 분이 걸릴 수 있습니다. 본 논문에서는 게임과 같은 실시간 애플리케이션에서 모션 블러를 위한 새로운 가능한-재구성 알고리즘을 소개합니다. 이 알고리즘은 확률적 샘플의 오프라인 스마트 재구성 아이디어를 차용하여 이미지 품질을 향상시키지만, 일반적인 비디오 게임 렌더러에서 생성된 이미지에서 작동합니다. 이 새로운 방법은 이전의 가능한 방법이 제한되거나 실패하는 경우에도 성공하며, 스마트 재구성보다 성능이 뛰어납니다. 이 방법은 예술적인 조정이 가능하여, 시청자의 주의를 특정 부분으로 유도하는 데 효과적입니다.

### 1.1 Previous Real-Time Plausible Methods

실시간 렌더링을 위한 이전의 가능한 모션 블러 방법은 여러 샘플을 평균화하거나 객체의 경계를 확장하는 등의 다양한 기법을 사용하여 모션 블러를 구현했습니다. 예를 들어, MotoGP 2와 Split Second 같은 레이싱 게임은 움직이는 객체의 텍스처를 여러 샘플로 흐리게 하여 경계선과 텍스처 이음새를 날카롭게 남기는 문제를 겪었습니다. 다른 접근 방식으로는 움직이는 객체의 기하학을 확장하거나 텍스처를 흐리게 하고, 이를 알파 블렌드하는 방법이 있습니다. 이 방법은 깊이 정렬이 완벽해야 하고 대략적으로 볼록한 기하학이 필요합니다.

최신 방법으로는 픽셀 단위의 블러가 있으며, 속도 버퍼를 렌더링한 후 각 픽셀에서 해당 속도를 따라 이웃 샘플을 누적하는 방식이 있습니다. 이러한 방법은 객체 경계가 날카롭고, 배경이 항상 흐려지는 등의 문제를 가지고 있습니다. 속도 확장 방법은 속도 버퍼를 확장하여 이러한 문제를 일부 해결하려고 했지만, 배경을 과도하게 흐리게 만드는 등의 문제를 가지고 있습니다. 예를 들어, Green의 방법은 속도 렌더링 시 객체를 확장하여 객체 경계를 부드럽게 하지만, 배경을 과도하게 흐리게 만듭니다. Lost Planet의 블러 방법은 속도 버퍼를 확장하여 배경을 흐리게 하지만, 객체의 이동 방향을 왜곡할 수 있습니다.

### 1.2 From Offline-Smart to Real-Time-Plausible Reconstruction

우리는 기존의 스마트 재구성 방법의 두 가지 핵심 아이디어를 사용하여 적은 수의 필터 탭을 통해 모션 블러를 재구성하는 새로운 방법을 개발했습니다. Shirley et al.의 스마트 재구성 방법은 픽셀당 많은 확률적 샘플이 필요하고 깊이 순서대로 정렬해야 하므로 현재의 래스터화 하드웨어에서 적용하기 어렵습니다. 그러나 우리는 그들의 기술의 두 가지 핵심 아이디어가 조밀하거나 확률적 입력을 필요로 하지 않는다고 관찰했습니다: 1) 픽셀 외부의 샘플을 고려합니다. 2) 샘플을 정적(즉, 선명한) 대 움직이는(즉, 흐릿한) 및 전경 대 배경의 두 가지 축으로 분류하고 결합합니다. 우리의 새로운 방법은 이러한 단계를 연속적이고 순서 독립적인 샘플 분류로 구현하여 모션 블러를 재구성합니다. 이를 통해 정렬 없이 상대적으로 적은 수의 필터 탭을 사용하여 모션 블러를 재구성할 수 있습니다.

![세 가지 다른 전략으로 수직 속도를 가진 빨간 로켓선을 흐리게 하는 방법: (d) 선형-산포, (e) 보수적 원형-모음, (g) 알고리즘에서 사용된 휴리스틱 선형-모음. 청색 화살표는 속도를 나타내고, 황색 화살표는 X에서 계산된 최종 색상에 영향을 미치는 메모리 작업을 나타내며, 이는 Y의 초기 색상에 따라 달라집니다. 이 예에서 k = 2입니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%201.png)

세 가지 다른 전략으로 수직 속도를 가진 빨간 로켓선을 흐리게 하는 방법: (d) 선형-산포, (e) 보수적 원형-모음, (g) 알고리즘에서 사용된 휴리스틱 선형-모음. 청색 화살표는 속도를 나타내고, 황색 화살표는 X에서 계산된 최종 색상에 영향을 미치는 메모리 작업을 나타내며, 이는 Y의 초기 색상에 따라 달라집니다. 이 예에서 k = 2입니다.

## 2 Algorithm

### 2.1 Overview

블러 알고리즘의 구조는 입력과 출력 버퍼의 크기 \( n = w \times h \) 에 따라 구성됩니다. 입력 이미지 \( C \)는 전통적인 래스터화에 의해 렌더링된 순간적인 색상 이미지입니다. 화면 속도 버퍼 \( V \)는 각 점이 이전 프레임에서 투영되었던 위치에서 현재 프레임으로의 픽셀 오프셋을 인코딩하며, 깊이 버퍼 \( Z \)는 카메라 공간의 깊이를 저장합니다. 블러 알고리즘은 \( V \)로부터 두 개의 작은 중간 버퍼를 생성한 다음, 이러한 버퍼들과 모든 입력 버퍼를 사용하여 출력을 재구성합니다. 재구성 필터는 특정 애플리케이션에 따라 최대 모션 블러 반경 \( k \) 픽셀과 \( S \) 재구성 필터 샘플 탭의 매개변수를 가집니다. 우리는 시청자와 객체 사이의 예상 거리, 객체의 속도, 노출 시간 등을 기반으로 \( k \)를 선택합니다. 실험 결과는 본 논문 3장에서 다룹니다.

![ 블러 알고리즘을 위한 세 가지 입력 버퍼와 세 가지 필터 패스의 시스템 다이어그램.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%202.png)

 블러 알고리즘을 위한 세 가지 입력 버퍼와 세 가지 필터 패스의 시스템 다이어그램.

### 2.2 Buffers

입력 색상 버퍼 \( C \)는 톤 매핑 또는 감마 인코딩 전에 고동적 범위의 프레임 버퍼입니다. 우리는 모션 블러 후에 블룸, 톤 매핑, FXAA 같은 전형적인 게임 후처리 과정을 추가합니다. MSAA를 사용하는 경우, \( C \)는 픽셀당 하나의 샘플을 가진 포스트 리졸브 버퍼여야 합니다. 향후에는 MSAA 샘플을 직접 읽어 에지에서의 이미지 품질을 높이고 노이즈를 줄일 수 있는 블러 알고리즘 변형을 상상할 수 있습니다.

우리는 G3D 라이브러리의 SS GBuffer 셰이더를 사용하여 속도와 선형 깊이 버퍼를 생성합니다. 방사형 속도는 노출 시간에 비례하여 클램프되고 최대 \( k \)로 제한됩니다. 예를 들어, 픽셀 \( X \)가 이전 프레임에서 \( X' \)로 이동했을 때, \( V[X] \)는 다음과 같이 계산됩니다:
\[ V[X] = \frac{1}{2} (X - X') \cdot (\text{노출 시간}) \cdot (\text{프레임 속도}) \]
이 값을 \( GL\_RG8 \) 버퍼에 저장하고, 콘솔에서는 메모리 절약을 위해 낮은 해상도로 렌더링합니다. 우리는 \( Z \)를 카메라 공간의 깊이 버퍼로 구현합니다. 모든 데이터 버퍼의 결합 크기는 720p 해상도에서 15MB입니다.

### 2.3 Filter Passes

알고리즘은 세 가지 2D 패스를 통해 타일 단위의 주요 속도를 계산하고, 이를 사용하여 각 픽셀의 모션 블러를 재구성합니다. 첫 번째 패스는 타일 단위로 주요 속도를 계산하여 \( TileMax \) 버퍼에 저장합니다. 두 번째 패스는 인접한 타일 내의 최대 속도를 계산하여 \( NeighborMax \) 버퍼에 저장합니다. 세 번째 패스는 전체 해상도에서 재구성 필터를 적용합니다. 각 픽셀 \( X \)에서, 주요 속도 벡터 \( \vec{v_N} \)를 따라 이웃 샘플 \( Y \)의 기여도를 계산합니다.

재구성 함수는 주요 속도 벡터 \( \vec{v_N} \)을 사용하여 주변 픽셀의 기여도를 계산하며, 세 가지 주요 경우를 다룹니다: 1) 흐릿한 \( Y \)가 \( X \) 앞에 있는 경우, 2) 흐릿한 \( X \) 뒤에 있는 배경을 추정하는 경우, 3) \( X \)와 \( Y \)가 서로의 속도 범위 내에서 흐릿한 경우입니다. 이 함수는 연속적이고 순서 독립적인 분류를 통해 노이즈를 줄이고, 상대적으로 적은 수의 샘플 탭을 사용하여 정확한 모션 블러를 재구성합니다.

## 3 Results

제안된 새로운 블러 알고리즘은 기존의 실시간 가능한 모션 블러 방법과 비교하여 뛰어난 품질을 보여줍니다. 여러 실험을 통해 다양한 장면에서 알고리즘의 성능과 시각적 품질을 검증했습니다.

![연속 분류 필터. cone: X가 Y의 점-확산 함수 내에 있는가? softDepthCompare: zb가 za보다 가까운가?](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%203.png)

연속 분류 필터. cone: X가 Y의 점-확산 함수 내에 있는가? softDepthCompare: zb가 za보다 가까운가?

먼저, 새로운 블러 알고리즘은 기존 방법들이 갖고 있는 문제들을 효과적으로 해결합니다. 그림 5는 모션 블러 알고리즘을 여러 가지 장면에 적용한 결과를 보여줍니다. 첫 번째 행은 필터링되지 않은 입력 이미지를, 두 번째 행은 확장되지 않은 속도 벡터를 따라 샘플을 수집한 결과를, 세 번째 행은 확장된 속도 버퍼를 사용한 결과를 보여줍니다. 마지막 행은 새로운 알고리즘을 적용한 결과로, 움직이는 객체가 원래 실루엣을 넘어 흐릿해지고, 객체 뒤의 배경이 선명하게 유지되며, 모든 이동 경계가 흐릿해집니다.

![실시간 모션 블러 방법의 비교. 큐브가 격자 벽이 있는 방의 두 상자 사이를 대각선으로 이동합니다. 삽입된 상자는 움직이는 객체의 경계 근처에서 중요한 전환 영역을 보여줍니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%204.png)

실시간 모션 블러 방법의 비교. 큐브가 격자 벽이 있는 방의 두 상자 사이를 대각선으로 이동합니다. 삽입된 상자는 움직이는 객체의 경계 근처에서 중요한 전환 영역을 보여줍니다.

그림 6은 큰 카메라 움직임으로 인한 모션 블러를 보여줍니다. 이 알고리즘은 카메라 움직임과 객체 움직임을 구분하지 않으며, 모두 화면 공간에서 다른 위치로 투영되는 씬 포인트로 처리합니다. 그림 상단은 화물칸 내부에서 카메라가 회전하는 장면으로, 주로 수평 블러가 발생합니다. 하단은 카메라가 Crytek Sponza 아트리움을 앞으로 이동하는 장면으로, 깊이에 따라 변하는 방사형 블러가 발생합니다.

![카메라 움직임 결과로, 왼쪽에 축소된 크기로 입력 버퍼가 표시됩니다. 상단: 수직 축을 중심으로 회전. 하단: 뷰 벡터를 따라 이동.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%205.png)

카메라 움직임 결과로, 왼쪽에 축소된 크기로 입력 버퍼가 표시됩니다. 상단: 수직 축을 중심으로 회전. 하단: 뷰 벡터를 따라 이동.

그림 7은 다른 속도와 방향으로 회전하는 맞물린 기어를 보여줍니다. 이 장면은 각 타일이 단일 속도 벡터에 의해 지배된다는 가정이 성립하지 않아 알고리즘이 어려움을 겪는 경우입니다. 결과적으로, 오른쪽 상단 기어의 내부 스포크에서 언더블러 현상이 관찰됩니다. 이는 배경의 큰 기어가 오른쪽 상단에서 NeighborMax를 지배하고 스포크 속도를 억제하기 때문입니다. 이 그림은 또한 결과 이미지의 노이즈에 대한 S 값의 영향을 보여줍니다. 콘솔에서는 S=5가 충분하지만, 더 정밀한 모니터를 사용하는 데스크탑 그래픽에서는 S=15가 이미지 품질을 향상시킵니다.

![재구성 필터에서 샘플 수 S를 다양하게 하여 이미지 평면에서 회전하는 중첩된 요소가 있는 장면. 하단 행은 큰 이미지의 좌상단에서 확대된 세부 사항을 보여줍니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%206.png)

재구성 필터에서 샘플 수 S를 다양하게 하여 이미지 평면에서 회전하는 중첩된 요소가 있는 장면. 하단 행은 큰 이미지의 좌상단에서 확대된 세부 사항을 보여줍니다.

그림 8은 스포츠카의 원본 이미지와 두 가지 다른 속도 버퍼로 렌더링된 결과를 보여줍니다. 가운데 결과는 카메라가 차 옆을 추적하는 경우로, 배경과 도로가 흐릿해집니다. 하단 결과는 정적 카메라로 렌더링된 경우로, 자동차가 흐릿해집니다. 이 장면들은 객체 움직임과 카메라 움직임의 차이를 잘 보여줍니다.

![상단: 입력 이미지; 블러 없음. 중간: 카메라가 차를 추적하는 경우; 배경과 도로가 흐릿하게 나타납니다. 하단: 월드 공간에서 카메라 움직임 없음; 차가 흐릿하게 나타납니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%207.png)

상단: 입력 이미지; 블러 없음. 중간: 카메라가 차를 추적하는 경우; 배경과 도로가 흐릿하게 나타납니다. 하단: 월드 공간에서 카메라 움직임 없음; 차가 흐릿하게 나타납니다.

그림 9는 게임 캐릭터를 인터랙티브하게 조작할 때의 프레임 시퀀스를 보여줍니다. 픽셀 단위의 속도가 캐릭터의 신체 전체에 걸쳐 적절하게 변하고, 순서 독립적인 필터 기여가 캐릭터가 자신을 겹치거나 장면과 겹칠 때 올바르게 혼합됩니다.

![인터랙티브 캐릭터와 카메라 애니메이션(S = 5)의 인게임 렌더링 프레임. 복잡한 애니메이션 결과로 단일 프레임 내에서 캐릭터 전체에 걸쳐 속도와 블러가 적절히 변합니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%208.png)

인터랙티브 캐릭터와 카메라 애니메이션(S = 5)의 인게임 렌더링 프레임. 복잡한 애니메이션 결과로 단일 프레임 내에서 캐릭터 전체에 걸쳐 속도와 블러가 적절히 변합니다.

알고리즘의 성능 측정 결과, GeForce 480에서 1280×720 해상도로 카메라 움직임으로 인해 모든 픽셀이 블러링될 때, k=31인 경우 6.2ms, k=15인 경우 3.0ms, k=7인 경우 2.8ms, k=5인 경우 2.7ms가 소요되었습니다. Xbox 360에서는 k=5인 경우 1.5ms가 소요되었습니다. 이러한 결과는 알고리즘이 현재의 하드웨어에서 실시간으로 작동할 수 있음을 보여줍니다.

## 4 Discussion

### 4.1 Artistic Controls

예술적 제어는 제작 특수 효과에 필수적입니다. 이미지 공간에서 작업함으로써 물리적으로 정확한 속도 버퍼에서 벗어나 예술적인 목적으로 조정하는 것이 상대적으로 용이합니다. 예를 들어, 기하학적 객체와 입자를 속도 버퍼에 직접 렌더링하여 이미지에 스머징 효과를 추가할 수 있습니다. 이는 Lost Planet 2에서 강한 폭발을 렌더링하는 데 사용되었습니다. 또한 열 왜곡, 큰 소리 스피커, 반투명한 "은폐" 캐릭터를 모방하는 데 적용될 수 있습니다.

!["속도 입자"는 폭발과 같은 특수 효과를 위해 속도 버퍼에만 렌더링됩니다.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%209.png)

"속도 입자"는 폭발과 같은 특수 효과를 위해 속도 버퍼에만 렌더링됩니다.

빠른 움직임을 강조하기 위해 입자 효과와 캐릭터 팔다리를 과도하게 흐리게 하거나, 눈에 자연스럽게 초점을 맞출 표지판과 같은 특정 객체는 덜 흐리게 하는 것이 종종 바람직합니다. 객체는 메시 상수나 속도-크기 텍스처 맵을 사용하여 렌더링되는 속도의 크기를 변경할 수 있습니다.

사람의 시선은 프레임 내에서 빠르게 이동하며 예측하기 어렵기 때문에, 예를 들어, 실제 생활에서 회전하는 사람은 반경 방향으로 바라보지 않고 고정된 씬 포인트 사이를 빠르게 이동합니다. 카메라가 큰, 천천히 변하는 속도의 객체를 추적하지 않는 한, 카메라 움직임의 중요성을 줄이는 것이 좋습니다. 이를 위해 각 프레임에서 객체의 위치를 사용하여 이전 프레임과 현재 프레임 사이에서 카메라 변환을 보간하여 \( X_0 \)를 계산합니다. 회전에는 slerp, 번역에는 lerp를 사용합니다. 우리는 제3자 시점에서는 이전 카메라의 0-15%만 사용하고, 낙하나 점프 패드와 같은 고속 특수 상황에서는 이를 증가시키는 것을 권장합니다.

![부가적인 버퍼와 함께 레이 트레이싱된 이미지를 후처리하는 시연.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%2010.png)

부가적인 버퍼와 함께 레이 트레이싱된 이미지를 후처리하는 시연.

![손으로 그린 깊이와 속도 버퍼(S = 9)를 사용하여 블러 처리된 실제 당구공 사진.](A%20Reconstruction%20Filter%20for%20Plausible%20Motion%20Blur%20bbb49d548c504b78af2ea73ab7dd5a67/Untitled%2011.png)

손으로 그린 깊이와 속도 버퍼(S = 9)를 사용하여 블러 처리된 실제 당구공 사진.

### 4.2 Limitations

입력이 전통적인 방식으로 렌더링된 이미지이기 때문에, 블러 알고리즘은 노출 시간 내의 단일 순간에 조명을 고정합니다. 이는 많은 모션 블러 기술이 갖는 일반적인 한계입니다. 오프라인 방법과 비교하여, 우리의 방법은 객체의 움직임에 따라 음영과 그림자를 흐리게 하며, 이는 반사나 그림자 캐스터의 움직임에 기반한 재구성 방법을 조사했지만, 실제 영역-빛의 반음영 효과가 작기 때문에 시각적 영향이 적었습니다.

또한, 새로운 방법은 모든 객체의 속도가 선형이라고 가정합니다. 실험 결과, 캐릭터와 회전 운동의 경우에도 이 가정으로 인한 시각적 오류의 영향은 제한적이었지만, 큰 비선형 운동을 저해상도로 렌더링할 경우 이미지 품질이 저하될 수 있습니다.

화면 공간 필터와 마찬가지로, 블러는 뷰포트 주위에 폭 \( k \)의 보호 대역을 필요로 하지만, 콘솔에서 렌더링된 이미지를 자르기 때문에 실제로는 보호 대역을 사용하지 않습니다.

### 4.3 Future Work

게임과 같은 응용 프로그램을 위해, 다음 단계는 한 지역 내에서 매우 다양한 속도를 처리하고 모션 블러와 초점 흐림을 결합하는 것입니다. 주된 속도 가정이 위반될 때 발생하는 아티팩트는 카메라 회전이 정적 요소에 대해 2D 속도를 생성하여 동적 요소와 직각을 이룰 때 가장 큽니다.

여러 속도를 지원하기 위해 \( V[X] \)와 \( \vec{v_N} \)을 따라 샘플링하거나, 한 지역 내에서 두 번째 주요 구성 요소를 추적하는 등의 여러 가능성이 있습니다.

가능한 초점 흐림 방법은 이미 단독으로 매우 효과적입니다. 모션 블러와 초점 흐림을 동시에 또는 순차적으로 적용할지, 어떤 방법을 사용할지가 열린 질문입니다.

하드웨어에서 구현된다면, 확률적 래스터화와 MSAA는 재구성 알고리즘에 훨씬 더 나은 입력을 제공할 수 있습니다. 이를 그 맥락에서 확장하는 것은 특정 래스터화 알고리즘과 샘플 패턴에 의해 형성될 흥미로운 도전 과제가 될 것입니다.