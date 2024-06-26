# Moving Basis Decomposition for Precomputed Light Transport

[https://www.ppsloan.org/publications/](https://www.ppsloan.org/publications/)

- EGSR 2021

![최근에 출시된 60Hz AAA 타이틀의 프로덕션 씬을 PlayStation 4, PlayStation 5, Windows PC, Xbox One, 그리고 Xbox Series X/S 플랫폼에서 우리의 기술을 사용하여 조명한 장면입니다. 우리는 대규모 실제 응용 프로그램에서 실시간 제약 하에 압축된 렌더링과 무결점 재구성을 가능하게 하는 사전 계산된 조명 전송 데이터의 효율적인 표현 방법을 제시합니다.](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled.png)

최근에 출시된 60Hz AAA 타이틀의 프로덕션 씬을 PlayStation 4, PlayStation 5, Windows PC, Xbox One, 그리고 Xbox Series X/S 플랫폼에서 우리의 기술을 사용하여 조명한 장면입니다. 우리는 대규모 실제 응용 프로그램에서 실시간 제약 하에 압축된 렌더링과 무결점 재구성을 가능하게 하는 사전 계산된 조명 전송 데이터의 효율적인 표현 방법을 제시합니다.

## 1. Introduction

물리 기반 렌더링은 컴퓨터 그래픽스에서 현실적인 전역 조명 모델을 구현하는 중요한 목표 중 하나입니다. 하지만 현대 비디오 게임의 성능 제약으로 인해 큰 장면에서 실시간 전역 조명을 구현하는 것은 여전히 불가능합니다. 따라서 사전 계산된 조명 기술이 여전히 널리 사용되고 있습니다. 이러한 기술 중 하나인 구형 조화 함수 조명 볼륨은 가상 세계의 크기가 커짐에 따라 확장하기 어려운 문제가 있습니다.

예를 들어, 특정 장면의 간접 조명을 구형 조화 함수로 표현하면 1.5GB의 메모리가 필요합니다. 이는 많은 플랫폼에서 사용할 수 있는 GPU 메모리 예산의 대부분을 차지하게 되어, 기하학적 데이터나 재질 데이터를 위한 공간이 거의 남지 않게 됩니다. 우리는 공간적으로 일관된 고차원 신호를 압축하는 문제를 해결하기 위해 이동 기저 분해(MBD)라는 새로운 기저 분해 프레임워크를 제안합니다. 이 방법은 높은 압축 비율을 유지하면서도 무결점 재구성을 제공합니다.

본 논문의 주요 기여는 다음과 같습니다. 첫째, 기존의 여러 선형 기저 분해 방법을 일반화하는 MBD 프레임워크를 제시합니다. 둘째, MBD 문제를 근사적으로 해결하기 위한 알고리즘을 개발하여 매우 큰 문제에도 잘 확장됩니다. 셋째, 사전 계산된 조명 전송에 MBD를 적용하여 기존 방법보다 성능이 향상된 결과를 실험적으로 입증합니다.

이 방법의 주요 한계는 입력 신호의 공간적 일관성에 대한 가정입니다. 이러한 가정과 그 영향에 대해서는 후속 섹션에서 자세히 논의할 것입니다.

## 2. Preliminaries and Related Methods

### 2.1. 기저 분해 프레임워크

우리는 고차원 데이터 벡터를 소수의 기저 벡터의 선형 결합으로 표현하는 문제를 다룹니다. 주어진 공간 좌표와 데이터 벡터 집합에서, 각 데이터 벡터를 적은 수의 기저 벡터로 근사화하여 메모리 효율성을 높이는 것이 목표입니다. 이러한 기저 분해는 메모리 사용량과 근사화 오차를 고려하여, 오차가 공간적으로 매끄럽게 분포되도록 합니다.

### 2.2. 관련 방법

기저 확장 및 차원 축소 기술을 사전 계산된 조명 데이터 압축에 적용한 여러 방법들을 소개합니다.

**K-평균과 K-SVD**: K-평균 벡터 양자화는 각 데이터 벡터를 클러스터 평균 벡터로 표현하며, K-SVD는 전역 사전(dictionary)을 구축하여 희소 선형 결합으로 데이터를 근사화합니다. 그러나 이 방법들은 공간적 좌표와 무관하게 클러스터를 선택하기 때문에 매끄러운 재구성을 보장하지 않습니다.

**PCA와 SVD**: PCA 기반 접근법은 SVD와의 연결로 인해 강력한 오차 경계를 가지지만, 전역 PCA 기저는 희소하지 않으며 근사 오차는 입력 데이터 분포에 크게 의존합니다. 블록 단위 PCA와 클러스터 PCA는 공간적 일관성을 활용하지만, 이 역시 공간적 도메인에서 매끄러운 재구성을 고려하지 않습니다.

**스펙트럼 방법**: 푸리에 변환이나 웨이블릿 변환에 기반한 스펙트럼 방법들은 전역 기저를 사용하지만, 작은 창이나 클러스터에서 작업하여 지역성을 도입할 수 있습니다. 그러나 이 방법들도 클러스터 불연속성 문제를 겪습니다.

**비선형 차원 축소**: 비선형 PCA는 비선형 일관성을 포착하지만 전역 PCA와 동일한 한계를 가지며, 지역 차트 또는 클러스터를 사용하는 방법들은 데이터 도메인에서만 국소적 클러스터 조정을 고려하여 공간적 도메인에서 매끄러운 재구성을 보장하지 않습니다.

**신경망 방법**: 비선형 PCA의 일반화로 신경망을 사용하여 지역 공간에서 사전 계산된 조명 데이터를 표현하지만, 윈도우 간의 불연속성 문제를 겪습니다.

이들 방법들은 독립적인 클러스터를 가정하여 클러스터 경계에서 불연속성을 초래하거나, 보간 후 재구성 접근법을 사용해 추가적인 메모리와 계산을 요구합니다. 우리의 주요 아이디어는 기저 분해 프레임워크와 보간을 결합하여 공간적 연속성을 확보하고, 보간 오차를 최소화하는 것입니다.

![독립 클러스터 및 클러스터 불연속성 문제. 비선형 참조 벡터 필드(d)를 블록 PCA[NNJ05](a, g), 클러스터 PCA[SHHS03](b, f), 그리고 우리의 방법(c, e)을 사용하여 한 클러스터당 하나의 기저 벡터로 4개(왼쪽) 또는 9개(오른쪽) 클러스터로 압축했습니다. 재구성 모델이 클러스터 간의 상호작용을 허용하지 않을 때(a, b, f, g), 클러스터 불연속성 아티팩트가 명확하게 나타납니다. 반면에, 우리의 방법(c, e)은 공간 커널을 통해 인접 클러스터 간의 상호작용을 허용하며, 참조(d)와 시각적으로 구별되지 않습니다. (c)와 (f)는 유사한 오차를 가지고 있지만, (c)가 시각적으로 더 나은 이유는 대략 동등한 오차 솔루션에서 매끄럽고 균일한 오차 분포가 비균일하고 불연속적인 분포보다 더 선호된다는 점을 강조합니다.](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%201.png)

독립 클러스터 및 클러스터 불연속성 문제. 비선형 참조 벡터 필드(d)를 블록 PCA[NNJ05](a, g), 클러스터 PCA[SHHS03](b, f), 그리고 우리의 방법(c, e)을 사용하여 한 클러스터당 하나의 기저 벡터로 4개(왼쪽) 또는 9개(오른쪽) 클러스터로 압축했습니다. 재구성 모델이 클러스터 간의 상호작용을 허용하지 않을 때(a, b, f, g), 클러스터 불연속성 아티팩트가 명확하게 나타납니다. 반면에, 우리의 방법(c, e)은 공간 커널을 통해 인접 클러스터 간의 상호작용을 허용하며, 참조(d)와 시각적으로 구별되지 않습니다. (c)와 (f)는 유사한 오차를 가지고 있지만, (c)가 시각적으로 더 나은 이유는 대략 동등한 오차 솔루션에서 매끄럽고 균일한 오차 분포가 비균일하고 불연속적인 분포보다 더 선호된다는 점을 강조합니다.

## 3. Method

### 3.1. 이동 기저 분해 (MBD)

이동 기저 분해의 핵심은 기저 벡터와 계수를 공간 도메인에서 각각 보간할 수 있는 능력입니다. 우리는 커널 함수를 사용하여 기저 벡터와 계수의 공간적 영향을 제어합니다. 구체적으로, 계수와 기저 벡터의 보간은 다음과 같이 공간 커널 확장을 통해 이루어집니다:

- 계수 보간: c(x)=∑mϕm(x)cm
    
    c(x)=∑mϕm(x)cmc(x) = \sum_{m} \phi_{m}(x)c_{m}
    
- 기저 벡터 보간: b(x)=∑nψn(x)Bn
    
    b(x)=∑nψn(x)Bnb(x) = \sum_{n} \psi_{n}(x)B_{n}
    

여기서 ϕm\phi_{m}ϕm와 ψn\psi_{n}ψn는 각각 계수와 기저 벡터의 공간 커널이며, cmc_{m}cm와 BnB_{n}Bn는 계수와 기저 벡터입니다. MBD는 이러한 커널 확장을 기저 분해 프레임워크와 결합하여 로컬 변화를 지속적으로 적응할 수 있는 능력을 제공합니다.

### 3.2. 최적화 문제

MBD를 최적화 문제로 공식화하여, 잔차 r(x)=f^(x)−f(x)r(x) = \hat{f}(x) - f(x)r(x)=f^(x)−f(x)를 최소화하는 것이 목표입니다. 여기서 f^(x)\hat{f}(x)f^(x)는 MBD 재구성 함수, f(x)f(x)f(x)는 목표 함수입니다. 손실 함수 L(B,c)L(B, c)L(B,c)는 잔차의 제곱합으로 정의되며, 정규화 항을 추가하여 스케일 모호성을 해결합니다:

Lλ(B,c)=L(B,c)+12λ∥c∥F2L_{\lambda}(B, c) = L(B, c) + \frac{1}{2} \lambda \|c\|_{F}^{2}Lλ(B,c)=L(B,c)+21λ∥c∥F2

최종 최적화 문제는 다음과 같습니다:

arg⁡min⁡B,cLλ(B,c)\arg\min_{B, c} L_{\lambda}(B, c)argminB,cLλ(B,c)

이 최적화 문제는 비선형 및 이중 볼록성을 가지며, 공간적으로 지역적인 커널로 인해 희소한 구조를 가집니다.

### 3.3. 솔버

최적화 문제를 해결하기 위해 확률적 준-뉴턴 강하법을 사용합니다. 각 최적화 반복 과정은 다음 단계로 이루어집니다:

1. 공간 샘플을 생성하고, 잔차의 그래디언트 및 헤시안 대각 성분을 평가합니다.
2. 그래디언트와 헤시안을 사용하여 현재의 계수와 기저 벡터 값을 업데이트합니다.
3. 손실이 증가하면, 그래디언트 방향으로 역추적 선형 검색을 수행합니다.

초기 값 설정은 전역 PCA를 사용하여 빠른 수렴을 보장합니다.

![사전 계산된 조명 전송에의 적용. 직접-간접 확산 조명 전송(위)과 직접 하늘 가시성(아래) 구성 요소로 이루어진 고차원 신호를 다양한 방법으로 압축하고 재구성 품질과 결과 근사 오차를 비교합니다. (상단 행의 참조 열은 0 오차 이미지 대신 전체 직접 및 간접 조명을 보여줍니다). MBD가 정량적으로 낮은 오차와 함께 질적으로 무결점 결과를 제공한다는 것이 핵심 관찰입니다. 또한, 기저 벡터 수가 증가함에 따라 오차 수렴이 일관됨을 보여줍니다. MBD는 클러스터 매핑이 암시적이기 때문에 CPCA에 비해 메모리를 덜 사용하면서도 경쟁력 있는 오차를 나타냅니다. (그래프의 점선은 이미지에서 사용된 기저 벡터 수를 나타냅니다).](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%202.png)

사전 계산된 조명 전송에의 적용. 직접-간접 확산 조명 전송(위)과 직접 하늘 가시성(아래) 구성 요소로 이루어진 고차원 신호를 다양한 방법으로 압축하고 재구성 품질과 결과 근사 오차를 비교합니다. (상단 행의 참조 열은 0 오차 이미지 대신 전체 직접 및 간접 조명을 보여줍니다). MBD가 정량적으로 낮은 오차와 함께 질적으로 무결점 결과를 제공한다는 것이 핵심 관찰입니다. 또한, 기저 벡터 수가 증가함에 따라 오차 수렴이 일관됨을 보여줍니다. MBD는 클러스터 매핑이 암시적이기 때문에 CPCA에 비해 메모리를 덜 사용하면서도 경쟁력 있는 오차를 나타냅니다. (그래프의 점선은 이미지에서 사용된 기저 벡터 수를 나타냅니다).

## 4. Results

MBD는 CPCA보다 메모리 사용량이 적고 계산 시간이 빠르면서도 높은 품질의 결과를 제공하며, 압축된 형식에서 직접 렌더링이 가능합니다. 다양한 기저 벡터 수와 공간적 지원 크기에서도 일관된 성능을 보여 실용적이고 효율적인 프레임워크임을 입증했습니다.

![무결점 재구성 비교. 동일한 수의 기저 벡터를 사용하여 비교 방법 간의 차이를 확대된 영역에서 강조합니다. 클러스터 불연속성 문제를 겪는 WBPCA, BPCA, CPCA와 달리, MBD는 참조와 시각적으로 가까운 무결점 재구성을 제공합니다. 입력 데이터는 해상도가 128x128x128 (Sponza/상단) 및 64x64x64 (Cornell/하단)인 조명 전송 연산자 볼륨(4.1절)으로 구성됩니다. 클러스터 불연속성 문제는 연속 재구성을 위해 볼륨 텍스처로 리샘플링하더라도 여전히 나타납니다. 즉, 공간적 고주파수 연속 재구성을 사용하는 것이 저주파수 클러스터 불연속성을 가리는 데 효과적이지 않습니다.](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%203.png)

무결점 재구성 비교. 동일한 수의 기저 벡터를 사용하여 비교 방법 간의 차이를 확대된 영역에서 강조합니다. 클러스터 불연속성 문제를 겪는 WBPCA, BPCA, CPCA와 달리, MBD는 참조와 시각적으로 가까운 무결점 재구성을 제공합니다. 입력 데이터는 해상도가 128x128x128 (Sponza/상단) 및 64x64x64 (Cornell/하단)인 조명 전송 연산자 볼륨(4.1절)으로 구성됩니다. 클러스터 불연속성 문제는 연속 재구성을 위해 볼륨 텍스처로 리샘플링하더라도 여전히 나타납니다. 즉, 공간적 고주파수 연속 재구성을 사용하는 것이 저주파수 클러스터 불연속성을 가리는 데 효과적이지 않습니다.

![기저 벡터 수와 연산자 근사 오차. 기저 벡터 수가 증가함에 따라 목표 조명 전송 연산자와 다양한 근사치 간의 프러베니우스 놈의 차이가 감소합니다. MBD와 CPCA는 유사한 경험적 수렴 속도를 가지지만, MBD는 비적응형 기저 지원 그리드를 사용하기 때문에 메모리를 덜 필요로 합니다. 비교를 위해, BPCA와 WBPCA는 MBD와 동일한 기저 지원 그리드를 사용하지만 수렴 속도는 느립니다. (점선은 비교 결과에서 사용된 기저 벡터 수를 나타냅니다).](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%204.png)

기저 벡터 수와 연산자 근사 오차. 기저 벡터 수가 증가함에 따라 목표 조명 전송 연산자와 다양한 근사치 간의 프러베니우스 놈의 차이가 감소합니다. MBD와 CPCA는 유사한 경험적 수렴 속도를 가지지만, MBD는 비적응형 기저 지원 그리드를 사용하기 때문에 메모리를 덜 필요로 합니다. 비교를 위해, BPCA와 WBPCA는 MBD와 동일한 기저 지원 그리드를 사용하지만 수렴 속도는 느립니다. (점선은 비교 결과에서 사용된 기저 벡터 수를 나타냅니다).

![기저 벡터 수와 근사 품질. 기저 벡터 수 또는 로컬 랭크가 증가함에 따라 재구성 품질을 비교합니다. MBD는 빠르게 수렴하여 4개의 기저 벡터만으로도 참조 솔루션에 시각적으로 가까운 반면, WBPCA, BPCA, CPCA는 15개의 기저 벡터를 사용해도 눈에 띄는 아티팩트를 보입니다(독자가 PDF 이미지를 확대해보길 권장합니다).](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%205.png)

기저 벡터 수와 근사 품질. 기저 벡터 수 또는 로컬 랭크가 증가함에 따라 재구성 품질을 비교합니다. MBD는 빠르게 수렴하여 4개의 기저 벡터만으로도 참조 솔루션에 시각적으로 가까운 반면, WBPCA, BPCA, CPCA는 15개의 기저 벡터를 사용해도 눈에 띄는 아티팩트를 보입니다(독자가 PDF 이미지를 확대해보길 권장합니다).

![기저 벡터 그리드 해상도와 근사 품질. 기저 벡터 그리드 크기의 함수로서 클러스터 수가 9^3 = 729에서 3^3 = 27로 감소할 때 재구성(왼쪽)과 근사 오차(오른쪽)를 비교합니다. 오른쪽 패널은 절대 오차 분포를 시각화하고 상대 평균 제곱 오차(흰색 레이블)를 보여줍니다. WBPCA, BPCA, CPCA는 하드 클러스터 경계에서 눈에 띄는 불연속성을 가지는 반면, MBD는 기저 커널 지원 크기의 변화에 대해 견고함을 보여줍니다.](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%206.png)

기저 벡터 그리드 해상도와 근사 품질. 기저 벡터 그리드 크기의 함수로서 클러스터 수가 9^3 = 729에서 3^3 = 27로 감소할 때 재구성(왼쪽)과 근사 오차(오른쪽)를 비교합니다. 오른쪽 패널은 절대 오차 분포를 시각화하고 상대 평균 제곱 오차(흰색 레이블)를 보여줍니다. WBPCA, BPCA, CPCA는 하드 클러스터 경계에서 눈에 띄는 불연속성을 가지는 반면, MBD는 기저 커널 지원 크기의 변화에 대해 견고함을 보여줍니다.

### 4.1. 사전 계산된 복사선 전송 실험

사전 계산된 복사선 전송에 MBD를 적용하여 실험을 수행했습니다. 실험 데이터는 3D 복사선 전송 볼륨으로, 각 볼륨 셀은 RGB 색상 채널에 대한 구형 조화 기저로 표현된 324차원 벡터를 포함합니다. 우리는 커널 함수로 삼선형 해트 커널을 사용하여 하드웨어 가속 텍스처 조회를 가능하게 했습니다. 압축은 기저 벡터의 수를 줄임으로써 이루어졌으며, CPCA와의 공정한 비교를 위해 모든 방법의 재구성을 3D 삼선형 그리드로 리샘플링했습니다.

### 4.2. 품질과 기저 벡터 수

기저 벡터 수의 변화에 따른 근사화를 질적 및 양적으로 비교했습니다. 고정된 조명 환경에서, MBD는 다른 방법들보다 시각적으로 더 적은 아티팩트를 보여주었습니다. 모든 가능한 조명 환경에서의 수렴 동향을 분석한 결과, 신호 특화된 CPCA와 유사한 성능을 보이면서도 MBD가 더 적은 메모리를 사용함을 확인했습니다.

### 4.3. 품질과 기저 지원 크기

기저 커널의 공간적 지원 크기를 조정하면서 근사화의 변화를 조사했습니다. 다른 방법들이 클러스터 수에 민감한 반면, MBD는 기저 커널 함수의 수가 적어도 일관된 근사화를 제공했습니다. 기저 커널 그리드의 해상도를 낮추면서도 높은 품질을 유지했습니다.

### 4.4. 계산 시간

MBD 솔버는 확률적 준-뉴턴 강하법을 사용하므로 GPU 구현에 적합하지만, 모든 방법을 멀티 스레드 C/C++로 구현하여 공정하게 비교했습니다. Intel Core i9-9960X CPU와 128GB RAM을 사용한 결과, MBD는 기존 방법들에 비해 계산 시간이 빠르며 대규모 문제에도 확장 가능함을 확인했습니다.

![계산 시간. 목표 볼륨/기저 해상도가 증가함에 따라 계산 시간을 비교합니다. 우리의 방법이 순수한 로컬 BPCA보다 약간 느리지만, 모든 해상도에서 CPCA보다 계산이 빠름을 알 수 있습니다.](Moving%20Basis%20Decomposition%20for%20Precomputed%20Light%20T%20c4f3f5ffb0434ba899dfdc92b82a38f1/Untitled%207.png)

계산 시간. 목표 볼륨/기저 해상도가 증가함에 따라 계산 시간을 비교합니다. 우리의 방법이 순수한 로컬 BPCA보다 약간 느리지만, 모든 해상도에서 CPCA보다 계산이 빠름을 알 수 있습니다.

## 5. Discussion

### 5.1. 이점과 한계

MBD의 주요 이점은 블록 기반 방법에 비해 무결점 재구성을 제공하고, 메모리 효율성이 높으며, 직접 렌더링이 가능하다는 점입니다. 또한, 기저와 계수 커널을 선택할 수 있는 유연성을 제공합니다. 그러나 MBD는 입력 데이터의 공간적 일관성을 가정하기 때문에, 장거리 일관성을 포착하는 데는 한계가 있습니다. 또한, MBD는 전역 최적화 문제를 해결해야 하므로 블록 기반 방법보다 복잡하지만, 계산 시간은 여전히 경쟁력이 있습니다.

### 5.2. 텍스처 압축 방법과의 관계

MBD는 텍스처 압축에서 공간적 매끄러움을 활용한 압축과 유사합니다. 특히, PVRTC와 유사하게, MBD는 공간적 커널을 사용하여 정보를 공유하고 압축합니다. PVRTC는 선형 보간된 두 색상 값을 혼합하여 재구성하는데, 이는 MBD의 특수한 경우로 볼 수 있습니다. MBD는 더 일반화된 형태로, 공간적 매끄러움을 유지하면서도 고차원 신호를 압축할 수 있습니다.

### 5.3. RBF와 셰퍼드 방법과의 관계

MBD는 커널 함수를 사용하여 정보 공유를 가능하게 하는 점에서 RBF 확장과 유사합니다. 그러나 RBF는 데이터 도메인에서 정의되며, 공간 도메인에서의 매끄러운 재구성을 고려하지 않습니다. 셰퍼드 방법은 공간 도메인에서 무결점 재구성을 제공하지만, 압축을 위한 분해는 고려하지 않습니다. MBD는 이러한 방법들을 결합하여 분석과 합성을 동시에 가능하게 합니다.

### 5.4. 클러스터링 방법과의 관계

블록 단위 PCA와 K-평균, K-SVD, 클러스터 PCA 등의 클러스터링 방법들은 공간 도메인에서 비연속성을 초래하는 반면, MBD는 커널을 사용하여 연속성을 유지합니다. 기존 방법들은 재구성 후 보간을 필요로 하지만, MBD는 재구성 과정에서 보간을 본질적인 부분으로 포함합니다.

### 5.5. 향후 연구

MBD는 자연 이미지나 다른 문제 도메인에서도 적용 가능성이 있으며, 적응형 커널과 강력한 적응형 랭크 솔버에 대한 추가 연구가 필요합니다. 신호 적응형 커널을 학습하여 효율적으로 평가하고 저장할 수 있는 가능성을 탐구하고, 국소 저차원 성분과 희소 고차원 성분으로 신호를 분리하는 방법도 연구할 가치가 있습니다. 또한, 위치 레이블이 없는 경우 저차원 임베딩을 학습하여 저차원 MBD를 수행하는 가능성도 제안됩니다.

### 요약

MBD는 데이터 도메인에서의 분석과 공간 도메인에서의 무결점 합성을 동시에 달성하기 위해 개발된 방법입니다. 우리는 MBD가 다른 문제 영역에 적용될 가능성이 있으며, 적응형 커널과 강력한 적응형 랭크 솔버에 대한 추가 연구가 필요하다고 결론지었습니다.

## 6. Conclusion

MBD는 데이터 도메인에서의 분석과 공간 도메인에서의 무결점 합성을 공동으로 달성하기 위해 공간 커널을 사용한 이동 기저 분해를 구축합니다. 우리는 MBD가 다른 문제 영역에 적용될 가능성이 있으며, 적응형 커널 및 강력한 적응형 랭크 솔버에 대한 추가 연구가 필요하다고 기대합니다.