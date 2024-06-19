# ImageGeneration

[Enhancing Detail Preservation for Customized Text-to-Image Generation:A Regularization-Free Approach](3/Enhancing%20Detail%20Preservation%20for%20Customized%20Text-%2051ab19dc222c49c2b11285d46c1e3f6f)

정규화 없이 더 빠른 학습과 세밀한 디테일 보존을 가능하게 하는 새로운 프레임워크 ProFusion을 소개합니다. 이 프레임워크는 텍스트 입력에 따라 맞춤형 이미지를 생성하며, 사전 학습된 PromptNet 모델을 사용해 FFHQ 데이터세트에서 미세 조정을 수행합니다. ProFusion은 실험에서 이전 방법들보다 향상된 이미지 품질과 텍스트 프롬프트 충족도를 보여주었습니다.

[GLIGEN:Open-Set Grounded Text-to-Image Generation](3/GLIGEN%20Open-Set%20Grounded%20Text-to-Image%20Generation%202b4844b8ac4b4a3884d2e5d65f767313)

언어 유도 이미지 생성(GLIGEN) 모델의 성능을 다각적으로 분석합니다. 이 연구는 특히, 게이트 자기 주의, 널 캡션 사용, 푸리에 대 MLP 임베딩 등의 기술에 대한 효과를 평가하며, 이미지 인페인팅 및 키포인트 접지와 같은 작업에서도 모델의 유연성과 강력한 성능을 입증합니다. GLIGEN 모델은 경계 상자, 참조 이미지, 키포인트 등의 다양한 입력을 사용하여 텍스트-이미지 생성에 추가적인 근거를 제공합니다. 실험 결과, 모델은 LVIS 데이터 세트와 COCO2017 검증 세트에서 사전 학습을 통해 성능이 향상됨을 보여줍니다.

[Image Style Transfer Using Convolutional Neural Networks](3/Image%20Style%20Transfer%20Using%20Convolutional%20Neural%20Ne%2032b476ad38134533a27829e01c370467)

컨볼루션 신경망을 이용한 이미지 스타일 전송 기술을 소개합니다. 이 기술은 높은 수준의 의미적 콘텐츠를 유지하면서 한 이미지의 스타일을 다른 이미지에 적용하는 방법입니다. 연구진은 VGG 네트워크를 사용하여 콘텐츠와 스타일을 분리하고, 이를 통해 새로운 이미지를 생성하는 방법을 개발했습니다. 이 방법은 콘텐츠와 스타일 손실을 최소화하는 최적화 문제로 공식화되며, 결과 이미지는 콘텐츠 이미지의 구조를 유지하면서 스타일 이미지의 텍스처와 색상을 적용합니다. 이 연구는 이미지 스타일 전송 분야에서 중요한 발전을 나타내며, 새로운 창의적 응용을 가능하게 합니다.

[Drag Your GAN: Interactive Point-based Manipulation on the Generative Image Manifold](3/Drag%20Your%20GAN%20Interactive%20Point-based%20Manipulation%2083ecb6eda899443f811c4e8d5e0aa5bb)

사용자가 사전 학습된 GAN(생성적 적대 신경망)으로 이미지를 보다 정확하고 인터랙티브하게 조작할 수 있는 새로운 기술, DragGAN을 소개합니다. 이 기술은 이미지의 특정 부분을 사용자가 원하는 위치로 드래그함으로써 이미지의 포즈, 모양, 표정, 레이아웃 등을 세밀하게 조절할 수 있게 해줍니다. DragGAN의 핵심은 사용자가 설정한 포인트를 따라 이미지를 변형하는 기능으로, 이를 통해 동물, 자동차, 사람, 풍경 등 다양한 카테고리의 이미지를 조작할 수 있습니다. 이 기술은 GAN의 생성 이미지 매니폴드 내에서 작동하기 때문에, 복잡한 시나리오에서도 사실적인 결과를 얻을 수 있습니다.

[Denoising Diffusion Probabilistic Models](3/Denoising%20Diffusion%20Probabilistic%20Models%20414e1dcce37a41a297b66433a83501ce)

고품질 이미지 생성을 위한 확산 확률 모델에 대해 소개합니다. 이 모델은 변형 추론을 사용한 마르코프 체인, 노이즈 제거 점수 매칭, 랑방 역학, 에너지 기반 및 자동 회귀 모델과의 연관성을 탐구합니다. 연구진은 확산 모델이 이미지의 기본 구조를 효과적으로 포착하고 다양한 분야에서 창의적으로 활용될 수 있음을 입증했습니다. 이 모델은 CIFAR10 및 LSUN 데이터 세트에서 우수한 성능을 보여주며, 코드는 GitHub에서 공개적으로 사용할 수 있습니다.

[High-Resolution Image Synthesis with Latent Diffusion Models](3/High-Resolution%20Image%20Synthesis%20with%20Latent%20Diffus%200da4edf563324a8084192bd68de409dc)

잠재 확산 모델(LDM)을 사용하여 고해상도 이미지 합성의 계산 복잡성을 줄이는 새로운 방법을 소개합니다. 연구원들은 자동 인코더의 잠재 공간에서 확산 모델을 훈련시켜, 고해상도 이미지를 효율적으로 합성하면서 시각적 충실도를 유지하는 방법을 개발했습니다. LDM은 무조건적 이미지 합성, 인페인팅, 초해상도 등 다양한 작업에서 뛰어난 성능을 보이며, 픽셀 기반 확산 모델보다 계산 비용을 크게 줄였습니다. 이 연구는 고품질 이미지 합성에 대한 새로운 접근 방식을 제시하며, 고해상도 이미지 합성의 가능성을 확장합니다.

[An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion](3/An%20Image%20is%20Worth%20One%20Word%20Personalizing%20Text-to-I%209f65831728f7402e876587dbad2d637e)

텍스트-이미지 모델을 사용하여 자연어를 통해 이미지를 생성하는 과정에서 고유한 개념을 표현하는 방법에 초점을 맞춥니다. 이 연구는 새로운 '의사 단어'를 통해 개별화된 창작물을 만들 수 있는 방법을 제시합니다. 이러한 '의사 단어'는 기존 텍스트-이미지 모델의 어휘를 확장하여, 사용자가 특정 개념을 더 명확하게 표현할 수 있도록 합니다. 이 접근 방식은 다양한 작업에서 우수한 성능을 보이며, 사용자가 독특한 오브젝트를 삽입하거나 스타일을 변형하는 데 유용합니다. 연구진은 이 기술이 예술적 영감에서 제품 디자인에 이르는 다양한 응용 분야에 활용될 수 있다고 믿습니다.

[DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation](3/DreamBooth%20Fine%20Tuning%20Text-to-Image%20Diffusion%20Mod%20a62b218116354a43950c6b0229fef37d)

대규모 텍스트-이미지 AI 모델을 개인화하는 새로운 방식을 제시합니다. 이 방법은 피사체의 소수 이미지를 입력으로 활용하여 모델에 피사체 고유의 식별자를 연결함으로써, 다양한 맥락과 조건에서 피사체의 새롭고 사실적인 이미지를 생성할 수 있게 합니다. 이는 피사체 중심의 이미지 생성을 위한 새로운 데이터 세트와 평가 프로세스를 통해 실현됩니다. 이 기술은 피사체의 주요 특징을 유지하면서 새로운 컨텍스트, 포즈, 조명에서의 이미지 생성을 가능하게 하여, 예술적 렌더링이나 텍스트 기반 뷰 합성에 활용될 수 있습니다.

[InstructPix2Pix: Learning to Follow Image Editing Instructions](3/InstructPix2Pix%20Learning%20to%20Follow%20Image%20Editing%20I%202593da58729548cba4706f9f0344b119)

사람이 작성한 지침에 따라 이미지를 편집하는 새로운 방법인 InstructPix2Pix를 소개합니다. 이 방법은 GPT-3 언어 모델과 Stable Diffusion 텍스트-이미지 모델을 결합하여 합성 데이터로 모델을 학습시킵니다. InstructPix2Pix는 명령어에 따라 편집 작업을 수행할 수 있으며, 추가 예제나 미세 조정 없이도 빠르고 직관적으로 이미지를 편집할 수 있는 능력을 제공합니다. 이 모델은 오브젝트 교체, 이미지 스타일 변경, 설정 수정과 같은 다양한 편집 작업을 효과적으로 수행할 수 있습니다.

[Adding Conditional Control to Text-to-Image Diffusion Models](3/Adding%20Conditional%20Control%20to%20Text-to-Image%20Diffus%20b1645e59912b4e29b30c7cf9e2f9dc46)

ControlNet이라는 신경망 구조를 소개합니다. ControlNet은 사전 학습된 대규모 확산 모델에 추가 입력 조건을 통합하여 제어하는 방법을 제공합니다. 이 방법은 작은 데이터셋에서도 견고하며, 개인용 디바이스에서 효과적으로 작동하고, 필요에 따라 대규모 데이터 세트로 확장 가능합니다. ControlNet의 적용으로, 확산 모델은 에지 맵, 세분화 맵, 키포인트 등의 조건부 입력을 통합하여 더욱 정교하게 제어할 수 있게 되었습니다.

[HyperNetworks](3/HyperNetworks%20374804b12cca4571b86dfcd18c75d87b)

인공 신경망의 가중치를 동적으로 생성하기 위해 사용되는 또 다른 작은 네트워크에 초점을 맞춥니다. 이 하이퍼네트워크는 주 신경망의 각 레이어에 대한 가중치를 생성하며, 특히 순환 신경망에서 시간에 따라 변하는 가중치를 생성하는 데 유용합니다. 연구진은 이 기술이 LSTM과 같은 순환 네트워크의 성능을 개선하고, 다양한 작업에서 경쟁력 있는 결과를 달성하는 데 도움이 된다는 것을 발견했습니다. 이는 하이퍼네트워크가 기존 방법에 비해 적은 수의 학습 가능한 매개변수로 비슷한 수준의 성능을 발휘할 수 있음을 시사합니다.

[A Style-Based Generator Architecture for Generative Adversarial Networks](3/A%20Style-Based%20Generator%20Architecture%20for%20Generativ%209ca2946393854a7db6cf072c756d6822)

생성적 적대 신경망(GAN)을 사용하여 고품질 이미지를 생성하는 새로운 스타일 기반 제너레이터 아키텍처를 소개합니다. 이 아키텍처는 각 컨볼루션 레이어에서 이미지 스타일을 조정하여 다양한 스케일의 이미지 특징을 제어할 수 있습니다. 또한, 네트워크에 직접 노이즈를 추가하여 확률적 변화와 고수준 속성을 자동으로 분리하는 기능을 제공합니다. 저자들은 이 아키텍처의 장점을 두 가지 새로운 자동화된 지표인 지각 경로 길이와 선형 분리 가능성을 통해 정량화했습니다. 이 연구는 GAN의 이해와 제어 가능성을 높이는 데 중요한 기여를 하고 있습니다.

[AnimateDiff: Animate Your Personalized Text-to-Image Diffusion Models without Specific Tuning](3/AnimateDiff%20Animate%20Your%20Personalized%20Text-to-Imag%20b4f0410aa19d4a508cd13517c9b4b89c)

사용자가 개인화된 텍스트-이미지(T2I) 모델을 특정 튜닝 없이 애니메이션 생성기로 확장할 수 있는 새로운 프레임워크, AnimateDiff를 소개합니다. 이 방법은 이미 학습된 동작 정보를 활용하여, 다양한 스타일의 애니메이션 클립을 생성할 수 있도록 합니다. 기존 T2I 모델의 파라미터는 그대로 유지되며, 동작 모델링 모듈만이 추가됩니다. 이 연구는 개인화된 T2I 모델의 애니메이션화에 있어 새로운 기준을 제시하며, 실험을 통해 다양한 스타일의 애니메이션에서 높은 수준의 품질과 모션 부드러움을 유지할 수 있음을 입증합니다.

[HyperDreamBooth: HyperNetworks for Fast Personalization of Text-to-Image Models](3/HyperDreamBooth%20HyperNetworks%20for%20Fast%20Personaliza%201a49cd1c5ef6481196afc377968ed195)

HyperDreamBooth, 텍스트-이미지 확산 모델의 개인화를 위한 새로운 기술을 소개합니다. 이 기술은 단일 입력 이미지를 사용하여 네트워크 가중치의 부분 집합을 빠르게 예측하고 세부 조정합니다. 이를 통해 기존의 DreamBooth 방법보다 25배 빠른 개인화가 가능하며, 모델의 완결성과 스타일 다양성을 유지하면서 대상의 본질과 세부 사항을 정확하게 근사합니다. 이 방법은 특히 하나 또는 몇 개의 피사체 이미지로 이미지를 생성하는 맥락에서 중요하며, 기존의 텍스트 반전이나 드림부스와 같은 방법보다 우수한 결과를 제공합니다.

