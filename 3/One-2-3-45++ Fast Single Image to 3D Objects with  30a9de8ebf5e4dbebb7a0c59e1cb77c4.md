# One-2-3-45++: Fast Single Image to 3D Objects with Consistent Multi-View Generation and 3D Diffusion

[https://sudo-ai-3d.github.io/One2345plus_page/](https://sudo-ai-3d.github.io/One2345plus_page/)

[https://arxiv.org/abs/2311.07885](https://arxiv.org/abs/2311.07885)

- Nov 2023

### 1. Introduction

이 논문에서는 수많은 애플리케이션이 있는 컴퓨터 비전에서 중요한 과제인 단일 이미지 또는 텍스트 프롬프트에서 3D 모양을 생성하는 발전된 방법에 대해 설명합니다. 2D 이미지 생성은 고급 방법과 방대한 데이터 세트를 통해 발전해 왔습니다. 그러나 사용 가능한 3D 훈련 데이터가 제한되어 있기 때문에 이러한 성공이 3D 생성으로 완전히 이전되지는 않았습니다. 기존의 많은 3D 모델은 소규모의 공개 3D 데이터 세트에 의존하기 때문에 실제 시나리오에서 보이지 않는 범주를 처리하는 데 어려움을 겪습니다.

드림퓨전(DreamFusion), Magic3D와 같은 일부 최신 접근 방식은 3D 표현을 생성하기 위해 기존 2D 모델을 활용합니다. 이러한 방법은 2D 모델을 사용하여 최적화 프로세스를 안내하는 2D 이미지로 3D 표현(예: NeRF 또는 메시)을 렌더링합니다. 이러한 방법은 효과적이기는 하지만 시간이 많이 소요될 수 있으며, 하나의 3D 형상을 생성하는 데 몇 시간이 걸리는 경우가 많습니다. 또한 '다중 얼굴' 문제, 채도 및 아티팩트와 같은 문제가 발생할 수 있습니다. 또한 서로 다른 무작위 시드를 사용할 때 일관성을 유지하는 데 어려움을 겪습니다.

최근의 혁신 기술인 One-2-3-45는 2D 확산 모델을 사용하여 다중 뷰 이미지를 예측한 다음 일반화 가능한 NeRF 방법을 사용하여 3D 모양으로 재구성함으로써 이 프로세스를 개선하려고 시도했습니다. 이 방법은 더 빠르지만 멀티뷰 이미지의 예측이 일관되지 않아 3D 재구성의 품질에 영향을 미치는 한계에 직면했습니다.

이 논문에서는 이러한 문제를 해결하는 새로운 방법인 One-2-3-45++를 소개합니다. 이 방법은 멀티뷰 2D 생성 및 3D 재구성의 두 단계로 구성됩니다. 첫 번째 단계는 6개의 뷰 이미지를 하나로 타일링하고 2D 확산 모델을 미세 조정하여 일관된 멀티뷰 이미지를 동시에 예측하는 것입니다. 이 접근 방식은 여러 뷰에 걸쳐 일관성을 보장합니다. 두 번째 단계에서는 디테일하고 텍스처가 있는 메시 예측을 위해 멀티뷰 조건부 3D 디퓨전 기반 모듈을 사용합니다. One-2-3-45++에는 일관된 멀티뷰 이미지를 가이드로 사용하여 텍스처 품질을 개선하는 경량 최적화 프로세스도 포함되어 있습니다.

One-2-3-45++는 1분 이내에 디테일한 텍스처의 사실적인 3D 메시를 생성하는 뛰어난 효율성이 돋보입니다. 생성 프로세스를 정밀하게 제어할 수 있습니다. 사용자 연구와 객관적인 지표를 포함한 종합적인 평가를 통해 견고성, 시각적 품질, 입력 이미지에 대한 충실도 면에서 우수성이 입증되었습니다. 이 방법은 2D 입력에서 3D 형상을 생성하는 분야에서 상당한 발전을 이루었습니다.

2

이 백서의 두 번째 섹션에서는 컴퓨터 비전에서 중요한 분야인 3D 생성 및 희소 뷰 재구성과 관련된 작업을 검토합니다.

3D 생성
3D 생성의 초기에는 포인트 클라우드, 복셀, 폴리곤 메시, 파라메트릭 모델, 암시적 필드와 같은 다양한 3D 표현을 생성하기 위해 합성 데이터 또는 실제 스캔에서 학습하는 네이티브 3D 생성 모델에 중점을 두었습니다. 하지만 3D 데이터 가용성이 제한되어 있어 이러한 모델은 의자, 자동차, 비행기, 사람 등 특정 카테고리에만 국한되는 경우가 많아 보이지 않는 카테고리로 일반화하는 데 한계가 있었습니다.

고급 2D 제너레이티브 모델(예: DALLE, Imagen, Stable Diffusion)과 비전 언어 모델(예: CLIP)의 도입은 3D 세계에 대한 강력한 인사이트를 제공했습니다. 그 결과 3D 생성에 혁신적인 접근 방식이 도입되었으며, 각각의 고유한 입력에 맞게 3D 표현을 최적화하는 DreamFusion, Magic3D, ProlificDreamer와 같은 모델이 대표적인 예입니다. 이러한 방법은 인상적인 결과에도 불구하고 긴 최적화 시간, 채도 문제, 다양성 부족이라는 문제에 직면했습니다. 일부는 2D 모델을 사용하여 입력 메시의 텍스처나 머티리얼을 생성하는 데 집중했습니다.

Zero123과 같은 최근 연구에서는 3D 생성을 위해 단일 이미지 또는 텍스트로부터 새로운 뷰를 합성할 때 사전 학습된 2D 확산 모델의 잠재력을 입증했습니다. 예를 들어, Zero123의 다중 뷰 이미지를 사용하는 One-2-3-45는 텍스처가 있는 3D 메시를 빠르게 생성할 수 있습니다. 하지만 3D 뷰의 일관성이 문제가 되었습니다. 이 논문은 다른 연구와 함께 이러한 멀티뷰 이미지의 일관성을 개선하여 더 나은 3D 재구성을 목표로 합니다.

스파스 뷰 재구성
기존의 3D 재구성 방법에서는 정확한 지오메트리 추론을 위해 많은 입력 이미지가 필요했습니다. 반면, 일반화 가능한 최신 NeRF 솔루션은 몇 장의 이미지로 새로운 장면으로 일반화하여 NeRF를 추론할 수 있습니다. 이러한 방법은 적은 수의 소스 뷰를 사용하고 2D 네트워크를 사용하여 특징을 추출한 다음 3D 공간으로 변환하여 밀도와 색상을 추론합니다. 여기서 문제는 정확한 대응을 가진 일관된 멀티뷰 이미지에 의존하거나 훈련 데이터 세트를 넘어 일반화할 수 있는 능력이 제한된다는 점입니다.

최근의 접근 방식은 확산 모델을 통합하여 희소 뷰 재구성을 지원합니다. 이러한 방법은 일반적으로 새로운 뷰 합성으로 문제에 접근하며 3D 콘텐츠를 생성하기 위해 추가 처리가 필요할 수 있습니다. 이 논문의 접근 방식은 다중 뷰 조건부 3D 확산 모델을 사용하여 3D 데이터 선행에서 직접 3D를 생성하므로 후처리가 필요하지 않습니다. 동시 연구에서는 재구성을 위해 특수 손실 함수와 함께 NeRF 기반 최적화를 사용합니다.

요약하자면, 이 논문은 3D 생성 및 재구성의 최근 발전과 과제에 대한 광범위한 맥락에서 2D 생성 모델 사용으로의 전환과 효과적인 3D 재구성을 위한 멀티뷰 이미지 생성의 일관성 개선의 필요성을 강조하며, 이 작업을 설명합니다.

3

이 백서의 방법 섹션에서는 One-2-3-45++ 모델을 사용하여 3D 콘텐츠를 제작하는 프로세스를 설명하며 컨셉 아트, 3D 모델링, 텍스처링이 포함된 기존 게임 스튜디오 워크플로와 유사점을 제시합니다.

3.1. 일관된 멀티뷰 생성
첫 번째 단계는 콘셉트 아티스트의 역할과 유사하게 단일 입력 이미지에서 오브젝트의 일관된 멀티뷰 이미지를 생성하는 것입니다. 이 작업은 사전 훈련된 2D 확산 모델을 미세 조정하여 수행됩니다. 제로123(Zero123)이라는 모델은 단일 이미지에서 새로운 뷰를 합성하는 데는 잠재력을 보였지만, 여러 뷰에 걸쳐 일관성을 유지하는 데는 어려움을 겪었습니다. 이 문제를 해결하기 위해 저자는 6개의 희박한 뷰 세트를 단일 이미지로 타일링하고 2D 확산 네트워크를 미세 조정하여 이 합성 이미지를 생성했습니다. 또한 모호함을 피하기 위해 카메라 포즈에 상대 방위각과 짝을 이루는 고정된 절대 고도 각도를 사용합니다. 이 접근 방식은 디퓨전 프로세스 중에 여러 뷰 간의 상호 작용을 보장하여 보다 일관된 멀티뷰 이미지를 생성합니다.

3.2. 멀티뷰 조건의 3D 디퓨전
다음 단계는 멀티뷰 조건부 3D 생성 모델을 사용하여 이러한 멀티뷰 이미지를 3D 메시로 변환하는 것입니다. 이 모델은 광범위한 3D 데이터를 학습하여 그럴듯한 3D 모양의 매니폴드를 학습합니다. 텍스처가 있는 3D 모양은 부호화된 거리 함수(SDF) 볼륨과 컬러 볼륨이라는 두 가지 개별 3D 볼륨으로 표현됩니다. 이 모델은 각 단계마다 별도의 확산 네트워크를 사용하여 거칠고 미세한 2단계 방식으로 고해상도 볼륨을 생성합니다. 생성된 멀티뷰 이미지를 가이드로 사용하면 3D 생성 프로세스가 간소화됩니다.

3.3. 텍스처 다듬기
마지막으로 생성된 메시를 경량 최적화 프로세스를 사용하여 텍스처를 개선합니다. 여기에는 생성된 메시의 지오메트리를 수정하고 텐소RF로 표현되는 컬러 필드를 최적화하는 작업이 포함됩니다. 그런 다음 최적화된 컬러 필드를 메시에 적용하여 텍스처 품질을 향상시킵니다.

요약하면, One-2-3-45++는 게임 개발에서 컨셉 아티스트, 3D 모델러, 텍스처 아티스트가 수행하는 역할과 유사하게 단일 입력 이미지에서 3D 콘텐츠를 생성하는 프로세스를 자동화합니다. 일관된 멀티뷰 이미지를 생성하는 것으로 시작하여 이러한 이미지를 사용하여 3D 메시를 생성하고 다듬어 고품질의 텍스처 3D 모델을 생성합니다. 이 방법은 일관성 및 텍스처 품질에 대한 이전의 문제를 해결하여 3D 콘텐츠 생성에 보다 효율적이고 효과적인 접근 방식을 제공합니다.

4

이 백서의 실험 섹션에서는 이미지와 텍스트 프롬프트 모두에서 3D 콘텐츠를 생성하는 One-2-3-45++ 방법의 성능 평가에 대해 자세히 설명합니다.

4.1. 이미지와 3D 비교
저자들은 One-2-3-45++를 DreamFusion과 같은 최적화 기반 접근 방식과 Shap-E와 같은 피드포워드 방식을 포함한 다양한 기준 방법과 비교합니다. 이 평가는 1,030개의 셰이프가 포함된 GSO 데이터 세트를 사용하여 기하학적 유사성에 대한 F-Score와 시각적 유사성에 대한 CLIP 유사성과 같은 메트릭을 사용합니다. 또한 53명의 참가자를 대상으로 다양한 방법으로 생성된 3D 도형 쌍을 비교하는 사용자 연구를 수행했습니다.

그 결과, One-2-3-45++는 F-Score와 CLIP 유사성 모두에서 모든 기준 방법보다 우수한 성능을 보였습니다. 사용자 연구에서도 One-2-3-45++의 결과에 대한 선호도가 상당히 높아 이를 뒷받침합니다. 또한 One-2-3-45++는 최적화 기반 방법보다 빠르며, 이는 실제 애플리케이션에서 매우 중요한 이점입니다.

4.2. 텍스트와 3D 비교
텍스트 프롬프트를 3D 도형으로 변환할 때에도 One-2-3-45++를 ProlificDreamer 및 ShapE와 같은 방법과 비교하여 테스트했습니다. 기준 방법의 긴 생성 시간으로 인해 평가는 50개의 텍스트 프롬프트로 제한되었습니다. CLIP 유사성이 측정 지표로 사용되었으며, 다른 사용자 연구에서는 53명의 참가자가 한 쌍의 결과를 비교했습니다.

이 시나리오에서 One-2-3-45++는 CLIP 유사도와 사용자 선호도 측면에서 다시 한 번 우수한 성능을 보여주었습니다. MVDream을 포함한 다른 방법보다 성능이 월등히 뛰어났으며 훨씬 더 빨랐기 때문에 효율성이 돋보였습니다.

4.3. 분석 및 제거 연구
저자들은 One-2-3-45++의 다양한 구성 요소의 영향을 이해하기 위해 여러 가지 제거 연구를 수행했습니다. 일관된 멀티뷰 생성 모듈을 제거하거나 3D 확산 모듈을 일반화 가능한 NeRF로 대체한 결과 성능이 저하되었습니다. 텍스처 세분화 모듈은 텍스처 품질을 크게 개선했습니다.

3D 확산 모듈에 대한 추가 분석을 통해 효과적인 3D 재구성을 위한 멀티뷰 이미지의 중요성이 강조되었습니다. 단일 입력 뷰의 글로벌 클립 기능에만 의존하면 성능이 저하되었습니다. 확산 모듈을 훈련할 때 멀티뷰 이미지를 통합하는 것도 유익한 것으로 나타났습니다.

GSO 데이터 세트를 사용하여 멀티뷰 생성 모듈과 Zero123 및 SyncDreamer와 같은 기존 방법을 추가로 비교했습니다. One-2-3-45++는 PSNR, LPIPS, 전경 마스크 IoU 측면에서 기존 방법을 능가하여 생성된 멀티뷰 이미지의 3D 일관성과 품질이 더 우수함을 보여주었습니다.

요약하면, 이러한 실험을 통해 One-2-3-45++는 이미지와 텍스트 프롬프트 모두에서 고품질의 일관된 3D 모양을 생성하는 데 효과적이며 속도, 시각적 품질 및 사용자 선호도 측면에서 기존 방법보다 뛰어나다는 것을 입증했습니다. 이 실험은 전체 파이프라인에서 각 구성 요소의 중요성을 강조하여 이 방법의 견고성과 효율성에 기여합니다.