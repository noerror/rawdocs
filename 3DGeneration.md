# 3DGeneration

[Monster Mash: A Single-View Approach to Casual 3D Modeling and Animation](3/Monster%20Mash%20A%20Single-View%20Approach%20to%20Casual%203D%20M%20b5d895237e8b42ebab1da46e5d91d8c0)

사용자가 2D 스케치를 그리면 이를 실시간으로 3D 모델로 변환하는 새로운 툴을 소개합니다. 이 툴은 복잡한 리깅이나 모델 파트 병합 없이 애니메이션을 가능하게 합니다. 이 방식의 혁신은 단일 시점에서 모델링과 애니메이션을 수행하면서도 깊이 일관성을 유지하는 새로운 변형 모델인 ARAP-L에 있습니다. 이는 3D 애니메이션을 더욱 캐주얼하고 즉흥적인 창의성으로 접근할 수 있게 만듭니다.

[DreamAvatar: Text-and-Shape Guided 3D Human Avatar Generation via Diffusion Models](3/DreamAvatar%20Text-and-Shape%20Guided%203D%20Human%20Avatar%20%208def0affd7c8422db49e119c95afb092)

텍스트 프롬프트와 형태 가이드를 기반으로 3D 인간 아바타를 생성하는 새로운 방법인 DreamAvatar를 소개합니다. 이 시스템은 파라메트릭 SMPL 모델을 사용하여 형태를 가이드하고, '표준 공간'과 '관찰 공간'이라는 이중 공간 설계를 통해 아바타 생성을 효율적으로 관리합니다. 추가적으로, 정상 일관성 손실을 통해 아바타의 지오메트리와 텍스처의 일관성을 보장합니다. 실험적 검증을 통해 DreamAvatar는 다른 최신 방법들과 비교하여 우수한 결과를 보여주었습니다. 그러나 이 시스템은 3D 메시의 거친 질감과 얼굴 텍스처의 일관성 부족과 같은 한계를 지니고 있습니다.

[Text-To-4D Dynamic Scene Generation](3/Text-To-4D%20Dynamic%20Scene%20Generation%20e9a56fc92d9148a7832c7c0615a212a4)

MAV3D, 텍스트 기반의 동적 3D 장면을 생성하는 새로운 방법을 소개합니다. 이 방법은 확산 모델과 동적 신경 방사장(NeRF)을 결합하여 다양한 관점에서 텍스트 프롬프트에 따른 시간적 3D 장면을 생성합니다. MAV3D는 정적 3D 장면에 동적 요소를 점진적으로 도입하고, 시간 인식 초해상도 미세 조정을 통해 해상도를 높이는 다단계 최적화 체계를 사용합니다. 그러나 이 방법은 실시간 애플리케이션, 텍스처 품질 개선, 비디오 생성 제어 등에서 한계를 인정합니다.

[StyleAvatar3D: Leveraging Image-Text Diffusion Models for High-Fidelity 3D Avatar Generation](3/StyleAvatar3D%20Leveraging%20Image-Text%20Diffusion%20Mode%20c94b3777fbc44fa4ab10406068df3141)

이미지-텍스트 확산 모델과 생성적 적대 신경망(GAN)을 결합하여 스타일화된 3D 아바타를 고품질로 생성하는 새로운 방법을 제시합니다. 이 방법은 다양한 스타일과 관점에서 아바타 이미지를 생성하며, 사용자가 텍스트 프롬프트나 이미지를 사용하여 아바타의 스타일을 정의할 수 있습니다. 포즈와 이미지 간의 정렬 불일치 문제를 해결하기 위해, 특정 프롬프트와 거친 관점에서 세밀한 관점까지 작동하는 GAN 판별기가 개발되었습니다. 이 연구는 생성된 아바타의 시각적 품질과 다양성이 기존 방식보다 우수함을 보여줍니다.

[Do 2D GANs Know 3D Shape? Unsupervised 3D shape reconstruction from 2D Image GANs](3/Do%202D%20GANs%20Know%203D%20Shape%20Unsupervised%203D%20shape%20rec%207cc9eba8a2b244f88ba0c2d365af4c30)

2D 이미지 GANs를 사용하여 3D 형상을 비지도 학습 방식으로 재구성하는 새로운 접근 방식인 GAN2Shape에 대해 탐구합니다. 이 연구는 2D 생성적 적대 신경망(GAN), 특히 StyleGAN2의 기능을 활용하여 3D 물체 모양을 복구합니다. GAN2Shape는 기존 3D 모델이나 감독 없이도 2D 이미지에서 다양한 시점과 조명 조건을 생성하여 3D 구조를 추론합니다. 이 방법은 초기 타원체 모양에서 시작하여 다양한 시점과 조명에서 렌더링된 '의사 샘플'을 통해 점진적으로 3D 모양을 개선합니다. 실험 결과는 이 기술이 다양한 대상 카테고리에 대해 단일 이미지의 3D 모양을 효과적으로 재구성하며, 3D 형상 생성을 위한 새로운 관점을 제공한다는 것을 입증합니다.

[A Shading-Guided Generative Implicit Model for Shape-Accurate 3D-Aware Image Synthesis](3/A%20Shading-Guided%20Generative%20Implicit%20Model%20for%20Sha%207abaf5e407444234ae9fe3a7aef68abe)

2D 이미지에서 3D 형상을 생성하는 새로운 방법, ShadeGAN에 대해 설명합니다. 기존 방법이 모양과 색상의 모호함으로 정확한 3D 형상을 생성하는 데 어려움을 겪었던 반면, ShadeGAN은 조명을 명시적으로 모델링하여 다양한 조명 조건에서 3D 형상의 셰이딩을 적용함으로써 이 모호성을 해결합니다. 실험 결과, ShadeGAN은 기존 방법들보다 뛰어난 3D 형상 재구성과 이미지 재조명 능력을 보여주었습니다. 저자들은 학습 및 추론 시간을 각각 24%와 48%까지 줄이는 효율적인 볼륨 렌더링 전략도 개발했으며, 이 코드는 Github에 공개될 예정입니다.

[Shap·E: Generating Conditional 3D Implicit Functions](3/Shap%C2%B7E%20Generating%20Conditional%203D%20Implicit%20Function%2023c27f47c0fc4010bc21b0002dd9d44d)

새로운 3D 생성 모델인 Shap-E는 텍스처 메시와 뉴럴 래디언스 필드(NeRF)를 모두 렌더링할 수 있는 파라미터를 생성합니다. 이 모델은 두 단계의 훈련 과정을 거칩니다. 첫 번째 단계에서는 3D 에셋을 파라미터로 변환하는 인코더를 훈련하고, 두 번째 단계에서는 이 인코더 출력을 사용하여 조건부 확산 모델을 훈련합니다. 쌍을 이루는 3D 및 텍스트 데이터로 구성된 대규모 데이터 세트를 사용하면 Shap-E는 복잡하고 다양한 3D 에셋을 단 몇 초 만에 생성할 수 있으며, 이는 이전 모델인 Point-E보다 빠른 수렴과 높은 품질의 샘플 생성을 가능하게 합니다. Shap-E의 모델 가중치, 추론용 코드, 샘플 출력은 GitHub에서 확인할 수 있습니다.

[NeRF: Representing Scenes as Neural Radiance Fields for View Synthesis](3/NeRF%20Representing%20Scenes%20as%20Neural%20Radiance%20Fields%2069855d8d4bed40dbb3cbb159a683f4b2)

Neural Radiance Fields(NeRF)를 사용하여 복잡한 3D 장면을 연속적인 체적 함수로 표현하고 새로운 시점에서 장면을 합성하는 방법을 제시합니다. 입력으로 5D 좌표(공간 위치와 보는 방향)를 받아, 해당 지점에서의 밀도와 색상을 출력하는 심층 네트워크를 활용합니다. 새로운 뷰는 카메라 광선에 따라 좌표를 쿼리하고 볼륨 렌더링을 통해 색상과 밀도를 이미지에 투영하여 생성합니다. 이 방법은 뉴럴 렌더링과 뷰 합성에서 이전 작업보다 우수한 성능을 보여주며, 복잡한 장면의 사실적인 이미지를 만들 수 있음을 보여줍니다. 위치 인코딩과 계층적 샘플링 전략을 사용하여 저해상도 및 높은 스토리지 비용 문제를 극복합니다.

[Fantasia3D: Disentangling Geometry and Appearance for High-quality Text-to-3D Content Creation](3/Fantasia3D%20Disentangling%20Geometry%20and%20Appearance%20f%20e6b2bcaa1b2245cca27dfd1d7b4f14d8)

지오메트리와 외관을 분리하여 고품질의 텍스트 기반 3D 콘텐츠를 생성하는 새로운 방법을 제안합니다. 이 방법은 지오메트리 모델링을 위해 DMTET라는 하이브리드 표현을 사용하며, 사전 학습된 이미지 확산 모델의 입력으로 사용되는 표면 법선을 통해 모양 생성을 더욱 세밀하게 제어합니다. 외관 모델링의 경우, 공간적으로 변화하는 BRDF 모델링을 도입하여 사실적인 렌더링을 통해 고품질 3D 생성을 달성합니다. 이 방법은 사용자 정의가 가능하며, 생성된 3D 에셋의 재조명, 편집 및 물리적 시뮬레이션을 지원합니다.

[Magic3D: High-Resolution Text-to-3D Content Creation](3/Magic3D%20High-Resolution%20Text-to-3D%20Content%20Creatio%2046c57881c619451c9db13ceac152c561)

