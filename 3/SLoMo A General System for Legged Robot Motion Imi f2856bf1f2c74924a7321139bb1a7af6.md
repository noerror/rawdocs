# SLoMo: A General System for Legged Robot Motion Imitation from Casual Videos

[https://arxiv.org/abs/2304.14389](https://arxiv.org/abs/2304.14389)

- Apr 2023

### 1. Introduction

로봇공학에서 가장 큰 도전 중 하나는 동물과 인간 수준의 민첩함을 가진 다리 로봇을 개발하는 것입니다. 이러한 목표를 달성하기 위한 접근 방식으로, 자연스러운 동작을 직접 모방하는 방법이 주목받고 있습니다. 이러한 모션 모방 절차는 일반적으로 세 단계로 구성됩니다: 첫째, 비디오나 이미지 시퀀스에서 모션 프리미티브를 추출하는 단계; 둘째, 이 모션 프리미티브를 로봇의 물리적 한계 내에서 처리하는 단계; 마지막으로, 이러한 움직임들을 로봇 하드웨어에서 실행하는 단계입니다.

그러나 이전의 모션 모방 연구는 주로 마커 기반, 다중 카메라 모션 캡처 시스템에 의존하고 있었으며, 이는 캡처된 동작의 다양성에 제한을 가지고 있었습니다. 효과적인 모션 전달 솔루션을 개발하는 데 있어서, 원시 비디오 데이터를 받아들여 실제 로봇에서 새로운 행동을 실행하는 것은 지금까지 도달하지 못한 목표였습니다.

본 논문에서는, 신경 렌더링 및 재구성, 궤적 최적화, 모델 예측 제어에서의 최근 발전을 활용하여, 저자들이 알기로는 최초로 단일 이동 카메라로 캡처된 인간 및 동물의 움직임 기술을 로봇 하드웨어로 전달할 수 있는 일반적인 프레임워크를 구축했습니다. 이 프레임워크는 세 가지 주요 모듈을 포함합니다: 1) 일상적인 비디오 영상으로부터 물리적으로 타당한 3D 키포인트 궤적을 생산하는 재구성 파이프라인; 2) 로봇의 물리적 한계를 존중하면서 키포인트 궤적을 밀접하게 모방하는 동적으로 실행 가능한 로봇 상태, 제어 및 접촉력 참조 궤적을 해결하는 궤적 최적화자; 3) 참조 궤적을 추적하기 위해 로봇 하드웨어에서 실시간으로 실행되는 모델 예측 제어기 (MPC).

본 연구의 접근 방식은 프레임워크의 모든 단계에서 로봇과 환경 간의 접촉 상호작용에 대해 명시적으로 고려하는 것이 특징입니다. 이를 통해 오프라인 참조 궤적을 하드웨어에서 안전하게 실행할 수 있으며, 온라인 MPC가 접촉 타이밍과 모델 불일치에 대해 강건합니다. 또한, 모델 기반 궤적 생성 및 제어 방법을 사용함으로써 현재의 블랙박스 강화 학습 접근법으로는 달성하기 어려운 각 파이프라인 단계에서의 설명 가능성을 유지할 수 있습니다. 마지막으로, 본 연구는 로봇 형태학에 대한 일반성 측면에서 이전 연구들과 차별화됩니다. 3D 재구성 파이프라인, 궤적 최적화자, MPC는 모두 로봇 형태에 구애받지 않으며, 이는 인간에서 인간형 로봇으로, 동물에서 사족보행 로봇으로의 동작 전달을 단일 통합된 프레임워크 내에서 가능하게 합니다.

### 2 BACKGROUND AND RELATED WORK

이 장에서는 인간 및 동물 모션 캡처, 접촉을 통한 궤적 최적화, 그리고 다리 로봇을 위한 온라인 제어와 관련된 문헌을 검토합니다.

A. 인간 및 동물 모션 캡처

