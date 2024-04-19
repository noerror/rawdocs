# 2D Gaussian Splatting for Geometrically Accurate Radiance Fields

[https://arxiv.org/abs/2403.17888](https://arxiv.org/abs/2403.17888)

[https://surfsplatting.github.io/](https://surfsplatting.github.io/)

- Mar 2024

## 1.Introduction

서론 부분은 컴퓨터 그래픽스와 컴퓨터 비전 분야에서 사실적인 새로운 시점 합성(NVS)과 정확한 기하 구조 재구성이라는 장기적인 목표를 중심으로 이야기를 풀어갑니다. 최근에는 3D Gaussian Splatting(3DGS) 기법이 이러한 목표 달성을 위한 유망한 대안으로 부상하고 있음을 언급합니다. 3DGS는 특히 고해상도에서의 실시간 NVS 결과 제공 면에서 주목을 받고 있으며, 다양한 응용 분야로의 빠른 확장이 이루어지고 있습니다. 이러한 확장에는 반사 방지 렌더링, 재료 모델링, 동적 장면 재구성, 애니메이션 가능한 아바타 생성 등이 포함됩니다.

![우리의 방법, 2DGS는 (a) 다시점 RGB 이미지로부터 복잡한 실제 세계 장면을 표현하고 재구성하기 위해 2D 방향성 디스크 세트를 최적화합니다. 이 최적화된 2D 디스크들은 표면에 밀착되어 정렬됩니다. (b) 2D 가우시안 스플래팅을 통해, 우리는 실시간으로 고품질의 새로운 시점 이미지를 노멀과 깊이 맵과 함께 렌더링할 수 있습니다. (c) 마지막으로, 우리의 방법은 최적화된 2D 디스크로부터 세밀하고 노이즈가 없는 삼각 메시 재구성을 제공합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled.png)

우리의 방법, 2DGS는 (a) 다시점 RGB 이미지로부터 복잡한 실제 세계 장면을 표현하고 재구성하기 위해 2D 방향성 디스크 세트를 최적화합니다. 이 최적화된 2D 디스크들은 표면에 밀착되어 정렬됩니다. (b) 2D 가우시안 스플래팅을 통해, 우리는 실시간으로 고품질의 새로운 시점 이미지를 노멀과 깊이 맵과 함께 렌더링할 수 있습니다. (c) 마지막으로, 우리의 방법은 최적화된 2D 디스크로부터 세밀하고 노이즈가 없는 삼각 메시 재구성을 제공합니다.

그러나 3DGS가 다루는 3D 가우시안이 완전한 각 방사도를 모델링하는 방식은 표면의 얇은 특성과 상충되어 복잡한 기하학적 형태를 정확하게 포착하는 데 한계가 있다고 지적합니다. 이와 대조적으로, 과거 연구에서는 surfels(표면 요소)이 복잡한 기하 구조를 효과적으로 표현할 수 있는 방법으로 제시되었습니다. Surfels는 지역적으로 객체 표면을 모양과 색상 속성으로 근사화하며, 이미 알려진 기하학적 구조로부터 파생될 수 있습니다. 이러한 접근법은 SLAM(동시적 위치 추정 및 맵핑)과 같은 로보틱스 작업에서도 널리 사용됩니다.

이러한 배경에서, 저자들은 3DGS의 장점과 surfels 기반 모델의 장점을 결합하면서 각각의 한계를 극복할 수 있는 새로운 방식인 2D Gaussian Splatting을 제안합니다. 이 접근법은 3DGS의 3차원 가우시안 대신 2차원 가우시안 원시체를 사용하여 3D 장면을 표현함으로써, 기하 구조 재현의 정확도를 크게 향상시키고, 다양한 시점에서의 일관성 있는 렌더링 결과를 달성합니다. 이를 통해 저자들은 기하학적 모델링에서의 도전을 극복하고, 사실적인 NVS 결과를 달성하는 새로운 방법론을 제시합니다.

![3DGS와 2DGS의 비교. 3DGS는 다른 시점에서 봤을 때 값 평가를 위해 다른 교차 평면을 사용하며, 이로 인해 일관성이 없습니다. 우리의 2DGS는 다시점에서 일관된 값 평가를 제공합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%201.png)

3DGS와 2DGS의 비교. 3DGS는 다른 시점에서 봤을 때 값 평가를 위해 다른 교차 평면을 사용하며, 이로 인해 일관성이 없습니다. 우리의 2DGS는 다시점에서 일관된 값 평가를 제공합니다.

## 2.Related work

"관련 작업" 챕터에서는 사실적인 새로운 시점 합성(NVS)과 3D 재구성 분야에서의 기존 연구와 최근의 발전을 개괄적으로 다룹니다. 이 챕터는 NVS, 3D 재구성, 차별화 가능한 포인트 기반 그래픽스, 그리고 이 분야에서의 동시 작업에 대해 설명하며, 각 분야에서의 주요 발전과 해당 연구들이 마주한 도전, 그리고 그 해결책에 대해 논의합니다.

### **2.1 Novel View Synthesis (NVS)**

- **NeRF의 도입**: 이 섹션은 Neural Radiance Fields (NeRF)를 중심으로 NVS 분야에서 이루어진 혁신을 소개합니다. NeRF는 기하학과 시점 의존적 외형을 다루기 위해 다층 퍼셉트론(MLP)을 사용하며, 볼륨 렌더링을 통해 뛰어난 렌더링 품질을 달성합니다.
- **NeRF 이후의 발전**: NeRF 이후에는 더 나은 렌더링 효율성, 학습 및 표현력 향상을 위한 여러 접근법이 제시되었습니다. 이는 NeRF의 한계를 극복하고 다양한 시나리오에서 사용할 수 있는 새로운 기술들로 발전하였습니다.

### **2.2 3D Reconstruction**

- **멀티뷰 스테레오 방식**: 전통적인 멀티뷰 스테레오(MVS) 방식부터 최근의 신경학적 접근법까지, 다양한 3D 재구성 기술이 소개됩니다. 이러한 기술들은 특징 매칭, 깊이 예측, 통합 등의 과정을 거쳐 3D 모델을 생성합니다.
- **신경학적 접근법의 발전**: 최근에는 MLP를 사용하여 암시적 표면을 모델링하고, 이를 통해 더 세밀한 표면 재구성을 가능하게 하는 연구가 진행되고 있습니다. 이러한 방법은 특히 RGB 이미지로부터의 세부적인 표면 재구성에 유용합니다.

### **2.3 Differentiable Point-based Graphics**

- **점 기반 렌더링의 발전**: 차별화 가능한 점 기반 그래픽스는 효율성과 복잡한 구조 표현의 유연성 때문에 광범위하게 탐색되고 있습니다. 여기서는 NPBG, DSS, Pulsar 같은 기술이 언급되며, 이들은 점 구름을 이용한 효과적인 이미지 생성 및 최적화 방법을 제공합니다.

### **2.4 Concurrent work**

- **동시 작업의 검토**: 이 섹션에서는 3DGS와 관련된 동시 작업을 검토합니다. 특히, 3D 가우시안 원시체에 추가적인 속성을 모델링하는 최신 연구들과 이들이 재조명 작업에 어떻게 적용되는지를 다룹니다. 또한, 3DGS와 비교하여 다른 방법론들의 장단점을 설명하며, 특히 표면 재구성에 초점을 맞춥니다.

이 챕터는 최근의 연구 동향을 개괄하며, NVS와 3D 재구성 분야에서의 주요 진보와 여전히 남아 있는 도전 과제들을 조명합니다. 또한, 이러한 배경 지식을 바탕으로 본 논문에서 제안하는 2D Gaussian Splatting 방법의 기여도와 차별성을 설명하는 데 기초를 마련합니다.

## **3.3D Gaussian Splatting**

"3D Gaussian Splatting" 챕터에서는 3D Gaussian Splatting(3DGS) 기법의 기본 원리와 이를 통해 3D 장면을 어떻게 표현하고 렌더링하는지에 대해 설명합니다. 이 기법은 3D 장면을 3D 가우시안 프리미티브로 표현하고, 이를 활용하여 다양한 뷰에서의 이미지를 생성하는 과정을 다룹니다.

### **3DGS의 기본 원리**

- **3D 가우시안 프리미티브**: 3DGS는 3D 장면을 3D 가우시안 프리미티브로 표현합니다. 각 가우시안은 위치와 3D 공분산 행렬로 정의되며, 이를 통해 장면의 볼륨적인 특성을 모델링합니다.
- **렌더링 과정**: 3D 가우시안 프리미티브는 카메라 좌표계로 변환되고, 2D 이미지 평면으로 투영됩니다. 이 과정에서 3D 가우시안은 지역적인 아핀 변환을 거쳐 2D 가우시안으로 변환되며, 최종 이미지는 이 2D 가우시안들을 기반으로 생성됩니다.

### **핵심 과제와 해결 방안**

- **표면 재구성의 도전**: 3DGS 방법은 복잡한 3D 장면을 효과적으로 렌더링할 수 있으나, 본질적으로 볼륨적인 표현을 사용하기 때문에 얇은 표면의 정확한 재구성에 어려움이 있습니다. 이는 3D 가우시안이 모든 방향의 방사도를 모델링하려고 하기 때문에 발생하는 문제입니다.
- **표면 노멀의 부재**: 3DGS는 표면 노멀을 명시적으로 모델링하지 않습니다. 이는 고품질 표면 재구성에 필수적인 요소로, 이의 부재는 재구성 품질을 저하시킬 수 있습니다.
- **다시점 일관성 문제**: 다양한 시점에서의 일관된 2D 투영을 얻기 어렵다는 점도 한계로 지적됩니다. 이는 특히 아핀 변환을 사용하여 3D 가우시안을 레이 공간으로 변환할 때, 중심 근처에서는 정확한 투영을 얻을 수 있지만, 주변 영역에서는 관점의 정확도가 떨어지는 문제를 초래합니다.

![2D 가우시안 스플래팅의 일러스트레이션. 2D 가우시안 스플랫은 중심점 𝐩, 접선 벡터 𝐭과 𝐭, 그리고 두 스케일링 인자가 분산을 조절하는 타원형 디스크로 특징지어집니다. 그들의 타원형 투영은 레이-스플랫 교차를 통해 샘플링되고(섹션 4.2), 이미지 공간에서 알파-블렌딩을 통해 누적됩니다. 2DGS는 경사하강법을 통해 색상, 깊이, 노멀과 같은 표면 속성을 재구성합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%202.png)

2D 가우시안 스플래팅의 일러스트레이션. 2D 가우시안 스플랫은 중심점 𝐩, 접선 벡터 𝐭과 𝐭, 그리고 두 스케일링 인자가 분산을 조절하는 타원형 디스크로 특징지어집니다. 그들의 타원형 투영은 레이-스플랫 교차를 통해 샘플링되고(섹션 4.2), 이미지 공간에서 알파-블렌딩을 통해 누적됩니다. 2DGS는 경사하강법을 통해 색상, 깊이, 노멀과 같은 표면 속성을 재구성합니다.

### **기술적 세부 사항**

- **공분산 행렬의 변환**: 3D 가우시안의 공분산 행렬은 스케일링 행렬과 회전 행렬로 분해되어, 카메라 좌표계로 변환되고 이미지 평면으로 투영됩니다. 이 과정에서 3D 공분산 행렬은 2D 공분산 행렬로 축소되어, 2D 가우시안을 정의합니다.
- **알파 블렌딩을 이용한 통합**: 최종 이미지는 앞에서부터 뒤로 알파 가중치가 적용된 외형을 통합함으로써 생성됩니다. 이는 각 가우시안 프리미티브의 기여도를 결정하고, 다양한 깊이에 걸쳐 부드러운 렌더링 결과를 제공합니다.

3DGS는 복잡한 3D 장면을 효과적으로 렌더링하는 강력한 기법이지만, 얇은 표면의 정밀한 재구성과 다시점 일관성 유지에 있어 몇 가지 핵심적인 한계를 가지고 있습니다. 이러한 한계는 3D 장면의 정확한 기하 구조 재현을 목표로 하는 연구에 있어 중요한 고려사항입니다

## **4.2D Gaussian Splatting**

"2D Gaussian Splatting" 챕터에서는 기존 3D Gaussian Splatting(3DGS)의 한계를 극복하고자 제안된, 2D Gaussian Splatting(2DGS) 기법에 대해 자세히 설명합니다. 이 새로운 접근법은 3D 장면을 2D 가우시안 원시체로 표현함으로써, 기하학적 정확성을 크게 향상시키고 렌더링 품질을 높이는 것을 목표로 합니다.

### **4.1 모델링**

- **2D 가우시안 원시체**: 2DGS는 3차원 공간에 임베딩된 "평평한" 2D 가우시안 원시체를 사용하여 3D 장면을 표현합니다. 이는 각 원시체가 방향성을 가진 타원형 디스크로 표현되며, 이를 통해 더 정밀한 기하학적 세부사항과 표면 정규화를 달성할 수 있습니다.
- **기하학적 정확성의 향상**: 3DGS와 달리, 2DGS는 레이와 스플랫의 명시적 교차를 사용하여, 다양한 관점에서 렌더링될 때 깊이의 일관성을 유지합니다. 이로 인해 기하학적 재구성의 정확성이 크게 향상됩니다.

### **4.2 스플랫팅**

- **투영과 레스터화**: 2D 가우시안 원시체는 이미지 공간으로 투영되어 레스터화됩니다. 이 과정은 홈오지니어스 좌표를 사용하여 일반적인 2D-2D 매핑으로 설명되며, 이를 통해 더 정확한 렌더링이 가능해집니다.
- **레이-스플랫 교차**: 특별히, 레이와 스플랫의 교차점은 두 개의 서로 직교하는 평면의 교차로 찾아집니다. 이 방식은 렌더링 과정에서 수치적 안정성을 제공하고, 렌더링의 품질을 높입니다.

### **장점과 기술적 세부 사항**

- **정확한 기하 구조 표현**: 2DGS의 주요 장점 중 하나는 기하 구조의 정확한 표현입니다. 이는 2D 가우시안이 실제 표면과 더 밀접하게 일치하도록 설계되었기 때문에 가능합니다.
- **정규화 손실의 도입**: 렌더링 품질을 더욱 향상시키기 위해, 깊이 왜곡과 정상 일관성에 대한 두 가지 정규화 손실이 도입됩니다. 이는 표면의 부드러움을 증가시키고, 렌더링된 깊이와 노멀 맵 간의 일치를 개선합니다.

![비교적 제한이 없는 실제 세계 데이터셋(주방, 자전거, 그루터기, 트리힐)의 장면을 사용하여 우리의 방법, 3DGS, SuGaR 간의 시각적 비교(테스트 세트 뷰). 첫 번째 열은 새로운 시점의 그라운드 트루스를 보여주고, 두 번째와 세 번째 열은 고신뢰도 새로운 시점과 정확한 기하학을 보여주는 우리의 렌더링 결과를 보여줍니다. 네 번째와 다섯 번째 열은 6미터 깊이 범위를 사용한 TSDF 융합을 통해 우리 방법과 3DGS의 표면 재구성 결과를 보여줍니다. 배경 영역(이 범위 밖)은 연한 회색으로 채색됩니다. 여섯 번째 열은 SuGaR의 결과를 보여줍니다. 두 기준과 비교하여, 우리의 방법은 날카로운 가장자리와 복잡한 기하학을 포착하는 데 뛰어납니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%203.png)

비교적 제한이 없는 실제 세계 데이터셋(주방, 자전거, 그루터기, 트리힐)의 장면을 사용하여 우리의 방법, 3DGS, SuGaR 간의 시각적 비교(테스트 세트 뷰). 첫 번째 열은 새로운 시점의 그라운드 트루스를 보여주고, 두 번째와 세 번째 열은 고신뢰도 새로운 시점과 정확한 기하학을 보여주는 우리의 렌더링 결과를 보여줍니다. 네 번째와 다섯 번째 열은 6미터 깊이 범위를 사용한 TSDF 융합을 통해 우리 방법과 3DGS의 표면 재구성 결과를 보여줍니다. 배경 영역(이 범위 밖)은 연한 회색으로 채색됩니다. 여섯 번째 열은 SuGaR의 결과를 보여줍니다. 두 기준과 비교하여, 우리의 방법은 날카로운 가장자리와 복잡한 기하학을 포착하는 데 뛰어납니다.

### **결론**

2D Gaussian Splatting은 3D 장면의 기하학적 모델링과 렌더링에 있어 중요한 발전을 대표합니다. 이 기법은 기존의 3DGS 방법이 가진 한계를 극복하고, 더 정밀한 기하 구조 재구성과 높은 품질의 렌더링 결과를 달성하기 위한 새로운 접근법을 제시합니다. 2DGS는 특히 복잡한 표면과 얇은 구조물의 정확한 모델링에 유리하며, 다양한 응용 분야에서의 사용 가능성을 열어줍니다.

## **5.Training**

"Training" 챕터에서는 2D Gaussian Splatting(2DGS) 모델을 최적화하는 과정과, 효과적인 기하 구조 재구성을 위해 도입된 정규화 항목들에 대해 설명합니다. 이 훈련 과정은 모델의 기하학적 모델링 능력을 향상시키기 위해 설계되었습니다.

### **기하 구조 재구성의 도전**

- **기본 문제**: 3D 재구성 작업은 본질적으로 제약이 없어 최적화만으로는 노이즈가 많은 재구성 결과를 초래할 수 있습니다. 이는 특히 광학적 손실만을 사용할 때 더욱 심화됩니다.

### **정규화 항목 도입**

- **깊이 왜곡 손실**: 볼륨 렌더링 과정에서 가우시안 프리미티브 간의 거리를 고려하지 않는 문제를 해결하기 위해, 깊이 왜곡 손실이 도입되었습니다. 이 손실은 광선과 스플랫의 교차점들 사이의 거리를 최소화함으로써, 가우시안 분포를 광선을 따라 더 집중시키는 효과를 냅니다.
- **정상 일관성 손실**: 2D 가우시안 프리미티브가 실제 표면과 일치하도록 보장하기 위해, 정상 일관성 손실이 적용됩니다. 이는 렌더링된 깊이의 그라디언트와 렌더링된 노멀 맵 간의 일치를 개선함으로써, 표면의 정확성을 향상시킵니다.

### **최적화 과정**

- **손실 함수**: 모델은 RGB 재구성 손실, 깊이 왜곡 손실, 정상 일관성 손실을 포함하는 복합 손실 함수를 최소화함으로써 훈련됩니다. 이 손실 함수는 기하 구조와 외형의 동시 최적화를 가능하게 하여, 고품질의 재구성 결과를 달성합니다.
- **훈련 데이터와 초기화**: 훈련은 포즈가 주어진 이미지 세트와 초기 희소 포인트 클라우드를 사용하여 수행됩니다. 모델의 파라미터는 이 데이터를 기반으로 최적화됩니다.

### **훈련 결과의 효과**

- **기하학적 정확성의 향상**: 도입된 정규화 항목은 기하학적 재구성의 정확성을 크게 향상시키며, 특히 복잡한 표면 세부사항과 텍스처가 있는 영역에서의 성능 개선을 보여줍니다.
- **노이즈 감소**: 복합 손실 함수의 최적화는 재구성된 장면에서의 노이즈를 줄이고, 더 부드러운 표면을 생성하는 데 기여합니다.

2DGS 모델의 훈련 과정은 정교한 기하 구조의 재구성을 가능하게 하며, 다양한 시각에서의 일관된 고품질 렌더링 결과를 달성하기 위한 핵심 단계입니다. 정규화 항목의 도입은 모델이 복잡한 장면을 보다 정확하게 모델링하고 재구성하는 데 중요한 역할을 합니다.

## **6.Experiments**

"Experiments" 챕터에서는 2D Gaussian Splatting(2DGS) 기법의 성능을 검증하기 위한 광범위한 실험 결과를 제공합니다. 이 실험들은 2DGS가 기존의 상태(state-of-the-art, SOTA) 기법들과 어떻게 비교되는지, 그리고 제안된 방법의 다양한 구성 요소들이 최종 재구성 품질에 어떤 영향을 미치는지를 평가합니다.

### **6.1 구현 세부 사항**

- **CUDA 커널**: 2DGS의 구현은 사용자 정의 CUDA 커널을 사용하여 최적화되었으며, 3DGS 프레임워크를 기반으로 구축되었습니다. 이는 깊이 왜곡 맵, 깊이 맵, 그리고 노멀 맵을 포함하여 정규화 과정에서 사용됩니다.
- **적응적 제어 전략**: 2D 가우시안 프리미티브의 수는 훈련 동안 적응적으로 조정됩니다. 이는 모델이 장면의 복잡성에 맞게 자동으로 조정될 수 있도록 하여 효율성과 품질을 동시에 높입니다.

![ DTU 벤치마크(Jensen et al., 2014)에서의 정성적 비교. 우리의 2DGS는 세밀하고 노이즈가 없는 표면을 생성합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%204.png)

 DTU 벤치마크(Jensen et al., 2014)에서의 정성적 비교. 우리의 2DGS는 세밀하고 노이즈가 없는 표면을 생성합니다.

### **6.2 성능 비교**

- **데이터셋**: DTU, Tanks and Temples, Mip-NeRF360 등 다양한 데이터셋에서 2DGS의 성능이 평가되었습니다. 이 데이터셋들은 각각 다른 특성을 가지고 있어, 모델의 범용성을 검증하는 데 적합합니다.
- **기하학적 재구성**: 2DGS는 Chamfer 거리 및 훈련 시간 면에서 기존 SOTA 방법들과 비교하여 우수한 성능을 보였습니다. 특히, 3DGS 및 다른 명시적 표현 방법들과 비교했을 때, 더 나은 기하학적 재구성 결과를 달성했습니다.
- **효율성**: 2DGS는 재구성 속도 면에서도 효율적이었습니다. 이는 임플리시트 방법에 비해 약 100배 빠르며, 동시에 수행된 다른 작업들보다도 3배 이상 빠른 성능을 보였습니다.

### **6.3 Ablation Study**

- **설계 선택의 영향**: 다양한 설계 선택이 최종 재구성 품질에 미치는 영향을 분석하기 위한 실험들이 수행되었습니다. 특히, 정규화 항목의 존재와 메시 추출 방법이 중점적으로 검토되었습니다.
- **정규화 항목**: 정규화 항목(깊이 왜곡과 정상 일관성 손실)이 모델 성능에 긍정적인 영향을 미치며, 특히 표면의 방향성과 깊이 정보의 일관성을 향상시킵니다.
- **메시 추출**: 실험은 다양한 메시 추출 방법을 평가했으며, TSDF 융합을 사용한 방법이 가장 효과적임을 보여줍니다. 이 방법은 정확한 깊이 정보를 기반으로 더 정확한 메시를 생성합니다.

![정규화 효과에 대한 정성적 연구. 왼쪽부터 입력 이미지, 노멀 일관성 없이, 깊이 왜곡 없이, 그리고 우리의 완전한 모델입니다. 노멀 일관성 손실을 비활성화하면 노이즈가 있는 표면 방향성이 생깁니다; 반대로, 깊이 왜곡 정규화를 생략하면 표면 노멀이 흐려집니다. 두 정규화를 모두 사용하는 완전한 모델은 날카롭고 평평한 특징을 성공적으로 포착합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%205.png)

정규화 효과에 대한 정성적 연구. 왼쪽부터 입력 이미지, 노멀 일관성 없이, 깊이 왜곡 없이, 그리고 우리의 완전한 모델입니다. 노멀 일관성 손실을 비활성화하면 노이즈가 있는 표면 방향성이 생깁니다; 반대로, 깊이 왜곡 정규화를 생략하면 표면 노멀이 흐려집니다. 두 정규화를 모두 사용하는 완전한 모델은 날카롭고 평평한 특징을 성공적으로 포착합니다.

### **결론**

이 챕터는 2DGS가 기존 방법들과 비교하여 기하학적 재구성과 NVS 성능 면에서 상당한 개선을 제공함을 보여줍니다. 실험을 통해 2DGS의 다양한 설계 결정이 최종 결과에 미치는 영향을 평가하고, 이 방법이 효율적이며 정확한 3D 장면 재구성을 가능하게 한다는 것을 입증합니다.

![ [Zwicker et al. 2001b][Kerbl et al. 2023]에서 채택한 아핀 근사는 관점 왜곡과 부정확한 깊이를 야기합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%206.png)

 [Zwicker et al. 2001b][Kerbl et al. 2023]에서 채택한 아핀 근사는 관점 왜곡과 부정확한 깊이를 야기합니다.

![MipNeRF360 [Barron et al. 2022b], 3DGS [Kerbl et al. 2023], 그리고 우리 방법에 의해 생성된 깊이 맵을 시각화합니다. 3DGS(d)와 2DGS(f)의 깊이 맵은 방정식 18을 사용하여 렌더링되고 MipNeRF360을 따라 시각화됩니다. 표면의 부드러움을 강조하기 위해, 우리는 방정식 15를 사용하여 깊이 점에서 추정된 노멀을 3DGS(c)와 우리 방법(e) 모두에 대해 추가적으로 시각화합니다. MipNeRF360은 합리적으로 부드러운 깊이 맵을 생성할 수 있지만, 그 샘플링 과정은 세부 구조의 손실을 초래할 수 있습니다. 3DGS와 2DGS 모두 얇은 구조를 모델링하는 데 뛰어나지만, (c)와 (e)에서 보이듯이 3DGS의 깊이 맵은 상당한 노이즈를 보여줍니다. 반대로, 우리의 접근법은 렌더링된 노멀 맵과 일관된 노멀을 가진 샘플링된 깊이 점을 생성함으로써 메싱 과정 동안 깊이 융합을 강화합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%207.png)

MipNeRF360 [Barron et al. 2022b], 3DGS [Kerbl et al. 2023], 그리고 우리 방법에 의해 생성된 깊이 맵을 시각화합니다. 3DGS(d)와 2DGS(f)의 깊이 맵은 방정식 18을 사용하여 렌더링되고 MipNeRF360을 따라 시각화됩니다. 표면의 부드러움을 강조하기 위해, 우리는 방정식 15를 사용하여 깊이 점에서 추정된 노멀을 3DGS(c)와 우리 방법(e) 모두에 대해 추가적으로 시각화합니다. MipNeRF360은 합리적으로 부드러운 깊이 맵을 생성할 수 있지만, 그 샘플링 과정은 세부 구조의 손실을 초래할 수 있습니다. 3DGS와 2DGS 모두 얇은 구조를 모델링하는 데 뛰어나지만, (c)와 (e)에서 보이듯이 3DGS의 깊이 맵은 상당한 노이즈를 보여줍니다. 반대로, 우리의 접근법은 렌더링된 노멀 맵과 일관된 노멀을 가진 샘플링된 깊이 점을 생성함으로써 메싱 과정 동안 깊이 융합을 강화합니다.

![ 우리의 2DGS와 3DGS [Kerbl et al. 2023]를 사용한 표면 재구성 비교. 메시는 깊이 맵에 TSDF를 적용하여 추출됩니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%208.png)

 우리의 2DGS와 3DGS [Kerbl et al. 2023]를 사용한 표면 재구성 비교. 메시는 깊이 맵에 TSDF를 적용하여 추출됩니다.

![Tanks and Temples 데이터셋 [Knapitsch et al. 2017]에 대한 정성적 연구.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%209.png)

Tanks and Temples 데이터셋 [Knapitsch et al. 2017]에 대한 정성적 연구.

![재구성된 2D 가우시안 디스크로부터의 외형 렌더링 결과, DTU, TnT, 그리고 Mip-NeRF360 데이터셋을 포함합니다.](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%2010.png)

재구성된 2D 가우시안 디스크로부터의 외형 렌더링 결과, DTU, TnT, 그리고 Mip-NeRF360 데이터셋을 포함합니다.

![한계점의 일러스트레이션: 우리의 2DGS는 예를 들어, 예시 (A)에 나타난 유리와 같은 반투명 표면의 정확한 재구성에 어려움을 겪습니다. 또한, 우리 방법은 (B)에서 보여주듯이 고광도 영역에서 구멍을 만드는 경향이 있습니다](2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf/Untitled%2011.png)

한계점의 일러스트레이션: 우리의 2DGS는 예를 들어, 예시 (A)에 나타난 유리와 같은 반투명 표면의 정확한 재구성에 어려움을 겪습니다. 또한, 우리 방법은 (B)에서 보여주듯이 고광도 영역에서 구멍을 만드는 경향이 있습니다

## **7.Conclusion**

"Conclusion" 챕터에서는 2D Gaussian Splatting(2DGS)에 대한 연구의 결론을 요약하고, 이 방법론이 제공하는 기여와 한계점을 검토합니다. 이 연구는 3D 장면의 기하학적 모델링과 렌더링에 있어 중요한 발전을 제시하며, 복잡한 장면의 사실적인 재구성과 렌더링을 가능하게 하는 새로운 접근 방식을 도입합니다.

### **주요 기여**

- **효율적인 차별화 가능한 렌더러**: 2DGS는 2D 가우시안 원시체를 사용하여 3D 장면을 표현하고, 이를 통해 정확도 높은 스플래팅과 기하학적 모델링을 실현합니다. 이 접근법은 기존의 3DGS 방식과 비교해 향상된 기하 구조 재현과 렌더링 품질을 달성합니다.
- **정규화 손실 도입**: 기하학적 재구성의 품질을 향상시키기 위해 깊이 왜곡과 정상 일관성에 대한 정규화 손실을 적용합니다. 이는 렌더링 과정에서의 정확도와 일관성을 높이며, 더 부드럽고 정밀한 표면을 생성합니다.
- **SOTA 결과 달성**: 제안된 방법은 기존의 명시적 표현 방법들과 비교하여 상태 기술의 결과를 달성합니다. 특히, 기하 구조 재구성과 NVS의 성능 면에서 우수한 결과를 보여줍니다.

### **한계점 및 미래 연구 방향**

- **반투명 표면 처리의 어려움**: 현재의 접근법은 주로 완전 불투명한 표면에 초점을 맞추고 있어, 반투명 표면이나 복잡한 빛 투과 특성을 가진 장면의 처리에는 한계가 있습니다.
- **텍스처와 기하학적 세부사항의 균형**: 현재 모델은 텍스처가 풍부한 영역에 비해 기하학적 세부사항을 덜 잘 재현할 수 있습니다. 이는 향후 연구에서 더욱 발전시킬 수 있는 부분입니다.
- **이미지 품질과 기하학적 정확성의 트레이드오프**: 정규화 과정에서 이미지 품질과 기하학적 정확성 사이의 균형을 맞추는 것은 여전히 도전적인 과제입니다. 특정 상황에서는 과도한 평활화로 인해 세부사항이 손실될 수 있습니다.