텍스트 프롬프트를 사용해 고품질 3D 메시 모델을 생성하는 2단계 최적화 프레임워크를 제시합니다. 이 기술은 저해상도 이미지 공간의 한계를 가진 기존 방법을 개선하여, 첫 단계에서 저해상도 디퓨전 프리와 스파스 3D 해시 그리드 구조를 사용해 기본 모델을 생성하고, 두 번째 단계에서는 고해상도 잠복 확산 모델과 효율적인 차별화 렌더러를 사용하여 텍스처가 있는 3D 메시 모델을 최적화합니다. Magic3D는 기존 방법보다 2배 빠른 시간에 고품질 모델을 생성하며, 사용자 연구에서 61.7%의 평가자가 이 방법을 선호함을 보여줍니다. Magic3D는 다양한 크리에이티브 애플리케이션에 적합한 새로운 3D 합성 제어 방법을 제공합니다.

[DREAMFUSION: TEXT-TO-3D USING 2D DIFFUSION](3/DREAMFUSION%20TEXT-TO-3D%20USING%202D%20DIFFUSION%20099e9203ced44fe3889ebbe9f03e7b73)

3D 데이터 없이도 사전 학습된 2D 텍스트-이미지 확산 모델을 이용해 텍스트로부터 3D 오브젝트와 장면을 생성하는 방법을 제시합니다. 이 방법은 신경 방사 필드(NeRF)와 같은 3D 모델을 다양한 각도에서 사실적인 2D 렌더링으로 최적화하는 확률 밀도 증류에 기반한 손실 함수(SDS)를 도입합니다. DreamFusion은 이 손실 함수를 이용해 NeRF를 미세 조정하고, 사용자의 텍스트 프롬프트에 따라 상세하고 일관된 3D 오브젝트와 장면을 생성합니다. 이 기술은 3D 훈련 데이터나 이미지 확산 모델에 대한 수정 없이도 3D 합성을 위한 사전 작업으로 확산 모델의 효과를 입증합니다.

[Zero-1-to-3: Zero-shot One Image to 3D Object](3/Zero-1-to-3%20Zero-shot%20One%20Image%20to%203D%20Object%20667018cd738f4c0b8c26c5f12c2cdb9f)

단 하나의 RGB 이미지를 사용하여 물체의 카메라 시점을 변경할 수 있는 새로운 방법을 소개합니다. 이 기술은 대규모 확산 모델을 활용하여 2D 이미지에서 학습한 기하학적 지식을 바탕으로 물체의 새로운 시점의 이미지를 생성합니다. 주요 성과로는 단일 RGB 이미지에서 3D 물체를 제로샷으로 재구성하고, 새로운 시각 합성을 위한 최첨단 결과를 달성한 것입니다. 이 방법은 인터넷 규모의 사전 학습을 활용하여 기존의 단일 뷰 3D 재구성 및 새로운 뷰 합성 모델보다 우수한 성능을 보여주었습니다.

[Instruct-NeRF2NeRF: Editing 3D Scenes with Instructions](3/Instruct-NeRF2NeRF%20Editing%203D%20Scenes%20with%20Instruct%20f18337f49f0e42c892f9259c88312c5a)

텍스트 지침을 사용하여 3D NeRF 장면을 편집하는 새로운 방법, Instruct-NeRF2NeRF를 소개합니다. 이 방법은 2D 확산 모델인 InstructPix2Pix를 활용하여 NeRF 장면의 이미지를 업데이트하고, 반복적 데이터 세트 업데이트를 통해 3D 표현을 개선합니다. 이 접근법은 다양한 NeRF 장면에서 평가되었으며, 사람, 사물 및 대규모 장면에서 광범위한 편집을 가능하게 합니다. 이 연구는 일반 사용자가 복잡한 도구나 전문 지식 없이도 텍스트 지침을 통해 직관적으로 3D 장면을 편집할 수 있도록 하는 중요한 발전을 나타냅니다.

[Tex2Shape: Detailed Full Human Body Geometry From a Single Image](3/Tex2Shape%20Detailed%20Full%20Human%20Body%20Geometry%20From%20a%201765df0b146b45ec9ab751cdb83f4370)

단일 이미지에서 완전한 인체 형태의 상세한 3D 지오메트리를 추론하는 새로운 방법을 소개합니다. 이 방법은 사진 속 얼굴, 머리카락, 옷, 주름 등을 포함한 상세한 인체 형태를 생성하며, 가려진 부분까지도 복원할 수 있습니다. 이 모델은 정렬된 이미지 간 변환 문제로 모양 회귀를 변환하는 기술을 사용합니다. 입력 이미지의 부분 텍스처 맵으로부터 상세한 노멀 및 벡터 변위 맵을 추정하며, 저해상도 스무스 바디 모델에 이를 적용해 디테일과 의상을 추가합니다. 이 방법은 합성 데이터로만 학습되었음에도 불구하고 실제 사진에 잘 일반화됩니다.

[Text2Shape: Generating Shapes from Natural Language by Learning Joint Embeddings (hold)](3/Text2Shape%20Generating%20Shapes%20from%20Natural%20Language%20598aaa08305b458b953f384fbc2366bc)

자연어 설명에서 3D 도형을 생성하는 새로운 방법, Text2Shape를 소개합니다. 이 기술은 복셀화된 3D 모양 데이터와 텍스트 설명의 공동 임베딩을 통해 학습됩니다. 이 모델은 언어와 3D 도형의 물리적 속성 간의 관계를 포착하여 자연어에서 컬러 3D 모양을 생성합니다. 연구진은 ShapeNet 데이터 세트를 사용하여 이 접근법을 평가하고, 텍스트-도형 검색과 생성에서 기존 방법보다 우수한 결과를 보여줍니다. 이 방법은 3D 디자인, 교육, 증강 현실 등 다양한 분야에서 응용 가능성을 제시합니다.

[PIFuHD: Multi-Level Pixel-Aligned Implicit Function for
High-Resolution 3D Human Digitization](3/PIFuHD%20Multi-Level%20Pixel-Aligned%20Implicit%20Function%202234fce8d48e409586c99d1194043df3)

단일 이미지에서 고해상도의 3D 인체 재구성을 위한 새로운 접근법을 제안합니다. 이 연구는 기존의 저해상도 이미지로 인한 3D 추정치의 정확도 문제를 해결하기 위해 다단계 엔드투엔드 아키텍처를 사용합니다. 거친 레벨에서는 저해상도 이미지로 전체적인 형상을 추론하고, 미세 레벨에서는 고해상도 이미지를 활용하여 세부적인 지오메트리에 집중합니다. 이 방법은 1k 해상도의 입력 이미지를 사용하여 단일 이미지 인체 형상 재구성에서 뛰어난 성능을 보여줍니다.

[HumanRF: High-Fidelity Neural Radiance Fields for Humans in Motion](3/HumanRF%20High-Fidelity%20Neural%20Radiance%20Fields%20for%20H%20ff1832104e554644a09ed10decbe0cb3)

움직이는 사람의 사실적인 이미지와 동영상을 생성하는 과제를 다룬 논문입니다. 이 연구는 신경 방사 필드(NeRF)를 활용하여 높은 해상도의 멀티뷰 비디오 데이터 세트인 ActorsHQ를 기반으로 인간의 동작을 캡처합니다. 저자들은 복잡한 인간의 움직임을 처리하고 긴 시퀀스를 관리 가능한 세그먼트로 분할하여 효율적으로 훈련하는 새로운 방법을 제안합니다. 이 연구의 핵심 목표는 컴퓨터로 생성된 사람의 이미지와 비디오의 사실감과 디테일을 개선하는 것입니다.

[HairNet: Single-View Hair Reconstruction using Convolutional Neural Networks](3/HairNet%20Single-View%20Hair%20Reconstruction%20using%20Conv%20c2a15432fdec4bc4af106a68a211b7da)

단일 이미지에서 3D 헤어 지오메트리를 생성하는 새로운 딥러닝 방법을 소개합니다. 이 방법은 컨볼루션 신경망(CNN)을 사용해 모발 이미지의 2D 방향 필드를 처리하고, 이를 통해 매개변수화된 2D 두피에 고르게 분포된 스트랜드 특징을 생성합니다. 연구진은 보다 사실적인 헤어스타일 생성을 위해 충돌 손실 기능을 도입하고, 각 가닥의 가시성을 재구성의 정확도를 높이기 위한 가중치로 사용했습니다. 네트워크의 인코더-디코더 구조는 다양한 헤어스타일 간 자연스러운 보간을 가능하게 하며, 렌더링된 합성 헤어 모델의 대규모 세트로 학습됩니다. 이 방법은 효율성과 속도 면에서 기존 접근법을 크게 개선하며, 실제 이미지와 동영상에 대한 효과적인 적용을 보여줍니다.

[RODIN: A Generative Model for Sculpting 3D Digital Avatars Using Diffusion](3/RODIN%20A%20Generative%20Model%20for%20Sculpting%203D%20Digital%20%20ddb428a5580b440d9051803dff36bd27)

확산 모델을 사용하여 3D 디지털 아바타를 생성하는 새로운 기술, RODIN을 소개합니다. 이 기술은 전통적으로 3D 아티스트에 의해 수행되던 복잡한 작업, 특히 머리카락과 같은 세부 사항을 포함하는 아바타 생성을 자동화합니다. RODIN은 아바타를 나타내기 위해 3D 공간의 색상과 밀도를 설명하는 NeRF(신경 방사 필드) 형식을 사용하며, 3D 인식 컨볼루션, 잠재적 컨디셔닝, 계층적 합성 등의 프로세스를 통해 상세한 아바타를 생성합니다. 이 모델은 고해상도의 결과물을 생성하며, 다양한 아바타와 세밀한 헤어스타일, 얼굴 털 생성에 특히 능숙합니다. 연구는 이 기술이 3D 디지털 아바타 생성에 큰 잠재력을 가지고 있음을 보여줍니다.

