# Wav2Lip: Accurately Lip-syncing Videos In The Wild

[https://github.com/Rudrabha/Wav2Lip](https://github.com/Rudrabha/Wav2Lip)

[https://arxiv.org/abs/2008.10010](https://arxiv.org/abs/2008.10010)

- Aug 2020

![새로운 Wav2Lip 모델은 역동적이고 제약이 없는 말하는 얼굴 동영상에서 훨씬 더 정확한 립싱크를 생성합니다. 정량적 지표에 따르면 생성된 비디오의 립싱크는 실제 동기화된 비디오와 거의 비슷합니다. 따라서 우리의 모델은 이전의 화자 독립적인 립싱크 접근 방식[17, 18]이 만족스러운 결과를 얻기 어려운 광범위한 실제 애플리케이션을 지원할 수 있다고 생각합니다.](Wav2Lip%20Accurately%20Lip-syncing%20Videos%20In%20The%20Wild%204e4ebce8dcde452ebebc95f85fa32d5c/Untitled.png)

새로운 Wav2Lip 모델은 역동적이고 제약이 없는 말하는 얼굴 동영상에서 훨씬 더 정확한 립싱크를 생성합니다. 정량적 지표에 따르면 생성된 비디오의 립싱크는 실제 동기화된 비디오와 거의 비슷합니다. 따라서 우리의 모델은 이전의 화자 독립적인 립싱크 접근 방식[17, 18]이 만족스러운 결과를 얻기 어려운 광범위한 실제 애플리케이션을 지원할 수 있다고 생각합니다.

### 1 INTRODUCTION

이 논문의 도입부에서는 동영상 콘텐츠의 신속한 제작, 특히 다양한 언어로 동영상을 제작해야 하는 필요성에 대해 설명합니다. 이 과정에서 중요한 측면은 말하는 얼굴 비디오를 다른 언어의 대상 음성과 일치하도록 립싱크하는 것입니다. 이 문제는 연구 커뮤니티에서 상당한 주목을 받아왔으며, 초기에는 단일 화자의 음성을 입술 움직임에 매핑하는 데 중점을 두었습니다. 최근 연구에서는 특정 화자에 국한되어 있지만 음성에서 직접 이미지를 생성하는 데 진전이 있었습니다.

화자 독립적 모델

이 논문은 텍스트 음성 변환 시스템의 합성 음성을 포함해 다양한 신원과 음성을 처리할 수 있는 화자 독립적 모델의 필요성을 강조합니다. 이러한 모델은 정적인 이미지를 넘어 동적이고 제약이 없는 비디오 콘텐츠까지 확장하여 모든 목소리의 모든 신원에 대한 입술 움직임을 정확하게 생성해야 합니다.

연구 초점 및 기여

저자들은 기존의 화자 독립적 모델에서 실제 동영상에서 다양한 입술 모양을 정확하게 변형하지 못해 종종 동기화되지 않는 부분이 발생한다는 점을 발견했습니다. 이 논문은 립싱크에 필요한 높은 정밀도(0.05~0.1초 정도의 오차도 눈에 띄게 나타남)를 인식하고 화자별 추가 데이터 없이 이러한 문제를 극복하는 데 중점을 둡니다.

이 연구는 임의의 말하는 얼굴 비디오와 임의의 음성을 동기화하는 데 탁월한 새로운 립싱크 네트워크인 'Wav2Lip'을 소개합니다. 저자는 제약이 없는 동영상에서 립싱크를 평가하기 위한 엄격한 벤치마크와 메트릭을 갖춘 새로운 평가 프레임워크를 제안합니다. 또한 립싱크 모델을 벤치마킹하기 위한 실제 동영상 데이터 세트인 'ReSyncED'를 소개합니다.

주요 주장

- "Wav2Lip"은 실제 립싱크 동영상에서 이전 모델보다 더 정확합니다.
- 제약이 없는 동영상에서 공정한 평가를 위한 새로운 평가 프레임워크.
- 보이지 않는 동영상에서의 성능 벤치마킹을 위한 데이터 세트인 "ReSyncED" 출시.
- "Wav2Lip"은 실제 동기화된 동영상과 일치하는 립싱크 정확도를 달성한 최초의 화자 독립적 모델로, 90% 이상의 인간 평가에서 호평을 받았습니다.

이  논문에서는 사용자가 자신의 오디오 및 비디오 샘플로 모델을 사용해 볼 수 있도록 웹 사이트에서 데모 비디오와 대화형 데모를 언급하고 있습니다. 백서의 나머지 부분은 이 분야의 최근 발전 상황을 다루고, 제안된 접근 방식을 설명하고, 새로운 평가 프레임워크를 제안하고, 잠재적인 적용 사례와 윤리적 문제를 논의하고, 연구를 마무리하도록 구성되어 있습니다.

### 2 RELATED WORK

2.1 음성을 통한 제한된 말하는 얼굴 생성

이 섹션에서는 말하는 얼굴 동영상을 생성하지만 처리할 수 있는 신원 및 어휘의 범위에 제약을 받는 작업을 검토하는 것으로 시작합니다. 이 분야의 초기 성과는 주로 버락 오바마 전 대통령과 같은 특정 인물에 초점을 맞춰 오디오 입력과 입술 움직임 사이의 관계를 규명하는 데 집중되었습니다. 하지만 이러한 방법은 훈련받은 화자에 국한되어 새로운 목소리나 신원에 적응할 수 없었습니다. 또한 각 화자에 대한 상당한 양의 데이터가 필요했으며, 보통 몇 시간이 걸렸습니다.

최근 이러한 데이터 요구 사항을 줄이기 위한 몇 가지 개발이 시도되었습니다. 예를 들어, 한 가지 접근 방식은 화자당 1시간의 데이터만 사용하여 문구를 추가하거나 제거하는 방식으로 동영상을 편집할 수 있었습니다. 또 다른 방법은 화자별 특징을 먼저 학습하여 약 5분으로 줄였지만, 이 경우에도 각 대상 화자에 대한 깨끗한 데이터가 필요했습니다. 이러한 방법의 가장 큰 한계는 제한된 어휘와 제한된 단어로 구성된 데이터 세트에 의존하기 때문에 실제 연설에서 음소와 비젬(입술 움직임)의 다양한 매핑을 학습하는 데 장애가 된다는 점입니다. 이 논문은 모든 정체성, 목소리, 어휘에 대한 립싱크에 초점을 맞춰 이러한 제약을 극복하는 것을 목표로 합니다.

2.2 음성에서 제약 없는 말하는 얼굴 생성하기

제약이 있는 방법과 달리 신원, 목소리, 언어에 대한 제약 없이 말하는 얼굴 영상을 생성하는 연구가 등장했습니다. 특히 두 개의 저명한 연구를 포함한 소수의 연구만이 이 분야에서 상당한 진전을 이루었습니다. 이 연구들은 주어진 오디오 세그먼트와 참조 얼굴 이미지로부터 얼굴의 립싱크 버전을 생성하는 작업을 공식화했습니다. 특히, LipGAN 모델은 마스킹된 대상 얼굴을 사용하여 포즈를 안내함으로써 생성된 얼굴을 원본 비디오에 원활하게 통합할 수 있었습니다.

하지만 이러한 접근법의 가장 큰 단점은 주로 정적인 이미지에서 성능이 떨어진다는 점입니다. 실제 환경에서 동적이고 제약이 없는 동영상에서 입술의 움직임을 정확하게 생성하는 데 어려움을 겪습니다. 이러한 방법과 달리, 이 논문에서는 제너레이터로 추가 학습하지 않고 사전 학습된 매우 정확한 립싱크 판별기를 사용합니다. 이러한 설계 선택은 우수한 립싱크 결과를 얻는 데 매우 중요하며, 이 논문에서 소개된 혁신의 발판을 마련했습니다.

### 3 ACCURATE SPEECH-DRIVEN LIP-SYNCING FOR VIDEOS IN THE WILD

핵심 아키텍처 개요

이 섹션에서는 동영상에서 정확한 립싱크를 생성하기 위해 제안된 방법의 핵심 아키텍처를 간략하게 설명합니다. 이 접근 방식은 잘 훈련된 립싱크 전문가의 학습을 기반으로 합니다. 저자들은 기존 아키텍처가 야생에서 립싱크 비디오의 정확도를 떨어뜨리는 두 가지 주요 원인으로 부적절한 손실 함수와 약한 립싱크 판별자를 꼽았습니다.

![우리의 접근 방식은 "이미 잘 훈련된 립싱크 전문가"로부터 학습하여 정확한 립싱크를 생성합니다. 재구성 손실만 사용하거나[17] GAN 설정에서 판별자를 훈련시키는 이전 작업[18]과 달리, 우리는 이미 립싱크 오류를 매우 정확하게 감지하는 사전 훈련된 판별자를 사용합니다. 노이즈가 생성된 얼굴에 대해 더 미세하게 조정하면 판별기의 립싱크 측정 능력이 저하되어 생성된 입술 모양에도 영향을 미친다는 것을 보여줍니다. 또한 시각적 품질 판별기를 사용하여 동기화 정확도와 함께 시각적 품질을 개선합니다.](Wav2Lip%20Accurately%20Lip-syncing%20Videos%20In%20The%20Wild%204e4ebce8dcde452ebebc95f85fa32d5c/Untitled%201.png)

우리의 접근 방식은 "이미 잘 훈련된 립싱크 전문가"로부터 학습하여 정확한 립싱크를 생성합니다. 재구성 손실만 사용하거나[17] GAN 설정에서 판별자를 훈련시키는 이전 작업[18]과 달리, 우리는 이미 립싱크 오류를 매우 정확하게 감지하는 사전 훈련된 판별자를 사용합니다. 노이즈가 생성된 얼굴에 대해 더 미세하게 조정하면 판별기의 립싱크 측정 능력이 저하되어 생성된 입술 모양에도 영향을 미친다는 것을 보여줍니다. 또한 시각적 품질 판별기를 사용하여 동기화 정확도와 함께 시각적 품질을 개선합니다.

3.1 립싱크의 약한 판별자로서의 픽셀 수준 재구성 손실

이 논문은 기존 작품에서 사용되는 픽셀 수준 재구성 손실이 립싱크 정확도를 판단하는 데 취약하다고 비판합니다. 이 손실은 얼굴과 배경을 포함한 전체 이미지에 대해 계산되기 때문에 세밀한 입술 모양 보정에 덜 집중하게 됩니다. 입술 영역은 전체 재구성 손실의 작은 부분이기 때문에 훈련 과정의 후반부까지 적절하게 최적화되지 않습니다.

3.2 약한 립싱크 판별자

기존 LipGAN의 립싱크 판별기는 오프싱크 오디오-입술 쌍을 감지하는 정확도가 약 56%에 불과한 것으로 나타났는데, 이 연구에서 사용된 전문가 판별기는 91%의 정확도를 보였습니다. 저자들은 립싱크 감지를 위해 단일 프레임에 의존하고 노이즈가 생성된 이미지로 판별기를 훈련시켜 오디오와 입술의 일치보다는 시각적 아티팩트에 초점을 맞추기 때문이라고 설명합니다.

3.3 립싱크 전문가가 필요함

제안된 솔루션은 생성된 프레임에 대한 미세 조정 없이 사전 훈련된 전문 립싱크 판별기(SyncNet)를 사용하는 것입니다. 얼굴 인코더와 오디오 인코더를 모두 사용하여 오디오와 비디오 간의 동기화를 구별하도록 훈련된 SyncNet을 사용합니다. 작성자는 입력 이미지 유형, 네트워크 깊이, 손실 함수를 변경하는 등 작업에 맞게 SyncNet을 수정합니다.

3.4 립싱크 전문가로부터 학습하여 정확한 립싱크 생성하기

정확한 립싱크 판별기가 훈련 중 부정확한 립싱크에 대해 제너레이터에 불이익을 주는 데 어떻게 사용되는지 설명합니다. 제너레이터 아키텍처에는 신원 인코더, 음성 인코더, 얼굴 디코더가 포함됩니다. 제너레이터는 생성된 프레임과 기준 프레임 간의 재구성 손실을 최소화하도록 훈련됩니다. 립싱크의 정확성을 보장하기 위해 제너레이터는 판별기의 전문가 동기화 손실을 사용하여 페널티를 받기도 합니다.

3.5 사실적인 얼굴 생성하기

정확한 입술 모양을 구현했음에도 불구하고 생성된 얼굴이 약간 흐릿하게 보이거나 아티팩트가 포함되는 경우가 있었습니다. 이 문제를 해결하기 위해 제작자는 GAN 설정에서 훈련된 시각적 품질 판별기를 추가했습니다. 이 판별기는 립싱크 정확도에 영향을 주지 않으면서 비현실적인 얼굴 생성에 불이익을 주는 데 중점을 둡니다. 전체 네트워크는 동기화 정확도와 시각적 품질에 대한 두 가지 판별자를 사용하여 최적화되어 우수한 결과를 보장합니다.

모델 훈련 및 추론

모델은 옵티마이저와 손실 함수에 대한 특정 설정이 있는 LRS2 훈련 세트에서만 훈련됩니다. 추론하는 동안 모델은 시각적 입력과 해당 오디오 세그먼트를 모두 사용하여 말하는 얼굴 비디오를 프레임 단위로 생성합니다.

공개 및 평가

저자들은 모든 코드와 모델이 공개될 것이며, 다음 섹션에서는 이전 모델과 비교하여 접근 방식에 대한 정량적 평가를 제공할 것이라고 언급하며 마무리합니다.

### 4 QUANTITATIVE EVALUATION

이 섹션에서는 모델의 정량적 평가에 대해 자세히 살펴봅니다. 이 모델은 LRS2 훈련 세트에서만 훈련되었음에도 불구하고 세 가지 다른 데이터 세트에서 평가됩니다. 또한 현재의 평가 프레임워크를 재검토하여 부적절한 부분을 강조하고 실제 립싱크 정확도를 평가하기 위한 새로운 벤치마크와 지표를 제안합니다.

![제안된 모델에서 생성된 얼굴의 예(녹색 및 노란색 윤곽선). 현재 가장 좋은 접근 방식[18]과 비교합니다(빨간색 윤곽선). 그림에 표시된 텍스트는 표시된 프레임에서 발화되는 발화를 나타냅니다. 우리 모델이 정확하고 자연스러운 입술 모양을 만들어내는 것을 볼 수 있습니다. 시각적 품질 판별자를 추가하면 시각적 품질도 크게 향상됩니다. 웹 사이트에서 데모 동영상을 확인해 보시기 바랍니다.](Wav2Lip%20Accurately%20Lip-syncing%20Videos%20In%20The%20Wild%204e4ebce8dcde452ebebc95f85fa32d5c/Untitled%202.png)

제안된 모델에서 생성된 얼굴의 예(녹색 및 노란색 윤곽선). 현재 가장 좋은 접근 방식[18]과 비교합니다(빨간색 윤곽선). 그림에 표시된 텍스트는 표시된 프레임에서 발화되는 발화를 나타냅니다. 우리 모델이 정확하고 자연스러운 입술 모양을 만들어내는 것을 볼 수 있습니다. 시각적 품질 판별자를 추가하면 시각적 품질도 크게 향상됩니다. 웹 사이트에서 데모 동영상을 확인해 보시기 바랍니다.

4.1 평가 프레임워크에 대한 재검토

저자들은 현재의 평가 프레임워크가 실제 적용성이 부족하고 일관성이 없다고 비판합니다. 이들은 평가 시 실제 애플리케이션에서 사용되는 실제 프레임이 아닌 임의의 참조 프레임을 사용하면 모델의 성능을 정확하게 반영하지 못한다고 주장합니다. 또한 이 방법은 시간적 일관성을 유지하지 못하며 립싱크에 특화된 지표를 사용하지 않고 일반적인 이미지 품질 지표를 사용합니다.

4.2 립싱크 평가를 위한 새로운 벤치마크 및 지표

이러한 문제를 해결하기 위해 저자들은 사전 학습된 SyncNet을 사용하여 립싱크 오류를 측정하는 새로운 접근 방식을 제안합니다. 이 방법은 비디오와 무작위로 선택한 오디오 쌍으로 일관된 테스트 세트를 생성하여 보다 신뢰할 수 있는 벤치마크를 제공합니다. "LSE-D"(립싱크 오류 - 거리)와 "LSE-C"(립싱크 오류 - 신뢰도)라는 이름의 새로운 지표는 립싱크의 정확도를 구체적으로 측정하도록 설계되었습니다.

4.3 새로운 벤치마크의 모델 비교

저자들은 이러한 새로운 지표를 사용하여 모델을 이전 접근 방식과 비교했습니다. 그 결과 제안된 방법을 사용하면 립싱크 정확도와 시각적 품질이 크게 향상되는 것으로 나타났습니다. 그러나 동기화 정확도와 시각적 품질 사이에 약간의 트레이드오프가 있음을 발견하고 두 가지 버전의 모델을 출시하게 되었습니다.

4.4 실제 평가

이 평가는 새로 생성된 데이터 세트인 "ReSyncED"(실제 평가 데이터 세트)를 통해 실제 시나리오로 확장됩니다. 이 데이터 세트에는 더빙된 콘텐츠와 텍스트 음성 변환 시스템의 합성 음성이 포함된 비디오 등 다양한 유형의 비디오가 포함됩니다. 모델은 새로운 지표를 사용하여 자동으로 평가되거나 동기화 정확도, 시각적 품질, 전반적인 경험 및 선호도를 판단하는 인간 평가자를 통해 평가됩니다.

4.5 전문가 판별자 평가

마지막으로 저자는 전문가 판별자에서 디자인 선택을 정당화하기 위해 제거 연구를 수행합니다. 이들은 생성된 얼굴에 대한 판별자를 미세 조정하지 않고 더 큰 시간적 창을 사용하면 립싱크 판별이 더 잘된다는 것을 입증합니다. 생성된 얼굴에 대한 미세 조정은 판별기가 오디오-입술 대응이 아닌 시각적 아티팩트에 집중하게 만들어 립싱크 정확도를 떨어뜨린다고 주장합니다.

결론적으로, 이 섹션에서는 통제된 벤치마크와 실제 시나리오에서 종합적인 평가를 통해 제안된 접근 방식의 효과를 입증하고 립싱크 정확도와 시각적 품질에서 이전 모델보다 우수하다는 점을 강조합니다.

### 5 APPLICATIONS & FAIR USE

이 논문에서는 시청각 콘텐츠의 소비가 증가하고 대규모 비디오 번역 및 제작이 필요한 상황에서 Wav2Lip 모델의 관련성을 강조합니다. 온라인 강의, 더빙 영화, 대중 연설 등 다양한 시나리오에서 비디오를 정확하게 립싱크할 수 있는 이 모델의 능력이 강조되어 있습니다. 이 기능은 다양한 언어로 더빙된 연설에 입술을 동기화하여 시청 경험을 향상시킬 수 있으며, 영화와 게임에서 CGI 캐릭터를 애니메이션화하는 데도 사용할 수 있어 수작업으로 인한 시간을 절약할 수 있습니다.

하지만 저자들은 이러한 첨단 립싱크 기술의 공정한 사용을 촉진하는 것도 중요하다고 강조합니다. Wav2Lip의 거의 사실적인 기능을 고려할 때 오용의 위험이 잠재적으로 존재합니다. 따라서 그들은 자신들의 모델을 사용하여 만든 모든 콘텐츠를 합성으로 명확하게 표시해야 한다고 주장합니다. 이들의 작업을 오픈소스화하려는 의도는 긍정적인 적용을 촉진할 뿐만 아니라 조작된 동영상 콘텐츠의 오용을 탐지하고 방지하는 연구와 토론을 장려하기 위한 것입니다.

### 6 CONCLUSION

결론적으로 이 논문은 제약이 없는 환경에서 정확한 립싱크 비디오를 생성하기 위한 새로운 접근 방식을 제시합니다. 자연스러운 입술 모션 생성을 위해 사전 훈련된 정확한 립싱크 '전문가'를 활용함으로써 현재 방법의 단점을 해결합니다. 또한 저자들은 현재의 정량적 평가 프레임워크를 비판적으로 재평가하여 립싱크 기술을 더욱 신뢰할 수 있고 정확하게 평가할 수 있는 새로운 벤치마크, 메트릭, 실제 평가 세트를 제안합니다.

Wav2Lip 모델은 정량적 평가와 정성적 평가 모두에서 기존 방법보다 훨씬 뛰어난 성능을 보였습니다. 이 논문은 이 분야의 향후 연구에 대한 격려와 함께, 이들의 연구가 정확한 입술 움직임뿐만 아니라 표정과 머리 포즈를 합성하는 데 새로운 방향을 제시할 수 있음을 시사하며 마무리됩니다. 저자들은 웹사이트에서 데모 동영상을 통해 이 모델이 실제로 작동하는 모습을 볼 수 있도록 독자들을 초대합니다.