[Tune-A-Video: One-Shot Tuning of Image Diffusion Models for Text-to-Video Generation](3/Tune-A-Video%20One-Shot%20Tuning%20of%20Image%20Diffusion%20Mo%20c32ea2951c4c4935bc8f50cd7122890c)

텍스트 기반의 비디오 생성을 위한 새로운 접근 방식을 제시합니다. 이 연구는 단 하나의 텍스트-비디오 쌍과 사전 학습된 텍스트-이미지(T2I) 모델을 사용하여 텍스트-비디오(T2V) 생성을 가능하게 합니다. 기존 T2V 모델들이 대규모 비디오 데이터 세트에 의존하는 것과 달리, 이 방법은 비용과 시간을 절감합니다. '튠어 비디오'는 T2I 모델의 공간적 자기주의를 시공간적 차원으로 확장하여, 프레임 간 일관된 콘텐츠 생성을 지원합니다. 또한, 구조 안내 기법과 효율적인 튜닝 전략을 도입하여 부드럽고 일관된 비디오를 만듭니다. 실험 결과, 이 방법은 다양한 텍스트 기반 비디오 생성 애플리케이션에서 우수한 성능을 보여줍니다.

[InternVid: A Large-scale Video-Text Dataset for Multimodal Understanding and Generation](3/InternVid%20A%20Large-scale%20Video-Text%20Dataset%20for%20Mul%208ae8de7dc1164348a7c4b07ee667d855)

비디오 언어 모델링을 위한 새로운 대규모 비디오 중심 데이터 세트 'InternVid'를 소개하는 연구입니다. 이 데이터 세트는 700만 개 이상의 비디오-텍스트 쌍을 포함하며, 멀티스케일 캡션 방식으로 고품질 데이터를 제공합니다. 이 연구는 또한 새로운 비디오 언어 모델 'ViCLIP'을 제시하는데, 이는 대조 학습과 마스크 모델링을 사용하여 효과적으로 비디오 언어 표현을 학습합니다. InternVid는 다양한 멀티모달 대화 시스템과 텍스트-비디오 생성 작업에도 사용될 수 있으며, 특히 고해상도 비디오를 생성하는 데에 유용합니다. 이 연구는 비디오 언어 이해와 생성에 대한 새로운 표준을 제시하며, 향후 연구와 애플리케이션을 위한 중요한 리소스를 제공합니다.

[DragonDiffusion: Enabling Drag-style Manipulation on Diffusion Models](3/DragonDiffusion%20Enabling%20Drag-style%20Manipulation%20o%20f33d9d8ad36244f69d9b0e27c9eb5c96)

새로운 이미지 편집 도구를 소개합니다. 이 도구는 객체 이동, 크기 조정, 외형 교체, 콘텐츠 드래그 등과 같은 복잡한 이미지 편집 작업을 위해 설계되었습니다. 기존의 Stable Diffusion 모델을 기반으로 하며, 특별한 모델 미세 조정이나 추가 학습 없이 사용할 수 있습니다. 이 기법은 중간 이미지 특징 간의 강력한 대응을 활용하여, 이미지 내에서 세밀한 편집을 가능하게 합니다. 결과적으로 DragonDiffusion은 편집된 이미지와 원본 이미지 간의 일관성을 유지하면서 정밀한 이미지 조작을 제공합니다.

[DragDiffusion: Harnessing Diffusion Models for Interactive Point-based Image Editing](3/DragDiffusion%20Harnessing%20Diffusion%20Models%20for%20Inte%20f3668d52c4e54e9993d23f20dc46cdda)

인터랙티브 포인트 기반 이미지 편집을 위한 새로운 방법인 드래그디퓨전(DRAGDIFFUSION)을 소개합니다. 이 방법은 사용자가 이미지에서 이동할 점을 선택하고, 해당 점들을 새로운 위치로 '드래그'하며, 변경하고자 하는 영역을 정의할 수 있게 합니다. 기존의 드래그 앤 드롭 방식과 달리, 드래그디퓨전은 확산 모델을 사용하여 보다 정밀한 이미지 편집을 가능하게 합니다. 이를 통해 사용자는 다양한 객체, 카테고리 및 스타일을 가진 이미지를 효과적으로 편집할 수 있습니다. 이 방법은 Unsplash, Pexels, Pixabay 등에서 가져온 이미지들을 사용하여 테스트되었습니다.

[SDXL: Improving Latent Diffusion Models for High-Resolution Image Synthesis](3/SDXL%20Improving%20Latent%20Diffusion%20Models%20for%20High-Re%20cf09bd5d1525463f9feaca0f35624659)

SDXL, 텍스트-이미지 합성을 위한 안정적 확산 모델의 개선된 버전을 소개합니다. 이 모델은 크기가 더 큰 UNet 백본과 두 번째 텍스트 인코더를 사용하며, 다양한 컨디셔닝 기법과 다중 종횡비 훈련을 통해 이미지의 시각적 품질을 향상시킵니다. SDXL은 이전 버전의 Stable Diffusion과 비교하여 현저한 개선을 보이며, 최첨단 이미지 생성기와 경쟁력 있는 결과를 제공합니다. 저자들은 이 모델의 코드와 가중치를 공개함으로써 개방적 연구와 투명성을 촉진합니다.

[JourneyDB: A Benchmark for Generative Image Understanding](3/JourneyDB%20A%20Benchmark%20for%20Generative%20Image%20Underst%208ea94bf889e640bbb8ab783e82fa1378)

인공 지능 생성 콘텐츠(AIGC)의 발전과 관련하여, 사용자가 텍스트 프롬프트로부터 고품질 이미지를 생성할 수 있는 다양한 플랫폼들의 인기 상승을 다루고 있습니다. 연구진은 시각적 이해 작업을 잘 수행하는 기존 기반 모델의 한계를 극복하기 위해 4백만 개의 이미지와 관련 텍스트 프롬프트로 구성된 데이터 세트, JourneyDB를 개발하였습니다. 이 데이터 세트는 프롬프트 반전, 스타일 검색, 이미지 캡션, 시각적 질문 답변 등의 작업으로 평가됩니다. 연구 결과, 최신 멀티모달 모델이 생성 콘텐츠에 대해 실제 데이터에 비해 덜 효과적으로 작동한다는 사실이 밝혀졌으며, 미세 조정을 통해 성능이 향상되었습니다. 이 연구는 생성된 이미지의 시각적 이해에 대한 새로운 관점을 제시하고, 생성 콘텐츠에 대한 현재 모델의 성능 한계를 조명합니다.

