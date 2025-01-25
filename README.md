# AYO-IMAGE: 스프라이트 애니메이션 프레임 생성

AYO-IMAGE는 픽셀 아트 디자이너를 돕기 위해 스프라이트 애니메이션을 자동으로 생성하는 딥러닝 프로젝트입니다. 스프라이트의 첫 번째 프레임을 입력하면 모델이 나머지 프레임을 예측하고 생성하여 아티스트의 작업 시간을 절약하고 생산성을 높일 수 있습니다.

## 목차

- [프로젝트 개요](#프로젝트-개요)
- [주요 기능](#주요-기능)
- [모델 아키텍처](#모델-아키텍처)
- [설치 방법](#설치-방법)
- [사용법](#사용법)
- [결과](#결과)
- [기여](#기여)
- [라이선스](#라이선스)

## 프로젝트 개요

AYO-IMAGE는 Conditional U-Net과 FrameCascadeGAN의 두 가지 주요 아키텍처를 사용하여 스프라이트 애니메이션을 생성합니다. 두 모델 모두 실시간 서비스로 배포되었으며, 사용자들이 자신의 요구에 맞는 모델을 선택할 수 있도록 합니다.

생성된 프레임은 입력 프레임의 스타일과 일관성을 유지하며, 다양한 종류의 동작에 적응할 수 있어 스프라이트 애니메이션 제작 과정을 효율적으로 만듭니다.

## 주요 기능

- **입력**: 스프라이트 애니메이션의 첫 번째 프레임.
- **출력**: 전체 애니메이션 시퀀스 예측.
- **모델**: Conditional U-Net과 FrameCascadeGAN.
- **손실 함수**: 향상된 이미지 품질을 위한 `mse_rgb` 커스텀 손실 함수.

## 모델 아키텍처

AYO-IMAGE는 스프라이트 애니메이션 생성을 위한 두 가지 주요 아키텍처를 제공합니다.

### 1. Conditional U-Net

Conditional U-Net 모델은 첫 번째 입력 프레임을 조건으로 하여 프레임을 생성합니다. 스킵 연결과 합성곱 레이어를 활용해 시퀀스의 각 프레임을 생성하며, 세부 사항을 유지하고 프레임 간에 매끄러운 전환을 보장합니다.

- **입력**: 애니메이션의 첫 번째 프레임.
- **출력**: 전체 애니메이션 시퀀스.
- **아키텍처**: U-Net 기반 네트워크로, 각 프레임을 생성하기 위한 조건부 입력을 사용합니다.

### 2. FrameCascadeGAN

FrameCascadeGAN은 일련의 GAN 모델로 구성되어 있으며, 각 GAN은 애니메이션 시퀀스에서 특정 프레임을 생성하는 역할을 합니다. 첫 번째 입력 프레임을 조건으로 하여 복잡한 동작을 생성할 수 있으며, 긴 시퀀스 처리에 효과적입니다.

- **입력**: 애니메이션의 첫 번째 프레임.
- **출력**: 예측된 프레임 시퀀스.
- **아키텍처**: 각 프레임 생성을 담당하는 조건부 GAN의 시리즈.

### 실시간 서비스

두 아키텍처는 모두 실시간 서비스로 배포되어 사용자들이:

- 스프라이트의 첫 번째 프레임을 업로드할 수 있습니다.
- Conditional U-Net 또는 FrameCascadeGAN 중에서 선택하여 전체 애니메이션을 생성할 수 있습니다.
- 결과 애니메이션을 다운로드할 수 있습니다.

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

AYO-IMAGE 모델은 하나의 입력 프레임에서 매끄럽고 일관된 스프라이트 애니메이션을 성공적으로 생성합니다. 아래는 입력과 생성된 프레임의 예시입니다:

**입력 프레임:**

TBU
![Input Frame](path-to-input-frame)

**생성된 프레임:**

TBU
![Generated Frames](path-to-generated-frames)

## 기여

기여는 언제나 환영합니다! 이 프로젝트에 기여하려면 다음 단계를 따라주세요:

1. 리포지토리를 포크하세요.
2. 새로운 기능 또는 버그 수정을 위한 브랜치를 만드세요.
3. 변경 사항을 적용하고 풀 리퀘스트를 제출하세요.

## 라이선스

이 프로젝트는 MIT 라이선스를 따릅니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

---

# AYO-IMAGE: Sprite Animation Frame Generation

AYO-IMAGE is a deep learning project designed to assist pixel art designers by automating the process of generating sprite animations. Given the first frame of a sprite's motion, the model predicts and generates subsequent frames, saving time and increasing productivity for artists.

## Table of Contents

- [Project Overview](#project-overview)
- [Features](#features)
- [Model Architectures](#model-architectures)
- [Installation](#installation)
- [Usage](#usage)
- [Results](#results)
- [Contributing](#contributing)
- [License](#license)

## Project Overview

AYO-IMAGE utilizes two main architectures—Conditional U-Net and FrameCascadeGAN—to generate smooth, high-quality sprite animations from a single input frame. Both models were deployed as live services, giving users options to select the model that best fits their artistic and technical needs.

The generated frames are consistent with the input frame's style and can adapt to various types of motions, making the process of sprite animation more efficient.

## Features

- **Input**: First frame of the sprite animation.
- **Output**: Predicted sequence of frames for the entire animation.
- **Model**: Conditional U-Net and FrameCascadeGAN.
- **Loss Function**: Custom `mse_rgb` loss function for improved image quality.

## Model Architectures

AYO-IMAGE offers two main architectures for sprite animation generation:

### 1. Conditional U-Net

The Conditional U-Net model is designed for frame generation by conditioning on the first input frame. It utilizes skip connections and convolutional layers to generate the sequence of frames, maintaining a high level of detail and ensuring smooth transitions between frames.

- **Input**: The first frame of the animation.
- **Output**: Entire animation sequence.
- **Architecture**: A U-Net-based network with conditional inputs for generating each frame in the sequence.

### 2. FrameCascadeGAN

FrameCascadeGAN is a cascade of GAN models, where each GAN is responsible for generating one specific frame in the sequence, conditioned on the first input frame. It provides more flexibility in generating complex movements and can handle longer sequences effectively.

- **Input**: The first frame of the animation.
- **Output**: Predicted frames in sequence.
- **Architecture**: A series of conditional GANs, each tasked with generating one frame in the motion sequence.

### Live Service

Both architectures have been deployed as live services, allowing users to:

- Upload a sprite's first frame.
- Choose between Conditional U-Net or FrameCascadeGAN to generate the full animation.
- Download the resulting animated sequence.

## Installation

### Prerequisites

Make sure you have the following installed:
- Python 3.8 or later
- PyTorch
- CUDA (optional, for GPU support)
- Other dependencies from `requirements.txt`

### Clone the Repository

```bash
git clone https://github.com/your-username/ayo-image.git
cd ayo-image
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

## Usage

1. Prepare your sprite image data in the `/data` folder. Ensure the first frame is named `frame_0.png`.
2. Run the model with the following command:

```bash
python generate_frames.py --model unet --input ./data/frame_0.png --output ./results/animation.gif
```

Or for FrameCascadeGAN:

```bash
python generate_frames.py --model framecascadegan --input ./data/frame_0.png --output ./results/animation.gif
```

### Parameters

- `--model`: Choose between `unet` or `framecascadegan`.
- `--input`: Path to the first frame image.
- `--output`: Path to save the generated animation.
- Additional hyperparameters (e.g., number of frames, GAN settings) can be tweaked in the `config.yaml` file.

## Results

The AYO-IMAGE models successfully generate smooth and coherent sprite animations from a single input frame. Below is an example of the input and the corresponding generated frames:

**Input Frame:**

TBU
![Input Frame](path-to-input-frame)

**Generated Frames:**

TBU
![Generated Frames](path-to-generated-frames)

## Contributing

Contributions are welcome! If you'd like to contribute to this project, please follow the steps below:

1. Fork the repository.
2. Create a new branch for your feature or bug fix.
3. Make your changes and submit a pull request.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for more information.
