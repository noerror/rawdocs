# Comparing the Quality of Human Pose Estimation with BlazePose or OpenPose

[https://www.researchgate.net/publication/358017174_Comparing_the_Quality_of_Human_Pose_Estimation_with_BlazePose_or_OpenPose](https://www.researchgate.net/publication/358017174_Comparing_the_Quality_of_Human_Pose_Estimation_with_BlazePose_or_OpenPose)

이 연구는 사진이나 동영상에서 사람의 신체 부위(예: 손, 팔꿈치, 무릎 등)의 위치를 추측하는 데 사용되는 두 가지 기술인 BlazePose와 OpenPose를 비교하는 것입니다. 이를 "사람 포즈 추정"이라고 하며 컴퓨터 비전이라는 분야의 일부입니다.

궁극적인 목표는 의사가 환자의 움직이는 영상을 보고 분석하여 문제나 진행 상황을 확인하는 '가상 동작 평가'에 이러한 기술을 사용하는 것입니다. 예를 들어, 환자가 집에서 특정 운동을 하는 영상을 분석하여 제대로 하고 있는지 확인하는 물리 치료에 이 기술을 사용할 수 있습니다.

블레이즈포즈는 스마트폰에서 사용할 수 있는 최신 모델이며, 오픈포즈는 잘 정립되고 인정받는 기술입니다.

이 연구에서는 의사들이 자주 분석하는 10가지 동작 동영상을 사용했습니다. 블레이즈포즈와 오픈포즈를 모두 사용하여 동영상 속 신체 위치를 추정했습니다. 그런 다음 추정치를 비교하여 얼마나 잘 일치하는지 확인했습니다.

OpenPose는 현장에서 널리 사용되고 있기 때문에 비교의 표준 또는 기준선으로 사용했습니다. 그런 다음 피어슨 상관관계와 제곱평균제곱오차라는 두 가지 수학적 방법을 사용하여 BlazePose의 추정치가 OpenPose의 추정치와 얼마나 잘 일치하는지 측정했습니다.

그 결과, 블레이즈포즈는 특히 무릎이나 팔꿈치와 같은 관절 부위에서 오픈포즈와 다르게 신체 위치를 추측하는 경우가 많다는 사실을 발견했습니다. 이는 블레이즈포즈가 아직 임상용으로 사용하기에는 충분하지 않을 수 있음을 시사합니다. 즉, 의사가 환자의 동영상에서 정확한 신체 위치 정보를 얻을 수 없다는 뜻입니다.

하지만 블레이즈포즈는 오픈포즈보다 훨씬 빠르고 스마트폰에서 잘 작동하기 때문에 스마트폰 앱의 특정 부분에서 동작을 평가하는 데 유용할 수 있습니다. 예를 들어, 환자의 움직임을 빠르게 확인하거나 환자가 어떤 활동을 하고 있는지 파악하는 데 사용할 수 있습니다.

결론적으로, 이 연구는 향후 동작 평가에 비디오를 사용하고자 하는 앱이나 시스템에서는 정확도 높은 OpenPose를 사용하여 신체 위치를 추정해야 한다고 제안합니다. 하지만 블레이즈포즈는 빠른 확인이나 활동 식별과 같은 다른 기능에도 여전히 유용할 수 있습니다.

### 1 Introduction

인공 지능(AI) 시스템은 환자 치료의 특정 부분을 개선할 수 있기 때문에 의료계에서 점점 더 인기를 얻고 있습니다. 코로나19 팬데믹으로 인해 가상 의료 서비스에 대한 수요가 증가하면서 사람들은 일반적으로 병원이나 클리닉에서 수행하던 업무에 AI를 사용하기 시작했습니다. 그 중 하나는 비디오 또는 센서 데이터를 통해 환자의 움직임을 원격으로 분석하는 '가상 움직임 평가'입니다.

기존에는 특수 장비를 갖춘 실험실에서 전문가 팀이 데이터를 수집하고 분석하여 그 결과를 바탕으로 의사 결정을 내리는 방식으로 움직임 분석을 수행했습니다. 하지만 위치, 장비의 가용성, 숙련된 인력의 필요성 때문에 일부 환자에게는 접근하기 어려울 수 있습니다. 가상 동작 평가는 이 프로세스에 대한 접근성을 높일 수 있습니다.

이미지나 동영상에서 사람의 신체 위치를 추정하는 AI 모델은 이러한 가상 평가에 매우 유용할 수 있습니다. 컨볼루션 신경망(CNN)이라는 AI 유형을 사용하는 이러한 모델은 스포츠 훈련, 재활, 수화 통역 등 여러 분야에서 성공적으로 사용되어 왔습니다.

하지만 이러한 모델이 의료 분야에서 유용하게 사용되려면 사람의 위치를 설명하는 데 사용되는 신체의 특정 지점인 '신체 키포인트'를 매우 정확하게 식별할 수 있어야 합니다. 사람의 체형, 포즈, 의복, 조명 조건 및 기타 요인이 매우 다양하기 때문에 이 작업은 매우 어렵습니다.

이 연구에서 연구진은 신체 위치를 추정하는 두 가지 모델을 비교하고자 했습니다: OpenPose와 BlazePose입니다. OpenPose는 널리 사용되어 왔으며 이전 연구에서 상당히 정확한 것으로 나타났습니다. 블레이즈포즈는 구글에서 개발한 최신 모델로, 가볍고 빠르도록 설계되어 스마트폰에서 사용하기에 좋습니다.

블레이즈포즈는 2단계 프로세스를 사용하여 동영상에서 사람의 움직임을 식별하고 추적합니다. 먼저 동영상의 첫 번째 프레임에서 사람을 식별한 다음 다음 프레임에서 사람의 움직임을 추적합니다. 이를 통해 실시간 피드백을 제공할 수 있어 평가 중에 환자의 움직임을 교정하는 데 도움이 될 수 있습니다.

이 연구의 주요 목표는 블레이즈포즈가 가상 동작 평가에 사용할 수 있을 만큼 정확한지, 신체 키포인트를 정확하게 식별하는 측면에서 오픈포즈와 어떻게 비교되는지 확인하는 것이었습니다.

### 2 Methods

A. 비디오 조달

이 연구를 위해 10개의 동영상을 수집했습니다. 이 중 절반은 어깨와 팔꿈치 등 상체의 움직임을 보여주었습니다. 나머지 절반은 하체의 움직임을 보여주기 위해 사람이 걷는 모습을 촬영했습니다. 이러한 동작은 의사가 사람의 전반적인 신체 움직임을 이해하기 위해 검사하는 경우가 많기 때문에 선택되었습니다. 이 동영상은 iPhone XS 및 XR로 촬영한 다음 AI 모델이 분석할 수 있는 형식으로 처리되었습니다.

B. 데이터 수집 및 처리

그런 다음 비디오는 비교 대상인 두 AI 모델인 블레이즈포즈와 오픈포즈에 입력되었습니다. 블레이즈포즈는 3D로 33개의 신체 키포인트(깊이 포함)와 함께 각 키포인트가 비디오 프레임에 포함될 확률에 대한 점수를 제공했습니다. 반면 OpenPose는 2D로 25개의 키포인트와 각 키포인트의 위치를 얼마나 확신할 수 있는지에 대한 점수를 제공했습니다.

![블레이즈포즈(왼쪽)와 오픈포즈(오른쪽)의 2차원 스켈레톤 키포인트 토폴로지. 블레이즈포즈는 키포인트가 25개인 오픈포즈에 비해 33개의 키포인트를 포함합니다. 블레이즈포즈에는 손과 얼굴에 더 많은 키포인트가 포함되어 있습니다.](Comparing%20the%20Quality%20of%20Human%20Pose%20Estimation%20wit%20dfe903d4931643269046f986477a8ad3/Untitled.png)

블레이즈포즈(왼쪽)와 오픈포즈(오른쪽)의 2차원 스켈레톤 키포인트 토폴로지. 블레이즈포즈는 키포인트가 25개인 오픈포즈에 비해 33개의 키포인트를 포함합니다. 블레이즈포즈에는 손과 얼굴에 더 많은 키포인트가 포함되어 있습니다.

두 모델 모두 식별할 수 있는 17개 키포인트의 2D 좌표만 분석에 고려했습니다.

연구원들은 OpenPose 데이터를 처리할 때 모델이 키포인트의 위치에 대해 10% 미만의 확신을 가지고 있는 부분을 채웠습니다. 그런 다음 더 나은 성능을 얻기 위해 데이터를 평활화했습니다. OpenPose가 키포인트를 식별할 수 없는 경우(일반적으로 신체 일부가 시야에서 가려져 있기 때문에) -1 값을 반환했습니다.

블레이즈포즈 역시 동일한 데이터 처리 방법을 사용했습니다. 그러나 신체 일부가 시야에서 가려진 경우에도 BlazePose는 이전 프레임을 기반으로 해당 위치를 추측하려고 했습니다. 연구진은 두 모델 모두 모든 키포인트를 식별한 프레임만 분석에 고려했습니다.

C. 데이터 분석

연구원들은 두 모델의 키포인트를 비디오에 오버레이하여 각 모델이 얼마나 잘 수행했는지 확인했습니다. 그런 다음 키포인트가 비디오의 실제 신체 부위와 얼마나 잘 일치하는지에 따라 4점 척도를 사용하여 OpenPose 비디오의 각 프레임에 점수를 매겼습니다.

그런 다음 이 점수를 사용하여 BlazePose가 키포인트를 얼마나 잘 추정했는지 비교했습니다. 피어슨 상관관계라는 수학적 방법을 사용하여 두 모델의 2D 좌표가 얼마나 밀접하게 일치하는지 확인했습니다. 상관관계가 +/-0.8 이상이면 상관관계가 높은 것으로 간주하고 더 이상 분석하지 않았습니다.

상관관계가 이 임계값보다 낮은 키포인트의 경우, 궤적을 플로팅하여 모델 간의 차이를 시각적으로 파악할 수 있도록 했습니다. 또한 두 키포인트 세트 간의 평균 차이를 측정하는 평균제곱근오차(RMSE)를 계산하고 이를 비디오 프레임의 각 축을 따라 픽셀의 백분율로 변환했습니다. 이를 통해 두 모델의 추정치 간의 차이를 정량화할 수 있었습니다.

### 3 Result

연구진은 이 연구를 위해 10개의 동영상을 사용했습니다. 이 중 5개의 동영상은 상체의 움직임을 보여주었고, 나머지 5개의 동영상은 하체의 움직임을 보여주기 위해 사람이 걷는 모습을 보여주었습니다. 이러한 동작은 사람의 전반적인 신체 움직임을 이해하는 데 중요하기 때문에 선택되었습니다.

영상에 표시된 동작과 관련된 키포인트에만 초점을 맞췄습니다. 상체 동영상에서는 사람이 가만히 서 있기 때문에 엉덩이 아래의 키포인트는 무시했습니다. 하체 동영상에서는 상체와 하체 모두 걷는 동작에 관여하기 때문에 목 위의 키포인트를 제외한 모든 키포인트를 고려했습니다.

두 모델 간에 총 200개의 키포인트 좌표를 비교했습니다. 이 중 26개 키포인트의 상관관계가 임계값 +/-0.8 미만이었으며, 이는 두 모델이 키포인트의 위치를 추정하는 방식에 상당한 차이가 있음을 나타냅니다. 여기에는 하체 동영상에서 15개의 키포인트와 상체 동영상에서 7개의 키포인트가 포함되었습니다.

블레이즈포즈의 키포인트는 평균적으로 오픈포즈의 키포인트와 0.88의 상관관계를 가졌습니다. 상관관계가 임계값보다 낮은 키포인트 간의 평균 차이(또는 오차)는 약 4.68%였습니다.

비디오의 키포인트를 육안으로 살펴본 결과, 블레이즈포즈의 키포인트가 실제 관절 위치와 항상 일치하는 것은 아니라는 사실을 발견했습니다. 이는 사람이 움직이거나, 비디오 배경의 대비가 높거나, 프레임에 다른 물체가 있을 때 발생할 가능성이 높았으며, 이로 인해 AI 모델이 키포인트를 정확하게 추정하기가 더 어려웠습니다.

![블레이즈포즈(왼쪽) 모델이 잘못 예측한 예시 키포인트 좌표계를 잘못 예측한 예. OpenPose(오른쪽)와 비교. 비디오 10 (A)에서 발 키포인트는 각 관절 중심에서 멀리 떨어져 있습니다. 비디오 6(B)에서 모델은 환경의 오브젝트를 신체 랜드마크에 대한 것으로 잘못 해석합니다.](Comparing%20the%20Quality%20of%20Human%20Pose%20Estimation%20wit%20dfe903d4931643269046f986477a8ad3/Untitled%201.png)

블레이즈포즈(왼쪽) 모델이 잘못 예측한 예시 키포인트 좌표계를 잘못 예측한 예. OpenPose(오른쪽)와 비교. 비디오 10 (A)에서 발 키포인트는 각 관절 중심에서 멀리 떨어져 있습니다. 비디오 6(B)에서 모델은 환경의 오브젝트를 신체 랜드마크에 대한 것으로 잘못 해석합니다.

### 4 Discussion

이 글에서는 사람의 포즈 추정을 위한 OpenPose와 BlazePose 모델의 차이점, 특히 임상적으로 관련된 움직임을 분석할 때 두 모델의 성능에 대해 설명합니다.

이 연구에 따르면 OpenPose가 움직임을 추적하는 데 사용되는 신체의 특정 지점인 키포인트를 더 정확하게 예측하는 것으로 나타났습니다. 정확한 키포인트 예측은 임상 환경에서 매우 중요한데, 실수로 인해 환자의 움직임에 대한 잘못된 평가가 이루어질 수 있기 때문입니다. 블레이즈포즈는 비디오를 더 빠르게 실시간으로 처리할 수 있지만, 환경의 물체를 키포인트로 잘못 식별하여 예측의 정확도가 떨어지는 경우가 있었습니다.

연구진은 키포인트가 계속 바뀌는 하체 움직임의 동영상에서 더 많은 부정확성을 발견했습니다. 하지만 블레이즈포즈는 사람이 가만히 서 있는 동영상에서 더 나은 성능을 보였습니다.

정확도는 낮지만 블레이즈포즈의 속도와 기타 기능은 특정 시나리오에서 여전히 유용할 수 있습니다. 예를 들어, 실시간 처리 기능은 즉각적인 피드백을 제공하여 동영상을 촬영하는 사람이 더 나은 영상을 캡처하여 분석에 활용할 수 있도록 안내할 수 있습니다. 키포인트가 프레임에 포함될 가능성을 나타내는 가시성 점수는 환자에게 모든 키포인트가 잘 보이도록 위치를 변경하라는 메시지를 표시하는 데 사용할 수 있습니다. 또한 블레이즈포즈의 3D 좌표 출력은 환자 움직임의 깊이를 평가하는 데 유용할 수 있습니다.

요약하면, OpenPose가 더 정확하므로 임상 포즈 추정에 더 적합하지만, BlazePose의 속도와 추가 기능은 가상 모션 평가 도구의 다른 부분에서 유용할 수 있습니다. 3D 좌표 출력의 정확도와 이러한 평가에 어떻게 통합할 수 있는지 결정하기 위해서는 향후 연구가 필요합니다.

### 5 Conclusion

OpenPose는 속도가 느리고 더 많은 계산 리소스가 필요하지만, 사람의 포즈 추정을 위한 키포인트를 예측하는 데는 더 정확하다는 연구 결과를 뒷받침하는 결과입니다. 반면 블레이즈포즈는 가볍고 빠르지만 정확도가 임상적으로 사용하기에는 충분하지 않습니다. 이 연구는 정확도는 낮지만 비디오 녹화 중 피사체의 위치를 개선하기 위해 실시간 피드백을 제공하는 등 블레이즈포즈가 여전히 유용한 용도로 사용될 수 있다고 권장합니다.

코로나19 팬데믹으로 인해 환자의 움직임을 원격으로 평가할 수 있는 가상 동작 평가의 필요성이 증폭되고 있습니다. 이러한 평가는 지리적 거리, 인프라 부족 등 기존 모션 실험실에 대한 접근을 제한할 수 있는 장벽을 극복할 수 있습니다. 정확한 AI 기반 포즈 추정 모델을 사용하면 이러한 가상 동작 평가의 효율성과 접근성을 크게 개선하여 임상 환경에서 널리 사용할 수 있습니다.