[TryOnDiffusion: A Tale of Two UNets](3/TryOnDiffusion%20A%20Tale%20of%20Two%20UNets%2064f3a849a45b4ddfb53d4a4c2fd8f5e6)

복잡한 체형과 자세 변화를 동반한 의류 시도 결과를 고해상도로 생성하는 새로운 방법을 제시합니다. 이 기술은 병렬 유니넷(Parallel-UNet) 아키텍처를 활용하여, 기존 가상 의류 시도 방식에서 발생하는 비현실적인 왜곡이나 오류를 줄입니다. TryOnDiffusion은 1024×1024 해상도로 의상의 세부사항을 잘 보존하며, 비전문가 사용자 연구에서도 다른 상위 방법들보다 우수한 평가를 받았습니다. 이 기술은 가상 시도 분야에서 중요한 발전을 이루었으며, 비디오 입력 처리 확장 등 미래 연구 계획을 갖고 있습니다.

[TokenFlow: Consistent Diffusion Features for Consistent Video Editing](3/TokenFlow%20Consistent%20Diffusion%20Features%20for%20Consis%20ce531d0ecfa841a7bb4e830bc1517e9e)

이미지 확산 모델을 활용한 새로운 텍스트 기반 비디오 편집 프레임워크, TokenFlow를 소개합니다. 이 방법은 비디오의 내부 표현을 확산 특성 공간에서 조사하여 일관된 비디오 편집을 가능하게 합니다. 특히, 원본 비디오의 움직임을 보존하면서도 텍스트 프롬프트에 따라 의미론적으로 수정된 비디오를 생성합니다. 이 프레임워크는 기존 베이스라인을 능가하는 시간적 일관성을 보여주지만, 구조적 변화가 필요한 편집에는 제한적이며, 이미지 편집 기법이 구조를 보존하는 데 실패할 경우 시각적 아티팩트를 초래할 수 있습니다. 연구는 확산 모델을 활용한 비디오 합성을 향상시킬 수 있는 새로운 인사이트를 제공하며, 미래 연구를 위한 기반을 마련합니다.

[Dual-Stream Diffusion Net for Text-to-Video Generation](3/Dual-Stream%20Diffusion%20Net%20for%20Text-to-Video%20Genera%20fc063fe861b94cfea7deefb273118d15)

새로운 이중 스트림 확산 네트워크(DSDN)를 소개합니다. 이 기술은 텍스트 설명에서 생성된 비디오의 일관성을 향상시키는 것을 목표로 합니다. DSDN은 두 개의 독립적인 스트림, 즉 비디오 내용과 동작을 각각 다루며, 이를 교차 트랜스포머 상호 작용 모듈을 통해 조화롭게 결합합니다. 이 방법은 실험적으로 더 연속적이고 깜박임이 적은 비디오 생성의 효과를 입증하며, 내용과 동작의 일관성과 다양성을 강화하는데 기여합니다.

[DragNUWA: Fine-grained Control in Video Generation by Integrating Text, Image, and Trajectory](3/DragNUWA%20Fine-grained%20Control%20in%20Video%20Generation%20%20c0e946d8895e478eaa65c39a83908eb4)

비디오 생성에 있어 텍스트, 이미지, 궤적(동작 경로)을 결합한 새로운 접근법을 소개합니다. 이 시스템은 비디오의 문맥적, 공간적, 시간적 측면을 통제할 수 있는 능력을 갖추고 있습니다. 이를 위해, 연구진은 비디오의 첫 이미지, 텍스트 설명, 그리고 동작 궤적을 사용하여 더 정교한 비디오 콘텐츠 생성을 실현했습니다. 특히, DragNUWA는 복잡한 동작 및 카메라 움직임을 포함한 궤적 제어, 이미지와 다양한 텍스트를 결합하여 새로운 객체를 추가하는 언어 제어, 그리고 실제 세계 및 예술적인 비디오 생성을 가능하게 하는 이미지 제어를 강조합니다. 이 시스템은 오픈 도메인 비디오에서 복잡한 궤적을 가진 여러 객체를 동시에 제어할 수 있으며, 16 프레임과 576 × 320 해상도의 비디오를 생성합니다. DragNUWA는 텍스트, 이미지, 궤적의 통합을 통해 비디오 제작의 미묘한 제어를 달성하는 혁신적인 접근법을 제시합니다.

[CoDeF: Content Deformation Fields for Temporally Consistent Video Processing](3/CoDeF%20Content%20Deformation%20Fields%20for%20Temporally%20Co%2049143d3328cf4e859c9361d6e768cfe7)

비디오 프로세싱에 대한 새로운 접근 방식을 제시하는 논문입니다. 이 방법은 이미지 처리 기술을 비디오에 적용하여 이미지 번역, 향상, 분할 등의 작업을 효과적으로 수행합니다. 연구진은 2D와 3D 해시 기반 시스템을 결합하여 복잡한 움직임을 포착하고, 비디오 재구성의 품질을 크게 향상시켰습니다. 이 방법은 비디오 출력물의 품질과 일관성을 개선함으로써 비디오 처리 기술에 혁신적인 변화를 가져올 것으로 기대됩니다.

[InstructDiffusion: A Generalist Modeling Interface for Vision Tasks](3/InstructDiffusion%20A%20Generalist%20Modeling%20Interface%20%200232fa6920944880a096a9e4f9a1ba71)

