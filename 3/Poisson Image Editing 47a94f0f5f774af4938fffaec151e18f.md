# Poisson Image Editing

[https://www.cs.jhu.edu/~misha/Fall07/Papers/Perez03.pdf](https://www.cs.jhu.edu/~misha/Fall07/Papers/Perez03.pdf)

푸아송 방정식은 이미지 영역을 매끄럽게 편집할 수 있는 새로운 도구를 만들기 위한 토대를 제공합니다. 이러한 도구를 사용하면 불투명 이미지 영역과 투명 이미지 영역을 이음새 없이 대상 영역으로 가져올 수 있습니다. 또한 선택한 영역 내에서 이미지의 텍스처, 조명 및 색상에 영향을 주거나 타일링 가능한 직사각형 선택 영역을 만드는 등 이미지의 모양을 매끄럽게 수정할 수 있습니다. 이러한 기술은 대화형 이미지 편집, 이미지 그라데이션 기반 작업, 가이드 보간, 원활한 복제와 같은 다양한 애플리케이션에 사용되어 이미지 처리 및 컴퓨터 비전 작업에서 향상된 사용자 경험을 제공합니다.

### 1 Introduction

이 작업의 목표는 수동으로 선택한 영역에 국한된 이미지의 국소적인 변경을 원활하고 손쉽게 수행할 수 있도록 하는 것입니다. 기존 툴을 사용하면 이음새가 보이는 경우가 많지만, 제안된 접근 방식은 디리클레 경계 조건이 포함된 푸아송 편미분 방정식을 사용하여 원활한 편집 및 복제를 달성합니다.

푸아송 방정식은 인간의 시각 시스템이 느린 강도 그라데이션보다 라플라시안 연산자에 의해 포착되는 2차 변화에 더 민감하다는 사실에 착안한 것입니다. 도메인에 대한 미지 함수의 라플라스 연산자와 그 경계 조건을 지정하면 고유한 해를 찾을 수 있습니다.

이 방법은 주어진 경계 조건에서 안내 벡터장이라고 하는 규정된 벡터장에 가장 가까운 기울기를 갖는 함수를 계산하는 최소화 문제로 해석할 수 있습니다(L2-규범). 결과 함수는 유도 필드의 공간적 변화를 면밀히 따르면서 경계 조건을 안쪽으로 보간합니다.

안내 벡터 필드에 대한 다양한 선택을 통해 오브젝트를 제거 및 추가하고, 사실적인 투명 오브젝트를 만들고, 복잡한 윤곽선을 가진 오브젝트를 처리할 수 있는 원활한 복제 도구를 사용할 수 있습니다. 또한 이 접근 방식을 사용하여 제한된 도메인 내에서 이미지의 모양을 수정하거나 오브젝트의 색상, 질감 또는 조명을 원활하게 변경하거나 타일링 가능한 직사각형 이미지 영역을 매끄럽게 만들 수 있습니다.

푸아송 방정식은 컴퓨터 비전, 특히 이미지 편집 애플리케이션에서 광범위하게 사용되어 왔습니다. 푸아송 방정식을 사용한 이전 연구로는 다음과 같은 세 가지가 있습니다:

Fattal 외(2002) - 하이 다이내믹 레인지(HDR) 이미지의 그라데이션 필드를 재조정하여 더 이상 그라데이션 필드가 아닌 벡터 필드를 생성했습니다. 새로운 이미지는 이 벡터 필드의 발산과 노이만 경계 조건을 오른쪽으로 하여 푸아송 방정식을 풀어서 얻습니다.

Elder와 Goldberg(2001) - 이미지의 가장자리 요소(에지)의 스파스 집합을 통해 이미지를 편집하는 시스템을 도입했습니다. 새 이미지는 새로운 에지 집합과 관련된 색상을 보간하고 에지 주변의 색상에 의해 주어진 디리클레 경계 조건으로 라플라스 방정식을 풀어서 얻습니다.

Lewis(2001) - 선택한 영역의 디테일에서 밝기 성분을 분리하고 밝기를 선택 경계에서 밝기의 조화 보간(라플라스 방정식 풀이)으로 대체하여 모피 이미지에서 반점을 제거했습니다.

원활한 복제를 달성하는 기존 기술로는 Adobe Photoshop 7의 힐링 브러시와 Burt와 Adelson(1983)이 제안한 다중 해상도 이미지 블렌딩이 있습니다. 후자는 라플라시안 피라미드를 사용하여 소스 이미지와 대상 이미지의 콘텐츠를 서로 다른 해상도 수준에서 혼합하지만 푸아송 기반 접근 방식에 비해 몇 가지 제한 사항이 있습니다.

인페인팅 기법 및 예제 기반 보간 방법과 같은 자동 보간 방법은 경계 조건에 대한 지식만을 사용하여 이미지 영역을 채웁니다. 이러한 방법은 다양한 기능을 제공하며 큰 구멍, 질감이 있는 경계, 텍스처 가져오기 등 다양한 상황을 처리할 수 있습니다.

### 2 Poisson solution to guided interpolation

이 섹션에서는 안내 벡터 필드를 사용한 이미지 보간에 대해 설명합니다. 멤브레인 보간이라고 하는 가장 간단한 보간이 최소화 문제의 해법으로 정의되는 유도 보간의 개념을 설명합니다. 그러나 이 간단한 방법은 이미지 편집 애플리케이션에서 보간이 흐릿해질 수 있습니다. 이를 극복하기 위해 저자는 최소화 문제의 확장 버전에서 사용되는 벡터 필드인 안내 필드를 도입했습니다. 이 안내 필드는 컬러 이미지의 푸아송 편집에 기본이 되는 것으로, 선택한 색상 공간의 세 가지 색상 채널에서 세 가지 푸아송 방정식을 독립적으로 해결합니다.

![안내 보간 표기법. 알 수 없는 함수 f는 소스 함수 g의 그라데이션 필드일 수도 있고 아닐 수도 있는 벡터 필드 v의 안내에 따라 Ω 영역에서 대상 함수 f ∗를 보간합니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled.png)

안내 보간 표기법. 알 수 없는 함수 f는 소스 함수 g의 그라데이션 필드일 수도 있고 아닐 수도 있는 벡터 필드 v의 안내에 따라 Ω 영역에서 대상 함수 f ∗를 보간합니다.

그런 다음 이산 이미지에 대해 다양한 방식으로 이산화하여 풀 수 있는 이산 푸아송 솔버에 대해 설명합니다. 또한 이산 이차 최적화 문제를 산출하는 변형 문제의 유한 차분 이산화에 대해 설명합니다. 이 문제에 대한 해법은 일련의 동시 선형 방정식을 만족하며, 고전적인 희소 대칭 양-정확 시스템을 형성합니다. 연속적인 과완화를 사용하는 가우스-사이델 반복 또는 V-사이클 멀티그리드와 같은 반복 솔버를 사용하면 중간 크기의 컬러 이미지 영역의 인터랙티브 편집에 충분히 빠르게 결과를 계산할 수 있습니다.

### 3 Seamless cloning

이 섹션에서는 안내 보간을 위한 그라데이션 가져오기에 대해 설명합니다. 안내 필드 v의 기본 선택은 소스 이미지 g에서 직접 가져온 그라데이션 필드입니다. 이 안내 필드를 사용한 원활한 복제는 소스 및 대상 경계를 준수하므로 기존 복제보다 더 유연하고 쉽게 사용할 수 있습니다. 이 방법은 원하지 않는 이미지 특징을 숨기거나, 새로운 요소를 삽입하거나, 소스 이미지와 대상 이미지의 특징을 정렬하는 등 다양한 이미지 편집 작업에 사용할 수 있습니다.

![은폐. 배경의 일부를 매끄럽게 가져오면 완전한 개체, 개체의 일부, 원하지 않는 아티팩트를 쉽게 숨길 수 있습니다. 두 예제 모두 다중 스트로크(표시되지 않음)가 사용되었습니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%201.png)

은폐. 배경의 일부를 매끄럽게 가져오면 완전한 개체, 개체의 일부, 원하지 않는 아티팩트를 쉽게 숨길 수 있습니다. 두 예제 모두 다중 스트로크(표시되지 않음)가 사용되었습니다.

![삽입. 이 방법은 복잡한 윤곽선을 가진 개체를 새 배경에 삽입할 때 그 위력을 충분히 발휘합니다. 이 경우 소스와 대상 간의 차이가 심하기 때문에 표준 이미지 복제를 사용할 수 없습니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%202.png)

삽입. 이 방법은 복잡한 윤곽선을 가진 개체를 새 배경에 삽입할 때 그 위력을 충분히 발휘합니다. 이 경우 소스와 대상 간의 차이가 심하기 때문에 표준 이미지 복제를 사용할 수 없습니다.

때로는 색상이 아닌 강도 패턴과 같이 소스 콘텐츠의 일부만 전송하는 것이 바람직할 수 있습니다. 한 가지 간단한 해결책은 소스 이미지를 미리 흑백으로 전환하는 것입니다.

![기능 교환. 원활한 복제를 통해 사용자는 한 개체의 특정 기능을 다른 기능으로 쉽게 대체할 수 있습니다. 두 번째 텍스처 교환 예시에서는 여러 개의 넓은 스트로크(표시되지 않음)가 사용되었습니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%203.png)

기능 교환. 원활한 복제를 통해 사용자는 한 개체의 특정 기능을 다른 기능으로 쉽게 대체할 수 있습니다. 두 번째 텍스처 교환 예시에서는 여러 개의 넓은 스트로크(표시되지 않음)가 사용되었습니다.

![흑백 전송. 텍스처 전송과 같은 경우에 따라 심리스 복제 후 소스 색상의 일부가 남는 것이 바람직하지 않을 수 있습니다. 이 문제는 미리 소스 이미지를 흑백으로 전환하면 해결됩니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%204.png)

흑백 전송. 텍스처 전송과 같은 경우에 따라 심리스 복제 후 소스 색상의 일부가 남는 것이 바람직하지 않을 수 있습니다. 이 문제는 미리 소스 이미지를 흑백으로 전환하면 해결됩니다.

구멍이 있는 물체를 추가하거나 부분적으로 투명한 물체를 추가하는 등 대상 이미지 f*의 속성을 소스 이미지 g의 속성과 결합하는 것이 바람직한 경우 저자는 혼합 그라디언트를 제안합니다. 안내 필드 v는 영역의 각 지점에서 f* 또는 g의 더 강한 변화를 유지하여 정의됩니다. 이 혼합 심리스 복제는 소스 이미지의 한 오브젝트를 대상 이미지의 다른 오브젝트에 가깝게 추가할 때 유용하며, 텍스처를 씻어내지 않고 더욱 매력적인 효과를 낼 수 있습니다.

![구멍이 있는 개체 삽입하기. (a) 고전적인 방법인 색상 기반 선택과 알파 마스킹은 시간이 오래 걸리고 종종 원하지 않는 후광이 남을 수 있으며, (b-c) 원본 이미지와 평균을 낸 심리스 복제는 효과적이지 않으며, (d) 느슨한 선택을 기반으로 한 혼합 심리스 복제가 효과적입니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%205.png)

구멍이 있는 개체 삽입하기. (a) 고전적인 방법인 색상 기반 선택과 알파 마스킹은 시간이 오래 걸리고 종종 원하지 않는 후광이 남을 수 있으며, (b-c) 원본 이미지와 평균을 낸 심리스 복제는 효과적이지 않으며, (d) 느슨한 선택을 기반으로 한 혼합 심리스 복제가 효과적입니다.

### 4 Selection editing

이 섹션에서는 원본 이미지에 전적으로 의존하는 안내 필드를 사용하여 제자리에서 이미지 변환을 수행하는 방법에 대해 설명합니다. 이 아이디어를 기반으로 텍스처 평탄화, 공간 선택적 조명 변경, 배경 또는 전경색 수정, 심리스 타일링 등 다양한 이미지 편집 기법을 소개합니다.

![투명한 개체 삽입하기. 혼합 심리스 복제를 사용하면 이 예제의 무지개와 같이 부분적으로 투명한 개체를 쉽게 전송할 수 있습니다. 그라디언트 필드의 비선형 혼합은 각 위치에서 소스 또는 대상 구조 중 더 두드러진 것을 선택합니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%206.png)

투명한 개체 삽입하기. 혼합 심리스 복제를 사용하면 이 예제의 무지개와 같이 부분적으로 투명한 개체를 쉽게 전송할 수 있습니다. 그라디언트 필드의 비선형 혼합은 각 위치에서 소스 또는 대상 구조 중 더 두드러진 것을 선택합니다.

![한 개체를 다른 개체에 가깝게 삽입하기. 심리스 복제를 사용하면 대상 이미지의 오브젝트가 선택한 영역 Ω에 닿으면 그 안으로 블리딩됩니다. 혼합 그라데이션을 안내 필드로 사용하면 블리딩이 억제됩니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%207.png)

한 개체를 다른 개체에 가깝게 삽입하기. 심리스 복제를 사용하면 대상 이미지의 오브젝트가 선택한 영역 Ω에 닿으면 그 안으로 블리딩됩니다. 혼합 그라데이션을 안내 필드로 사용하면 블리딩이 억제됩니다.

텍스처 평탄화: 이미지 그라데이션 ∇f*는 가장 두드러진 특징만 남기는 스파스 체를 통과합니다. 그 결과 작은 입자 디테일은 씻겨 나가고 주요 구조는 보존된 평평한 모양이 만들어집니다.

![텍스처 평탄화. 푸아송 솔버와 통합하기 전에 가장자리 위치의 그라데이션만 유지함으로써 선택된 영역의 텍스처를 씻어내어 그 내용물이 평평한 면을 갖도록 합니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%208.png)

텍스처 평탄화. 푸아송 솔버와 통합하기 전에 가장자리 위치의 그라데이션만 유지함으로써 선택된 영역의 텍스처를 씻어내어 그 내용물이 평평한 면을 갖도록 합니다.

국소 조명이 변경됩니다: 이미지 로그의 그라데이션 필드를 변환하여 큰 그라데이션은 줄이고 작은 그라데이션은 늘립니다. 이 방법은 노출이 부족한 피사체를 보정하거나 스페큘러 반사를 줄이는 데 사용할 수 있습니다.

![국부 조명의 변화. 선택 영역 내부의 그라데이션 필드에 적절한 비선형 변환을 적용한 다음 푸아송 솔버와 다시 통합하면 이미지의 겉보기 조명을 국부적으로 수정할 수 있습니다. 이 기능은 노출이 부족한 전경 오브젝트를 강조하거나 스페큘러 반사를 줄이는 데 유용합니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%209.png)

국부 조명의 변화. 선택 영역 내부의 그라데이션 필드에 적절한 비선형 변환을 적용한 다음 푸아송 솔버와 다시 통합하면 이미지의 겉보기 조명을 국부적으로 수정할 수 있습니다. 이 기능은 노출이 부족한 전경 오브젝트를 강조하거나 스페큘러 반사를 줄이는 데 유용합니다.

로컬 색상 변경: 푸아송 편집은 색상을 조작할 수 있는 강력한 도구입니다. 원본 컬러 이미지와 선택 항목이 주어지면 서로 다른 색상의 두 가지 이미지 버전을 매끄럽게 혼합할 수 있습니다. 이 기법을 사용하면 관심 있는 일부 개체를 제외한 이미지의 모든 개체를 흑백으로 전환하거나 느슨하게 선택한 개체의 색상을 수정할 수 있습니다.

![로컬 색상 변경. 왼쪽: 관심 물체를 느슨하게 둘러싼 선택 Ω을 보여주는 원본 이미지, 가운데: g를 원본 컬러 이미지로, f∗를 g의 휘도로 설정하여 수행한 배경 탈색, 오른쪽: 원본 이미지의 RGB 채널에 각각 1.5, 0.5, 0.5를 곱하여 소스 이미지를 형성하여 관심 물체를 다시 색칠하는 모습.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%2010.png)

로컬 색상 변경. 왼쪽: 관심 물체를 느슨하게 둘러싼 선택 Ω을 보여주는 원본 이미지, 가운데: g를 원본 컬러 이미지로, f∗를 g의 휘도로 설정하여 수행한 배경 탈색, 오른쪽: 원본 이미지의 RGB 채널에 각각 1.5, 0.5, 0.5를 곱하여 소스 이미지를 형성하여 관심 물체를 다시 색칠하는 모습.

심리스 타일링: 도메인이 직사각형인 경우 푸아송 솔버로 주기적인 경계 조건을 적용하여 콘텐츠를 타일링할 수 있도록 만들 수 있습니다. 이렇게 하면 이음새나 불연속성 없이 매끄럽게 타일링할 수 있는 이미지가 생성됩니다.

![심리스 타일링. 푸아송 솔버와 통합하기 전에 직사각형 영역의 경계에 주기적인 경계 값을 설정하면 타일링 가능한 이미지가 생성됩니다.](Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f/Untitled%2011.png)

심리스 타일링. 푸아송 솔버와 통합하기 전에 직사각형 영역의 경계에 주기적인 경계 값을 설정하면 타일링 가능한 이미지가 생성됩니다.

이러한 기법은 복잡한 선택이나 수동 조정 없이 이미지를 편집할 수 있는 대체 방법을 제공하여 기존 방식에 비해 유연성과 편의성이 뛰어납니다.

- 기타 [https://link.springer.com/content/pdf/10.1007/s41095-015-0027-z.pdf](https://link.springer.com/content/pdf/10.1007/s41095-015-0027-z.pdf)
    
    이 백서에서는 두 이미지를 자연스럽게 혼합하는 것을 목표로 하는 수정 푸아송 블렌딩(MPB)이라는 새로운 이미지 블렌딩 기법을 소개합니다. 푸아송 이미지 편집과 같은 기존 방식은 색상 번짐과 번짐 아티팩트가 발생하여 출력 이미지에 얼룩과 왜곡된 색상을 유발할 수 있습니다.
    
    MPB는 이러한 문제를 해결하고 푸아송 이미지 편집, 이미지 병합, 포토몽타주, 이미지 조화, 오류 허용 이미지 합성 등 다른 블렌딩 기법과 비교합니다. 그 결과 MPB가 다른 방법보다 블리딩 문제를 줄이고 더 자연스러운 합성 이미지를 생성하는 것으로 나타났습니다. 또한 이 기술은 동영상에서 깜박임 없이 효율적으로 물체를 복제하는 데 적합합니다.
    
    이 논문은 여러 섹션으로 구성되어 있습니다. 먼저 관련 연구를 검토한 후 그라데이션 도메인 처리에 대한 배경을 설명합니다. 이어서 제안된 블렌딩 기법에 대한 자세한 설명과 실험 결과 및 다른 기법과의 비교가 이어집니다. 논문은 연구 결과를 요약하는 것으로 마무리됩니다.
    
    2 Related wo
    
    두 이미지를 매끄럽게 혼합하기 위해 다양한 이미지 블렌딩 기술이 개발되었습니다. 이러한 기법 중 일부는 다음과 같습니다:
    
    알파 블렌딩: 이미지 형식에서 알파 채널을 사용하여 투명도를 표현하고 소스 이미지와 대상 이미지의 원래 색상을 보존합니다. 그러나 이미지의 조명이 다른 경우 바람직하지 않은 결과가 나올 수 있습니다.
    
    힐링 브러시: 포토샵 7.0에 도입된 기능으로 텍스처와 조명이 다른 이미지 영역을 매끄럽게 복제할 수 있습니다. 디자이너가 원활한 이미지 복제를 위해 가장 많이 사용하는 도구입니다.
    
    다중 스케일 표현: 다중 스케일 피라미드를 사용하여 텍스처를 일치시키고 텍스처와 노이즈 패턴이 다른 이미지 간에 모양을 전송합니다. 소스 이미지와 대상 이미지에 확률적 텍스처가 없는 경우 만족스럽지 않은 결과를 얻을 수 있습니다.
    
    평균값 좌표: 가중치를 사용하여 내부 경계 픽셀을 보간합니다. 이 방법은 대상 이미지와 소스 이미지의 경계에 가까운 픽셀의 텍스처가 거의 비슷할 때 좋은 결과를 얻을 수 있습니다. 그러나 오목한 영역에는 문제가 있습니다.
    
    패치 기반 방법: 이미지 복제 및 인페인팅에 널리 사용됩니다. 이 방법은 패치 매칭에 중점을 두고 강력한 결과를 제공하지만 성능보다 품질이 강조될 수 있습니다.
    
    그라디언트 도메인 기법: 픽셀 강도 대신 그라데이션 도메인을 이미지 복제에 사용합니다. 푸아송 이미지 편집이 그 예이지만 색상 번짐과 번짐 아티팩트 문제가 있습니다.
    
    이러한 기법을 개선하기 위해 색상 번짐, 아티팩트, 부자연스러운 블렌딩과 같은 문제를 해결하기 위해 다른 많은 방법이 개발되었습니다. 각 방법에는 장단점이 있으며, 블렌딩하는 이미지의 특정 요구 사항에 따라 그 효과가 달라집니다.
    
    3 Background
    
    푸아송 이미지 편집과 같은 그라데이션 도메인 기술은 특정 함수를 최소화하여 이미지를 블렌딩합니다. 이 프로세스에는 소스 이미지와 대상 이미지의 그라데이션과 대상 이미지 경계의 픽셀 강도를 사용하는 것이 포함됩니다. 목표는 자연스러운 모양을 유지하면서 소스 이미지 픽셀을 대상 이미지에 매끄럽게 복제하는 것입니다.
    
    픽셀 블렌딩의 최소화 문제는 다음과 같이 표현됩니다:
    최소 f ∫∫Ω |Of|^2, f|∂Ω = f*|∂Ω에 따름.
    
    여기서 f는 생성된 이미지의 미지 스칼라 함수, Of는 f의 그라데이션, f*는 대상 이미지의 스칼라 함수, Ω은 미지 영역, ∂Ω은 경계 조건입니다. 이 방정식을 풀면 생성된 이미지의 블렌딩 영역에서 미지의 픽셀의 강도 값을 결정할 수 있습니다.
    
    그러나 푸아송 이미지 편집에는 블리딩 아티팩트와 컬러 블리딩이라는 두 가지 주요 문제가 있습니다. 블리딩 아티팩트는 블렌딩된 영역의 경계에 얼룩이 생기는 것을 말하며, 컬러 블리딩은 생성된 이미지에서 대상 이미지의 색상이 소스 이미지의 색상을 지배하는 것을 말합니다. 이러한 문제는 대상 이미지의 경계 픽셀 강도와 알 수 없는 영역의 픽셀 강도 간의 종속성 때문에 발생합니다. 결과적으로 대상 이미지의 색상이 소스 이미지의 통합 영역을 지배하여 번짐과 부자연스러운 블렌딩이 발생할 수 있습니다.
    
    4 Modified Poisson blending
    
    수정 푸아송 블렌딩(MPB) 기술은 대상 이미지와 소스 이미지의 경계 픽셀에 대한 의존도를 보다 균형 있게 조정하여 블리딩 아티팩트와 색상 번짐을 줄이는 것을 목표로 합니다. 이는 2단계 프로세스와 알파 합성을 통해 이루어집니다.
    
    첫 번째 단계에서 MPB는 블렌딩 영역을 반전하여 이진 마스크의 보간으로 지정된 대상 이미지의 픽셀을 마스크의 보간으로 지정된 소스 이미지의 해당 픽셀과 블렌딩합니다. 이렇게 하면 합성 이미지 I1이 생성됩니다.
    두 번째 단계에서 MPB는 반전되지 않은 마스크를 사용하여 표준 푸아송 블렌딩을 수행하여 두 번째 이미지 I2를 생성합니다.
    마지막으로 MPB는 두 이미지(I1과 I2)를 합성하여 색상 번짐을 줄입니다.
    이를 위해 두 가지 최소화 문제가 해결되며, 그 결과 다음 방정식이 도출됩니다:
    
    |Np|fp - Σq∈Np∩Ω fq = Σq∈Np∩∂Ω ftq + Σq∈Np wpq (10)
    |Np|f2p - Σq∈Np∩Ω₀ f2q = Σq∈Np∩∂Ω₀ fsq + Σq∈Np tpq (11)
    이 방정식은 소스(fs) 및 대상(ft) 이미지의 경계 픽셀에 대한 균형 잡힌 의존성을 보장하여 합성된 이미지 I2의 아티팩트를 줄이는 데 도움이 됩니다.
    
    색상 번짐을 해결하기 위해 생성된 이미지 I1 및 I2를 사용하여 추가 알파 블렌딩 단계가 적용됩니다. 이렇게 하면 최종 합성 이미지 I3에서 소스 이미지 색상이 대상 이미지 색상에 의해 지배되는 것을 방지할 수 있습니다:
    
    I3(x,y) = (1 - MA(x,y))I2(x,y) + MA(x,y)I1(x,y) (12)
    
    바이너리 마스크 MA는 필터링 프로세스, 형태학적 연산 및 평균 커널을 사용하여 대상 이미지 H와 합성 이미지 I2에서 생성됩니다. 마지막으로 대칭 가우시안 저역 통과 필터를 적용하여 최종 알파 마스크 MA를 생성합니다.
    
    수정 푸아송 블렌딩 기법은 경계 픽셀에 대한 의존성을 보다 균형 있게 보장하고 추가 알파 블렌딩을 적용하여 이미지 블렌딩에서 블리딩 아티팩트와 색상 번짐을 효과적으로 줄입니다.
    
    수정 푸아송 블렌딩(MPB) 기법은 Matlab에서 세 단계로 구현됩니다:
    
    첫 번째 단계에서는 원본 푸아송 블렌딩 작업에서 소스 이미지가 타깃 이미지로 사용되고 타깃 이미지가 소스 이미지로 사용됩니다. 원본 바이너리 마스크가 반전되어 합성 이미지 I1이 생성됩니다. 이 이미지는 두 번째 단계에서 소스 이미지로 사용됩니다.
    
    두 번째 단계에서는 원본 대상 이미지가 원본 바이너리 마스크와 푸아송 블렌딩 작업에서 대상 이미지로 사용되어 두 번째 합성 이미지 I2를 생성합니다. 이 단계에서는 소스 이미지와 타깃 이미지 모두에 대한 의존성을 보장하여 아티팩트를 줄입니다.
    
    세 번째 단계에서는 이전 단계에서 생성된 이미지(I1 및 I2)를 알파 블렌딩 연산을 사용하여 블렌딩합니다. 원본 대상 이미지와 두 번째 단계에서 생성된 이미지가 바이너리 마스크를 결정하는 데 사용됩니다. 이 단계에서는 이미지 I3을 생성하여 색상 번짐 문제를 해결합니다.
    
    MPB 기법은 소스 이미지와 대상 이미지의 경계 픽셀에 대한 의존도를 보다 공평하게 유지함으로써 푸아송 이미지 편집 방식에 비해 아티팩트를 줄입니다. 색상 번짐 문제는 추가 알파 합성 단계를 사용하여 해결되며, 합성 프로세스 후에도 소스 이미지의 원본 색상을 잘 보존합니다.
    
    최종 알파 마스크(MA)는 합성 이미지의 다양한 버전을 생성하는 T, hA, n1, n2, σ의 값을 변경하여 생성됩니다. T 값은 마스크의 크기에 정비례하며, T 값이 클수록 더 큰 마스크를 생성합니다. hA, n1, n2, σ 값이 높을수록 마스크가 비례적으로 더 흐릿해집니다.
    
    5 Results and discussion
    
    수정 푸아송 블렌딩(MPB) 기법은 다양한 소스 이미지와 대상 이미지에 대해 테스트되었으며 다른 복제 방법에 비해 좋은 결과를 보여주었습니다. 이 방법은 알파 합성과 그라데이션 도메인 기술을 결합하여 원본 이미지의 원본 색상을 보존합니다. MPB는 푸아송 블렌딩에서 발생하는 색상 번짐과 블리딩 아티팩트를 줄이고 원본 이미지 색상을 변경 없이 보존하는 데 있어 스케치2포토보다 뛰어난 성능을 발휘합니다.
    
    다른 방법에 비해 MPB는 아티팩트를 줄이고 대상 및 소스 경계 픽셀에 대한 의존도를 보다 공평하게 유지하여 원활한 복제가 가능합니다. MPB의 시간 복잡도는 소스 이미지 크기에 관계없이 거의 동일한 시간이 걸리기 때문에 대형 소스 이미지의 푸아송 이미지 편집과 유사합니다. 이는 MPB의 두 번째 단계에서 미지의 영역의 크기가 작기 때문입니다.
    
    비디오 처리 측면에서 MPB는 합성된 이미지의 블리딩 아티팩트를 줄여 깜박임을 최소화합니다. 다른 기법에 비해 동영상에서 블렌딩이 눈에 띄지 않아 프레임 수가 많은 동영상에서 인페인팅에 더 적합합니다. 소스 이미지와 대상 이미지 모두에서 잘린 관심 영역에만 블렌딩 프로세스를 적용하기 때문에 24fps, 1920x1080픽셀 비디오의 200프레임을 처리하는 데 필요한 시간에서 볼 수 있듯이 MPB는 다른 블렌딩 기법보다 우수한 성능을 보여줍니다.