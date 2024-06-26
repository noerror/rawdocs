# Text-To-4D Dynamic Scene Generation

*이 논문에서는 확산 모델과 동적 신경 방사장(Neural Radiance Field, NeRF)을 활용하여 텍스트 설명에서 동적 3D 장면을 생성하는 새로운 방법인 MAV3D를 소개합니다. 제안된 시스템은 기존 확산 기반 모델의 기능을 향상시켜 다양한 관점에서 텍스트 프롬프트에 지정된 대로 시간적 3D 장면을 생성할 수 있습니다. 이 작업에는 대체 방법과 비교한 모델 성능 평가, 모델 설계를 정당화하기 위한 철저한 제거 연구, 이미지에서 동적 장면을 생성하기 위한 모델 확장 등이 포함됩니다. 그러나 이 논문은 실시간 애플리케이션을 위해 동적 NeRF를 일련의 분리된 메시로 변환할 때 발생하는 비효율성, 텍스처 품질 개선 및 비디오 생성 제어의 필요성 등의 한계도 인정하고 있습니다.*

![Untitled](Text-To-4D%20Dynamic%20Scene%20Generation%20e9a56fc92d9148a7832c7c0615a212a4/Untitled.png)

[https://make-a-video3d.github.io/](https://make-a-video3d.github.io/)

[https://arxiv.org/pdf/2301.11280.pdf](https://arxiv.org/pdf/2301.11280.pdf)

1. Introduction

이 백서에서는 자연어 입력에서 4D(3D + 시간) 동적 장면을 생성하는 새로운 방법인 MAV3D(MakeA-Video3D)를 소개합니다. 자연어 프롬프트에서 2D 이미지와 3D 도형 또는 비디오를 생성하는 데 상당한 진전이 있었지만, 이 작업은 시간 및 공간 기능을 병합하여 4D 출력을 생성합니다.

한 가지 중요한 과제는 2D 또는 3D 이미지 생성의 경우와 달리 학습할 텍스트 주석이 포함된 대규모 4D 모델 데이터 세트가 없다는 것입니다. 이 논문에서는 사전 학습된 2D 비디오 생성기로 시작한 다음, 이를 '통계적' 멀티 카메라 설정으로 사용하여 다양한 관점에서 물체의 형상과 측광을 재구성할 것을 제안합니다.

그러나 기존 방법을 사용한 직접 최적화는 동적 3D 장면의 효율적인 표현, 감독 소스, 공간과 시간 모두에서 해상도 확장 등의 여러 가지 장애물로 인해 만족스럽지 못한 결과를 초래합니다.

이러한 문제를 극복하기 위해 저자는 신경 방사 필드(NeRF)를 기반으로 4D 장면을 6개의 다중 해상도 특징면 집합으로 표현합니다. 저자는 이 표현을 감독하기 위해 다단계 훈련 파이프라인을 제안합니다. 처음에는 텍스트-투-이미지 모델을 사용하여 정적 3D 장면을 텍스트 프롬프트에 맞춘 다음 점수 증류 샘플링과 새로운 시간 인식 모션 정규화기를 사용하여 모델에 점진적으로 동적 요소를 도입합니다.

시간 인식 초해상도 미세 조정을 통해 출력의 해상도를 높입니다. 저자들은 텍스트-비디오 모델의 초고해상도 모듈에서 스코어 증류 샘플링을 사용하여 3D 장면 모델을 감독함으로써 시각적 충실도를 높이고 추론 중에 더 높은 해상도의 출력을 가능하게 합니다.

텍스트-비디오 모델과 동적 NeRF를 사용하여 4D 시간적 표현을 생성하는 MAV3D의 도입, 4D 장면 표현을 향상시키기 위해 정적, 시간적, 초고해상도 모델의 그라데이션 정보를 점진적으로 포함하는 다단계 최적화 체계, 선택과 결과를 입증하기 위한 제거 연구를 포함한 광범위한 실험 등이 저자들의 공헌입니다.

2. Related work

이 백서에서는 뉴럴 렌더링과 머신 러닝 분야의 여러 발전된 기술을 통합하는 방법에 대해 설명합니다.

신경 방사 필드(NeRF)는 장면 좌표를 입력으로 받아 볼륨 렌더링을 통해 이미지를 형성하는 신경망을 통해 3D 장면을 표현하기 때문에 활용됩니다. 이는 희소 또는 다중 해상도일 수 있는 복셀 그리드와 같은 3D 데이터 구조를 사용하여 개선되었으며 텐서 인수분해 또는 해싱을 통해 더욱 효율적으로 만들 수 있습니다.

동적 장면을 생성할 때는 자유 시점 비디오와 공간과 시간에 따라 네트워크를 조절하고 깊이 또는 장면 흐름에서 추가적인 감독을 통합하여 NeRF를 동적으로 만드는 최근 접근 방식에서 영감을 얻었습니다. 일부 접근 방식은 3D 포인트를 정적인 표준 장면으로 시간에 따라 변형하는 방식을 사용합니다. 효율성을 위해 명시적 복셀 그리드 또는 텐서 인수분해를 사용할 수 있습니다.

텍스트에서 3D 장면을 생성하는 개념은 오래전부터 사용되어 왔으며, 초기에는 텍스트에서 기하학적 관계를 파싱하고 알려진 오브젝트 라이브러리에서 장면을 구축하는 시도가 있었습니다. 일부 접근 방식은 텍스트와 도형의 쌍으로 구성된 데이터 세트에서 신경망을 엔드 투 엔드로 훈련하지만, 쌍으로 구성된 데이터가 충분하지 않기 때문에 확장하기가 어렵습니다. 다른 접근 방식은 사전 학습된 CLIP 모델 또는 텍스트-이미지 확산 모델을 사용하여 텍스트에서 3D 모양을 생성합니다. 이 백서에서는 이 전략을 확장하여 텍스트-비디오 모델을 사용하여 4D 콘텐츠를 생성합니다.

![Untitled](Text-To-4D%20Dynamic%20Scene%20Generation%20e9a56fc92d9148a7832c7c0615a212a4/Untitled%201.png)

최근 확산 모델은 크게 개선되어 고급 이미지 합성 및 비디오와 같은 다른 미디어 형식에 대한 생성 모델을 생성할 수 있게 되었습니다. 저자의 비디오 생성기는 텍스트-투-이미지(T2I) 모델을 기반으로 하고 레이블이 없는 비디오를 학습하는 MAV(MakeA-Video) 모델을 기반으로 합니다.

3. Method

이 프로젝트의 목표는 텍스트 설명을 기반으로 동적 3D 장면을 생성하는 방법을 만드는 것입니다. 텍스트를 3D 장면이나 동적 3D 장면과 짝을 이루는 훈련 데이터가 없기 때문에 이 작업은 어렵습니다. 대규모 이미지, 텍스트, 비디오 데이터로 훈련된 사전 훈련된 텍스트-비디오(T2V) 확산 모델을 장면의 모양과 동작을 모델링하는 장면으로 활용하여 이 문제를 해결합니다.

![MAV3D의 전체 파이프라인. 먼저 세 개의 순수 공간 평면(녹색으로 표시)만 활용하고 단일 이미지를 렌더링한 다음 T2I 모델을 사용하여 SDS 손실을 계산합니다. 두 번째 단계에서는 부드러운 전환을 위해 초기화된 세 개의 평면(주황색)을 추가로 추가합니다. 0으로 초기화하고, 전체 비디오를 렌더링하고, T2V 모델을 사용하여 SDS-T 손실을 계산합니다. 세 번째 단계(SRFT)에서는 다음을 수행합니다. 고해상도 컴포넌트에 입력으로 전달되는 고해상도 비디오를 추가로 렌더링하며, 저해상도를 조건으로 합니다.](Text-To-4D%20Dynamic%20Scene%20Generation%20e9a56fc92d9148a7832c7c0615a212a4/Untitled%202.png)

MAV3D의 전체 파이프라인. 먼저 세 개의 순수 공간 평면(녹색으로 표시)만 활용하고 단일 이미지를 렌더링한 다음 T2I 모델을 사용하여 SDS 손실을 계산합니다. 두 번째 단계에서는 부드러운 전환을 위해 초기화된 세 개의 평면(주황색)을 추가로 추가합니다. 0으로 초기화하고, 전체 비디오를 렌더링하고, T2V 모델을 사용하여 SDS-T 손실을 계산합니다. 세 번째 단계(SRFT)에서는 다음을 수행합니다. 고해상도 컴포넌트에 입력으로 전달되는 고해상도 비디오를 추가로 렌더링하며, 저해상도를 조건으로 합니다.

텍스트 프롬프트가 주어지면 공간과 시간의 어느 지점에서든 프롬프트에 따라 장면을 모델링하는 4D 장면 표현을 맞춥니다. 페어링된 훈련 데이터가 없기 때문에 결과물을 직접 감독할 수는 없지만, 이 표현에서 이미지 시퀀스를 렌더링하여 비디오로 전환할 수 있습니다. 그런 다음 이 비디오와 텍스트 프롬프트를 T2V 확산 모델에 전달하여 비디오의 사실성과 텍스트 프롬프트에 대한 정렬에 점수를 매깁니다. 그런 다음 점수 증류 샘플링(SDS)을 사용하여 씬 파라미터에 대한 업데이트 방향을 계산합니다.

새로운 4D 장면 표현, 사실적인 모션을 위한 여러 모션 정규화기를 포함하는 다단계 정적-동적 최적화 체계, 모델의 해상도를 개선하기 위한 초해상도 미세 조정(SRFT)을 사용합니다.

동적 3D 씬은 최근 뉴럴 렌더링의 발전과 같이 암시적으로 표현됩니다. 큰 씬 모션을 유연하게 모델링할 수 있는 씬 모델 아키텍처로 HexPlane을 선택했습니다.

이 모델을 훈련하기 위해 사전 훈련된 조건부 비디오 생성기를 위한 SDS의 확장인 시간적 점수 증류 샘플링(SDS-T)을 도입했습니다. SDS를 사용하여 헥스플레인을 직접 최적화하면 시각적 아티팩트와 최적이 아닌 수렴이 발생하므로 먼저 정적 3D 장면을 최적화한 다음 이를 4D로 확장하는 다단계 정적-동적 최적화 방식을 사용합니다.

- 텍스트 프롬프트에서 동적 4D 장면을 생성하기 위해 MAV3D 모델에서 사용하는 2단계 프로세스
    
    이 프로세스는 정적 3D 씬을 생성하는 것으로 시작하여 시간 차원을 포함하도록 확장하여 효과적으로 4D 동적 씬이 됩니다.
    
    1. 정적에서 동적으로: 정적 최적화(3D)
        
        이 프로세스는 텍스트 프롬프트와 일치하는 정적 3D 씬을 최적화하는 것으로 시작됩니다. 이를 위해 모델의 핵심 구성 요소인 헥스 평면의 세 시간 평면이 0으로 설정됩니다. 이렇게 하면 모델이 정적 3D 씬을 생성하도록 효과적으로 제한됩니다.
        
        이 단계에서 모델은 무작위 카메라 포즈(이 경우 8개)를 샘플링하고 각 뷰에서 저해상도(64x64) 이미지를 렌더링합니다. 이 작업은 드림퓨전 모델과 유사한 뷰 종속 프롬프트 엔지니어링 기법을 사용하여 수행됩니다.
        
        그런 다음 생성된 장면은 사전 학습된 텍스트-이미지 변환(T2I) 모델에 기반한 점수 증류 샘플링(SDS)을 사용하여 감독됩니다. 이 모델은 Singer 외(2022)에서 참조한 것으로, 이미지와 해당 텍스트 설명의 대규모 데이터 세트에 대해 사전 학습되었습니다.
        
    2. 동적 최적화(4D)
        
        정적 3D 장면이 생성되고 최적화되면 프로세스의 두 번째 단계가 시작됩니다. 이 단계에서는 헥스플레인의 세 가지 시간 평면을 변경하여 동적 4D 씬을 생성하도록 모델을 업데이트합니다.
        
        이제 모델은 정적 이미지 대신 저해상도 비디오 일괄 렌더링(64x64, 각 16프레임)을 수행합니다. 다시 한 번, 각 비디오에 대해 8개의 서로 다른 무작위 카메라 포즈가 샘플링됩니다.
        
        생성된 비디오는 Singer 외(2022)에서 참조한 사전 학습된 텍스트-비디오(T2V) 모델을 기반으로 SDS-T라고 하는 SDS의 확장을 사용하여 감독됩니다. SDS-T 방법은 모델이 생성된 동적 장면을 텍스트 프롬프트와 일치하도록 최적화하는 데 도움이 됩니다.
        
        이 단계에서는 모델이 고품질의 4D 합성을 생성할 수 있도록 여러 정규화 기법을 사용합니다. 정규화는 학습 과정에서 과적합을 방지하고 모델의 일반화를 개선하기 위해 적용되는 제약 조건 또는 페널티입니다.
        
    
    요약하면, 이 접근 방식은 시각적 아티팩트와 최적이 아닌 수렴을 방지하기 위해 다단계 프로세스를 통해 정적 3D 씬에서 동적 4D 씬으로 전환합니다.
    

SRFT(초고해상도 미세 조정)의 마지막 단계에서는 사전 학습 및 고정된 비디오 초고해상도 모듈을 사용하여 4D 씬 모델에서 고해상도 렌더링을 개선합니다. SRFT로 미세 조정하면 사실적인 고해상도 비디오가 만들어지지만 텍스트 프롬프트에 맞춰 정렬되지는 않으므로 SRFT 중에 SDS-T도 사용합니다.

4. Experiments

MAV3D는 텍스트 설명에서 동적인 3D 장면을 생성하기 위해 개발된 새로운 방법입니다. 동종 최초의 방법으로서 기준선으로 삼은 세 가지 대체 방법과 비교하여 평가되었습니다. 또한 MAV3D는 텍스트-비디오(T2V) 및 텍스트-3D 작업을 위한 기존 방법과 비교하여 단순화된 버전으로 평가되었습니다. 또한 설계 접근 방식을 뒷받침하기 위해 광범위한 제거 연구가 수행되었습니다.

생성된 비디오는 텍스트와 생성된 장면 간의 일관성을 측정하는 CLIP R-Precision 메트릭을 사용하여 평가되었습니다. 질적 지표도 사용되었는데, 인간 평가자에게 품질, 텍스트 충실도, 모션 양, 모션 사실성을 기준으로 생성된 비디오를 비교하도록 요청했습니다.

텍스트에서 4D로 변환 작업의 경우, MAV3D는 객관적인 R-정확도 지표와 모든 지표에서 인간 평가자가 선호하는 지표 모두에서 세 가지 기준 방법보다 우수한 성능을 보였습니다. 다양한 카메라 시야각에서 테스트한 결과, MAV3D는 모든 시야각에서 일관되게 장면을 렌더링하는 능력을 보여주었습니다.

Text-to-3D 작업의 경우 MAV3D\t라는 MAV3D의 단순화된 버전을 Stable-DreamFusion 및 Point-E와 비교했습니다. MAV3D\t는 품질과 텍스트 정렬 측면에서 이들보다 우수한 성능을 보였습니다. 마찬가지로 텍스트-비디오 작업의 경우, 또 다른 단순화된 버전인 MAV3D\z가 유망한 결과를 보여주었습니다.

절제 연구를 통해 장면 초고해상도 미세 조정 및 정적 장면 사전 학습과 같은 특정 기능을 포함하면 생성된 비디오의 품질이 향상되는 것으로 나타났습니다. 또한 모션 레귤레이터와 특정 NeRF 백본의 활용은 생성된 장면의 모션 품질과 사실감에 긍정적으로 기여하는 것으로 나타났습니다.

MAV3D는 실시간 렌더링도 가능했습니다. 생성된 3D 모델은 기존 그래픽 엔진과 호환되는 텍스처 메시로 변환할 수 있어 가상 현실 및 게임과 같은 애플리케이션에 적합했습니다. 또한 주어진 입력 이미지에서 깊이와 모션을 모두 생성할 수 있는 기능을 시연하며 이미지에서 4D로 변환하는 작업을 위한 MAV3D의 확장 기능도 선보였습니다.

5. Discussion

애니메이션 3D 콘텐츠 제작은 복잡한 작업이며, 특히 전문가용으로 설계된 도구의 가용성이 제한되어 있기 때문에 더욱 그렇습니다. 정적인 3D 오브젝트를 생성할 수 있는 모델은 있지만 동적인 장면을 만드는 것은 더 어렵습니다. 특히 텍스트 설명이 있든 없든 4D 모델이 부족하다는 점은 이러한 복잡성을 더욱 가중시킵니다.

MAV3D는 여러 확산 모델과 동적 신경 방사 필드(NeRF)를 결합하여 세계 지식을 3D 시간적 표현에 통합하는 새로운 접근 방식입니다. 이 모델은 여러 관점에서 볼 수 있는 텍스트 설명에서 동적 장면을 생성할 수 있도록 함으로써 기존 확산 기반 모델의 기능을 향상시킵니다.

이러한 발전에도 불구하고 MAV3D에는 몇 가지 한계가 있습니다. 실시간 애플리케이션을 위해 동적 NeRF를 일련의 분리된 메시로 변환하는 프로세스는 비효율적입니다. 정점의 궤적을 직접 예측하면 이 문제를 개선할 수 있습니다. 초고해상도 정보를 사용하여 표현 품질이 향상되었지만 더 세밀한 텍스처를 생성하기 위해서는 추가적인 개선이 필요합니다. 또한 표현의 품질은 다양한 뷰에서 비디오를 생성하는 T2V(텍스트-비디오) 모델의 용량에 따라 달라집니다. 보기에 따른 프롬프트를 사용하면 다중 얼굴 문제를 완화하는 데 도움이 되지만, 비디오 생성기를 더 많이 제어할 수 있다면 도움이 될 것입니다.

- SDS
    
    점수 증류 샘플링(SDS)은 기존 샘플링 방법보다 빠른 확산 모델에서 샘플링하는 기법입니다. SDS는 확산 모델의 점수를 더 작은 모델로 증류하여 샘플을 빠르게 생성하는 데 사용할 수 있습니다.
    
    확산 모델의 점수를 추출하기 위해 SDS는 엔트로피 최소화라는 기술을 사용합니다. 엔트로피 최소화는 주어진 데이터 집합에 대해 가장 가능성이 높은 설명을 찾는 기법입니다. SDS의 경우 데이터는 확산 모델의 출력이고 설명은 확산 모델의 점수입니다.
    
    확산 모델의 점수가 추출되면 이를 사용하여 샘플을 생성할 수 있습니다. 이를 위해 더 작은 모델에 무작위 노이즈 벡터가 주어지고 이 점수를 사용하여 샘플을 생성합니다. 그런 다음 샘플을 확산 모델에 다시 전달하고 원하는 샘플이 생성될 때까지 이 과정을 반복합니다.
    
    SDS는 확산 모델의 전체 출력을 생성할 필요가 없기 때문에 기존 샘플링 방식보다 빠른 샘플링 방식입니다. 대신 훨씬 더 작은 확산 모델의 점수만 생성하면 됩니다. 따라서 SDS는 실시간 렌더링 및 애니메이션과 같이 속도가 중요한 애플리케이션에 이상적입니다.
    
    다음은 SDS의 몇 가지 장점입니다:
    
    기존 샘플링 방법보다 빠릅니다.
    직접 사용하기에는 너무 큰 확산 모델에서 샘플을 생성하는 데 사용할 수 있습니다.
    원하는 데이터에 대해 학습되지 않은 확산 모델에서 샘플을 생성하는 데 사용할 수 있습니다.
    다음은 SDS의 몇 가지 단점입니다:
    
    기존 샘플링 방법보다 정확도가 떨어질 수 있습니다.
    기존 샘플링 방법보다 구현하기가 더 어려울 수 있습니다.
    생성된 샘플의 품질을 제어하기가 더 어려울 수 있습니다.