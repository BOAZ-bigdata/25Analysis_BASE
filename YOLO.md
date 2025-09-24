## Background & Related Work
### Object Detection 
- **Classification (What)**
	- 한 이미지에 있는 물체가 무엇인지 알아내는 것
- **Localization (Where)**
	- 이미지가 어디에 있는지 찾는 것

### YOLO 탄생 아이디어
#### 기존 접근법(DPM, R-CNN)의 문제점 
- Pipeline이 복잡 -> 최적화의 어려움, 느림
- 제한된 범위에서만 object classification이 이루어짐 -> 전역적인 정보를 볼 수 없음
#### YOLO의 차별점
- 하나의 Convolutional network를 사용해 **간단한 구조**를 가짐.
- 예측 시 **전역적인 정보**를 사용함.
- 객체에 대한 generalizable representation을 학습.

## Method
### Unified Detection
1. input image를 $S \times S$ grid로 나눔
2. 각 grid가 B개의 bounding box와 각 box의 confidence score를 예측
3. 각 grid cell이 C개의 conditional class probability를 예측
4. conditional class probability와 confidence prediction을 곱해, 각 box에 대한 **class-specific confidence score**를 구함.

### Network
GoogLeNet에 영감을 받아 설계.
- 24 convolutional layer + 2 fully connected layer
- 1 x 1 reduction layer, 3 x 3 convolutional layer

### Training
#### Pretraining
- ImageNet 1000-class competition dataset으로 훈련.
- 224 x 224 resolution
#### Detection으로의 전환
- 4개의 convolutional layer와 2개의 fc layer 추가
- 448 x 448 resolution (fine-grained visual information을 위해)
- box coordinate를 0과 1 사이 값으로 정규화해 표현
- Leaky ReLU 사용
### Multi-part loss function
- object가 없는 cell의 수가 더 많음. -> object를 가진 cell의 영향력이 감소하여 학습이 약화.
		-> 별도의 파라미터를 통해 object가 있을 때의 loss를 증가, 없을 때를 감소시킴.
- 박스 size가 detection 성능에 영향을 주기 때문에 이 또한 학습에 반영해야 함.
		-> bounding box의 width와 height의 square root 또한 예측.
### Inference
- Single Forward Pass로 진행
- 같은 객체에 대해 중복 박스가 생성되는 문제 -> NMS를 통해 해결.

## Limitation
- Grid 제약 : 한 셀 당 박스와 클래스의 개수가 제약됨.
- 작은 물체 탐지 취약 : 다운샘플링 + 손실함수 한계로 localiztion error 증가
- Aspect ratio 일반화 어려움
- Loss-Metric 불일치
- NMS의 한계 : 근접 객체 제거 위험

## Experiments
### Comparison
- 다른 탐지기들에 비해 월등이 빠르면서, 안정적인 정확도를 기록 
### Error Analysis
- YOLO와 Fast R-CNN의 상위 5개 예측을 5개의 카테고리로 나누어 분류한 결과, 각 분류기의 특징을 파악할 수 있었음.
	- YOLO : Localization Error 높음
	- Fast R-CNN : Background Error 높음
-> 서로의 단점을 보완해주어, 함께 사용 시 성능 향상을 보임.
### Generalizability
- 다른 모델에 비해 다양한 데이터셋 (VOC, Picasso)에서도 모두 잘 작동함 : 픽셀 수준 차이가 커도 구조적, 맥락적 특징을 잘 잡아냄.
-> 일반화 성능 우수

## Conclusion
객체 탐지 패러다임을 바꾼 모델로, 아래와 같은 기여를 했다고 볼 수 있음.
- **End-to-End Unified Model** : 하나의 네트워크로 모든 과정을 수행.
- **Fastest Real-Time Detector** : 빠르면서 우수한 정확도
- **Strong Generalization** : 새로운 도메인에도 강인 -> 실제 응용 가능

