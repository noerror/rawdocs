# Drag Your GAN: Interactive Point-based Manipulation on the Generative Image Manifold

*사전 학습된 GAN을 사용해 정확하고 인터랙티브하게 조작할 수 있는 새로운 포인트 기반 이미지 편집 기술인 DragGAN을 공개합니다! 디지털 이미지 편집의 판도를 바꾸는 이 기술은 직관적이고 강력한 실시간 변경을 위한 기반을 마련합니다 #AI #ImageEditing #GANs*
[https://vcai.mpi-inf.mpg.de/projects/DragGAN/](https://vcai.mpi-inf.mpg.de/projects/DragGAN/)

[https://github.com/XingangPan/DragGAN](https://github.com/XingangPan/DragGAN)

[https://vcai.mpi-inf.mpg.de/projects/DragGAN/data/paper.pdf](https://vcai.mpi-inf.mpg.de/projects/DragGAN/data/paper.pdf)

이 연구에서는 사실적인 이미지를 생성하는 데 사용되는 AI 모델의 일종인 생성적 적대 신경망(GAN)의 출력을 제어하는 새로운 방법인 DragGAN을 소개합니다. 기존의 GAN 제어 방법은 수동으로 주석을 단 훈련 데이터나 사전 3D 모델이 필요하기 때문에 제한적인 경우가 많았지만, DragGAN은 유연성, 정밀도, 범용성이 뛰어납니다.

![Untitled](Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb/Untitled.png)

DragGAN을 사용하면 이미지의 모든 점을 목표 위치로 '드래그'할 수 있으므로 생성된 오브젝트의 포즈, 모양, 표정, 레이아웃과 같은 측면을 정밀하게 제어할 수 있습니다. 이는 포인트를 목표 위치로 이동하는 피처 기반 모션 감독과 이동 중인 포인트의 위치를 찾기 위해 제너레이터 기능을 사용하는 새로운 포인트 추적 접근 방식이라는 두 가지 주요 구성 요소를 통해 이루어집니다.

DragGAN을 통해 사용자는 이미지를 변형하고 픽셀을 배치할 위치를 정확하게 결정할 수 있습니다. 동물, 자동차, 사람, 풍경 등 다양한 카테고리를 조작하는 데 사용할 수 있습니다. 이러한 조작은 GAN의 생성 이미지 매니폴드 내에서 이루어지기 때문에 가려진 콘텐츠를 환영처럼 보이게 하거나 오브젝트의 강성을 따라 모양을 변형하는 등 까다로운 시나리오에서도 사실적인 결과물을 생성하는 경향이 있습니다.

DragGAN의 결과는 이미지 조작 및 포인트 추적 작업에서 이전 방법보다 유리하다는 것을 보여주는 유망한 결과입니다. 심지어 GAN 반전이라는 프로세스를 통해 실제 이미지를 조작할 수도 있습니다.

1 INTRODUCTION

이 연구는 사실적인 이미지를 생성하는 데 사용되는 AI 모델의 일종인 생성적 적대 신경망(GAN)의 이미지 생성을 제어하기 위한 보다 인터랙티브하고 사용자 친화적인 접근 방식을 탐구합니다. 사용자가 이미지 속 사물의 위치, 모양, 표정, 몸의 포즈 등을 보다 세밀하게 제어할 수 있도록 하는 것이 목표입니다. 이는 소셜 미디어, 영화 사전 시각화, 미디어 편집, 자동차 디자인과 같은 분야에서 특히 유용합니다.

현재 GAN을 제어하는 방법은 일반적으로 이전 3D 모델이나 수동으로 주석이 달린 데이터를 사용하는 지도 학습에 의존하기 때문에 유연성, 정밀도, 일반성이 제한됩니다. 이 연구는 DragGAN이라는 시스템을 통해 이러한 한계를 해결하는 것을 목표로 합니다. 기존 방식과 달리 DragGAN은 사용자가 이미지의 임의의 점을 클릭하고 원하는 위치로 드래그할 수 있어 유연하고 정밀하며 다양한 방식으로 이미지 합성을 제어할 수 있습니다.

DragGAN에는 두 가지 핵심 요소가 있는데, 하나는 타겟을 향한 포인트(핸들 포인트라고 함)의 이동을 감독하는 것이고, 다른 하나는 편집 프로세스의 각 단계에서 핸들 포인트를 추적하는 것입니다. 이 추적 기능을 통해 편집 프로세스 중에 포인트 위치를 지속적으로 조정할 수 있으므로 사용자는 이미지의 최종 모양을 정밀하게 제어할 수 있습니다.

DragGAN은 표준 하드웨어에서 효율적으로 작동하여 실시간 인터랙티브 편집 세션을 가능하게 합니다. 동물, 사람, 자동차, 풍경 등 다양한 데이터 세트에 대해 광범위한 테스트를 거쳤으며, 다양한 시나리오에서 효과적인 것으로 입증되었습니다.

DragGAN의 포인트 기반 조작 방식은 조작 대상의 고유 구조를 존중합니다. 예를 들어, 사자 입 안의 이빨과 같이 가려진 콘텐츠를 인식하고 말 다리의 구부러짐과 같이 물체의 강성을 따라갈 수 있습니다.

마지막으로, DragGAN은 GAN 반전 기술과 결합하여 실제 이미지를 편집할 수도 있습니다. 연구 결과에 따르면 DragGAN은 이전 방법보다 성능이 뛰어나며 이미지 편집 및 조작을 위한 보다 사용자 친화적이고 유연하며 정밀한 도구를 제공합니다.

2 RELATED WORK

이 논문에서는 인터랙티브 콘텐츠 제작을 위한 다양한 생성 모델에 대해 설명하며, 주로 생성적 적대 신경망(GAN)과 확산 모델에 중점을 둡니다.

무조건 GAN은 단순한 임의의 입력을 사실적인 이미지로 변환할 수 있지만 출력의 특정 측면에 대한 제어가 부족합니다. 이 문제를 해결하기 위해 세분화 맵이나 3D 변수와 같은 추가 입력을 받아 생성된 이미지를 더 잘 제어할 수 있는 조건부 GAN이 개발되었습니다. 또한 감독 또는 비감독 방식으로 입력 벡터를 조작하여 무조건 GAN에 제어 기능을 제공하려는 시도도 있었습니다. 그러나 이러한 방법의 대부분은 기하학적 속성에 대한 대략적인 제어만 허용합니다.

제안된 솔루션인 DragGAN을 사용하면 포인트 기반 편집을 통해 공간 속성을 세밀하게 제어할 수 있습니다. 이는 배포를 벗어난 이미지 편집만 가능하고 사실적인 결과를 보장할 수 없는 GANWarping이나 단일 포인트 편집만 지원하고 정밀도가 부족한 UserControllableLT와 같은 이전 모델에서는 볼 수 없었던 수준의 정밀도를 제공합니다.

3D 표현을 생성하는 3D 인식 GAN도 개발되었습니다. 글로벌 포즈나 라이팅을 제어할 수 있지만 DragGAN의 포인트 기반 편집 정도에는 미치지 못합니다.

또 다른 생성 모델인 확산 모델은 무작위로 샘플링된 노이즈의 반복적인 노이즈 제거를 통해 사실적인 이미지를 생성합니다. 고품질의 결과물을 생성할 수 있지만 정밀한 공간 제어가 부족하고 GAN에 비해 속도가 느립니다.

이 논문에서는 제안된 DragGAN 모델에 필수적인 포인트 추적에 대해서도 설명합니다. 비디오에서 포인트를 추적하는 전통적인 방법에는 광학 흐름 추정과 딥러닝 기반 접근 방식(예: RAFT 및 PIP)이 있습니다. 그러나 이 논문은 GAN의 특징 공간이 특징 매칭을 통한 포인트 추적을 가능하게 할 만큼 변별력이 뛰어나며, 이는 최첨단 방법보다 성능이 뛰어나고 보다 효율적인 인터랙티브 편집을 지원한다는 사실을 보여줍니다.

3 METHOD

이 연구에서는 생성적 적대 신경망(GAN)을 위한 대화형 이미지 조작 기법을 소개합니다. 이 기법을 사용하면 사용자가 이미지를 클릭하기만 하면 '핸들 포인트'와 '타깃 포인트' 쌍을 정의할 수 있습니다. 목표는 핸들 포인트를 해당 타깃 포인트로 이동하여 이미지 내의 오브젝트를 이동하는 것입니다.

이 작업은 잠상 코드가 중간 잠상 코드에 매핑된 다음 출력 이미지를 생성하기 위해 제너레이터로 전송되는 StyleGAN2 아키텍처를 기반으로 합니다. 이 잠재 코드는 제너레이터에서 다양한 수준의 속성을 제어하며, 기본적으로 저차원 잠재 공간을 고차원 이미지 공간에 매핑합니다.

![파이프라인 개요. GAN으로 생성된 이미지가 주어지면 사용자는 여러 개의 핸들 포인트(빨간색 점), 타겟 포인트(파란색 점), 그리고 선택적으로 편집 중 움직일 수 있는 영역을 나타내는 마스크(밝은 영역)만 설정하면 됩니다. 우리의 접근 방식은 모션 감독(3.2절)과 포인트 트래킹을 반복적으로 수행합니다. (3.3절). 모션 감독 단계에서는 핸들 포인트(빨간색 점)가 목표 포인트(파란색 점)를 향해 이동하도록 유도하고, 포인트 추적 단계에서는 핸들 포인트를 업데이트합니다. 핸들 포인트를 업데이트하여 이미지에서 객체를 추적합니다. 이 프로세스는 핸들 포인트가 해당 목표 포인트에 도달할 때까지 계속됩니다.](Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb/Untitled%201.png)

파이프라인 개요. GAN으로 생성된 이미지가 주어지면 사용자는 여러 개의 핸들 포인트(빨간색 점), 타겟 포인트(파란색 점), 그리고 선택적으로 편집 중 움직일 수 있는 영역을 나타내는 마스크(밝은 영역)만 설정하면 됩니다. 우리의 접근 방식은 모션 감독(3.2절)과 포인트 트래킹을 반복적으로 수행합니다. (3.3절). 모션 감독 단계에서는 핸들 포인트(빨간색 점)가 목표 포인트(파란색 점)를 향해 이동하도록 유도하고, 포인트 추적 단계에서는 핸들 포인트를 업데이트합니다. 핸들 포인트를 업데이트하여 이미지에서 객체를 추적합니다. 이 프로세스는 핸들 포인트가 해당 목표 포인트에 도달할 때까지 계속됩니다.

이미지 조작 프로세스는 반복적입니다. 각 단계에서 모션 감독 손실 함수를 사용하여 잠재 코드를 최적화하고 핸들 포인트를 타깃으로 이동합니다. 그 결과 새로운 잠재 코드와 새로운 이미지가 생성됩니다. 그런 다음 추적 프로세스가 오브젝트의 변경에 따라 핸들 포인트의 위치를 업데이트합니다. 이 루프는 핸들 포인트가 목표 위치에 도달할 때까지 계속됩니다.

![모션 감시는 제너레이터의 피처 맵에서 시프트 패치 손실 를 통해 이루어집니다. 가장 가까운 이웃 검색을 통해 동일한 특징 공간에서 포인트 추적을 수행합니다.](Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb/Untitled%202.png)

모션 감시는 제너레이터의 피처 맵에서 시프트 패치 손실 를 통해 이루어집니다. 가장 가까운 이웃 검색을 통해 동일한 특징 공간에서 포인트 추적을 수행합니다.

또한 사용자는 바이너리 마스크를 그려서 이미지의 어느 영역을 움직일 수 있는지 정의할 수 있습니다. 마스크를 사용하는 경우 손실 함수에 추가 항을 추가하여 최적화 프로세스 중에 마스크가 제거된 영역이 고정된 상태로 유지되도록 합니다.

이 프로세스를 효율적으로 만들기 위해 이 연구에서는 광학 흐름 추정과 같은 추가 모델에 의존하지 않는 포인트 추적 방법을 도입했습니다. 대신, GAN의 차별적 특징을 활용하여 특징 패치에서 가장 가까운 이웃 검색을 통해 포인트를 추적합니다.

이 접근 방식은 파이토치 프레임워크와 아담 옵티마이저를 사용하여 잠재 코드를 최적화하는 방식으로 구현되었습니다. 사용자 친화적인 GUI도 개발되어 사용자가 편집할 때마다 몇 초의 지연 시간만으로 이미지를 대화형으로 조작할 수 있습니다.

4 EXPERIMENTS

연구진은 FFHQ, AFHQCat, SHHQ, LSUN 자동차, LSUN 고양이, 랜드스케이프 본부, 현미경, 사자, 개, 코끼리를 포함한 자체 추출 데이터 세트 등 다양한 데이터 세트에서 사전 학습된 StyleGAN2를 사용하여 이미지 조작 방법을 평가합니다.

이 평가의 주요 비교 모델은 마스크 입력을 지원하지 않지만 사용자가 고정점을 정의할 수 있는 UserControllableLT입니다. 테스트는 16x16 그리드를 사용하여 마스크 외부의 점을 고정 점으로 사용하여 수행됩니다. 포인트 추적을 위해 RAFT 및 PIP와 추가 비교를 수행했습니다.

정성적 평가 결과, 이 방법이 핸들 포인트를 정확하게 조작할 수 있어 동물의 포즈나 자동차의 모양을 변경하는 등 보다 자연스러운 결과를 얻을 수 있는 것으로 나타났습니다. 이에 비해 UserControllableLT는 핸들 포인트를 정확하게 이동하지 못하고 종종 원치 않는 변경이 발생했습니다.

또한 연구팀은 StyleGAN의 잠재 공간에 실제 이미지를 삽입하는 GAN 반전 기법을 사용하여 실제 이미지 편집을 수행했습니다.

정량적 평가에는 얼굴 랜드마크 조작과 쌍을 이루는 이미지 재구성이 포함됩니다. 이 방법의 성능은 특히 조작의 정확성과 이미지 품질 유지 측면에서 UserControllableLT보다 우수했습니다. 또한 이 접근 방식은 페어링된 이미지 재구성에서도 더 나은 결과를 얻었습니다.

![마스크의 효과. 우리의 접근 방식은 이동 가능한 영역을 마스킹할 수 있습니다. 강아지의 머리 부분을 마스킹한 후 나머지 부분은 거의 변하지 않습니다.](Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb/Untitled%203.png)

마스크의 효과. 우리의 접근 방식은 이동 가능한 영역을 마스킹할 수 있습니다. 강아지의 머리 부분을 마스킹한 후 나머지 부분은 거의 변하지 않습니다.

이 연구는 또한 이 방법을 통해 사용자가 이동 가능한 영역을 나타내는 바이너리 마스크를 입력할 수 있음을 보여줍니다. 마스크 기능은 모호함을 줄이고 특정 영역을 고정하는 데 도움이 됩니다. 이 방법은 훈련 이미지 분포에서 벗어난 이미지를 생성하는 분포 외 조작을 처리할 수 있는 기능이 있습니다. 그러나 품질은 여전히 훈련 데이터의 다양성에 영향을 받습니다.

저자들은 이미지 조작 방법이 오용될 가능성이 있기 때문에 이 방법을 사용하는 모든 애플리케이션이나 연구는 인격권 및 개인정보 보호 규정을 엄격하게 준수해야 한다고 지적합니다.

5 CONCLUSION

연구진은 포인트 기반 이미지 편집을 위한 인터랙티브 방법인 DragGAN을 도입했습니다. 이 접근 방식은 사전 학습된 GAN(생성적 적대 신경망)을 사용하여 사실적인 이미지의 범위를 벗어나지 않으면서도 사용자 입력을 밀접하게 따르는 이미지를 생성합니다. 이 방법의 주요 차별화 요소는 도메인별 모델링이나 보조 네트워크에 의존하지 않는다는 점입니다.

DragGAN의 성공은 두 가지 핵심 요소, 즉 여러 핸들 포인트를 점진적으로 목표 위치로 이동시키는 잠재 코드의 최적화와 이러한 핸들 포인트의 궤적을 정확하게 따르는 포인트 추적 절차에 달려 있습니다. 이 시스템은 GAN의 중간 피처 맵의 성능을 활용하여 인터랙티브한 속도로 픽셀 단위의 완벽한 이미지 변환을 달성합니다.

DragGAN은 기존 방식에 비해 GAN 기반 조작에서 우수한 성능을 입증했으며, 생성 전구체를 사용하여 이미지 편집을 향상시킬 수 있는 새로운 길을 제시했습니다. 연구팀은 향후 연구에서 포인트 기반 편집을 3D 제너레이티브 모델까지 확장할 계획입니다.

![Untitled](Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb/Untitled%204.png)

- 요약
    
    문제 정의: 연구진은 인터랙티브한 포인트 기반 이미지 편집이라는 과제를 해결하고 있습니다. 이들의 목표는 사전 학습된 생성적 적대 신경망(GAN)을 사용하여 사용자 입력을 정확하게 따르는 이미지를 생성하는 동시에 사실적인 이미지를 생성하는 접근 방식을 만드는 것입니다.
    
    접근 방식 개발: 연구진은 도메인별 모델링이나 보조 네트워크에 의존하지 않는 DragGAN이라는 방법을 설계했습니다. 연구진은 이 접근법의 두 가지 핵심 요소인 잠재 코드 최적화와 포인트 추적 절차를 제시합니다.
    
    잠재 코드 최적화: 이 단계의 목표는 여러 '핸들 포인트'(사용자가 이동하거나 편집하려는 이미지의 선택된 포인트)를 목표 위치로 점진적으로 이동하는 것입니다. 이 이동을 통해 이미지의 특정 측면을 조작할 수 있습니다.
    
    포인트 추적 절차: 이 프로세스는 핸들 포인트의 궤적을 정확하게 추적하는 역할을 합니다. 이렇게 하면 핸들 포인트가 사용자가 의도한 방향으로 이동하여 이미지 조작의 충실도를 유지할 수 있습니다.
    
    GAN 활용: 두 구성 요소(잠재 코드 최적화 및 포인트 추적)는 모두 GAN의 중간 피처 맵의 성능을 활용하여 픽셀 단위의 정밀한 이미지 변형과 인터랙티브 성능을 구현합니다.
    
    접근 방식 평가: 연구원들은 다양한 해상도의 여러 데이터 세트에서 접근 방식을 테스트합니다. 다양한 메트릭에서 UserControllableLT, RAFT, PIP와 같은 다른 기준 방법과 DragGAN의 결과를 비교합니다. 평가에는 정성적 평가와 정량적 평가가 모두 포함되며, 이 평가는 이 방법이 포인트를 얼마나 잘 조작하고 추적할 수 있는지에 중점을 둡니다.
    
    강점과 약점 파악: DragGAN은 GAN 기반 이미지 조작에서 우수한 성능을 보이지만 한계도 있습니다. 예를 들어, 훈련 데이터에 없는 포즈를 만들려고 할 때 아티팩트가 발생할 수 있으며, 텍스처가 없는 영역에서 추적이 드리프트될 수 있습니다.
    
    향후 작업 고려: 2D 이미지 조작을 위한 DragGAN의 성공을 바탕으로 연구진은 포인트 기반 편집을 3D 생성 모델까지 확장할 계획입니다. 이렇게 하면 훨씬 더 강력하고 유연한 이미지 및 모양 조작이 가능해집니다.
    
    사회적 영향에 대한 논의: 연구진은 가짜 포즈나 표정을 가진 실제 사람의 이미지를 만드는 등 이 방법이 오용될 가능성도 고려하고 있습니다. 연구진은 드래그간을 사용할 때는 개인의 권리와 개인정보 보호 규정을 존중해야 한다고 강조합니다.