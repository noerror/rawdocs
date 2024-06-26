# Enforcing Temporal Consistency in Video Depth Estimation

[https://openaccess.thecvf.com/content/ICCV2021W/PBDL/papers/Li_Enforcing_Temporal_Consistency_in_Video_Depth_Estimation_ICCVW_2021_paper.pdf](https://openaccess.thecvf.com/content/ICCV2021W/PBDL/papers/Li_Enforcing_Temporal_Consistency_in_Video_Depth_Estimation_ICCVW_2021_paper.pdf)

[https://github.com/yu-li/TCMonoDepth](https://github.com/yu-li/TCMonoDepth)

- 2021

### 1. Introduction

이 논문은 컴퓨터 비전의 핵심 과제인 2D 이미지와 비디오에서 3D 정보를 복구하는 문제를 다룹니다. 전통적으로 깊이 인식(2.5D 표현의 한 형태)은 증강 현실, 얼굴 인식, 자율 주행 등에 응용되는 이 간극을 메우기 위해 사용되었습니다. 그러나 단일 이미지 또는 비디오에서 깊이를 추정하는 것(단안 깊이 추정)은 축척 모호성으로 인해 매우 어렵기로 악명이 높습니다.

![네 프레임에 대한 두 가지 깊이 추정 결과와 비교. 구조-모션(DeepV2D [41]) 또는 순차 모델(CLSTM [55])을 사용하는 기존 비디오 기반 방법은 적용할 수 있는 장면에 한계가 있으며, 이 두 가지 일반적인 장면에 대한 깊이 추정 결과는 만족스럽지 않습니다. 최첨단 단일 이미지 깊이 예측 방법인 MiDaS [32]는 일반적인 방법이지만, 두 시퀀스를 프레임별로 처리하면 깊이 맵이 깜박입니다. 우리의 방법은 단일 이미지 깊이 방법에 기반을 두고 있지만, 시간적 일관성이 강제되었습니다. 간단한 프레임 별 처리를 통해 시간적으로 안정적인 깊이 추정을 달성하면서 깊이 품질을 유지할 수 있습니다.](Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17/Untitled.png)

네 프레임에 대한 두 가지 깊이 추정 결과와 비교. 구조-모션(DeepV2D [41]) 또는 순차 모델(CLSTM [55])을 사용하는 기존 비디오 기반 방법은 적용할 수 있는 장면에 한계가 있으며, 이 두 가지 일반적인 장면에 대한 깊이 추정 결과는 만족스럽지 않습니다. 최첨단 단일 이미지 깊이 예측 방법인 MiDaS [32]는 일반적인 방법이지만, 두 시퀀스를 프레임별로 처리하면 깊이 맵이 깜박입니다. 우리의 방법은 단일 이미지 깊이 방법에 기반을 두고 있지만, 시간적 일관성이 강제되었습니다. 간단한 프레임 별 처리를 통해 시간적으로 안정적인 깊이 추정을 달성하면서 깊이 품질을 유지할 수 있습니다.

이 문제에 대한 기존 접근 방식은 음영과 텍스처, 물체 크기와 위치, 오클루전 및 원근법 단서 등 다양한 방법을 사용했습니다. 딥러닝이 등장하면서 컨볼루션 신경망(CNN) 기반 방법이 상당한 성과를 거두었습니다.

그러나 이러한 방법은 동영상에 적용할 때 시간적 안정성에 어려움을 겪어 시각적 깜박임이 발생하는 경우가 많습니다. 이 문제를 해결하기 위해 컨볼루션 LSTM과 같은 반복 구조를 사용하는 방법도 있지만, 이러한 방법은 시간적 일관성을 보장하지 않으며 확보하기 어렵고 비용이 많이 드는 안정적인 기준 진실에 크게 의존합니다.

저자들은 비디오 깊이 추정의 시간적 일관성에 초점을 맞춘 새로운 접근 방식을 제안합니다. 저자들은 플리커링이 연속된 프레임의 해당 픽셀이 크게 다를 때 발생한다고 가정합니다. 이러한 상황에서 예측을 제한하고 조정함으로써 모델은 보다 일관된 깊이 추정치를 생성할 수 있습니다. 또한 저자들은 시간에 따른 깊이 추정치의 안정성을 평가하는 새로운 메트릭을 소개합니다.

이 논문은 이 접근 방식이 깊이 추정 품질을 저하시키지 않으면서 시간적 일관성을 개선한다는 것을 보여줍니다. 중요한 점은 이 방법을 일반화할 수 있으며 깊이 기준이 없는 실제 장면에 적용할 수 있다는 것입니다. 저자들은 크게 세 가지를 기여했습니다:

비디오 깊이 추정의 안정성을 평가하는 새로운 메트릭을 소개합니다.
훈련 중에 시간적 제약을 부과하여 깊이 예측 안정성을 개선하는 효과적인 방법을 제안합니다.
또한 이 방법을 확장하여 깊이 기준이 없는 동적 동영상에 적용하여 레이블이 없는 동영상을 사용하여 모델을 효과적으로 정규화할 수 있음을 보여줍니다.

### 2. Related Work

이 논문에서는 2D 이미지와 동영상에서 3D 정보를 복구하는 작업과 관련하여 단일 깊이 추정, 동영상에서 깊이 추정, 시간적 일관성 등 세 가지 영역에 대해 설명합니다.

단일 깊이 추정:
단일 이미지로부터의 깊이 복구에 관한 광범위한 연구가 진행되었으며, 초기에는 음영, 오클루전 또는 시맨틱 레이블을 통해 구조를 예측하는 방법이 사용되었습니다. 딥러닝이 부상하면서 다양한 컨볼루션 아키텍처가 이 작업에 사용되었습니다. 일부 연구자들은 서수 손실 및 멀티스케일 그라데이션 손실과 같이 더 나은 깊이 추정을 위한 특수 손실 함수를 개발했습니다. 이 분야의 한 가지 과제는 단일 이미지를 깊이 결과로 직접 회귀시키려면 레이블이 촘촘하게 지정된 많은 데이터가 필요하기 때문에 수집하기가 어렵다는 점입니다. 이에 대한 해결책으로는 소스 이미지에서 수동으로 심도를 생성하거나, 움직임으로부터의 구조를 사용하여 멀티뷰 재구성에서 심도를 추출하거나, 측광 손실을 사용하는 자체 감독 접근 방식이 있습니다. 그러나 이러한 모든 방법에는 시간적 요소가 없기 때문에 비디오 시퀀스에 적용하면 시각적 깜박임이 발생합니다.

비디오로부터 깊이 추정:
다중 프레임 순차 데이터는 깊이 추정을 위한 추가 정보를 제공할 수 있으며, 여러 연구에서 다중 뷰 제약 조건이 모든 대상 프레임에 대한 기하학적 구조를 해결하는 데 도움이 될 수 있음을 보여줍니다. 이러한 방법은 레이블이 지정된 데이터가 필요하지 않으며 보다 안정적인 결과를 생성하는 경향이 있습니다. 하지만 동적인 오브젝트가 있는 빠르게 변화하는 장면에 적용하면 해당 관계를 찾는 데 어려움이 있어 성능이 저하됩니다. 또한 계산 집약적입니다. LSTM과 같은 순환신경망은 시간적 정보를 캡처하는 데 사용되어 왔지만, 다양한 시나리오에서 대량의 심도 데이터 공급에 의존하기 때문에 성능이 떨어집니다. 일부 연구자들은 신경망의 장점과 멀티뷰 제약 조건을 결합하여 깊이 추정 네트워크를 개선했으며, 다른 연구자들은 카메라 포즈와 깊이 정렬을 추정하여 구조로부터의 움직임의 한계를 극복했습니다.

시간적 일관성:
단일 프레임 기반 방법을 비디오 클립에 적용하면 시각적 깜박임 또는 불안정성이 발생합니다. 이는 동일한 픽셀의 깊이 값이 여러 프레임에 걸쳐 안정적이지 않아 시각적 일관성이 크게 떨어지기 때문입니다. 비디오 간 합성, 비디오 향상, 스타일 전송, 의미적 분할 등에서 시간적 일관성을 확보하기 위해 다양한 연구가 시도되었습니다. 그러나 현재 비디오 깊이 추정 영역에서 시간적 일관성에 대한 명확한 정의는 없습니다. 이 논문에서 저자는 광학적 흐름에 의존하고 2D 지각과 밀접한 관련이 있는 새로운 일관성 지표를 제안합니다. 또한 비디오 깊이 추정에서 시간적 일관성을 강화하는 실용적인 방법을 제안합니다.

### 3. Our Definition and Method

깊이 추정을 위한 시간적 일관성 메트릭을 소개하고 모델 학습 중에 이를 적용하는 방법을 간략하게 설명합니다.

시간적 일관성 메트릭:
저자는 물체의 움직임, 카메라 이동, 같은 영역에서의 잦은 드리프트 등으로 인한 변화를 고려하여 순차적인 깊이 결과의 안정성을 측정하는 메트릭을 제안합니다. 이 지표는 광학적 흐름을 사용하여 프레임 사이에 해당하는 픽셀을 식별하고 비디오 전체에서 어떻게 변동하는지 측정합니다. 비디오의 시간적 일관성은 전체 시퀀스에 대한 편차가 허용되지 않는 평균 비율로 계산됩니다.

![깊이 추정에 시간적 일관성을 부과하는 파이프라인. 우리는 네트워크를 사용하여 각 프레임에 독립적으로 깊이 맵을 직접 예측합니다. 또한, 우리는 연속적인 두 프레임 사이의 광학 플로우를 계산하고 플로우 벡터를 사용하여 한 프레임을 다른 프레임과 정렬하도록 왜곡합니다. 이 정렬과 가림 처리 후, 우리는 우리의 훈련에서 두 사이의 깊이 차이 손실을 최소화하려고 노력합니다. 이렇게 하면 깊이 추정 네트워크에서 시간적 일관성을 강제할 수 있습니다. 추론 시간 동안, 우리의 깊이 추정 네트워크는 프레임별로 깊이 맵을 직접 추정할 수 있어 계산적으로 효율적입니다.](Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17/Untitled%201.png)

깊이 추정에 시간적 일관성을 부과하는 파이프라인. 우리는 네트워크를 사용하여 각 프레임에 독립적으로 깊이 맵을 직접 예측합니다. 또한, 우리는 연속적인 두 프레임 사이의 광학 플로우를 계산하고 플로우 벡터를 사용하여 한 프레임을 다른 프레임과 정렬하도록 왜곡합니다. 이 정렬과 가림 처리 후, 우리는 우리의 훈련에서 두 사이의 깊이 차이 손실을 최소화하려고 노력합니다. 이렇게 하면 깊이 추정 네트워크에서 시간적 일관성을 강제할 수 있습니다. 추론 시간 동안, 우리의 깊이 추정 네트워크는 프레임별로 깊이 맵을 직접 추정할 수 있어 계산적으로 효율적입니다.

비디오 깊이 추정을 위한 시간적 일관성 적용:
저자들은 단일 이미지 깊이 추정에 시간적 안정성을 추가하는 훈련 파이프라인을 제안합니다. 이 파이프라인은 두 개의 연속된 프레임을 깊이 네트워크에 입력으로 사용하여 해당 깊이 예측을 생성합니다. 깊이 손실은 이러한 예측과 지상 실측 깊이 사이의 거리로 계산됩니다.

이러한 기존의 수심 예측 손실에 더해, 저자들은 연속된 두 프레임 사이의 큰 수심 변화에 불이익을 주는 시간적 일관성 손실을 제안합니다. 두 프레임 사이의 광학적 흐름을 추정하여 차이를 측정하기 전에 수심 예측을 정렬합니다. 수심 추정 모델을 훈련하기 위한 전체 손실 함수는 수심 손실과 시간적 일관성 손실을 결합합니다.

저자는 대규모의 다양한 비디오 심도 데이터 세트를 캡처하는 데 따르는 어려움을 해결하기 위해 최첨단 단안 심도 방법에서 추출한 슈퍼비전을 사용할 것을 제안합니다. 특히 일반화 능력이 뛰어나고 깊이 추정을 위한 슈퍼비전 신호를 제공할 수 있는 MiDAS 네트워크를 교사로 사용합니다.

### 4. Experiments

이 섹션에서는 비디오 깊이 추정에서 시간적 안정성을 강화하기 위한 접근 방식을 테스트하기 위해 일련의 실험을 수행합니다.

다양한 실내 장면을 포함하는 464개의 비디오 시퀀스로 구성된 NYU Depth v2 데이터 세트가 테스트에 사용됩니다. 데이터 세트는 630개의 비디오 클립을 생성하도록 처리되어 테스트 세트에 사용됩니다.

![NYU 검증 세트에서 다른 최첨단 방법들과의 시각적 비교. 첫 번째 행: NYU에서 입력 프레임. 두 번째 행: CLSTM을 사용하여 생성된 깊이 추정 맵. 세 번째 행: DeepV2D를 사용하여 생성된 깊이 추정 맵. 마지막 행: 우리의 방법을 사용하여 생성된 깊이 추정 맵. 비디오 안정성을 더 잘 시각화하기 위해, 마지막 열에서는 다른 프레임과 깊이 추정에서 동일한 영역을 확대하고 연결했습니다.](Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17/Untitled%202.png)

NYU 검증 세트에서 다른 최첨단 방법들과의 시각적 비교. 첫 번째 행: NYU에서 입력 프레임. 두 번째 행: CLSTM을 사용하여 생성된 깊이 추정 맵. 세 번째 행: DeepV2D를 사용하여 생성된 깊이 추정 맵. 마지막 행: 우리의 방법을 사용하여 생성된 깊이 추정 맵. 비디오 안정성을 더 잘 시각화하기 위해, 마지막 열에서는 다른 프레임과 깊이 추정에서 동일한 영역을 확대하고 연결했습니다.

![NYU 검증 세트에서 다른 최첨단 방법들과의 시각적 비교. 첫 번째 행: NYU에서 입력 프레임. 두 번째 행: BTS를 사용하여 생성된 깊이 추정 맵. 세 번째 행: CLSTM을 사용하여 생성된 깊이 추정 맵. 마지막 행: 우리의 방법을 사용하여 생성된 깊이 추정 맵. 비디오 안정성을 더 잘 시각화하기 위해, 마지막 열에서는 다른 프레임과 깊이 추정에서 동일한 영역을 확대하고 연결했습니다.](Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17/Untitled%203.png)

NYU 검증 세트에서 다른 최첨단 방법들과의 시각적 비교. 첫 번째 행: NYU에서 입력 프레임. 두 번째 행: BTS를 사용하여 생성된 깊이 추정 맵. 세 번째 행: CLSTM을 사용하여 생성된 깊이 추정 맵. 마지막 행: 우리의 방법을 사용하여 생성된 깊이 추정 맵. 비디오 안정성을 더 잘 시각화하기 위해, 마지막 열에서는 다른 프레임과 깊이 추정에서 동일한 영역을 확대하고 연결했습니다.

저자의 방법은 다른 학습 기반 방법에 비해 속도와 시간적 일관성 지표에서 우수한 성능을 보여줍니다. 또한 이 방법은 시각적 비교 예시에서 깜박임이 적고 시간적 일관성이 더 우수합니다.

![비디오 깊이 추정의 시각적 비교와 그림의 줄무늬는 연속된 프레임에서 자른 것입니다. 최첨단 단일 이미지 기반 방법인 MiDaS [32]는 프레임별로 고품질의 깊이 맵을 생성할 수 있지만 시간이 지남에 따라 눈에 띄는 깜박임이 있습니다. 시간적 일관성을 깊이 추정 모델에 부여한 후, 우리의 방법은 시간적으로 더 안정적인 깊이 예측을 할 수 있습니다.](Enforcing%20Temporal%20Consistency%20in%20Video%20Depth%20Esti%20326766c09fa949c6acfced2e17cc7f17/Untitled%204.png)

비디오 깊이 추정의 시각적 비교와 그림의 줄무늬는 연속된 프레임에서 자른 것입니다. 최첨단 단일 이미지 기반 방법인 MiDaS [32]는 프레임별로 고품질의 깊이 맵을 생성할 수 있지만 시간이 지남에 따라 눈에 띄는 깜박임이 있습니다. 시간적 일관성을 깊이 추정 모델에 부여한 후, 우리의 방법은 시간적으로 더 안정적인 깊이 예측을 할 수 있습니다.

다음으로 저자들은 보다 다양한 실외 장면이 포함된 DAVIS 및 Videvo 데이터 세트에서 실험을 수행하여 접근 방식의 일반화 가능성을 테스트합니다. 지상 실측 데이터가 없는 경우, MiDAS 모델의 출력을 감독으로 사용하여 증류 방식으로 모델을 훈련합니다.

생성된 뎁스 비디오의 시각적 안정성을 비교하기 위해 20명의 참가자를 대상으로 사용자 연구를 실시했습니다. 이 연구에서 참가자들은 DAVIS 테스트 세트의 각 시퀀스에 대해 가장 안정적인 심도 비디오를 선택해야 했습니다. 그 결과 저자들의 방법이 시간적 일관성에서 다른 방법보다 월등히 뛰어났으며, 참가자들이 가장 안정적이라고 보편적으로 선택한 것으로 나타났습니다.

마지막으로 저자들은 사용자 연구를 통해 제안한 시간적 일관성 메트릭의 유효성을 검증했습니다. 연구진은 동일한 비디오 시퀀스에서 각각 다른 모델을 사용하여 생성된 20쌍의 심도 비디오를 준비했습니다. 참가자들은 각 쌍에서 더 안정적인 심도 비디오를 선택하도록 요청받았습니다. 그 결과, 제안된 지표가 시각적 안정성에 대한 인간의 인식을 충분히 나타내며 지표 점수와 참가자의 선택 간에 높은 상관관계가 있는 것으로 나타났습니다.

### 5. Conclusion

결론적으로, 저자는 단일 프레임 추론에서 비디오 깊이 추정의 시간적 일관성을 향상시키는 간단하지만 강력한 방법을 제시합니다. 저자들은 비디오 안정성에 대한 인간의 인식과 일치하는 시간적 일관성 지표를 제안합니다. 저자들은 다양한 실험을 통해 이 방법이 보다 안정적인 깊이 추정을 제공할 수 있으며, 해당 깊이 지상 실측 자료 없이도 동적인 실제 비디오에 적용할 수 있음을 입증했습니다. 이 연구는 다양한 애플리케이션에서 비디오 깊이 추정 기법의 품질과 신뢰성을 향상시킬 수 있는 유망한 잠재력을 가지고 있습니다.