[PanoHead: Geometry-Aware 3D Full-Head Synthesis in 360∘](3/PanoHead%20Geometry-Aware%203D%20Full-Head%20Synthesis%20in%20%20fbc90bd931a44bc6ae95649102a94535)

PanoHead라는 새로운 3D 생성적 적대 신경망(GAN) 모델을 소개합니다. PanoHead는 단일 뷰 이미지에서 전체 360° 머리 모델을 생성할 수 있으며, 이는 기존의 3D GAN 모델이 달성하지 못한 일입니다. 이 모델은 전경 인식 생성기와 트라이 디스커네이터를 사용하여 3D 풀 헤드 지오메트리를 합성하고, 새로운 3D 트라이그리드 볼륨 표현으로 머리의 앞면과 뒷면을 분리합니다. 또한, 2단계 이미지 정렬 프로세스를 통해 머리 뒤쪽의 카메라 뷰를 정확하게 추정하고 정렬 문제를 처리합니다. PanoHead는 다양한 각도에서 고품질의 일관된 3D 헤드 이미지를 생성하는 데 중요한 발전을 보여줍니다

[SSIF: Single-shot Implicit Morphable Faces with Consistent Texture Parameterization](3/SSIF%20Single-shot%20Implicit%20Morphable%20Faces%20with%20Con%208bde70b3f6a346f880bd9ba90b9c711b)

단일 RGB 이미지로부터 고품질의 편집 가능한 3D 디지털 아바타를 재구성하는 새로운 방법을 제시합니다. 이 방법은 암시적 기하학적 표현과 명시적인 텍스처 맵을 통합하여, 큰 자세 변화에서 새로운 뷰의 합성과 표현력 있는 비선형 얼굴 애니메이션 공간을 자연스럽게 지원합니다. 또한 사용자가 텍스처 맵을 직접 편집할 수 있으며, 재조명과 같은 추가 하류 애플리케이션을 위한 3D 자산 추출도 가능합니다. 이 연구는 영화, 가상 현실, 텔레프레즌스와 같은 분야에 적용될 수 있는 잠재력을 가지고 있으며, 매우 사실적인 '디지털 트윈'으로 이어질 수 있습니다.

[FitMe: Deep Photorealistic 3D Morphable Model Avatars](3/FitMe%20Deep%20Photorealistic%203D%20Morphable%20Model%20Avata%209811165e20304a6ebd331af81d04d895)

단일 이미지에서 상세한 3D 얼굴 특징을 재구성하기 위한 방법을 제시합니다. 이는 기존 3D 모퍼블 모델의 한계를 극복하고, 실제 같은 렌더링을 가능하게 하는 중요한 진전을 나타냅니다. FitMe는 고해상도 얼굴 반사 텍스처 맵을 생성하여, 정확한 미분 가능한 렌더링을 통해 표준 렌더링 애플리케이션에서 직접 사용할 수 있는 상세하고 완전한 재구성을 만들어냅니다. 이 방법은 또한 피부톤 증강과 차별화된 렌더링 절차를 포함하여, 높은 신원 유사성과 충실도 높은 얼굴 재구성을 달성합니다.

[A Hierarchical Representation Network for Accurate and Detailed Face Reconstruction from In-The-Wild Images](3/A%20Hierarchical%20Representation%20Network%20for%20Accurate%20fb80d7e2fd6d4978aa1e7d19698c7c3d)

자연 이미지로부터 3D 얼굴을 정밀하게 재구성하는 계층적 표현 네트워크(HRN)에 대해 다룹니다. HRN은 얼굴 형상을 저주파, 중주파, 고주파 디테일로 나누어 모델링하고, 디리터칭 모듈을 사용하여 베이스 텍스처를 정제합니다. 이 방법은 기존 3DMM 방식에 비해 중간 주파수 디테일을 더 잘 재현하며, 두 대규모 벤치마크에서 우수한 성능을 보였습니다. 이 논문은 고충실도 3D 얼굴 재구성을 위한 새로운 접근법과, 이를 위한 고품질 3D 얼굴 데이터세트인 FaceHD-100을 소개합니다.

[Sketch-A-Shape: Zero-Shot Sketch-to-3D Shape Generation](3/Sketch-A-Shape%20Zero-Shot%20Sketch-to-3D%20Shape%20Genera%2099388a4fd307426fad01d84c3e774c09)

제로샷 스케치-3D 생성 모델을 소개하는 논문입니다. 이 모델은 사전 학습된 비전 모델인 CLIP 및 DINOv2를 활용하여, 대충 그린 스케치에서부터 전문적인 스케치까지 다양한 스케치의 로컬 시맨틱 신호를 식별하고 이를 기반으로 일관된 3D 형상을 합성합니다. 이 논문의 핵심은 쌍을 이루는 스케치-3D 데이터 세트가 필요 없는 최초의 제로샷 방법인 'Sketch-A-Shape'의 개발과, 이 모델이 다양한 스케치의 복잡성을 일반화할 수 있는 능력을 입증한 것입니다. 실험 결과, 이 모델은 다양한 유형의 스케치에서 우수한 성능을 보였으며, 인간 지각 평가에서도 높은 인식률을 보였습니다.

[Efficient Geometry-aware 3D Generative Adversarial Networks](3/Efficient%20Geometry-aware%203D%20Generative%20Adversarial%201847091bbd954a4a9869c5d7b4014bf1)

D 생성적 적대 신경망(GAN)의 새로운 접근 방식을 제시합니다. 이 연구에서는 기존의 3D GAN 모델이 직면한 문제, 즉 계산 비효율성과 낮은 이미지 품질을 해결하기 위해 명시적-암시적 하이브리드 3D 표현 방식인 트라이플레인을 도입합니다. 이 방법은 2D CNN 기반 특징 생성기를 사용하면서도 고품질의 3D 뉴럴 볼륨 렌더링을 가능하게 합니다. 실험 결과, 이 방법은 기존의 3D 인식 GAN에 비해 더 높은 품질의 이미지와 뷰 일관성을 제공하는 것으로 나타났습니다. 또한, 이 연구는 3D GAN의 새로운 응용 가능성을 열어줄 수 있음을 보여주며, 향후 더 많은 연구와 개선이 기대됩니다.

[Neural Volumes: Learning Dynamic Renderable Volumes from Images](3/Neural%20Volumes%20Learning%20Dynamic%20Renderable%20Volumes%2003b559cf1fdd4bf98f417db7d6851dcb)

3D 볼륨 재구성의 새로운 접근 방식을 제시하는 논문입니다. 이 연구에서는 다중 카메라 이미지를 사용하여 동적, 반투명 3D 모델을 학습하고 렌더링하는 방법을 개발했습니다. 복잡한 현실 현상을 정확하게 캡처하기 위해 머신러닝 기반의 체적 표현과 레이 마칭 알고리즘을 활용합니다. 이 접근법은 효과적인 해상도 향상을 위한 워핑 기법과 뷰 컨디셔닝을 포함하여, 다양한 시점에서 일반화되고 동적인 장면을 재구성하며, 엔드투엔드 학습을 가능하게 합니다. 이 방식은 복잡한 표면과 물체, 예를 들어 머리카락이나 연기 등을 정확하게 재구성하는 능력을 보여주며, VR과 같은 인터랙티브 애플리케이션에 적합합니다.

[AvatarVerse: High-quality & Stable 3D Avatar Creation from Text and Pose](3/AvatarVerse%20High-quality%20&%20Stable%203D%20Avatar%20Creati%20e870eb72de3743fc99e2cbff524f6588)

텍스트 설명과 포즈를 사용하여 고품질의 3D 아바타를 생성하는 새로운 기술을 소개합니다. 이 기술은 2D 확산 모델과 달리, 다양성이 제한된 3D 모델을 넘어서며, 고충실도 3D 모델 생성의 일반적인 문제들을 해결합니다. AvatarVerse는 텍스트와 포즈를 사용해 정확하고, 세밀하며, 일관된 아바타를 생성할 수 있는 방법을 제공합니다. 이를 위해 컨트롤넷과 덴스포즈, SDS 손실, 고해상도 전략 및 부드러움 손실을 포함한 다양한 기술을 사용합니다. 이 기술은 텍스트와 포즈만으로 아바타를 생성하는 새로운 방법을 제시하며, 세부적인 정제 과정을 통해 손과 액세서리를 포함한 더 나은 품질의 아바타를 제작합니다.

[TeCH: Text-guided Reconstruction of Lifelike Clothed Humans](3/TeCH%20Text-guided%20Reconstruction%20of%20Lifelike%20Clothe%20666e0b9bc0ee472e9a55acdba671bc15)

단일 이미지와 텍스트 정보를 결합하여 사실적인 3D 옷 입은 인간의 모델을 재구성하는 새로운 기술을 소개합니다. 이 방법은 TeCH라고 명명되었으며, 이미지의 시각 정보와 세부적인 텍스트 설명을 활용하여 풍부한 디테일과 정확한 텍스처를 포함하는 3D 모델을 생성합니다. 복잡한 옷차림과 얼굴 특징을 포함한 전체 몸 구조를 정밀하게 복원할 수 있으며, 이는 게임, 가상 현실 등 다양한 분야에서 활용될 수 있습니다. 이 기술은 다양한 데이터 세트에 대해 테스트되었으며, TeCH는 다른 상위 방법들에 비해 더 우수한 디테일과 렌더링 품질을 보여주었습니다.

