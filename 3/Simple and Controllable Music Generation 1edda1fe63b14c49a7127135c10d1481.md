# Simple and Controllable Music Generation

[https://github.com/facebookresearch/audiocraft](https://github.com/facebookresearch/audiocraft)

[https://ai.honu.io/papers/musicgen/](https://ai.honu.io/papers/musicgen/)

1 Introduction

이 글에서는 텍스트 설명을 기반으로 고품질의 음악을 생성할 수 있는 뮤직젠이라는 음악 생성 모델을 만드는 방법에 대해 설명합니다. 이해하기 쉽게 분석해 보겠습니다:

도전 과제: 음악은 복잡하기 때문에 인공지능을 사용하여 음악을 생성하는 것은 어렵습니다. 음악은 광범위한 주파수와 긴 음표 시퀀스를 가지고 있으며 다양한 악기의 하모니와 멜로디가 결합되어 있습니다. 게다가 인간은 음이 이상하게 들리는 것을 잘 알아차리기 때문에 인공지능이 실수를 거의 하지 않아야 합니다. 또한 사용자가 건반, 악기, 멜로디, 장르 등 다양한 측면을 제어할 수 있도록 해야 하는 과제도 있습니다.

배경 연구: 최근의 몇 가지 발전은 AI를 사용하여 음악을 생성하는 능력을 향상시키는 데 도움이 되었습니다. 이러한 발전에는 오디오 표현 학습, 시퀀스 생성, 오디오 합성 등이 포함됩니다. 유용한 기술 중 하나는 오디오를 여러 개의 개별 토큰 스트림 또는 사운드를 나타내는 개별 정보 조각으로 표현하는 것입니다. 그러나 이러한 스트림은 상호 의존적이며 동시에 처리해야 하므로 작업이 더 복잡해집니다.

이전 작업: 이러한 스트림을 처리하기 위해 지연을 시키거나 계층적으로 모델링하는 등 다양한 접근 방식이 있었습니다. 일부 연구자들은 토큰의 첫 번째 스트림만 모델링한 다음 네트워크를 사용하여 나머지 스트림을 비순차적으로 모델링하는 두 단계로 문제를 처리할 것을 제안했습니다.

뮤직젠: 이 모델은 이러한 여러 병렬 음향 토큰 스트림을 관리하기 위한 프레임워크를 도입했습니다. 이 모델은 비지도 멜로디 컨디셔닝을 사용하여 생성된 샘플의 제어를 개선하여 모델이 주어진 화성과 멜로디 구조에 맞는 음악을 생성할 수 있도록 합니다. 광범위한 평가에 따르면 뮤직젠은 다른 방법보다 상당한 차이로 더 나은 성능을 발휘하는 것으로 나타났습니다. 생성된 음악은 고품질이며 텍스트 설명을 따르면서 주어진 화성 구조에 잘 맞습니다.

![섹션 2.2에 제시된 코드북 인터리빙 패턴. 각 시간 단계 t1, t2, . . . , tn은 4개의 양자화된 값으로 구성됩니다(k1, . . . , k4에 해당). 자동 회귀 모델링을 할 때 을 사용하면 다양한 방식으로 평탄화하거나 인터리브하여 4개의 병렬 스트림이 있는 새로운 시퀀스를 만들 수 있습니다. 과 단계 s1, s2, . . . , sm. 시퀀스 단계의 총 수 M은 패턴과 원본에 따라 다릅니다. 단계 수 N. 0은 패턴에서 빈 위치를 나타내는 특수 토큰입니다.](Simple%20and%20Controllable%20Music%20Generation%201edda1fe63b14c49a7127135c10d1481/Untitled.png)

섹션 2.2에 제시된 코드북 인터리빙 패턴. 각 시간 단계 t1, t2, . . . , tn은 4개의 양자화된 값으로 구성됩니다(k1, . . . , k4에 해당). 자동 회귀 모델링을 할 때 을 사용하면 다양한 방식으로 평탄화하거나 인터리브하여 4개의 병렬 스트림이 있는 새로운 시퀀스를 만들 수 있습니다. 과 단계 s1, s2, . . . , sm. 시퀀스 단계의 총 수 M은 패턴과 원본에 따라 다릅니다. 단계 수 N. 0은 패턴에서 빈 위치를 나타내는 특수 토큰입니다.

기여도: MUSICGEN의 제작자는 32kHz의 주파수에서 고음질 음악을 생성할 수 있는 간단하고 효율적인 모델을 만들었습니다. 이들은 단일 단계 언어 모델과 여러 정보 스트림을 처리하는 효율적인 전략으로 MUSICGEN이 일관된 음악을 생성할 수 있음을 입증했습니다. 뮤직젠은 제공된 텍스트와 멜로디 모두에 일관된 음악을 생성할 수 있습니다. 모델에서 각 구성 요소의 중요성을 강조하기 위해 철저한 평가를 수행했습니다.

2 Method

설명해주신 뮤직젠 시스템은 텍스트 또는 멜로디 표현에 따라 음악 생성에 사용되는 새로운 자동 회귀 트랜스포머 기반 모델입니다. 다음은 그 아키텍처와 특징을 요약한 것입니다:

오디오 토큰화: 뮤직젠은 잔여 벡터 양자화(RVQ)를 사용하여 양자화된 잠재 공간을 가진 컨볼루션 자동 인코더인 EnCodec을 사용합니다. 이 인코더는 프레임 속도가 현저히 낮은 연속 텐서로 오디오를 인코딩합니다. 그런 다음 연속적인 표현은 오디오 샘플을 나타내는 일련의 이산 토큰으로 양자화됩니다.

코드북 인터리빙 패턴: 이 시스템은 코드북 인터리빙 패턴을 사용하여 각 시간 단계마다 여러 개의 코드북이 필요한 문제를 처리하며, 이는 RVQ로 인해 발생합니다. 정확한 평탄화 자동 회귀 분해, 부정확한 자동 회귀 분해, 임의의 코드북 인터리빙 패턴 등 다양한 분해가 있습니다. 분해 패턴의 선택은 전체 모델의 효과와 효율성에 큰 영향을 미칠 수 있습니다.

모델 컨디셔닝: 이 모델은 텍스트 또는 멜로디에 기반한 조건부 생성을 지원합니다. 텍스트 컨디셔닝의 경우 사전 학습된 텍스트 인코더(T5), 명령어 기반 언어 모델, CLAP과 같은 텍스트-오디오 공동 표현 모델을 사용하는 등 다양한 방법이 테스트되었습니다. 멜로디 컨디셔닝의 경우, 이 모델을 사용하면 입력의 크로마그램과 텍스트 설명을 공동으로 컨디셔닝하여 멜로디 구조를 제어할 수 있습니다.

모델 아키텍처: 이 아키텍처에는 코드북 투영 및 위치 임베딩 프로세스와 트랜스포머 디코더가 포함됩니다. 모델의 최종 계층은 트랜스포머 디코더의 출력을 Ps+1에 의해 주어진 인덱스에서 취한 Q 값에 대한 로짓 예측으로 변환합니다.

이러한 기법을 활용하여 뮤직젠은 제공된 텍스트 또는 멜로디 입력을 기반으로 고품질의 음악 작곡을 생성할 수 있습니다. 다양한 인터리빙 패턴과 컨디셔닝 접근 방식을 사용하여 모델의 성능을 최적화할 수 있는 유연성과 다양한 옵션을 제공합니다.

3 Experimental setup

뮤직젠이라는 음악 생성 시스템을 설명하는 것 같습니다. 핵심 사항을 간략하게 요약하면 다음과 같습니다:

오디오 토큰화 모델: 32kHz 모노 포닉 오디오에는 5개의 레이어로 구성된 비인과적 EnCodec 모델이 사용됩니다. 여기에는 각각 코드북 크기가 2048인 4개의 양자화기가 있는 RVQ로 양자화된 임베딩이 사용됩니다. 이 모델은 무작위로 잘린 1초 오디오 세그먼트로 훈련됩니다.

트랜스포머 모델: 다양한 크기(300M, 1.5B, 3.3B 매개변수)의 자동 회귀 트랜스포머 모델은 메모리 효율성을 위해 플래시 주의력을 사용하여 훈련됩니다. 이 모델은 30초 오디오 크롭으로 훈련되며, AdamW 옵티마이저 사용, 192개의 예제 배치 크기, 1.0의 그라디언트 클리핑, 4000단계의 워밍업이 포함된 코사인 학습 속도 스케줄 등 여러 훈련 파라미터가 지정됩니다. 샘플링은 상위 k 샘플링을 사용하여 수행됩니다.

텍스트 전처리: 중단 단어를 생략하고 나머지 텍스트를 정규화하는 텍스트 정규화 방식이 사용됩니다. 추가 음악 데이터 세트 주석은 텍스트 설명과 연결하여 실험합니다.

코드북 패턴 및 컨디셔닝: "지연" 인터리빙 패턴이 사용됩니다. 텍스트 컨디셔닝에는 멜로디 컨디셔닝이 추가된 T5 텍스트 인코더가 사용됩니다. 멜로디 컨디셔닝을 위해 크로마그램이 계산됩니다.

데이터 세트: 뮤직젠은 10,000개의 고품질 음악 트랙으로 구성된 내부 데이터 세트와 셔터스톡 및 폰드5 음악 데이터 컬렉션을 포함하여 20,000시간 분량의 라이선스 음악으로 학습됩니다. 이 모델은 MusicCaps 벤치마크와 528개의 음악 트랙으로 구성된 도메인 내 평가 세트에서 평가됩니다.

평가: 뮤직젠은 리퓨전, 무사이, 뮤직엘엠, 노이즈2뮤직과 같은 다른 모델과 비교됩니다. 객관적인 지표로는 프레셰 오디오 거리(FAD), 쿨백-라이버 다이버전스(KL), CLAP 점수가 사용됩니다. 평가자가 전반적인 품질과 텍스트 입력과의 관련성에 대해 샘플에 점수를 매기는 인간 연구도 평가에 사용됩니다.

4 Results

제공된 설명을 바탕으로 연구원들은 MUSICGEN이라는 텍스트-음악 생성 모델을 개발했으며, 기존의 여러 모델 및 기준선과 비교하여 그 성능을 평가했습니다. 좀 더 자세히 살펴보겠습니다.

뮤직젠 모델: 이 모델은 트랜스포머 아키텍처를 기반으로 하며, 텍스트 프롬프트에 대해 학습된 버전과 텍스트 프롬프트와 멜로디 입력(크로마그램) 모두에 대해 학습된 버전이 있습니다.

훈련 및 평가 데이터 세트: 연구원들은 내부 데이터 세트와 셔터스톡 및 폰드5 음악 데이터 컬렉션을 포함한 20,000시간 분량의 라이선스 음악으로 뮤직젠을 훈련시켰습니다. 모델 평가는 MusicCaps 벤치마크와 528개의 음악 트랙으로 구성된 도메인 내 보유 세트를 사용하여 수행되었습니다.

평가 지표: 모델의 성능을 평가하기 위해 프레셰 오디오 거리(FAD), 쿨백-라이버 발산(KL), 박수 점수, 멜로디 평가를 위한 크로마 코사인 유사도 등 여러 가지 객관적 및 주관적 지표를 사용했습니다. 또한 평가자가 생성된 음악의 전반적인 품질과 제공된 텍스트 입력과의 관련성을 평가하는 인간 연구도 수행되었습니다.

기준선과의 비교: 뮤직젠은 무사이, 리퓨전, 뮤직엘엠, 노이즈2뮤직 등 여러 기존 모델과 비교되었습니다. 제시된 결과를 보면, 오디오 품질과 제공된 텍스트 설명에 대한 준수 측면에서 모두 MUSICGEN이 평가된 기준선보다 더 나은 성능을 보이는 것으로 나타났습니다.

멜로디 평가: 크로마그램 컨디셔닝으로 훈련된 모델의 경우, 연구진은 새로운 메트릭과 인간 연구를 도입하여 컨디셔닝된 멜로디가 생성된 음악과 얼마나 잘 일치하는지를 평가했습니다. 그 결과, 뮤직젠은 주어진 멜로디를 따르는 음악을 생성할 수 있어 생성된 결과물을 더 잘 제어할 수 있는 것으로 나타났습니다.

절제 연구: 코드북 인터리빙 패턴의 효과와 모델 크기의 영향을 포함하여 모델의 여러 측면을 개별적으로 연구했습니다.

요약하자면, 연구진은 텍스트와 선택적으로 멜로디 입력에 따라 음악을 생성할 수 있는 모델을 개발했으며, 다른 모델과의 평가 및 비교를 통해 이 모델의 잠재적 효과를 입증했습니다. 또한 코드북 패턴 및 모델 크기와 같은 다양한 요소가 모델의 출력에 어떤 영향을 미칠 수 있는지도 보여주었습니다.

5 Related work

이 책은 오디오 표현 및 음악 생성 분야의 관련 작업에 대한 개요를 제공하며, 특히 신경망 접근 방식에 중점을 둡니다. 크게 세 가지 영역으로 나눌 수 있습니다:

오디오 표현: 이 섹션에서는 생성 모델에서 사용하기 위해 음악 신호가 어떻게 표현되었는지 살펴봅니다. 특히, 음성 언어 모델을 구축하기 위해 K-평균을 통해 음성 표현을 정량화하는 방법이 Lakhotia 외. [2021]에 의해 제안되었습니다. 그 후 데포세즈 외[2022]와 제기두르 외[2021]는 벡터 양자화 가변 자동 인코더(VQ-VAE)를 원시 파형에 직접 적용했으며, 이 접근 방식은 이후 다른 연구에서 텍스트-오디오 생성에 사용되었습니다.

음악 생성: 음악 생성에는 다양한 접근 방식이 사용되었습니다. 여기에는 기호 음악 생성을 위한 GAN 기반 방법(Dong et al., [2018]), 기호 음악에 대한 비지도 분할(Bassan et al., [2022]), 반복 신경망을 사용한 다성 음악 모델링(Ycart et al., [2017]) 등이 있습니다. 이 분야의 추가 발전에는 VQ-VAE 및 트랜스포머 사용(Dhariwal 외., [2020]), 미디 음표를 예측하면서 주어진 비디오에 대한 음악을 생성하는 접근 방식(Gan 외., [2020])이 포함됩니다. 최근에는 아고스티넬리(Agostinelli) 등[2023]이 "시맨틱 토큰"과 "음향 토큰"의 다중 스트림을 사용하는 방법을 제안했습니다.

오디오 생성: 여러 연구가 텍스트-오디오 생성에 초점을 맞췄습니다. Yang 등[2022]은 오디오 스펙트로그램을 표현하기 위해 VQ-VAE를 사용한 다음 생성에 이산 확산 모델을 적용했습니다. Kreuk 등[2022]은 이산 오디오 표현에 트랜스포머 언어 모델을 적용할 것을 제안했으며, 다른 연구자들은 텍스트-오디오 및 이미지-오디오 생성과 같은 작업을 위한 잠재 확산 모델에 초점을 맞췄습니다(Huang 등[2023a], Liu 등[2023]).

전반적으로, 앞서 설명한 연구들은 음악 및 오디오 콘텐츠 생성을 위한 보다 발전되고 미묘한 모델을 만들기 위한 업계 전반의 노력을 보여줍니다.

6 Discussion

이 요약에서는 텍스트와 멜로디 모두에 조건을 지정할 수 있는 제어 가능한 음악 생성 모델인 MUSICGEN 모델에 대한 개요를 제공합니다. 이 모델은 코드북 인터리빙 전략을 사용하여 생성 품질을 개선하고 자동 회귀 시간 단계의 수를 줄입니다. 또한 모델 크기, 컨디셔닝 방법, 텍스트 전처리 기법 등의 요소에 대한 포괄적인 연구도 포함되어 있습니다. 또한 저자는 생성된 오디오의 멜로디를 제어하기 위해 크로마그램 기반 컨디셔닝 방법을 도입했습니다.

이 모델의 성공에도 불구하고 저자는 생성된 오디오가 컨디셔닝을 준수하는지에 대한 세밀한 제어가 부족하다는 점을 몇 가지 한계로 지적합니다. 텍스트 컨디셔닝은 간단한 데이터 보강이 가능하지만, 오디오에 대한 컨디셔닝은 추가 연구가 필요합니다.

이 모델의 윤리적 함의에 대해서도 논의합니다. 저자들은 모든 학습 데이터가 합법적으로 획득되었는지 확인했으며, 주로 서양식 음악이 포함된 데이터 세트에 다양성이 부족할 수 있음을 인정했습니다. 또한 제너레이티브 모델이 아티스트에게 불공정한 경쟁을 야기할 수 있는 가능성에 대해서도 논의했습니다. 하지만 멜로디 컨디셔닝을 도입하는 등 모델의 제어 기능을 개선하면 음악 애호가와 전문가 모두에게 유용한 모델이 될 수 있을 것으로 기대했습니다. 또한 공개 연구를 통해 모든 당사자가 이러한 모델에 동등하게 접근할 수 있도록 보장할 수 있다고 주장합니다.

A Appendix

이 섹션에서는 저자가 모델 세부 사항을 자세히 살펴보고 추가 실험 결과에 대해 논의합니다.

모델 세부 사항

코드북 인터리빙 패턴: 저자들은 뮤직젠 모델에서 사용되는 두 가지 코드북 패턴, 즉 부분 평탄화와 부분 지연에 대해 설명합니다. 두 패턴 모두 첫 번째 코드북이 가장 중요하며, 나머지 코드북을 병렬로 예측하면 지연이나 평탄화 발생을 줄이면서 좋은 성능을 유지할 수 있다는 사실을 활용하도록 설계되었습니다.

![양자화된 값의 병렬 스트림 4개(k1, . . . , k4에 해당)와 N개의 시간 단계(t1, . . . , kn)가 있는 시퀀스에 적용된 부분 평탄화 및 부분 지연 코드북 패턴을 시각화합니다. "부분 평탄화"는 첫 번째 코드북을 전용 스텝으로 분리하고 코드북 2, 3, 4의 병렬 샘플링과 인터리브하여 인터리브 시퀀스 스텝 수 M이 원래 스텝 수 N의 두 배가 되도록 합니다. "부분 지연" 패턴은 코드북 2, 3, 4를 동일한 양만큼 지연시키는 것으로 구성되며, 이 경우 1의 지연을 사용합니다. 인터리브 시퀀스의 총 스텝 수는 N입니다(단순화를 위해 마지막 스텝 제외).](Simple%20and%20Controllable%20Music%20Generation%201edda1fe63b14c49a7127135c10d1481/Untitled%201.png)

양자화된 값의 병렬 스트림 4개(k1, . . . , k4에 해당)와 N개의 시간 단계(t1, . . . , kn)가 있는 시퀀스에 적용된 부분 평탄화 및 부분 지연 코드북 패턴을 시각화합니다. "부분 평탄화"는 첫 번째 코드북을 전용 스텝으로 분리하고 코드북 2, 3, 4의 병렬 샘플링과 인터리브하여 인터리브 시퀀스 스텝 수 M이 원래 스텝 수 N의 두 배가 되도록 합니다. "부분 지연" 패턴은 코드북 2, 3, 4를 동일한 양만큼 지연시키는 것으로 구성되며, 이 경우 1의 지연을 사용합니다. 인터리브 시퀀스의 총 스텝 수는 N입니다(단순화를 위해 마지막 스텝 제외).

멜로디 컨디셔닝: 저자들은 크로마그램 표현을 기반으로 멜로디 컨디셔닝을 위한 비지도 접근 방식을 소개합니다. 이 프로세스에는 Demucs라는 프로세스를 사용하여 레퍼런스 트랙을 드럼, 베이스, 보컬 및 기타의 네 가지 구성 요소로 분해하는 과정이 포함됩니다. 잔류 파형의 멜로디 구조를 복구하기 위해 드럼과 베이스는 생략됩니다. 그런 다음 크로마그램을 추출하여 컨디셔닝에 사용합니다.

추가 실험 결과

텍스트 인코더의 효과: 저자는 세 가지 텍스트 인코더를 비교합니다: T5, Flan-T5, CLAP입니다. 실험 결과, T5와 Flan-T5는 객관적인 지표에서 비슷한 성능을 보였으며, T5의 성능이 약간 더 좋았으나 CLAP 기반 모델은 CLAP 점수를 제외한 객관적인 지표와 주관적인 지표가 모두 나빴습니다.

텍스트 증강의 효과: 저자들은 조건 병합, 텍스트 정규화, 단어 드롭아웃을 포함한 텍스트 증강 전략의 영향을 연구합니다. 객관적인 지표에 따르면 조건 병합을 통해 추가 메타데이터를 활용할 때 FAD와 KL이 향상되는 것으로 나타났습니다. 그러나 텍스트 정규화나 단어 드롭아웃은 결과를 개선하지 못했습니다.

![레퍼런스 멜로디에서 시간에 따른 정량화된 크로마그램 빈의 시각화(왼쪽), 크로마와 텍스트를 조건으로 생성된 음악(가운데), 텍스트만 조건으로 생성된 음악(오른쪽). (오른쪽). 각 행은 서로 다른 크로마 조건을 사용하여 생성되며, 모든 행은 다음을 공유합니다. 동일한 텍스트 조건을 공유합니다: "일렉트릭 기타와 무거운 드럼이 있는 90년대 록 노래". 우리는 입력 멜로디에 대한 강한 크로마 컨디셔닝으로 생성된 음악 샘플에 대해 입력 멜로디를 준수하는 동시에 입력 텍스트에 따라 새로운 스타일을 렌더링합니다.](Simple%20and%20Controllable%20Music%20Generation%201edda1fe63b14c49a7127135c10d1481/Untitled%202.png)

레퍼런스 멜로디에서 시간에 따른 정량화된 크로마그램 빈의 시각화(왼쪽), 크로마와 텍스트를 조건으로 생성된 음악(가운데), 텍스트만 조건으로 생성된 음악(오른쪽). (오른쪽). 각 행은 서로 다른 크로마 조건을 사용하여 생성되며, 모든 행은 다음을 공유합니다. 동일한 텍스트 조건을 공유합니다: "일렉트릭 기타와 무거운 드럼이 있는 90년대 록 노래". 우리는 입력 멜로디에 대한 강한 크로마 컨디셔닝으로 생성된 음악 샘플에 대해 입력 멜로디를 준수하는 동시에 입력 텍스트에 따라 새로운 스타일을 렌더링합니다.

D-적응의 효과: 저자들은 아담 옵티마이저의 동적 학습 속도 선택 방법인 D-Adaptation의 효과에 대해 설명합니다. 300M 매개변수 모델에서는 수렴이 개선되었지만 더 큰 모델에서는 성능이 저하되는 것을 관찰했습니다. D-Adaptation의 효과와 가장 큰 모델로 확장할 수 있는지 여부를 이해하려면 추가 조사가 필요합니다.

- 요약
    
    소개: 저자들은 연구의 문제와 목표를 제시합니다. 텍스트와 멜로디에 따라 조절할 수 있는 단일 단계 제어 가능한 음악 생성 모델의 필요성에 대해 설명합니다. 또한 그들이 제안한 모델인 뮤직젠의 독특한 측면에 대해서도 간략하게 언급합니다.
    
    관련 연구: 저자들은 오디오 표현 및 음악 생성 분야의 관련 연구에 대해 논의합니다. 저자들은 자신들의 연구와 다른 사람들의 연구를 연결하여 자신들의 방법이 더 큰 연구 맥락에 어떻게 들어맞는지 논의합니다.
    
    방법론: 저자들은 생성된 오디오의 멜로디를 제어하기 위해 코드북 인터리빙 전략과 크로마그램 기반 컨디셔닝을 어떻게 사용하는지 설명하면서 MUSICGEN의 구조와 기능을 자세히 설명합니다. 또한 모델의 성능을 평가하는 데 사용되는 객관적 및 주관적 평가에 대해서도 설명합니다.
    
    평가: 저자들은 뮤직젠을 기존 모델과 비교하여 오디오 품질과 제공된 텍스트 설명 준수 측면에서 뮤직젠의 우수한 성능을 제시합니다. 또한 다양한 모델 크기, 컨디셔닝 방법, 텍스트 전처리 기법이 MUSICGEN의 성능에 미치는 영향에 대해 논의합니다.
    
    결과: 저자들은 평가 결과를 발표하여 MUSICGEN이 다른 모델보다 성능이 뛰어나며, 간단한 코드북 인터리빙과 같은 특정 전략으로 자동 회귀 시간 단계를 줄이면서 생성 품질을 향상시킬 수 있음을 보여줍니다.
    
    한계 및 윤리적 고려 사항: 저자는 조건 준수에 대한 세밀한 제어 기능을 제공하지 못하는 등 모델의 한계를 인정합니다. 또한 데이터 세트의 다양성 부족과 아티스트에 대한 불공정한 경쟁 가능성 등 연구의 윤리적 함의에 대해서도 논의합니다.
    
    결론: 저자들은 논문의 요점을 다시 한 번 강조하며, 자신들이 제안한 모델인 MUSICGEN이 음악 생성 분야를 발전시킬 수 있는 잠재력을 강조하고 이 모델이 음악 아마추어와 전문가 모두에게 유용할 수 있기를 희망한다고 결론을 내립니다. 또한 오디오 컨디셔닝을 위한 데이터 증강에 대한 추가 연구를 장려합니다.