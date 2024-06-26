# FLAME:Learning a model of facial shape and expression from 4D scans

[https://flame.is.tue.mpg.de/](https://flame.is.tue.mpg.de/)

[https://github.com/Rubikplayer/flame-fitting](https://github.com/Rubikplayer/flame-fitting)

- Siggraph Asia 2017

![FLAME 예시. 위: D3DFACS 데이터 세트의 샘플. 가운데: 모델 전용 등록. 하단: 모델만을 사용한 Beeler 등[2011]의 피사체에 대한 표현식 전이](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled.png)

FLAME 예시. 위: D3DFACS 데이터 세트의 샘플. 가운데: 모델 전용 등록. 하단: 모델만을 사용한 Beeler 등[2011]의 피사체에 대한 표현식 전이

### 1 INTRODUCTION

서론 부분에서는 3D 얼굴 모델링 기술의 현재 상황과 이 분야에서 마주한 주요 문제점을 설명합니다. 기술적으로는 매우 정교하고 실제와 구분이 어려운 고급 얼굴 모델링 방법이 존재하지만, 이러한 방법들은 대부분 방대한 양의 수동 작업을 필요로 합니다. 반면에, 저렴하고 접근성이 높은 소비자용 깊이 센서를 사용하는 방법은 훨씬 간편하지만, 얼굴의 형태와 표정을 자연스럽게 표현하는 데에는 한계가 있습니다.

이러한 상황에서, 이 연구는 고급 방법과 저급 방법 사이의 격차를 메우고자 합니다. 연구팀은 수천 개의 3D 얼굴 스캔 데이터를 정확하게 정렬하고 학습하여, 새로운 3D 얼굴 모델인 FLAME을 개발했습니다. FLAME 모델은 기존 그래픽 소프트웨어와 호환되면서도, 데이터에 쉽게 적용할 수 있고, 얼굴의 다양한 형태와 표정을 보다 자연스럽게 표현할 수 있는 능력을 갖추고 있습니다. 이 모델은 특히 선형 형태 공간, 조절 가능한 턱, 목, 눈알의 움직임, 그리고 표정 변화를 다루는 데에 특화되어 있습니다.

결론적으로, 서론에서는 3D 얼굴 모델링 기술의 현재 문제를 진단하고, 이를 해결하기 위한 새로운 접근법인 FLAME 모델의 개발 배경과 목표를 소개합니다.

### 2 RELATED WORK

관련 작업 부분에서는 3D 얼굴 모델링 분야에서 이전에 수행된 연구와 기술적 접근 방법들을 검토합니다. 이 섹션은 크게 두 가지 주요 영역으로 나누어 집니다: 얼굴의 형태와 표정을 모델링하는 기존 방법들의 설명, 그리고 이러한 방법들의 한계점과 FLAME 모델이 이를 어떻게 해결하려고 시도하는지에 대한 논의입니다.

**얼굴 형태 모델링**

- 초기 모델들은 주로 유럽인의 젊은 얼굴에서 중성 표정을 기반으로 한 제한된 수의 3D 스캔을 사용하여 만들어졌습니다.
- FaceWarehouse와 같은 최근 모델들은 다양한 나이와 인종의 사람들로부터 20가지 다른 표정을 포함하여 더 많은 데이터를 수집하였지만, 여전히 데이터의 양이 충분하지 않아 모델이 표현할 수 있는 얼굴 형태의 범위가 제한됩니다.

**얼굴 표정 모델링**

- 얼굴 표정에 대한 변화를 모델링하기 위한 여러 접근 방법이 소개되었습니다. 이는 주로 주성분 분석(PCA)을 사용하여 중성 얼굴 형태에서의 표정 변화를 캡처하는 데 초점을 맞춥니다.
- 표정 변화를 모델링하는 데 있어, 특정 표정들을 독립적으로 다루는 방법부터 얼굴 전체의 상관관계를 고려하는 방법까지 다양한 시도가 있었습니다.

**FLAME 모델의 접근 방법**

- FLAME 모델은 이러한 기존 모델들의 한계를 극복하기 위해 더 넓은 범위의 연령, 인종, 성별을 포함한 훨씬 더 많은 데이터에서 학습됩니다.
- 모델은 얼굴의 정체성(Identity), 자세(Pose), 그리고 표정(Expression)을 분리하여 표현하는 방법을 채택함으로써, 각각의 변화를 더 정확하게 캡처하고 조절할 수 있습니다.
- 특히, FLAME은 얼굴 표정과 함께 머리, 목, 눈동자의 움직임도 모델링하여, 전반적인 얼굴 애니메이션의 자연스러움과 실감나는 표현을 개선합니다.

결론적으로, 이 섹션은 3D 얼굴 모델링 기술이 직면한 주요 과제들을 식별하고, 이전 연구들이 어떻게 이 문제들에 접근했는지를 설명합니다. 또한, FLAME 모델이 기존 모델들의 한계를 어떻게 극복하려고 시도하는지에 대한 개요를 제공합니다.

### 3 MODEL FORMULATION

![모델의 매개변수화(여성 모델 표시). 왼쪽: -3에서 +3 표준편차 사이의 처음 세 가지 모양 구성 요소의 활성화. 가운데: 6개의 목과 턱 관절 중 4개의 관절을 회전 방식으로 작동시키는 포즈 파라미터. 오른쪽: 표준 편차 -3에서 +3 사이의 처음 세 가지 표현식 구성 요소 활성화.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%201.png)

모델의 매개변수화(여성 모델 표시). 왼쪽: -3에서 +3 표준편차 사이의 처음 세 가지 모양 구성 요소의 활성화. 가운데: 6개의 목과 턱 관절 중 4개의 관절을 회전 방식으로 작동시키는 포즈 파라미터. 오른쪽: 표준 편차 -3에서 +3 사이의 처음 세 가지 표현식 구성 요소 활성화.

![여성(왼쪽) 및 남성(오른쪽) FLAME 모델의 관절 위치. 분홍색/노란색은 오른쪽/왼쪽 눈을 나타냅니다. 빨간색은 목 관절, 파란색은 턱을 나타냅니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%202.png)

여성(왼쪽) 및 남성(오른쪽) FLAME 모델의 관절 위치. 분홍색/노란색은 오른쪽/왼쪽 눈을 나타냅니다. 빨간색은 목 관절, 파란색은 턱을 나타냅니다.

**FLAME 모델의 주요 구성 요소**

1. **선형 형태 공간**: FLAME 모델은 3,800개의 인간 머리 스캔 데이터로부터 학습된 선형 형태 공간을 사용하여, 다양한 얼굴 형태의 변화를 모델링합니다. 이 형태 공간은 인간 얼굴의 다양성을 포착하여, 성별, 연령, 인종 등 다양한 특성을 반영할 수 있습니다.
2. **이동 가능한 턱, 목, 눈알**: FLAME 모델은 턱, 목, 눈알의 움직임을 별도로 모델링하여, 얼굴의 자세와 방향 변화를 자연스럽게 표현할 수 있습니다. 이를 통해 머리가 돌아가거나 눈이 움직이는 등의 행동을 정밀하게 재현할 수 있습니다.
3. **자세에 따른 보정형 블렌드셰이프**: 자세 변화에 따라 얼굴 형태에 발생할 수 있는 비선형 변형을 보정하기 위해, 자세에 따른 보정형 블렌드셰이프를 사용합니다. 예를 들어, 턱을 움직일 때 발생하는 얼굴의 변형을 보다 정확하게 모델링할 수 있습니다.
4. **전역 표정 블렌드셰이프**: 얼굴의 다양한 표정 변화를 모델링하기 위해, 전역 표정 블렌드셰이프가 사용됩니다. 이는 웃음, 놀람, 슬픔 등의 다양한 표정을 표현할 수 있게 해줍니다.

**모델의 작동 원리**

FLAME 모델은 위의 구성 요소들을 통합하여, 3D 얼굴 모델을 생성합니다. 사용자는 형태(shape), 자세(pose), 표정(expression)에 대한 파라미터를 조절함으로써, 원하는 얼굴 형태와 표정을 정밀하게 조정할 수 있습니다. 이 모델은 선형 및 비선형 변형을 조합하여, 실제 인간 얼굴의 복잡한 형태와 표정 변화를 자연스럽게 재현할 수 있도록 설계되었습니다.

결론적으로, "모델 정의" 부분은 FLAME 모델이 얼굴 형태와 표정을 어떻게 모델링하는지에 대한 기술적인 설명을 제공합니다. 이를 통해 FLAME 모델이 기존의 3D 얼굴 모델링 방법보다 더 높은 수준의 자연스러움과 정확성을 달성할 수 있는 이유를 이해할 수 있습니다.

### 4 TEMPORAL REGISTRATION

"시간적 정렬(Temporal Registration)" 부분은 FLAME 모델을 어떻게 다양한 3D 얼굴 스캔과 동적 시퀀스에 정확하게 맞추어, 모델을 훈련시키는지에 대한 과정을 설명합니다. 이 과정은 모델이 실제 인간 얼굴의 다양한 형태와 표정을 정밀하게 포착하고 재현할 수 있도록 합니다. 여기서는 이 복잡한 과정을 몇 가지 주요 단계로 나누어 간단하게 설명하겠습니다.

![얼굴 등록, 모델 훈련 및 표정 변환 적용에 대한 개요.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%203.png)

얼굴 등록, 모델 훈련 및 표정 변환 적용에 대한 개요.

**초기 모델**

- **초기 모델의 필요성**: 정확한 시간적 정렬을 위해서는 시작점으로 사용될 초기 모델이 필요합니다. 이는 FLAME 모델의 기본 구조와 파라미터를 설정하는 데 사용됩니다.

**단일 프레임 정렬**

1. **모델 기반 정렬**: 첫 번째 단계에서는 모델의 형태와 자세 파라미터를 조절하여 개별 3D 스캔에 최대한 근접하게 맞춥니다. 이 과정은 모델이 스캔 데이터의 기본적인 형태와 표정을 포착할 수 있도록 합니다.
2. **결합 정렬**: 이 단계에서는 스캔 데이터와 모델 간의 정밀한 맞춤을 위해 모델의 형태를 추가로 조정합니다. 이는 스캔 데이터의 세부적인 특징까지 모델이 재현할 수 있도록 돕습니다.
3. **텍스처 기반 정렬**: 마지막 단계에서는 텍스처 정보를 사용하여 모델의 정밀도를 더욱 향상시킵니다. 이는 특히 얼굴의 세부적인 특징과 표정을 더 정확하게 포착하는 데 도움을 줍니다.

**순차적 정렬**

- **개인화된 템플릿 생성**: 각 개인의 고유한 얼굴 특성을 반영하기 위해, 개별적으로 맞춤화된 템플릿을 생성합니다. 이 템플릿은 향후 모든 시퀀스의 정렬 과정에서 기준점으로 사용됩니다.
- **시퀀스 맞춤**: 이제 개인화된 템플릿을 사용하여, 전체 동적 시퀀스를 정렬합니다. 이 과정은 각 프레임마다 모델 파라미터를 조정하여, 얼굴의 움직임과 변화를 정확하게 추적합니다.
    
    ![CMU Intraface 랜드마크 추적기[Xiong and la Torre 2013]에서 예측한 49개의 랜드마크(왼쪽) 및 토폴로지에 정의된 동일한 랜드마크(오른쪽).](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%204.png)
    
    CMU Intraface 랜드마크 추적기[Xiong and la Torre 2013]에서 예측한 49개의 랜드마크(왼쪽) 및 토폴로지에 정의된 동일한 랜드마크(오른쪽).
    

**데이터**

- 이 과정에서 사용된 데이터는 3,800개의 CAESAR 스캔, D3DFACS 데이터셋, 그리고 자체 캡처한 시퀀스 등으로 구성됩니다. 이 데이터는 모델이 다양한 인간 얼굴의 형태와 표정을 포괄적으로 학습할 수 있는 토대를 제공합니다.

![샘플 등록. 위: CAESAR 본체 데이터베이스에서 추출한 형상 데이터. 가운데: 목 주위의 머리 회전(왼쪽)과 입 관절(오른쪽)이 있는 자체 캡처된 포즈 데이터의 샘플 등록. 아래: D3DFACS의 표현 데이터 샘플 등록(왼쪽) 및 자체 캡처 시퀀스(오른쪽). 보충 문서에 추가 등록이 나와 있습니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%205.png)

샘플 등록. 위: CAESAR 본체 데이터베이스에서 추출한 형상 데이터. 가운데: 목 주위의 머리 회전(왼쪽)과 입 관절(오른쪽)이 있는 자체 캡처된 포즈 데이터의 샘플 등록. 아래: D3DFACS의 표현 데이터 샘플 등록(왼쪽) 및 자체 캡처 시퀀스(오른쪽). 보충 문서에 추가 등록이 나와 있습니다.

시간적 정렬 과정은 FLAME 모델이 실제 인간의 다양한 얼굴 형태와 표정을 정밀하게 모델링하고 재현할 수 있도록 하는 핵심적인 단계입니다. 이 과정을 통해, 모델은 개별 인물의 독특한 얼굴 특성과 표정 변화를 정확하게 포착하고 재현할 수 있는 능력을 갖추게 됩니다.

### 5 DATA

"데이터" 부분은 FLAME 모델을 훈련시키기 위해 사용된 다양한 3D 얼굴 스캔 데이터에 대해 설명합니다. 이 데이터는 모델이 실제 인간 얼굴의 다양한 형태와 표정을 학습하고, 이를 정밀하게 모델링할 수 있도록 하는데 필수적입니다. 데이터 세트의 구성, 특성 및 사용 방법에 대해 간단히 요약하겠습니다.

![커플링 에지 가중치가 높은 헤드 영역(왼쪽)과 라플라시안 가중치가 높은 헤드 영역(오른쪽).](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%206.png)

커플링 에지 가중치가 높은 헤드 영역(왼쪽)과 라플라시안 가중치가 높은 헤드 영역(오른쪽).

**데이터 세트 구성**

1. **CAESAR 데이터셋**: FLAME 모델의 핵심 데이터 중 하나로, 3,800개의 다양한 인종, 연령, 성별을 포함하는 전신 스캔 데이터입니다. 이 데이터는 주로 얼굴의 정체성(identity) 형태 공간을 학습하는 데 사용됩니다.
2. **D3DFACS 데이터셋**: 4D 얼굴 캡처 데이터로, 얼굴 표정의 변화를 담고 있습니다. 이 데이터는 FLAME 모델이 얼굴 표정의 다양성을 학습하고 재현할 수 있도록 돕습니다.
3. **자체 캡처한 시퀀스**: 연구팀이 직접 캡처한 추가적인 4D 얼굴 시퀀스 데이터입니다. 이는 모델의 표현력을 더욱 풍부하게 하고, 실제 인간 얼굴의 다양한 움직임과 표정을 포괄적으로 모델링할 수 있도록 합니다.

**데이터의 특성**

- 이 데이터 세트들은 인간 얼굴의 광범위한 변화를 포함하고 있으며, 다양한 인종, 연령, 성별에 걸친 얼굴 형태와 표정의 변화를 포착합니다.
- CAESAR 데이터셋은 주로 얼굴 형태의 다양성을 학습하는 데 사용되며, D3DFACS와 자체 캡처한 시퀀스는 얼굴 표정과 동적인 움직임을 모델링하는 데 중점을 둡니다.

**데이터 사용 방법**

- **얼굴 형태 학습**: CAESAR 데이터셋의 스캔을 사용하여 FLAME 모델의 선형 형태 공간을 학습합니다. 이 공간은 모델이 다양한 인간 얼굴의 기본적인 형태를 재현할 수 있도록 합니다.
- **얼굴 표정과 움직임 학습**: D3DFACS 데이터셋과 자체 캡처한 시퀀스를 사용하여, 모델이 얼굴 표정의 다양한 변화와 동적인 움직임을 정밀하게 모델링할 수 있도록 합니다.
- **시간적 정렬 과정**: 이 데이터를 통해, 모델은 다양한 얼굴 형태와 표정 변화에 대해 정밀한 정렬을 수행하며, 이 과정을 통해 모델의 파라미터를 최적화하고 학습합니다.

결론적으로, "데이터" 부분은 FLAME 모델을 훈련시키는 데 사용된 다양한 3D 얼굴 스캔 데이터 세트의 구성, 특성 및 이를 사용한 모델 학습 과정을 설명합니다. 이 데이터를 통해 모델은 실제 인간 얼굴의 광범위한 변화를 학습하고, 이를 정밀하게 재현할 수 있는 능력을 갖추게 됩니다.

### 6 MODEL TRAINING

"모델 훈련(Model Training)" 부분은 FLAME 모델이 어떻게 다양한 얼굴 형태와 표정을 학습하여 모델링하는지에 대한 과정을 설명합니다. 이 과정은 모델이 실제 인간 얼굴의 복잡한 변화를 정밀하게 재현할 수 있도록 만드는 핵심 단계입니다. 모델 훈련 과정은 크게 세 부분으로 나누어 볼 수 있습니다: 포즈(pose) 파라미터 훈련, 표정(expression) 파라미터 훈련, 그리고 정체성(identity) 형태 파라미터 훈련입니다.

![한 스캔에 대한 모델 전용, 결합 및 텍스처 기반 등록 단계의 결과. 위: 각 등록에 대한 스캔, 등록 및 스캔과 메시 간 거리가 스캔에 색상으로 구분되어 시각화되어 있습니다. 아래: 원본 텍스처 이미지, 각 단계의 합성 텍스처 이미지 및 해당 측광 오류.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%207.png)

한 스캔에 대한 모델 전용, 결합 및 텍스처 기반 등록 단계의 결과. 위: 각 등록에 대한 스캔, 등록 및 스캔과 메시 간 거리가 스캔에 색상으로 구분되어 시각화되어 있습니다. 아래: 원본 텍스처 이미지, 각 단계의 합성 텍스처 이미지 및 해당 측광 오류.

![교대 등록 방식의 각 반복 결과](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%208.png)

교대 등록 방식의 각 반복 결과

![등록과 스캔 표면 사이의 버텍스당 거리 중앙값. 위: 모든 암컷(왼쪽) 및 수컷(오른쪽) 훈련 시퀀스의 모든 프레임에 걸친 거리 측정값. 아래쪽: Beeler 등[2011] 시퀀스(왼쪽)의 모든 등록 프레임에 걸친 거리 측정값과 표면 내 드리프트를 측정한 실측 오차(오른쪽). 보충 동영상은 전체 등록 시퀀스를 보여줍니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%209.png)

등록과 스캔 표면 사이의 버텍스당 거리 중앙값. 위: 모든 암컷(왼쪽) 및 수컷(오른쪽) 훈련 시퀀스의 모든 프레임에 걸친 거리 측정값. 아래쪽: Beeler 등[2011] 시퀀스(왼쪽)의 모든 등록 프레임에 걸친 거리 측정값과 표면 내 드리프트를 측정한 실측 오차(오른쪽). 보충 동영상은 전체 등록 시퀀스를 보여줍니다.

![D3DFACS 데이터베이스의 한 시퀀스(위)와 자체 캡처한 시퀀스(아래)의 샘플 프레임, 등록, 스캔-메시 간 거리. 보충 문서에는 추가 등록이 나와 있습니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2010.png)

D3DFACS 데이터베이스의 한 시퀀스(위)와 자체 캡처한 시퀀스(아래)의 샘플 프레임, 등록, 스캔-메시 간 거리. 보충 문서에는 추가 등록이 나와 있습니다.

**1. 포즈 파라미터 훈련**

- **목적**: 모델이 머리, 목, 눈동자의 움직임을 정밀하게 재현할 수 있도록 합니다.
- **방법**: 다양한 포즈 데이터(예: 턱 움직임, 머리 회전 등)를 사용하여 모델이 이러한 움직임을 정확하게 모델링할 수 있도록 포즈 관련 파라미터를 조정하고 최적화합니다.

**2. 표정 파라미터 훈련**

- **목적**: 모델이 다양한 표정 변화를 자연스럽게 표현할 수 있도록 합니다.
- **방법**: 표정 변화 데이터(예: 웃음, 놀람 등)를 사용하여, 모델이 얼굴의 다양한 비선형 표정 변화를 캡처하고 재현할 수 있도록 표정 파라미터를 학습합니다. 이 과정에서는 얼굴의 포즈를 '고정' 상태로 만든 후, 표정만 변화시키며 훈련합니다.

**3. 정체성 형태 파라미터 훈련**

- **목적**: 모델이 성별, 연령, 인종 등 다양한 인간의 얼굴 정체성을 반영할 수 있도록 합니다.
- **방법**: 대규모의 3D 얼굴 스캔 데이터를 사용하여, 모델이 인간 얼굴의 광범위한 형태 변화를 학습합니다. 이 과정에서는 주로 CAESAR 데이터셋과 같은 정적인 3D 스캔을 사용하여, 얼굴 형태의 다양성을 모델에 반영합니다.

**최적화 과정**

- 모델 훈련 과정에서는 각 파라미터 세트(포즈, 표정, 정체성 형태)를 독립적으로 최적화합니다. 이를 통해, 모델이 얼굴의 움직임, 표정 변화, 그리고 기본적인 얼굴 형태를 각각 정확하게 캡처하고 모델링할 수 있도록 합니다.
- 최적화는 주로 3D 스캔과 모델 간의 차이를 최소화하는 방향으로 이루어집니다. 이 과정을 통해, 모델은 실제 인간 얼굴과 가능한 가까운 결과를 생성할 수 있습니다.

결론적으로, "모델 훈련" 부분은 FLAME 모델이 실제 인간 얼굴의 다양한 형태와 표정을 어떻게 학습하고 모델링하는지에 대한 과정을 상세하게 설명합니다. 이 과정을 통해, 모델은 인간 얼굴의 복잡한 변화를 정밀하게 재현할 수 있는 능력을 갖추게 됩니다.

### 7 RESULTS

"결과" 부분에서는 FLAME 모델이 실제로 얼마나 잘 작동하는지, 그리고 기존의 다른 얼굴 모델링 기술과 비교했을 때 어떤 이점이 있는지를 설명합니다. 이 부분은 모델의 성능을 평가하고, 실제 데이터에 모델을 적용한 결과를 보여줍니다. 여기서는 FLAME 모델의 주요 결과와 발견사항을 간단히 요약하겠습니다.

![암컷과 수컷 FLAME 모델의 아이덴티티 형상 공간(위)과 표현 공간(아래)의 정량적 평가. 왼쪽부터: 소형화, 일반화 여성, 일반화 남성, 특이성 여성, 특이성 남성. 0 mm >1 mm 0 mm >1 mm 스캔 FLAME 49 오류 FLAME 49](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2011.png)

암컷과 수컷 FLAME 모델의 아이덴티티 형상 공간(위)과 표현 공간(아래)의 정량적 평가. 왼쪽부터: 소형화, 일반화 여성, 일반화 남성, 특이성 여성, 특이성 남성. 0 mm >1 mm 0 mm >1 mm 스캔 FLAME 49 오류 FLAME 49

![다양한 수의 아이덴티티 구성 요소로 BU-3DFE 얼굴 데이터베이스의 중립 스캔을 맞추기 위한 FLAME 아이덴티티 공간의 표현력. 보충 문서에는 추가 예제가 나와 있습니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2012.png)

다양한 수의 아이덴티티 구성 요소로 BU-3DFE 얼굴 데이터베이스의 중립 스캔을 맞추기 위한 FLAME 아이덴티티 공간의 표현력. 보충 문서에는 추가 예제가 나와 있습니다.

![목과 턱 관절의 다양한 동작에 대한 포즈 블렌드 셰이프의 회전 방식에 따른 영향. 포즈 블렌드 셰이프가 활성화되지 않은 경우(위)와 활성화된 경우(아래)의 FLAME 시각화.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2013.png)

목과 턱 관절의 다양한 동작에 대한 포즈 블렌드 셰이프의 회전 방식에 따른 영향. 포즈 블렌드 셰이프가 활성화되지 않은 경우(위)와 활성화된 경우(아래)의 FLAME 시각화.

![BU-3DFE 데이터베이스의 중립 스캔 피팅을 위한 바젤 얼굴 모델(BFM)[Paysan 외. 2009], FaceWarehouse 모델[Cao 외. 2014] 및 FLAME의 비교. 보충 문서에 추가 예제가 나와 있습니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2014.png)

BU-3DFE 데이터베이스의 중립 스캔 피팅을 위한 바젤 얼굴 모델(BFM)[Paysan 외. 2009], FaceWarehouse 모델[Cao 외. 2014] 및 FLAME의 비교. 보충 문서에 추가 예제가 나와 있습니다.

![테스트 데이터의 모든 프레임에서 측정한 스캔 표면에 대한 등록 사이의 버텍스당 거리 중앙값. 위: 여성 데이터. 아래: 남성 데이터.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2015.png)

테스트 데이터의 모든 프레임에서 측정한 스캔 표면에 대한 등록 사이의 버텍스당 거리 중앙값. 위: 여성 데이터. 아래: 남성 데이터.

**1. 모델의 정확성과 표현력**

- FLAME 모델은 기존 모델들보다 더 정확하게 3D 얼굴 스캔과 동적인 4D 시퀀스 데이터에 맞춰질 수 있음을 보여줍니다. 이는 FLAME 모델이 얼굴 형태와 표정의 미묘한 변화까지 포착할 수 있는 높은 수준의 표현력을 갖추고 있음을 의미합니다.

**2. 다양한 데이터셋에 대한 모델 적용**

- FLAME 모델은 다양한 출처에서 얻은 데이터셋에 적용되었으며, 각각의 경우에서 우수한 성능을 보였습니다. 특히, 정적인 3D 스캔과 동적인 표정 변화를 포함하는 4D 시퀀스에 모델을 적용한 결과, 모델이 실제 얼굴과 매우 유사한 형태와 표정을 재현할 수 있음을 확인했습니다.

**3. 기존 모델과의 비교**

- FLAME 모델은 Basel Face Model(BFM) 및 FaceWarehouse 모델과 같은 기존의 얼굴 모델들과 비교되었습니다. 비교 결과, FLAME 모델은 특히 표정 변화를 모델링하는 능력에서 뛰어남을 보였으며, 더 정밀한 얼굴 형태와 표정 재현이 가능함을 보여줍니다.

**4. 2D 이미지에서의 얼굴 형태 복원**

- FLAME 모델은 2D 이미지로부터 3D 얼굴 형태를 복원하는 데에도 사용될 수 있음을 보여줍니다. 이는 모델이 실제 얼굴의 3D 형태를 2D 이미지에 기반하여 정확하게 추정할 수 있음을 의미합니다.

**5. 표정 전달(Transfer)**

- FLAME 모델은 한 사람의 표정을 다른 사람의 얼굴 형태에 전달하는 데에도 효과적으로 사용될 수 있습니다. 이를 통해, 다양한 표정의 동적 시퀀스를 생성할 수 있으며, 이는 애니메이션 제작이나 게임 디자인 등 다양한 분야에서 활용될 수 있습니다.

결과 부분에서는 FLAME 모델이 기존의 다른 얼굴 모델링 기술들과 비교하여 더 높은 정확성과 표현력을 보여주며, 다양한 어플리케이션에서 유용하게 사용될 수 있음을 보여줍니다. 모델의 이러한 성능은 FLAME 모델이 얼굴 모델링 분야에서 중요한 진전을 나타내며, 향후 더 발전될 가능성을 가지고 있음을 시사합니다.

![고해상도 모션 시퀀스의 재구성 품질과 FaceWarehouse(FW)의 비교. 세 모션 시퀀스의 중간 프레임. FLAME은 FW와 동일한 수의 파라미터를 갖도록 제한됩니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2016.png)

고해상도 모션 시퀀스의 재구성 품질과 FaceWarehouse(FW)의 비교. 세 모션 시퀀스의 중간 프레임. FLAME은 FW와 동일한 수의 파라미터를 갖도록 제한됩니다.

![단일 2D 이미지에서 3D 얼굴 피팅을 위한 FaceWarehouse 모델(위)과 FLAME(아래)의 비교. 스캔(분홍색)은 평가용으로만 사용된다는 점에 유의하세요. 보충 문서에 추가 예시가 나와 있습니다.](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2017.png)

단일 2D 이미지에서 3D 얼굴 피팅을 위한 FaceWarehouse 모델(위)과 FLAME(아래)의 비교. 스캔(분홍색)은 평가용으로만 사용된다는 점에 유의하세요. 보충 문서에 추가 예시가 나와 있습니다.

![소스 시퀀스(파란색)에서 정적 타깃 스캔(분홍색)으로의 표현식 전송. 스캔을 위해 정렬된 개인화된 템플릿은 녹색으로, 전송된 표현식은 노란색으로 표시됩니다. 보충 문서에 추가 예시가 나와 있습니다](FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415/Untitled%2018.png)

소스 시퀀스(파란색)에서 정적 타깃 스캔(분홍색)으로의 표현식 전송. 스캔을 위해 정렬된 개인화된 템플릿은 녹색으로, 전송된 표현식은 노란색으로 표시됩니다. 보충 문서에 추가 예시가 나와 있습니다

### 8 CONCLUSION

**FLAME 모델의 기여**

- **표현력과 정확성**: FLAME 모델은 얼굴 형태와 표정의 다양성을 높은 정확성으로 재현할 수 있는 능력을 제공합니다. 이는 복잡한 인간 얼굴의 미묘한 변화까지 포착할 수 있음을 의미합니다.
- **범용성**: 다양한 연령, 성별, 인종을 포괄하는 광범위한 데이터로부터 학습되었기 때문에, FLAME 모델은 매우 다양한 인간 얼굴을 모델링할 수 있습니다.
- **응용 가능성**: FLAME 모델은 3D 얼굴 재구성, 애니메이션, 게임 개발, 가상 현실, 그리고 인간-컴퓨터 상호작용 분야 등 다양한 응용 분야에 적용될 수 있습니다.

**잠재적인 응용 분야**

- FLAME 모델은 그래픽 디자인, 애니메이션 제작, 게임 개발에서 실시간 얼굴 표현의 생성과 조작을 위한 강력한 도구로 활용될 수 있습니다.
- 또한, 가상 현실(VR) 및 증강 현실(AR) 경험을 개선하기 위해 사용자의 실제 얼굴 표정을 가상 환경에 반영하는 데에도 사용될 수 있습니다.
- 의료 및 심리학 연구에서 인간 얼굴의 표정 변화를 분석하거나, 표정 인식 훈련 도구로도 응용될 수 있습니다.

**향후 연구 방향**

- FLAME 모델은 현재의 성과에도 불구하고, 계속해서 발전될 수 있는 여지가 많습니다. 예를 들어, 더 정밀한 표정 변화 모델링, 사용자 친화적인 인터페이스 개발, 더 다양한 데이터셋을 이용한 학습 등이 연구될 수 있습니다.
- 추가적으로, 모델의 더 나은 일반화 능력을 위해 더 광범위하고 다양한 데이터를 수집하고 학습하는 연구가 필요합니다.
- 기술의 발전과 함께, FLAME 모델을 기반으로 한 새로운 알고리즘과 응용 프로그램의 개발도 중요한 연구 분야입니다.