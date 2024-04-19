# GaussianDreamer: Fast Generation from Text to 3D Gaussian Splatting with Point Cloud Priors

[https://arxiv.org/abs/2310.08529](https://arxiv.org/abs/2310.08529)

[https://taoranyi.com/gaussiandreamer/](https://taoranyi.com/gaussiandreamer/)

### 1. Introduction

기존에는 3D 에셋을 제작하는 데 많은 비용이 들고 전문적인 작업이 필요했습니다. 하지만 디지털 디자인의 환경이 변화하고 있습니다. 최근 다양한 연구 논문에서 인용되고 있는 확산 모델은 디테일하고 사실적인 2D 이미지를 생성하는 데 놀라울 정도로 능숙해졌습니다. 이러한 2D 모델의 성능을 활용하여 3D 영역에 적용함으로써 3D 에셋을 더 쉽게 제작할 수 있도록 하는 것이 최근 급부상하고 있는 관심 분야입니다. 이는 텍스트 설명을 3D 시각적 모델로 직접 변환하는 것과 같은 작업과 특히 관련이 있습니다.

![유니티는 가우시안드리머라는 간단하면서도 효율적인 프레임워크를 제안합니다. 이 프레임워크는 가우시안 스플래팅을 통해 3D와 2D 확산 모델을 연결하여 3D 일관성과 풍부한 생성 디테일을 모두 갖췄습니다. 이 방법을 사용하면 단일 GPU에서 25분 이내에 학습을 완료하고 실시간 렌더링을 구현할 수 있습니다.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled.png)

유니티는 가우시안드리머라는 간단하면서도 효율적인 프레임워크를 제안합니다. 이 프레임워크는 가우시안 스플래팅을 통해 3D와 2D 확산 모델을 연결하여 3D 일관성과 풍부한 생성 디테일을 모두 갖췄습니다. 이 방법을 사용하면 단일 GPU에서 25분 이내에 학습을 완료하고 실시간 렌더링을 구현할 수 있습니다.

이 목표를 달성하기 위해 두 가지 주요 전략이 있습니다:

3D 확산 모델: 이 방법은 3D 데이터로 특별히 새로운 확산 모델을 훈련하는 것입니다. 이 방법은 견고한 3D 일관성을 직접적으로 구현할 수 있지만, 상당한 어려움이 있습니다. 3D 데이터를 확보하는 것은 어려울 뿐만 아니라 비용도 많이 듭니다. 사용 가능한 3D 데이터 세트가 2D 데이터 세트에 비해 상당히 작기 때문에 결과물인 3D 에셋은 특히 복잡한 텍스트 프롬프트에 직면했을 때 풍부함, 복잡성 및 적응성이 부족한 경우가 많습니다.

2D를 3D로 전환: 여기서는 기존 2D 확산 모델을 3D 생성에 맞게 조정합니다. 이 접근 방식은 2D 모델의 방대한 데이터를 활용할 수 있어 디테일과 복잡성이 높은 에셋을 제작할 수 있다는 분명한 이점이 있습니다. 하지만 한 가지 문제가 있습니다. 이러한 2D 모델은 3D 관점을 염두에 두고 설계되지 않았기 때문에 특히 복잡한 디자인의 경우 결과물인 3D 에셋에서 기하학적 일관성을 유지하는 것이 어렵습니다.

이 백서에서는 두 가지 전략의 장점을 조화롭게 결합하기 위해 3D 가우시안 스플래팅을 활용하는 혁신적인 기법을 소개합니다. 이 프로세스는 3D 디퓨전 모델에서 생성된 기본 3D 모델로 시작하는데, 이 모델은 일관성이 있지만 세밀한 디테일이 부족할 수 있습니다. 이 거친 모델은 3D 가우시안 세트가 오버레이되는 기초 역할을 합니다. 포인트 클라우드와 유사한 가우시안 집합은 본질적으로 기하학적 원리와 일치하여 일관성을 보장합니다. 디자인을 더욱 정교하고 풍부하게 만들기 위해 노이즈 포인트 증가 및 색상 섭동과 같은 추가 기법이 사용됩니다. 이 접근 방식의 장점은 2D 확산 모델과의 상호 작용에 있으며, 점수 증류 샘플링(SDS)이라는 특정 손실 함수를 사용하여 3D 가우시안 모델을 최적화합니다. 3D 확산 모델과 가우시안 스플래팅의 인사이트가 결합되어 전체 프로세스가 효율적일 뿐만 아니라 신속합니다. 놀랍게도 최종 3D 에셋은 다른 포맷으로 변환할 필요 없이 즉시 렌더링할 수 있습니다.

요약하면 이 연구의 주요 공헌은 다음과 같습니다:

3D와 2D 확산 모델을 능숙하게 연결하여 최종 결과물에 기하학적 일관성과 풍부한 디테일을 모두 보장하는 '가우시안드리머'라는 새로운 텍스트-3D 변환 방법의 공개.

노이즈 포인트 그로잉 및 컬러 퍼트러베이션과 같은 기술을 도입하여 초기화된 3D 가우시안 효과를 향상시켜 최종 에셋을 더욱 풍부하게 만듭니다.

이 방법론은 간단하지만 매우 효과적입니다. 단일 GPU를 사용하여 단 25분 만에 디테일한 3D 모델을 제작할 수 있습니다. 또한 이러한 에셋은 50MB 미만의 저장 공간을 필요로 하는 경량이며 실시간으로 렌더링할 수 있습니다.

### 2. Related Works

디지털 디자인 영역에서는 특히 디퓨전 모델을 사용하여 텍스트를 3D 에셋으로 변환하는 데 있어 유망한 발전이 있었습니다. 이러한 모델은 크게 2D 확산 모델과 3D 확산 모델의 두 가지 범주로 나뉩니다. 이름에서 알 수 있듯이 두 모델은 사용하는 훈련 데이터의 차원에 따라 구분됩니다.

이 분야의 선구적인 연구 중 하나는 점수 증류 샘플링(SDS) 방법을 도입한 '드림퓨전(Dreamfusion)'입니다. 이 방법은 2D 확산 모델에서 얻은 인사이트를 활용하여 3D 표현을 업데이트합니다. 2D 확산 모델을 3D 공간으로 원활하게 전환하기 위한 또 다른 주목할 만한 접근 방식인 '스코어 자코비안 체인(SJC)'이 제안되었습니다. 이러한 기초적인 방법에 이어 3D 에셋 생성의 품질을 향상시키기 위한 여러 연구가 진행되었습니다. 그러나 생성된 3D 에셋의 '다중면 문제'라는 난제가 여전히 남아 있습니다. 이를 해결하기 위해 다양한 시점의 의미를 강조하고 멀티뷰 데이터를 통합하는 등 다양한 기법이 제안되었습니다. 일부 모델에서는 3D 모델의 각 시점을 해당 텍스트와 일치시키기 위해 '클립'을 사용하기도 했습니다.

이러한 발전의 와중에 주목할 만한 기술인 3D 가우시안 스플래팅이 등장하여 DreamGaussion 및 GSGEN과 같은 작품에 사용되고 있습니다. 드림가우션은 단일 이미지에서 3D 에셋을 생성하는 데 중점을 두는 반면, GSGEN은 텍스트를 고품질 3D 에셋으로 변환하는 데 탁월합니다. 유니티의 접근 방식은 이러한 방법과 유사하게 3D 가우시안 스플래팅의 잠재력을 활용하여 텍스트 프롬프트에서 상세한 3D 에셋을 신속하게 생성할 수 있습니다.

이와는 별도로 3D 데이터세트만을 학습하는 Point-E, Shap-E, 3DGen과 같은 3D 확산 모델은 텍스트 지시가 주어지면 단 몇 분 만에 3D 에셋을 생성할 수 있습니다. 하지만 이 과정에서 병목 현상이 발생하는 것은 데이터 요구 사항입니다. 강력한 확산 모델을 훈련하려면 상당한 양의 3D 데이터가 필요합니다. 이러한 데이터 세트의 희소성을 고려할 때 최고 수준의 3D 에셋을 제작하는 것은 여전히 어려운 과제입니다. 유니티는 3D 가우시안 스플래팅을 통해 2D와 3D 확산 모델의 강점을 시너지로 결합하여 이러한 격차를 해소하고자 합니다. 이 하이브리드 접근 방식은 기하학적 일관성을 보장할 뿐만 아니라 생성된 3D 에셋에 더 높은 수준의 다양성과 사실감을 불어넣습니다.

### 3. Method

프로세스 소개:
이 방법은 2D 및 3D 확산 모델의 강점을 모두 활용하여 텍스트를 3D 에셋으로 변환하는 작업을 개선하는 데 중점을 두며, 특히 3D 가우시안 스플래팅이라는 기법을 통해 이루어집니다. 이 프레임워크는 3D 가우시안 모델을 사용하여 3D 가우시안 초기화를 먼저 수행한 다음 2D 확산 모델을 사용하여 최적화하는 순차적인 방식으로 구축됩니다.

![가우시안드리머의 전체 프레임워크. 먼저 3D 확산 모델을 활용하여 초기화된 포인트 클라우드를 생성합니다. 포인트 클라우드에 노이즈 점 증가와 색상 섭동을 실행한 후 3D 가우시안 초기화를 진행합니다. 초기화된 3D 가우스안은 2D 확산 모델에서 SDS 방법을 사용하여 추가로 최적화됩니다. 마지막으로 3D 가우시안 스플래팅[12]을 사용하여 3D 가우시안으로 이미지를 렌더링합니다.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%201.png)

가우시안드리머의 전체 프레임워크. 먼저 3D 확산 모델을 활용하여 초기화된 포인트 클라우드를 생성합니다. 포인트 클라우드에 노이즈 점 증가와 색상 섭동을 실행한 후 3D 가우시안 초기화를 진행합니다. 초기화된 3D 가우스안은 2D 확산 모델에서 SDS 방법을 사용하여 추가로 최적화됩니다. 마지막으로 3D 가우시안 스플래팅[12]을 사용하여 3D 가우시안으로 이미지를 렌더링합니다.

기본 개념:

확산 모델: '드림퓨전'은 2D와 3D 확산 모델을 연결하는 방식입니다. 2D 확산 모델 인사이트를 사용하여 3D 표현을 최적화하여 2D 모델의 이미지와 가장 유사한 이미지를 생성하는 것을 목표로 합니다. 반면에 'Shap-E' 방식은 3D 확산 모델을 표현하며 텍스트 프롬프트를 사용하여 3D 에셋을 빠르게 생성할 수 있습니다.

3D 가우시안 스플래팅: 새로운 관점에서 이미지를 생성하는 최첨단 기법입니다. 기존 방식과 달리 이 방식은 이방성 가우시안 세트를 사용하여 실시간으로 이미지를 스플래팅하여 생성합니다. 이러한 가우시안에는 중심 위치, 색상, 불투명도, 공분산과 같은 특정 속성이 있습니다.

프레임워크 살펴보기:
절차는 텍스트 입력을 기반으로 기본 3D 모양을 생성하는 "Shap-E" 메서드를 사용하여 3D 에셋을 초기화하는 것으로 시작됩니다. 그런 다음 이 모양을 포인트 집합으로 변환합니다. 이러한 포인트를 향상시키기 위해 "노이즈 포인트 성장"이라는 프로세스를 거치고 색상이 약간 변경됩니다. 이렇게 개선된 포인트를 사용하여 초기 3D 가우시안 포인트를 형성합니다. 그런 다음 2D 확산 모델을 사용하여 이 가우시안들을 개선하여 품질을 향상시킵니다.

3D 가우시안 초기화:
3D 확산 모델을 사용하여 제공된 텍스트로부터 3D 표현을 생성합니다. 이 3D 표현은 트라이앵글 메시로 변환되고, 이 메시를 다시 색상이 연결된 포인트 모음으로 변환합니다. 그러나 이러한 포인트와 색상은 드물고 단순한 경우가 많습니다.

![노이즈 점 증가 및 색상 섭동 과정의 그림.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%202.png)

노이즈 점 증가 및 색상 섭동 과정의 그림.

포인트 품질 향상: 초기 포인트의 희소성과 단순성을 해결하기 위해 원래 포인트 주위에 추가 포인트를 추가하고 색상을 약간 변경하여 보다 풍부한 표현을 제공합니다. 이 프로세스에는 초기 점 주위에 경계 상자를 계산한 다음 이 상자 내에 더 많은 점을 도입하는 작업이 포함됩니다. 이 새로운 점의 색상은 더 세밀하게 표현하기 위해 약간 변형됩니다.
3D 가우시안 최적화:
초기화 후 2D 확산 모델을 사용하여 시각적 효과를 높이기 위해 3D 가우시안 값을 향상시킵니다. 이를 위해 "스코어 증류 샘플링"(SDS) 기법이 사용됩니다. 가우시안 이미지를 이미지로 렌더링한 다음 2D 확산 모델의 샘플과 비교합니다. 가우시안 이미지를 2D 모델 샘플과 더 유사하게 만들기 위해 조정이 이루어집니다. 간단한 최적화 단계를 거친 후 2D 및 3D 확산 모델의 강점을 모두 활용하여 우수한 품질의 3D 에셋을 완성합니다.

### 4. Experiments

4.1. 구현 세부 사항:
제안된 방법은 파이토치 프레임워크와 쓰리스튜디오 소프트웨어를 사용하여 구축되었습니다. 이 방법은 Shap-E 3D 확산 모델을 사용하며 안정성AI/안정적확산-2-1-base로 알려진 2D 확산 모델을 사용합니다. 학습 속도 및 기타 세부 사항을 포함하여 3D 가우시안 최적화를 위한 특정 파라미터가 설정되어 있습니다. 모든 실험은 RTX 3090 그래픽 카드에서 수행되었으며 25분 이내에 완료되었습니다.

![우리의 방법과 Dreamfusion [23], Magic3D [16], Fantasia3D [4]의 질적 비교.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%203.png)

우리의 방법과 Dreamfusion [23], Magic3D [16], Fantasia3D [4]의 질적 비교.

![가우시안드리머에 의해 생성된 더 많은 샘플. 각 샘플의 두 가지 보기가 표시됩니다.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%204.png)

가우시안드리머에 의해 생성된 더 많은 샘플. 각 샘플의 두 가지 보기가 표시됩니다.

4.2. 시각화 결과:
이 방법의 결과를 기존 솔루션인 Magic3D1 및 Fantasia3D와 비교했습니다. 새로운 방법은 비슷한 품질의 결과를 얻을 수 있었을 뿐만 아니라 훈련 속도도 8~12배 빨랐습니다. 제안된 방법의 또 다른 장점은 실시간 렌더링이 가능하여 추가 변환이 필요하지 않다는 점입니다. 이 방법으로 생성된 시각화 샘플인 '가우시안드리머'는 일관된 3D 렌더링과 고품질 디테일을 보여주었습니다.

4.3. 제거 연구 및 분석:
3D 가우시안 초기화의 영향을 이해하기 위해 제거 연구를 수행했습니다. 연구 결과, 제안된 초기화 기법이 무작위 초기화에 비해 더 사실적이고 우수한 지오메트리를 생성하는 것으로 나타났습니다. 이 연구는 또한 노이즈 점 증가와 색상 섭동의 중요성을 강조하여 주어진 텍스트 프롬프트에 대한 디테일과 정렬을 크게 개선했습니다.

![3D 가우시안 초기화에 대한 제거 연구. 여기서 Shap-E[11] 렌더링 해상도는 256x256입니다.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%205.png)

3D 가우시안 초기화에 대한 제거 연구. 여기서 Shap-E[11] 렌더링 해상도는 256x256입니다.

![노이즈 점 성장 및 색상 섭동의 제거 연구. "Grow&Pertb."는 노이즈 점 증가 및 색상 섭동을 나타냅니다.](GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67/Untitled%206.png)

노이즈 점 성장 및 색상 섭동의 제거 연구. "Grow&Pertb."는 노이즈 점 증가 및 색상 섭동을 나타냅니다.

### 5. Conclusion

"가우시안 드리머"는 3D 및 2D 확산 모델을 모두 활용하고 가우시안 스플래팅 표현을 사용하여 텍스트를 3D 렌더링으로 효율적으로 변환하는 새로운 방법입니다. 이 접근 방식은 3D 일관성을 보장하면서 매우 세밀하고 사실적인 3D 모델을 생성합니다. 독특한 특징은 3D 확산 모델의 포인트 클라우드 프리오어를 사용하여 수렴 속도를 향상시킨다는 것입니다. 각 3D 샘플은 단일 GPU에서 단 25분 이내에 빠르게 생성됩니다. 향후 작업은 3D 확산 모델 전구체의 잠재력을 최대한 활용하여 더욱 강력한 지오메트리 일관성을 달성하는 데 초점을 맞출 예정입니다.