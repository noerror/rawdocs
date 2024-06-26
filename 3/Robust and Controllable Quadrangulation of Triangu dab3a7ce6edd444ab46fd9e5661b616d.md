# Robust and Controllable Quadrangulation of Triangular and Rectangular Regions

[https://cims.nyu.edu/gcl/papers/sketch-based-generation-and-editing-of-quad-meshes-siggraph-2013-takayama-et-al-tech-report.pdf](https://cims.nyu.edu/gcl/papers/sketch-based-generation-and-editing-of-quad-meshes-siggraph-2013-takayama-et-al-tech-report.pdf)

- 2013

### 1 Introduction

3D 표면을 거친 4분할 메시로 재메싱하는 프로세스는 비디오 게임이나 영화에서 사용되는데, 종종 특정 메시 토폴로지를 필요로 한다. 이 프로세스에 대한 알고리즘은 존재하지만, 사용자가 제어할 수 없는 경우가 많다. 이러한 기술 격차로 인해 사용자는 다양한 제약 조건 하에서 여러 번 알고리즘을 실행해야 한다. 또한, 이러한 알고리즘은 메쉬 해상도가 거칠어질수록 성능이 저하된다. 그 결과, 많은 아티스트들이 3D-Coat나 ZBrush와 같은 수동 모델링 도구로 리메싱을 되돌리고 있다. 이러한 도구는 완전한 제어가 가능하지만, 특히 부분적인 쿼드 메시를 병합할 때 시간이 오래 걸리고 어렵다.

Takayama 등(2013)의 병행 연구에서 혁신적인 수동 사각형 메쉬 재메싱 시스템이 발표되었다. 이 시스템은 사각형 메쉬를 내부에 여러 개의 사각형이 있는 N변 패치의 집합으로 간주한다. 사용자는 패치의 경계를 윤곽을 잡고, 각 경계에 있는 변의 세분화 개수를 자유롭게 지정할 수 있다. 특이점(표준 수 이외의 사각형으로 공유되는 정점)은 필요에 따라 각 패치 내에 추가된다. 이 접근법은 지정된 수의 변 분할을 가진 N변 영역을 사각형화하는 알고리즘에 초점을 맞추고 있으며, Nasri 등의 기존 알고리즘은 다양한 변 분할에 대응하는 데 한계가 있으며, 특히 아티스트가 복잡한 모델 부분에 초점을 맞출 때 문제가 된다. 

위의 단점을 해결하기 위해 우리는 삼각형과 직사각형의 영역을 사각형화하기 위해 특별히 설계된 새로운 알고리즘을 도입한다. 이 알고리즘은 원하는 에지 세분화에 관계없이 유효한 사각형 메쉬 토폴로지를 생성할 수 있도록 보장한다. 그러나 우리의 알고리즘은 현재 삼각형과 직사각형 영역만 지원한다. 오각형 영역의 경우, 우리는 여전히 Nasri 등의 방법에 의존하고 있는데, 이는 경우에 따라 불충분할 수 있다.

### 2 Algorithm

이 알고리즘의 핵심은 사각형 메쉬의 경계면은 항상 짝수라는 중요한 이해에 있다. 즉, 4분할 메쉬를 생성할 때, 경계면의 총 분할 개수는 짝수여야 하며, 알고리즘은 이를 유효한 입력으로 간주한다. 이를 고려하면, 알고리즘은 다양한 방법으로 에지 세분화를 조정하여 메쉬를 구축할 수 있는 유연성을 갖게 된다.

![직사각형과 삼각형 영역을 네모로 나누기 위해 사용되는 기본적인 위상 패턴입니다. 빨간색 원으로 표시된 꼭지점은 모서리를 나타냅니다. 오렌지색과 파란색으로 채워진 꼭지점은 각각 3과 5의 밸런스를 가진 불규칙한 꼭지점을 나타냅니다. pX와 pY는 직사각형의 해당 측면에 채워진 정규 격자 네모를 나타냅니다(간결성을 위해 패턴 1에서만 표시됨). pB, pR, pL도 삼각형에 대해 유사하게 정의됩니다. α와 β는 이 위치에 삽입된 엣지 흐름의 횟수를 나타냅니다. Nasri 등 [2009]에 의해 제안된 패턴에 비해 우리의 모든 패턴은 한 변만이 하나의 엣지만 가지고 있어, 엣지의 세분화 수가 매우 작고 극단적일 때도 영역의 유효한 네모 나누기를 보장하는 데 중요합니다.](Robust%20and%20Controllable%20Quadrangulation%20of%20Triangu%20dab3a7ce6edd444ab46fd9e5661b616d/Untitled.png)

직사각형과 삼각형 영역을 네모로 나누기 위해 사용되는 기본적인 위상 패턴입니다. 빨간색 원으로 표시된 꼭지점은 모서리를 나타냅니다. 오렌지색과 파란색으로 채워진 꼭지점은 각각 3과 5의 밸런스를 가진 불규칙한 꼭지점을 나타냅니다. pX와 pY는 직사각형의 해당 측면에 채워진 정규 격자 네모를 나타냅니다(간결성을 위해 패턴 1에서만 표시됨). pB, pR, pL도 삼각형에 대해 유사하게 정의됩니다. α와 β는 이 위치에 삽입된 엣지 흐름의 횟수를 나타냅니다. Nasri 등 [2009]에 의해 제안된 패턴에 비해 우리의 모든 패턴은 한 변만이 하나의 엣지만 가지고 있어, 엣지의 세분화 수가 매우 작고 극단적일 때도 영역의 유효한 네모 나누기를 보장하는 데 중요합니다.

토폴로지 패턴 사용
이 알고리즘의 주요 전략은 미리 정의된 토폴로지 패턴에 의존하는 것이다. 이는 입력 제약 조건에 따라 메시를 구축하는 방법을 지시하는 레이아웃 또는 템플릿이다. 직사각형 영역에는 4가지, 삼각형 영역에는 3가지 패턴이 있다. 요청된 에지 세분화에 따라 특정 패턴이 선택된다. 일부 패턴은 과거 연구에서 전환된 것이고, 다른 패턴은 새로 도입된 것이다. 중요한 것은 각 패턴이 얼마나 많은 세분화가 입력되더라도 불규칙한 정점 수를 일정하게 유지하여 일관성을 보장하는 것이다.

![패치 패턴(여기서는 패턴 2, cf. 그림 1 참조)을 사용하여 네모만으로 나누기를 생성합니다. 패치 모서리는 빨간색 경계를 가진 원으로 표시됩니다.](Robust%20and%20Controllable%20Quadrangulation%20of%20Triangu%20dab3a7ce6edd444ab46fd9e5661b616d/Untitled%201.png)

패치 패턴(여기서는 패턴 2, cf. 그림 1 참조)을 사용하여 네모만으로 나누기를 생성합니다. 패치 모서리는 빨간색 경계를 가진 원으로 표시됩니다.

적절한 패턴 선택
패턴의 선택은 마주보는 변의 세분화 사이의 차이에 따라 달라진다. 직사각형 영역의 경우 이러한 차이를 dX와 dY라고 하며, 패턴 선택에 대한 지침이 된다. 간단한 경우 간단한 선택이 가능하지만, 더 복잡한 세분화 분포에서는 올바른 패턴을 선택하기 위해 추가 조건을 평가해야 한다. 삼각형 영역은 정렬된 변의 길이에 따라 비슷한 과정을 거친다. 이 길이에 따라 세 가지 패턴 중 하나가 선택된다.

패턴 매개변수 계산
패턴이 선택되면 다음 단계는 가장자리 세분화 요구 사항을 충족하기 위해 사양을 계산하는 것이다. 여기에는 패딩(여분의 공간 추가)의 양과 에지 플로우 삽입 등 다양한 파라미터의 계산이 포함된다. 각 패턴에 대해 고유한 구조에서 도출된 특정 공식이 이러한 값을 결정하는 데 도움이 된다.

![패딩을 반대편에 추가함으로써(상단) 또는 패턴의 중앙에(하단) 불규칙한 꼭지점을 이동시킵니다.](Robust%20and%20Controllable%20Quadrangulation%20of%20Triangu%20dab3a7ce6edd444ab46fd9e5661b616d/Untitled%202.png)

패딩을 반대편에 추가함으로써(상단) 또는 패턴의 중앙에(하단) 불규칙한 꼭지점을 이동시킵니다.

비교와 한계
소개된 알고리즘은 4분할 메쉬를 생성하는 체계적인 방법을 제공하지만, 유일한 방법은 아니며, 나스리 등의 연구와 같은 과거 연구는 유사한 입력에 대한 대체 메쉬 토폴로지를 제공했다. 그러나 현재의 알고리즘은 불규칙한 정점의 연결성을 더 잘 보장하고 더 바람직한 메시 구조로 유도한다. 주의해야 할 한계는 이 접근법을 5변 이상의 다각형으로 확장하는 것이 더 복잡하고 현재로서는 미래의 과제로 간주된다는 것이다. 실제로 사용자는 삼각형과 직사각형 패턴을 결합하여 이러한 영역을 메쉬화할 수 있다.

요약하면, 이 알고리즘은 입력된 변의 세분화를 기반으로 사각형 메쉬를 생성하는 구조화된 방법을 제공하며, 일관성을 보장하고 사용자의 요구를 충족시키기 위해 사전 정의된 패턴과 수학적 계산에 의존하고 있다.

### 3 Proof of algorithm robustness

이 섹션에서는 다양한 변 분할 수를 가진 삼각형 영역과 직사각형 영역의 사각형화를 위해 설계된 알고리즘의 견고성을 증명한다. 이 알고리즘은 유효 사각형 분할을 달성할 수 있도록 보장하는 타당성 조건이라고 불리는 특정 조건을 기반으로 작동한다. 이러한 조건을 다양한 패턴에 대해 설명한다.

3.1 사각형 영역의 사분할

직사각형 영역에서 변 사이의 차이는 dX=B-T와 dY=R-L로 주어지며, dX가 0인 경우, 영역은 단순히 표준 격자를 사용하여 사각형으로 분할할 수 있다. 사각형화에는 다양한 패턴이 있다:

패턴 1: 이 패턴은 dX=dY>0일 때 적용된다. 이 패턴과 관련된 조건은 dX가 dY와 같을 때 모든 경우에 유효하다는 것을 보장한다.

패턴 2: 이 패턴은 dX>dY일 때 나타난다. 이는 관련 타당성 조건 B≦R+T+L이 충족되면 dX>dY인 모든 경우를 포함한다. 이 조건은 2009년 Nasri 등(Nasri et al.)도 인용하고 있다.

패턴 3: dX>dY이고 dY=0인 경우 패턴 3이 이용된다. 이러한 경우 모두 이 패턴이 유효하다.

패턴 4: dX>dY>0인 마지막 경우에 사용된다. 관련 조건은 이 패턴이 이러한 경우에 유효하다는 것을 보장한다.

![극단적인 경우에 직사각형 영역의 네모 나누기를 [Nasri 등. 2009]의 패턴(왼쪽)과 우리의 패턴 4(오른쪽)를 사용하여 수행합니다. 우리의 패턴은 불규칙한 꼭지점이 가능한 한 동일한 엣지 흐름으로 연결되도록 보장한다는 점을 주목하십시오.](Robust%20and%20Controllable%20Quadrangulation%20of%20Triangu%20dab3a7ce6edd444ab46fd9e5661b616d/Untitled%203.png)

극단적인 경우에 직사각형 영역의 네모 나누기를 [Nasri 등. 2009]의 패턴(왼쪽)과 우리의 패턴 4(오른쪽)를 사용하여 수행합니다. 우리의 패턴은 불규칙한 꼭지점이 가능한 한 동일한 엣지 흐름으로 연결되도록 보장한다는 점을 주목하십시오.

3.2 삼각형 영역의 사각형화

삼각형 영역의 경우, 변의 차이로 인해 고유한 조건을 가진 유사한 패턴이 성립한다.

패턴 1: 이 패턴은 B=R≧L의 모든 경우에 유효하며, B≦R+L이라는 또 다른 조건도 이 패턴과 관련이 있으며, 이 조건은 2009년에 Nasri 등에서도 언급하였다.

패턴 2: B>R+L이고 R=L인 경우 이 패턴이 채택된다. 이 패턴의 유효성을 보장하기 위해 매개변수 α에 근거한 조건이 설정된다. 관련 조건은 B>R+L이고 R=L일 때 이 패턴이 유효하다는 것을 보장한다.

패턴 3: 마지막 패턴은 B>R+L이고 R>L인 경우에 사용된다. 패턴 2와 마찬가지로 타당성 조건은 파라미터 α를 기반으로 한다. R>L의 모든 경우를 커버한다.

결론적으로, 이 알고리즘의 견고성은 패턴과 각각의 타당성 조건의 확립을 통해 삼각형 영역과 직사각형 영역 모두에 대한 유효한 사각형화를 보장한다.