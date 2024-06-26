# Deep Inverse Rendering for High-resolution SVBRDF Estimation from an Arbitrary Number of Images

*이 논문에서 저자는 여러 장의 사진에서 고해상도 공간 가변 양방향 반사율 분포 함수(SVBRDF)를 추정하기 위한 새로운 딥러닝 프레임워크를 소개합니다. 이 프레임워크는 자동 인코더를 사용해 SVBRDF의 잠재 공간을 학습하고 이 공간에서 최적화합니다. 결과 SVBRDF의 정밀도는 입력 이미지의 수에 따라 조정됩니다. 제안된 방법은 기존의 단일 이미지 SVBRDF 추정 방법보다 성능이 뛰어납니다. 저자는 특히 딥 리버스 렌더링 최적화를 위한 초기화 프로세스를 개선하는 등 향후 연구 과제를 제시합니다.*

[https://gao-duan.github.io/publications/mvsvbrdf/mvsvbrdf_low_resolution.pdf](https://gao-duan.github.io/publications/mvsvbrdf/mvsvbrdf_low_resolution.pdf)

[https://github.com/msraig/DeepInverseRendering](https://github.com/msraig/DeepInverseRendering)

![https://github.com/msraig/DeepInverseRendering/blob/master/mvsvbrdf.png?raw=true](https://github.com/msraig/DeepInverseRendering/blob/master/mvsvbrdf.png?raw=true)

1

이 문서에서는 공간에 따라 달라지는 재료의 표면 반사 속성을 추정하는 데 따르는 어려움에 대해 설명합니다. 역 렌더링과 같은 전통적인 방법은 많은 수의 사진으로 정확한 결과를 얻을 수 있지만 입력 수가 적으면 어려움을 겪습니다. 딥러닝을 사용하는 최신 방법은 단일 이미지로 그럴듯한 결과를 생성할 수 있지만 이미지의 모호하거나 보이지 않는 반사율 특징을 처리하는 데 어려움을 겪습니다.

이 논문에서는 여러 장의 사진에서 고해상도 표면 반사율 특성을 추정하는 통합 프레임워크를 소개합니다. 딥 인버스 렌더링이라고 명명된 이 방법은 딥 러닝과 역 렌더링을 결합하여 완전 컨볼루션 자동 인코더로 특징지어지는 학습된 잠재 공간에서 작동합니다. 이 방법은 렌더링 오류에 따라 최적화되며, 더 많은 이미지가 입력될수록 모델의 추정치가 '그럴듯한'에서 '정확한'으로 발전합니다.

일반 자동 인코더와 달리 이 프레임워크는 그라데이션 품질에 부정적인 영향을 미치기 때문에 디코더에서 일괄 정규화를 사용하지 않습니다. 대신 최적화 견고성을 개선하기 위해 훈련 중에 평활도 손실을 포함합니다. 저자는 결과를 개선하기 위해 이전의 단일 이미지 솔루션에서 그럴듯한 추정치로 시작할 것을 제안합니다.

이 모델의 장점은 자동 인코더를 재학습할 필요 없이 잠재 특징 맵의 해상도를 확장하는 것만으로 고해상도 SVBRDF(공간 가변 양방향 반사율 분포 함수)를 쉽게 지원할 수 있다는 것입니다.

이 논문은 공개적으로 사용 가능한 다양한 데이터 세트와 공간적으로 다양한 재료의 실제 사진을 사용하여 모델의 효과를 입증합니다. 저자들은 이 솔루션이 대상 자료가 훈련 데이터세트를 벗어난 경우에도 단일 사진에서 추정된 SVBRDF의 품질을 향상시킨다고 주장합니다.

요약하면, 저자들은 여러 장의 사진을 처리할 수 있고, 해상도에 제한을 받지 않으며, 특히 대상 자료가 훈련 데이터셋을 벗어난 경우에도 SVBRDF의 품질을 향상시키는 SVBRDF 복구 방법을 제안합니다.

2

이 단원에서는 전문 연구소의 전유물이었던 다양한 재료의 외관을 캡처하기 위한 이미지 기반 모델링 도구의 대중화에 대해 설명합니다. 이러한 방법은 수집 매개변수의 변화에 대한 견고성이 필요하므로 간단하고 효율적인 수집 프로세스의 중요성이 강조됩니다.

일부 방법은 휴대폰 카메라와 같이 간단하고 가벼운 설정을 사용하여 재료의 외관을 최대한 정확하게 모델링하는 것을 목표로 합니다. Riviere 외, Hui 외, Palma 외, Dong 외, Xia 외 등이 개발한 이러한 기법은 한 장면을 여러 번 관찰하고 휴리스틱 또는 희소성 가정을 사용합니다. 그러나 이러한 접근 방식은 입력 이미지가 매우 적을 경우 의미 있는 반사율 추정치를 제공할 수 없습니다.

다른 기법들은 입력 이미지의 수를 최소화하면서 재료 특성에 대한 그럴듯한 추정치를 복구하는 것을 목표로 합니다. Aittala 외, Xu 외, Zhou 외의 방법은 재료의 공간 관계 또는 희소성을 활용합니다. 이러한 방법과 달리 이 논문에서 제안한 기법은 공간 관계를 인코딩하는 학습된 특징에 의존합니다.

최근에는 학습 기반 기법도 제안되고 있습니다. Li 등, Ye 등, Deschaintre 등, Li 등의 기법은 컨볼루션 신경망을 사용하여 한 장의 사진에서 반사율 특성을 추론합니다. 그러나 이러한 딥러닝 솔루션은 여러 입력 이미지를 처리하도록 쉽게 확장되지 않습니다.

이 논문의 저자들은 딥 러닝과 역 렌더링을 결합한 방법을 제안합니다. '딥 인버스 렌더링'이라고 불리는 이 새로운 방법은 자동 인코더를 사용하여 최적화 프로세스를 안내합니다. 입력 사진 수에 대한 유연성이 뛰어나고 최적화를 위해 수작업 휴리스틱에 의존하지 않는다는 점이 특징입니다. 공간적으로 다양한 양방향 반사율 분포 함수(SVBRDF)의 학습된 임베딩을 통해 반사율 파라미터 맵의 특징을 최적화합니다.

3

이 글에서는 여러 장의 사진을 사용하여 다양한 재료의 반사율 속성을 추정하는 방법을 소개합니다. 저자는 기존 역 렌더링 파이프라인의 일부로 딥 러닝 모델을 사용하는 시스템인 딥 리버스 렌더링 프레임워크를 제안합니다. 이 시스템은 실제 사진과 예상 반사율 파라미터의 렌더링 간의 차이를 최소화하여 반사율 속성을 최적화합니다.

저자는 머티리얼이 평면형이고 각 표면 지점의 반사율 특성이 Cook-Torrance 마이크로 패싯 BRDF(양방향 반사율 분포 함수) 모델로 표현될 수 있다고 가정합니다. 재료의 사진은 카메라 근처에 위치한 점 광원에 의해 조명되며, 거의 정상에 가까운 입사각에서 최소 한 장의 사진이 촬영됩니다. 모든 이미지는 탑뷰 시점과 일치하도록 조정됩니다.

딥 인버스 렌더링 프레임워크는 픽셀 단위가 아닌 전체 이미지를 최적화하는 데 중점을 둡니다. 이 방법은 완전 컨볼루션 자동 인코더가 SVBRDF(공간 가변 양방향 반사율 분포 함수)를 해당 잠재 벡터로 변환하거나 그 반대로 변환하는 잠재 공간을 도입합니다.

저자들은 이 접근 방식이 다양한 입력 조건을 유연하게 처리할 수 있고 취약한 수작업 정규화기가 필요하지 않다고 생각합니다. 이론적 분석에 따르면 딥 리버스 렌더링 접근 방식은 추론 네트워크 학습에 비해 훈련 중에 볼 수 없는 예제를 처리하는 데 더 적합하다고 합니다.

4

이 단원에서는 머티리얼의 복잡한 공간 변화를 정확하게 재현하면서 최적화를 수행할 수 있도록 잠재 공간을 줄여주는 자동 인코더의 일종인 딥 리버스 렌더링 네트워크의 구조와 기능에 대해 간략하게 설명합니다.

이 네트워크는 표준 자동 인코더 구조를 사용하여 256 × 256 반사 속성 맵을 입력으로 받아 피처 길이가 512인 8 × 8 해상도 피처 맵으로 축소합니다. 이 네트워크는 반사율 맵의 L1 손실과 재료의 9개 시각화에 대한 L1 로그 손실을 결합하는 손실 함수를 사용하여 훈련됩니다.

저자들은 훈련 중 성능과 안정성을 향상시키기 위해 컨볼루션 신경망에서 흔히 사용되는 일괄 정규화 개념을 실험합니다. 일괄 노멀라이제이션은 노이즈를 줄이고 더 깨끗한 반사 맵을 생성하지만, 선명도와 가장자리 디테일이 손실될 수 있음을 관찰했습니다. 이 문제를 해결하기 위해 그들은 잠재 벡터를 최적화하기 위해 좋은 그라데이션을 생성하는 디코더를 원하기 때문에 디코더에 배치 노멀라이제이션을 적용하지 않습니다. 저자는 인코더의 마지막 레이어에서만 배치 정규화를 사용하는 것이 가장 효과적이라는 결론을 내립니다.

또한 저자는 평활도 손실 함수를 도입하여 잠재 코드의 작은 변화가 SVBRDF의 작은 변화를 가져올 수 있도록 합니다. 훈련 손실에 이 기능을 추가하면 잠재 공간을 재구성하여 근처의 잠재 코드가 유사한 디코딩된 SVBRDF를 갖도록 하여 보간 또는 최적화를 위한 더 나은 조건을 제공할 수 있습니다.

![평활화 손실이 있는 학습된 잠재 공간의 t-SNE 시각화 평활도 손실이 있는 경우와 없는 경우. 평활도 손실이 있는 경우, 결과 잠상 공간은 보다 연속적입니다.](Deep%20Inverse%20Rendering%20for%20High-resolution%20SVBRDF%20%205716fe7aebe44fa2ba44ca526124f462/Untitled.png)

평활화 손실이 있는 학습된 잠재 공간의 t-SNE 시각화 평활도 손실이 있는 경우와 없는 경우. 평활도 손실이 있는 경우, 결과 잠상 공간은 보다 연속적입니다.

마지막으로 저자들은 딥 리버스 렌더링 네트워크가 고해상도 SVBRDF와 함께 작동하는 능력에 대해 설명합니다. 완전 컨볼루션 자동 인코더는 입력 SVBRDF를 고해상도로 인코딩할 수 있어 더 큰 잠재 코드를 생성하고 네트워크가 고해상도 SVBRDF에서 효과적으로 작동할 수 있도록 합니다.

5

이 섹션에서는 많은 역 렌더링 방법에서 좋은 결과를 얻기 위한 최적화 프로세스 초기화의 중요성에 대해 설명합니다. 이 최적화 프로세스에서 자동 인코더의 역할을 확률적 용어를 사용하여 설명하려고 합니다.

베이지의 정리를 적용하고 각 이미지가 독립적이라고 가정하여 역 렌더링을 SVBRDF s의 조건부 확률을 최대화하는 것으로 표현하고 공식을 재작성합니다. 이 공식에는 두 가지 확률, 즉 SVBRDF가 주어진 이미지의 확률과 SVBRDF 자체의 확률이 포함됩니다.

입력 이미지가 많으면 렌더링 손실이 지배적이기 때문에 정규화 기법으로서 SVBRDF의 확률의 역할이 덜 중요해집니다. 그러나 입력 이미지의 수가 적으면 SVBRDF의 확률의 역할이 더욱 중요해집니다.

저자들은 자동 인코더가 SVBRDF의 확률을 제공하는 것이 아니라 SVBRDF의 공간에 대한 잠재적 임베딩을 제공한다고 설명합니다. 그러나 훈련 프로세스는 근처의 잠재 벡터가 유사한 SVBRDF로 디코딩되도록 잠재 공간을 구성합니다. 따라서 SVBRDF의 확률을 로컬에서 상수로 근사화합니다.

최적화를 위한 최적의 시작점은 분포의 모양이 렌더링 손실에 의해 지배되는 영역입니다. 그러나 이를 달성하려면 최대화하려는 확률에 가까운 시작점을 선택해야 하는데, 이는 실용적이지 않습니다.

대신 대략적인 프록시 확률을 최대화하고 사전 학습 기반의 단일 이미지 SVBRDF 추정 방법을 사용하여 최적화 프로세스를 시작할 것을 제안합니다. 실제로 이 접근 방식은 이러한 초기 SVBRDF에서 그럴듯한 솔루션으로 수렴할 수 있다는 것을 발견했습니다. 시작점이 서로 다른 5개의 입력 사진으로부터 최적화된 목재 재료의 예를 통해 이를 설명합니다. 이 방법은 임의의 시작점에서는 그럴듯한 결과를 생성하지 못하지만 단일 이미지 SVBRDF 추정치에서 초기화하면 성공합니다.

6

저자는 완전 컨볼루션 신경 인코더-디코더 네트워크에서 정보가 "병목 현상"을 통과할 때 디테일이 손실되는 문제에 대해 설명합니다. 많은 컨볼루션 네트워크에서 이 문제는 인코더에서 디코더로 직접 정보를 전달하는 스킵 연결을 사용하여 해결됩니다. 그러나 자동 인코더가 생성하는 잠재 공간이 주요 목표인 특정 애플리케이션에서는 스킵 연결을 사용할 수 없습니다.

결과적으로 디코딩된 SVBRDF(공간 가변 양방향 반사율 분포 함수)가 덜 세밀해집니다. 한 가지 해결책은 잠재 코드의 크기를 늘리는 것이지만, 이 경우 최적화된 SVBRDF에 바람직하지 않은 아티팩트가 발생할 수 있습니다.

대신 저자들은 반사율 맵을 직접 조정하는 개선 후처리 단계를 제안합니다. 이는 고전적인 역 렌더링과 유사하며, 손실된 디테일을 다시 도입하는 데 도움이 됩니다. 저자는 딥 리버스 렌더링 프레임워크의 솔루션이 이 프로세스를 위한 좋은 출발점이 될 수 있다고 주장합니다. 이미 좋은 솔루션에 가까워졌기 때문에 개선된 결과물에 수렴하는 데 적당한 수의 반복만 필요합니다.

예시를 통해 이 프로세스를 설명하면 원래 딥 리버스 렌더링 SVBRDF의 대부분의 기능을 유지하면서 누락된 디테일을 다시 도입할 수 있습니다.

7

저자의 프레임워크는 Tensorflow로 구현되었으며, Tensorflow에 내장된 레이어로 구성된 차별적인 렌더러를 포함합니다. 학습률은 0.0001, β1은 0.5로 설정하고 기타 하이퍼파라미터는 Tensorflow의 기본값으로 설정한 Adam 최적화 알고리즘을 사용하여 자동 인코더를 훈련합니다. 네트워크는 분산이 0.02인 제로 평균 정규 분포에서 무작위로 추출한 값으로 초기화되고, 64개의 샘플로 구성된 미니 배치를 사용하여 10만 번 반복 학습됩니다. 훈련에는 듀얼 NVidia GTX 1080Ti GPU 워크스테이션에서 약 16시간이 소요됩니다.

딥 리버스 렌더링 최적화도 텐서플로에서 구현되며 학습률은 0.001, β1은 0.5로 설정하고 다른 모든 하이퍼파라미터는 기본값으로 설정한 Adam 최적화를 사용합니다. Adam은 해상도에 관계없이 4K 반복으로 실행됩니다. 세분화도 비슷한 설정으로 Adam을 사용하여 수행됩니다. 효율성을 높이기 위해 다중 해상도 최적화 전략이 사용되며, 탑뷰를 다운샘플링하고 딥 인버스 렌더링 알고리즘을 수행하는 것으로 시작합니다. 그런 다음 출력은 업스케일링되어 다음 최적화 라운드의 시작점으로 사용됩니다. 이 프로세스는 목표 해상도에 도달할 때까지 계속됩니다. 저자는 이 접근 방식이 비슷한 결과를 유지하면서 계산 시간을 절반으로 줄인다는 사실을 발견했습니다.

그런 다음 저자는 공개적으로 사용 가능한 데이터 세트의 SVBRDF에서 결과의 품질과 정확성을 검증합니다. 딥 리버스 렌더링은 입력 사진과 더 정확하게 일치하고, 아티팩트가 적으면서도 더 그럴듯한 결과를 제공하며, 이전의 단일 이미지 방식에 비해 더 깨끗한 반사율 속성 맵을 생성합니다. 입력 사진의 수를 점진적으로 늘리면 결과의 정확도가 향상됩니다.

또한 저자들은 휴대폰 카메라로 캡처한 실제 재료 샘플에 대해 프레임워크를 테스트하여 재료와 입력 사진의 개수에 따라 그럴듯한 결과를 얻었습니다.

8

포스트 프로세싱 리파인딩을 사용한 딥 인버스 렌더링, 리파인딩을 사용하지 않은 딥 인버스 렌더링, 클래식 인버스 렌더링을 비교한 것입니다. 비교는 다양한 수의 입력 사진에서 소재 추정을 사용하여 이루어지며, 세 가지 소재가 선택되어 표시됩니다.

리파이닝이 없는 딥 리버스 렌더링은 그럴듯한 결과를 제공하지만 디테일이 부족할 수 있습니다. 리파이닝 단계를 추가하면 이러한 디테일을 되살릴 수 있습니다. 클래식 리버스 렌더링은 때때로 성공할 수 있지만 눈에 띄는 아티팩트로 실패하는 경우가 많으며, 이는 시작 지점이 대상 SVBRDF에서 너무 멀리 떨어져 있을 수 있음을 나타냅니다. 그러나 이미지 수가 증가함에 따라 클래식 리버스 렌더링은 아티팩트를 더 적게 생성합니다. 개선된 딥 리버스 렌더링은 일관되게 가장 우수한 성능을 보이며, 더 적은 수의 입력 사진으로 더 일찍 아티팩트 없는 결과를 생성하고 반사율 속성 맵과 렌더링된 이미지에서 더 낮은 오류를 생성하는 것으로 나타났습니다.

대규모 합성 데이터 세트에서 세 가지 방법 간의 정확도 차이에 대한 정량적 분석을 추가로 수행했습니다. 그 결과, 세분화를 사용한 딥 리버스 렌더링이 일반적으로 가장 좋은 결과를 생성하고 렌더링 오류를 낮추는 것으로 나타났습니다.

다양한 렌더링 방법을 비교하는 것 외에도 저다이나믹 레인지(LDR)와 하이다이나믹 레인지(HDR) 사진을 사용할 때의 영향도 조사했습니다. 복구된 LDR SVBRDF는 하이라이트의 일부 피크가 클램핑된 경우에도 매우 정확한 것으로 나타났습니다.

딥 리버스 렌더링의 맥락에서 다양한 손실 함수를 평가했습니다. 사용된 훈련 손실 함수는 규칙화와 디테일 유지 사이의 균형을 맞췄습니다.

또한 이 연구에서는 초기화 정확도의 영향을 조사했으며, 최적이 아닌 시작점이 최적화 프로세스에서 로컬 최소값으로 이어질 수 있음을 발견했습니다. 이 연구는 정확하고 그럴듯한 초기화 사용의 중요성을 주장했습니다.

마지막으로, 이 연구는 이 방법이 어느 정도의 조명 오류와 환경 조명에도 견고하다는 것을 보여주었습니다. 이 방법은 조명 위치에서 최대 10도의 오차와 중간 정도의 환경 조명을 처리할 수 있습니다. 그러나 이상적인 조건에서 벗어날 경우 표면 법선과 스페큘러 구성 요소에 오류가 발생할 수 있습니다.

9

이 논문에서는 여러 장의 사진에서 역 렌더링을 사용하여 고해상도 SVBRDF(공간 가변 양방향 반사율 분포 함수) 추정을 위한 새로운 프레임워크를 소개합니다. 이 방법은 입력 사진의 수에 따라 조정되며, 제약이 적은 촬영(예: 단일 사진)의 경우 그럴듯한 추정치를 제공하고 완전히 제약된 조건의 경우 정확한 재구성을 제공합니다.

이 프레임워크의 강점은 불안정한 수작업 휴리스틱이나 정규화 용어에 의존하지 않고 학습된 특징을 직접 최적화한다는 점입니다. 이는 AI 모델인 자동 인코더가 학습한 SVBRDF의 잠재 공간에서 최적화를 수행함으로써 이루어집니다. 또한 저자는 최적화를 용이하게 하기 위해 학습된 잠재 공간을 정규화하기 위한 몇 가지 개선 사항을 제안합니다.

이 프레임워크는 입력 사진의 수에 관계없이 고해상도 SVBRDF를 추정할 수 있으며, 기존의 딥러닝 기반 단일 이미지 SVBRDF 추정 방법의 품질을 개선할 수 있음을 입증했습니다.

향후 연구에서는 현재 기존 방법에 의존하고 있는 딥 리버스 렌더링 최적화의 초기화를 개선하는 것을 목표로 하고 있습니다. 초기화 네트워크와 자동 인코더의 공동 최적화는 향후 연구의 흥미로운 방향으로 제안됩니다.

- 요약
    
    문제의 공식화: 이 연구의 목표는 입력 사진의 수에 상관없이 역 렌더링을 사용하여 공간적으로 가변적인 양방향 반사율 분포 함수(SVBRDF)의 고해상도 추정치를 생성하는 것입니다.
    
    딥 역 렌더링 프레임워크 구축: 이 문제를 해결하기 위해 새로운 딥러닝 프레임워크가 설계되었습니다. 이 프레임워크는 학습된 SVBRDF의 잠재 공간에서 최적화합니다. 이 공간을 학습하기 위해 자동 인코더라는 신경망이 사용됩니다.
    
    자동 인코더 훈련하기: 자동 인코더는 SVBRDF와 저차원 잠재 공간 사이의 매핑을 생성하도록 학습됩니다. 그런 다음 이 훈련된 모델을 사용하여 SVBRDF의 특징과 규칙성을 학습합니다.
    
    개선 사항 구현: 연구원들은 학습된 잠재 공간을 규칙화하여 최적화를 용이하게 하기 위해 다양한 개선 사항을 제안합니다.
    
    딥 인버스 렌더링: 그런 다음 자동 인코더는 딥 리버스 렌더링 파이프라인에서 사용되며, 딥 리버스 렌더링 파이프라인은 원하는 수의 사진을 입력으로 받아 SVBRDF의 고해상도 추정치를 출력합니다.
    
    리파이닝 단계: 자동 인코더가 재현할 수 없는 디테일을 다시 도입하기 위해 포스트 프로세싱 개선 단계가 포함됩니다.
    
    비교 분석: 딥 인버스 렌더링 접근 방식을 기존 인버스 렌더링 및 딥 인버스 렌더링 + 리파이닝과 비교합니다. 분석 결과, 딥 리버스 렌더링과 리파이닝이 포함된 딥 리버스 렌더링이 일반적으로 가장 좋은 결과를 생성하는 것으로 나타났습니다.
    
    다양한 입력으로 테스트: 프레임워크는 저다이나믹 레인지(LDR) 및 하이다이나믹 레인지(HDR) 사진으로 테스트하여 견고성과 다용도성을 입증합니다.
    
    실패 분석: 또한 저자는 실패 사례를 시연하고 초기화가 충분히 정확하지 않거나 그럴듯하지 않았기 때문이라는 것을 확인합니다.
    
    앞으로의 전망: 연구자들은 향후 연구에서 딥 리버스 렌더링 최적화의 초기화 프로세스를 개선해야 한다고 제안합니다. 이 논문은 초기화 네트워크와 자동 인코더를 공동으로 최적화하는 것이 향후 연구의 유망한 방향이 될 수 있다고 제안하며 끝을 맺습니다.