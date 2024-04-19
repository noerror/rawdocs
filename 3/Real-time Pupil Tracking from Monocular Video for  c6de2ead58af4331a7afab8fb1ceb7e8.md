# Real-time Pupil Tracking from Monocular Video for Digital Puppetry

[https://arxiv.org/abs/2006.11341](https://arxiv.org/abs/2006.11341)

- Jun 2020

### 1. Introduction

이 논문에서는 사람의 라이브 영상을 사용하여 가상 인형에 실시간으로 애니메이션을 적용하는 작업에 대해 설명합니다. 이 작업을 위한 다양한 기술은 입력 데이터(예: 싱글뷰 비디오, 멀티뷰, 심도 이미지)와 방법(직접 최적화, 신경망을 사용한 예측, 휴리스틱)에 따라 분류할 수 있습니다.

![가상 아바타에서의 눈 블렌드 형상 추적의 최종 렌더링. 왼쪽 - 원본 이미지, 오른쪽 - 획득된 블렌드 형상에 의해 구동되는 아바타가 겹쳐진 이미지.](Real-time%20Pupil%20Tracking%20from%20Monocular%20Video%20for%20%20c6de2ead58af4331a7afab8fb1ceb7e8/Untitled.png)

가상 아바타에서의 눈 블렌드 형상 추적의 최종 렌더링. 왼쪽 - 원본 이미지, 오른쪽 - 획득된 블렌드 형상에 의해 구동되는 아바타가 겹쳐진 이미지.

저자는 별도의 센서나 보정 없이 모바일 장치에서 인형극을 만드는 데 중점을 둡니다. 저자들은 이전 기술의 단점인 눈동자의 위치를 추적하지 않아 표현력이 부족하다는 점을 지적합니다.

이를 해결하기 위해 연구진은 2단계 프로세스를 제안합니다. 먼저 신경망을 사용하여 동공의 위치를 예측합니다. 그런 다음 변위 기반 알고리즘을 적용하여 동공의 블렌드 모양을 추정합니다.

저자의 모델은 각 눈에서 동공, 홍채 바깥쪽 원, 눈 윤곽선 등 5개의 지점을 감지합니다. 휴리스틱을 사용하여 이러한 점의 위치를 기반으로 블렌드 모양 계수를 구한 다음 다양한 눈동자 움직임을 표현하는 데 사용할 수 있습니다.

이 시스템은 한 번에 한 프레임만 필요하며 심도 카메라와 같은 추가 센서가 필요하지 않습니다. 그 결과 그림 1과 같이 부드럽게 애니메이션이 적용되고 표현력이 뛰어난 가상 인형이 탄생합니다.

### 2. Neural network based eye landmarks

이 논문에서는 가상 인형의 애니메이션을 향상시키기 위해 눈의 랜드마크를 정확하게 예측하는 2단계 접근 방식에 대해 설명합니다. 먼저 최신 얼굴 메시 추정 파이프라인을 사용하여 사람의 얼굴에 대해 468개의 버텍스 메시를 예측합니다. 이로부터 눈 영역을 추출하여 더 작은 신경망에 공급하여 고품질의 랜드마크를 추가로 생성합니다.

![동공 블렌드 형상 획득의 개요. 자세한 내용은 본문을 참조하세요.](Real-time%20Pupil%20Tracking%20from%20Monocular%20Video%20for%20%20c6de2ead58af4331a7afab8fb1ceb7e8/Untitled%201.png)

동공 블렌드 형상 획득의 개요. 자세한 내용은 본문을 참조하세요.

구체적으로, 얼굴 메시 추정기에서 눈 랜드마크의 중심을 중심으로 64x64 픽셀 영역을 잘라내어 작은 신경망에 입력합니다. 이 네트워크는 동공 중심, 홍채 바깥쪽 원의 4개 지점, 눈 윤곽의 16개 지점을 포함한 주요 지점의 위치를 예측합니다.

이러한 추가 랜드마크는 원래의 얼굴 메시와 통합되어 478개의 정점을 포함하는 보다 정교한 얼굴 메시를 생성합니다. 이 랜드마크 중 5개는 특히 눈동자를 추적하여 가상 인형의 표현력을 향상시킵니다.

이 신경망은 GPU 백엔드가 있는 TensorFlow Lite와 인식 파이프라인을 구축하기 위한 프레임워크인 MediaPipe를 사용하여 모바일 장치에서 실행되도록 구축되었습니다.

모델 아키텍처 측면에서 신경망에는 최신 모델과 유사한 여러 가지 '병목 현상'이 있습니다. 각 랜드마크에 대한 x, y 좌표를 출력하는 완전히 연결된 레이어로 끝납니다. 이러한 설계는 복잡함에도 불구하고 메모리 사용량을 낮게 유지하며 CPU에서 실시간 성능을 구현하고 최신 휴대폰의 GPU 기능을 사용하면 더욱 빠른 성능을 구현할 수 있습니다. 런타임 측정값과 오류율은 표 1에 나와 있습니다.

### 3. Displacement-based pupil blend shape estimation

이 섹션에서는 정제된 얼굴 메시를 사용하여 눈동자에 대해 바깥쪽, 안쪽, 위쪽, 아래쪽을 가리키는 네 가지 블렌드 모양을 예측하는 방법에 대해 설명합니다. 이는 각 블렌드 셰이프에 대해 메시의 한 쌍의 정점을 선택하는 변위 기반 접근 방식을 사용하여 수행됩니다.

그런 다음 이 두 정점 사이의 변위를 측정하여 미리 결정된 두 가지 변위, 즉 블렌드 셰이프가 최소로 활성화된 변위(Dneutral)와 블렌드 셰이프가 최대로 활성화된 변위(Dactivated)를 사용하여 측정한 변위와 비교합니다. 이 비교를 기반으로 각 동공 블렌드 셰이프에 0과 1 사이의 스칼라 값이 할당됩니다.

그 후 반대되는 블렌드 셰이프 쌍을 두 개의 통합 블렌드 셰이프로 결합한 다음 평활화 프로세스를 거칩니다. 이 블렌드 모양 추정은 양쪽 눈 사이에서도 동기화됩니다. 파이프라인의 개요는 그림 2에 나와 있습니다.

이 알고리즘은 두 개의 변위(Dneutral 및 Dactivated)를 정의해야 하므로 휴리스틱이 핵심적인 역할을 합니다. 이러한 초기 변위는 대표적인 얼굴 메시 데이터 세트를 기반으로 추정되지만 모든 개별 변형을 포착할 수는 없습니다.

이를 극복하기 위해 실시간 보정 단계가 도입되었습니다. 이 단계에서는 모든 반복에서 변위를 추적하는 표준 점수 계산 알고리즘을 사용하여 지정된 신뢰 구간 내의 변위를 '신뢰할 수 있는 변위' 원형 버퍼에 추가합니다. 그런 다음 보정된 변위는 이러한 신뢰할 수 있는 변위의 평균으로 계산되며, 표준 편차는 다음 반복을 위한 신뢰 구간으로 사용됩니다. 이 접근 방식은 사람마다 다른 변위 변화를 고려하여 시스템의 신뢰도를 향상시킵니다. 알고리즘 1에서 이 프로세스에 대한 자세한 내용을 확인할 수 있습니다.

### 4. Datasets and training

눈 주변 지점의 2D 위치를 예측하기 위한 신경망을 훈련하기 위해 전 세계에서 수집한 데이터 세트에서 약 20,000개의 수동 주석이 달린 이미지를 사용했습니다. 이러한 이미지에는 견고성을 보장하기 위해 아핀(회전, 뒤집기) 및 색상 변경(색조, 채도, 비선형 매핑, 사실적인 카메라 노이즈 주입)을 포함한 다양한 변환을 적용했습니다. 네트워크는 Adam 옵티마이저를 사용하여 250개의 에포크에 걸쳐 훈련되었습니다. 손실 함수로 눈의 눈금을 손실에 포함시키지 않도록 눈 간 거리(MSE IED)로 정규화된 평균 제곱 거리를 사용했습니다.

이 섹션에서는 표준 점수 필터 알고리즘도 소개합니다. 이 필터는 현재 변위(Dcurrent)와 평균 변위(Dmean)의 차이를 계산합니다. 이 차이가 정의된 간격 내에 있으면 현재 변위가 신뢰할 수 있는 것으로 간주되어 '신뢰할 수 있는 변위' 목록에 추가됩니다. 그렇지 않은 경우, 현재 변위와 평균 변위의 가중치 평균으로 새로운 신뢰할 수 있는 변위가 계산되며, 가중치는 영향력 계수(Finfluence)에 의해 결정됩니다. 그런 다음 다음 반복을 준비하기 위해 이 계수를 약간 줄입니다.

최종 보정된 변위(Dcalibrated)는 신뢰할 수 있는 변위의 평균으로 계산되며, 다음 반복에 사용하기 위해 이 신뢰할 수 있는 변위 목록을 기반으로 새로운 분산이 계산됩니다. 이 접근 방식은 평균과 분산을 실시간으로 지속적으로 조정하여 시스템이 변위의 변화에 적응할 수 있도록 합니다.

### 5. Results

훈련된 모델의 성능을 정량화하기 위해 약 4000개의 수동 주석이 달린 이미지가 사용되었습니다. 이 모델은 테스트 데이터 세트에서 7.16%의 눈 간 거리(MAD IED)로 정규화된 평균 절대 거리(Mean Absolute Distance)를 달성했습니다. 비교를 위해 수동 주석의 오류는 간단한 사용 사례(얼굴이 정면으로 회전하는 경우)의 경우 5.73%, 더 복잡한 시나리오의 경우 7.04%였습니다. 이러한 오류는 동일한 이미지에 대해 서로 다른 주석을 사용하여 측정되었습니다.

얼굴 메시와 눈 개선 모델의 추론 속도도 다양한 휴대폰 모델에서 평가되었으며, 그 결과는 표 1에 나와 있습니다. 이를 통해 모델의 성능과 모바일 장치에서 실시간으로 사용할 수 있는 가능성을 더 잘 이해할 수 있습니다.

### 6. Conclusion

결론적으로, 저자들은 모바일 디바이스의 라이브 비디오에서 실시간 동공 추적을 위한 새로운 접근 방식을 제시했습니다. 이 시스템은 사전 보정 없이 단일 카메라 이미지에서 동공 블렌드 모양을 추정할 수 있는 포괄적인 파이프라인을 제공합니다. 따라서 기존의 모든 블렌드 셰이프 구현과 통합할 수 있어 다용도로 사용할 수 있습니다. 또한 이 시스템은 가상 퍼펫의 눈동자 움직임을 정확하게 제어할 수 있는 즉시 사용 가능한 솔루션을 제공합니다.