[MVDream: Multi-view Diffusion for 3D Generation](3/MVDream%20Multi-view%20Diffusion%20for%203D%20Generation%20332f41c2a6ea43108193758c2324a1e2)

3D 컨텐츠 제작의 복잡성에 대응하는 새로운 멀티뷰 확산 모델을 소개합니다. 이 기술은 다양한 시점에서 일관성 있는 이미지를 생성하며, 2D 리프팅 방법의 한계를 극복합니다. MVDream 모델은 멀티뷰 이미지와 실제 이미지를 학습하여, 기존 방식에 비해 우수한 일관성과 적응성을 보여줍니다. 이 모델은 3D 데이터 세트를 사용해 일관된 멀티뷰 이미지를 생성하며, 3D 생성 작업에 필수적인 멀티뷰 확산을 직접 훈련합니다. 이 연구는 3D 생성을 위한 2D 확산 모델의 적용과 그 한계, 멀티뷰 드림부스의 구현 등을 포함합니다. 결과적으로, MVDream은 3D 모델 생성에서 고도의 견고성과 품질을 달성하는 것으로 나타났습니다.

[CityDreamer: Compositional Generative Model of Unbounded 3D Cities](3/CityDreamer%20Compositional%20Generative%20Model%20of%20Unbo%20c844086f52564743ab770e7ad8f4b8f6)

메타버스와 3D 도시 제작에 대한 관심이 높아짐에 따라 개발된 새로운 3D 도시 생성 모델, CityDreamer를 소개합니다. 이 모델은 건물, 도로, 공원 등의 도시 요소를 분리하여 생성하고, 다양한 스타일과 일관된 시점을 가진 무한한 도시 레이아웃을 생성합니다. CityDreamer는 OSM과 GoogleEarth라는 두 개의 데이터 세트를 활용하여 사실적인 3D 도시를 제작합니다. OSM은 80개 도시의 지도와 건물 높이 데이터를, GoogleEarth는 뉴욕시의 24,000장 이미지를 제공합니다. 실험 결과, CityDreamer는 기존 3D 모델들과 비교하여 우수한 성능을 보여주었습니다.

[NeROIC: Neural Rendering of Objects from Online Image Collections](3/NeROIC%20Neural%20Rendering%20of%20Objects%20from%20Online%20Ima%20da63874a85ac4b8a8b2345c3f428e618)

온라인 이미지 컬렉션으로부터 객체를 신경망 렌더링하는 새로운 방법론을 제시합니다. 이 접근법은 임의의 온라인 이미지에서 객체의 형태와 표면 특성을 파악하여, 새로운 환경 및 조명 조건에서 재조명 및 합성이 가능하게 합니다. 기존의 신경 방사 필드(NeRF) 모델을 개선하여, 다양한 조명 및 카메라 설정에서 촬영된 이미지들을 활용할 수 있게 만들었습니다. 이 방법은 객체의 기하학적 및 광학적 특성을 순차적으로 분석하고, 다양한 실제 데이터 세트에서의 테스트를 통해 기존 기법보다 우수한 성능과 효율성을 입증했습니다. 이 연구는 온라인 이미지 컬렉션에서 사용할 수 있는 희귀 또는 멸종 위기에 처한 객체를 재렌더링하는 새로운 가능성을 열어줍니다.

[3D Gaussian Splattingfor Real-Time Radiance Field Rendering](3/3D%20Gaussian%20Splattingfor%20Real-Time%20Radiance%20Field%20%20132fec7ea090417aafd41282aa3b426b)

실시간 방사형 필드 렌더링을 위한 새로운 3D 가우시안 스플래팅 기술을 소개합니다. 이 연구는 최고 품질의 기존 방법들과 동등한 품질을 유지하면서도, 가장 빠른 기존 방법들과 경쟁할 수 있는 최적화 시간을 필요로 합니다. 핵심은 실시간으로 차별화 가능한 렌더러와 결합된 새로운 3D 가우시안 장면 표현입니다. 이는 장면 최적화와 새로운 뷰 합성에 중요한 속도 향상을 제공합니다. InstantNGP와 비교하여 비슷한 훈련 시간 동안 유사한 품질을 달성하며, 51분 동안 훈련함으로써 Mip-NeRF360보다 약간 더 나은 최고 수준의 품질을 달성합니다.

[360∘ Reconstruction From a Single Image Using Space Carved Outpainting](3/360%E2%88%98%20Reconstruction%20From%20a%20Single%20Image%20Using%20Spac%20f605a112375048d7a456d3e4cbc83c3c)

POP3D라는 새로운 기술을 소개합니다. 이 기술은 단일 RGB 이미지를 사용하여 전체 360° 뷰의 3D 재구성을 가능하게 합니다. POP3D는 단 하나의 이미지에서 시작하여, 기하학적 특징을 추론하고, 이를 바탕으로 물체의 추가 뷰를 '페인팅'하여 점진적으로 3D 모델을 개선합니다. 이 접근 방식은 많은 이미지나 복잡한 데이터가 필요 없이, 높은 충실도의 재구성을 제공합니다. 본 연구는 현재의 3D 모델링 기술에서 일반화와 충실도 문제를 해결하는 새로운 방법을 제시하며, 이 분야에서 중요한 발전을 나타냅니다.

[Text-to-3D using Gaussian Splatting](3/Text-to-3D%20using%20Gaussian%20Splatting%20822e21a790f649259db5229e7b94b521)

GSGEN, 즉 3D 가우시안 스플래팅 기반의 새로운 텍스트-3D 생성 모델을 소개합니다. 이 모델은 텍스트 설명에서 고도의 세부 사항을 가진 3D 자산을 생성하는 데 초점을 맞춥니다. 이 방법은 기하학적 일관성을 개선하기 위해 가우시안 그룹으로 3D 콘텐츠를 표현하며, 지오메트리 최적화와 외형 개선의 두 단계를 통해 최적화됩니다. GSGEN은 가우시안 그룹을 3D 포인트 클라우드 확산 선행과 표준 2D 이미지 선행의 안내로 미세 조정하여 보다 일관된 3D 지오메트리를 얻습니다. 이 연구는 텍스트에서 세밀하고 일관된 3D 에셋을 생성하는 새로운 방법을 제시함으로써 3D 콘텐츠 생성 분야에 기여합니다.

[GaussianDreamer: Fast Generation from Text to 3D Gaussian Splatting with Point Cloud Priors](3/GaussianDreamer%20Fast%20Generation%20from%20Text%20to%203D%20Ga%20620096ed7d5a4736846abdf35f8e0d67)

텍스트를 사용하여 3D 시각적 모델을 생성하는 새로운 방법, 가우시안드리머를 소개합니다. 이 방법은 3D와 2D 확산 모델을 연결하고, 3D 가우시안 스플래팅을 사용하여 3D 일관성과 풍부한 생성 디테일을 모두 달성합니다. 이 프로세스는 3D 디퓨전 모델로부터 생성된 기본 3D 모델로 시작하며, 이 모델은 3D 가우시안 세트의 기초로 활용됩니다. 또한, 노이즈 포인트 증가 및 색상 섭동 기법을 도입하여 초기화된 3D 가우시안 효과를 향상시킵니다. 이 방법은 간단하면서도 효과적이며, 단일 GPU에서 단 25분 만에 디테일한 3D 모델을 제작할 수 있습니다. 이 방법으로 생성된 에셋은 경량이며 실시간으로 렌더링할 수 있습니다.

[4K4D: Real-Time 4D View Synthesis at 4K Resolution](3/4K4D%20Real-Time%204D%20View%20Synthesis%20at%204K%20Resolution%208e942a00010e411da6a6906fe1418b77)

4K 해상도에서 4차원(3D + 시간) 실시간 뷰 합성을 달성하는 새로운 방법, 4K4D를 소개합니다. 이 기술은 멀티뷰 비디오로부터 4D 신경 표현을 재구성하여 고해상도 이미지를 빠르게 렌더링합니다. 4K4D는 하이브리드 외형 모델과 결합된 4D 포인트 클라우드 표현을 활용, 각 점을 학습 가능한 벡터로 처리하고, 신경망을 통과시켜 다양한 속성을 결정합니다. 이 모델은 고유하게 최적화되어 안정성과 견고성을 보장하며, 높은 렌더링 속도를 제공합니다. 복잡한 장면에서도 높은 품질과 속도를 달성함으로써, 가상 현실, 증강 현실, 스포츠 중계 등의 분야에서 활용될 잠재력을 가집니다.

[DreamGaussian: Generative Gaussian Splatting for Efficient 3D Content Creation](3/DreamGaussian%20Generative%20Gaussian%20Splatting%20for%20Ef%20d37224c20d5e4954ba22885b1d41e897)

이미지 및 텍스트에서 3D 콘텐츠를 효율적으로 생성하는 새로운 방법을 제시합니다. 이 방법은 3D 가우시안 스플래팅 기술을 활용하여 단 몇 분 안에 고품질의 텍스처 메시를 생성합니다. 이 프로세스는 텍스처 메시 추출과 UV 공간에서의 텍스처 품질 개선을 포함하여 빠르고 효율적인 3D 콘텐츠 제작을 가능하게 합니다. 연구는 이러한 접근 방식이 3D 콘텐츠 제작의 속도와 품질을 모두 개선한다고 주장합니다.

[ZeroNVS: Zero-Shot 360-Degree View Synthesis from a Single Real Image](3/ZeroNVS%20Zero-Shot%20360-Degree%20View%20Synthesis%20from%20a%200400880e607240e0bc2f1e0bc3a2396f)

