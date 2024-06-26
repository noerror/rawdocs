# Data-Driven Interactive Quadrangulation

*이 논문은 디지털 애니메이션과 엔터테인먼트 산업에서 매우 중요한 3D 모델에 쿼드 레이아웃을 생성하는 새로운 알고리즘을 제시합니다. 이 알고리즘은 기존 쿼드 메시에서 쿼드랭귤레이션 패턴을 학습하는 전처리 단계를 기반으로 하며, 40개의 메시에서 총 477,567개의 패턴을 학습했습니다.*

*인터랙티브 시스템을 통해 아티스트는 3D 모델에 수동으로 드로잉하여 에지 흐름과 경계 제약 조건을 제안할 수 있습니다. 그런 다음 알고리즘은 이 정보를 사용하여 학습된 패턴 데이터베이스를 쿼리하여 아티스트의 입력에 따라 원하는 영역을 테셀레이션합니다. 이 논문은 이 접근 방식이 복잡한 모델을 재토폴로지화하기 위한 작업량을 줄여 이전 방법보다 더 광범위한 솔루션을 제공한다고 제안합니다.*

*이 툴의 효과는 비공식적인 사용자 연구를 통해 검증되었으며, 효율성 향상과 전문 아티스트의 긍정적인 피드백을 통해 유망한 결과를 보여주었습니다. 그러나 저자들은 복잡하고 다양한 패치와 관련된 한계를 인정하며 개선 및 추가 개발의 여지가 있음을 인정합니다. 저자들은 추가 연구와 개선을 위해 소스 코드와 대규모 패치 데이터베이스를 공개할 계획입니다.*

