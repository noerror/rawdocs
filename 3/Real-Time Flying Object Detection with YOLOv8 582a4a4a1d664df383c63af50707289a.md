# Real-Time Flying Object Detection with YOLOv8

[https://arxiv.org/abs/2305.09972](https://arxiv.org/abs/2305.09972)

[https://docs.ultralytics.com/](https://docs.ultralytics.com/)

1. Introduction/Background/Motivation

최근 드론과 소형 무인 항공기가 암살 시도, 마약 밀수, 감시와 같은 악의적인 활동에 사용되면서 효율적인 드론 탐지의 중요성이 강조되고 있습니다. 현재 대부분의 드론 탐지 기술은 드론이 작고 은밀하며 소음이 적기 때문에 신뢰할 수 있는 결과를 제공하는 데 어려움을 겪고 있습니다. 게다가 드론은 전자기 신호가 작기 때문에 최신 레이더 시스템을 회피하는 경우가 많습니다. 미국 국경 순찰 지역과 같이 복잡한 사막 배경과 드론과 카메라 간의 상당한 거리가 효과적인 탐지를 방해하는 환경에서는 이 문제가 더욱 심각해집니다.

이 문제를 해결하기 위해 저자는 다른 연구자들이 추가 개발 또는 직접 구현에 사용할 수 있도록 일반화된 실시간 비행 물체 감지 모델을 만들 것을 제안합니다. 이 모델은 합리적인 프레임 속도를 유지하면서 더 높은 해상도에서 다양한 클래스에 대해 우수한 성능을 발휘하도록 설계되었습니다. 이 모델은 40개의 비행 물체 카테고리로 구성된 다양한 데이터 세트를 학습하여 비행 물체의 추상적인 특징 표현을 학습하도록 유도합니다. 그런 다음 전이 학습을 적용하여 오클루전, 작은 공간 크기, 회전 등이 특징인 '실제' 환경에 맞게 모델을 조정합니다.

성능을 최적화하기 위해 저자는 빠른 추론 속도로 유명한 최신 단일 샷 검출기인 YOLOv8을 사용하지만, 아직 아키텍처와 기능을 설명하는 공식 문서가 발표되지는 않았습니다.

그러나 실시간 객체 감지는 객체의 공간 크기, 종횡비, 추론 속도 및 노이즈가 다양하기 때문에 복잡합니다. 특히 위치, 크기, 회전, 궤적이 빠르게 변하는 비행 물체의 경우 더욱 그렇습니다.

저자의 초기 모델은 다양한 비행 물체의 이미지 15,064장으로 구성된 데이터셋을 80%의 훈련과 20%의 검증 비율로 학습했습니다. 2022년에 생성된 이 데이터 세트는 다운로드 횟수가 15회에 불과하며, 클래스 불균형이 있는 롱테일 분포를 나타냅니다.

또한 저자들은 전이 학습을 위해 더 먼 거리에 있는 비행 물체에 초점을 맞춘 두 번째 데이터 세트를 사용했습니다. 이 데이터 세트는 11,998개의 이미지로 구성되어 있으며 2022년에 게시되었으며, 이 연구 당시 다운로드 횟수는 5회에 불과했습니다.

2. Approach

연구진은 드론 및 기타 비행 물체를 감지하고 분류하기 위해 이 모델에 대한 공식적인 연구 논문이 없음에도 불구하고 YOLOv8(You Only Look Once 버전 8) 모델을 사용하기로 결정했습니다. 이러한 결정은 이 모델의 뛰어난 비행 물체 감지 성능과 추론 속도 및 정확도의 균형을 고려한 결과입니다.

연구팀은 소형, 중형, 대형 모델을 비교한 후 중형 모델을 선택했습니다. 이 모델은 1080p HD 비디오에서 초당 50프레임(fps)의 적절한 속도로 작동하면서 우수한 정확도를 유지했습니다. 그런 다음 하이퍼 파라미터 튜닝에 대한 욕심 많은 접근 방식을 사용하여 이 모델을 최적화했습니다.

모델의 성능을 평가하기 위해 물체 감지 작업에서 모델의 정확도를 측정하는 평균 정밀도(mAP)라는 메트릭을 사용했습니다. 이는 데이터 세트의 클래스 불균형이 커서 대표성이 낮은 클래스에서 성능이 저하되는 경우가 많기 때문에 특히 중요했습니다.

그런 다음 연구팀은 모델 예측의 오류를 추정하는 데 사용되는 수학적 방법인 특정 손실 함수를 사용하여 YOLOv8 모델을 학습시켰습니다. 이 손실 함수에는 박스 손실(모델이 객체 주변의 경계 상자를 얼마나 잘 예측했는지), 분류 손실(모델이 객체를 얼마나 잘 분류했는지), 분포 초점 손실(모델이 불균형한 데이터 세트를 얼마나 잘 처리하는지 측정하는 지표)이 통합되었습니다.

그 결과, 최적화된 모델은 1080p 동영상에서 평균 0.685의 정밀도와 초당 50프레임의 처리 속도를 달성했습니다. 이는 이 모델이 드론 및 기타 비행 물체를 실시간으로 효과적으로 감지하고 분류할 수 있음을 보여줍니다.

3. Experiments and Results

3.1. 모델 혼동 및 진단
클래스 간 분산이 낮은 데이터 세트, 즉 서로 매우 유사하게 보이는 클래스를 처리할 때 객체 감지에 어려움을 겪습니다. 예를 들어, 모델은 비슷하게 생긴 두 항공기인 F-14와 F-18을 잘못 분류하는 경우가 많습니다. 연구팀은 욜로 시리즈 알고리즘을 위한 오픈 소스 툴박스인 MMYolo를 사용하여 활성화 맵을 만들어 모델이 이미지에서 어떤 부분을 중요하게 분류하는지 파악했습니다. 이 맵을 검토한 결과, 비슷한 특징 활성화가 F-14와 F-18을 혼동하는 원인이 될 수 있다는 사실을 발견했습니다.

연구팀이 사용한 모델인 YOLOv8은 CSPDarknet53을 백본으로 사용하여 여러 해상도에서 특징을 추출합니다. 이 모델에는 물체 모양과 텍스처를 감지하는 데 도움이 되는 반복 모듈과 여러 감지 헤드가 있습니다. 모델의 백본은 4개의 섹션으로 구성되며, 각 섹션은 c2f 모듈로 끝납니다. 각 c2f 모듈은 이미지의 다양한 측면을 점점 더 세밀하게 분석하며, 이 레이어의 활성화로 인해 유사하게 보이는 클래스 간에 혼동이 발생할 수 있습니다.

3.2. 모델 예제 및 결과
이 모델은 매우 작은 물체 감지, 배경에 섞여 있는 비행 물체 식별, 다양한 유형의 비행 물체 분류라는 세 가지 까다로운 조건에서 우수한 성능을 보였습니다. 이러한 어려움에도 불구하고 이 모델은 새, 드론, 여객기, 그리고 잘 알려지지 않은 V22 항공기를 인상적인 정확도로 정확하게 분류할 수 있었습니다.

3.3. 개선된 모델 - 전이 학습
이 모델은 실제 데이터 세트에 전이 학습을 적용한 후 비행 물체의 특징 표현을 효과적으로 추출했습니다. 이 모델은 훈련 과정에서 특정 클래스가 부족하거나 배경 조건이 까다로운 경우에도 새, 드론, 비행기, 헬리콥터와 같은 작은 물체를 감지하고 분류하는 데 탁월한 성능을 발휘했습니다. 이러한 어려운 조건에서도 뛰어난 성능을 보인다는 것은 이 모델이 전이 학습의 강력한 기반 역할을 한다는 것을 의미합니다. 개선된 모델은 190회 훈련 후 비행기, 헬리콥터, 드론 클래스 전반에서 0.835의 mAP50-95를 달성했습니다.

4. Model Architecture

2015년에 처음 도입된 YOLO(You Only Look Once) 모델은 실시간으로 작동하는 객체 감지 알고리즘으로, 이 분야에서 널리 사용되고 있습니다. 이 모델은 객체 감지를 원패스 회귀 문제로 해석하여 입력 이미지를 그리드로 나누고 한 번의 실행으로 바운딩 박스와 클래스 확률을 예측합니다. 첫 번째 반복인 YOLOv1은 당시 다른 모델에 비해 속도와 성능이 뛰어나다는 평가를 받았습니다.

![Untitled](Real-Time%20Flying%20Object%20Detection%20with%20YOLOv8%20582a4a4a1d664df383c63af50707289a/Untitled.png)

24개의 컨볼루션 레이어와 2개의 완전히 연결된 레이어로 구성된 YOLOv1은 ImageNet 2012 데이터세트에서 훈련되고 검증된 신경망을 사용합니다. 예측된 바운딩 박스 좌표, 클래스 확률, 그리고 특정 계수에 의해 조절되는 실측값 간의 차이를 기반으로 손실을 계산합니다.

Ultralytics에서 개발한 YOLOv5는 모델을 특징 추출을 위한 백본, 특징 정제를 위한 넥, 예측을 위한 헤드로 분리하는 보다 모듈화된 아키텍처를 도입했습니다. 백본인 Darknet53은 작은 필터 창과 잔여 연결로 특징 추출에 중점을 둡니다. 넥은 공간 피라미드 풀링 모듈과 CSP-경로 집계 네트워크를 사용하여 다양한 규모에 걸쳐 공간 및 의미 정보를 향상시킵니다. YOLOv5의 헤드는 세 가지 척도에서 바운딩 박스, 클래스 확률, 신뢰 점수를 예측하고 비-최대 억제(Non-maximum Suppression)를 사용하여 겹치는 바운딩 박스를 제거합니다.

최신 버전인 YOLOv8은 이전 버전의 아키텍처를 유지하면서 피처 피라미드 네트워크(FPN)와 경로 집계 네트워크(PAN)를 모두 활용하는 새로운 네트워크와 주석 프로세스를 간소화하는 새로운 라벨링 도구와 같은 개선 사항을 도입했습니다. FPN은 입력 이미지의 공간 해상도를 낮추는 동시에 특징 채널을 늘려 다양한 규모와 해상도로 물체를 감지합니다. PAN은 스킵 연결을 통해 서로 다른 네트워크 레벨의 특징을 집계하여 특징 캡처를 향상시킵니다.

![Untitled](Real-Time%20Flying%20Object%20Detection%20with%20YOLOv8%20582a4a4a1d664df383c63af50707289a/Untitled%201.png)

YOLOv8과 YOLOv5를 비교한 결과, YOLOv8은 이상값이 적고 mAP 측면에서 우수한 성능을 보여줍니다. 이러한 우수한 성능의 비결은 FAN 및 PAN 모듈을 사용하고 더 크고 다양한 데이터 세트에 대한 학습을 수행했기 때문입니다. 또한 새로운 라벨링 도구인 RoboFlow Annotate를 통합하고 Soft-NMS를 비롯한 고급 후처리 기법을 사용합니다. 3개의 출력 헤드가 있는 YOLOv5와 달리, YOLOv8은 앵커가 없는 감지 메커니즘을 사용해 출력 헤드가 하나뿐입니다.

YOLOv8의 물체 감지 속도는 YOLOv5보다 약간 느리지만, 최신 GPU에서 실시간으로 이미지를 처리할 수 있습니다. 두 모델 모두 훈련 세트의 무작위 이미지 4개를 하나의 모자이크 이미지로 결합하는 데이터 증강 기법인 모자이크 증강을 사용합니다.

- Yolo 개요
    
    "You Only Look Once"(YOLO)는 실시간 객체 감지를 위한 일련의 컨볼루션 신경망(CNN)입니다. 이미지와 동영상에서 객체를 식별하는 데 널리 사용되는 시스템으로, 이미지 픽셀부터 경계 상자 좌표 및 클래스 확률까지 객체 감지를 단일 회귀 문제로 간주하는 독특한 접근 방식을 사용합니다.
    
    YOLO의 작동 방식에 대한 자세한 설명은 다음과 같습니다:
    
    단일 패스 감지: R-CNN 및 그 변형과 같은 기존의 객체 감지 방법은 먼저 이미지에서 '관심 영역'을 선택한 다음 이러한 영역을 서로 다른 객체 카테고리로 분류하는 두 단계를 사용합니다. 이름에서 알 수 있듯이 YOLO는 다른 접근 방식을 취합니다. 전체 이미지를 한 번만 보고 물체가 어디에 있는지, 어떤 범주에 속하는지 동시에 예측합니다.
    
    그리드 시스템: YOLO는 입력 이미지를 S x S 그리드로 나눕니다. 각 그리드 셀은 특정 수의 경계 상자(상자 예측은 그리드 셀에 대한 상자의 너비, 높이 및 오프셋을 기준으로 이루어짐), 이러한 상자에 대한 신뢰 점수 및 클래스 확률을 예측합니다. 신뢰 점수는 개체가 상자 안에 있을 가능성과 상자가 얼마나 정확한지를 반영하며, 클래스 확률은 개체의 범주를 결정합니다.
    
    바운딩 박스 예측: 각 바운딩 박스 예측은 x, y, w, h 및 신뢰도의 다섯 가지 구성 요소를 제공합니다. (x, y) 좌표는 그리드 셀의 경계를 기준으로 한 상자의 중심을 나타냅니다. 너비와 높이(w, h)는 전체 이미지를 기준으로 예측됩니다. 신뢰도 점수(신뢰도)는 상자에 물체가 포함될 확률(물체 포함 여부 점수)과 예측된 상자가 물체에 얼마나 잘 맞는지를 나타냅니다.
    
    손실 함수: YOLO 알고리즘은 로컬라이제이션 손실(경계 상자의 오류), 분류 손실(클래스 예측의 오류), 신뢰도 손실(객체성 점수의 오류)을 고려하는 사용자 지정 손실 함수를 사용합니다.
    
    성능: YOLO는 실시간 성능으로 유명합니다. 객체 감지를 위해 2단계 접근 방식을 사용하는 방법(예: R-CNN 및 그 변형)보다 훨씬 빠릅니다. 하지만 작은 물체나 서로 밀접하게 그룹화된 물체에는 어려움을 겪습니다.
    
    YOLO는 초기 버전의 한계를 개선하기 위해 여러 차례 업데이트(예: 2023년 6월 기준 YOLOv2, v3, v4, v5, v8)를 거쳤으며, 각 업데이트에는 더 빠른 처리, 더 높은 정밀도 및 감지 정확도를 위한 새로운 기능이 도입되었습니다.