# Remesh

[Instant Field-Aligned Meshes](3/Instant%20Field-Aligned%20Meshes%20d27f76722c104a0f80a6f2bebd24efa8)

컴퓨터 그래픽스와 CAD 애플리케이션에 사용되는 메시 생성 방법에 대한 새로운 접근법을 제시합니다. 이 방법은 빠르고 효율적으로 대규모 데이터 세트를 처리할 수 있으며, 지역적인 스무딩 연산자에 기반을 둔 새로운 접근법을 사용합니다. 본 연구는 방향 필드와 위치 필드를 최적화하여 높은 등방성과 정확한 위치를 갖는 메시를 생성합니다. 이 알고리즘은 중간 크기의 데이터셋을 1초 미만으로 처리할 수 있으며, 가장 큰 데이터셋도 10분 이내에 처리하는 효율성을 보여줍니다. 이 방법은 대화식 브러시 도구를 통해 사용자가 특이점의 위치와 수를 제어할 수 있도록 하며, 이는 실시간으로 시각화되어 빠른 디자인 반복을 가능하게 합니다.

[Dev2PQ: Planar Quadrilateral Strip Remeshing of Developable Surfaces](3/Dev2PQ%20Planar%20Quadrilateral%20Strip%20Remeshing%20of%20Dev%203c3281f2c4f44d6fa9e71351783073a4)

개발 가능한 표면의 삼각형 메시를 평면형 면을 가진 다각형 메시(PQ 메시)로 변환하는 새로운 알고리즘을 소개합니다. 이 알고리즘은 개발 가능한 모델링과 제작에 중요하며, 실험을 통해 평탄도 오차 측정으로 성공적인 변환을 입증합니다. 그러나 노이즈가 많은 입력과 얇은 피처 처리에 한계가 있으며, 향후 개선 사항으로는 개발할 수 없는 표면에 대한 적용, 자동 파라미터 조정, 국부 곡률에 따른 출력 메시 해상도 조정 등이 있습니다.

[Data-Driven Interactive Quadrangulation](3/Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873)

3D 모델링 분야에서 쿼드 레이아웃을 생성하는 새로운 알고리즘을 제시합니다. 이 알고리즘은 기존 쿼드 메시에서 학습한 패턴을 사용하여, 사용자가 3D 모델에 스케치한 경계와 에지 흐름 제약 조건에 맞춰 자동으로 테셀레이션을 수행합니다. 비공식적인 사용자 연구를 통해 효율성과 아티스트들의 긍정적인 반응을 확인했지만, 복잡하고 다양한 패치에 관한 한계도 인정하며, 추가 개발의 여지를 남깁니다.

[Pattern-Based Quadrangulation for N-Sided Patches](3/Pattern-Based%20Quadrangulation%20for%20N-Sided%20Patches%2040811a4097d049cd981ed32526c3eae6)

N면 패치를 효율적으로 사변형 메시로 변환하는 새로운 알고리즘을 소개합니다. 이 알고리즘은 OpenMesh 프레임워크와 lp_solve를 활용하여 개발되었으며, 테스트 결과 기존 방법보다 빠르고 고품질의 결과를 제공합니다. 하지만, 현재 알고리즘은 사전 정의된 패턴을 사용하여 제한된 범위의 사변형만 처리할 수 있으며, 향후 연구에서는 이 범위를 확장하고 기하학적 최적화 방법을 개선하는 것이 제안됩니다.

[Topology-based Smoothing of 2D Scalar Fields with C1-Continuity: Derivatives of f](3/Topology-based%20Smoothing%20of%202D%20Scalar%20Fields%20with%20%206391cb1e9a4c49488053767a0d0726c3)

2차원 스칼라 필드의 평활화 방법을 제시하며, 이 과정에서 필드의 위상학적 특징을 보존합니다. 주요 접근 방식은 이산 위상 단순화와 제약된 바이 라플라시안 최적화를 결합하는 것으로, 이를 통해 출력의 위상 구조가 유지됩니다. 저자들은 유체 역학 시뮬레이션과 입자 이미지 속도 측정 데이터를 포함한 다양한 분야에서 이 방법의 효과를 입증합니다. 결과적으로, 이 논문은 2D 스칼라 필드를 처리하는 새롭고 효과적인 방법을 제시하며, 향후 연구에서는 이상값 식별과 모스 셀 내 다양한 단조성 설명의 영향을 탐구할 것임을 밝힙니다.

