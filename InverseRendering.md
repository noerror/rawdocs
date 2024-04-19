# InverseRendering

[Deep Inverse Rendering for High-resolution SVBRDF Estimation from an Arbitrary Number of Images](3/Deep%20Inverse%20Rendering%20for%20High-resolution%20SVBRDF%20%205716fe7aebe44fa2ba44ca526124f462)

다양한 수의 사진으로부터 고해상도 공간 가변 양방향 반사율 분포 함수(SVBRDF)를 추정하기 위한 새로운 딥러닝 프레임워크가 소개됩니다. 이 프레임워크는 자동 인코더를 활용하여 SVBRDF의 잠재 공간을 학습하고, 이를 최적화하여 정밀한 SVBRDF를 생성합니다. 제안된 방법은 기존의 단일 이미지 SVBRDF 추정 방법보다 성능이 뛰어나며, 특히 대상 자료가 훈련 데이터셋을 벗어난 경우에도 품질을 개선할 수 있습니다.

[[Continuous Remeshing For Inverse Rendering](https://onlinelibrary.wiley.com/doi/10.1002/cav.2101)](3/Continuous%20Remeshing%20For%20Inverse%20Rendering%20bbb48ceeeb4340ff9d45e2407386ac12)

역 렌더링 과정에서 삼각 메시를 최적화하는 새로운 방법을 제시합니다. 이 기술은 기존 방법보다 빠르고 정확하게 단순하고 복잡한 모델을 처리할 수 있으며, 메시 삼각 측량을 독립적으로 최적화하여 복잡한 메시를 재구성할 수 있습니다. 이 방법은 글로벌 자기 교차 감지와 토폴로지 변경과 같은 과제를 극복해야 하며, 물리 기반 셰이딩과 관련된 실제 문제에 대한 잠재적 적용 가능성을 강조합니다.

[Large Steps in Inverse Rendering of Geometry](3/Large%20Steps%20in%20Inverse%20Rendering%20of%20Geometry%202334c74aa8af48ad905b31c752419d88)

차별화 렌더링에서 발생하는 문제를 해결하고 지오메트리 최적화를 개선하는 새로운 기술을 제시합니다. 이 기술은 큰 그라데이션 단계와 메시 왜곡 문제에 대처하기 위해 라플라시안 정규화 대안을 사용합니다. 라플라시안 정규화를 통해 솔루션의 평활성을 개선하고, 강력한 이미지 재구성을 위한 기반을 마련합니다. 이 연구는 차별화 렌더링 분야에서의 큰 발전을 나타내며, 지오메트리 최적화 및 텍스처 재구성에 있어 향상된 성능을 보여줍니다.

[GAN2X: Non-Lambertian Inverse Rendering of Image GANs](3/GAN2X%20Non-Lambertian%20Inverse%20Rendering%20of%20Image%20GA%206274c5a6e8c840f68ab3e34ef73d8cfb)

생성적 적대 신경망(GAN)을 활용하여 페어링되지 않은 이미지에서 3D 형상, 알베도, 스페큘러 속성을 복구하는 새로운 비지도 역 렌더링 방법을 제시합니다. 이 기술을 통해, 다양한 시점이나 조명 조건에서의 오브젝트를 재렌더링할 수 있으며, 특히 싱글뷰 2D 이미지에서 3D 얼굴을 재구성하는 데 뛰어난 성능을 보입니다. GAN2X는 이미지의 미세한 디테일을 이해하고, 2D GAN을 3D GAN으로 변환하는 데도 유용할 수 있음을 시사합니다. 이러한 방법은 3D 오브젝트 조작 및 편집에 새로운 가능성을 열어줍니다.

[Normal Map Estimation in the Wild](3/Normal%20Map%20Estimation%20in%20the%20Wild%207035caee8fa740778ed0bfb2e4006517)

단일 RGB 이미지로부터 물체의 3D 기하학적 정보를 추출하는 노멀 맵 추정 방법을 소개합니다. 이 연구는 합성곱 신경망을 사용하여 단일 뷰 이미지를 처리하고, Pix2Pix 네트워크를 기반으로 한 아키텍처를 통해 기하학적 디테일을 보존하는 동시에 조명 변화에 강인한 노멀 예측을 가능하게 합니다. 네트워크 구조는 인코더와 디코더, 잔여 연결, 이중 선형 업샘플링을 포함하며, 최종 예측은 쌍곡탄젠트 함수로 정규화됩니다. 훈련은 합성 데이터와 쌍을 이루는 실측 노멀 맵을 사용하며, 여러 데이터 증강 기법과 아담 옵티마이저를 적용합니다. 결과적으로, 이 방법은 제어되지 않은 환경에서 촬영된 실제 사진을 사용하여 정확한 노멀 맵을 생성하고, 이미지 기반 외형 편집 작업에 유용하게 적용될 수 있음을 보여줍니다.

[Interactive Normal Reconstruction from a Single Image](3/Interactive%20Normal%20Reconstruction%20from%20a%20Single%20Im%20eeffb03ce8f246308d9f40b560f77d7e)

단일 이미지에서 표면 법선을 복구하기 위한 대화형 시스템을 소개하는 논문입니다. 이 시스템은 두 단계로 구성됩니다: 첫 번째 단계에서는 셰이딩으로부터의 모양(SfS) 알고리즘을 사용하여 이미지에서 고주파 디테일을 복구합니다. 두 번째 단계에서는 상대 법선을 조작하여 SfS 결과의 저주파 편향성을 수정하는 사용자 조작 시스템을 활용합니다. 이 접근 방식은 스파스 마크업을 사용하여 방향성 구에서 수행되며, 마크업 접근 방식은 최근의 법선 마크업 접근 방식에서 영감을 얻었습니다. 이 방법은 사람의 상호 작용과 이미지의 지오메트리 정보를 결합하여 복잡한 이미지 객체의 고품질 가시 표면 모델을 생성합니다.

[Graphics Capsule: Learning Hierarchical 3D Face Representations from 2D Images](3/Graphics%20Capsule%20Learning%20Hierarchical%203D%20Face%20Rep%20e4058f3876c84d1e90bcd7daa94a959e)

IGC-Net(해석 가능한 그래픽 캡슐 네트워크)이라는 비지도 방식의 컨볼루션 신경망을 소개합니다. 이 네트워크는 2D 얼굴 이미지에서 3D 표현을 학습하며, 얼굴을 여섯 개의 의미적으로 일관된 부분으로 분해합니다. IGC-Net은 크로스뷰 얼굴 인식에서 2D 자동 인코더보다 우수하며, 비지도 얼굴 분할 작업에서도 높은 성능을 보입니다. 제거 연구를 통해 네트워크의 의미 있는 부분 발견 기능의 중요성을 강조하며, 얼굴 인식, 분할, 포즈 추정 등의 얼굴 분석 작업에 적용 가능성을 제시합니다.

[FFHQ-UV: Normalized Facial UV-Texture Dataset for 3D Face Reconstruction](3/FFHQ-UV%20Normalized%20Facial%20UV-Texture%20Dataset%20for%203%20179a4ee0e2b74da2ac861d29635220bb)

단일 이미지로부터 얼굴의 3D 모양과 텍스처를 재구성하는 문제에 대해 논의합니다. 이 연구의 핵심은 얼굴의 정체성을 유지하면서 다양한 조명 조건에서도 고품질의 텍스처 맵을 복구하는 새로운 방법론을 제시하는 것입니다. 기존 방법론의 한계를 극복하기 위해, 저자들은 StyleGAN 기반 이미지 편집을 통해 하나의 자연 이미지에서 다중 뷰의 정규화된 얼굴을 생성하는 자동화된 파이프라인을 개발했습니다. 이를 통해 FFHQ-UV 데이터셋을 생성했으며, 이 데이터셋은 50,000개 이상의 균일한 조명의 얼굴 텍스처 UV 맵으로 구성되어 있습니다. 결과적으로, 이 데이터셋을 사용한 GAN 기반 텍스처 디코더는 기존 방식보다 우수한 3D 얼굴 재구성 성능을 보여줍니다.

[Dynamic Facial Asset and Rig Generation from a Single Scan](3/Dynamic%20Facial%20Asset%20and%20Rig%20Generation%20from%20a%20Sin%20f7af534f59b24752a9427cc3a2dd56f5)

단일 3D 스캔을 사용하여 개인화된 디지털 휴먼 얼굴 에셋과 리그를 생성하는 새로운 방법을 소개합니다. 이 방법은 영화, 게임 제작, 가상현실 등의 분야에 적용 가능합니다. 고도의 기술과 시간이 많이 소요되는 기존 방식과 달리, 이 접근법은 빠르고 자동화된 프로세스를 제공하며, '언캐니 밸리' 현상을 피하는 사실적인 얼굴을 생성할 수 있습니다. 연구진은 3D 스캔 데이터를 기반으로 개인화된 블렌드 셰이프와 물리 기반 텍스처 맵을 생성하며, 이를 통해 개인의 고유한 특성을 반영한 리깅된 모델을 만듭니다. 이 프레임워크는 고품질 얼굴 리그 에셋을 자동으로 생성하며, 전문적인 프로덕션 파이프라인에 직접 통합할 수 있습니다.

[SDM-UniPS: Scalable, Detailed, and Mask-Free Universal Photometric Stereo](3/SDM-UniPS%20Scalable,%20Detailed,%20and%20Mask-Free%20Univer%20673e9bc582564c61bbcbe3fa7630ded0)

SDM-UniPS, 즉 확장 가능하고 상세하며 마스크가 필요 없는 범용 측광 스테레오 네트워크를 소개합니다. 이 네트워크는 제어되지 않은 조명 조건 하에서 촬영된 이미지로부터 상세한 표면 맵을 생성합니다. 기존 UniPS 방법의 세 가지 주요 문제점을 해결하기 위해 개발된 이 네트워크는 다양한 입력 이미지 크기를 수용하면서 조명 특징을 효과적으로 추출하는 스케일 불변 공간 조명 특징 인코더, 픽셀 샘플링 트랜스포머를 사용하여 전역 정보를 고려하는 새로운 표면 법선 디코더, 그리고 다양한 물체, 텍스처, 조명 조건이 포함된 합성 훈련 데이터 세트를 특징으로 합니다. 이 방법은 기존 방식보다 시간을 절약할 수 있으며, '야생' 환경에서도 측광 스테레오를 수행할 수 있는 가능성을 제시합니다.

[Towards Ghost-free Shadow Removal via Dual Hierarchical Aggregation Network and Shadow Matting GAN](3/Towards%20Ghost-free%20Shadow%20Removal%20via%20Dual%20Hierarc%20060762920b504ff8ba4d65eccee67e00)

그림자 제거를 위한 새로운 접근 방식을 제시합니다. 이 방법은 특히 광원 반대편에 드리워진 그림자를 제거하는데 중점을 둡니다. 연구진은 그림자 제거 작업에 집중하여, 이미지에서 그림자를 효과적으로 제거하고 유령 현상을 최소화하는 새로운 네트워크 구조를 개발했습니다. 이 구조는 그림자가 있는 이미지와 그림자가 없는 이미지 사이에서 의미 정보를 공유한다는 관점에 기반하며, 확장 컨볼루션과 계층적 집계 주의 모델을 통합해 저수준의 유용한 특징을 보존합니다. 또한, 그림자 합성을 위해 생성적 적대 신경망(GAN)을 사용하여 자연 현상에서 영감을 얻은 그림자 이미지를 생성합니다. 실험 결과, 이 새로운 알고리즘은 시각적 품질 면에서 이전 방법들보다 우수한 성능을 보였습니다.

[Neural Haircut: Prior-Guided Strand-Based Hair Reconstruction](3/Neural%20Haircut%20Prior-Guided%20Strand-Based%20Hair%20Reco%20ddd7aee550e543c58f4f478642b525e0)

이미지나 비디오를 기반으로 사람 머리카락을 모델링하는 새로운 방법을 제시합니다. 이 방법은 머리카락의 복잡한 구조를 고려해 특수 효과, 텔레프레즌스, 게임 등 다양한 분야에 적용 가능합니다. 먼저 체적 표현을 사용해 대략적인 헤어 형태를 재구성한 후, 합동 최적화 과정을 통해 미세한 헤어 줄기를 맞춥니다. 이 과정은 렌더링 기반 손실과 합성 데이터에서 학습된 사전을 포함합니다. 저자들은 이 방법을 통해 3D 헤어 모델을 더 정확하게 재구성할 수 있음을 보여줍니다.