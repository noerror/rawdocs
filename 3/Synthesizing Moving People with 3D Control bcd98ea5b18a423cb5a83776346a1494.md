# Synthesizing Moving People with 3D Control

[https://arxiv.org/abs/2401.10889](https://arxiv.org/abs/2401.10889)

- Jan 2024

### 1. Introduction

인간의 임의의 사진을 기반으로 해당 인물이 다른 사람의 행동을 모방할 수 있는지 여부에 초점을 맞추고 있습니다. 이러한 문제 해결을 위해서는 인간의 자세가 시간에 따라 어떻게 변화하는지에 대한 깊은 이해가 필요하며, 인간의 외모와 의상에 관한 지식을 학습하는 것이 필수적입니다. 예를 들어, 이 논문에서 제시된 그림 1에서 볼 수 있듯이, 한 배우가 걷기, 달리기와 같은 간단한 행동부터 싸우기, 춤추기와 같은 복잡한 행동에 이르기까지 다양한 동작을 수행할 수 있음을 설명합니다.

![모방 게임: '배우'라는 사람의 동영상이 주어졌을 때, 그 사람의 동작을 새로운 사람 '모방자'에게 전달하고자 합니다. 이 그림에서 첫 번째 행은 배우(미셸 콴)가 98년 올림픽 연기를 하는 일련의 프레임을 보여줍니다. 삽입된 행 은 이 비디오에서 추출한 3D 포즈를 보여줍니다. 이제 모방자라는 새로운 인물의 단일 이미지가 주어지면, 우리 모델은 모방자의 새로운 이미지를 새로운 렌더링을 합성하여 배우의 동작을 3D로 복사할 수 있습니다.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled.png)

모방 게임: '배우'라는 사람의 동영상이 주어졌을 때, 그 사람의 동작을 새로운 사람 '모방자'에게 전달하고자 합니다. 이 그림에서 첫 번째 행은 배우(미셸 콴)가 98년 올림픽 연기를 하는 일련의 프레임을 보여줍니다. 삽입된 행 은 이 비디오에서 추출한 3D 포즈를 보여줍니다. 이제 모방자라는 새로운 인물의 단일 이미지가 주어지면, 우리 모델은 모방자의 새로운 이미지를 새로운 렌더링을 합성하여 배우의 동작을 3D로 복사할 수 있습니다.

이러한 문제에 접근하기 위해, 저자들은 3DHM이라는 두 단계 프레임워크를 제안합니다. 이 프레임워크는 단일 이미지에서 텍스처 맵을 완성하고, 이를 바탕으로 3D 인간을 렌더링하여 다른 사람의 행동을 모방하게 합니다. 이 연구에서는 상태 최고의 3D 인간 자세 복구 모델인 '4DHumans'를 사용하여 배우의 동작 신호를 추출하고 시간에 따라 추적하는 방법을 사용합니다. 이는 한 번에 한 장의 이미지로부터 모방자의 전체 텍스처 맵을 생성하는 데 필요한 기술적인 도전을 극복하는 데 중점을 두고 있습니다.

따라서 이 서론 부분은 인간의 동작을 모방하는 데 있어서 중요한 기술적 도전과 이를 해결하기 위한 새로운 방법론의 필요성을 강조하고 있습니다. 이러한 접근 방식은 인간의 복잡한 동작과 관련된 디지털 이미지를 생성하고 처리하는 분야에서 중요한 진전을 나타내고 있습니다.

### 2. Related Works

인간 생성과 움직이는 사람들을 합성하는 데 있어서의 주요 연구 동향과 이 분야에서의 기술적 어려움들을 다룹니다.

첫 번째로, 인간 생성에 관한 연구는 인간의 3D 구조를 이해하는 것이 중요하다고 강조합니다. 인간 생성은 단순한 이미지 변환보다 훨씬 복잡한 과제로, 주어진 텍스트 프롬프트나 포즈 조건을 기반으로 하는 기존의 생성 모델들이 종종 비합리적인 인간 이미지나 비디오를 생성하는 경향이 있습니다. 이는 인간의 자세와 움직임을 제대로 이해하고 재현하는 것이 어렵다는 것을 의미합니다. 이러한 문제를 해결하기 위해, 일부 연구에서는 인간의 몸 구조에 대한 사전 지식을 생성 과정에 통합함으로써 생성된 이미지의 질을 향상시키려고 시도합니다.

두 번째로, 움직이는 사람들을 합성하는 것은 더욱 도전적인 과제입니다. 기존의 방법들은 주어진 지시에 따라 비디오를 합성할 수 있지만, 이러한 방법들은 종종 인간의 특성을 정확하게 포착하지 못하고 비현실적인 인간 형상을 생성할 위험이 있습니다. 예를 들어, Make-a-Video나 Imagen Video 같은 기술은 지시에 따라 비디오를 합성할 수 있지만, 인간의 속성을 정확히 캡쳐하는 데는 한계가 있습니다. 이에 반해, 일부 연구는 포즈-투-픽셀 매핑을 직접 학습하는 방식을 채택하지만, 이 방법은 일반적으로 단일 인물에 대해서만 훈련되고 사용될 수 있습니다.

따라서 이 섹션은 인간 생성과 합성 분야에서의 기술적인 어려움과 현재의 연구 동향을 요약하며, 이러한 문제들을 해결하기 위한 새로운 접근법의 필요성을 강조합니다. 이는 논문의 나머지 부분에서 제안된 3DHM 프레임워크의 중요성과 혁신성을 부각시키는 데 기여합니다.

### 3. Synthesizing Moving People

'3DHM' 프레임워크를 통한 이동하는 사람들의 합성 방법에 대해 설명합니다. 이 챕터는 두 주요 단계, 즉 질감 맵 인페인팅과 인간 렌더링에 초점을 맞춥니다.

![3DHM 개요: 모델 파이프라인의 개요를 보여줍니다. 모방자의 이미지와 배우의 3D 포즈 시퀀스가 주어지면 모방자의 이미지와 3D 포즈 시퀀스가 주어지면 먼저 모방자의 전체 텍스처 맵을 생성하고, 이를 액터에서 추출한 3D 포즈 시퀀스에 적용할 수 있습니다. 모방자의 텍스처 매핑된 중간 렌더링을 생성합니다. 그런 다음 이 중간 렌더링을 2단계 모델로 전달하여 모델에 전달하여 SMPL 메시 렌더링을 실제 이미지의 보다 사실적인 렌더링으로 투영합니다. 참고: 빨간색 상자는 입력을 나타내고, 노란색 상자는 는 1단계의 중간 예측, 파란색 상자는 2단계의 최종 출력을 나타냅니다. 움직이는 사람 애니메이션을 만들려면 을 만들려면 1단계를 한 번만 실행하면 완전한 텍스처 맵을 얻을 수 있습니다.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled%201.png)

3DHM 개요: 모델 파이프라인의 개요를 보여줍니다. 모방자의 이미지와 배우의 3D 포즈 시퀀스가 주어지면 모방자의 이미지와 3D 포즈 시퀀스가 주어지면 먼저 모방자의 전체 텍스처 맵을 생성하고, 이를 액터에서 추출한 3D 포즈 시퀀스에 적용할 수 있습니다. 모방자의 텍스처 매핑된 중간 렌더링을 생성합니다. 그런 다음 이 중간 렌더링을 2단계 모델로 전달하여 모델에 전달하여 SMPL 메시 렌더링을 실제 이미지의 보다 사실적인 렌더링으로 투영합니다. 참고: 빨간색 상자는 입력을 나타내고, 노란색 상자는 는 1단계의 중간 예측, 파란색 상자는 2단계의 최종 출력을 나타냅니다. 움직이는 사람 애니메이션을 만들려면 을 만들려면 1단계를 한 번만 실행하면 완전한 텍스처 맵을 얻을 수 있습니다.

1. **질감 맵 인페인팅(Texture Map Inpainting)**: 이 단계의 목적은 모방자의 보이지 않는 부분을 채워 완전한 질감 맵을 생성하는 것입니다. 연구팀은 먼저 3D 메시를 입력 이미지에 렌더링하여 부분적으로 보이는 질감 맵을 추출합니다. 이는 '4DHumans'를 사용하여 각 보이는 삼각형에 대해 색상을 샘플링함으로써 수행됩니다. 이렇게 얻은 부분적 질감 맵은 확산 모델을 통해 완성되어 보이지 않는 영역을 채웁니다. 이 과정은 '실제 인간'이라는 고정된 텍스트 입력을 사용하는 Stable Diffusion Inpainting 모델에 기반합니다.
    
    ![3DHM의 1단계: 첫 번째 단계에서는 모방자의 단일 뷰 이미지가 주어지면 먼저 4Dhumans [9] 스타일의 샘플링을 적용하여 방식을 적용하여 부분 텍스처 맵과 그에 해당하는 가시성 맵을 추출합니다. 이 두 가지 입력은 페인팅 내 디퓨전 모델에 전달하여 그럴듯한 전체 텍스처 맵을 생성합니다. 이 예시에서는 모방자의 뒷모습만 보이지만, 모델은 모델은 모방자의 옷과 일치하는 그럴듯한 전면 영역을 일관된 전면 영역을 만들어낼 수 있었습니다.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled%202.png)
    
    3DHM의 1단계: 첫 번째 단계에서는 모방자의 단일 뷰 이미지가 주어지면 먼저 4Dhumans [9] 스타일의 샘플링을 적용하여 방식을 적용하여 부분 텍스처 맵과 그에 해당하는 가시성 맵을 추출합니다. 이 두 가지 입력은 페인팅 내 디퓨전 모델에 전달하여 그럴듯한 전체 텍스처 맵을 생성합니다. 이 예시에서는 모방자의 뒷모습만 보이지만, 모델은 모델은 모방자의 옷과 일치하는 그럴듯한 전면 영역을 일관된 전면 영역을 만들어낼 수 있었습니다.
    
2. **인간 렌더링(Human Rendering)**: 이 단계에서는 배우의 동작을 모방하는 현실적인 인간 모방자의 렌더링을 목표로 합니다. 중간 렌더링은 배우의 포즈와 단계 1의 질감 맵을 사용하여 생성됩니다. 하지만 이 렌더링은 몸에 밀착된 옷만을 반영하며, 실제 의상의 변형이나 다양한 헤어스타일은 포함하지 않습니다. 연구팀은 이를 해결하기 위해, 실제 RGB 이미지와 중간 렌더링을 페어링하여 스테이지 2 확산 모델을 훈련시킵니다. 이 모델은 Stable Diffusion 모델의 인코더 가중치를 복제하고, 3D 조건에 따라 제어 가능한 분기를 처리합니다.
    
    ![3DHM의 2단계: 이 그림은 1단계 접근 방식의 추론을 보여줍니다. 1단계 접근 방식을 보여줍니다. 배우의 포즈가 있는 모방자의 중간 렌더링이 주어지면 의 중간 렌더링과 배우의 포즈 및 모방자의 실제 RGB 이미지가 주어집니다, 우리 모델은 배우의 포즈에 따라 모방자의 사실적인 렌더링을 합성할 수 있습니다. 모방자의 사실적인 렌더링을 합성할 수 있습니다.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled%203.png)
    
    3DHM의 2단계: 이 그림은 1단계 접근 방식의 추론을 보여줍니다. 1단계 접근 방식을 보여줍니다. 배우의 포즈가 있는 모방자의 중간 렌더링이 주어지면 의 중간 렌더링과 배우의 포즈 및 모방자의 실제 RGB 이미지가 주어집니다, 우리 모델은 배우의 포즈에 따라 모방자의 사실적인 렌더링을 합성할 수 있습니다. 모방자의 사실적인 렌더링을 합성할 수 있습니다.
    

이 챕터는 이 두 단계를 통해 배우의 동작을 모방하는 현실적인 인간 렌더링을 생성하는 3DHM 프레임워크의 세부 사항을 제공합니다. 질감 맵 인페인팅은 모방자의 완전한 질감 맵을 생성하는 데 중점을 두며, 인간 렌더링 단계는 이 질감 맵을 활용하여 현실적인 외모와 의상을 갖춘 인간 모방자를 생성합니다. 이러한 접근 방식은 인간의 동작과 외모를 디지털 환경에서 더 정확하게 재현하고 모방하는 데 중요한 기술적 발전을 나타냅니다.

### 4. Experiments

실험을 위해 연구팀은 2,524개의 3D 인간 비디오를 포함하는 데이터셋을 구축합니다. 이 데이터셋은 2K2K, THuman2.0, People-Snapshot 등 다양한 소스에서 수집된 3D 인간 모델을 기반으로 합니다. 4DHumans 모델을 사용하여 이들 비디오에서 3D 자세를 추출하고, 이를 훈련, 검증, 테스트에 활용합니다. 데이터셋에 대한 보다 자세한 정보는 논문의 부록에서 제공됩니다.

평가 메트릭으로는 이미지 기반 메트릭(PSNR, SSIM, FID, LPIPS, L1)과 비디오 기반 메트릭(FVD)을 사용합니다. 또한, 3D 자세 정확성을 평가하기 위해 MPVPE와 PA-MPVPE 메트릭을 활용합니다.

구현과 관련하여, 연구팀은 모든 데이터셋에 대해 일정한 학습률을 설정하고, 스테이지 1과 스테이지 2의 두 단계에 걸쳐 사전 훈련된 확산 모델을 사용합니다. 스테이지 1의 Inpainting Diffusion과 스테이지 2의 Rendering Diffusion 모델은 각각 NVIDIA A100 GPU 8개를 사용하여 수 주간 훈련됩니다.

실험 결과에서는 3DHM의 성능을 다양한 기준으로 평가하고, 기존의 방법들과 비교합니다. 이를 통해 연구팀은 3DHM이 프레임별 생성 품질과 비디오 수준의 생성 품질 모두에서 우수한 성능을 보임을 입증합니다. 또한, 연구팀은 3DHM을 다양한 시나리오에서 테스트하여 모델이 인간 모션 비디오를 어떻게 효과적으로 생성하는지 보여줍니다. 이러한 실험들은 3DHM 프레임워크가 인간의 동작과 외모를 정확하게 재현하고 모방하는 데 있어 중요한 기술적 발전을 나타낸다는 것을 증명합니다.

![동일한 포즈의 다양한 시점에 대한 정성적 결과, 무작위 비디오의 모션과 텍스트 입력의 모션.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled%204.png)

동일한 포즈의 다양한 시점에 대한 정성적 결과, 무작위 비디오의 모션과 텍스트 입력의 모션.

![무작위 실제 사람 사진(한국 여배우)에 대한 다른 2D 제어 방식과의 정성적 비교. 다양한 3D 포즈 또는 동일한 3D 포즈를 다른 시점에 적용했습니다. 2D 포즈는 접는 동작을 포착하지 못할 수도 있고 인체의 디테일. 우리의 접근 방식인 3DHM은 3D 제어를 통해 이 간극을 메울 수 있다는 것을 알 수 있었습니다.](Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494/Untitled%205.png)

무작위 실제 사람 사진(한국 여배우)에 대한 다른 2D 제어 방식과의 정성적 비교. 다양한 3D 포즈 또는 동일한 3D 포즈를 다른 시점에 적용했습니다. 2D 포즈는 접는 동작을 포착하지 못할 수도 있고 인체의 디테일. 우리의 접근 방식인 3DHM은 3D 제어를 통해 이 간극을 메울 수 있다는 것을 알 수 있었습니다.

### 5. Analysis and Discussion

연구팀은 여러 구성 요소가 모델 성능에 미치는 영향을 평가하기 위해 제거 연구(ablation study)를 수행합니다. 이 연구는 질감 맵 재구성과 외관 레이턴트가 모델 성능에 결정적인 역할을 한다는 것을 발견합니다. 또한, SMPL 파라미터를 모델에 직접 추가하는 것이 모든 평가 지표에 대해 성능을 향상시키지 않는 것으로 나타났습니다. 이는 SMPL 파라미터의 부정확성이 확산 훈련 과정에서 모순된 정보를 제공할 수 있기 때문입니다.

연구팀은 3DHM과 기존의 DreamPose 및 DisCO 모델의 결과를 비교하여 3D 제어가 모델의 일반화 능력에 어떻게 영향을 미치는지 분석합니다. 3DHM은 3D 제어를 통해 외관을 포즈에 더 잘 연결하고 신체 형태를 보존함으로써, 제한된 3D 인간 데이터에 대해서만 훈련된 경우에도 실제 인간 사진에 대한 일반화 성능이 우수함을 보여줍니다. 반면에 DreamPose는 주제별로 특화된 훈련이 필요하고, DisCO는 여러 공개 데이터셋을 사용하여 인간 속성에 대한 사전 훈련을 거치지만, 목표 포즈 없이 사람을 합성하는 데는 실패합니다.

연구팀은 3DHM이 독립적으로 인간 모션 비디오의 프레임을 생성하기 때문에 시간적 일관성을 보장할 수 없다는 점을 지적합니다. 예를 들어, 연속 프레임 간의 의상의 광원이 변할 수 있습니다. 이를 해결하기 위한 가능한 방법으로는 여러 프레임을 동시에 예측하는 모델 훈련이나, 이전에 생성된 프레임에 대한 확률적 조건부 생성을 제안합니다. 또한, 2,000명의 인간 데이터로 훈련된 3DHM은 모든 세부적인 질감을 완전히 재구성할 수 없으며, 더 많은 인간 데이터로 훈련함으로써 이러한 문제를 완화할 수 있을 것으로 기대합니다.

### 6. Conclusion

연구팀은 3DHM이란 두 단계 확산 모델 기반 프레임워크를 통해 임의의 사진과 목표 인간 포즈를 바탕으로 움직이는 사람을 합성하는 새로운 방법을 제안했습니다. 이 프레임워크의 핵심은 최첨단 3D 포즈 예측 모델을 사용하여 인간 모션 데이터를 생성하고, 임의의 비디오에 대한 훈련을 가능하게 함으로써, 정확한 그라운드 트루스 라벨이 필요 없다는 점에 있습니다. 이는 장거리 모션 생성과 임의의 포즈 처리에 효과적이며, 기존 접근 방식들보다 우수한 성능을 보입니다.

결론적으로, 이 연구는 디지털 이미지 생성과 처리 분야에서 인간의 동작과 외모를 더 정확하게 재현하고 모방하는 데 중요한 기술적 발전을 나타냅니다. 연구팀은 이 프레임워크가 다양한 응용 분야에 적용될 수 있는 잠재력을 가지고 있으며, 이 분야의 지속적인 발전을 위한 기초를 마련했다고 강조합니다. 이러한 발전은 인간 중심의 디지털 콘텐츠 생성에 있어 새로운 가능성을 열어줄 것으로 기대됩니다.