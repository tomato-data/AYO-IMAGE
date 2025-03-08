### AYO-IMAGE: 스프라이트 애니메이션 프레임 생성

AYO-IMAGE는 픽셀 아트 디자이너를 위한 도구로, 스프라이트 애니메이션의 첫 번째 프레임을 입력받아 나머지 프레임을 자동으로 생성해주는 딥러닝 프로젝트입니다. 이 프로젝트는 디자이너의 작업 시간을 절약하고, 애니메이션 제작의 효율성을 높이기 위해 개발되었습니다.

## 목차

- [프로젝트 개요](#프로젝트-개요)
- [주요 기여](#주요-기여)
- [설치 방법](#설치-방법)
- [사용법](#사용법)
- [결과](#결과)
- [기여 방법](#기여-방법)
- [라이선스](#라이선스)

## 프로젝트 개요

AYO-IMAGE는 두 가지 주요 아키텍처인 Conditional U-Net과 FrameCascadeGAN을 사용하여 스프라이트 애니메이션을 생성합니다. 첫 번째 프레임을 입력으로 받고, 나머지 애니메이션 프레임들을 예측하여 자동으로 생성하는 방식입니다.

## 주요 기여

이 프로젝트에서 저는 여러 가지 실험을 통해 성능을 개선하는 데 중점을 두었습니다. 주요 기여는 다음과 같습니다:

1. **모델 실험 및 최적화**:
   - CNN, DNN, CNN-DNN 혼합 아키텍처를 사용하여 여러 모델을 실험하였습니다.
   - 초기 autoencoder 모델을 활용하여 스프라이트 생성 가능성을 확인했으나, 노이즈와 RGB 처리 문제를 해결하기 위해 추가 최적화가 필요하다고 판단했습니다.
   - Conditional U-Net은 첫 번째 프레임과 positional embedding을 입력으로 받아 이후의 프레임을 예측하는 방식으로, positional embedding 값에 따라 몇 번째 프레임이 나오는지를 정할 수 있습니다.
   - FrameCascadeGAN은 각 프레임을 독립적으로 생성하는 여러 개의 GAN으로 구성되어 복잡한 애니메이션 시퀀스에 적합한 방식입니다.

2. **MSE_RGB 손실 함수 도입**:
   - 저는 모델의 성능을 개선하기 위해 `mse_rgb`라는 커스텀 손실 함수를 제안하고 구현했습니다. 이 함수는 Keras의 backend 모듈인 K를 사용해 구현되었습니다. 기존 mse는 전체 matrix를 평균낸 scalar 값을 역전파에 사용하는데 반해, mse_rgb는 channel 값으로만 평균낸 input image의 너비 * 높이의 크기와 같은 matrix(pixel-wise)를 역전파에 사용합니다.
   - 이 함수를 통해 출력 이미지 품질을 향상시켰고, 모델의 손실이 10^-1 단위에서 10^-2 단위로 10% 미만으로 줄어든 것을 확인하였습니다.

3. **팀 내 협업 및 커뮤니케이션**:
   - 모델의 성능과 개선 사항을 시각화한 다이어그램 제작을 통해 팀원 간의 원활한 소통을 도왔습니다.
   - 실험 결과와 성능 분석을 문서화하고 프레젠테이션 자료를 작성하여 팀 프로젝트의 이해도를 높였습니다.

3. **모델 선택 및 배포**:
   - Conditional U-Net과 FrameCascadeGAN 모델을 비교하고 성능을 분석하여, 각 모델의 장단점을 사용자에게 제공했습니다.
   - 프로젝트는 현재 실시간 서비스로 운영되고 있지는 않지만, 향후 사용자가 선택할 수 있는 맞춤형 애니메이션 생성 서비스를 염두에 두고 모델 배포 전략을 설계하는 데 기여했습니다.

## 설치 방법

### 필수 사항

다음이 설치되어 있는지 확인하세요:
- Python 3.8 이상
- PyTorch
- CUDA (선택사항, GPU 지원용)
- `requirements.txt`의 기타 의존성

### 리포지토리 클론

```bash
git clone https://github.com/your-username/ayo-image.git
cd ayo-image
```

### 의존성 설치

```bash
pip install -r requirements.txt
```

## 사용법

1. `/data` 폴더에 스프라이트 이미지 데이터를 준비하세요. 첫 번째 프레임은 `frame_0.png`로 이름을 지정해야 합니다.
2. 모델을 실행하려면 다음 명령을 사용하세요:

```bash
python generate_frames.py --model unet --input ./data/frame_0.png --output ./results/animation.gif
```

FrameCascadeGAN을 사용하려면:

```bash
python generate_frames.py --model framecascadegan --input ./data/frame_0.png --output ./results/animation.gif
```

### 매개변수

- `--model`: `unet` 또는 `framecascadegan` 중 선택.
- `--input`: 첫 번째 프레임 이미지의 경로.
- `--output`: 생성된 애니메이션을 저장할 경로.
- 기타 하이퍼파라미터는 `config.yaml` 파일에서 조정할 수 있습니다.

## 결과

AYO-IMAGE는 하나의 입력 프레임에서 매끄럽고 일관된 스프라이트 애니메이션을 성공적으로 생성합니다. 아래는 입력과 생성된 프레임의 예시입니다:

**입력 프레임:**

TBU
![Input Frame](path-to-input-frame)

**생성된 프레임:**

TBU
![Generated Frames](path-to-generated-frames)

## 기여 방법

기여는 언제나 환영합니다! 이 프로젝트에 기여하려면 다음 단계를 따라주세요:

1. 리포지토리를 포크하세요.
2. 새로운 기능 또는 버그 수정을 위한 브랜치를 만드세요.
3. 변경 사항을 적용하고 풀 리퀘스트를 제출하세요.