[A Parametric Space Approach to the Computation of Multi-Scale Geometric Features](3/A%20Parametric%20Space%20Approach%20to%20the%20Computation%20of%20%2016a49e2681eb45449abf918e94d1751f)

3D 객체의 기하학적 특성을 실시간으로 계산하는 새로운 파라메트릭 공간 접근 방식을 제시합니다. 저자들은 기존의 CPU 기반 방법과 달리, 객체의 기하학적 데이터와 추가 인접 정보를 2차원 레이아웃으로 전환함으로써, 특정 지점의 주변 지역을 즉시 재구성할 수 있는 방법을 개발했습니다. 이 접근법은 GPU 및 멀티코어 컴퓨팅과 호환되어, 변형 가능하거나 애니메이션이 적용된 객체에서도 실시간 계산이 가능합니다. 이 연구는 객체 매칭 및 검색과 같은 다양한 애플리케이션에 유용할 것으로 보입니다.

[Globally Optimal Direction Fields](3/Globally%20Optimal%20Direction%20Fields%20ac05e8e0c4c4472e84bacba2574d2488)

표면의 n방향 필드 생성에 관한 연구입니다. 이 연구는 표면에 가능한 최소 에너지를 갖는 최적의 n방향 필드를 찾는 것을 목표로 합니다. 여기서 '전역'은 국부적 최소값이 아닌, 가능한 모든 구성 중 최적의 해를 의미합니다. 연구의 핵심은 n방향 필드가 표면의 각 점에 고르게 분포된 단위 탄젠트 벡터 집합을 할당하며, 이 필드의 평활도와 정렬을 측정합니다. 실험적으로 이 방법은 최첨단 기술보다 효율적이며, 동일하거나 더 나은 품질의 필드를 생성합니다. 이 방법은 완전히 자동적이며, 주요 곡률 방향과 같은 주어진 가이드 필드에 정렬된 필드를 생성할 수 있습니다.

[Regularization of voxel art](3/Regularization%20of%20voxel%20art%20cdc9fe1465764701bf1f982cba99e67d)

3D 복셀 아트를 부드럽게 재구성하는 방법을 제시합니다. 이 연구는 3D 픽셀인 복셀을 사용하여 객체를 렌더링하는 컴퓨터 그래픽의 한 기법에 초점을 맞춥니다. 저자들은 저해상도 오브젝트를 더 부드럽고 세밀하게 보이게 만드는 새로운 정규화 방법을 소개합니다. 이 방법은 복셀 모델의 거친 표면을 매끄럽게 하여, 보다 현실적인 렌더링 결과를 제공합니다. 이 과정에서 수학적 공식을 사용하여 복셀 아트의 블록성을 줄이고, 원래 모양을 유지하면서 표면을 최대한 부드럽게 만들려고 합니다. 이 연구는 특히 간단한 모델로 시작하여 시간이 지남에 따라 모델을 다듬으려는 게임 디자이너나 아티스트에게 유용할 수 있습니다.

[Design-driven quadrangulation of closed 3D curves](3/Design-driven%20quadrangulation%20of%20closed%203D%20curves%2045bd9404aa164a559c27397556dba701)

닫힌 3D 커브를 기반으로 한 사변형 메시 생성 방법을 소개합니다. 이 방법은 커브의 지오메트리를 분석하여, 디자이너의 의도를 반영하는 사변형 네트워크를 형성합니다. 이 과정은 두 가지 주요 단계로 이루어집니다: 커브 세그먼트의 이중 그래프 생성과 사변형 네트워크의 구성. 연구는 3D 디자인 커브 네트워크를 보간하는 가상의 표면을 구성하는 첫 번째 솔루션을 제시하며, 결과물은 디자이너의 기대와 시청자의 인식에 부합해야 합니다. 이 알고리즘은 닫힌 커브와 커브 네트워크에서 테스트되었으며, 정량적 평가를 통해 그 성능이 입증되었습니다.

[Practical quad mesh simplification](3/Practical%20quad%20mesh%20simplification%20f5d952b7f97d4014904ccfa34ede319a)

