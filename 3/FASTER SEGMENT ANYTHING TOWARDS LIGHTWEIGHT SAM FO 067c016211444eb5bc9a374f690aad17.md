# FASTER SEGMENT ANYTHING: TOWARDS LIGHTWEIGHT SAM FOR MOBILE APPLICATIONS

[https://arxiv.org/abs/2306.14289](https://arxiv.org/abs/2306.14289)

1 Introduction

2023년에 Zhang 등이 개발한 자연어 처리(NLP) 모델인 ChatGPT는 인공 지능 생성 콘텐츠(AIGC) 분야에 큰 영향을 미쳤습니다. 이 모델은 대규모 텍스트 데이터 세트에 대해 학습된 GPT 시리즈 모델을 기반으로 합니다. 이러한 성공에 힘입어 메타 리서치 팀은 '세그먼트 애니씽'(SAM) 프로젝트를 개발하게 되었습니다. 비전 기반 AI 모델인 SAM은 이미지 인코더와 텍스트 가이드 마스크 디코더를 결합하여 AI의 비전을 크게 도약시켰습니다.

SAM이 주목을 받은 이유는 자연어 처리와 마찬가지로 비전 분야에서 기초 모델과 신속한 엔지니어링을 결합한 최초의 모델이었기 때문입니다. 또한 SAM은 NLP의 라벨 예측과 유사한 기본적인 비전 작업인 라벨 없는 세그먼테이션을 수행했습니다. 또한 텍스트 가이드 세분화 및 세분화된 이미지 편집과 같은 고급 비전 애플리케이션을 위한 다른 모델과 호환이 가능했습니다. 하지만 SAM은 이미지 인코더로 인해 연산량이 많아 모바일이나 기타 리소스가 제한된 디바이스에는 적합하지 않았습니다.

이 문제를 해결하기 위해 저자들은 무거운 이미지 인코더를 더 작은 버전으로 대체하여 모바일 기기에 더 적합한 모델을 만드는 MobileSAM이라는 경량 버전의 SAM을 제안했습니다. MobileSAM을 만드는 과정에는 기본 이미지 인코더에서 더 작은 인코더로 지식을 추출한 다음 마스크 디코더를 새롭고 가벼운 이미지 인코더에 맞게 미세 조정하는 작업이 포함되었습니다.

![Untitled](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled.png)

인코더 파라미터를 100배, 총 파라미터를 60배 줄였음에도 불구하고 MobileSAM은 기존의 무거운 SAM 모델과 동등한 성능을 발휘합니다. 이 최적화된 버전은 단일 이미지를 약 10밀리초 만에 처리하므로 동시 실행되는 FastSAM 모델보다 약 5배 빠르고 크기는 7배 작아져 모바일 AI 애플리케이션에 중요한 진전을 이루었습니다.

2 Related work

올해 4월 초에 출시된 이후 "Segment Anything"(SAM) 프로젝트는 다양한 관점에서 광범위하게 연구되고 있습니다. 한 가지 연구는 의료 영상과 위장된 물체 또는 투명한 물체 탐지를 포함한 실제 시나리오에서 SAM의 성능을 조사하는 것이었습니다. SAM은 일반적으로 성능이 우수하지만 이러한 까다로운 상황에서는 어려움을 겪는 것으로 나타났습니다.

또한 SAM의 실용성을 개선하기 위한 연구도 진행 중입니다. 예를 들어, Attack-SAM은 SAM의 출력 마스크가 적대적인 공격을 통해 조작될 수 있음을 보여주었습니다. 또한 Qiao 등의 연구에서는 적대적 교란의 경우를 제외하고 SAM의 높은 견고성을 보여주었습니다.

일부 연구자들은 SAM의 다용도성을 보여주는 데 주력하기도 했습니다. 예를 들어, Grounded SAM은 SAM과 Grounding DINO를 결합하여 텍스트 입력 기반 세분화를 가능하게 했습니다. 다른 연구에서는 의미론적 세분화를 위해 SAM을 CLIP과 같은 모델과 결합했습니다. 이 외에도 이미지 편집, 인페인팅 작업, 동영상 내 객체 추적, 3D 객체 재구성 등 다양한 분야에서 SAM이 사용되어 그 폭넓은 적용 가능성을 입증했습니다.

SAM의 핵심 구성 요소인 비전 트랜스포머(ViT)의 경우, 모바일 비전 애플리케이션을 위해 가볍고 효율적으로 만들기 위해 많은 노력을 기울여 왔습니다. 이 개념은 매개변수와 계산 시간을 줄이기 위해 깊이별 컨볼루션과 포인트별 컨볼루션의 조합에 의존하는 MobileNet이 그 예입니다. 리소스가 제한된 디바이스의 모델 속도를 향상시키기 위해 더 작은 버전의 ViT를 도입하기 위한 추가 작업이 진행 중입니다. 이러한 노력은 리소스가 제한된 모바일 기기에 적합한 차세대 SAM을 만들기 위해 유니티가 제안한 분리 증류 방식과도 일치합니다.

3 Mobile-Friendly SAM

중점적으로 살펴볼 모델인 SAM(Segment Anything Model)은 ViT 기반 이미지 인코더와 프롬프트 가이드 마스크 디코더로 구성됩니다. 인코더는 입력에서 이미지 임베딩을 생성한 다음 디코더에서 처리하여 마스크를 생성합니다. 이 시스템을 사용하면 동일한 프롬프트에서 여러 개의 마스크를 생성할 수 있으므로 잠재적인 모호성을 해결할 수 있습니다.

이 프로젝트의 목표는 가볍고 빠른 형식으로 만족스러운 성능을 제공하는 모바일 친화적인 버전의 SAM(MobileSAM)을 만드는 것입니다. 기존 SAM의 마스크 디코더는 이미 경량화되었지만, 이미지 인코더는 600만 개 이상의 파라미터를 가진 ViT-H를 기반으로 하고 있어 모바일 기기에서 사용하기에는 너무 무겁습니다. 따라서 모바일 친화적인 SAM을 만드는 핵심은 SAM의 원래 기능이나 특성을 잃지 않으면서 무거운 이미지 인코더를 더 가벼운 것으로 대체하는 데 있습니다.

![Figure 2: SAM의 결합 지식 증류. 왼쪽 하위 그림은 완전히 결합된 증류를 나타내며, 오른쪽은 반 결합 증류를 나타냅니다.](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled%201.png)

Figure 2: SAM의 결합 지식 증류. 왼쪽 하위 그림은 완전히 결합된 증류를 나타내며, 오른쪽은 반 결합 증류를 나타냅니다.

우리가 제안한 방법은 "결합 증류"라는 프로세스를 사용하여 더 작은 이미지 인코더로 새로운 SAM을 재훈련하는 것입니다. 그러나 ViT-H 이미지 인코더로 SAM을 훈련하려면 상당한 리소스가 필요하기 때문에 많은 연구자들에게 부담이 될 수 있습니다. 따라서 더 작은 이미지 인코더를 사용하고 제공된 분할 데이터 세트를 사용하여 새로운 SAM을 재훈련할 것을 제안합니다. 이 프로세스는 기본적으로 ViT-H 기반 SAM의 지식을 더 작은 이미지 인코더가 있는 SAM으로 이전하는 것입니다.

이미지 인코더와 디코더를 함께 최적화해야 하는 과제를 극복하기 위해 이 작업을 이미지 인코더 증류와 마스크 디코더 미세 조정이라는 두 가지 하위 작업으로 나눌 것을 제안합니다. 먼저 ViT-H에서 더 작은 인코더로 지식을 이전하여 이미지 인코더에서 증류를 수행합니다. 이미 경량화된 기존 SAM의 마스크 디코더 아키텍처는 그대로 유지할 계획입니다. 그런 다음 복사 및 고정된 마스크 디코더로 이미지 인코더의 최적화를 수행하여 불량한 이미지 인코더로 인해 마스크 디코더의 품질이 저하되는 것을 방지할 수 있습니다. 우리는 이 프로세스를 반결합 증류라고 부릅니다.

![SAM을 위한 분리된 증류.](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled%202.png)

SAM을 위한 분리된 증류.

그러나 이 방법은 프롬프트 선택의 무작위성으로 인해 마스크 디코더가 가변적이어서 최적화 난이도가 높아지는 등 여전히 어려운 점이 많다는 것을 발견했습니다. 그 결과, 결합 디코더를 사용하지 않고 원본 SAM의 ViT-H에서 직접 소형 이미지 인코더를 증류하는 분리 증류를 제안했습니다. 이 방법을 사용하면 기존 접근 방식에서 마스크 예측에 필요한 초점 손실과 주사위 손실의 조합 대신 간단한 MSE 손실을 사용할 수 있습니다.

예비 평가에서는 결합 증류와 분리 증류를 비교했습니다. 그 결과, 분리 증류는 결합 증류에 비해 1% 미만의 계산 리소스를 필요로 하면서도 우수한 mIoU 성능을 달성하는 것으로 나타났습니다. 향후 연구에서는 제안한 분리 증류 방법을 사용하여 TinyViT 기반 모델을 실험할 계획입니다.

4 Experiments

개략적인 연구에 따르면 MobileSAM이라는 세그먼트-애니씽 모델(SAM)을 경량화할 것을 제안합니다. 모바일 기기용으로 설계된 이 기술은 기존 SAM보다 만족스러운 성능과 빠른 작동 속도를 제공합니다. 이 프로젝트의 접근 방식은 SAM의 모든 원래 기능을 유지하면서 무거운 이미지 인코더(600M 이상의 파라미터를 가진 ViT-H)를 가벼운 인코더로 대체하는 데 있습니다.

두 가지 증류 방법, 즉 결합 증류와 분리 증류가 검토되었습니다. 결합 증류는 이미지 인코더와 디코더 간의 상호 의존성과 최적화로 인해 어려움이 있습니다. 따라서 연구원들은 증류 과정을 이미지 인코더 증류와 마스크 디코더 미세 조정이라는 두 가지 작업으로 나눌 것을 제안했습니다. 이 접근 방식을 디커플링 증류라고 합니다.

![하나의 점을 프롬프트로 사용한 마스크 예측](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled%203.png)

하나의 점을 프롬프트로 사용한 마스크 예측

개념 증명을 위해 더 작고 효율적인 이미지 인코더(ViT-Tiny)를 사용하여 실험을 수행했습니다. 분리 증류 접근 방식은 필요한 계산 리소스를 크게 줄이면서도 결합 증류에 비해 우수한 성능을 달성했습니다. MobileSAM의 훈련 및 평가는 단일 GPU에서 SA-1B 데이터 세트의 1%를 사용하여 수행되었으며, 기존 SAM과 비슷한 수준의 만족스러운 마스크 예측을 달성했습니다.

![박스를 프롬프트로 사용한 마스크 예측](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled%204.png)

박스를 프롬프트로 사용한 마스크 예측

또한 MobileSAM을 SAM을 간소화하기 위한 다른 시도인 FastSAM과 비교했습니다. MobileSAM은 파라미터 수가 1,000만 개 미만임에도 불구하고 우수한 성능을 보였으며, FastSAM보다 4배 더 빨랐습니다. 또한 MobileSAM은 FastSAM보다 훨씬 더 큰 mIoU를 보여 원래 SAM의 마스크 예측과 더 유사한 것으로 나타났습니다.

'모든 것을 세그먼트화' 성능 측면에서 MobileSAM은 원본 SAM과 매우 잘 일치하며, 일부 오브젝트를 예측하지 못하고 경계가 매끄럽지 않은 마스크를 생성하는 FastSAM보다 성능이 뛰어납니다. 전반적으로 MobileSAM은 성능에 큰 타협 없이 모바일 애플리케이션을 위한 효율적인 SAM의 대안을 제공하는 것으로 보입니다.

![모든 것을 세그먼트하는 결과의 비교](FASTER%20SEGMENT%20ANYTHING%20TOWARDS%20LIGHTWEIGHT%20SAM%20FO%20067c016211444eb5bc9a374f690aad17/Untitled%205.png)

모든 것을 세그먼트하는 결과의 비교

5 Conclusion

결론적으로, 이 연구는 무거운 이미지 인코더를 더 가볍고 효율적인 대안으로 대체하여 세그먼트-애니싱 모델(SAM)을 보다 모바일 친화적으로 만드는 것을 목표로 했습니다. 이 새로운 모델에 기존 SAM 논문에서와 동일한 훈련 접근법을 사용하면 특히 제한된 훈련 리소스 조건에서 만족스럽지 못한 성능을 보이는 것으로 관찰되었습니다.

연구진은 이미지 인코더와 마스크 디코더의 결합 최적화가 주요 과제임을 확인했습니다. 이에 대한 해결책으로 연구진은 원래 SAM의 이미지 인코더(ViT-H)에서 경량 이미지 인코더로 지식을 증류하는 분리 증류 방법을 제안했습니다. 이 접근 방식은 원래 SAM의 마스크 디코더와 자동으로 호환되는 경량 이미지 인코더를 생성했습니다.

그 결과 MobileSAM 모델은 60배 이상 작아졌지만 성능은 오리지널 SAM과 동등합니다. 동시 실행되는 FastSAM 모델보다 성능이 뛰어나며 4배 더 빠르고 7배 더 작아 모바일 애플리케이션에 이상적입니다.

MobileSAM의 중요한 측면은 이미지 인코더만 교체할 뿐 원본 SAM 파이프라인을 유지한다는 점입니다. 따라서 기존 SAM 기반 프로젝트에서 최소한의 노력으로 헤비급 SAM에서 경량 버전으로 원활하게 전환할 수 있습니다. 따라서 MobileSAM은 모바일 이미지 세분화를 위한 매우 효율적이고 실용적인 솔루션입니다.

- 정리
    
    소개 및 동기
    이 백서는 이미지 분할에는 매우 효과적이지만 모바일 디바이스에서는 리소스를 너무 많이 사용한다는 문제점을 파악하는 것으로 시작합니다. 이는 이 모델이 사용하는 무거운 이미지 인코더(ViT-H) 때문입니다. 연구진은 모바일 애플리케이션에 적합한 더 가벼운 버전의 SAM을 개발하는 것을 목표로 하고 있으며, 이를 MobileSAM이라고 부릅니다.
    
    접근 방식
    연구진은 기존 SAM의 이미지 인코더를 더 가벼운 버전으로 교체하여 이를 달성하기 위한 전략을 제안합니다. 크기 대비 성능이 뛰어난 ViT-Tiny를 백본으로 사용하기로 결정했습니다. MobileSAM은 기존 SAM의 파이프라인을 그대로 유지하면서 이미지 인코더만 교체합니다.
    
    방법론
    연구원들은 분리 증류 방식을 개발하여 기존 SAM의 ViT-H 이미지 인코더에서 얻은 지식을 더 가벼운 버전으로 추출합니다. 이미지 인코더는 이미지의 해상도를 점진적으로 낮추는 4단계를 거치며, 전체 프로세스는 원본 SAM의 마스크 디코더와의 호환성을 보장하는 방식으로 수행됩니다.
    
    실험 설정 및 결과
    경량 인코더는 SA-1B 데이터 세트의 하위 집합에 대해 훈련되었으며, 그 결과 MobileSAM은 원본 SAM과 동등한 성능을 발휘하는 것으로 확인되었습니다. 또한 동시 모델인 FastSAM과 MobileSAM을 비교한 결과, 비슷한 성능을 제공하면서도 속도와 크기 면에서 MobileSAM이 FastSAM보다 우수한 것으로 나타났습니다. 또한 훈련 계산이 SAM의 성능에 미치는 영향을 조사하기 위해 절제 연구도 수행했습니다.
    
    결론
    연구진은 크기가 더 작고 성능은 기존 SAM과 비슷한 MobileSAM이 모바일 애플리케이션에 더 적합하다는 결론을 내렸습니다. 또한 FastSAM보다 더 빠르고 작습니다. 이 연구는 기존 SAM 기반 프로젝트가 플러그 앤 플레이 특성으로 인해 거의 추가 작업 없이 MobileSAM으로 원활하게 전환할 수 있다는 주장으로 결론을 내립니다.