- **MoCap 시스템:** 영화 산업 및 연구실에서 널리 채택되어, 인간 및 동물의 몸 움직임을 연구하는 데 중요한 역할을 합니다.
- **관련 작업:** 다양한 연구에서 MoCap 데이터에 의존하고 있으나, 마커 기반 MoCap 시스템은 캡처 범위가 제한적이고 카메라 구성 변경에 매우 민감하여 실외 사용이 어렵습니다.
- **도전 과제:** 비용이 많이 들고, MoCap 시스템을 사용하여 연구할 수 있는 환경과 대상의 다양성에 한계가 있습니다.
- **대안적 접근법:** 최근에는 마커가 없는 모션 캡처가 인기를 얻고 있지만, 이들 시스템 역시 실내 스튜디오와 수백 대의 동기화된 카메라가 필요하여, 실세계 대상과 행동으로 일반화하기 어렵습니다.

B. 접촉을 통한 궤적 최적화

- **궤적 최적화:** 로봇 시스템을 위한 동적 행동을 설계하는 데 강력한 도구입니다. 로봇 동역학과 환경 제약 조건 아래에서 최적의 제어 시퀀스를 해결합니다.
- **접촉 문제:** 다리 로봇의 계획 및 제어에서 가장 어려운 문제 중 하나는 발이 환경과 접촉하거나 끊어질 때 발생하는 불연속적인 충격 이벤트에 대해 추론하는 것입니다.
- **접촉 스케줄:** 접촉 스케줄이 미리 정의되지 않은 경우, 접촉 상호작용은 암시적으로 해결되어야 하며, 이는 더 크고 도전적인 비선형 최적화 문제를 초래합니다.

C. 다리 로봇을 위한 온라인 제어

- **MPC 및 RL:** 로봇의 동적 보행 행동을 실행하기 위한 인기 있는 접근 방식으로 MPC(모델 예측 제어)와 RL(강화 학습)이 사용됩니다.
- **오프라인 최적화와 온라인 MPC:** 오프라인 궤적 최적화 및 온라인 MPC 파이프라인은 여러 인상적인 실제 로봇 행동을 가능하게 했습니다.
- **모델 프리 RL:** 경험을 수집하여 심층 신경망으로 표현된 피드백 제어 정책을 학습하려고 시도합니다.

### 3 VIDEO-TO-ROBOT MOTION TRANSFER

본 장에서는 실세계 비디오로부터 로봇 모션 모방을 위한 SLoMo 알고리즘을 소개합니다. 주요 세부 사항은 다음과 같습니다:

### A. 일상적인 비디오에서의 물리 정보 복구

- **목표:** 타겟 동물이나 인간의 3D 키포인트 궤적을 세계 좌표계에서 추정하는 것입니다.
- **접근법:** 예술적 형태와 키포인트 궤적을 동시에 재구성하며, 물리적 타당성을 위한 최적화를 포함합니다.
- **핵심 요소:**
    - **형태 모델:** 객체의 시각적 특성을 나타내는 다층 퍼셉트론(MLP)을 사용합니다.
    - **운동 골격 모델:** 목표 운동 골격 모델을 사용하여 키포인트 궤적을 계산합니다.
    - **차별화 가능한 볼륨 렌더링:** 객체 형태와 장면 모델을 사용하여 카메라 뷰 변환과 내부 매개변수에 따라 이미지를 차별화 가능하게 렌더링합니다.

B. 접촉 암시적 궤적 최적화

- **목적:** 단일 카메라 비디오에서 생성된 키포인트 궤적을 바탕으로 로봇 동역학과 접촉 제약 조건을 고려한 궤적 최적화 문제를 해결합니다.
- **접근법:** 접촉 암시적 궤적 최적화를 사용하여 로봇 상태, 제어 및 접촉력을 공동으로 해결합니다.
- **키 포인트:**
    - **비선형 프로그램:** 로봇의 비선형 동역학을 포함하고, 추출된 키포인트 상태를 추적하는 쿼드라틱 트래킹 비용을 최적화합니다.
    - **접촉 일정:** 접촉 일정을 미리 정의하지 않고 최적화기가 자동으로 실행 가능한 로봇 보행 시퀀스를 생성합니다.

C. 접촉 암시적 모델 예측 제어

- **목적:** 실시간으로 참조 궤적을 안정적으로 추적합니다.
- **방법:** 접촉 암시적 모델 예측 제어(CI-MPC)를 사용하여 로봇이 환경과 접촉하는 상황을 제어합니다.
- **핵심 요소:**
    - **시간 변동 LCP:** 참조 궤적에 대한 비선형 보완 문제를 로컬 근사화하여 해결합니다.
    - **트래킹 문제:** 상태 및 제어 결정 변수와 참조 상태 및 제어 궤적 간의 차이를 최소화합니다.