컴퓨터 그래픽 분야에서 쿼드 메시 단순화의 복잡성에 대한 새로운 접근 방식을 제시합니다. 쿼드 메시, 즉 사각형으로만 이루어진 메시는 모델링, 시뮬레이션, 렌더링 등 다양한 애플리케이션에서 널리 사용되지만, 기존 연구에서는 충분히 다루어지지 않았습니다. 이 논문은 볼록하고, 평평하며, 직각의 쿼드로 구성된 메시 생성을 목표로 하며, 새로운 로컬 연산자 집합을 통해 쿼드 구조를 보존하고 선 특징과 적응형 샘플링을 유지합니다. 이 접근 방식은 쿼드 메시의 품질을 극대화하고 다양한 테셀레이션 밀도를 생성하는 데 효율적이며, 새로운 트라이앵글-쿼드 변환 알고리즘을 통해 우수한 테셀레이션 품질을 보장합니다.

[2D quad mesh generation using divide and conquer poly-square maps](3/2D%20quad%20mesh%20generation%20using%20divide%20and%20conquer%20p%20b560341fdfbe4b5f89277af9582c5583)

성능 컴퓨팅을 활용하여 복잡한 2D 기하학적 영역에 대한 구조화된 사각형 메시를 생성하는 새로운 알고리즘을 제시합니다. 연구팀은 분산 및 병렬 계산을 사용하여 대규모 데이터를 효율적으로 처리하는 '분할 및 정복' 전략을 채택했습니다. 이 접근법은 복잡한 지형에 대한 고품질의 사각형 메시를 생성하며, 특히 경계 근처에서 메시 품질 저하와 특이점의 존재를 최소화합니다. 이 연구는 컴퓨터 그래픽과 과학적 계산 분야에서 사각형 메시 생성의 효율성과 품질을 향상시키는 데 기여합니다.

[Quad-Mesh Generation and Processing:a survey](3/Quad-Mesh%20Generation%20and%20Processing%20a%20survey%208e33f7e213bd402587dc810fbd4d9742)

컴퓨터 그래픽, 기계 공학, 건축 등의 분야에서 중요한 사각형 메쉬 생성 및 처리에 관한 종합적인 개관을 제공합니다. 쿼드 메쉬는 그 구조적 특징으로 인해 다양한 애플리케이션에서 선호되며, CAD, 시뮬레이션, 텍스처링 등 다양한 목적에 사용됩니다. 이 논문은 쿼드 메쉬의 다양한 특성과 생성 방법, 그리고 이에 따른 처리 방법을 상세히 논의합니다. 쿼드 메쉬 생성은 트라이-쿼드 변환, 표면 위의 패치 정의, 파라메터화 기반 방법 등을 포함하며, 이러한 방법들은 각기 다른 측면에서 쿼드 메쉬의 질과 효율성을 향상시킵니다. 이러한 접근법들은 쿼드 메쉬의 생성과 처리에 있어 중요한 발전을 나타내지만, 여전히 해결해야 할 과제들이 많이 남아있습니다.

[Integer-Grid Maps for Reliable Quad Meshing](3/Integer-Grid%20Maps%20for%20Reliable%20Quad%20Meshing%20f24a98a8bf9f4ee09b036930a3b6b7bd)

삼각형 메시를 사변형 메시로 변환하는 새로운 방법론을 제시하는 논문입니다. 이 방법은 특이점의 정수 위치를 결정하고, 결과 메시의 품질을 최적화하기 위해 혼합 정수 이차 프로그래밍과 복잡성 감소 알고리즘을 사용합니다. 주요 목표는 퇴화 없는 일관된 매개변수화를 제공하고, 사각형 메쉬 생성에 있어서의 신뢰성과 효율성을 향상시키는 것입니다. 이 방법은 다양한 크기와 복잡도를 가진 메시에 적용 가능하며, 특히 애니메이션과 시뮬레이션에서의 쿼드 메시 생성에 유용합니다.

[QuadCover - Surface Parameterization using Branched Coverings](3/QuadCover%20-%20Surface%20Parameterization%20using%20Branche%209f6bdc80c94f40f784b5a031b5389c54)

2D 단순 곡면의 전역 파라미터화를 위한 자동화된 방법을 제안합니다. 이 연구는 리만의 로컬 차트 개념을 활용하여, 사용자 정의 프레임 필드에 의해 안내되는 파라미터화를 생성합니다. 알고리즘은 서페이스의 토폴로지를 고려하여 통합 가능한 필드를 구성하며, 프레임 필드의 분기점은 분기 커버링 스페이스를 사용하여 해결합니다. 이 방법은 기존의 표면 파라미터화, 쿼드 메시 생성, 사각 리메싱 등의 연구를 기반으로 하며, 텍스처 매핑, 이미지 처리 알고리즘 확장, 리메싱 등 다양한 응용 분야에 적용될 수 있음을 시사합니다. 이 논문은 표면의 프레임 필드와 분기된 커버링 공간과의 관계에 대한 새로운 이론적 프레임워크를 제시하며, 최적의 파라미터화를 위한 국부적으로 통합 가능한 프레임 필드의 사용 방법을 소개합니다.

