# ImageProcessing

[Poisson Image Editing](3/Poisson%20Image%20Editing%2047a94f0f5f774af4938fffaec151e18f)

"수정된 푸아송 블렌딩(MPB)"이라는 새로운 이미지 블렌딩 기술을 소개합니다. 이 기술은 기존 푸아송 이미지 편집의 한계를 극복하고, 블리딩 아티팩트 및 색상 번짐 문제를 줄이는 데 중점을 둡니다. MPB는 두 이미지 간의 경계 픽셀에 대한 의존도를 균형있게 조정하고, 알파 블렌딩을 통해 자연스러운 합성 이미지를 생성합니다. 이 기술은 특히 동영상 처리에서 깜박임 없이 효과적인 오브젝트 복제를 가능하게 합니다. 실험 결과는 MPB가 기존 방법에 비해 더 나은 성능을 보여주며, 특히 대형 소스 이미지와 동영상 처리에 효과적임을 보여줍니다.

[LightGlue: Local Feature Matching at Light Speed](3/LightGlue%20Local%20Feature%20Matching%20at%20Light%20Speed%20ab7a8e6890084e0fa11d024b37cb2866)

이미지 매칭을 위한 새로운 딥러닝 기반 네트워크인 LightGlue를 소개합니다. 이 모델은 기존의 SuperGlue 모델을 개선한 것으로, 이미지 간의 로컬 특징 매칭을 더 빠르고 효율적으로 수행합니다. LightGlue는 이미지의 희소 특징을 빠르고 정확하게 매칭할 수 있는 적응적 정지 메커니즘을 갖추고 있어, 처리 속도와 정확성 사이의 균형을 효과적으로 조절합니다. 이 모델은 특히 3D 매핑, 카메라 추적 등의 컴퓨터 비전 작업에 유용하며, SuperGlue의 복잡성과 훈련 어려움을 해결하는 동시에 정확도를 유지합니다. 연구 결과에 따르면, LightGlue는 SuperGlue에 비해 상당히 개선된 속도와 성능을 보여주며, 다양한 시나리오에서 이미지 매칭 작업에 적합한 효과적인 도구임이 입증되었습니다.

[Real-ESRGAN: Training Real-World Blind Super-Resolution with Pure Synthetic Data](3/Real-ESRGAN%20Training%20Real-World%20Blind%20Super-Resolu%2043352acc8e3e488ea1c5d52591a14f5b)

Real-ESRGAN, 실제 저해상도 이미지를 개선하기 위한 모델을 소개합니다. Real-ESRGAN은 순수한 합성 데이터로만 훈련되어 있으며, 복잡한 고차 열화 프로세스와 sinc 필터를 사용하여 실제 이미지의 일반적인 아티팩트를 시뮬레이션합니다. 이 모델은 스펙트럼 정규화와 U-Net 판별기를 통합하여 훈련 중 안정성을 높였습니다. 실험 결과, 이 모델은 다수의 실제 이미지에서 세부 사항을 향상시키고 아티팩트를 효과적으로 제거했습니다. 하지만, 에일리어싱 문제와 훈련 중 발생한 아티팩트 등의 한계도 드러났습니다. Real-ESRGAN의 접근 방식은 실제 저해상도 이미지의 품질을 향상시키는 새로운 방법을 제시하며, 향후 연구를 위한 기반을 마련합니다.

[DeDoDe: Detect, Don't Describe -- Describe, Don't Detect for Local Feature Matching](3/DeDoDe%20Detect,%20Don't%20Describe%20--%20Describe,%20Don't%20D%204e9dd5b38b634b5aa41be592c654fc57)

기존의 특징 매칭 접근 방식과는 다른 새로운 방식을 소개합니다. 이 연구는 키포인트 감지와 설명을 분리하는 혁신적인 방법을 제안하여, 각각의 과정에 집중함으로써 전체적인 매칭 성능을 향상시키고자 합니다. DeDoDe는 대규모 움직임으로부터의 구조(SfM) 기반 3D 트랙을 사용하여 키포인트 감지를 최적화하며, 독립적으로 훈련된 설명자를 사용하여 감지된 키포인트를 더 잘 설명합니다. 실험 결과는 DeDoDe가 기존의 매칭 방식과 최신 기법 간의 성능 격차를 줄이는 데 성공했음을 보여줍니다. 이 연구는 기하학적 키포인트 감지와 설명의 결합이 아닌 분리를 통한 효율적인 접근 방식을 통해 매칭 프레임워크에 혁신을 가져왔습니다. 그러나 DeDoDe는 기본 디텍터에 의존적이며, 방향이나 스케일 추정을 하지 않는 등의 한계점도 가지고 있습니다.

[A Novel Approach to Transform Bitmap to Vector Image for Data Reliable Visualization considering the Image Triangulation and Color Selection](3/A%20Novel%20Approach%20to%20Transform%20Bitmap%20to%20Vector%20Ima%20bc33ce3cad434e65be78c5800426c717)

이 논문은 픽셀 수준에서의 초기 메쉬 구성, 메쉬 단순화, 그리고 색상 선택을 포함하는 새로운 비트맵에서 벡터 이미지로의 변환 방법을 제안합니다. 이 접근 방식은 기존의 방법들과 비교하여 메모리 사용량을 줄이고 처리 속도를 높이는 주요 차별점을 가지며, 큰 색상 불연속성 없이 "한 삼각형에 하나의 색상" 방법을 적용합니다.