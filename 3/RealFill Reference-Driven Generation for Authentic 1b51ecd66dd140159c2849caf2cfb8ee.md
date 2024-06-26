# RealFill: Reference-Driven Generation for Authentic Image Completion

[https://arxiv.org/abs/2309.16668](https://arxiv.org/abs/2309.16668)

[https://realfill.github.io/](https://realfill.github.io/)

- Sep 2023

### 1. Introduction

사진은 우리 삶에서 찰나의 순간을 포착하는 강력한 도구입니다. 하지만 때때로 우리의 기억 전체를 담아내기에는 부족할 때가 있습니다. 예를 들어, 춤추는 아이의 거의 완벽한 사진에서 왕관과 같은 중요한 디테일이 프레임에서 살짝 벗어난 경우를 생각해 보세요. 다른 사진에서는 왕관이 잘 드러나지만 그 순간의 마법 같은 순간을 포착하지 못합니다. 이 백서에서는 이미지에서 누락된 부분을 원본 장면에 가깝게 재현하는 것을 목표로 하는 '진정한 이미지 완성'의 도전 과제에 대해 자세히 설명합니다.

![동일한 장면을 대략적으로 캡처한 몇 개의 참조 이미지와 누락된 영역이 있는 대상 이미지가 주어지면 RealFill은 실제 장면에 충실한 이미지 콘텐츠로 대상 이미지를 완성할 수 있습니다. 반면 표준 프롬프트 기반 인페인팅 방법은 원본 장면에 대한 지식이 부족하기 때문에 그럴듯하지만 실제와 다른 콘텐츠가 만들어집니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled.png)

동일한 장면을 대략적으로 캡처한 몇 개의 참조 이미지와 누락된 영역이 있는 대상 이미지가 주어지면 RealFill은 실제 장면에 충실한 이미지 콘텐츠로 대상 이미지를 완성할 수 있습니다. 반면 표준 프롬프트 기반 인페인팅 방법은 원본 장면에 대한 지식이 부족하기 때문에 그럴듯하지만 실제와 다른 콘텐츠가 만들어집니다.

한 장의 사진으로 완벽한 구도, 타이밍, 각도를 포착할 수 없는 경우가 많습니다. 이러한 이미지는 한 번 촬영하면 그 순간을 표현하는 것과 마찬가지로 변경할 수 없습니다. 특히 사용 가능한 참조 사진이 원근감, 조명 또는 움직이는 요소 측면에서 크게 다를 수 있는 경우 이러한 이미지를 완성하는 것이 과제입니다.

기존 솔루션은 매칭, 깊이 추정, 3D 변환에 중점을 둔 지오메트리 기반 기법을 사용했습니다. 하지만 이러한 방식은 복잡하거나 동적인 장면에서는 성능이 저하될 수 있습니다. 최근 제너레이티브 모델의 발전으로 이미지 인페인팅 작업에서 가능성을 보여주었습니다. 하지만 텍스트 프롬프트로만 안내할 때는 장면의 사실성을 유지하는 데 어려움을 겪기도 합니다.
우리는 참조 기반 이미지 완성을 위한 새로운 프레임워크인 'RealFill'을 도입했습니다. 이 프레임워크는 대상 이미지와 레퍼런스 이미지를 모두 사용하여 기존 인페인팅 모델을 미세 조정하는 것으로 시작됩니다. 이렇게 하면 모델이 장면의 콘텐츠, 조명, 스타일을 이해할 수 있습니다. 그런 다음 모델이 대상 이미지의 누락된 부분을 채웁니다. 생성된 콘텐츠의 품질을 향상시키기 위해 우리는 "대응 기반 시드 선택"을 사용합니다. 이 기술은 참조 이미지와 잘 일치하지 않는 샘플을 필터링하여 사람의 개입을 최소화하면서 고품질의 결과물을 보장합니다.

RealFill은 원본 콘텐츠를 보존하면서 대상 이미지를 인페인팅하거나 아웃페인팅하는 데 탁월한 성능을 발휘합니다. 다양한 시점이나 조명 조건과 같이 참조 이미지와 대상 이미지 간의 상당한 차이를 처리할 수 있습니다. 이 기능은 이전의 지오메트리 기반 방법과 차별화됩니다.

이미지 완성에 대한 기존 벤치마크는 단순한 작업에 초점을 맞추는 경우가 많습니다. 보다 까다로운 시나리오에서 RealFill의 성능을 평가하기 위해 우리는 "RealBench"를 도입했습니다. 이 데이터 세트는 인페인팅과 아웃페인팅 작업을 모두 포함하는 33개의 장면으로 구성되어 있습니다. 테스트 결과, 다양한 이미지 유사성 메트릭에서 RealFill이 다른 방법보다 훨씬 뛰어난 성능을 보였습니다.

이 논문은 사실적인 이미지 완성 문제에 대한 새로운 관점을 제시합니다. 우리는 이미지의 누락된 부분을 원본 장면에 사실적인 방식으로 채우는 것을 목표로 합니다. 우리의 솔루션인 리얼필은 제너레이티브 인페인팅 모델의 성능을 활용하여 레퍼런스 이미지로 모델을 향상시켜 이 목표를 달성합니다. 또한 리얼벤치 도입으로 이러한 방법을 평가할 수 있는 강력한 수단을 제공함으로써 이 분야가 의미 있는 방식으로 계속 발전할 수 있도록 지원합니다.

### 2. Related Work

이미지의 누락된 부분을 그럴듯한 콘텐츠로 채우는 기술인 이미지 완성은 컴퓨터 비전 분야에서 오랫동안 해결해야 할 과제였습니다. 이 백서에서는 확산 모델, 전통적인 이미지 완성 방법 및 참조 기반 인페인팅의 사용에 중점을 두고 이 영역에서 이루어진 다양한 기술과 발전에 대해 자세히 설명합니다.

확산 모델은 텍스트-이미지(T2I) 생성의 강력한 도구로 부상했습니다. 뛰어난 성능으로 인해 연구자들은 사전 학습된 모델을 다양한 전문 작업에 적용하고 있습니다. 예를 들어, 일부 방법은 특정 개체나 스타일을 기반으로 이미지를 생성하기 위해 T2I 모델을 개인화하는 데 중점을 둡니다. 또 다른 방법은 모델을 수정하여 새로운 컨디셔닝 신호를 추가함으로써 보다 제어된 이미지 생성 또는 편집을 가능하게 합니다. 이러한 조정은 텍스트에서 3D로 생성하거나 T2I 모델을 제너레이티브 비디오 모델로 변환하는 등의 작업으로 확장되었습니다. 이러한 모델의 적응성은 참조 기반 이미지 완성과 같은 작업에서 그 잠재력을 보여줍니다.

역사적으로 이미지 완성(흔히 인페인팅 또는 아웃페인팅이라고도 함)은 수작업에 의한 휴리스틱에 의존했습니다. 하지만 딥러닝의 등장으로 패러다임이 엔드투엔드 신경망으로 바뀌었습니다. 이러한 네트워크는 원본 이미지와 마스크를 입력으로 받아 완성된 이미지를 생성합니다. 이 작업의 복잡성을 고려할 때, 최근의 많은 방법들은 사전 학습된 생성 모델을 활용하고 있습니다. 확산 기반 솔루션은 텍스트 기반 이미지 완성에서 가능성을 보였지만, 텍스트 프롬프트에 의존하기 때문에 제어에 어려움을 겪는 경우가 많습니다. 이러한 한계를 해결하는 것이 저희 연구의 주요 초점입니다.

참조 기반 인페인팅 기법은 참조 이미지를 활용하여 완성 과정을 안내합니다. 이 영역의 기존 방법에는 깊이 추정, 이미지 왜곡, 조화 등 복잡한 파이프라인이 포함됩니다. 그러나 이러한 파이프라인은 특히 복잡한 장면이나 모양이 변경되는 까다로운 시나리오에서 오류가 발생하기 쉽습니다. 주목할 만한 접근 방식인 '예제별 페인트'는 확산 모델을 미세 조정하여 참조 이미지와 대상 이미지 모두에 대해 생성 조건을 지정합니다. 하지만 이 방법은 단일 참조 이미지에서 높은 수준의 의미 정보만 캡처합니다. 우리가 제안한 방법은 여러 개의 레퍼런스 이미지를 활용하여 외관이 크게 변하는 상황에서도 원본 장면에 충실한 시각적으로 매력적인 결과를 얻을 수 있다는 점이 특징입니다.

이미지 완성 기술의 여정은 수작업 방식에서 정교한 딥러닝 모델로 전환하는 과정을 거쳤습니다. 확산 모델은 상당한 발전을 가져왔지만, 제어와 진위 여부를 확인하는 것은 여전히 어려운 과제입니다. 이번 조사에서는 여러 참조 이미지를 사용하여 설득력 있고 사실적인 이미지 완성 결과를 얻을 수 있는 잠재력을 강조했습니다.

### 3. Method

이미지 완성은 이미지의 누락된 부분을 채워 전체 이미지를 완성하는 작업입니다. 채워진 콘텐츠가 사실적일 뿐만 아니라 원본 장면과 일치하도록 하는 것이 과제입니다. 제안된 방법인 'RealFill'은 참조 이미지를 사용하여 완성 프로세스를 안내함으로써 이를 달성하는 것을 목표로 합니다.

레퍼런스 이미지 세트(최대 5개)를 사용하여 유사한 장면을 묘사하는 대상 이미지를 완성하는 것이 목표입니다. 완성된 이미지는 사실적으로 보일 뿐만 아니라 참조 이미지에 있는 콘텐츠에 충실해야 합니다. 이를 "실제 이미지 완성"이라고 합니다. 레퍼런스 이미지와 타깃 이미지가 시점, 조명, 스타일, 심지어 동적 요소까지 크게 다를 수 있기 때문에 이 과제는 더욱 어려워집니다.

확산 모델은 기본 배포를 타깃 데이터 배포로 변환하는 생성 툴입니다. 이 과정에는 깨끗한 데이터 포인트에 다양한 수준의 노이즈를 추가한 다음 신경망을 훈련하여 이 노이즈를 예측하는 과정이 포함됩니다. '드림부스' 기술은 텍스트-이미지(T2I) 확산 모델을 위해 이 프로세스를 개선하여 의미적 수정을 통해 특정 피사체를 생성할 수 있도록 합니다. '낮은 순위 적응(LoRA)'을 통합함으로써 모델은 훈련 중에 특정 매개변수만 업데이트하여 메모리 효율성이 향상됩니다.

이 작업에는 참조 이미지와 누락된 영역이 있는 대상 이미지를 사용하는 것이 포함됩니다. 목표는 마스크가 0인 경우 원본 콘텐츠를 유지하고 마스크가 1인 경우 참조 이미지와 일치하는 조화로운 출력 이미지를 생성하는 것입니다. 참조 이미지와 대상 이미지가 충분히 겹쳐야 그럴듯하게 완성될 것으로 예상됩니다.

지오메트리 기반과 재구성 기반의 기존 방법은 기하학적 제약이 없고 입력 이미지가 제한적이기 때문에 이 작업에서 어려움을 겪습니다. 제안된 솔루션인 'RealFill'은 참조 이미지의 지식을 사용하여 사전 학습된 생성 모델을 미세 조정합니다. 이렇게 하면 완성된 이미지를 생성할 때 모델이 장면의 콘텐츠를 인식할 수 있습니다.

훈련은 최첨단 T2I 확산 인페인팅 모델로 시작됩니다. 그런 다음 이 모델은 무작위로 생성된 바이너리 마스크와 함께 참조 이미지와 대상 이미지를 모두 사용하여 미세 조정됩니다. 학습이 완료되면 모델이 이미지를 생성한 다음 원본 대상 이미지와 조화를 이루어 최종 결과물을 생성합니다.

![RealFill - 학습 및 추론 파이프라인. 이 방법의 입력은 채울 대상 이미지와 동일한 장면의 참조 이미지 몇 개입니다. 먼저 레퍼런스 이미지와 타깃 이미지에 대해 사전 훈련된 인페인팅 확산 모델의 LoRA 가중치를 미세 조정합니다(무작위 패치는 마스킹된 상태로). 그런 다음 조정된 모델을 사용하여 목표 이미지의 원하는 영역을 채우면, 예를 들어 춤추는 소녀의 왕관이 레퍼런스 이미지와 비교할 때 포즈와 관절이 상당히 다른데도 불구하고 목표 이미지에서는 소녀의 왕관이 복원되는 등 충실하고 고품질의 결과물을 얻을 수 있습니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%201.png)

RealFill - 학습 및 추론 파이프라인. 이 방법의 입력은 채울 대상 이미지와 동일한 장면의 참조 이미지 몇 개입니다. 먼저 레퍼런스 이미지와 타깃 이미지에 대해 사전 훈련된 인페인팅 확산 모델의 LoRA 가중치를 미세 조정합니다(무작위 패치는 마스킹된 상태로). 그런 다음 조정된 모델을 사용하여 목표 이미지의 원하는 영역을 채우면, 예를 들어 춤추는 소녀의 왕관이 레퍼런스 이미지와 비교할 때 포즈와 관절이 상당히 다른데도 불구하고 목표 이미지에서는 소녀의 왕관이 복원되는 등 충실하고 고품질의 결과물을 얻을 수 있습니다.

확산 추론 프로세스는 입력 시드에 따라 다양한 결과를 생성할 수 있습니다. 고품질 출력을 보장하기 위해 "대응 기반 시드 선택" 방법이 도입되었습니다. 이 방법은 출력 이미지와 기준 이미지 사이의 이미지 특징 대응 수를 지표로 사용하여 출력의 충실도를 측정합니다. 이 방법은 일치하는 키포인트를 기준으로 생성된 결과의 순위를 매김으로써 고품질의 소수의 결과 세트로 자동 필터링할 수 있으므로 수동 선택의 필요성을 줄여줍니다.

'리얼필'은 레퍼런스 이미지와 확산 모델을 활용하여 사실적인 이미지를 완성하는 새로운 접근 방식을 제시합니다. 대응 기반 시드 선택 방법을 도입하여 사람의 개입을 최소화하면서 고품질의 사실적이고 충실한 이미지 완성을 보장합니다.

### 4. Experiments

그림 3과 4에서 RealFill의 기능을 확인할 수 있으며, 이 기능은 참조 이미지에 충실한 방식으로 이미지를 성공적으로 완성합니다. 다양한 카메라 각도, 조명 조건, 피사체 포즈와 같은 문제에도 불구하고 RealFill은 사전 학습된 확산 모델의 기본 지식과 미세 조정을 통해 얻은 장면별 인사이트 덕분에 탁월한 성능을 발휘합니다.

![그림3 RealFill을 사용한 레퍼런스 기반 아웃페인팅. 왼쪽의 참조 이미지가 주어지면 RealFill은 오른쪽의 해당 대상 이미지에 아웃페인팅할 수 있습니다. 흰색 상자 안쪽 영역은 알려진 픽셀로 네트워크에 제공되고 흰색 상자 바깥쪽 영역은 모두 생성됩니다. 결과 결과 RealFill은 시점, 조리개, 조명, 이미지 스타일, 물체의 움직임 등 레퍼런스와 타깃 간에 극적인 차이가 있는 경우에도 레퍼런스에 충실한 고품질 이미지를 생성하는 것으로 나타났습니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%202.png)

그림3 RealFill을 사용한 레퍼런스 기반 아웃페인팅. 왼쪽의 참조 이미지가 주어지면 RealFill은 오른쪽의 해당 대상 이미지에 아웃페인팅할 수 있습니다. 흰색 상자 안쪽 영역은 알려진 픽셀로 네트워크에 제공되고 흰색 상자 바깥쪽 영역은 모두 생성됩니다. 결과 결과 RealFill은 시점, 조리개, 조명, 이미지 스타일, 물체의 움직임 등 레퍼런스와 타깃 간에 극적인 차이가 있는 경우에도 레퍼런스에 충실한 고품질 이미지를 생성하는 것으로 나타났습니다.

![그림4 RealFill을 사용한 레퍼런스 기반 인페인팅. 왼쪽의 레퍼런스 이미지가 주어졌을 때 RealFill은 타깃 이미지에서 원하지 않는 오브젝트를 제거하고 가려진 내용을 충실하게 드러낼 수 있을 뿐만 아니라(왼쪽 열), 레퍼런스 이미지와 타깃 이미지 간의 시점이 크게 달라져도 오브젝트를 장면에 삽입할 수 있습니다(오른쪽 열). 왼쪽 아래 예시에서는 레퍼런스 이미지와 타깃 이미지의 조리개 값도 다르지만 RealFill은 머그잔 뒤에 있는 건물을 복원할 뿐만 아니라 타깃 이미지에서 보이는 블러도 적절하게 유지합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%203.png)

그림4 RealFill을 사용한 레퍼런스 기반 인페인팅. 왼쪽의 레퍼런스 이미지가 주어졌을 때 RealFill은 타깃 이미지에서 원하지 않는 오브젝트를 제거하고 가려진 내용을 충실하게 드러낼 수 있을 뿐만 아니라(왼쪽 열), 레퍼런스 이미지와 타깃 이미지 간의 시점이 크게 달라져도 오브젝트를 장면에 삽입할 수 있습니다(오른쪽 열). 왼쪽 아래 예시에서는 레퍼런스 이미지와 타깃 이미지의 조리개 값도 다르지만 RealFill은 머그잔 뒤에 있는 건물을 복원할 뿐만 아니라 타깃 이미지에서 보이는 블러도 적절하게 유지합니다.

평가 데이터 세트: 현재 이미지 완성도에 대한 벤치마크는 대부분 사소한 조정에 초점을 맞추고 있습니다. 리얼필의 기능을 더 잘 평가하기 위해 새로운 데이터 세트인 리얼벤치(RealBench)를 만들었습니다. 이 데이터 세트는 참조 이미지, 대상 이미지, 누락된 영역을 나타내는 마스크, 실제 완성된 이미지가 각각 포함된 33개의 장면으로 구성됩니다. 이러한 장면에는 조명 변화부터 피사체 포즈까지 다양한 문제가 있습니다.

평가 지표: 모델 결과물의 품질을 측정하기 위해 여러 지표를 사용했습니다. 이러한 지표는 PSNR 및 SSIM과 같은 낮은 수준의 이미지 유사성 측정부터 CLIP 및 DINO 임베딩을 사용한 높은 수준의 지표까지 다양했습니다. 또한 이미지 레이아웃과 콘텐츠의 차이를 강조하기 위해 DreamSim을 사용했습니다.

기준선 비교: RealFill은 다른 두 가지 방법과 비교되었습니다: 예제별 페인트와 안정적인 확산 페인팅입니다. 전자는 하나의 참조 이미지만 사용하지만 후자는 장면을 설명하는 자세한 프롬프트가 필요합니다. 공정한 평가를 위해 각 방법에 성공할 수 있는 최상의 조건을 부여했습니다.

![RealFill과 기준선 방법의 비교. 투명한 흰색 마스크가 대상 이미지의 변경되지 않은 알려진 영역에 오버레이됩니다. 예제별 페인트는 높은 수준의 시맨틱 정보만 캡처하는 클립 임베딩에 의존하기 때문에 레퍼런스 이미지와의 충실도가 떨어집니다. 안정적인 확산 페인팅은 그럴듯한 결과를 생성하지만 프롬프트의 표현력이 제한되어 있기 때문에 레퍼런스 이미지와 일치하지 않습니다. 반면, 리얼필은 레퍼런스 이미지에 대한 충실도가 높은 고품질 결과를 생성합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%204.png)

RealFill과 기준선 방법의 비교. 투명한 흰색 마스크가 대상 이미지의 변경되지 않은 알려진 영역에 오버레이됩니다. 예제별 페인트는 높은 수준의 시맨틱 정보만 캡처하는 클립 임베딩에 의존하기 때문에 레퍼런스 이미지와의 충실도가 떨어집니다. 안정적인 확산 페인팅은 그럴듯한 결과를 생성하지만 프롬프트의 표현력이 제한되어 있기 때문에 레퍼런스 이미지와 일치하지 않습니다. 반면, 리얼필은 레퍼런스 이미지에 대한 충실도가 높은 고품질 결과를 생성합니다.

리얼필 구현: 모든 장면에서 일관된 하이퍼파라미터를 사용하여 2,000번의 반복을 통해 모델을 미세 조정했습니다. 이러한 일관성을 통해 공정한 비교가 가능했습니다.

정량적 비교: 표 1에서 볼 수 있듯이 모든 유사성 메트릭에서 RealFill이 기준 메서드를 능가했습니다. 이러한 정량적 우위는 우수한 성능을 입증합니다.

정성적 비교: 그림 5는 RealFill과 베이스라인을 시각적으로 대조합니다. RealFill의 출력은 높은 품질을 보여줄 뿐만 아니라 원본 장면을 더 정확하게 반영합니다. 예제별 페인트는 복잡한 장면에서 어려움을 겪는 반면, 안정적인 확산 인페인팅은 그럴듯한 이미지를 생성하지만 언어 프롬프트의 한계로 인해 원본 장면에서 벗어나는 경우가 많습니다.

대응 기반 시드 선택: 리얼필의 독자적인 시드 선택 메커니즘을 평가했습니다. 일치하는 키포인트를 기반으로 출력의 순위를 매기고 순위가 낮은 샘플을 필터링함으로써 모델의 성능이 크게 향상되었습니다. 그림 6은 이를 더욱 잘 보여주는데, 일반적으로 일치하는 키포인트 수가 적을수록 결과가 좋지 않음을 나타냅니다.

![그림6 RealFill을 사용한 레퍼런스 기반 인페인팅. 왼쪽의 레퍼런스 이미지가 주어졌을 때 RealFill은 타깃 이미지에서 원하지 않는 오브젝트를 제거하고 가려진 내용을 충실하게 드러낼 수 있을 뿐만 아니라(왼쪽 열), 레퍼런스 이미지와 타깃 이미지 간의 시점이 크게 달라져도 오브젝트를 장면에 삽입할 수 있습니다(오른쪽 열). 왼쪽 아래 예시에서는 레퍼런스 이미지와 타깃 이미지의 조리개 값도 다르지만 RealFill은 머그잔 뒤에 있는 건물을 복원할 뿐만 아니라 타깃 이미지에서 보이는 블러도 적절하게 유지합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%205.png)

그림6 RealFill을 사용한 레퍼런스 기반 인페인팅. 왼쪽의 레퍼런스 이미지가 주어졌을 때 RealFill은 타깃 이미지에서 원하지 않는 오브젝트를 제거하고 가려진 내용을 충실하게 드러낼 수 있을 뿐만 아니라(왼쪽 열), 레퍼런스 이미지와 타깃 이미지 간의 시점이 크게 달라져도 오브젝트를 장면에 삽입할 수 있습니다(오른쪽 열). 왼쪽 아래 예시에서는 레퍼런스 이미지와 타깃 이미지의 조리개 값도 다르지만 RealFill은 머그잔 뒤에 있는 건물을 복원할 뿐만 아니라 타깃 이미지에서 보이는 블러도 적절하게 유지합니다.

엄격한 실험을 통해 리얼필은 사실적인 이미지를 완성하는 데 있어 뛰어난 성능을 입증했습니다. 까다로운 조건에서도 고품질의 장면에 충실한 이미지를 생성하는 능력은 다른 방법과 차별화됩니다. 혁신적인 대응 기반 시드 선택은 성능을 더욱 향상시켜 최고 수준의 결과물을 생성할 수 있도록 보장합니다.

### 5. Discussion

대안적 접근 방식: 대안적 접근법: 성능이 우수할까요?

이미지 스티칭: 직접적인 방법으로는 참조 이미지와 타깃 이미지를 대응 관계에 따라 스티칭하는 방법이 있습니다. 그러나 이 방법은 특히 시점, 조명 또는 움직이는 물체에 큰 변화가 있을 때 종종 실패합니다. 예를 들어, 그림 7에서 많은 상용 소프트웨어 솔루션이 불충분한 대응으로 인해 만족스러운 결과를 얻지 못한 반면, RealFill은 장면을 사실적이고 정확하게 재현할 수 있었습니다.

![그림7 RealFill과 기준선 방법의 비교. 투명한 흰색 마스크가 대상 이미지의 변경되지 않은 알려진 영역에 오버레이됩니다. 예제별 페인트는 높은 수준의 시맨틱 정보만 캡처하는 클립 임베딩에 의존하기 때문에 레퍼런스 이미지와의 충실도가 떨어집니다. 안정적인 확산 페인팅은 그럴듯한 결과를 생성하지만 프롬프트의 표현력이 제한되어 있기 때문에 레퍼런스 이미지와 일치하지 않습니다. 반면, 리얼필은 레퍼런스 이미지에 대한 충실도가 높은 고품질 결과를 생성합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%206.png)

그림7 RealFill과 기준선 방법의 비교. 투명한 흰색 마스크가 대상 이미지의 변경되지 않은 알려진 영역에 오버레이됩니다. 예제별 페인트는 높은 수준의 시맨틱 정보만 캡처하는 클립 임베딩에 의존하기 때문에 레퍼런스 이미지와의 충실도가 떨어집니다. 안정적인 확산 페인팅은 그럴듯한 결과를 생성하지만 프롬프트의 표현력이 제한되어 있기 때문에 레퍼런스 이미지와 일치하지 않습니다. 반면, 리얼필은 레퍼런스 이미지에 대한 충실도가 높은 고품질 결과를 생성합니다.

바닐라 드림부스: 또 다른 접근 방식은 레퍼런스 이미지에서 표준 안정 확산 모델을 미세 조정한 다음 이 모델을 인페인팅에 사용하는 것입니다. 그림 8에서 볼 수 있듯이 이 방법은 디퓨저 라이브러리에서 인기가 있지만 RealFill에 비해 성능이 떨어집니다.

![그림8 참조 이미지에서 표준 안정 확산 모델을 미세 조정하여 누락된 영역을 채우는 데 사용하는 바닐라 드림부스는 리얼필에 비해 현저히 떨어지는 결과를 가져옵니다. 강도 하이퍼 파라미터의 다양한 수준을 사용하여 다양한 샘플을 보여줍니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%207.png)

그림8 참조 이미지에서 표준 안정 확산 모델을 미세 조정하여 누락된 영역을 채우는 데 사용하는 바닐라 드림부스는 리얼필에 비해 현저히 떨어지는 결과를 가져옵니다. 강도 하이퍼 파라미터의 다양한 수준을 사용하여 다양한 샘플을 보여줍니다.

리얼필의 강점

씬 컴포지션: RealFill은 장면 내의 여러 요소를 이해하고 연관시키는 고유한 기능을 가지고 있습니다. 빈 캔버스를 조건으로 설정하면 그림 9와 같이 다양한 씬 구조를 생성할 수 있어 씬의 구성을 이해할 수 있는 능력을 보여줍니다.

![예를 들어 첫 번째 줄과 두 번째 줄에 사람이 추가되거나 제거되는 등 빈 이미지를 입력으로 조건화할 경우 RealFill은 여러 장면 변형을 생성할 수 있습니다. 이는 미세 조정된 모델이 장면 내부의 요소들을 구성적인 방식으로 연관시킬 수 있음을 시사합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%208.png)

예를 들어 첫 번째 줄과 두 번째 줄에 사람이 추가되거나 제거되는 등 빈 이미지를 입력으로 조건화할 경우 RealFill은 여러 장면 변형을 생성할 수 있습니다. 이는 미세 조정된 모델이 장면 내부의 요소들을 구성적인 방식으로 연관시킬 수 있음을 시사합니다.

대응 캡처: 리얼필은 서로 다른 장면을 묘사하는 경우에도 레퍼런스 이미지의 콘텐츠를 타깃에 원활하게 통합할 수 있습니다. 이는 그림 10에서 강조된 바와 같이 사전 학습된 안정적 확산 모델을 사용한 이전 작업에서도 관찰된 현상인 이미지 간의 실제 및 가상 대응을 모두 식별하고 활용하는 데 능숙하다는 것을 나타냅니다.

![그림 10 참조 이미지와 목표 이미지가 동일한 장면을 묘사하지 않는 경우에도 미세 조정된 모델은 의미적으로 합리적인 방식으로 참조 콘텐츠를 목표 이미지에 융합할 수 있으며, 이는 입력 이미지 간의 실제 또는 가상의 대응을 모두 포착할 수 있음을 시사합니다.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%209.png)

그림 10 참조 이미지와 목표 이미지가 동일한 장면을 묘사하지 않는 경우에도 미세 조정된 모델은 의미적으로 합리적인 방식으로 참조 콘텐츠를 목표 이미지에 융합할 수 있으며, 이는 입력 이미지 간의 실제 또는 가상의 대응을 모두 포착할 수 있음을 시사합니다.

리얼필은 강점에도 불구하고 한계가 있습니다. 이 모델은 입력 이미지에 대한 그라데이션 기반 미세 조정 프로세스가 필요하기 때문에 상대적으로 느리고 실시간 애플리케이션에는 적합하지 않습니다. 또한 레퍼런스 이미지와 타깃 이미지 간에 시점이 크게 바뀌는 경우, 특히 레퍼런스 이미지가 하나만 있는 경우 RealFill이 3D 장면을 정확하게 재현하지 못할 수 있습니다. 그림 11의 예를 보면 허스키의 포즈가 레퍼런스와 다른 것을 알 수 있습니다. 또한 RealFill은 사전 학습된 기본 모델의 이전 이미지에 의존하기 때문에 기본 모델의 약점을 그대로 이어받습니다. 예를 들어, 텍스트나 얼굴 특징과 같은 복잡한 이미지 디테일을 생성하는 데 어려움을 겪습니다. 이러한 한계는 상점 간판의 철자가 틀린 그림 11에서 잘 드러납니다.

![그림 11 (위) 출력된 허스키 플러시 인형의 포즈가 레퍼런스와 다른 경우와 같이 정확한 3D 장면 구조를 복구하지 못하는 경우, (아래) 텍스트와 같이 기본 T2I 모델에서도 처리하기 어려운 경우를 처리하는 데 실패하는 경우.](RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee/Untitled%2010.png)

그림 11 (위) 출력된 허스키 플러시 인형의 포즈가 레퍼런스와 다른 경우와 같이 정확한 3D 장면 구조를 복구하지 못하는 경우, (아래) 텍스트와 같이 기본 T2I 모델에서도 처리하기 어려운 경우를 처리하는 데 실패하는 경우.

RealFill은 이미지 완성에 대한 유망한 접근 방식을 제공하며 다양한 시나리오에서 다른 방법보다 뛰어난 성능을 발휘합니다. 장면 구성을 이해하고 일치하는 부분을 캡처하는 기능이 차별화됩니다. 그러나 특히 시점이 크게 바뀌거나 복잡한 디테일이 있는 실시간 애플리케이션과 시나리오에서는 그 한계를 인식하는 것이 중요합니다.

### 6. Societal Impact

이 연구의 주요 목표는 사용자의 창의적인 표현력을 향상시키고 사진의 품질을 향상시키는 도구를 제공하는 것입니다. 그러나 고급 이미지 생성의 영역에는 사회적 파급 효과가 없는 것은 아닙니다. 우리와 같은 도구는 혁신적이지만, 특히 민감한 개인 속성을 수정할 수 있는 기능이 있는 경우 내재된 우려를 수반합니다. 우리가 사용하는 기본 모델인 '안정적 확산'도 이러한 우려에서 자유롭지 않습니다. 조사 결과, 트위터의 방식이 편향적이거나 유해한 콘텐츠를 생산하기 쉽다는 사실은 밝혀지지 않았지만, 경계를 늦추지 않는 것이 중요합니다. 광범위한 과학계는 편견과 잠재적 피해를 줄이기 위한 연구에 우선순위를 두고 이러한 이미지 생성 도구가 책임감 있게 사용될 수 있도록 해야 합니다.

### 7. Conclusion

이 연구에서는 사실적인 이미지 완성이라는 개념을 소개했습니다. 이 개념의 목표는 가능한 대안을 상상하기보다는 장면이 원래 어떻게 생겼는지에 초점을 맞춰 참조 이미지를 기반으로 이미지의 누락된 부분을 채우는 것입니다. 우리의 솔루션인 RealFill은 주어진 이미지에 텍스트-투-이미지(T2I) 인페인팅 확산 모델을 적용한 다음 이 정제된 모델을 완성 작업에 사용하여 이를 달성합니다. 결과는 조명에서 오브젝트 위치에 이르기까지 참조 이미지와 대상 이미지가 다양한 측면에서 크게 다른 경우에도 RealFill이 충실도 높은 이미지 완성을 생성할 수 있음을 보여줍니다.