컴퓨터 비전 분야의 새로운 접근법을 소개합니다. 이 연구에서는 입력 이미지와 인간의 지시를 받아 이미지 편집, 세분화, 키포인트 추정, 검출 및 저수준 비전 작업을 수행하는 통합 모델을 제시합니다. 컴퓨터 비전 작업의 다양성 때문에 이 분야에는 통일된 솔루션이 부족했으나, 이 연구는 모든 비전 문제를 '이미지 생성' 문제로 취급하는 새로운 방법을 통해 이러한 한계를 극복하려고 합니다. 연구진은 이미지에서 특정 객체의 색상 변경 또는 특정 지점에 색상 점을 배치하는 등의 방법을 통해 시각적 정보를 처리합니다. 이 접근법은 연속적인 데이터를 불연속적인 덩어리로 바꾸는 오류를 피할 수 있습니다. 이 논문은 컴퓨터 비전 작업에 대한 보다 일반화되고 통합된 접근 방식을 제시하며, 이는 컴퓨터 비전 시스템을 개선하는 데 큰 진전을 이룰 수 있습니다.

[MagiCapture: High-Resolution Multi-Concept Portrait Customization](3/MagiCapture%20High-Resolution%20Multi-Concept%20Portrait%200fc0344786a947158e1c52fedfed8858)

고해상도 초상화 이미지를 다중 개념으로 맞춤화하는 새로운 기술을 소개합니다. 이 기술은 사진 스튜디오의 비용과 편집 과정을 대체할 수 있는 방법으로, 몇 가지 입력 및 참조 사진을 사용하여 특정 스타일의 이미지를 생성합니다. MagiCapture는 합성 프롬프트 학습과 고유한 손실 함수를 사용하여 보다 사실적인 이미지를 생성하는 데 기여합니다. 이 기술은 품질 테스트와 실제 애플리케이션에서 우수한 성능을 보이며, 피사체와 스타일을 모두 포착하는 고해상도 인물 이미지를 생성하는 새로운 방법으로 평가됩니다.

[Generative Image Dynamics](3/Generative%20Image%20Dynamics%20f0948d67712842aaa7d50974da90adca)

단일 이미지에서 자연스러운 동적 장면을 생성하는 새로운 방법을 제시합니다. 이 연구는 신경 스토캐스틱 모션 텍스처라는 개념을 도입하여 단일 RGB 이미지로부터 장기적인 동작 궤적을 예측합니다. 이 방법은 푸리에 변환을 활용하여 동작을 모델링하고, 이미지에 생동감을 부여하거나 사용자 상호작용에 반응하는 다양한 애플리케이션에 적용될 수 있습니다. 연구팀은 다양한 자연 장면, 특히 바람에 흔들리는 나무나 꽃에 초점을 맞추어 실제 동영상에서 모션 데이터를 학습했으며, 이를 통해 자연스러운 움직임을 예측하는 데 성공했습니다. 이 접근법은 단순한 이미지 생성이 아니라 움직임의 예측에 초점을 맞추고 있으며, 이를 통해 더 자연스러운 애니메이션을 제공합니다.

[PixArt-α: Fast Training of Diffusion Transformer for Photorealistic Text-to-Image Synthesis](3/PixArt-%CE%B1%20Fast%20Training%20of%20Diffusion%20Transformer%20fo%20245586a9b1ec42a38e7a684f0f00c14a)

새로운 텍스트-이미지 변환 기술인 PIXART-α를 소개합니다. 이 기술은 혁신적인 훈련 전략, 효율적인 변환기 구조, 고품질 훈련 데이터를 통해 이미지 생성의 품질을 유지하면서 계산 비용을 크게 줄입니다. PIXART-α는 기존 대비 낮은 비용과 환경 영향으로 상당한 이미지 품질을 제공합니다. 이 연구는 AI 및 그래픽 컴퓨팅 커뮤니티에 저렴하면서도 효과적인 텍스트-이미지 모델 개발 가능성을 제시합니다.

[RealFill: Reference-Driven Generation for Authentic Image Completion](3/RealFill%20Reference-Driven%20Generation%20for%20Authentic%201b51ecd66dd140159c2849caf2cfb8ee)

이미지 완성 분야에서 참조 기반의 접근 방식을 새롭게 제시합니다. 이 연구에서는 참조 이미지와 목표 이미지를 활용하여 누락된 부분을 사실적으로 채우는 새로운 기술인 'RealFill'을 개발했습니다. RealFill은 참조 이미지를 사용하여 인페인팅 모델을 미세 조정함으로써, 대상 이미지의 누락된 부분을 참조 이미지와 일치하게 만듭니다. 이는 특히 다양한 카메라 각도, 조명 조건, 피사체 포즈에서 참조 이미지와 대상 이미지 간의 상당한 차이를 처리하는 데 탁월한 능력을 보여줍니다. 이 연구는 참조 이미지를 사용하여 인페인팅과 아웃페인팅을 모두 수행하는 새로운 데이터 세트 'RealBench'를 도입함으로써, 복잡한 시나리오에서의 성능을 평가합니다. RealFill은 기존 방법들과 비교하여 높은 충실도와 품질의 결과물을 생성하는 것으로 나타났습니다.

[Kandinsky: an Improved Text-to-Image Synthesis with Image Prior and Latent Diffusion](3/Kandinsky%20an%20Improved%20Text-to-Image%20Synthesis%20with%20bcb54052d04c4980a8773a465b808bc7)

텍스트-이미지 생성 분야에서 새로운 잠재 확산 아키텍처, 칸딘스키를 소개합니다. 이 모델은 기존 모델들과 비교하여 향상된 이미지 생성 품질을 보여주며, 이미지 사전 확산과 잠재 확산을 결합한 첫 번째 시도입니다. 칸딘스키는 다양한 텍스트 인코더를 사용하며, 이미지 사전과 잠재 확산 프로세스를 통해 텍스트를 이미지로 변환하는 최첨단 방법을 제시합니다. 실험 결과, 칸딘스키는 경쟁 모델과 비교하여 뛰어난 FID 점수를 보여줍니다. 이 모델은 웹 기반 이미지 편집기와 텔레그램 봇을 통해 다양한 이미지 생성 기능을 제공하며, Apache 2.0 라이선스로 공개되어 있어 널리 접근할 수 있습니다.

