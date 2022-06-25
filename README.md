# Detection_Polyps_using_Deep_Learning


## Motivation
- 식생활의 서구화로 인하여 대장암의 발병률이 증가하고 있고 대다수의 대장암이 용종에서 대장암으로 변이되는 과정을 거침
- 용종을 조기에 발견하여 제거하는 것이 대장암으로 인한 사망률을 76 ~ 90% 낮춤
- 하지만 국내 평균 용종 간과율은 14.1 ~ 26.6%이며 대장 내시경 시술자에 따라 간과율의 차이가 존재
- 시술자 간의 간과율의 편차를 줄이고 전문의 진단에 도움을 주고자 하는 것이 목적


## Data introduction
- Kvasir-SEG Dataset(https://datasets.simula.no/kvasir-seg/)
- 용종 이미지와 마스킹 이미지 각각 1000장 / 용종 검출 비디오 샘플 3개


## Experiment Method
- Image segmentation
  - 미리 정해진 클래스의 집합을 이용하여 주어진 이미지의 각 픽셀을 특정 클래스로 분류하는 작업
- Image Augmentation
  - 데이터의 양을 늘리기 위해 원본에 각종 변환을 적용하여 개수를 증강시키는 기법

## Model
- U-net
  - 네트워크 구성 형태가 U자형 모양이라 U-net이라는 이름이 붙여졌으며 대칭 구조를 띔

 ![image](https://user-images.githubusercontent.com/80506107/175772754-b8cd1b1e-6140-4532-9004-7a4ed75b278a.png)

- DeeplabV3
  - ResNet을 기본적인 특징 추출기로 사용하여 마지막 블록에서 Atrous Convolution을 사용해서 다양한 특징들의 크기를 뽑아낼 수 있도록 함
 
![image](https://user-images.githubusercontent.com/80506107/175772765-37448bc9-525e-4d7a-8d3e-030a173366ad.png)

- 성능 평가 지표로는 MIOU를 사용

## Model result
![image](https://user-images.githubusercontent.com/80506107/175772794-e70bd30e-5c9f-49db-909c-0e931ec7ac92.png)
- Unet의 MIOU가 0.02 정도 높은 것을 알 수 있음

## Conclusion
- U-net의 성능이 실시간 용종 검출에 유의한 성능을 보임
- 초기에 더 많은 용종을 자동으로 감지하는 것이 대장암의 예방과 생존 모두를 개선하는 데 중요한 역할을 보임
- 용종뿐만 아니라 다른 질병에 해당하는 마스킹데이터가 존재한다면 다른 질병도 검출 가능

## Reference
https://fuchsia-runner-4af.notion.site/9aeca402487d47fdb54e9b9bf1939fd9?v=2effe84b879f44e2a2192a1c80c3e783


2021 - 12 빅데이터공학과 학술제 우수상
