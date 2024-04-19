# LRM: LARGE RECONSTRUCTION MODEL FOR SINGLE IMAGE TO 3D

[https://arxiv.org/abs/2311.04400](https://arxiv.org/abs/2311.04400)

[https://yiconghong.me/LRM/](https://yiconghong.me/LRM/)

- Nov 2023

### 1 INTRODUCTION

이 백서에서는 산업 디자인, 애니메이션, 게임, AR/VR과 같은 분야의 오랜 과제를 해결하기 위해 단일 이미지에서 새로운 3D 재구성 방법을 개발하는 방법에 대해 설명합니다. 기존의 접근 방식은 특정 오브젝트 범주에 초점을 맞추거나 집중적인 파라미터 튜닝 및 최적화가 필요하다는 점에서 한계가 있었습니다. 또한 모양별 최적화와 같은 기존 방법은 속도가 느리고 비실용적인 경우가 많았습니다.

저자들은 자연어 및 이미지 처리의 성공 요인으로 확장 가능한 신경망(예: 트랜스포머), 대규모 데이터 세트, 자체 감독 학습 목표를 꼽았습니다. 이러한 요소 덕분에 모델은 일반 데이터를 효과적으로 학습할 수 있었습니다. 이 논문은 언어 모델링의 GPT 시리즈와 유사한 접근 방식을 3D 재구성에도 적용할 수 있음을 시사합니다.

이 논문에서는 단일 이미지를 3D 모델로 변환하기 위한 대규모 재구성 모델(LRM)을 제안합니다. LRM은 트랜스포머 기반 인코더-디코더 아키텍처를 활용하여 3D 표현을 학습합니다. 사전 학습된 시각 트랜스포머(DINO)를 이미지 인코더로 사용하고 이미지-삼면 트랜스포머 디코더를 사용하여 2D 이미지 특징을 3D 삼면 형식으로 투영합니다. 이 프로세스에는 교차 주의 및 자체 주의 메커니즘이 포함됩니다. 그런 다음 출력은 삼면 특징 맵으로 재구성되며, 이 맵을 통해 모든 시점에서 이미지를 렌더링할 수 있습니다.

LRM의 설계는 간결하고 확장 가능한 3D 표현을 위해 완전한 트랜스포머 기반 파이프라인과 삼면 NeRF(신경 방사 필드)를 사용하여 확장성과 효율성을 강조합니다. 다른 방법과 달리 LRM은 과도한 3D 인식 정규화나 복잡한 하이퍼 파라미터 튜닝을 피하기 때문에 훈련에 효율적이며 다양한 멀티뷰 이미지 데이터 세트에 적용할 수 있습니다.

이 논문에서는 LRM이 5억 개 이상의 파라미터를 포함하고 다양한 카테고리의 약 100만 개의 3D 모양과 비디오 데이터에 대해 학습된 최초의 대규모 3D 재구성 모델이라고 주장합니다. 이 규모는 더 얕은 네트워크와 더 작은 데이터 세트를 사용했던 이전 방법을 훨씬 능가합니다. 저자는 광범위한 실제 세계 및 생성 모델링된 이미지에서 충실도 높은 3D 형상을 재구성할 수 있는 LRM의 기능을 강조합니다. 특히 LRM은 다운스트림 애플리케이션에 실용적이며, 사후 최적화 없이 단 5초 만에 3D 형상을 생성할 수 있습니다.

### 2 RELATED WORK

백서의 두 번째 섹션인 '관련 연구'에서는 단일 이미지에서 3D로 재구성하는 기술의 발전과 현재 상태에 대한 개요를 제공하며, 이 분야에서 저자들의 새로운 접근 방식을 맥락에 맞게 설명합니다.

단일 이미지를 3D로 재구성하는 초기 작업에서는 포인트 클라우드, 복셀, 메시와 같은 다양한 방법과 부호화된 거리 함수(SDF), 점유 네트워크, 신경 방사 필드(NeRF)와 같은 암시적 표현을 탐색했습니다. 카테고리별 재구성을 위해 3D 템플릿, 시맨틱, 포즈를 형상 선행 요소로 사용하는 연구도 있었습니다. 그러나 이러한 카테고리에 구애받지 않는 방법은 종종 세밀한 디테일을 생성하는 데 어려움을 겪었습니다.

최근의 추세는 사전 학습된 이미지 및 언어 모델을 사용하여 이미지 대 3D 재구성을 위한 시맨틱 및 멀티뷰 가이드를 도입하는 것입니다. Zero-1-to-3 및 Make-It-3D와 같은 이러한 방법에는 입력 이미지 및 카메라 포즈에 대한 컨디셔닝, 텍스트 대 이미지 확산을 안내하기 위한 텍스트 설명, 기하학적 및 의미론적 타당성을 위한 특정 손실과 같은 다양한 기법이 활용됩니다.

이와는 대조적으로 저자들의 대규모 재구성 모델(LRM)은 사전 학습된 시각 언어 모델이나 생성 모델의 안내에 의존하지 않는 순수한 데이터 중심 접근 방식입니다. 대신 최소한의 확장 가능한 3D 감독을 사용하여 임의의 물체를 재구성하는 학습에 중점을 둡니다.

이 섹션에서는 이미지에서 3D 표현을 학습하는 개념에 대해서도 살펴봅니다. 생성 속성을 가진 기존 모델에서는 인코더-디코더 프레임워크를 사용하여 이미지와 3D 데이터의 분포를 모델링했습니다. 그러나 이러한 방법은 풍부한 3D 데이터를 확보하는 데 어려움이 있어 종종 거친 결과를 생성합니다. GINA-3D 및 MCC와 같은 다른 접근 방식은 이미지를 3D 표현으로 변환하기 위해 다양한 기술을 적용하지만 규모, 디테일 및 초점 면에서 한계가 있습니다.

마지막으로 이 섹션에서는 3D를 새로운 방식으로 간주하는 멀티모달 3D 학습에 대해 다룹니다. 초기 시도에서는 인코딩된 이미지와 3D 표현 간의 차이를 최소화했습니다. ULIP 및 CLIP2와 같은 최근 연구는 대조 학습을 통해 3D, 언어 및 이미지를 연결합니다. LRM 방법은 특히 단일 이미지에서 3D로 재구성하는 데 초점을 맞춘다는 점에서 이들과 다릅니다. 또한 저자는 3D 형상에 대한 설명을 생성하고 텍스트 대 3D 생성 모델을 훈련하기 위해 언어-3D 쌍을 사용하는 Cap3D를 언급하며 3D를 대규모 언어 모델과 연결하는 광범위한 추세를 강조합니다.

### 3 METHOD

백서의 '방법' 섹션에서는 단일 이미지에서 3D 재구성을 위해 설계된 시스템인 대규모 재구성 모델(LRM)의 아키텍처와 기능에 대해 자세히 설명합니다.

![단일 이미지를 NeRF로 재구성하기 위한 완전히 차별화 가능한 트랜스포머 기반 인코더-디코더 프레임워크인 LRM의 전체 아키텍처. LRM은 사전 학습된 비전 모델(DINO)을 적용하여 입력 이미지를 인코딩하고(3.1초), 이미지 특징이 교차 주의를 통해 대형 트랜스포머 디코더에 의해 3D 삼면 표현으로 투영된 다음(3.2초), 다층 퍼셉트론이 볼류메트릭 렌더링을 위한 포인트 색상과 밀도를 예측합니다(3.3초). 전체 네트워크는 간단한 이미지 재구성 손실(3.4초)로 약 백만 개의 3D 데이터(4.1초)에 대해 엔드투엔드 방식으로 훈련됩니다.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled.png)

단일 이미지를 NeRF로 재구성하기 위한 완전히 차별화 가능한 트랜스포머 기반 인코더-디코더 프레임워크인 LRM의 전체 아키텍처. LRM은 사전 학습된 비전 모델(DINO)을 적용하여 입력 이미지를 인코딩하고(3.1초), 이미지 특징이 교차 주의를 통해 대형 트랜스포머 디코더에 의해 3D 삼면 표현으로 투영된 다음(3.2초), 다층 퍼셉트론이 볼류메트릭 렌더링을 위한 포인트 색상과 밀도를 예측합니다(3.3초). 전체 네트워크는 간단한 이미지 재구성 손실(3.4초)로 약 백만 개의 3D 데이터(4.1초)에 대해 엔드투엔드 방식으로 훈련됩니다.

LRM 아키텍처
LRM은 몇 가지 주요 구성 요소로 이루어져 있습니다:

이미지 인코더: 사전 학습된 시각적 변환기(ViT)를 활용하여 입력 RGB 이미지를 패치별 특징 토큰으로 인코딩합니다. 특히 3D 공간에서 지오메트리와 색상을 재구성하는 데 필수적인 상세한 구조 및 텍스처 정보 캡처로 잘 알려진 모델인 DINO를 사용합니다.
이미지-투-트라이플레인 디코더: 이 컴포넌트는 이미지와 카메라 기능을 학습 가능한 공간적 위치 임베딩에 투영하여 삼면 표현으로 변환합니다. 기하학적 및 외관 정보를 통해 단일 이미지 재구성에 내재된 모호함을 보정합니다.
삼면 표현: 3D 포인트 특징을 얻기 위해 쿼리되는 세 축으로 정렬된 특징 평면이 포함된 형식을 활용합니다.
볼류메트릭 렌더링: 3D 포인트 피처는 다층 인식(MLP)을 통과하여 볼류메트릭 렌더링을 위한 RGB 및 밀도 값을 예측합니다.
세부 프로세스
카메라 피처는 카메라 외재 매트릭스와 내재 파라미터로 구성됩니다. 이러한 피처는 MLP를 사용하여 정규화되고 임베드됩니다.
크로스 어텐션을 통해 이미지 피처를 쿼리한 다음 이러한 피처를 업샘플링하여 3면 표현을 달성합니다.
디코더의 트랜스포머 레이어는 카메라 기능으로 변조를 처리하며 크로스 어텐션, 자체 어텐션 및 MLP 하위 레이어를 포함합니다.
최종 출력은 업샘플링되어 최종 트라이플레인 표현으로 재형성됩니다.
트라이플레인-NeRF
LRM의 이 측면은 삼면 표현에서 쿼리된 포인트 피처로부터 RGB와 밀도를 예측하기 위해 삼면-NeRF 공식을 활용합니다. 여기에 사용된 MLP는 ReLU 활성화와 함께 여러 선형 레이어를 사용합니다.

훈련 목표
훈련 과정에는 단일 입력 이미지에서 3D 모양을 재구성하고 추가 측면도를 사용하여 가이드를 제공하는 과정이 포함됩니다. 훈련 데이터에는 각 도형에 대해 무작위로 선택된 측면도가 포함됩니다. 재구성 손실은 픽셀 단위의 L2 손실과 지각 이미지 패치 유사성의 조합을 사용하여 계산되므로 렌더링된 뷰가 실측 뷰와 거의 일치하도록 보장합니다.

요약하면, LRM 아키텍처는 단일 이미지에서 3D 모델을 효과적으로 재구성하기 위해 ViT, 삼면 표현, 특수 설계된 디코더와 같은 고급 구성 요소를 결합한 정교한 시스템입니다. 이 훈련 프로세스는 여러 뷰와 상세한 손실 계산을 통합하여 재구성된 모델의 높은 충실도를 보장합니다.

### 4 EXPERIMENTS

백서의 '실험' 섹션에서는 대규모 재구성 모델(LRM)을 테스트하고 평가한 방법을 자세히 살펴볼 수 있습니다.

![단일 이미지에서 LRM으로 재구성한 도형의 새로운 뷰(RGB 및 깊이)를 렌더링한 모습. 훈련 중에 모델이 관찰한 이미지는 없습니다. 생성된 이미지는 Adobe Firefly를 사용하여 생성됩니다. 마지막 두 행은 결과를 오브젝트 오브젝트(GT)의 렌더링된 지상 실측 이미지와 비교합니다. 더 선명한 시각화를 보려면 확대하세요.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled%201.png)

단일 이미지에서 LRM으로 재구성한 도형의 새로운 뷰(RGB 및 깊이)를 렌더링한 모습. 훈련 중에 모델이 관찰한 이미지는 없습니다. 생성된 이미지는 Adobe Firefly를 사용하여 생성됩니다. 마지막 두 행은 결과를 오브젝트 오브젝트(GT)의 렌더링된 지상 실측 이미지와 비교합니다. 더 선명한 시각화를 보려면 확대하세요.

![One-2-3-45(Liu et al., 2023a)와 비교. 체리 피킹을 피하기 위해 처음 세 줄의 입력 이미지는 One-2-3-45의 논문 또는 데모 페이지에 제공된 예제에서 선택되었습니다. 어떤 이미지도 훈련 중에 모델에 의해 관찰되지 않습니다. 더 선명한 시각화를 위해 확대하세요.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled%202.png)

One-2-3-45(Liu et al., 2023a)와 비교. 체리 피킹을 피하기 위해 처음 세 줄의 입력 이미지는 One-2-3-45의 논문 또는 데모 페이지에 제공된 예제에서 선택되었습니다. 어떤 이미지도 훈련 중에 모델에 의해 관찰되지 않습니다. 더 선명한 시각화를 위해 확대하세요.

훈련 및 테스트에 사용된 데이터
LRM은 Objaverse의 합성 3D 에셋과 MVImgNet의 실제 오브젝트 비디오로 구성된 상당한 양의 데이터 세트를 학습했습니다. 이러한 데이터 세트는 모델이 일반화 가능한 교차 형상의 3D를 학습하는 데 도움이 되었습니다. 합성 데이터의 경우 다양한 카메라 각도에서 이미지를 렌더링했으며, 실제 세계 비디오의 경우 이미지를 조정하여 물체의 중심을 맞추고 카메라 포즈를 정규화했습니다. 이 훈련에는 73만 개 이상의 3D 에셋과 22만 개의 동영상이 사용되었습니다.

연구팀은 LRM의 성능을 평가하기 위해 Objaverse, MVImgNet, ImageNet 등 다양한 소스의 이미지를 사용했습니다. 또한 실제 세계에서 캡처한 이미지와 생성된 이미지도 포함했습니다. 이러한 접근 방식을 통해 다양한 입력에 대해 LRM의 효과를 테스트할 수 있었습니다.

구현 세부 사항
LRM 구현의 주요 측면은 다음과 같습니다:

카메라 노멀라이제이션: 이 단계는 카메라 포즈를 모델의 학습 데이터와 정렬하여 정확한 이미지 대 삼차원 모델링을 지원하는 데 필수적이었습니다.
네트워크 아키텍처: 이 모델은 이미지 인코더로 ViT-B/16을 사용했으며 이미지-삼면 디코더와 MLP너프 모두에 여러 레이어를 사용했습니다.
훈련 프로세스: 합성 데이터와 실제 데이터의 균형을 맞추는 데 중점을 두고 NVIDIA A100 GPU에서 LRM을 훈련했습니다. 모델은 약 3일 동안 30회에 걸쳐 훈련되었습니다.
추론: 이 단계에서 LRM은 훈련에 사용된 것과 유사한 정규화된 카메라 파라미터를 가정합니다. 단일 GPU에서 5초 이내에 전체 재구성 프로세스를 완료할 수 있습니다.
결과 및 시각화
결과는 다양한 이미지 유형에서 고충실도 3D 형상을 재구성하는 LRM의 능력을 보여줍니다. 이 백서의 시각화는 이 모델이 복잡한 형상과 텍스처를 정확하게 캡처하여 강력한 일반화 기능을 보여 줍니다. 또한 LRM은 동시 작업인 One-2-3-45와 비교했을 때 디테일과 표면 일관성이 우수한 것으로 나타났습니다.

한계점
인상적인 결과에도 불구하고 LRM에는 몇 가지 한계가 있습니다:

![이 방법의 실패 사례. 세 가지 예시 모두 가려진 영역의 텍스처가 흐릿하고 카메라 매개변수의 가정이 매우 부정확하여 왜곡이 발생했습니다.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled%203.png)

이 방법의 실패 사례. 세 가지 예시 모두 가려진 영역의 텍스처가 흐릿하고 카메라 매개변수의 가정이 매우 부정확하여 왜곡이 발생했습니다.

가려진 영역의 텍스처가 흐릿합니다: 단일 이미지에서 3D로 재구성하는 확률적 특성으로 인해 이 모델은 보이지 않는 영역에 대해 평균 텍스처를 생성하는 경우가 있습니다.
고정 카메라 파라미터: 이 가정은 테스트 이미지의 실제 매개변수와 항상 일치하지 않을 수 있으므로 잠재적으로 왜곡된 재구성을 초래할 수 있습니다.
배경이 없는 물체에 초점 맞추기: LRM은 배경이나 복잡한 장면을 처리하지 않습니다.
램버트 가정: 이 모델은 보기에 따라 달라지는 외관을 고려하지 않으므로 반짝이는 금속이나 광택이 나는 세라믹과 같은 재질을 재구성하는 데는 효과가 제한됩니다.
요약하면, LRM은 단일 이미지에서 3D로 재구성하는 데 있어 상당한 발전을 보여 다양한 입력에 걸쳐 높은 충실도와 디테일을 구현하지만, 특히 더 복잡한 시나리오와 머티리얼을 처리하는 데 있어 추가 개발이 필요한 영역도 있습니다.

### 5 CONCLUSION

결론
이 백서에서는 단일 이미지에서 3D를 재구성하는 분야의 선구적인 프레임워크인 대규모 재구성 모델(LRM)을 소개했습니다. LRM은 100만 개의 3D 오브젝트로 구성된 방대한 데이터 세트에서 학습된 대규모 트랜스포머 기반 아키텍처를 사용하는 것이 특징입니다. 이 모델은 훈련과 고충실도 3D 형상 렌더링 모두에서 효율성이 뛰어나며, 단 5초 만에 완료할 수 있습니다. 네트워크의 단순성과 차별성, 그리고 기본적인 이미지 재구성 손실을 사용하여 엔드 투 엔드 학습이 가능한 능력 덕분에 LRM은 다양한 실제 애플리케이션에 잠재적으로 획기적인 도구가 될 수 있습니다.

향후 연구 방향
이 논문은 향후 연구를 위한 두 가지 주요 방향을 제시합니다:

모델 및 훈련 데이터 확장: 저자들은 LRM의 간단한 트랜스포머 기반 설계를 통해 쉽게 확장할 수 있다고 제안합니다. 개선 사항으로는 더 큰 이미지 인코더, 추가 주의 레이어, 더 높은 해상도의 삼면 표현 등이 있습니다. 또한 LRM은 감독을 위해 멀티뷰 이미지만 필요하기 때문에 다양한 3D, 비디오 및 이미지 데이터 세트를 훈련에 활용할 수 있습니다. 이러한 개선 사항을 통해 모델의 일반화 기능과 재구성 품질이 더욱 향상될 것으로 기대됩니다.
멀티모달 3D 생성 모델로의 확장: 이 논문에서는 언어 설명과 3D 형상을 연결하여 텍스트에서 3D로 효율적으로 생성 및 편집할 수 있도록 LRM의 삼면 표현을 활용할 것을 제안합니다. 이를 통해 텍스트 설명에서 새로운 3D 형상을 생성하는 등 3D 콘텐츠 제작에 새로운 길을 열 수 있습니다.
윤리 선언문
저자들은 LRM이 결정론적이고 제너레이티브 모델에 비해 다양한 비윤리적 콘텐츠를 생성하는 경향이 적지만, 오해의 소지가 있거나 부적절한 이미지를 입력하면 여전히 비윤리적인 3D 오브젝트를 생성할 수 있음을 인정합니다. 연구진은 모델의 학습 데이터에 주로 윤리적 콘텐츠가 포함되어 있다고 강조합니다. 이 모델은 작업을 자동화하여 3D 디자이너의 역할에 영향을 미칠 수 있지만, 크리에이티브 산업의 성장을 촉진하고 접근성을 향상시킬 수 있는 잠재력도 가지고 있습니다.

재현성 성명서
LRM은 공개적으로 사용 가능한 코드베이스를 사용하여 구축되고 공개적으로 액세스 가능한 데이터로 학습되어 재현성을 지원합니다. 이 백서에서는 데이터 전처리, 네트워크 아키텍처 및 교육에 대한 광범위한 세부 정보를 제공하여 해당 분야의 다른 사람들이 이 작업을 재현하고 구축할 수 있도록 지원합니다.

요약하자면, LRM은 3D 재구성 분야에서 중요한 진전을 이루었으며, 광범위한 애플리케이션을 처리할 수 있는 매우 효율적이고 확장 가능한 모델을 제공합니다. 향후 개발 방향은 멀티모달 3D 콘텐츠 제작 및 일반화 기능을 더욱 발전시킬 수 있는 가능성을 제시합니다. 윤리적 고려 사항과 모델의 재현성 또한 신중하게 다루고 있습니다.

### 부록

A. 모델 구성 요소의 배경

A.1 NeRF

NeRF(신경 방사장): LRM은 핵심 3D 표현으로 NeRF를 채택합니다. NeRF는 공간적으로 변화하는 색상 및 밀도 필드 함수를 모델링하며, 이는 사실적인 디테일의 이미지를 렌더링하는 데 매우 중요합니다.

트라이플레인 NeRF 변형: 이 변형은 복셀 그리드 표현에 비해 효율적이고 복잡성이 낮기 때문에 사용됩니다.

볼륨 렌더링: NeRF는 이미지 재구성 손실을 통해 최적화된 차등 볼륨 렌더링을 사용하므로 LRM의 목적에 효과적입니다.

A.2 트랜스포머 레이어

주의 연산자: 이 연산자는 트랜스포머 디코더에서 중요한 역할을 하며, 입력과 조건 특징 간의 관계를 측정하기 위해 주의 점수를 계산합니다.

다중 헤드 어텐션: 이 접근 방식을 사용하면 조건 특징에 여러 번 주의하여 여러 가지 주의 모드를 캡처할 수 있습니다.

트랜스포머 레이어: 트랜스포머의 세부 주의 계층은 최근 이 분야의 발전에 따라 추가 선형 계층과 함께 멀티 헤드 주의 계층을 사용합니다.

B. 트레이닝 설정

훈련 세부 사항: 학습 속도 조정을 위한 코사인 스케줄, 옵티마이저 설정, 그라디언트 클리핑, 혼합 정밀도 훈련 사용과 같은 추가 훈련 설정 세부 정보를 지정합니다.
효율적인 훈련: 훈련 효율성을 높이기 위해 참조 뷰 크기 조정 및 선택된 영역 재구성 등의 기법이 사용됩니다.

C. 분석

데이터, 하이퍼파라미터, 훈련 방법: 저자는 PSNR, CLIP 유사도, SSIM, LPIPS와 같은 메트릭을 사용하여 이러한 요소들이 LRM의 성능에 미치는 영향을 평가합니다.

합성 데이터와 실제 데이터 비교: 합성 데이터와 실제 데이터를 결합하면 둘 중 하나만 사용하는 것보다 더 나은 결과를 얻을 수 있습니다.

모델 하이퍼 파라미터: NeRF의 레이어 수, 해상도 및 MLP 레이어 조정에 대해 설명합니다.

카메라 포즈 정규화: 훈련에서 카메라 포즈를 정규화하면 모델의 일반화 능력에 큰 영향을 미칩니다.
이미지 수량 및 해상도: 훈련에서 더 많은 측면 뷰와 더 높은 렌더링 해상도는 이미지 품질을 향상시킵니다.

LPIPS 손실: LPIPS 목표는 모델의 성능에 큰 영향을 미치며, 학습 과정에서 그 중요성을 보여줍니다.

![LRM으로 렌더링된 새로운 뷰와 기준 이미지(GT)의 비교. 훈련 중에 모델이 관찰한 이미지는 없습니다. Objaverse의 GT 심도 이미지는 3D 모델에서 렌더링됩니다. 더 선명한 시각화를 위해 확대하세요.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled%204.png)

LRM으로 렌더링된 새로운 뷰와 기준 이미지(GT)의 비교. 훈련 중에 모델이 관찰한 이미지는 없습니다. Objaverse의 GT 심도 이미지는 3D 모델에서 렌더링됩니다. 더 선명한 시각화를 위해 확대하세요.

![단일 이미지에서 LRM이 재구성한 도형의 새로운 보기(RGB 및 깊이)를 렌더링합니다. 훈련 중에 모델이 관찰한 이미지는 없습니다. 생성된 이미지는 Adobe Firefly에 의해 생성됩니다.](LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9/Untitled%205.png)

단일 이미지에서 LRM이 재구성한 도형의 새로운 보기(RGB 및 깊이)를 렌더링합니다. 훈련 중에 모델이 관찰한 이미지는 없습니다. 생성된 이미지는 Adobe Firefly에 의해 생성됩니다.

D. 시각화

추가 시각화: 부록에는 다양한 이미지 소스를 사용하여 재구성된 3D 형상에 대한 추가 시각화가 포함되어 있습니다.
전처리 기능: 카메라로 캡처한 이미지와 생성된 이미지 등 다양한 유형의 이미지를 사전 처리하기 위한 휴리스틱 기능이 구현되어 있습니다.
데모 및 상호 작용성: 이 백서에서는 비디오 데모와 인터랙티브 3D 메시를 위한 익명의 웹페이지를 참조합니다.
요약하면, 부록에서는 LRM의 기술적 측면, 훈련 설정, 다양한 실험에 대한 자세한 분석, 기능에 대한 추가적인 시각적 데모에 대한 포괄적인 인사이트를 제공합니다. 이러한 세부 사항은 LRM을 개발하는 데 들어간 연구의 철저함과 실험의 깊이를 강조합니다.