[Idea2Img: Iterative Self-Refinement with GPT-4V(ision) for Automatic Image Design and Generation](3/Idea2Img%20Iterative%20Self-Refinement%20with%20GPT-4V(isi%201219daa957f34478a1a80225de401391)

이미지 디자인과 생성 과정을 자동화하는 새로운 프레임워크인 Idea2Img를 제시합니다. 이 프레임워크는 GPT-4V(ision), 즉 텍스트와 이미지를 모두 이해할 수 있는 대규모 다중 모드 모델을 사용하여 사용자의 아이디어를 시각적 이미지로 변환합니다. Idea2Img는 반복적인 자체 개선 과정을 통해 텍스트 프롬프트를 생성 및 수정하고, 최상의 초안 이미지를 선택하며, 피드백을 제공합니다. 이 과정은 인간의 반복적인 정제 과정을 모방하여 이미지 생성 과정을 더욱 효율적이고 직관적으로 만듭니다. 연구 결과는 Idea2Img가 다양한 디자인 시나리오에서 이미지 품질을 일관되게 개선하는 것을 보여줍니다.

[HyperHuman: Hyper-Realistic Human Generation with Latent Structural Diffusion](3/HyperHuman%20Hyper-Realistic%20Human%20Generation%20with%20L%2098c34514ca224f11852861eaec3d73b3)

하이퍼휴먼이라는 혁신적인 프레임워크를 소개합니다. 이는 텍스트 설명이나 포즈 같은 사용자 입력을 기반으로 매우 사실적인 인간 이미지를 생성합니다. 기존의 방법들이 학습 중 안정성 문제나 제한된 데이터 세트에서의 성능 문제를 겪는 반면, 하이퍼휴먼은 복잡한 인간 구조를 효과적으로 모델링합니다. 이 프레임워크는 잠재 구조 확산 모델과 구조 가이드 리파이너라는 두 가지 혁신적인 모듈을 포함하여, 인간의 외형과 내부 구조 사이의 상관관계를 포착합니다. 이를 위해 3억 4천만 개의 고품질 인체 이미지를 포함한 휴먼버스 데이터 세트가 사용되었습니다. 하이퍼휴먼은 제어 가능하고 사실적인 인간 이미지 생성을 가능하게 하며, 다양한 시나리오에서 우수한 성능을 보여주었습니다.

[DynVideo-E: Harnessing Dynamic NeRF for Large-Scale Motion- and View-Change Human-Centric Video Editing](3/DynVideo-E%20Harnessing%20Dynamic%20NeRF%20for%20Large-Scale%203988caa627aa4931b8e956fcca62b460)

동적 신경 방사 필드(NeRF)를 활용하여 복잡한 움직임과 시점 변화를 포함한 인간 중심의 비디오 편집에 관한 연구입니다. 이 방법은 레퍼런스 피사체 이미지와 배경 스타일 이미지를 기반으로 대규모 모션 및 뷰 변경을 필요로 하는 비디오를 일관성 있게 편집할 수 있게 합니다. 연구에서는 다양한 2D 표현을 사용한 비디오 편집을 이미지 편집으로 전환하는 시도도 포함되어 있으나, 이는 대규모 모션과 시점 변화가 있는 동영상에서는 어려움을 겪습니다. DynVideo-E는 동적 NeRF를 사용하여 비디오 데이터를 3D 공간으로 통합하고, 재구성 손실, 스코어 증류 샘플링(SDS), 해상도 개선, 스타일 전송과 같은 전략을 통합하여 3D 공간에서의 비디오 편집을 개선합니다. 이 연구는 동적인 사람 중심의 동영상에 대해 엄격한 테스트를 거쳐 높은 선호도와 일관된 고품질 비디오 편집 결과를 보여주었습니다.

[PhotoMaker: Customizing Realistic Human Photos via Stacked ID Embedding](3/PhotoMaker%20Customizing%20Realistic%20Human%20Photos%20via%20%20ca69f877b28d4b62be53b73473a4f4e7)

입력된 신분증 이미지를 기반으로 다양한 맞춤형 신분증 사진을 생성하는 새로운 방법을 제시합니다. 이 프레임워크는 텍스트 프롬프트를 사용하여 개인화된 ID 이미지를 생성하며, ID 정보를 잘 보존하면서 사실적인 사람 사진을 만들어냅니다. 포토메이커는 속성 변경, 예술 작품이나 오래된 사진의 인물을 현실로 불러오기, 신원 혼합 등 다양한 애플리케이션을 지원합니다. 이 방법은 입력된 ID 이미지의 특징을 유지하면서 새로운 이미지를 생성하는 데 효율적이고 다재다능한 접근 방식을 제공합니다.

[DreaMoving: A Human Dance Video Generation Framework based on Diffusion Models](3/DreaMoving%20A%20Human%20Dance%20Video%20Generation%20Framewor%209604cd5d06054ba991894d881d09f579)

인간 중심의 춤 동영상을 생성하는 새로운 프레임워크, DreaMoving을 소개합니다. 이 프레임워크는 기존의 텍스트-비디오 모델과는 달리, 인간의 춤 동작을 보다 정확하게 학습하고 재현하는 데 초점을 맞추고 있습니다. DreaMoving은 안정적인 확산 모델을 기반으로 하며, 노이즈 제거 U-넷, 비디오 제어 네트워크, 콘텐츠 가이드 등을 포함한 다중 네트워크 구조를 갖추고 있습니다. 이 프레임워크는 텍스트와 이미지 프롬프트를 결합하여 비디오 콘텐츠를 보다 세부적으로 제어할 수 있으며, 고품질의 개인화된 비디오 콘텐츠 생성 능력을 갖추고 있습니다.

[PALP: Prompt Aligned Personalization of Text-to-Image Models](3/PALP%20Prompt%20Aligned%20Personalization%20of%20Text-to-Ima%202fecfc54e39445608dd39267724330b8)

텍스트-이미지 모델을 개인화하는 새로운 방법을 제안합니다. 이 연구는 특정 피사체를 포함하면서도 프롬프트에 충실한 이미지를 생성하는 데 있어 기존 모델들의 한계를 극복하기 위해 소량의 개인 이미지 세트로 모델을 미세 조정하고 강력한 정규화를 적용하는 방법을 도입합니다. 저자들은 개인화와 프롬프트 정렬을 결합한 프레임워크를 소개하며, 이는 특정 프롬프트와 관련된 한계를 극복하고 텍스트-이미지 모델의 잠재력을 극대화하는 데 목표를 둡니다. 이 방법은 정성적, 정량적 평가를 통해 기존 기준선에 비해 우수한 결과를 보여주며, 다양한 소스에서 영감을 얻는 능력을 강조합니다.

[DREAM-Talk: Diffusion-based Realistic Emotional Audio-driven Method for Single Image Talking Face Generation](3/DREAM-Talk%20Diffusion-based%20Realistic%20Emotional%20Aud%20955ac22534c043ed9cc4f5dc175593f3)

DREAM-Talk은 오디오 시퀀스, 지정된 인물 이미지, 감정 스타일 예시를 기반으로 고품질 감정 표현과 사실적인 립싱크를 갖춘 말하는 얼굴 비디오를 생성하는 새로운 기술입니다. 현존하는 방법들은 감정 표현의 미묘함과 다양성을 포착하는 데 어려움을 겪는 반면, DREAM-Talk은 두 가지 주요 단계를 사용합니다: EmoDiff 모듈과 립 리파이닝. EmoDiff는 감정 조건부 확산 모델을 사용하여 역동적인 감정 표현을 포착하며, 립 리파이닝 단계는 정확한 립싱크를 보장합니다. 이 방법은 감정 표현, 입술 동기화, 신원 보존, 이미지 품질 면에서 기존 방식보다 우수한 결과를 제공합니다. DREAM-Talk의 성공은 가상현실, 엔터테인먼트, 커뮤니케이션 등 다양한 분야에 적용될 수 있는 실감나고 감정적으로 몰입할 수 있는 디지털 인간 표현을 가능하게 합니다.

[Wav2Lip: Accurately Lip-syncing Videos In The Wild](3/Wav2Lip%20Accurately%20Lip-syncing%20Videos%20In%20The%20Wild%204e4ebce8dcde452ebebc95f85fa32d5c)

동영상에서 자연스러운 립싱크를 생성하기 위한 새로운 접근법을 제시합니다. 이 모델은 다양한 환경과 화자에 대해 정확한 입술 움직임을 동기화할 수 있는 능력을 갖추고 있습니다. 이 연구는 립싱크의 정확성을 보장하기 위해 사전에 훈련된 립싱크 '전문가'를 활용하며, 이를 통해 기존 모델의 단점을 극복합니다. 또한, 이 논문은 현재 립싱크 평가 방식의 한계를 지적하고 새로운 평가 메트릭과 벤치마크를 제안합니다. Wav2Lip은 정량적 및 정성적 평가 모두에서 우수한 성능을 보이며, 이러한 기술의 활용 가능성을 넓혀 다양한 언어로 더빙된 콘텐츠나 CGI 캐릭터의 애니메이션 제작에 기여할 수 있습니다. 이 모델은 합성 콘텐츠의 표시를 통해 공정한 사용을 촉진하며, 조작된 동영상 콘텐츠의 오용을 탐지하고 방지하는 연구에 기여할 수 있습니다.

[DiffusionGPT : LLM-Driven Text-to-Image Generation System](3/DiffusionGPT%20LLM-Driven%20Text-to-Image%20Generation%20S%208dbc9a8f19bb4c72a0798ec81397f3ab)

DiffusionGPT는 대규모 언어 모델(LLM)을 활용하여 다양한 유형의 프롬프트 입력을 처리하고 특화된 도메인 전문가 모델과 결합하는 새로운 텍스트-이미지 생성 시스템입니다. 이 시스템은 프롬프트 기반, 명령어 기반, 영감 기반, 가설 기반의 다양한 입력 유형을 처리할 수 있으며, 우수한 품질의 이미지 결과물을 생성합니다. DiffusionGPT는 프롬프트 구문 분석, 모델 구축 및 검색, 모델 선택, 실행 생성의 네 단계로 구성된 워크플로를 통해 동작합니다. 이 시스템은 기존의 확산 모델들과 비교하여 사람이나 장면과 같은 범주에서 더 사실적이고 세밀한 이미지를 생성하며, 사용자 연구에서도 높은 선호도를 얻었습니다. DiffusionGPT는 LLM의 최적화와 모델 후보 확장을 통해 더 인상적인 결과를 얻고, 텍스트-이미지 작업을 넘어 다양한 작업에 적용할 계획입니다.

[Synthesizing Moving People with 3D Control](3/Synthesizing%20Moving%20People%20with%203D%20Control%20bcd98ea5b18a423cb5a83776346a1494)

임의의 사진을 기반으로 하여 해당 인물이 다른 사람의 행동을 모방할 수 있도록 하는 새로운 방법을 제안합니다. 이를 위해, 저자들은 '3DHM'이라는 두 단계 프레임워크를 개발했습니다. 첫 번째 단계에서는 모방자의 완전한 질감 맵을 생성하고, 두 번째 단계에서는 이 질감 맵을 사용하여 배우의 포즈를 모방하는 사실적인 인간 모방자를 렌더링합니다. 이 과정에서는 3D 자세 복구 모델인 '4DHumans'를 사용하여 배우의 동작을 추출하고, 이를 시간에 따라 추적합니다. 연구팀은 이 방법을 사용하여 다양한 동작과 외모를 갖는 인간 모방자의 사실적인 렌더링을 생성하며, 이는 디지털 이미지 처리 분야에서 중요한 기술적 발전을 나타냅니다.

[DreamMatcher: Appearance Matching Self-Attention for Semantically-Consistent Text-to-Image Personalization](3/DreamMatcher%20Appearance%20Matching%20Self-Attention%20fo%20cca45cdd2b3943a28766b5d04ab9e338)

DreamMatcher는 참조 이미지를 기반으로 개인화된 이미지를 생성하는 획기적인 기법으로, 외형 매칭 자기주의(Appearance Matching Self-Attention, AMA)를 통해 참조 이미지의 외형적 특성을 타겟 이미지에 정교하게 통합합니다. 이 과정에서 의미적 매칭과 일관성 모델링을 활용하여 참조 외형을 타겟 구조에 의미론적으로 일치시키며, 이는 세밀한 주제 세부사항의 향상을 가능하게 합니다. 또한, DreamMatcher는 추가적인 훈련이나 미세 조정 없이도 기존의 어떤 T2I 개인화 모델과 호환될 수 있는 범용성과 호환성을 갖추고 있어, 다양한 기존 모델과 쉽게 통합될 수 있는 강점을 지닙니다. 이러한 접근 방식은 참조 이미지의 외형을 보다 정확하고 다양하게 재현할 수 있는 새로운 가능성을 열어, T2I 개인화 기술의 새로운 지평을 엽니다.

[Magic-Me: Identity-Specific Video Customized Diffusion](3/Magic-Me%20Identity-Specific%20Video%20Customized%20Diffus%20881a0618373a4c2ab7d5613eeb3f3601)

Video Custom Diffusion (VCD) 프레임워크는 텍스트 기반의 비디오 생성에서 아이덴티티 특정과 시간적 일관성을 달성하기 위해 세 가지 핵심 기술을 도입합니다. 첫 번째로, 3D Gaussian Noise Prior를 사용하여 비디오 프레임 간의 일관성을 강화하고, 동작의 안정성을 높이면서 동작의 크기를 조절합니다. 이는 추론 단계에서만 적용되어 복잡한 재훈련 과정 없이도 효과를 발휘합니다. 두 번째로, ID 모듈은 확장된 ID 토큰을 통해 특정 아이덴티티의 시각적 특성을 정밀하게 포착하고 재현하는 데 초점을 맞춥니다. 이 모듈은 prompt-to-segmentation 기법을 활용하여 아이덴티티와 배경을 구분하고, 아이덴티티의 세부 사항을 더욱 명확히 합니다. 마지막으로, Face VCD와 Tiled VCD 기술은 각각 비디오 내 인물의 얼굴을 선명하게 하고 전반적인 해상도를 향상시키는 방식으로, 비디오의 품질과 아이덴티티 표현의 세밀함을 개선합니다. 이 세 가지 기술의 결합을 통해 VCD는 주어진 텍스트 프롬프트에 따라 아이덴티티를 정확하게 유지하면서 시간적으로 일관된 비디오를 생성할 수 있는 능력을 제공합니다.

[EMO: Emote Portrait Alive - Generating Expressive Portrait Videos with Audio2Video Diffusion Model under Weak Conditions](3/EMO%20Emote%20Portrait%20Alive%20-%20Generating%20Expressive%20P%202c48af6dead841b691c445bd2f2b16fa)

참조 이미지와 음성 데이터를 결합하여 표현력 있는 얼굴 표정과 머리 움직임을 가진 토킹 헤드 비디오를 생성하는 새로운 프레임워크를 제안합니다. 주목할 점은, 음성 입력과 시각적 모션 데이터를 통합적으로 처리하여, 입력 오디오의 길이에 관계없이 자연스러운 동작과 표정을 지닌 비디오를 생성할 수 있다는 점입니다.

[OOTDiffusion: Outfitting Fusion based Latent Diffusion for Controllable Virtual Try-on](3/OOTDiffusion%20Outfitting%20Fusion%20based%20Latent%20Diffus%20be848675f0b442c58fd0b86175d19705)

OOTDiffusion은 사전 훈련된 잠재 확산 모델을 활용하여 의류의 세부 특성을 효과적으로 학습하고, 착용 융합 과정을 통해 목표 인간 몸체에 자연스럽게 통합하는 새로운 이미지 기반 가상 시착 기술을 제시합니다. 이 방법은 독립적인 와핑 과정 없이도 정보 손실이나 특성 왜곡 없이 다양한 목표 인간 몸체 유형과 자세에 맞게 의류 특성을 부드럽게 적응시키는 데 성공함으로써, 현실성과 제어 가능성을 대폭 향상시킵니다.

[Boximator: Generating Rich and Controllable Motions for Video Synthesis](3/Boximator%20Generating%20Rich%20and%20Controllable%20Motions%2089c3b4c210114238acbeebb4586c0403)

Boximator는 비디오 내 객체의 움직임을 세밀하게 제어할 수 있는 박스 형태의 제약 조건을 도입하여, 기존 비디오 합성 모델의 한계를 극복합니다. 이 방법은 사용자가 직관적으로 객체의 위치와 경로를 조절할 수 있게 함으로써, 비디오 생성 과정에서 더 큰 유연성과 정밀도를 제공합니다.

[Champ: Controllable and Consistent Human Image Animation with 3D Parametric Guidance](3/Champ%20Controllable%20and%20Consistent%20Human%20Image%20Anim%20c5726e417ede407cbb3bdd2483fe9d06)

"Champ" 논문은 SMPL 3D 파라메트릭 인간 모델과 잠재 확산 모델을 통합하여, 인간 이미지 애니메이션의 정확도와 현실성을 크게 향상시키는 새로운 방법을 제시합니다. 이 방법은 다층 정보를 활용하여 인간의 동작과 형태에 대한 상세한 가이드를 제공하고, 실제 인간의 움직임과 형태를 보다 현실적으로 포착할 수 있는 차별점을 가집니다.

[MaPa: Text-driven Photorealistic Material Painting for 3D Shapes](3/MaPa%20Text-driven%20Photorealistic%20Material%20Painting%20%20ea0ed45292d343daa1e7f1d80001c701)

본 논문은 텍스트 설명을 기반으로 3D 메시에 대해 사실적이고 조명에 따라 변화하는 고해상도 재료를 생성할 수 있는 새로운 프레임워크를 제안합니다. 이 방법은 사전 훈련된 2D 확산 모델을 사용하여 세그먼트별 절차적 재료 그래프를 최적화하고, 이를 통해 사용자가 쉽게 재료를 편집할 수 있도록 지원합니다.

[Stylus: Automatic Adapter Selection for Diffusion Models](3/Stylus%20Automatic%20Adapter%20Selection%20for%20Diffusion%20M%2051190521010f4a9bbcd48b8193071050)

이 논문은 사용자 프롬프트에 기반하여 관련성 높은 어댑터를 자동으로 선택하고 조합하는 새로운 시스템인 Stylus를 제안합니다. Stylus는 시각적 충실도, 텍스트 정렬, 이미지 다양성을 향상시키며, 기존의 이미지 생성 모델과 비교하여 더욱 효과적으로 다양한 이미지를 생성합니다.

[Paint by Inpaint: Learning to Add Image Objects by Removing Them First](3/Paint%20by%20Inpaint%20Learning%20to%20Add%20Image%20Objects%20by%20%20447cb8bd63934041820bfb79b48d67e5)

이 논문에서 제안하는 방법론은 기존의 이미지에서 객체를 제거하는 인페인팅 과정을 역으로 활용하여 새로운 객체를 추가하는 'Paint by Inpaint' 프레임워크를 소개합니다. 즉, 이미지에서 먼저 객체를 제거한 후, 이를 기반으로 새로운 객체를 추가하는 방식을 사용합니다. 이 역방향 접근 방식은 추가된 객체가 자연스러워 보이게 하는 데 중점을 두며, 결과적으로 편집된 이미지의 품질을 향상시키는 것을 목표로 합니다.