단일 실제 이미지로부터 360도 전경 합성을 가능하게 하는 새로운 기술, 제로NVS를 소개합니다. 이 기술은 복잡한 실제 장면에 대해 360도 뷰를 생성하며, 이를 위해 새로운 카메라 매개변수화 및 정규화 방법과 'SDS 앵커링' 메커니즘을 도입합니다. 제로NVS는 다양한 소스의 혼합 데이터 세트를 사용하여 훈련되며, 이를 통해 다양한 카메라 설정과 3D 지상 실측 데이터 유형을 포함시킵니다. 실험 결과, 제로NVS는 이미지 합성 리얼리즘에서 새로운 기준을 세웠으며, Mip-NeRF 360 데이터세트를 새로운 평가 표준으로 제안합니다. 이 백서는 전체 실제 장면에 대한 360도 뷰 합성을 위한 새로운 기술의 공헌을 강조합니다.

[DreamCraft3D: Hierarchical 3D Generation with Bootstrapped Diffusion Prior](3/DreamCraft3D%20Hierarchical%203D%20Generation%20with%20Boots%200e279f21e89f4a268c56f03e4387c724)

2D 이미지에서 시작하여 복잡한 3D 에셋을 전체적으로 일관된 품질로 제작하는 새로운 방법, DreamCraft3D를 소개합니다. 이 프로세스는 2D 초안에서부터 시작하여 조각과 텍스처링 단계를 거쳐 3D 오브젝트를 만듭니다. 여기에는 다양한 3D 데이터로 학습된 시점 조건부 이미지 변환 모델을 사용하여 풍부한 3D 프리뷰를 제공합니다. 또한, 부트스트랩 스코어 증류라는 기법을 통해 3D 모델의 텍스처를 향상시키며, 이 방법은 최적화 과정 중에 진화하는 분포를 학습하여 점차 더 세밀한 텍스처로 이어집니다. 드림크래프트3D는 복잡한 지오메트리와 모든 각도에서 일관된 사실적인 텍스처로 창의적인 3D 에셋을 제작할 수 있음을 보여줍니다.

[Dreaming Your Room Space with Text-DrivenPanoramic Texture Propagation](3/Dreaming%20Your%20Room%20Space%20with%20Text-DrivenPanoramic%20ad759c6d99ee408eb52c1c1f5a59cfc9)

텍스트 프롬프트를 사용하여 실제 실내 장면을 몰입감 있는 판타지 스타일의 환경으로 변환하는 새로운 프레임워크를 소개합니다. 이 기술은 HMD(헤드 마운트 디스플레이)와 6-DoF(6자유도) 렌더링을 활용하여 사용자가 스타일라이즈된 가상 장면을 경험할 수 있도록 합니다. 드림스페이스는 텍스트 프롬프트에서 의미적 일관성과 공간적 일관성을 가진 씬 메시의 텍스처를 생성합니다. 이 프로세스에는 초기 파노라마 텍스처 생성, 전체 씬으로의 텍스처 전파 및 VR 애플리케이션용 메시로의 베이킹이 포함됩니다. 이 프레임워크는 텍스처와 씬 지오메트리의 정렬, 복잡한 씬의 오클루전 처리 등을 위한 하향식 접근 방식을 사용합니다.

[Instant3D: Fast Text-to-3D with Sparse-View Generation and Large Reconstruction Model](3/Instant3D%20Fast%20Text-to-3D%20with%20Sparse-View%20Generat%20a5461f4df9ab45ccb47fbbc342a342e6)

텍스트 프롬프트로부터 단 20초 내에 고품질의 3D NeRF 자산을 생성하는 새로운 방법, Instant3D를 소개합니다. 이 방법은 사전 학습된 2D 확산 모델을 활용하여 먼저 4개의 다양한 시점 이미지를 생성한 후, 이 이미지들로부터 3D 모델을 재구성하는 과정을 거칩니다. 이 과정은 기존의 3D 이미지 생성 방법에 비해 훨씬 빠르고, 다양한 텍스트 프롬프트에 대해 고품질의 3D 에셋을 생성할 수 있는 능력을 갖추고 있습니다. Instant3D는 특히 텍스트-이미지 확산 모델의 성능을 활용하여, 복잡한 프롬프트에도 불구하고 다양하고 고품질의 3D 자산을 생성할 수 있음을 보여줍니다.

[LRM: LARGE RECONSTRUCTION MODEL FOR SINGLE IMAGE TO 3D](3/LRM%20LARGE%20RECONSTRUCTION%20MODEL%20FOR%20SINGLE%20IMAGE%20TO%204c77297e352949628b9053be767510e9)

단일 이미지에서 3D 모델을 생성하는 새로운 접근법인 대규모 재구성 모델(LRM)을 소개합니다. 이 모델은 사전 학습된 시각 트랜스포머를 이미지 인코더로 사용하고, 이미지 특징을 3D 삼면 형식으로 투영하는 고급 인코더-디코더 구조를 활용합니다. LRM은 5억 개 이상의 파라미터를 포함하는 대규모 모델로, 100만 개 이상의 3D 모양과 비디오 데이터에 대해 학습되었습니다. 이 모델은 다양한 멀티뷰 이미지 데이터 세트에 적용 가능하며, 과도한 3D 인식 정규화나 복잡한 하이퍼 파라미터 튜닝 없이도 효율적으로 훈련할 수 있습니다. LRM은 사후 최적화 없이도 단 5초 만에 3D 형상을 생성할 수 있으며, 실용적인 다운스트림 애플리케이션에 사용될 수 있습니다.

[One-2-3-45++: Fast Single Image to 3D Objects with Consistent Multi-View Generation and 3D Diffusion](3/One-2-3-45++%20Fast%20Single%20Image%20to%203D%20Objects%20with%20%2030a9de8ebf5e4dbebb7a0c59e1cb77c4)

단일 이미지 또는 텍스트 프롬프트에서 3D 모양을 빠르고 일관되게 생성하는 새로운 방법, One-2-3-45++를 소개합니다. 이 방법은 멀티뷰 2D 생성과 3D 재구성의 두 단계로 구성되어 있으며, 2D 확산 모델을 미세 조정하여 일관된 멀티뷰 이미지를 예측한 다음, 멀티뷰 조건부 3D 디퓨전 기반 모듈을 사용해 텍스처가 있는 메시로 재구성합니다. 실험 결과, One-2-3-45++는 기존 방법보다 더 빠르고 정확하며, 1분 이내에 디테일한 텍스처의 사실적인 3D 메시를 생성하는 능력이 입증되었습니다.

[Deep Marching Tetrahedra: a Hybrid Representation for High-Resolution 3D Shape Synthesis](3/Deep%20Marching%20Tetrahedra%20a%20Hybrid%20Representation%20f%20fab3befcac1d4cf2a193e5770284f844)

고해상도 3D 형상 생성을 위한 새로운 접근 방식, DMTET를 소개합니다. 이 기술은 간단한 복셀과 같은 사용자 입력에서 고해상도 3D 모델로 변환하는 데 초점을 맞추며, 암시적 및 명시적 표현의 장점을 결합합니다. DMTET는 효율적이면서 고품질의 형상을 생성하는 것으로 실험적으로 입증되었습니다. 이 기술은 AR/VR, 로봇공학, 건축, 게임 및 영화 등 다양한 분야에서 3D 콘텐츠의 필요성을 충족시킬 수 있는 잠재력을 가지고 있습니다.

[LucidDreamer: Towards High-Fidelity Text-to-3D Generation via Interval Score Matching](3/LucidDreamer%20Towards%20High-Fidelity%20Text-to-3D%20Gene%206f4b3ea18bf0478fa44b95d3550a8462)

텍스트에서 고품질 3D 콘텐츠를 생성하는 새로운 프레임워크인 LucidDreamer를 소개합니다. 이 프레임워크는 사전 훈련된 2D 확산 모델을 사용하여 고해상도 텍스처와 형상을 추출하고, 새로운 Interval Score Matching 기법과 고급 3D 증류 파이프라인을 통해 짧은 훈련 시간에도 우수한 품질의 3D 생성 결과를 달성합니다. 이 논문은 점수 증류 샘플링(SDS)의 과도한 스무딩 문제를 해결하기 위해 ISM을 도입하며, 이를 통해 더 현실적이고 상세한 결과를 산출합니다.

[Single-Image 3D Human Digitization with Shape-Guided Diffusion](3/Single-Image%203D%20Human%20Digitization%20with%20Shape-Guid%208aaffc0ad74e48e0a608c2f2b1f9b45d)

단일 이미지를 사용하여 사람의 3D 텍스처를 합성하는 새로운 기술을 제시합니다. 이 방법은 고용량 2D 확산 모델을 사용하여, 지도 학습이나 3D 스캔 데이터에 의존하지 않고 사람 모양을 미리 학습시켜 단일 이미지에서 3D 일관된 텍스처를 생성합니다. 핵심 아이디어는 기존의 단일 이미지 3D 재구성 방식의 한계를 극복하는 것으로, 특히 가려진 영역이나 뒷모습의 사실적 재현에 중점을 두고 있습니다. 이 연구는 3D 휴먼 메시 생성을 위한 새로운 접근 방식을 제안하며, 특히 데이터 수집 노력에 대한 통합적 접근 방식을 통해 3D 및 일반 2D/3D 합성 방법에 잠재적으로 도움이 될 수 있는 가능성을 보여줍니다.

[MeshGPT: Generating Triangle Meshes with Decoder-Only Transformers](3/MeshGPT%20Generating%20Triangle%20Meshes%20with%20Decoder-On%20748074cd0e65428988982497f80a61ea)