[Mixed-Integer Quadrangulation](3/Mixed-Integer%20Quadrangulation%204efe9a7e812e4e4dabbb5224338e0a57)

비정형 삼각형 메시에서 고품질 쿼드 메시를 생성하는 과정을 다룬 논문입니다. 이 기술은 원시 지오메트리 데이터를 텍스처링 및 모양 수정과 같은 작업에 더 적합한 형식으로 변환합니다. 논문은 이상적인 특이점 위치를 찾고 쿼드랭귤레이션의 전역 구조를 최적화하는 것에 초점을 맞춥니다. 크로스 필드를 부드럽게 보간하여 전역 매개변수화를 생성하고, 이를 통해 품질 기준을 충족하는 사각화를 구축합니다. 이 방법은 기하학적으로 의미 있는 위치에 특이점을 자동으로 배치하고 다양한 제약 조건을 충족하는 매끄러운 쿼드 메시를 생성하는 데 효과적입니다.

[Trivial Connections on Discrete Surfaces](3/Trivial%20Connections%20on%20Discrete%20Surfaces%2093a3c013909f4cfc92efacb5028e7097)

컴퓨터 그래픽에서 표면의 다른 점들 사이의 접선 방향을 비교하는 새로운 방법을 제시합니다. 이 연구는 벡터 필드 평활화, 표면 텍스처링, 메시 변형 등의 애플리케이션에 중요한 역할을 하는, 접선 공간을 다른 접선 공간에 매핑하는 과정을 다룹니다. 저자들은 효율적이고 계산하기 쉬운 알고리즘을 통해 한 지점에서 다른 지점으로 벡터를 일관되게 매핑하는 '연결'을 생성합니다. 이 알고리즘은 경계가 있든 없든 임의의 속성을 가진 단순한 표면에서 작동하며, 글로벌 최적을 제공하고 원치 않는 특이점 생성을 방지합니다.

[Boundary First Flattening](3/Boundary%20First%20Flattening%20f98c81ef41b641c084ac2b65b59422b9)

컨포멀 평탄화 과정을 효과적으로 제어하는 새로운 방법, BFF(Boundary First Flattening)를 소개합니다. 이 방법은 행렬 인수분해를 통해 최종 모양의 완벽한 제어를 가능하게 하며, 영역 왜곡을 최소화하고, 경계 길이와 각도를 조작할 수 있는 기능을 제공합니다. BFF는 복잡한 대형 메시에서도 사용자가 대화형으로 맵을 편집하거나 최적화할 수 있는 새로운 컨포멀 파라미터화 방식을 제공합니다. 이 방법은 기존 LSCM 방법의 대안으로 사용되며, 모양 및 영역 왜곡을 더 잘 제어할 수 있습니다. BFF 프로세스는 경계 곡선을 생성하고 이를 도메인 내부로 확장하는 세 단계로 구성되며, 이러한 계산은 주로 희소 선형 문제를 해결함으로써 이루어집니다.

[Rectangular Multi-Chart Geometry Images](3/Rectangular%20Multi-Chart%20Geometry%20Images%205bddf5b2692a4e18b872bd755757fca4)

다면체 표면에서 저왜곡의 직사각형 차트를 생성하는 새로운 방법을 제시합니다. 이 방법은 표면을 준직사각형 클러스터로 분해하여 픽셀 또는 '텍셀'을 직사각형 차트에 매핑합니다. 이를 통해 이미지 처리 연산을 사용하여 표면 PDE(편미분 방정식)를 효율적으로 해결할 수 있습니다. 또한, 이 방법은 매개변수화 거리를 사용하여 클러스터 형성을 안내하고, T-정션과 자기 이웃 차트를 허용하여 왜곡을 최소화합니다. 이 접근법은 직사각형 클러스터에 대한 반복 클러스터링과 차트 매개변수화 과정을 포함하며, 이를 통해 다양한 표면 처리 애플리케이션에 적용될 수 있음을 보여줍니다.

[Least Squares Conformal Maps for Automatic Texture Atlas Generation](3/Least%20Squares%20Conformal%20Maps%20for%20Automatic%20Texture%207cd3b77d03144573a4c8992b6d1aa204)

