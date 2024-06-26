# Generating Fine-Grained Human Motions Using ChatGPT-Refined Descriptions

[https://arxiv.org/abs/2312.02772](https://arxiv.org/abs/2312.02772)

[https://sx0207.github.io/fg-mdm/](https://sx0207.github.io/fg-mdm/)

- Dec 2023

![FG-MDM은 다양한 신체 부위에 대한 세밀한 묘사를 사용하여 고품질의 세분화되고 양식화된 모션을 생성할 수 있습니다.](Generating%20Fine-Grained%20Human%20Motions%20Using%20ChatGP%20e60962cbca4e43c3960c42b26e1ccad4/Untitled.png)

FG-MDM은 다양한 신체 부위에 대한 세밀한 묘사를 사용하여 고품질의 세분화되고 양식화된 모션을 생성할 수 있습니다.

### 1. Introduction

컴퓨터 비전과 컴퓨터 그래픽의 교차점에 있는 분야인 인간 모션 생성의 과제와 발전에 대해 설명합니다. 이 연구는 가상 현실, 증강 현실 및 영화 특수 효과 분야의 애플리케이션에 필수적입니다.

인간 모션 생성은 GAN, VAE, 확산 모델과 같은 심층 생성 모델을 사용하면서 상당한 발전을 이루었습니다. 하지만 텍스트 설명을 기반으로 세밀한 신체 움직임과 스타일을 포함하는 세분화된 모션을 생성하는 것은 여전히 덜 연구된 영역입니다. SINC나 MotionDiffuse와 같은 기존 방식은 자연어에서 모션을 생성하는 데 한계가 있거나 부족한 훈련 데이터로 인해 제어력이 부족합니다. 마찬가지로 스타일화된 모션 생성에 대한 MotionCLIP의 접근 방식은 단순한 동작과 스타일로 제한됩니다.

이 분야의 가장 큰 장애물은 상세하고 스타일화된 텍스트 주석이 포함된 데이터 세트가 부족하다는 점입니다. 기존 데이터 세트는 단순하고 직접적인 설명만 있는 경우가 많아 세밀하고 스타일화된 모션을 생성하기에는 부적합합니다. 이러한 한계로 인해 학습 데이터에서 잘 표현되지 않는 모션을 생성하기가 어렵습니다.

이러한 문제를 해결하기 위해 이 책에서는 분할 및 정복 전략을 사용하는 새로운 접근 방식을 제안합니다. 이 방법은 다양한 신체 부위에 대한 동작에 상세한 설명을 덧붙여 다시 주석을 달고, 이러한 부위를 데이터 세트의 해당 동작과 연관시킬 수 있도록 하는 것입니다. 이 전략은 복잡한 동작을 팔과 다리의 움직임으로 설명하는 것처럼 더 단순하고 설명 가능한 부분으로 나누는 것으로 예시할 수 있습니다.

이러한 세부적인 주석 프로세스를 지원하기 위해 OpenAI의 GPT 시리즈와 같은 대규모 언어 모델이 활용되었습니다. 이러한 모델, 특히 ChatGPT는 사람의 동작에 대한 상세하고 중복되지 않은 설명을 생성하는 데 잠재력을 보여주었습니다. 저자들은 ChatGPT-3.5를 사용하여 HumanML3D 및 KIT 데이터 세트의 텍스트 설명을 세분화된 설명으로 변환하여 상세하고 스타일화된 동작을 생성하는 데 도움을 주었습니다.

이를 바탕으로 저자는 인간 모션 생성을 위한 새로운 프레임워크인 세분화된 인간 모션 확산 모델(FG-MDM)을 소개합니다. 이 모델은 ChatGPT에서 생성된 세분화된 설명을 사용하여 트랜스포머 기반 확산 모델을 안내합니다. 이 모델은 CLIP을 사용하여 이러한 설명을 전역 및 부분 토큰으로 인코딩하여 모델이 사람 동작의 전체적인 측면과 세부적인 측면 모두에 집중할 수 있도록 합니다. 이러한 접근 방식은 생성된 모션의 정확성과 완성도를 향상시킵니다.

이 작업의 기여도는 상당합니다. 첫째, 특별히 주석이 달린 데이터 세트에 의존하지 않고도 상세하고 양식화된 사람 모션을 생성할 수 있는 새로운 프레임워크를 제시합니다. 둘째, 짧은 텍스트를 신체 부위에 대한 상세한 설명으로 변환하는 데 ChatGPT를 효과적으로 사용하여 생성 프로세스에 기여한다는 것을 보여줍니다. 마지막으로, 훈련 데이터에 적합할 뿐만 아니라 일반화하여 세밀하고 양식화된 모션을 생성하는 모델의 능력을 실험을 통해 평가합니다. 이 연구는 인간 모션 생성 분야에서 주목할 만한 발전을 이루었습니다.

![FG-MDM의 전체 파이프라인. 이 모델은 텍스트 조건이 주어졌을 때 시간 단계 t에서 모션 x 1:n t에서 깨끗한 모션 xˆ 1:n 0까지 확산 모델의 노이즈 제거 과정을 학습합니다. 입력된 텍스트는 먼저 ChatGPT에 의해 신체의 여러 부위에 대한 세분화된 설명 D1:k로 의역되며, 여기서 k는 신체 부위의 수를 나타냅니다. 그런 다음 이러한 설명은 사전 학습된 CLIP 텍스트 인코더에 입력되어 시간 단계 t와 함께 트랜스포머의 입력 토큰 P T1:k에 투영됩니다. 전체적으로 세분화된 텍스트는 글로벌 입력 토큰 GL로 추가 인코딩되어 전체적인 정보를 제공합니다. 확산 모델의 샘플링 프로세스에서는 초기 랜덤 노이즈 x 1:n T를 샘플링한 다음, 깨끗한 모션 xˆ 1:n 0을 생성하기 위해 T번의 반복을 수행합니다. 각 샘플링 단계 t에서 P T1:k 및 GL의 안내에 따라 트랜스포머 인코더는 클린 모션 xˆ 1:n 0을 예측한 다음 x 1:n t-1로 다시 노이즈 처리합니다.](Generating%20Fine-Grained%20Human%20Motions%20Using%20ChatGP%20e60962cbca4e43c3960c42b26e1ccad4/Untitled%201.png)

FG-MDM의 전체 파이프라인. 이 모델은 텍스트 조건이 주어졌을 때 시간 단계 t에서 모션 x 1:n t에서 깨끗한 모션 xˆ 1:n 0까지 확산 모델의 노이즈 제거 과정을 학습합니다. 입력된 텍스트는 먼저 ChatGPT에 의해 신체의 여러 부위에 대한 세분화된 설명 D1:k로 의역되며, 여기서 k는 신체 부위의 수를 나타냅니다. 그런 다음 이러한 설명은 사전 학습된 CLIP 텍스트 인코더에 입력되어 시간 단계 t와 함께 트랜스포머의 입력 토큰 P T1:k에 투영됩니다. 전체적으로 세분화된 텍스트는 글로벌 입력 토큰 GL로 추가 인코딩되어 전체적인 정보를 제공합니다. 확산 모델의 샘플링 프로세스에서는 초기 랜덤 노이즈 x 1:n T를 샘플링한 다음, 깨끗한 모션 xˆ 1:n 0을 생성하기 위해 T번의 반복을 수행합니다. 각 샘플링 단계 t에서 P T1:k 및 GL의 안내에 따라 트랜스포머 인코더는 클린 모션 xˆ 1:n 0을 예측한 다음 x 1:n t-1로 다시 노이즈 처리합니다.

### 2. Related Work

크게 두 가지 분야의 기존 문헌을 종합적으로 검토합니다: 인간 모션 생성과 확산 생성 모델의 두 가지 주요 영역에 대한 기존 문헌을 종합적으로 검토하여 이 논문이 기여한 바를 설명합니다.

휴먼 모션 생성

최근 몇 년 동안 인간의 모션 생성은 상당한 주목을 받고 있습니다. 이 분야는 무조건적인 생성 모델을 사용하는 것에서 텍스트, 사전 모션, 액션 클래스, 음악 등 다양한 입력 조건을 사용하여 모션을 생성하는 방식으로 발전해 왔습니다. 이 백서에서는 텍스트-투-모션 생성에 초점을 맞춥니다.

처음에는 텍스트-투-모션 작업을 시퀀스-투-시퀀스 모델로 접근했습니다. 그러나 이 분야가 발전함에 따라 연구자들은 단순한 동작 레이블을 넘어서는 작업을 진행했습니다. 주목할 만한 발전으로는 텍스트에서 생성되는 모션의 품질과 다양성을 개선한 Guo 등의 변형 자동 인코더 사용이 있습니다. 인공 지능 생성 콘텐츠(AIGC) 분야에서 차용한 개념인 확산 모델의 도입으로 텍스트-모션 생성이 더욱 향상되었으며, MDM과 같은 모델은 상당한 성과를 보여주었습니다.

최근 연구에서는 세분화되거나 양식화된 모션을 생성하는 방법도 연구되고 있습니다. 이 분야에서 진전을 이룬 두 가지 예로 MotionCLIP과 MotionDiffuse가 있습니다. MotionCLIP은 모션을 잠재 피처 공간의 텍스트 및 이미지와 정렬하여 스타일화된 동작을 생성하고, MotionDiffuse는 신체 부위의 모션과 스타일화를 정밀하게 제어할 수 있습니다. GestureDiffuCLIP은 스타일 가이드를 디퓨전 모델에 통합하여 사실적이고 스타일화된 제스처를 생성할 수 있도록 지원합니다. 뮤직 투 모션 분야에서는 여러 음악 장르의 영향을 받은 다양한 스타일의 댄스 동작을 생성하는 데 중점을 두고 연구해 왔습니다.

중요한 발전은 텍스트 조건부 모션 생성에 대규모 언어 모델(LLM)을 통합한 것입니다. ActionGPT는 LLM으로 동작 클래스 설명을 강화하여 획기적인 발전을 이루었지만, 다양한 신체 부위에 대한 상세한 설명을 제공하는 데는 부족했습니다. 또 다른 주목할 만한 모델인 SINC는 ChatGPT를 사용하여 텍스트 설명에서 관련된 신체 부위를 식별하고 다양한 신체 부위에 대한 여러 모션을 생성하여 인상적인 결과를 달성했습니다. MotionGPT는 프롬프트를 통해 다양한 모션 관련 작업을 지원하는 사전 학습된 모션 언어 모델을 설계하여 이러한 접근 방식을 더욱 발전시켰습니다.

확산 생성 모델

확산 모델은 열역학에서 발견되는 확률적 확산 과정을 기반으로 하는 신경 생성 모델입니다. 이 모델은 데이터 분포에서 샘플에 노이즈를 추가하는 것으로 시작한 다음 신경망을 사용하여 이 과정을 역으로 진행하여 점진적으로 노이즈를 제거하여 샘플을 복원합니다. 이 모델은 이미지 생성 분야에서 상당한 성공을 거두었습니다.

조건부 생성의 경우, 분류기 유도 및 분류기 없는 확산과 같은 다양한 방법이 제안되었습니다. 뛰어난 생성 품질로 인해 확산 모델은 모션 생성 영역에 성공적으로 통합되어 사실적이고 다양한 사람의 움직임을 생성하는 데 인상적인 결과를 가져왔습니다.

관련 연구를 종합적으로 검토한 이 논문은 특히 고급 모델과 기법을 사용한 세밀하고 양식화된 모션 생성 분야에서 이 논문이 인간 모션 생성 분야에 기여한 맥락과 관련성을 확립합니다.

### 3. Method

**목표 및 개요**

목표는 주어진 텍스트 설명과 일치하는 일련의 인간 모션(x1:n)을 생성하는 것입니다. 각 모션 프레임은 관절 회전 또는 위치 측면에서 사람의 포즈를 나타내며, 'J'는 관절의 개수, 'D'는 관절 표현의 차원을 나타냅니다. 이 프로세스는 ChatGPT를 사용하여 입력 텍스트를 세밀하게 의역하는 것으로 시작하여 다양한 신체 부위에 대한 상세한 설명을 생성합니다. 그런 다음 이러한 설명을 통해 확산 모델이 해당 인체 동작을 생성하도록 안내합니다.

**프롬프트 전략**

이 방법의 핵심은 ChatGPT-3.5를 사용하여 주어진 텍스트의 다양한 신체 부위를 기반으로 상세한 설명을 생성하는 것입니다. ChatGPT의 응답 효과는 프롬프트의 명확성과 세부 사항에 따라 크게 달라집니다. 이 작업을 위해 프롬프트는 입력 텍스트에 설명된 동작을 팔, 다리, 몸통, 목, 엉덩이, 허리와 같은 특정 신체 부위에 대한 자세한 설명으로 변환하도록 설계되었습니다.

**모션 생성을 위한 확산 모델**

기본 개념

확산 모델은 마르코프 체인을 따르는 정방향 및 역방향 프로세스에서 작동합니다. 포워드 프로세스에서는 시간이 지남에 따라 원래 모션에 가우시안 노이즈가 추가됩니다. 역방향 프로세스에서 모델은 노이즈가 완전히 제거될 때까지 모션에서 노이즈를 반복적으로 제거합니다.

네트워크 구현

이 모델의 네트워크는 G로 표시되며, 간단한 트랜스포머 인코더 아키텍처를 기반으로 구축됩니다. 각 시간 단계에서 노이즈를 예측하는 기존의 확산 모델과 달리, 이 모델은 깨끗한 모션을 직접 예측합니다. 트랜스포머 인코더에 입력되는 정보에는 노이즈 모션, 텍스트 조건 코드, 노이즈 시간 단계가 포함되어 클린 모션이 생성됩니다.

글로벌 토큰 및 파트 토큰

모델의 텍스트 조건은 CLIP을 사용하여 글로벌 토큰과 부분 토큰으로 인코딩됩니다. 글로벌 토큰은 전체 모션 정보를 나타내며, 파트 토큰은 특정 신체 부위에 해당합니다. 이러한 이중 접근 방식을 통해 세밀하고 정확한 모션을 생성할 수 있습니다.

손실 함수

이 모델은 훈련에 간단한 손실 함수를 사용하여 노이즈가 아닌 신호 자체를 예측하는 데 중점을 둡니다. 생성된 모션이 자연스럽고 운동학적으로 그럴듯하도록 하기 위해 추가적인 기하학적 손실이 사용됩니다. 여기에는 위치, 발 접촉, 속도에 대한 손실이 포함됩니다. 전체 훈련 손실은 이러한 개별 손실을 결합하고 균형 계수를 통해 가중치를 부여합니다.

요약하면, 이 방법은 ChatGPT를 사용하여 텍스트 설명을 다양한 신체 부위에 대한 자세한 동작 지침으로 변환한 다음, 트랜스포머 기반 네트워크와 특수 손실 함수가 포함된 확산 모델을 사용하여 이러한 지침에 부합하는 사실적인 인간 동작을 생성하는 것입니다.

### 4. Experiments

**데이터 세트**

- HumanML3D 데이터세트: 14,616개의 모션 시퀀스와 44,970개의 텍스트 주석이 포함된 대규모 데이터 세트입니다.
- KIT 데이터세트: 3,911개의 모션 시퀀스와 6,353개의 텍스트 설명이 포함된 소규모 데이터 세트.
두 데이터 세트 모두 ChatGPT-3.5를 사용하여 전처리하여 설명을 세분화된 설명으로 확장하여 FG-MDM을 학습시켰습니다.

**평가 지표**

- 모델의 성능은 R 정밀도, FID(프레셰트 시작 거리), 멀티모달 거리, 다양성 및 멀티모달 메트릭을 사용하여 평가했습니다. 이러한 지표는 생성된 모션과 입력 텍스트 간의 상관관계, 모션의 다양성, 생성된 모션의 품질을 평가했습니다.

**구현 세부 사항**

- 연구에 사용된 트랜스포머 모델은 512개의 특징 차원, 4개의 주의 헤드, 8개의 인코더 레이어, 0.1의 드롭아웃 비율을 가졌습니다.
- 텍스트 인코딩에는 ChatGPT-3.5와 CLIP-ViT-B/32가 사용되었습니다.
- 훈련은 단일 NVIDIA GeForce RTX3090 GPU에서 약 6일이 소요되었습니다.

이전 작업과의 비교 분석

FG-MDM을 최근의 네 가지 모션 생성 방식과 비교했습니다: T2M-GPT, MLD, 모션디퓨즈, MDM. 세밀한 묘사를 처리하는 능력을 기준으로 비교했습니다. 그 결과, FG-MDM은 특히 FID에서 다른 방법보다 실제 동작에 가까운 모션을 생성하는 능력이 뛰어나다는 것을 보여주었습니다.

![세밀한 모션의 정성적 결과. FG-MDM을 MDM [35] 및 MLD [5]와 비교합니다. 세 모델 모두 HumanML3D에서 훈련되었습니다. 더 나은 시각화를 위해 일부 포즈 프레임은 중첩을 방지하기 위해 이동되었습니다. 더 많은 동영상 데모는 보충 자료를 참조하세요.](Generating%20Fine-Grained%20Human%20Motions%20Using%20ChatGP%20e60962cbca4e43c3960c42b26e1ccad4/Untitled%202.png)

세밀한 모션의 정성적 결과. FG-MDM을 MDM [35] 및 MLD [5]와 비교합니다. 세 모델 모두 HumanML3D에서 훈련되었습니다. 더 나은 시각화를 위해 일부 포즈 프레임은 중첩을 방지하기 위해 이동되었습니다. 더 많은 동영상 데모는 보충 자료를 참조하세요.

**절제 연구**

두 가지 제거 연구가 수행되었습니다:

- ChatGPT로 생성된 텍스트: 세분화된 설명을 원래의 짧은 텍스트로 대체하면 성능이 저하되어 상세한 설명의 중요성을 알 수 있었습니다.
- 글로벌 토큰 및 부품 토큰 사용: 부분 토큰을 제거하면 성능이 저하되어 모델의 효율성에 대한 부분 토큰의 기여도를 강조했습니다.

**일반화 기능 연구**

훈련 데이터 세트의 범위를 넘어선 모션을 생성하는 FG-MDM의 능력을 입증하기 위해 HumanML3D 및 KIT 데이터 세트에 포함되지 않은 사용자 지정 텍스트를 사용했습니다. 그 결과 FG-MDM이 복잡한 텍스트 설명과 더 일관성 있는 모션을 생성할 수 있는 것으로 나타나 강력한 일반화 능력을 보여주었습니다.

![스타일라이즈드 모션의 질적 결과. 세 모델 모두 휴먼ML3D로 훈련되었습니다. 더 많은 동영상 데모는 보충 자료를 참조하세요.](Generating%20Fine-Grained%20Human%20Motions%20Using%20ChatGP%20e60962cbca4e43c3960c42b26e1ccad4/Untitled%203.png)

스타일라이즈드 모션의 질적 결과. 세 모델 모두 휴먼ML3D로 훈련되었습니다. 더 많은 동영상 데모는 보충 자료를 참조하세요.

**사용자 연구**

10명의 참가자가 참여한 사용자 연구를 통해 인간의 시각적 인식을 기반으로 생성된 모션의 품질을 평가했습니다. 참가자들은 생성된 모션과 텍스트 설명의 일치 정도를 평가했습니다. FG-MDM은 MDM 및 MotionDiffuse에 비해 세밀하고 양식화된 모션을 생성할 때 텍스트와 훨씬 더 잘 일치하는 것으로 나타났습니다.

이러한 실험과 연구를 종합해 보면, 세밀한 텍스트 설명에서 정확하고 다양한 사람의 움직임을 생성하는 데 있어 FG-MDM의 우수성, 현재의 최첨단 방법을 능가하는 능력, 강력한 일반화 능력 등이 입증되었습니다.

### 5 Conculusion

이 연구는 세분화된 인체 모션 확산 모델(FG-MDM)의 개발을 통해 인체 모션 생성 분야에서 상당한 진전을 이루었습니다. 이 연구의 핵심 혁신은 HumanML3D 및 KIT 데이터 세트의 텍스트 주석을 세밀하게 의역하는 데 ChatGPT를 사용했다는 데 있습니다. 특정 신체 부위에 초점을 맞춘 이러한 상세한 설명은 확산 모델 학습을 위한 보다 정확하고 포괄적인 입력을 제공합니다.

FG-MDM은 이러한 세분화된 설명을 활용하여 모션 생성 프로세스를 효과적으로 안내합니다. 이 접근 방식을 통해 모델은 훈련 데이터 세트에 존재하는 모션을 복제할 수 있을 뿐만 아니라 기존 데이터 분포를 넘어서는 세밀하고 양식화된 모션을 생성할 수 있습니다. 훈련 데이터에 잘 표현되지 않은 동작을 포함해 다양한 인간 동작을 생성할 수 있는 이 모델의 능력은 인간 동작 합성에 있어 주목할 만한 진전입니다.

이 연구는 앞으로 더 발전할 수 있는 잠재력을 인정하고 있습니다. 향후 개선해야 할 주요 영역은 사람 동작에 대한 세분화된 주석의 품질입니다. 이러한 주석을 개선함으로써 FG-MDM은 모션 생성의 정확성과 사실성을 훨씬 더 높일 수 있습니다. 현재 진행 중인 이 개발은 사실적인 인체 모션 생성이 중요한 가상 현실, 애니메이션, 인터랙티브 시스템 등 다양한 애플리케이션에 적용될 수 있을 것으로 기대됩니다.