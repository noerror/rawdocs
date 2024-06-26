# DREAM-Talk: Diffusion-based Realistic Emotional Audio-driven Method for Single Image Talking Face Generation

[https://arxiv.org/abs/2312.13578](https://arxiv.org/abs/2312.13578)

- Dec 2023

![DREAM-Talk는 주행 오디오 시퀀스, 지정된 인물 이미지, 감정 스타일 예시(감정적인 말하는 얼굴 클립)를 입력으로 받아 고품질의 감정 표현을 특징으로 하는 사실적인 립싱크가 적용된 말하는 얼굴 비디오를 생성합니다. 결과물에는 실제 사람의 이미지와 AIGC가 생성한 이미지가 모두 포함됩니다. 더 많은 결과물은 프로젝트 페이지를 참조하세요.](DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3/Untitled.png)

DREAM-Talk는 주행 오디오 시퀀스, 지정된 인물 이미지, 감정 스타일 예시(감정적인 말하는 얼굴 클립)를 입력으로 받아 고품질의 감정 표현을 특징으로 하는 사실적인 립싱크가 적용된 말하는 얼굴 비디오를 생성합니다. 결과물에는 실제 사람의 이미지와 AIGC가 생성한 이미지가 모두 포함됩니다. 더 많은 결과물은 프로젝트 페이지를 참조하세요.

### 1. Introduction

화상 회의, 가상 비서, 엔터테인먼트에 필수적인 말하는 얼굴 생성 연구 분야는 최근 얼굴 표정에 감정을 통합하는 데 초점을 맞추고 있습니다. 현재의 방법은 MEAD와 같은 데이터 세트를 사용하지만, 사실적이고 표현력이 풍부한 말하는 얼굴을 생성하는 데 어려움을 겪습니다.

도전 과제

- 감정 표현과 립싱크 정확도: 현재 방식은 정확한 립싱크를 유지하면서 표현적인 감정을 생성해야 하는 두 가지 과제에 직면해 있습니다. 예를 들어, SPACE 모델은 학습을 개선하기 위해 추가 데이터 세트를 사용했지만, 이로 인해 표현력이 떨어지는 결과를 초래했습니다. EAMM은 비감정적 측면과 감정적 측면의 모듈을 분리하여 이를 극복하려고 했지만, 이 접근 방식은 감정적인 말을 할 때 입 움직임의 표현력에 영향을 미쳤습니다.
- 감정 표현 모델링: 감정 표현의 미묘함과 다양성을 포착하는 것은 어렵습니다. LSTM이나 CNN을 사용하는 현재의 모델은 일반 음성은 처리할 수 있지만 감정 표현의 뉘앙스를 묘사하는 데는 부족하여 종종 인위적으로 보이는 감정 표현을 만들어냅니다.

2단계 프로세스를 사용하는 새로운 방법인 DREAM-Talk를 소개합니다:

- EmoDiff 모듈: 첫 번째 단계에서는 감정 조건부 확산 모델을 사용하여 역동적인 감정 표현을 포착합니다. 이 모듈은 특히 감정적 뉘앙스에 초점을 맞춰 오디오를 얼굴 표정으로 변환합니다.
- 립 리파이닝: 두 번째 단계는 감정 표현을 손상시키지 않으면서 정확한 립싱크를 보장합니다. 새로운 네트워크를 사용하여 오디오 신호와 감정 스타일에 따라 입술 움직임을 세분화합니다. 이 방법은 다른 얼굴 특징과 별도로 입술의 움직임을 고유하게 최적화하여 감정 표현의 강도를 유지합니다.

기여 및 결과

- 드림톡은 표현적인 감정 생성과 정밀한 립싱크를 효과적으로 결합합니다.
- EmoDiff 모듈은 역동적인 감정 표현과 머리 포즈를 능숙하게 생성합니다.
- 혁신적인 입술 세분화 네트워크가 입 동작의 정밀도를 향상시킵니다.
- 이 접근 방식에는 더 나은 세분화를 위해 입 매개변수를 분리하는 감성 ARKit 데이터 세트가 포함됩니다.
- 실험 결과는 눈썹, 눈 깜빡임, 입 모양과 같은 얼굴 영역에서 감정적 뉘앙스를 사실적으로 포착하는 DREAM-Talk의 능력을 보여줍니다.

드림톡은 감정 표현이 풍부하고 생생한 말하는 얼굴을 생성하는 데 있어 상당한 진전을 이루었으며, 이 분야의 이전 과제를 해결하고 오디오와 동기화된 더욱 사실적이고 역동적인 얼굴 애니메이션을 제공합니다.

### 2. Related Work

오디오 기반 말하는 얼굴 생성

오디오 입력을 이용한 말하는 얼굴 생성은 상당한 발전을 이루었습니다. 초기 연구는 적응형 아바타를 위해 오디오를 음소 시퀀스로 변환하는 데 중점을 두었습니다. 이후 오디오와 화자의 단일 이미지 또는 비디오에서 말하는 얼굴을 합성하는 기술이 발전했습니다. ATVGnet과 같은 방법은 처음에는 오디오와 신원 이미지에서 생성된 랜드마크를 얼굴 합성에 사용했습니다. 일부 접근 방식은 말하는 머리 합성을 공간 및 스타일 구성 요소로 분해하여 몇 컷의 새로운 뷰 합성에서 성능을 개선했습니다. 최근의 혁신에는 3D 얼굴 모델과 오디오 기반 신경 방사장(NeRF) 모델을 사용하여 사실감과 비디오 품질을 향상시키는 것이 포함됩니다. 그러나 사실적인 표정, 정확한 입술 움직임, 시간적 일관성을 달성하는 것과 같은 과제는 특히 GAN 기반 및 확산 모델 접근 방식에서 여전히 남아 있습니다.

![드림톡 프레임워크의 파이프라인. 입력 오디오, 초기 상태, 감정 스타일을 조건으로 하여 먼저 시퀀스 모델링을 위한 트랜스포머 기반 아키텍처를 활용하여 시간에 따른 3D 표현의 노이즈 제거를 학습하기 위해 EmoDiff를 사용합니다. 초기 상태는 첫 번째 프레임의 표정에 해당하며, 감정 스타일은 입력 오디오와 무관하게 임의로 선택된 표정 클립으로 정의됩니다. 그런 다음 조건화된 오디오와 감정 표현을 활용하여 입술 개선 모델은 감정의 강도를 변경하지 않고 입을 더욱 최적화합니다. 그런 다음 블렌드셰이프 릭에서 해당 3D 렌더링 얼굴을 생성합니다. 마지막으로 미세 조정된 Face-Vid2Vid 모델[34]을 사용하여 감정적인 말하기 비디오를 생성합니다.](DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3/Untitled%201.png)

드림톡 프레임워크의 파이프라인. 입력 오디오, 초기 상태, 감정 스타일을 조건으로 하여 먼저 시퀀스 모델링을 위한 트랜스포머 기반 아키텍처를 활용하여 시간에 따른 3D 표현의 노이즈 제거를 학습하기 위해 EmoDiff를 사용합니다. 초기 상태는 첫 번째 프레임의 표정에 해당하며, 감정 스타일은 입력 오디오와 무관하게 임의로 선택된 표정 클립으로 정의됩니다. 그런 다음 조건화된 오디오와 감정 표현을 활용하여 입술 개선 모델은 감정의 강도를 변경하지 않고 입을 더욱 최적화합니다. 그런 다음 블렌드셰이프 릭에서 해당 3D 렌더링 얼굴을 생성합니다. 마지막으로 미세 조정된 Face-Vid2Vid 모델[34]을 사용하여 감정적인 말하기 비디오를 생성합니다.

감성 오디오 기반 말하는 얼굴 생성

말하는 얼굴에 감정 표현을 통합하는 것은 최근의 연구 트렌드입니다. 다양한 표정과 합성을 위한 ExprGAN과 범주형 감정에 따라 조절되는 시스템 등 다양한 방법이 개발되었습니다. MEAD 데이터 세트는 감정적인 말하는 얼굴 생성을 위한 기준을 제공함으로써 이 분야에 크게 기여했습니다. EAMM 및 EMMN과 같은 혁신적인 방법은 감정 비디오 합성에 2D 키포인트 변위를 사용했지만, 이는 품질에 영향을 미칠 수 있었습니다. 일부 연구는 입 모양, 머리 포즈, 감정 표현을 세밀하게 제어하는 데 중점을 두었습니다. SPACE 방법은 얼굴 랜드마크 예측과 감정 조절로 작업을 세분화하여 말하는 얼굴 비디오에서 더 높은 해상도와 제어 가능성을 달성했습니다. 현재 작업은 확산 모델을 사용하여 표정 시퀀스를 예측하여 더 표현력 있는 결과를 목표로 합니다.

요약하자면, 오디오 기반 말하는 얼굴 생성 분야는 크게 발전해 왔으며, 최근에는 감정 표현을 통합하는 데 중점을 두고 있습니다. 이러한 발전에도 불구하고 사실성, 입술 동기화, 감정적 깊이 등의 과제는 여전히 남아 있습니다. 디퓨전 모델을 비롯한 최신 방법들은 이러한 장애물을 극복하기 위해 노력하고 있으며, 보다 생생하고 감정적으로 풍부한 말하는 얼굴 애니메이션을 목표로 하고 있습니다.

### 3. Method

3.1 사전 작업: 2D 랜드마크 기반 방법보다 3D를 선택하는 이유

이 방법은 기존의 2D 랜드마크 기반 방법보다 3D 모델링을 사용한다는 점에서 차별화됩니다. 3D 모델링은 머리의 포즈 변화에 관계없이 일관된 얼굴 모양과 사실적인 렌더링을 유지하는 데 유리합니다. 주성분 분석(PCA)을 사용하고 특정 얼굴 속성을 분리하는 데 어려움을 겪는 3D 모퍼블 모델이나 FLAME과 달리, 이 접근 방식은 ARKit 블렌드 셰이프를 사용합니다. 얼굴 동작 코딩 시스템(FACS)에 기반한 52개의 개별 파라미터를 갖춘 ARKit을 사용하면 입, 눈, 눈썹 등 다양한 얼굴 영역을 독립적이고 해부학적으로 일관되게 제어할 수 있습니다. 이러한 특수성은 전체적인 얼굴 표현력을 유지하면서 입 영역을 향상시키려는 프로젝트의 목표에 매우 중요합니다.

![시퀀스당 N개의 프레임이 포함된 조건의 시간 위치 인식 임베딩. 초기 상태는 오디오의 첫 번째 프레임에 해당하고, 중간 세 프레임은 감정 스타일에 해당합니다. 추가 비트는 유효 정보를 나타내는 데 사용됩니다. 마지막으로, 초기 상태 및 감정 스타일 조건은 오디오 특징과 프레임 단위로 병합됩니다.](DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3/Untitled%202.png)

시퀀스당 N개의 프레임이 포함된 조건의 시간 위치 인식 임베딩. 초기 상태는 오디오의 첫 번째 프레임에 해당하고, 중간 세 프레임은 감정 스타일에 해당합니다. 추가 비트는 유효 정보를 나타내는 데 사용됩니다. 마지막으로, 초기 상태 및 감정 스타일 조건은 오디오 특징과 프레임 단위로 병합됩니다.

3.2 EmoDiff 모듈: 3D 표정 생성을 위한 확산 기반 모델

EmoDiff 모듈은 오디오에서 역동적이고 사실적인 3D 얼굴 표정을 생성하는 데 따르는 문제를 해결하기 위해 고안된 새로운 확산 기반 모델입니다. 이 모듈은 순방향 노이즈 제거 프로세스와 역방향 확산 프로세스가 포함된 노이즈 제거 확산 확률 모델(DDPM)을 사용합니다. 이 모델은 오디오, 초기 상태, 감정 스타일과 같은 추가 조건을 통합하고, 양식 융합을 위한 트랜스포머 구조와 장기적인 의존성을 위한 교차 주의 메커니즘을 활용합니다. 훈련 목표는 품질과 다양성의 균형을 맞추기 위해 단순화된 손실 함수와 분류기 없는 가이드로 최적화됩니다. 시간 인식 위치 임베딩 및 장기 동적 샘플링 방법을 도입하여 얼굴 표정의 시간적 연속성과 감정적 스타일 표현을 보장합니다.

3.3 입술 개선: 오디오-입 모양 정렬 보장

확산 모델은 역동적인 감정 표현을 생성하지만 오디오의 영향을 감소시켜 오디오와 입 모양이 어긋나는 경향이 있습니다. 이 문제를 해결하기 위해 오디오 인코딩에는 LSTM을, 감정 인코딩에는 CNN을 사용하는 립싱크 개선 네트워크가 도입되었습니다. 이 네트워크는 입력된 오디오 및 감정 스타일에 밀접하게 일치하도록 입 모양 매개변수를 재보정하고 개선합니다. 정제된 파라미터는 생성된 머리 포즈와 결합하여 3D 블렌드 셰이프 릭에 애니메이션을 적용하여 입의 움직임과 오디오의 동기화를 개선합니다.

3.4 얼굴 뉴럴 렌더링: 사실적인 말하는 머리 효과

3D 렌더링된 아바타 이미지를 생성한 후 모션 전송 기술을 사용하여 다양한 피사체에 대해 사실적인 말하는 머리 효과를 만듭니다. 신경 렌더링 파이프라인으로 Face-Vid2Vid 방식이 사용되며, 고해상도의 표현력이 풍부한 말하는 비디오를 사용하여 더욱 세밀하게 조정됩니다. 이 과정에는 초고해상도 방법을 사용하여 최종 이미지 해상도를 512x512로 높이는 작업이 포함됩니다. 최종 렌더링 출력물에서 동일성 보존과 사실적인 움직임을 보장하기 위해 상대적 모션 전송 기법이 적용됩니다.

요약하자면, 이 방법은 사실적인 3D 감정 표현 얼굴을 생성하기 위한 혁신적인 접근 방식을 제시합니다. 이 방법은 확산 기반 모델링, 립싱크 개선, 신경 렌더링과 같은 고급 기술을 활용하여 말하는 얼굴 생성 분야에서 동적 표정 생성, 오디오 정렬, 사실적인 렌더링의 과제를 해결합니다.

### 4. Experiments

4.1 구현 세부 사항

실험은 아담 옵티마이저를 사용하여 V100 GPU에서 수행되었습니다. 프레임 속도는 25 FPS로 설정했으며, MEAD 감정 데이터 세트와 HDTF 다중 문자 데이터 세트의 두 가지 데이터 세트를 사용했습니다. 이모디프 모듈 훈련에는 1000회 동안 32프레임의 시퀀스가 사용되었고, 입술 세분화 모델에는 50회 동안 8프레임의 슬라이딩 창이 사용되었습니다.

![최신 모델과 우리의 접근 방식 비교: 첫 번째와 두 번째 비교에서는 각각 MEAD와 HDTF 데이터 세트에 대한 평가를 수행합니다. 세 번째 비교에서는 AIGC가 생성한 얼굴 하나를 활용합니다. 또한 릭 모델 결과를 중간 표현으로 시각화합니다. 이 방법은 감정 표현, 입술 동기화, 신원 보존, 이미지 품질 측면에서 일관되게 훨씬 우수한 결과를 제공합니다. 더 나은 비교를 위해 보충 동영상을 참조하세요.](DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3/Untitled%203.png)

최신 모델과 우리의 접근 방식 비교: 첫 번째와 두 번째 비교에서는 각각 MEAD와 HDTF 데이터 세트에 대한 평가를 수행합니다. 세 번째 비교에서는 AIGC가 생성한 얼굴 하나를 활용합니다. 또한 릭 모델 결과를 중간 표현으로 시각화합니다. 이 방법은 감정 표현, 입술 동기화, 신원 보존, 이미지 품질 측면에서 일관되게 훨씬 우수한 결과를 제공합니다. 더 나은 비교를 위해 보충 동영상을 참조하세요.

4.2 최신 방법과의 비교

비교 분석 결과, 새드토커나 메이크잇토크와 같은 기존 방식은 감성적인 음성 생성 기능이 부족하다는 것을 알 수 있었습니다. EAMM은 감성적인 비디오를 생성하지만 정체성과 역동적인 얼굴 표정이 부족합니다. EVP는 감성적인 음성을 생성하지만 감정의 역동성이 부족하고 단일 이미지에서 구동할 수 없습니다. 그러나 드림톡은 역동적인 감정 표현, 아이덴티티 유지, 립싱크 정확도 보장에 탁월합니다.

!['화난' 감정에 대해 EVP가 제공하는 비디오 시퀀스와의 비교. EVP는 각 영상에 대해 별도의 학습이 필요하므로 임의의 캐릭터로 테스트할 수 없습니다.](DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3/Untitled%204.png)

'화난' 감정에 대해 EVP가 제공하는 비디오 시퀀스와의 비교. EVP는 각 영상에 대해 별도의 학습이 필요하므로 임의의 캐릭터로 테스트할 수 없습니다.

4.2.1 정성적 평가

드림톡을 세 가지 최첨단 방법과 비교했습니다. 메이크잇토크와 새드토커는 단일 이미지에서 원하는 감정을 생성하지 못했고, EAMM은 감정의 역동성과 영상 품질이 저하된 반면, 드림톡은 감정 표현, 립싱크, 아이덴티티 유지, 영상 품질에서 더 우수한 성능을 보였습니다.

4.2.2 정량적 평가

평가에는 싱크넷, 랜드마크 거리(F-LMD), LPIPS, CPBD와 같은 지표가 사용되었습니다. 드림톡은 기존 방식보다 향상된 성능을 보여줬으며, 감정 표현과 립싱크 및 화질의 균형을 성공적으로 맞췄습니다.

4.3 제거 연구

제거 연구는 세 가지 측면에 중점을 두었습니다:

- 자동 회귀 생성: 긴 시퀀스를 생성하는 데 필요한 이 방법은 시퀀스 간의 연속성을 보장합니다.
- 감성 스타일 임베딩: 드림톡의 방식은 지나치게 부드러운 감정을 생성하는 EAMM의 접근 방식과 달리 눈 깜빡임과 같은 고주파 정보를 포착합니다.
- 입술 다듬기: 오디오를 기반으로 입술 움직임을 최적화하여 보다 동기화된 입술 움직임을 구현합니다.

4.4 사용자 연구

20명의 참가자가 참여한 주관적 사용자 연구에서 다양한 음성 생성 기술을 비교했습니다. EAMM은 감정을 생성할 수 있었지만 비디오 품질이 떨어졌습니다. 메이크잇톡과 새드토커는 감정 생성이 부족했지만 전반적인 품질이 더 좋았습니다. 반면에 드림톡은 감정 강도와 고품질 생성을 유지하는 데 성공했습니다.

요약하자면, 드림토크는 3D 감정 표현 얼굴을 생성하는 데 있어 상당한 발전을 보여주었습니다. 동적 감정 표현, 립싱크 정확도, 전반적인 비디오 품질 면에서 기존 방식보다 뛰어난 성능을 발휘합니다. 자동 회귀 생성, 감정 스타일 임베딩, 입술 다듬기와 같은 혁신적인 기술의 조합은 드림톡이 감정적인 말하는 얼굴 생성 분야에서 우위를 점할 수 있도록 보장합니다.

### 5. Conclusion

이 논문에서는 정확한 입술 동기화를 통해 감정을 표현하는 말하는 얼굴을 생성하는 혁신적인 솔루션으로 DREAM-Talk 프레임워크를 소개했습니다. 이 2단계 접근 방식은 EmoDiff 모듈과 립 정제 프로세스로 구성됩니다. 드림톡의 핵심 요소인 이모디프 모듈은 감정 조건부 확산 모델을 사용하여 감정 표현의 뉘앙스를 효과적으로 포착합니다. 한편, 립 리파인데이션 단계에서는 입술의 움직임을 발화 단어와 정확하게 일치시켜 말하는 얼굴의 전체적인 사실감을 향상시키는 데 중점을 둡니다.

드림톡의 가장 큰 성과는 현장에서 기존 기술을 뛰어넘는 성능입니다. 높은 비디오 품질을 유지하면서 얼굴의 감정 표현력이 눈에 띄게 향상되었습니다. 이러한 발전으로 DREAM-Talk는 감정적인 말하는 얼굴을 생성하는 영역에서 중요한 돌파구를 마련했습니다.

드림톡은 다양한 영역에 걸쳐 잠재적으로 적용될 수 있는 광범위한 의미를 지니고 있습니다. 가상현실, 엔터테인먼트, 커뮤니케이션 및 기타 인터랙티브 기술 분야에서 중요한 요소인 사실적일 뿐만 아니라 감정적으로 몰입할 수 있는 디지털 인간 표현을 만들 수 있습니다.

결론적으로, 드림톡은 말하는 얼굴 개발에서 상당한 진전을 이루었으며, 실감나고 감정이 풍부하며 동시에 립싱크가 가능한 디지털 휴먼 아바타를 생성할 수 있는 방법을 제공합니다. 이 프레임워크는 이 분야의 새로운 표준을 제시하고 디지털 인간 상호 작용의 미래를 위한 새로운 가능성을 열어줍니다.