3D 모델의 텍스처 아틀라스 생성을 위한 새로운 방법을 제시합니다. 이 방법은 최소 제곱 컨포멀 맵(LSCM)을 사용하여 3D 표면을 2D 도메인에 매핑하면서 각도 변형과 불균일한 스케일링을 최소화합니다. 이 접근법은 복잡한 경계와 큰 삼각형이 있는 모델에서도 견고하게 작동하며, 각도를 국부적으로 보존하는 '컨포멀' 매핑을 찾는 것을 목표로 합니다. 이 논문은 텍스처 아틀라스 생성을 위해 차트를 자연스럽게 생성하고, 텍스처 공간에 정확하게 매핑하는 방법을 소개하며, 스캔한 메시와 모델링한 메시 모두에 효과적인 결과를 보여줍니다.

[Flexible Isosurface Extraction for Gradient-Based Mesh Optimization](3/Flexible%20Isosurface%20Extraction%20for%20Gradient-Based%20%20f2bede583dac4cef86c86bee5b1b3d22)

'플렉시큐브(FlexiCubes)'라는 새로운 3D 형상 최적화 기술을 소개합니다. 이 기술은 메시 추출에 더 많은 유연성을 제공하며, 그 결과는 런타임과 메모리 사용량이 소폭 증가하는 것으로 나타납니다. 그러나 이는 전체 계산 과정에서 크게 중요하지 않은 수준입니다. 플렉시큐브는 유연한 이중 표현을 사용하는데, 이로 인해 자체 교차하지 않는 출력을 보장하지 않으며 메시에서 불연속성이 발생할 가능성이 있습니다. 이 방법은 사진 측량, 애니메이션 오브젝트의 메시 단순화, 3D 메시 생성, 미분 가능한 물리 시뮬레이션 등 다양한 분야에 성공적으로 적용되었습니다.

[POLYGON OFFSETTING BY COMPUTING WINDING NUMBERS](3/POLYGON%20OFFSETTING%20BY%20COMPUTING%20WINDING%20NUMBERS%20e9d49424a70c406bb6b494954361fe0e)

컴퓨터 지원 설계(CAD) 및 제조(CAM)에서 중요한 기술인 폴리곤 오프셋에 대한 새로운 접근 방식을 제시합니다. 이 연구는 기존의 오프셋 방법과 달리 다각형의 모든 모서리를 오프셋하고 원형 호로 간격을 좁히는 대신, 와인딩 수를 계산하여 오프셋 영역을 결정하는 방법을 소개합니다. 이 방법은 겹치지 않는 다각형을 만들고 원치 않는 루프를 제거함으로써, 보다 효율적이고 신뢰할 수 있는 결과를 제공합니다. 이 알고리즘은 OpenGL 유틸리티 라이브러리를 사용하며, 복잡성과 공간 효율성 측면에서 상업적 대안보다 우수합니다.

[Reach For the Spheres: Tangency-Aware Surface Reconstruction of SDFs](3/Reach%20For%20the%20Spheres%20Tangency-Aware%20Surface%20Recon%20693e9399b5964776b7f294ef317604e0)

SDF(이산 부호 거리 필드) 데이터를 활용하여 3D 모델을 정밀하게 재구성하는 새로운 방법을 제시합니다. 기존 방법들이 저해상도에서 거친 형상을 생성하는 데 반해, 이 방법은 SDF 내의 각 데이터 포인트가 구를 상징하며, 모델링되는 실제 표면은 이러한 각 구와 최소 한 번씩 접촉해야 한다는 기하학적 인사이트를 바탕으로 합니다. 이 방법은 SDF 데이터의 기하학적 정보를 최대한 활용하여, 머신 러닝에 의존하지 않고도 더 세밀한 3D 모델을 생성할 수 있는 장점을 가집니다. 이 접근법은 특히 SDF 데이터가 희소할 때 강점을 발휘하며, 다양한 해상도에서 보다 날카로운 특징을 재구성하는 데 효과적입니다.

[Sketch-Based Generation and Editing of Quad Meshes](3/Sketch-Based%20Generation%20and%20Editing%20of%20Quad%20Meshes%20db71978566d946778ed800b1ef7a7ffe)