트랜스포머 모델을 사용하여 삼각형 메시를 생성하는 새로운 방법을 소개합니다. 이 방식은 기하학적 어휘에서 토큰을 생성하여 삼각형 메시의 면으로 디코딩하는 자동 회귀 방식을 채택합니다. 이 접근법은 기존의 복셀, 포인트 클라우드, 뉴럴 필드와 같은 다양한 3D 표현 방식과 비교하여 날카로운 모서리와 높은 충실도를 가진 깔끔하고 일관된 메시 생성이 가능합니다. MeshGPT는 3D 그래픽, 특히 비디오 게임, 영화, 가상 현실 분야에서 중요한 역할을 할 것으로 기대됩니다.

[HyperDreamer: Hyper-Realistic 3D Content Generation and Editing from a Single Image](3/HyperDreamer%20Hyper-Realistic%203D%20Content%20Generation%201dd98ac158c943c7817a9e967b74219c)

단일 RGB 이미지를 사용하여 고품질의 사실적인 3D 모델을 생성하고, 이를 모든 각도에서 볼 수 있도록 하며, 또한 편집이 가능하게 하는 새로운 기술을 소개합니다. 이 방법은 기존의 3D 콘텐츠 생성 기술의 한계를 극복하기 위해 개발되었으며, HyperDreamer라는 프레임워크를 통해 고해상도 텍스처 생성, 시맨틱 인식 물질 추정, 인터랙티브 편집이 가능합니다. 이 기술은 기존 방법보다 우수한 3D 생성 및 편집 품질을 제공하며, 이를 통해 AI 생성 3D 콘텐츠에 대한 접근성과 실용성을 크게 향상시킬 수 있다고 결론짓습니다.

[ImageDream: Image-Prompt Multi-view Diffusion for 3D Generation](3/ImageDream%20Image-Prompt%20Multi-view%20Diffusion%20for%203%205c85d3d57f1c46148f4f4df95961596e)

단일 이미지를 기반으로 고품질의 3D 모델을 생성하는 혁신적인 프레임워크입니다. 이 프레임워크는 기존 기법들보다 3D 지오메트리 품질을 크게 향상시켰으며, 이미지 프롬프트에서 텍스트와의 정렬 면에서도 우수합니다. ImageDream은 복잡한 3D 생성 과제에 이미지를 활용함으로써 텍스트만을 사용할 때보다 더 정확하고 상세한 모델을 생성할 수 있는 장점이 있습니다. 그러나 이미지 기반 3D 생성은 다양한 시각적 특성을 고려해야 하며, 기하학적 일관성을 유지하는 데 어려움이 있습니다. ImageDream은 이러한 문제를 해결하고자 다단계 이미지 프롬프트 컨트롤러와 점수 증류 조정을 통합하여, 입력 이미지와 일관된 3D 모델을 생성합니다. 실험 결과, ImageDream은 기존 방식에 비해 더 정확한 지오메트리와 텍스처 품질을 가진 오브젝트 생성에 뛰어난 성능을 보였습니다.

[Text-Guided 3D Face Synthesis -- From Generation to Editing](3/Text-Guided%203D%20Face%20Synthesis%20--%20From%20Generation%20t%20dbe55700538f4655a4b64b1281b124b4)

텍스트 입력을 사용하여 정밀하고 편집 가능한 3D 얼굴을 생성하는 새로운 기술, FaceG2E를 소개합니다. 이 기술은 얼굴의 생성과 편집을 포함하는 진보적인 프레임워크로, 3D 얼굴을 순차적으로 편집하는 데 있어서 획기적입니다. FaceG2E는 얼굴 지오메트리와 텍스처를 개별적으로 생성하고, 이를 미세 조정된 텍스처 확산 모델을 통해 향상시킵니다. 또한, 자체 가이드 일관성 유지 편집을 통해 특정 얼굴 속성을 효율적으로 편집할 수 있으며, 이를 통해 원치 않는 변경을 방지합니다. 이 연구는 텍스트 기반 3D 얼굴 합성 및 편집을 위한 완벽한 파이프라인을 제공하며, 고충실도 얼굴 생성 및 정밀한 편집 제어를 위한 혁신적인 구성 요소를 도입했습니다.

[PF-LRM: Pose-Free Large Reconstruction Model for Joint Pose and Shape Prediction](3/PF-LRM%20Pose-Free%20Large%20Reconstruction%20Model%20for%20Jo%2062453518308043e69dd27bd1abef8763)

카메라 포즈와 3D 형상을 동시에 예측하는 새로운 방법을 소개합니다. 이 연구에서는 적은 수의 이미지로부터 신경 방사 필드(NeRF)를 사용하여 사실적인 3D 객체와 정확한 포즈를 재구성할 수 있는 PF-LRM 기법을 제시합니다. 이 방법의 핵심은 2D 이미지 토큰과 3D NeRF 토큰을 모두 처리할 수 있는 확장 가능한 트랜스포머 모델을 사용하는 것입니다. 이 모델은 대규모 데이터 세트에서 훈련되었으며, 특히 희박한 입력 이미지에서도 포즈 추정과 새로운 뷰 합성에서 최첨단 결과를 달성했습니다. 이 연구는 3D 재구성 분야에서 중요한 발전을 나타내며, 텍스트 및 이미지-3D 생성과 같은 다양한 애플리케이션에 적용될 수 있는 가능성을 제시합니다.

[Text-to-3D Generation with Bidirectional Diffusion using both 2D and 3D priors](3/Text-to-3D%20Generation%20with%20Bidirectional%20Diffusion%204288ca41237b4ea39dd30b8538e3d0cd)

3D 오브젝트 생성을 위한 새로운 접근법, BiDiff를 제시합니다. 이 방법은 2D와 3D 확산 모델의 강점을 결합하여, 텍스처와 기하학적 일관성이 향상된 3D 오브젝트를 빠르게 생성합니다. BiDiff는 양방향 안내를 도입하여 2D와 3D 생성 프로세스 간의 상호작용을 개선하며, 이를 통해 다양하고 사실적인 텍스처와 정교한 지오메트리를 제공합니다. 이 연구는 3D 오브젝트 생성의 속도, 다양성, 그리고 품질 측면에서 중요한 발전을 보여줍니다.

[SEEAvatar: Photorealistic Text-to-3D Avatar Generation with Constrained Geometry and Appearance](3/SEEAvatar%20Photorealistic%20Text-to-3D%20Avatar%20Generat%20d9330055a9ca4dd5a3913307d5869fb9)

텍스트 설명에서 사실적인 3D 아바타를 생성하는 새로운 방법, SEEAvatar를 소개합니다. 이 방법은 템플릿 기반 접근 방식과 진화하는 제약 조건을 통해 실제 인간의 형상과 세부 사항을 더 정확하게 모방합니다. SEEAvatar는 복잡한 지오메트리와 사실적인 외관을 결합하여 고품질의 3D 메시와 텍스처를 생성하며, 이를 통해 게임, 가상 현실, 영화 제작 등 다양한 분야에 적용 가능한 사실적인 인물 사진과 장면을 만들 수 있습니다. 그러나 이 방법은 아직 머리카락이나 속눈썹과 같은 매우 세밀한 구조, 복잡한 액세서리의 처리에 어려움을 겪고 있으며, 외관과 지오메트리 간의 정확한 매칭에도 한계가 있음을 인정합니다.

[HeadCraft: Modeling High-Detail Shape Variations for Animated 3DMMs](3/HeadCraft%20Modeling%20High-Detail%20Shape%20Variations%20fo%204183aaa5e00a4638a37755b3749dd102)

고해상도의 애니메이션 가능한 3D 헤드 모델을 생성하는 새로운 방법, HeadCraft를 소개합니다. 이 방법은 파라메트릭 템플릿 머리에 자유 표면 변위를 적용하여 대규모 3D 헤드 스캔 데이터 세트에 등록하고, 이를 2D 변위 맵으로 학습하여 세밀한 헤드 모델을 생성합니다. 이 모델은 StyleGAN2를 사용하여 학습되며, 다양한 깊이 관찰과 맞춤형 변형을 통해 모델의 다재다능함을 입증합니다. 이 연구는 3D 헤드 모델링 분야에서 사실적인 디테일과 애니메이션 가능성의 균형을 찾는 데 중요한 진전을 이루었습니다.

[Paint3D: Paint Anything 3D with Lighting-Less Texture Diffusion Models](3/Paint3D%20Paint%20Anything%203D%20with%20Lighting-Less%20Textu%2093e4491f73ad4f119b8f77f1a630f8d2)

인공지능을 활용하여 조명 효과 없이 다양한 3D 오브젝트에 대해 고품질의 텍스처를 생성하는 새로운 기술을 소개하는 논문입니다. 이 기술은 다중 뷰 이미지를 샘플링하고 3D 메시로 역투영하여 초기 텍스처 맵을 생성한 후, 라이팅이 없는 텍스처를 생성하는 두 단계 접근 방식을 사용합니다. Paint3D는 광범위한 실험을 통해 다양한 텍스트 또는 이미지를 입력으로 하는 3D 오브젝트의 텍스처링에 탁월한 성과를 보여주었으며, 고유한 조명 효과 없이 고품질의 2K 텍스처를 생성합니다. 이 연구는 3D 텍스처 생성 분야에서 중요한 발전을 이루었지만, 머티리얼 맵 생성 및 지오메트리 수정과 같은 영역에서의 한계도 인정합니다.

[HAAR: Text-Conditioned Generative Model of 3D Strand-based Human Hairstyles](3/HAAR%20Text-Conditioned%20Generative%20Model%20of%203D%20Stran%200bea58a1a6db4b1291c24c27acc3115b)