[https://igl.ethz.ch/projects/ddq/ddq-paper.pdf](https://igl.ethz.ch/projects/ddq/ddq-paper.pdf)

[https://igl.ethz.ch/projects/ddq/](https://igl.ethz.ch/projects/ddq/)

[https://www.youtube.com/watch?v=H8K5CyQB_kc](https://www.youtube.com/watch?v=H8K5CyQB_kc)

- 2015 (SIGGRAPH)

![수작업으로 만든 쿼드랭귤레이션 입력 모델 세트가 주어지면 저희 알고리즘은 이를 설계하는 데 사용된 쿼드랭귤레이션 패턴을 학습합니다. 이 지식은 스케치 기반 레토폴로지 도구에 사용되어 사용자가 스케치한 패치를 세분화된 가장자리로 대화형으로 사각화합니다. 사용자는 패치 내부에 스트로크를 스케치하여 특정 에지 흐름을 제안할 수 있으며, 시스템은 이를 따라 자동으로 사각 측량을 선택합니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled.png)

수작업으로 만든 쿼드랭귤레이션 입력 모델 세트가 주어지면 저희 알고리즘은 이를 설계하는 데 사용된 쿼드랭귤레이션 패턴을 학습합니다. 이 지식은 스케치 기반 레토폴로지 도구에 사용되어 사용자가 스케치한 패치를 세분화된 가장자리로 대화형으로 사각화합니다. 사용자는 패치 내부에 스트로크를 스케치하여 특정 에지 흐름을 제안할 수 있으며, 시스템은 이를 따라 자동으로 사각 측량을 선택합니다.

### 1 Introduction

이 글에서는 애니메이션 및 CAD(컴퓨터 지원 설계) 업계에서 널리 사용되는 사변형 메시를 설명합니다. 특히 서페이스를 구조화되지 않은 삼각형 형식에서 고품질 다각형 메시로 변환하는 과정, 즉 리토폴로지라는 프로세스에 중점을 둡니다. 현재의 방법으로는 고밀도 메시를 안정적으로 생성할 수 있지만, 거친 패치 레이아웃이나 리토폴로지를 생성하는 것은 복잡한 문제입니다. 이는 주로 표면 지오메트리와 사용 목적에 따라 달라지는 이상적인 쿼드 메시 사이의 연결이 약하거나 아예 존재하지 않기 때문입니다.

업계에서는 전문가가 이러한 거친 쿼드 레이아웃을 수동으로 생성합니다. 수동 방식은 시간이 많이 걸리고 오류가 발생하기 쉽지만, 특정 애플리케이션 요구 사항에 따라 레이아웃을 조정할 때 상당한 유연성을 제공합니다. 이 프로세스를 보다 효율적으로 만들기 위해 스케치 기반 리토폴로지 방법이 개발되어 작업의 상당 부분을 자동화하는 동시에 설계자가 완전히 처음부터 다시 시작하지 않고도 토폴로지를 조정할 수 있습니다.

저자들은 대화형 쿼드랭귤레이션을 위한 새로운 데이터 기반 방법을 제안하여 희박한 스케치에서 복잡한 디자인을 만들 수 있도록 합니다. 이 연구의 주요 기여 사항은 다음과 같습니다:

최종 테셀레이션에서 원하는 에지 흐름을 지정하기 위한 스케치 기반 사용자 인터페이스.
다각형 패치에서 광범위한 사각형을 생성하기 위한 데이터 기반 탐색 방법.
이 데이터 기반 접근 방식은 수동으로 설계된 쿼드 메시 데이터베이스에서 가져온 대규모 테셀레이션 예제 모음을 기반으로 합니다. 이 방법의 효과는 일반성과 타당성을 제공하는 수동으로 디자인된 쿼드 메시 데이터베이스와 아티스트가 최종 디자인을 효율적으로 정의할 수 있는 스케치 인터페이스의 결합에서 비롯됩니다.

저자는 이 알고리즘을 대화형 스케치 기반 레토폴로지 시스템에 통합하고 비공식 사용자 연구를 통해 실용성을 검증했습니다. 이 알고리즘은 큰 다각형 영역을 자동으로 채우면서 중요한 영역의 작은 디테일을 정밀하게 조정할 수 있습니다. 사용자에게 표시되는 테셀레이션은 사용자가 제어하는 구조 및 기하학적 기준에 따라 정렬됩니다.

저자들은 주로 위상학적 측면에 초점을 맞춘 학습 기반 사각화 접근법을 향한 초기 단계를 밟았다고 결론을 내립니다. 저자들의 데이터베이스는 메시 연결에 기반을 두고 있는데, 메시 연결은 위상학에서 가장 필수적인 품질 척도이자 조합적 특성으로 인해 가장 까다롭기 때문입니다.

### 2 Related work

이 문서에서는 3D 모델링 및 애니메이션의 맥락에서 자동 쿼드 메시 및 사용자 지원 레토폴로지와 관련된 다양한 알고리즘과 방법에 대해 설명합니다.

자동 쿼드 메시: 자동 쿼드 메시를 위한 알고리즘은 광범위하게 연구되어 왔으며, 특이점 배치 및 거칠기 측면에서 높은 품질을 제공합니다. 하지만 이러한 알고리즘은 제어 기능이 부족하기 때문에 프로덕션 파이프라인에서 사용하기가 어렵습니다. 사용자가 정의한 제약 조건을 조금만 변경해도 문제의 글로벌 특성으로 인해 최종 쿼드랭귤레이션이 크게 달라질 수 있습니다. 또한 이러한 알고리즘은 대화형 방식이 아니기 때문에 파라미터 조정이 직관적이지 않고 시간이 많이 소요됩니다.

사용자 지원 방법: 사용자 지원 방법은 일반적으로 스케치 기반 레토폴로지 기법을 기반으로 합니다. 이 방식에서는 사용자가 입력 표면에 다각형 패치를 인터랙티브하게 스케치하면 자동으로 쿼드로 변환됩니다. 이러한 기법은 더 많은 제어 기능을 제공하지만, 채우기 패턴을 생성하는 것은 여전히 어려울 수 있습니다.

영감 쿼드랭귤레이션: 이 방법은 교차 파라미터화를 사용하여 머리, 팔 또는 몸통과 같은 서페이스의 서로 다른 부분 간에 쿼드랭귤레이션을 전송합니다. 이 접근 방식은 저자의 방법과 비슷한 동기를 공유하지만 메시 레이아웃에 대한 정밀한 로컬 제어 기능을 제공하지 않습니다.

연결 편집 작업: 사용자가 불규칙한 정점 쌍을 이동하여 기존 쿼드 메시를 수정할 수 있는 방법입니다. 유용한 로컬 연산자를 제공하고 메시 토폴로지를 미세 조정할 수 있지만, 모델을 처음부터 다시 토폴로지화하는 데는 적합하지 않습니다.

글로벌 스케치 기반 리토폴로지 알고리즘: 최근에 제안된 이 알고리즘은 사용자가 에지 루프를 생성한 다음 사변형 체인으로 변환하는 데 도움을 줍니다. 이 알고리즘은 수동 입력이 덜 필요하지만, 사변형을 로컬로 수정할 수 있다는 단점이 있습니다.

이러한 방법과 달리, 저자의 기술은 수동으로 설계된 모델에서 패턴을 학습하고 사용자 제약 조건에 따라 즉시 가져옵니다. 이를 통해 결과 쿼드랭귤레이션을 정밀하게 제어하여 국소적이고 유연한 리토폴로지 재사용이 가능합니다.

### 3 Method

이 방법은 사용자가 비정형 삼각형 메시에서 다각형 영역을 스케치한 다음 수동으로 디자인한 패턴을 사용하여 사각화하는 패턴 기반 사각화 프레임워크를 개선한 것입니다. 그러나 우리는 기존의 수동으로 설계된 메시 모델에서 복잡한 쿼드랭귤레이션 패턴을 학습하여 이 아이디어를 확장합니다.

![기본 패치 채우기 파이프라인입니다. 채울 영역은 경계 제약 조건을 정의합니다. 경계 제약 조건은 5면과 각각의 세분화 수입니다(a). 데이터베이스에서 5면 패턴을 모두 검색하고(b), 각 패턴에 대해 주어진 경계 제약 조건과 일치하는 가능한 모든 폴리코드 확장(컬러 에지 체인)을 찾고(c), 확장된 패치를 입력 지오메트리에 맞춥니다(d).](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%201.png)

기본 패치 채우기 파이프라인입니다. 채울 영역은 경계 제약 조건을 정의합니다. 경계 제약 조건은 5면과 각각의 세분화 수입니다(a). 데이터베이스에서 5면 패턴을 모두 검색하고(b), 각 패턴에 대해 주어진 경계 제약 조건과 일치하는 가능한 모든 폴리코드 확장(컬러 에지 체인)을 찾고(c), 확장된 패치를 입력 지오메트리에 맞춥니다(d).

![사용자가 원하는 엣지 플로우(빨간색 선)를 스케치하면 시스템은 주어진 플로우에 부합하는 패턴만 DB에서 검색하고 생성된 패치를 그에 따라 정렬합니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%202.png)

사용자가 원하는 엣지 플로우(빨간색 선)를 스케치하면 시스템은 주어진 플로우에 부합하는 패턴만 DB에서 검색하고 생성된 패치를 그에 따라 정렬합니다.

![학습 단계 개요. 패치는 원본 메시에서 샘플링되고, 해당 패턴은 데이터베이스에 인코딩된 연결성을 저장하기 전에 폴리코드 붕괴를 통해 생성됩니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%203.png)

학습 단계 개요. 패치는 원본 메시에서 샘플링되고, 해당 패턴은 데이터베이스에 인코딩된 연결성을 저장하기 전에 폴리코드 붕괴를 통해 생성됩니다.

이 접근 방식에서는 학습 데이터 세트에서 메시를 분석하여 많은 수의 패턴(현재 구현에서는 477,000개)을 추출합니다. 이 오프라인 학습 단계는 패턴 데이터베이스의 생성으로 이어집니다. 사용 중에 데이터베이스를 실시간으로 쿼리하여 사용자가 지정한 경계 조건과 원하는 엣지 흐름에 부합하는 패턴을 찾을 수 있습니다. 그런 다음 이러한 패턴은 더 높은 품질의 패치에 우선순위를 부여하는 순서로 사용자에게 표시됩니다.

이 패턴 기반 사각화 프로세스는 사용자가 그린 경계 제약 조건으로 시작하여 경계 제약 조건에 맞게 확장할 수 있는 패턴으로 채워집니다. 미리 정의된 패턴이 아닌 학습된 패턴 데이터베이스를 사용하여 모든 경계 제약 조건에 대해 일치하는 패턴을 많이 제공합니다. 방대한 데이터베이스를 통해 쿼리 한 번으로 수백 개의 일치하는 패치를 검색할 수 있으며, 이를 사용자 정의 기준에 따라 정렬하여 쉽게 선택할 수 있습니다.

![트리에서 하나의 경로를 따라 전체 측면 및 볼록 패치 확장 트리에서. 첫 번째와 두 번째 이미지 사이의 두 단계 확장 이미지 사이의 두 단계 확장은 생략됩니다. 모든 파란색 패치는 전체면 확장 중에 생성됩니다. 확장 중에 생성되며, 볼록 패치 확장 중에는 패치 (e), (i), (j), (k) 패치는 반사 정점을 포함하고 있어 패턴을 생성하지 않습니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%204.png)

트리에서 하나의 경로를 따라 전체 측면 및 볼록 패치 확장 트리에서. 첫 번째와 두 번째 이미지 사이의 두 단계 확장 이미지 사이의 두 단계 확장은 생략됩니다. 모든 파란색 패치는 전체면 확장 중에 생성됩니다. 확장 중에 생성되며, 볼록 패치 확장 중에는 패치 (e), (i), (j), (k) 패치는 반사 정점을 포함하고 있어 패턴을 생성하지 않습니다.

![패치를 열거하는 동안 동일한 패치가 다른 경로로 패치를 발견할 수 있습니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%205.png)

패치를 열거하는 동안 동일한 패치가 다른 경로로 패치를 발견할 수 있습니다.

학습 단계는 훈련 세트의 각 메시에서 패치를 추출하는 패치 열거와 각 패치의 '폴리코드'를 에지 체인으로 축소하여 패턴으로 축소하는 패턴 감소의 두 가지 주요 단계로 구성됩니다. 또한 속도와 효율성을 보장하기 위해 메시를 사전 처리하여 각 메시를 단순화하고 중복 패치를 관리합니다. 그런 다음 각 패턴은 연결성을 설명하는 고유한 문자열로 인코딩되어 메모리와 디스크 공간을 줄이고 효율적인 쿼리를 지원합니다. 데이터베이스는 이러한 패턴을 속성에 따라 클래스로 구성합니다.

![모서리 V0에서 방문을 시작하는 템플릿 패턴의 면 열거입니다. 특수 기호 #는 코드에서 경계 가장자리를 나타내는 데 사용됩니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%206.png)

모서리 V0에서 방문을 시작하는 템플릿 패턴의 면 열거입니다. 특수 기호 #는 코드에서 경계 가장자리를 나타내는 데 사용됩니다.

전반적으로 이 방법은 절차적 생성 접근 방식에 비해 일반적으로 사용되는 패턴을 더 효과적으로 샘플링하므로, 결과 사변형을 더 정밀하게 제어하여 보다 지역화되고 유연한 리토폴로지 재사용이 가능합니다.

### 4 Sketching queries

이 설명은 사용자 정의 제약 조건에 따라 서페이스에 대한 패치를 생성하는 대화형 시스템을 말합니다. 간단히 설명하면 다음과 같습니다:

1. 사용자가 서페이스에 커브를 스케치하여 경계 및 가장자리 흐름 제약 조건을 설정합니다.
2. 시스템은 이러한 제약 조건을 충족하는 일련의 패치를 생성합니다.
3. 에지 흐름 제약 조건이 없는 단순한 경계 제약 조건의 경우, 시스템은 데이터베이스에서 주어진 면 수를 가진 모든 패턴을 검색합니다. 이러한 패턴은 테스트를 거쳐 패치로 확장되며, 패치가 발견되는 즉시 시각화됩니다.
4. 시스템은 대화형 응답을 제공하기 위해 패턴을 병렬로 처리하고 데이터베이스에 저장된 순서대로 패치를 표시합니다.
5. 사용자는 패치의 비정상적인 버텍스 수와 같은 기능에 제한을 설정하여 검색 속도를 높일 수 있습니다. 그러나 경계 제약 조건과 일치하는 패치가 너무 많으면 에지 흐름 제약 조건을 추가하여 검색 범위를 좁히고 가장 적합한 패치가 먼저 표시되도록 할 수 있습니다.
6. 시스템에는 각 패턴에서 엣지 흐름의 동작을 설명하는 코딩 메커니즘이 있습니다. 이를 통해 에지 흐름 제약 조건과 일치하지 않는 패턴을 즉시 거부하는 데 도움이 됩니다.
    
    ![두 패턴의 경계 레이블 지정: 두 패턴 모두 경계 간 에지 흐름 제약 조건과 호환되지만 패턴 (a)만 루프 에지 흐름 제약 조건과 호환되며 패턴 (b)는 거부됩니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%207.png)
    
    두 패턴의 경계 레이블 지정: 두 패턴 모두 경계 간 에지 흐름 제약 조건과 호환되지만 패턴 (a)만 루프 에지 흐름 제약 조건과 호환되며 패턴 (b)는 거부됩니다.
    
7. 엣지 흐름 제약 조건은 쿼리 응답에서 시스템의 효율성과 정확성을 개선하는 데도 사용됩니다. 시스템은 선형 방정식 시스템을 설정하여 에지 흐름 제약 조건이 패턴의 폴리코드(경계와 경계를 잇는 에지 묶음)와 일치하는지 확인합니다.
    
    ![에지 흐름 제약 조건을 스케치하면 사용자는 다음을 수행할 수 있습니다. 패턴을 필터링하고, 추가 제약 조건을 설정하여 생성된 패치 세트를 폴리코드가 정확히 일치하는 패치로 줄입니다. 스케치와 동일한 가장자리에서 시작하여 동일한 가장자리로 끝남 (C) 또는 단지 해당 가장자리에 가깝게 (d).](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%208.png)
    
    에지 흐름 제약 조건을 스케치하면 사용자는 다음을 수행할 수 있습니다. 패턴을 필터링하고, 추가 제약 조건을 설정하여 생성된 패치 세트를 폴리코드가 정확히 일치하는 패치로 줄입니다. 스케치와 동일한 가장자리에서 시작하여 동일한 가장자리로 끝남 (C) 또는 단지 해당 가장자리에 가깝게 (d).
    
8. 시스템은 에지 흐름 제약 조건과의 정렬에 따라 패치의 순위를 매깁니다. 정렬은 스케치를 패치에서 가능한 모든 폴리코드 스켈레톤과 비교하고 거리를 합산하여 측정합니다.
    
    ![에지 흐름 제약 조건에 따른 솔루션 정렬: 스케치된 흐름에 대한 폴리코드의 정렬은 적분 오차로 측정됩니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%209.png)
    
    에지 흐름 제약 조건에 따른 솔루션 정렬: 스케치된 흐름에 대한 폴리코드의 정렬은 적분 오차로 측정됩니다.
    
9. 이 시스템은 쿼드의 평균 품질에 기반한 다른 순위 지정 방법도 실험했지만 품질이 낮은 패치를 생성했습니다.
    
    ![쿼드 품질 순위(a)로 찾은 첫 번째 패치는 비공식 사용자 연구에서 아티스트들이 선호하는 기준인 흐름 정렬 순위(b)로 찾은 첫 번째 패치보다 인지 품질이 훨씬 낮습니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%2010.png)
    
    쿼드 품질 순위(a)로 찾은 첫 번째 패치는 비공식 사용자 연구에서 아티스트들이 선호하는 기준인 흐름 정렬 순위(b)로 찾은 첫 번째 패치보다 인지 품질이 훨씬 낮습니다.
    
10. 스케치와의 정렬을 더욱 개선하기 위해 시스템은 라플라시안 편집이라는 기술을 사용하여 패치를 변형합니다. 이 작업은 정렬 기준에 영향을 미치므로 모든 패치에서 수행됩니다.

### 5 Results

연구팀은 다각형 테셀레이션 학습을 위한 알고리즘을 설계하고 이를 C++로 구현했습니다. 이 프로그램은 12코어 인텔 제온 E5와 64GB 메모리를 갖춘 워크스테이션에서 실행되었습니다. 이들은 아이겐 라이브러리와 Achterberg 2009의 솔버를 사용하여 수동으로 모델링한 40개의 메시에서 477,567개의 템플릿 패턴을 학습했습니다. 이를 통해 최대 34개의 면을 가진 다각형의 테셀레이션이 가능합니다.

대부분의 자동화된 쿼드랭귤레이션 알고리즘은 기하학적 특징만 캡처하고 사람 얼굴의 뺨과 같은 의미적 특징은 놓칩니다. 연구팀의 방법을 사용하면 아티스트가 이러한 특징에 맞춰 쿼드를 수동으로 정렬할 수 있으므로 애니메이션 및 엔터테인먼트 산업에 이상적입니다.

![[Bommes 외, 2009]에서 제안한 자동 방법(a)과 저희의 접근 방식(b)을 비교한 것입니다.](Data-Driven%20Interactive%20Quadrangulation%20f4dcee598dfa41108e49e9ef0abef873/Untitled%2011.png)

[Bommes 외, 2009]에서 제안한 자동 방법(a)과 저희의 접근 방식(b)을 비교한 것입니다.

우리

연구팀은 이 방법을 2014년 타카야마(Takayama) 등의 접근 방식과 비교했습니다. 타카야마 접근 방식은 15개의 사전 정의된 패턴을 사용하여 최대 6개의 면을 가진 폴리곤을 테셀레이션할 수 있지만, 임의의 에지 흐름 제약 조건을 위한 확장 패치를 충분히 다양하게 제공하지 못합니다.

공개적으로 사용 가능한 스케치 기반 모델링 사용자 인터페이스와 통합된 시스템을 사용하여 세 명의 전문 아티스트와 함께 비공식 사용자 연구를 수행했습니다. 아티스트들은 더 큰 패치로 작업하고 더 짧은 시간에 더 나은 결과를 얻을 수 있는 팀의 방법을 선호했으며, 이는 타카야마 등의 방법과 비교했을 때 더 나은 결과를 얻을 수 있었습니다. 이 방법은 생산성을 높일 수 있는 잠재력을 보여주었습니다.

저해상도 리토폴로지 작업과 패치 분류 기준 등 몇 가지 한계가 있었습니다. 연구팀은 학습 중에 경계 조건과 패턴 빈도를 기반으로 패치를 제공하는 시스템을 도입했습니다. 이 시스템은 스트로크를 추가함으로써 솔루션을 필터링하여 가장 적합한 솔루션으로 선택의 폭을 좁혔습니다.

또한 변의 수가 많은 다각형을 테셀레이션하는 시스템의 성능도 시연했습니다. 또한 위상학적으로 볼록한 패치를 처리하고 여러 오목한 패턴도 학습했습니다. 또한 눈이나 너클과 같은 특징을 따라가는 내부 가장자리 루프를 생성하고 표면의 복잡하고 형태학적으로 확장된 부분을 처리할 수 있었습니다.

### 6 Limitations and concluding remarks

연구팀은 데이터 기반 접근 방식을 사용하여 기존 쿼드 메시에서 고품질의 쿼드랭귤레이션 패턴을 학습하는 대화형 시스템을 개발했습니다. 이 방법은 복잡한 모델을 다시 토폴로지화하는 데 필요한 노력을 크게 줄여줍니다. 현재 데이터베이스는 40개의 메시에서 학습한 477,567개의 패턴으로 구성되어 있으며, 이는 이 방법의 효과를 입증합니다.

그러나 이 데이터베이스는 6면이 넘는 패치에 대해 가능한 모든 구성을 포함하지 않을 수 있습니다. 볼록한 패턴은 많이 포함되어 있지만 오목한 패턴은 적습니다. 데이터베이스가 만족스러운 답을 제공하지 못하는 경우, 사용자는 복잡한 패치를 더 간단한 패치로 분할하거나 기존 도구를 사용하여 수동으로 설계할 수 있습니다. 이 도구는 10,000개의 무작위 쿼리로 테스트했을 때 매우 높은 성공률을 보였습니다.

향후 개발팀은 소스 코드를 공개할 계획이며, 사용자들이 패치 레이아웃을 공유하여 대규모 공개 패치 데이터베이스를 만드는 데 기여하기를 희망합니다. 보다 광범위한 패치 데이터베이스가 구축되면 시스템의 효율성이 크게 향상될 수 있습니다. 광범위한 솔루션 공간을 관리하려면 캐싱 전략과 고급 쿼리 시스템이 필요할 수 있습니다. 연결 압축을 위해 정교한 알고리즘을 사용하면 쿼리 시스템의 효율성을 유지하면서 메모리 사용량을 줄이는 데 도움이 될 수 있습니다.

- 요약
    
    1단계: 메시에서 패턴 학습
    
    알고리즘은 수동으로 모델링된 40개의 메시에서 템플릿 패턴을 학습하는 것으로 시작합니다. 이 단계에는 이러한 메시의 기존 구조를 조사하고 폴리곤이 어떻게 테셀레이션되거나 서로 맞물리는지에 대한 패턴을 추출하는 작업이 포함될 수 있습니다.
    
    2단계: 패턴 저장
    
    그런 다음 이러한 패턴을 데이터베이스에 저장합니다. 이 데이터베이스는 알고리즘이 새로운 쿼드 레이아웃을 생성하는 데 사용하는 핵심 리소스를 형성합니다.
    
    3단계: 사용자 입력 수락
    
    사용자는 3D 모델에 원하는 쿼드 레이아웃을 스케치하여 인터랙티브 세션을 시작합니다. 스케치 인터페이스를 통해 아티스트는 쿼드를 시맨틱 피처에 수동으로 정렬할 수 있습니다.
    
    4단계: 패턴 매칭 및 검색
    
    사용자가 모델에 스케치와 같은 쿼리를 입력하면 알고리즘이 데이터베이스에서 원하는 레이아웃에 맞는 템플릿 패치를 검색합니다. 알고리즘은 경계 조건과 사용자가 스케치한 에지 흐름 제약 조건과의 호환성을 고려합니다.
    
    5단계: 테셀레이션
    
    데이터베이스에서 선택한 패치를 사용하여 스케치 영역을 테셀레이션 또는 타일링합니다. 테셀레이션은 사용자의 경계 및 가장자리 흐름 제약 조건을 최대한 존중합니다.
    
    6단계: 결과 표시
    
    알고리즘은 테셀레이션된 결과를 사용자에게 표시합니다. 둘 이상의 유효한 테셀레이션이 가능한 경우 사용자에게 옵션 목록을 표시합니다.
    
    7단계: 사용자 피드백 및 반복 작업
    
    사용자가 피드백을 제공하거나 스케치 및 제약 조건을 수정합니다. 알고리즘은 사용자가 결과 쿼드 메시를 만족할 때까지 이 피드백을 기반으로 4~6단계를 반복합니다.