스케치 기반 쿼드 메시 생성 및 편집 방법을 소개합니다. 이 연구는 3D 모델링에서 중요한 순수 사변형 메시의 수동 생성 및 편집을 간소화하는 새로운 접근법을 제공합니다. 사용자는 기본 커브 네트워크를 스케치하고, 이 네트워크는 자동으로 메시의 일부로 통합되며, 필요에 따라 특이점이 추가됩니다. 이 시스템은 고도로 세분화된 표면을 빠르게 생성할 수 있으며, 특히 복잡한 3D 형상을 쿼드로 리메시하는 데 유용합니다. 사용자 인터페이스는 스파인 스케치와 자동 완성 기능을 포함하며, 전문 아티스트는 이를 통해 쿼드 밀도와 에지 흐름을 제어할 수 있습니다. 이 방법은 기존의 수동 및 자동 메시 기법의 한계를 극복하고, 사용자가 메시의 결과를 세밀하게 제어할 수 있게 합니다.

[Robust and Controllable Quadrangulation of Triangular and Rectangular Regions](3/Robust%20and%20Controllable%20Quadrangulation%20of%20Triangu%20dab3a7ce6edd444ab46fd9e5661b616d)

3D 표면의 재메싱, 특히 삼각형과 직사각형 영역을 사각형 메시로 변환하는 과정에 초점을 맞춥니다. 이 연구는 사각형 메쉬를 N변 패치의 집합으로 간주하며, 사용자가 패치 경계와 세분화 수를 제어할 수 있도록 합니다. 특히, 다양한 변 세분화에 대응하며, 필요에 따라 특이점을 추가합니다. 이 알고리즘은 사각형 메쉬의 경계면이 항상 짝수라는 원칙에 기반하여, 원하는 에지 세분화에 관계없이 유효한 사각형 메쉬 토폴로지를 생성합니다. 이러한 접근법은 특히 복잡한 모델 부분에 초점을 맞출 때 유용하며, 직사각형과 삼각형 영역에 특화되어 있습니다.

[Sketch-Based Garment Design with Quad Meshes](3/Sketch-Based%20Garment%20Design%20with%20Quad%20Meshes%20232cabdb3aba4a889a9a3fcb57ca816b)

의류 모델링의 복잡성을 단순화하는 새로운 스케치 기반 방법을 소개합니다. 사용자는 3D 환경에서 천의 윤곽을 그린 후, 이를 고품질 사각형 메쉬로 변환하여 신체의 각 부분에 매핑합니다. 이 방법은 왜곡을 최소화하면서 다양한 형상에 대응하는 개선된 메쉬 생성 접근법을 사용합니다. 빠른 시뮬레이션 시스템을 통해 천의 독특한 특성을 포착하고, 모델링된 천이 신체의 움직임에 자연스럽게 반응할 수 있게 합니다. 이 기술은 사용자가 스케치를 통해 직관적으로 의류 디자인을 생성하고, 디지털 애니메이션에서의 사실적인 표현을 가능하게 합니다.

[Exploring Quadrangulations](3/Exploring%20Quadrangulations%206d6c867cee2c473da82d264496104361)

쿼드랭귤레이션, 즉 사각형 메시 생성에 대한 새로운 접근 방식을 제시합니다. 이 연구는 사용자가 주어진 메시의 모든 가능한 쿼드랭귤레이션 옵션을 효율적으로 탐색할 수 있는 방법을 개발하는 것을 목표로 합니다. 연구팀은 먼저 메시를 특정 표면 세그먼트로 나누고, 이를 선형 정수 프로그래밍을 통해 사각화하는 방법을 제안합니다. 이 과정에서 '단순 패치'라는 개념을 도입하여 복잡한 세그먼트를 단순화합니다. 또한, 다양한 잠재적 변형을 사용자에게 제시하고, 이를 탐색할 수 있도록 하는 세 가지 전략을 사용합니다: 안내식 열거, 클러스터링, 정렬. 이 연구는 쿼드 메시 레이아웃의 유용성을 강조하고, 새로운 쿼드랭귤레이션 방법의 가능성을 탐구합니다.

[QuadriFlow: A Scalable and Robust Method for Quadrangulation](3/QuadriFlow%20A%20Scalable%20and%20Robust%20Method%20for%20Quadra%20e4f90dec3b164cbca855d4fedbe9c3a2)

QuadriFlow는 위치 필드의 모든 특이점을 제거하여 사각 메쉬 생성에서 Instant Meshes보다 더 적은 불규칙한 정점을 생성하고, IGM에 비해 적은 왜곡을 보이는 향상된 알고리즘을 제공합니다. 이는 대규모 메쉬에 대한 효율성과 품질에서 기존 방법들을 능가하는 빠르고 견고한 메쉬 생성 솔루션입니다.