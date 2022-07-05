# U-net

# Introduction

- convolutional networks는 오래 동안 존재해 왔으며, 그것들은 훈련 세트의 이미지 크기와 네트워크의 사이즈를 고려하기 때문에 제한이 있었음
- convolutional networks에서는 결과 이미지가 단일 분류인 경우가 많지만, 많은 이미지 업무에서는(특히 생물학 이미지 처리) 지역적 특성을 원하는 경우가 많음 → 클래스 label를 각 픽셀에 할당함

# U-net Architecture
![Untitled](https://user-images.githubusercontent.com/80506107/177286522-74cd8385-f093-4bb7-be27-c535a20f014e.png)

- 파란 박스는 피처맵이며 각 박스의 채널 수는 그 위에 작성 되어 있음
- x, y, z 크기는 박스 왼쪽 구석에 작성되어 있으며, 하얀 박스는 복사된 feature map임
- 각각 화살표는 서로 다른 기능을 함
- fully convolutional network를 하고 있음
- 이 아키텍처를 수정하고 확장하여 매우 적은 수의 이미지로 작동하고 세분화함
- 주요 아이디어는 contracting network에서 pooling 연산자가 upsampling 연산자로 대체 되어 작동하는 것임

# Overlap-Tite Input

![Untitled 1](https://user-images.githubusercontent.com/80506107/177286554-2fe19fa4-0ad6-4b0b-8f11-f87e61b8caab.png)

- Fully Convolutional Network 구조의 특성상 입력 이미지의 크기에 제약이 없음
- 따라서 이미지 전체를 사용하는 대신 overlap-tite 전략을 사용
- 위와 같이 이미지를 타일(patch)로 나누어서 입력함
- 파란 영역의 이미지를 입력하면 노란 영역의 segmentation 결과를 얻음
- 다음 tile에 대한 segmentation을 얻기 위해서는 이전 입력의 일부분이 포함되므로 Overlap-Tite 전략이라 함
- 이미지의 경계 부분에는 0이나 임의의 패딩값을 사용하는 대신 이미지 경계 부분의 미러링을 이용한 Extrapolation 기법을 사용

# Contracting Path(수축 경로)

![Untitled 2](https://user-images.githubusercontent.com/80506107/177286578-2cb7d4d2-74aa-4aa9-8816-8ae817593e84.png)

- 입력 이미지의 Context 포착을 목적으로 구성 되어 있으며
- FCNs처럼 VGG-based Architecture 기반임
- 각 스텝마다 3x3 convolution과 2x2 max-pooling을 진행하므로 feature map이 줄어들면서 channel 수가 늘어남
- Convolution에는 ReLU 연산이 사용됨

# Expanding Path(확장 경로)


![Untitled 3](https://user-images.githubusercontent.com/80506107/177286611-9d4e353d-efdc-4eb9-be54-93d44ee9056f.png)

- Contracting Path와 반대로 특징맵을 확장하며 각 스텝마다 2x2 Up-convolution을 수행
- 3x3 convolution을 두 차례씩 반복(ReLU포함)
- 2x2 max-pooling(stride : 2)
- Up-conv를 통해 Up-sampling마다 채널의 수를 반으로 줄임
- Up-conv된 특징맵은 Contracting path의 테두리가 Cropped된 특징맵과 concatenation 함
- 마지막 레이어에 1x1 convolution 연산
- 최종 출력인 segmentation map의 크기는 Input Image 크기보다 작음  Convolution 연산에서 패딩을 사용하지 않기 때문

# Skip Architecture

![Untitled 4](https://user-images.githubusercontent.com/80506107/177286639-26a06562-80a6-4c55-97fe-5a2e3f058179.png)

- 각 Expanding Step마다 Up-conv 된 특징맵은 Contracting path의 cropped된 특징맵과 concatenation 함

# Training

- 네트워크의 출력 값은 픽셀 단위의 softmax로 예측
- 최종 특징 맵(채널 k)에 대한 픽셀 x의 예측 값은 다음과 같이 계산됨

![Untitled 5](https://user-images.githubusercontent.com/80506107/177286658-9efe2d86-d716-43b8-8580-42be5a1609e8.png)

- Loss function은 Cross-entorpy 함수가 사용되며 Touching Cells 분리를 고려하기 위해 Weight map loss가 포함됨

![Untitled 6](https://user-images.githubusercontent.com/80506107/177286678-1e77eb03-32b1-4d45-a6bd-661b5f38f7c9.png)

# 참고

[U-Net 논문 리뷰 - U-Net: Convolutional Networks for Biomedical Image Segmentation](https://medium.com/@msmapark2/u-net-%EB%85%BC%EB%AC%B8-%EB%A6%AC%EB%B7%B0-u-net-convolutional-networks-for-biomedical-image-segmentation-456d6901b28a)

[U-net](https://arxiv.org/pdf/1505.04597.pdf)