### 4 EXPERIMENTS AND RESULTS

본 장에서는 SLoMo 프레임워크의 다양한 로봇 시스템에 대한 실험 결과를 소개합니다. 주요 실험들은 다음과 같습니다:

A. 실험 설정

- **3D 재구성:** PyTorch를 사용하여 구현되었으며, 1분 길이의 비디오 처리에 8시간이 소요됩니다.
- **컴퓨터 구성:** 오프라인 궤적 최적화와 온라인 MPC는 인텔 i9-12900KS CPU와 64GB 메모리를 갖춘 워크스테이션 컴퓨터에서 실행됩니다.
- **하드웨어 실험:** Unitree Go1 사족보행 로봇을 사용합니다.

B. 사족보행 로봇 실험

- **실험 사례:**
    - **개 동작:** 물을 닿으려는 개 (dog-reach).
    - **고양이 동작:** 거실을 가로지르는 고양이 (cat pace).
    - **개 CPR 동작:** 인간에게 CPR을 하는 개 (dog CPR).
- **로봇 모델:** 로봇의 발을 점 질량으로 모델링하는 단순화된 동역학 모델을 사용합니다.
- **하드웨어 설정:** Unitree Go1 로봇에서 PD 컨트롤러를 사용하여 MPC 정책에 의해 계산된 힘과 발 위치를 추적합니다.

C. 휴머노이드 로봇 실험

- **실험 사례:**
    - **인간 스트레칭 동작:** 양팔을 들어올리는 스트레칭 동작을 모방합니다.
    - **점핑잭 동작:** 점핑잭 운동을 하는 인간을 모방합니다.
- **로봇 모델:** 발을 사각형 프리즘으로, 손을 점 질량으로 모델링하는 휴머노이드 동역학 모델을 사용합니다.

D. 강화 학습(RL) 정책과의 비교

- **목적:** 실험에서 강화 학습 방법을 SLoMo 방법과 비교합니다.
- **결과:** RL 정책은 일관된 성능을 보이지만, 시드 간 변동성이 높고, 모델 기반 최적화와 비교가 어렵습니다.

### 5 DISCUSSION AND CONCLUSIONS

본 장에서는 SLoMo 프레임워크의 주요 결과를 요약하고 향후 연구 방향을 논의합니다.

주요 결과

- **SLoMo 프레임워크:** 실세계 캐주얼 모노큘러 비디오에서 포착된 동물 및 인간 동작을 모방하는 로봇을 가능하게 하는 첫 번째 프레임워크입니다.
- **3D 재구성의 효과:** 최근의 3D 재구성 기술은 단일 RGB 비디오에서 물리적으로 타당한 동작 궤적을 추출하는 데 효과적입니다.
- **동적 실행 가능성:** 추출된 궤적은 물리 기반 롤아웃 비용에도 불구하고 노이즈가 많고 동적으로 실행 가능하지 않아, 실제 로봇에서 직접 실행하기에는 안전하지 않습니다.
- **궤적 최적화와 MPC:** 접촉 암시적 궤적 최적화와 모델 예측 제어를 사용하여, 행동의 주기성에 관계없이 인간 및 동물의 임의적인 행동을 일반화합니다.

한계 및 미래 연구

- **모델 단순화 및 가정:** 본 연구에서는 점 발 모델을 사용하여 사족보행 및 인간형 동역학을 나타냈으나, 향후 연구에서는 전체 몸체 동역학을 사용하여 로봇의 능력을 최대한 활용할 필요가 있습니다.
- **재구성의 계산 비용:** 재구성 단계가 계산적으로 많은 비용이 듭니다. 또한, 재구성된 캐릭터를 로봇의 운동학과 더 잘 맞도록 수동으로 조정해야 합니다. 이러한 문제를 자동화하고 재구성을 가속화하는 것이 중요합니다.
- **비디오 캐릭터와 로봇 간의 형태학 차이:** 비디오 캐릭터와 해당 로봇 간의 형태학적 차이를 원칙적이고 자동화된 방식으로 처리하는 것이 중요합니다.