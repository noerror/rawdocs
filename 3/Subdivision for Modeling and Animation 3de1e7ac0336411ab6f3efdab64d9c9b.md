# Subdivision for Modeling and Animation

[https://graphics.pixar.com/opensubdiv/docs/intro.html](https://graphics.pixar.com/opensubdiv/docs/intro.html)

- 1999 Siggraph

## 1 Introduction

20년 전 Catmull과 Clark, Doo와 Sabin의 논문이 표면 모델링을 위한 서브디비전의 시작을 알렸으며, 올해 Pixar가 아카데미 단편 애니메이션 상을 수상한 "Geri's Game"에서 서브디비전이 큰 스크린에 등장하는 또 다른 이정표가 세워졌다고 언급합니다. 서브디비전의 기본 아이디어는 매우 오래되었으며, 1940년대 후반과 1950년대 초반으로 거슬러 올라가 G. de Rham이 부드러운 곡선을 설명하기 위해 "코너 컷팅"을 사용한 때부터 시작되었습니다. 최근에는 컴퓨터 그래픽스와 컴퓨터 지원 기하학적 설계(CAGD)에서 서브디비전 표면이 널리 적용되고 있으며, 이는 멀티레졸루션 기술의 중요성과 관련이 깊습니다. 서브디비전은 여러 문제를 우아하게 해결하며, 임의의 토폴로지, 확장성, 표현의 통일성, 수치적 안정성 및 코드 간결성 등의 이점을 제공합니다. 이 강좌와 동반되는 노트에서는 서브디비전의 기본 원칙, 서브디비전 규칙의 구성 방법, 분석 접근 방식, 그리고 실제 응용으로의 전환을 다룹니다. 초기 챕터는 기본 원칙을 이해하는 데 중점을 두고, 후속 챕터에서는 상호작용 멀티레졸루션 메쉬 편집, 웨이브렛과 서브디비전 표면, 서브디비전을 이용한 모델링 및 애니메이션, 그리고 "Geri's Game" 제작에서의 서브디비전 표면 적용 등을 다룹니다. 서브디비전이 현재 많은 관심을 받고 있는 이유 중 하나는 구현이 매우 쉽고 효율적이라는 점입니다. 서브디비전의 수학적 이론은 아름답지만 복잡하며, 이 노트에서는 주로 컴퓨터 그래픽스 실무자를 대상으로 하여 이론적인 세부 사항을 다루지는 않습니다. 추가 자료는 온라인에서 확인할 수 있습니다.

## 2 Foundations I: Basic Ideas

![평면에서 커브의 세분화 예시. 왼쪽에는 4개의 점이 직선 세그먼트로 연결되어 있습니다. 그 오른쪽에는 세분화된 버전이 있습니다. 이전 점 사이에 3개의 새 점이 삽입되고 이를 연결하는 선형 커브가 다시 그려집니다. 두 단계를 더 세분화하면 곡선이 다소 부드러워지기 시작합니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled.png)

평면에서 커브의 세분화 예시. 왼쪽에는 4개의 점이 직선 세그먼트로 연결되어 있습니다. 그 오른쪽에는 세분화된 버전이 있습니다. 이전 점 사이에 3개의 새 점이 삽입되고 이를 연결하는 선형 커브가 다시 그려집니다. 두 단계를 더 세분화하면 곡선이 다소 부드러워지기 시작합니다.

### 2.1 The Idea of Subdivision

서브디비전의 기본 아이디어는 간단합니다. 기본적인 입력 다각형(또는 다면체)을 점차적으로 정제(refine)하여 더 부드럽고 상세한 표면을 생성하는 것입니다. 이를 통해 초기의 거친 형태가 정교하고 매끄러운 표면으로 변환됩니다. 이러한 과정은 반복적이며, 각 단계에서 새로운 점들이 추가되어 점차적으로 더욱 미세한 구조가 생성됩니다. 이 과정은 대칭적이며 국부적(local) 규칙에 의해 정의되며, 결과적으로 임의의 초기 형태에서도 원하는 부드러운 표면을 얻을 수 있습니다. 서브디비전은 초기 형태의 토폴로지를 유지하면서 점진적으로 세밀한 표면을 생성하는데 효과적입니다.

![3단계의 연속적인 세분화를 보여주는 서페이스의 세분화 예시. 왼쪽은 서페이스에 근사한 초기 삼각형 메시입니다. 각 삼각형은 특정 세분화 규칙에 따라 4개로 분할됩니다(가운데). 오른쪽에서는 메시가 다시 한 번 이러한 방식으로 세분화됩니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%201.png)

3단계의 연속적인 세분화를 보여주는 서페이스의 세분화 예시. 왼쪽은 서페이스에 근사한 초기 삼각형 메시입니다. 각 삼각형은 특정 세분화 규칙에 따라 4개로 분할됩니다(가운데). 오른쪽에서는 메시가 다시 한 번 이러한 방식으로 세분화됩니다.

![두 개의 특수 정점이 있는 메쉬로, 하나는 원자가 6이고 다른 하나는 원자가 3입니다. 사변형 패치의 경우 표준 원자가는 4입니다. 특이점에서 만나는 스플라인 패치 사이의 높은 연속성을 보장하기 위해서는 특별한 노력이 필요하지만, 세분화는 이러한 상황을 자연스럽게 처리합니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%202.png)

두 개의 특수 정점이 있는 메쉬로, 하나는 원자가 6이고 다른 하나는 원자가 3입니다. 사변형 패치의 경우 표준 원자가는 4입니다. 특이점에서 만나는 스플라인 패치 사이의 높은 연속성을 보장하기 위해서는 특별한 노력이 필요하지만, 세분화는 이러한 상황을 자연스럽게 처리합니다.

### 2.2 Review of Splines

스플라인(spline)은 컴퓨터 그래픽스와 기하학적 설계에서 곡선을 생성하는 데 널리 사용됩니다. 일반적으로 스플라인은 제어점과 연결된 다항식을 사용하여 부드러운 곡선을 정의합니다. 스플라인은 여러 종류가 있으며, 대표적으로 B-스플라인(B-spline)과 베지어 스플라인(Bezier spline)이 있습니다. B-스플라인은 연속적이고 부드러운 곡선을 생성하며, 각 제어점의 가중치를 조절하여 곡선의 형태를 변경할 수 있습니다. 베지어 스플라인은 제어점 사이의 곡선을 매끄럽게 연결하며, 일반적으로 그래픽 디자인과 애니메이션에 사용됩니다. 서브디비전 표면은 스플라인의 개념을 확장하여 3차원 표면을 생성합니다. 스플라인의 수학적 기초와 특성은 서브디비전의 이해를 돕는 중요한 개념입니다.

### 2.3 Subdivision as Repeated Refinement

서브디비전은 반복적인 정제 과정을 통해 초기의 거친 다각형 메쉬를 점진적으로 더 부드럽고 정교한 형태로 변환하는 방법입니다. 이 과정은 각 단계에서 새로운 점을 추가하고, 기존 점을 이동시켜 더 세밀한 구조를 형성합니다. 서브디비전 알고리즘은 각 단계에서 일정한 규칙에 따라 점의 위치를 계산하며, 이러한 규칙은 다양한 방식으로 정의될 수 있습니다. 가장 일반적인 방식은 평균화 과정(averaging process)으로, 각 점의 위치를 주변 점들의 평균으로 이동시켜 점진적으로 부드러운 표면을 형성합니다. 이러한 반복적인 정제 과정은 컴퓨터 그래픽스와 기하학적 설계에서 매우 중요한 역할을 하며, 초기의 거친 형태에서도 매우 정교하고 매끄러운 표면을 생성할 수 있습니다.

### 2.4 Analysis of Subdivision

서브디비전의 분석은 주로 알고리즘의 수렴성과 생성된 표면의 부드러움을 평가하는 데 중점을 둡니다. 서브디비전 알고리즘은 반복적인 과정이기 때문에, 각 단계에서 생성된 표면이 점진적으로 부드럽고 정교해지는지를 분석하는 것이 중요합니다. 이를 위해 다양한 수학적 기법이 사용되며, 특히 고유값 분석(eigenvalue analysis)과 Fourier 변환이 많이 사용됩니다. 고유값 분석은 서브디비전 매트릭스의 고유값을 계산하여 알고리즘의 수렴성을 평가합니다. Fourier 변환은 서브디비전 과정에서 생성된 주파수 성분을 분석하여 표면의 부드러움을 평가합니다. 이러한 분석 기법을 통해 서브디비전 알고리즘의 성능을 평가하고, 생성된 표면이 원하는 특성을 가지는지를 확인할 수 있습니다.

![이동 시 불변성.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%203.png)

이동 시 불변성.

![세분 행렬을 n개의 제어점 집합에 반복적으로 적용하면 제어점이 탄젠트 벡터와 정렬된 구성으로 수렴하게 됩니다. 다양한 세분화 레벨은 명확성을 위해 수직으로 오프셋되었습니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%204.png)

세분 행렬을 n개의 제어점 집합에 반복적으로 적용하면 제어점이 탄젠트 벡터와 정렬된 구성으로 수렴하게 됩니다. 다양한 세분화 레벨은 명확성을 위해 수직으로 오프셋되었습니다.

## 3 Subdivision Surfaces

### 3.1 Subdivision Surfaces: an Example

이 절에서는 구체적인 서브디비전 표면의 예를 통해 개념을 설명합니다. 예를 들어, 간단한 다각형 메쉬를 반복적으로 세분화하여 부드럽고 연속적인 표면으로 변환하는 과정을 보여줍니다. 이 과정을 통해 독자들은 서브디비전이 어떻게 초기 메쉬의 형태를 점진적으로 부드럽고 정밀한 표면으로 변화시키는지 이해하게 됩니다. 예제는 단계별로 서브디비전 규칙을 적용하고, 각 단계에서 메쉬가 어떻게 변하는지 시각적으로 설명합니다.

![삼각형 메시의 세분화. 새 정점은 검은색 점으로 표시됩니다. 제어 메시의 각 가장자리가 두 개로 분할되고 새 정점이 다시 연결되어 4개의 새 삼각형이 형성되어 메시의 각 삼각형을 대체합니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%205.png)

삼각형 메시의 세분화. 새 정점은 검은색 점으로 표시됩니다. 제어 메시의 각 가장자리가 두 개로 분할되고 새 정점이 다시 연결되어 4개의 새 삼각형이 형성되어 메시의 각 삼각형을 대체합니다.

### 3.2 Natural Parameterization of Subdivision Surfaces

서브디비전 표면의 자연 매개변수화는 각 점이 원래 메쉬의 매개변수 공간에서의 위치를 가지도록 하는 방법을 다룹니다. 이 절에서는 서브디비전 표면의 각 정점이 어떻게 매개변수 공간에서 자연스럽게 배치되는지 설명합니다. 이는 텍스처 매핑, 애니메이션, 그리고 다른 기하학적 처리를 할 때 중요한 개념입니다. 자연 매개변수화는 서브디비전 과정에서의 일관성과 정확성을 보장하는 데 필수적입니다.

![세분화 표면의 자연스러운 매개변수화](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%206.png)

세분화 표면의 자연스러운 매개변수화

### 3.3 Subdivision Matrix

서브디비전 행렬은 서브디비전 규칙을 수학적으로 표현하는 도구입니다. 이 절에서는 서브디비전 행렬의 정의와 그 역할을 설명합니다. 서브디비전 행렬은 초기 메쉬의 정점 좌표를 새로운 정점 좌표로 변환하는 과정을 수학적으로 모델링합니다. 이 행렬을 사용하면 서브디비전 과정의 수학적 분석이 가능하며, 다양한 서브디비전 방법을 비교하고 평가할 수 있습니다.

![차수 3의 꼭지점 근처의 루프 세분 방식. 두 개의 고리에서 3 x 3 + 1 = 10점이 필요합니다.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%207.png)

차수 3의 꼭지점 근처의 루프 세분 방식. 두 개의 고리에서 3 x 3 + 1 = 10점이 필요합니다.

### 3.4 Smoothness of Surfaces

서브디비전 표면의 부드러움은 결과 표면의 품질을 결정짓는 중요한 요소입니다. 이 절에서는 서브디비전 표면의 부드러움을 정의하고 평가하는 방법을 다룹니다. 특정 서브디비전 규칙이 얼마나 부드러운 표면을 생성하는지, 그리고 이 부드러움이 수학적으로 어떻게 표현되는지를 설명합니다. 특히, 서브디비전 표면의 연속성과 곡률을 분석하여 부드러움을 평가하는 방법을 제시합니다.

### 3.5 Analysis of Subdivision Surfaces

서브디비전 표면의 분석은 다양한 서브디비전 규칙의 특성을 이해하는 데 중요합니다. 이 절에서는 서브디비전 표면의 수학적 분석 기법을 소개합니다. 서브디비전 표면의 수렴성, 안정성, 그리고 다른 수학적 특성을 분석하여 어떤 서브디비전 규칙이 특정 상황에서 가장 적합한지를 판단합니다. 이 분석은 서브디비전 표면의 품질을 보장하고, 원하는 결과를 얻기 위해 적절한 서브디비전 방법을 선택하는 데 도움을 줍니다.

### 3.6 Piecewise-smooth Surfaces and Subdivision

부분적으로 부드러운 표면은 서브디비전 표면에서 중요한 개념입니다. 이 절에서는 부분적으로 부드러운 표면을 생성하는 서브디비전 방법을 다룹니다. 이러한 방법은 표면의 특정 부분이 부드럽지 않거나 날카로운 특징을 유지해야 할 때 유용합니다. 이를 통해 복잡한 형태의 객체를 보다 현실감 있게 모델링할 수 있습니다. 부분적으로 부드러운 표면을 생성하는 서브디비전 규칙과 그 적용 예시를 통해 독자들은 다양한 형태를 모델링하는 방법을 배우게 됩니다.

![부분적으로 매끄러운 경계를 가진 서페이스의 차트](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%208.png)

부분적으로 매끄러운 경계를 가진 서페이스의 차트

## 4 Subdivision Zoo

### 4.1 Overview of Subdivision Schemes

이 절에서는 서브디비전 기법의 개요를 소개합니다. 서브디비전 기법은 초기 다각형 메쉬를 반복적으로 세분화하여 부드럽고 정교한 표면을 생성하는 과정입니다. 다양한 서브디비전 기법이 존재하며, 각 기법은 특정한 방식으로 정점을 생성하고 연결합니다. 이 절에서는 대표적인 서브디비전 기법의 종류와 그 특징을 간략하게 설명합니다.

![다양한 세분화 규칙.](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%209.png)

다양한 세분화 규칙.

### 4.2 Loop Scheme

루프(Looop) 기법은 삼각형 메쉬를 기반으로 하는 서브디비전 기법입니다. 이 절에서는 루프 기법의 기본 원리와 적용 방법을 설명합니다. 루프 기법은 각 삼각형의 중심과 모서리의 중점을 연결하여 새로운 정점을 생성합니다. 이 과정을 반복하면 부드럽고 연속적인 표면이 생성됩니다. 루프 기법은 특히 유기적인 형태의 모델링에 적합하며, 부드러운 곡면을 효과적으로 생성할 수 있습니다.

![Untitled](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%2010.png)

### 4.3 Modified Butterfly Scheme

수정된 버터플라이 기법은 이진 분할 알고리즘을 사용하는 서브디비전 기법입니다. 이 절에서는 수정된 버터플라이 기법의 원리와 적용 방법을 설명합니다. 이 기법은 주로 삼각형 메쉬에 적용되며, 각 삼각형의 중심과 인접한 삼각형의 정점을 사용하여 새로운 정점을 생성합니다. 수정된 버터플라이 기법은 기존의 버터플라이 기법의 단점을 보완하여 더 안정적이고 부드러운 표면을 생성합니다.

![Untitled](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%2011.png)

### 4.4 Catmull-Clark Scheme

캣멀-클라크 기법은 사각형 메쉬를 기반으로 하는 서브디비전 기법입니다. 이 절에서는 캣멀-클라크 기법의 기본 원리와 적용 방법을 설명합니다. 이 기법은 각 사각형의 중심과 모서리의 중점을 연결하여 새로운 정점을 생성합니다. 이 과정을 반복하면 부드럽고 연속적인 표면이 생성됩니다. 캣멀-클라크 기법은 주로 산업 디자인과 애니메이션에서 널리 사용되며, 복잡한 형태의 모델링에 적합합니다.

![Untitled](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%2012.png)

### 4.5 Kobbelt Scheme

코벨트 기법은 사각형 메쉬를 기반으로 하는 또 다른 서브디비전 기법입니다. 이 절에서는 코벨트 기법의 원리와 적용 방법을 설명합니다. 코벨트 기법은 각 사각형의 중심과 모서리의 중점을 사용하여 새로운 정점을 생성하며, 이 과정을 반복하여 부드러운 표면을 생성합니다. 코벨트 기법은 특히 기하학적 형태의 모델링에 적합하며, 정교한 표면을 생성할 수 있습니다.

![Untitled](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%2013.png)

### 4.6 Doo-Sabin and Midedge Schemes

두-사빈 기법과 미드엣지 기법은 다각형 메쉬를 기반으로 하는 서브디비전 기법입니다. 이 절에서는 두-사빈 기법과 미드엣지 기법의 원리와 적용 방법을 설명합니다. 두-사빈 기법은 각 다각형의 중심과 모서리의 중점을 사용하여 새로운 정점을 생성하며, 미드엣지 기법은 각 모서리의 중점과 인접한 면의 중심을 사용하여 새로운 정점을 생성합니다. 두 기법 모두 부드럽고 연속적인 표면을 생성하는 데 효과적입니다.

![Untitled](Subdivision%20for%20Modeling%20and%20Animation%203de1e7ac0336411ab6f3efdab64d9c9b/Untitled%2014.png)

### 4.7 Limitations of Stationary Subdivision

정적 서브디비전 기법의 한계는 서브디비전 표면의 품질과 적용 가능성을 제한할 수 있습니다. 이 절에서는 정적 서브디비전 기법의 주요 한계와 그 원인을 설명합니다. 정적 서브디비전 기법은 반복적으로 동일한 규칙을 적용하므로, 특정 형태에서는 부드러운 표면을 생성하지 못하거나, 원하는 결과를 얻기 어려울 수 있습니다. 이러한 한계를 극복하기 위한 다양한 접근법과 해결책도 논의됩니다.