텍스트 설명을 기반으로 사실적인 인간의 헤어스타일을 생성하는 새로운 방법, HAAR을 소개합니다. 이 방법은 3D 스트랜드 기반의 지오메트리 표현을 사용하여 기존 컴퓨터 그래픽 파이프라인에 쉽게 통합될 수 있습니다. HAAR은 텍스트 설명으로 구동되는 최초의 생성 모델로, 사실적인 애니메이션을 지원합니다. 이 모델은 복잡하고 노동 집약적인 수동 3D 헤어 생성 프로세스를 사용자 친화적인 인터페이스로 대체합니다. HAAR은 다양한 소스에서 수집된 약 10,000개의 3D 헤어스타일을 포함하는 방대한 데이터 세트를 바탕으로 훈련되었으며, 이를 통해 다양한 스타일의 헤어 에셋을 빠르게 생성할 수 있습니다.

[Make-A-Character: High Quality Text-to-3D Character Generation within Minutes](3/Make-A-Character%20High%20Quality%20Text-to-3D%20Character%208e52d3f2ee6d4892ac96e25b47a8333c)

텍스트 프롬프트를 사용하여 사실적인 3D 아바타를 빠르게 생성하는 혁신적인 시스템, '마하'를 소개합니다. 이 시스템은 사용자가 얼굴 모양, 눈의 특징, 헤어스타일 등 다양한 얼굴 특징을 사용자 지정할 수 있게 하며, 고급 언어 및 비전 모델을 사용하여 간단한 텍스트 설명으로 실제와 같은 3D 아바타를 생성합니다. Mach는 메타버스, AI 에이전트, 비디오 게임, VR/AR, 영화 산업에서 필요한 개인화된 아바타를 제작하는 데 큰 도움을 줍니다.

[DiffusionGAN3D: Boosting Text-guided 3D Generation and Domain Adaption by Combining 3D GANs and Diffusion Priors](3/DiffusionGAN3D%20Boosting%20Text-guided%203D%20Generation%20%20feef96545ceb46ee862235b05bbcac9c)

3D GAN과 확산 사전을 결합하여 텍스트 가이드 3D 생성 및 도메인 적응을 개선하는 새로운 접근법을 제시합니다. 이 프레임워크는 3D 도메인 적응, 텍스트-아바타 변환, 프로그레시브 텍스처 개선을 위한 2단계 절차를 포함합니다. 주요 기여도는 고품질 텍스트 가이드 3D 도메인 적응, 로컬 편집 시나리오의 적응, 향상된 텍스트-아바타 성능, 프로그레시브 텍스처 개선입니다. 실험 결과, DiffusionGAN3D는 3D 도메인 적응과 텍스트-아바타 생성에서 다양성, 이미지 품질, 텍스트-이미지 대응 측면에서 우수한 성능을 보여주었습니다. 이 기술은 3D 이미지 생성 분야에서 중요한 발전을 나타내며, 텍스트 가이드 3D 도메인 적응 및 아바타 생성의 새로운 표준을 제시합니다.

[Hyper-VolTran: Fast and Generalizable One-Shot Image to 3D Object Structure via HyperNetworks](3/Hyper-VolTran%20Fast%20and%20Generalizable%20One-Shot%20Imag%203de9dd2f31054e7ab925f2a8871981cd)

단일 이미지에서 3D 오브젝트 구조를 재구성하는 새로운 방법을 제시하는 논문입니다. 이 방식은 하이퍼네트워크와 트랜스포머 모듈을 통합하여, 단일 뷰 이미지로부터 다양한 뷰를 합성하고 이를 3D 재구성에 사용합니다. 특히, 이 방법은 일반화 및 처리 속도 면에서 기존 방법들을 능가하며, 단 한 번의 피드 포워드 작업으로 빠른 3D 메시 재구성을 가능하게 합니다. 실험 결과, Hyper-VolTran은 다양한 메트릭에서 우수한 성능을 보여주며, 특히 보이지 않는 물체에 대한 일반화 능력이 뛰어납니다. 이는 3D 모델링 및 컴퓨터 비전 분야에서 효율적인 도구로서의 가능성을 시사합니다

[What You See is What You GAN: Rendering Every Pixel for High-Fidelity Geometry in 3D GANs](3/What%20You%20See%20is%20What%20You%20GAN%20Rendering%20Every%20Pixel%20166a8b87b2cc480abca50859dee344f3)

3D GAN을 사용하여, 고해상도에서의 신경 체적 렌더링을 효율적으로 확장하는 새로운 방법을 제시합니다. 이 연구는 각 픽셀을 명시적으로 렌더링하여 더욱 정확한 3D 기하학적 세부사항과 일관된 이미지를 생성하는 방법을 소개합니다. 본 연구는 더 적은 깊이 샘플을 사용하여 안정적인 신경 렌더링을 생성하는 방법을 제시하고, 기존 3D GANs 대비 효율적인 샘플링 방식을 보여줍니다. 이로써 고품질의 3D 장면 생성에 있어 중요한 진전을 이루었습니다.

[TextureDreamer: Image-guided Texture Synthesis through Geometry-aware Diffusion](3/TextureDreamer%20Image-guided%20Texture%20Synthesis%20thro%20431260c0345d4dbfaf91a1b478717e68)

희소한 이미지 세트에서 고화질, 재조명 가능한 텍스처를 합성하는 새로운 프레임워크입니다. 이 기법은 3D 객체의 소수의 뷰를 사용하여 다른 범주의 대상 기하학에 텍스처를 적용할 수 있습니다. 이는 기존의 방법들이 해결하지 못한 복잡한 문제입니다. TextureDreamer는 최신 확산 기반 생성 모델에서 영감을 받았지만, 단순히 텍스트로는 복잡한 패턴을 표현하는 데 한계가 있음을 인식했습니다. 이에 따라, 소수의 이미지에서 텍스처 정보를 추출하고 이를 확산 모델에 적용하는 방식으로 진보를 이루었습니다. 프레임워크의 핵심은 개인화된 기하학을 고려한 점수 증류(PGSD)로, 이를 통해 3D 메시에 재조명 가능한 텍스처를 효과적으로 전송할 수 있습니다. 실험 결과, TextureDreamer는 다양한 객체 기하학에 매끄럽고 기하학적으로 인식되는 텍스처를 합성할 수 있음을 보여주었습니다. 하지만, 특수하고 반복되지 않는 텍스처의 전송에는 어려움이 있으며, 강한 반사광이 있거나 입력 이미지가 객체 전체를 커버하지 않는 경우 일부 문제가 발생할 수 있습니다.

[Score Distillation Sampling with Learned Manifold Corrective](3/Score%20Distillation%20Sampling%20with%20Learned%20Manifold%20%20576e9309c1714bb093621f4936a7a87f)

텍스트-이미지 확산 모델에 대한 새로운 접근 방식인 LMC-SDS (Learned Manifold Corrective with Score Distillation Sampling)를 소개합니다. 기존 Score Distillation Sampling의 한계를 극복하고자 하는 이 연구는 이미지 합성, 편집, 이미지-이미지 변환 네트워크 훈련, 텍스트-3D 합성 등 다양한 응용 프로그램에서 그 효과를 입증합니다. LMC-SDS는 이미지의 전역적인 변화에 집중하며, 더 자연스러운 이미지를 생성하기 위해 고주파 세부 사항보다는 이미지 매니폴드의 방향으로 더 나은 그라디언트를 제공합니다. 이 연구는 최적화의 안정성을 개선하고, 결과의 충실도를 높이는 방향으로 확산 모델의 내재된 이미지 사전 지식을 활용하며, 이 분야의 연구 및 응용 프로그램 발전에 기여합니다.

[LGM: Large Multi-View Gaussian Model for High-Resolution 3D Content Creation](3/LGM%20Large%20Multi-View%20Gaussian%20Model%20for%20High-Resol%2087f2377993d74e9892aeeb1bcf3e3212)

 가우시안 스플래팅과 비대칭 U-Net 아키텍처를 사용하여 단일 뷰 이미지 또는 텍스트 입력에서 고해상도 3D 콘텐츠를 신속하게 생성하는 새로운 방법론을 제시합니다. 이 접근 방식은 고메모리 요구 사항과 저해상도 훈련의 한계를 극복하며, 다양한 입력에 대해 복잡하고 상세한 3D 객체를 효과적으로 생성하는 능력을 입증합니다.

[EscherNet: A Generative Model for Scalable View Synthesis](3/EscherNet%20A%20Generative%20Model%20for%20Scalable%20View%20Syn%20d7e0d99522554aba8f9cf89287966ed3)

EscherNet은 유연한 수의 레퍼런스 뷰를 기반으로 임의의 카메라 포즈로 일관된 타깃 뷰를 생성할 수 있는 새로운 확산 모델로, 기존 방법들이 볼륨 렌더링이나 실제 3D 기하학에 의존하던 한계를 극복하며 참조 뷰와 대상 뷰 사이, 그리고 대상 뷰들 사이의 일관성을 자연스럽게 통합합니다. 이는 카메라 위치 인코딩(CaPE) 설계를 통해 특정 좌표 시스템이나 실제 3D 기하학 없이도 확장 가능한 뷰 합성을 달성하는 차별점을 가집니다.

[Advances in 3D Generation: A Survey](3/Advances%20in%203D%20Generation%20A%20Survey%20cca2df54f4524b8dabe275ea4f0caca9)

이 논문은 지난 10년 동안의 3D 생성 기술 발전을 조사하며, 특히 신경망을 이용한 표현 방식과 생성 모델의 발전에 초점을 맞춥니다. 3D-GAN, DeepSDF, EG3D 등 다양한 3D 생성 결과를 검토하고, 이 분야가 비디오 게임, 영화 제작 등 광범위한 응용 분야에서 요구하는 3D 자산 생성의 중요성을 강조합니다.

[HeadStudio: Text to Animatable Head Avatars with 3D Gaussian Splatting](3/HeadStudio%20Text%20to%20Animatable%20Head%20Avatars%20with%203D%20136ea570ed6e43a4ace1f70c790bd3b4)

HeadStudio는 텍스트 입력만으로 사실적이고 애니메이션 가능한 3D 헤드 아바타를 생성할 수 있는 프레임워크를 제시하여, 기존의 텍스트 기반 아바타 생성 방법들이 직면한 동적 애니메이션 확장의 한계를 극복합니다. 이 프레임워크는 3D 가우시안 스플래팅과 FLAME 모델을 결합하여, 고품질의 실시간 렌더링 아바타 생성에서 탁월한 성능을 보임으로써 디지털 아바타 생성 분야에서의 새로운 가능성을 열어줍니다.

[FLAME:Learning a model of facial shape and expression from 4D scans](3/FLAME%20Learning%20a%20model%20of%20facial%20shape%20and%20express%2037c0c5d8f66249e2a971f7d852d01415)

FLAME 모델은 다양한 연령, 성별, 인종을 포괄하는 광범위한 데이터로부터 학습하여, 3D 얼굴 모델링에서의 표현력과 정확성을 크게 향상시킵니다. 이 모델은 얼굴 형태, 자세, 표정을 독립적으로 모델링하여, 기존 방법들이 제한적으로 다루던 얼굴의 미묘한 변화까지 자연스럽게 재현할 수 있는 능력을 갖추고 있습니다.

[Consolidating Attention Features for Multi-view Image Editing](3/Consolidating%20Attention%20Features%20for%20Multi-view%20Im%20603b2e367d9c49d087c75ad31048353b)

이 논문은 멀티뷰 이미지 세트의 일관된 편집을 위해 3D 기하학적 제어를 사용하여 이미지 간의 일관성을 향상시키는 새로운 접근 방식을 제안합니다. 이를 위해 이미지 확산 모델을 활용하고, 생성 과정에서 주의 특성을 점진적으로 통합하기 위해 QNeRF라는 쿼리 특성 공간 신경 복사 필드를 도입합니다. 이 방법은 다양한 시점에서 촬영된 이미지들 사이의 일관성을 유지하면서 복잡한 형태와 아티큘레이션 변경을 가능하게 하며, 실험 결과는 제안된 접근법이 뛰어난 시각적 품질과 일관성을 달성함을 보여줍니다.

[V3D: Video Diffusion Models are Effective 3D Generators](3/V3D%20Video%20Diffusion%20Models%20are%20Effective%203D%20Genera%20caed33d40e244d9a956a789dea77d767)

V3D는 비디오 확산 모델을 미세 조정하여 고품질의 3D 객체 및 장면을 빠르고 일관되게 생성하는 새로운 방법을 제안합니다. 이 접근법은 3분 이내에 섬세한 3D 자산을 생성할 수 있으며, 기존의 3D 생성 방법에 비해 상당한 성능 향상을 보여줍니다.

[SV3D: Novel Multi-view Synthesis and 3D Generation from a Single Image using Latent Video Diffusion](3/SV3D%20Novel%20Multi-view%20Synthesis%20and%203D%20Generation%20%20df3e40a7d5c04ce189a8af55da1f3e62)

SV3D는 단일 이미지로부터 다양한 시점의 고품질 이미지를 생성하고 이를 활용해 3D 메시를 최적화하는 새로운 방법론을 제시합니다. 이 모델은 비디오 확산 기술을 활용하여 높은 해상도에서 임의의 카메라 경로에 대한 일관된 시점 합성을 가능하게 함으로써, 제어 가능성, 다시점 일관성, 및 실세계 이미지에 대한 일반화 능력을 크게 향상시킵니다.

[GRM: Large Gaussian Reconstruction Model for Efficient 3D Reconstruction and Generation](3/GRM%20Large%20Gaussian%20Reconstruction%20Model%20for%20Effici%200510a86b60cc4100baef78ed10e21257)

이 논문은 픽셀 정렬 3D 가우시안을 활용하고 순수 트랜스포머 기반 아키텍처를 도입한 가우시안 재구성 모델(GRM)을 소개하여, 희소 뷰 3D 재구성, 텍스트 및 이미지에서 3D 생성까지 다양한 3D 생성 작업에서 빠르고 고품질의 결과를 달성합니다. GRM의 핵심 차별점은 높은 해상도의 3D 콘텐츠를 효율적으로 생성할 수 있는 새로운 트랜스포머 기반 업샘플러와 장거리 시각적 단서를 효과적으로 활용하는 능력에 있습니다

[Garment3DGen: 3D Garment Stylization and Texture Generation](3/Garment3DGen%203D%20Garment%20Stylization%20and%20Texture%20Ge%20df71a5f08201485a8f5b96b2528282f0)

Garment3DGen은 이미지나 텍스트 프롬프트로부터 직접 시뮬레이션 준비가 된 3D 옷감을 자동으로 생성하는 새로운 방법론을 제시합니다. 이 방법론은 고품질의 텍스처화된 3D 옷감을 빠르고 효율적으로 생성함으로써, 전문 소프트웨어와 전문 지식에 대한 의존성을 줄입니다.

[2D Gaussian Splatting for Geometrically Accurate Radiance Fields](3/2D%20Gaussian%20Splatting%20for%20Geometrically%20Accurate%20R%20314a2dcc862641498c0b63bbe297d3cf)

 2D Gaussian Splatting 기법을 도입하여 3D 장면의 기하학적 모델링과 렌더링에서 정확도와 효율성을 크게 향상시켰습니다. 또한, 깊이 왜곡과 정상 일관성에 대한 새로운 정규화 손실을 적용함으로써, 세밀하고 노이즈가 없는 고품질의 표면 재구성을 가능하게 합니다. 2D Gaussian Splatting은 3D Gaussian Splatting에 비해 다양한 시점에서 일관된 깊이와 노멀을 제공하며, 더 정밀하고 노이즈가 적은 기하학적 재구성을 달성합니다.

[MonoPatchNeRF: Improving Neural Radiance Fields with Patch-based Monocular Guidance](3/MonoPatchNeRF%20Improving%20Neural%20Radiance%20Fields%20wit%2030b9510208ae4c759420d2d65b131195)

이 논문은 MonoPatchNeRF라는 새로운 방법을 제안하여 테스트 뷰에서 실제적인 이미지와 정확한 법선을 렌더링하고, 완전한 메쉬를 재구성함으로써 단일 이미지로부터 높은 품질의 3D 재구성을 달성합니다. 또한, 새로운 비지도 학습 기법인 가상 뷰 패치 정규화를 도입하여 훈련 속도를 높이고, 밀도 제약을 사용하여 기하학적 안정성을 향상시키는 등 다양한 기술적 혁신을 제시합니다.

[Taming Latent Diffusion Model for Neural Radiance Field Inpainting](3/Taming%20Latent%20Diffusion%20Model%20for%20Neural%20Radiance%20%205160ed00167f4627a20542dfa5c98467)

이 논문은 NeRF 인페인팅을 위해 라텐트 확산 모델과 마스크 적대적 학습을 활용하는 새로운 방법을 제시합니다. 제안된 방법은 특히 고주파 세부 사항을 강조하고 질감의 일관성을 유지하면서 시각적으로 현실적인 3D 이미지를 생성할 수 있도록 개선된 성능을 보여줍니다.

[Urban Radiance Field Representation with Deformable Neural Mesh Primitives](3/Urban%20Radiance%20Field%20Representation%20with%20Deformabl%209ab9c60f624646628eb486e7f0e166f9)

 Deformable Neural Mesh Primitives (DNMP)를 사용하여 대규모 도시 환경에서 효율적이고 사실적인 3D 렌더링을 달성하는 새로운 방법을 제안합니다. DNMP는 기하학적 및 발광 정보를 매개변수화하여 전통적인 렌더링 기법들의 한계를 극복하고, 계산 효율성을 크게 향상시키면서도 높은 품질의 이미지를 생성할 수 있습니다.

[Urban Architect: Steerable 3D Urban Scene Generation with Layout Prior](3/Urban%20Architect%20Steerable%203D%20Urban%20Scene%20Generatio%20abe11a4da0b347d1ace260fd0e8d6ca4)

이 논문은 기존 텍스트-이미지 확산 모델을 활용하여 3D 도시 장면을 생성하는 'Urban Architect'라는 방법론을 도입합니다. 이 방법론은 3D 레이아웃을 조건부 정보로 적용하여 Variational Score Distillation (VSD) 과정을 제어하고, 이를 통해 생성된 장면의 기하학적 일관성을 유지하면서 사실적인 렌더링을 실현합니다.

[Hash3D: Training-free Acceleration for 3D Generation](3/Hash3D%20Training-free%20Acceleration%20for%203D%20Generatio%20429d9b1246f8474a8266a3b600c7f655)

3D 생성 모델링에서 Hash3D라는 새로운 기술을 소개하여, 기존 방법 대비 계산 시간을 크게 단축시키면서도 시각적 품질을 유지하는 방법을 제안합니다. Hash3D는 중복 계산을 최소화하기 위해 그리드 기반 해시 테이블을 활용하여 특징을 효율적으로 저장하고 재사용함으로써, 다양한 3D 생성 작업에 걸쳐 높은 일관성과 성능을 보장합니다.