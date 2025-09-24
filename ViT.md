## Introduction
- NLP에서 Transformer가 강한 long-range dependency를 보여주고, 실제로 대규모 corpus를 통해 학습한 모델의 지식을 전이해 다양한 task에 적용 가능한 엄청난 성공을 보임.
- 따라서 CV에서도 CNN에서 발전하여, Attention을 결합하고자 하였지만 하드웨어 가속화 단계에서 보편화에 실패함.
-> Transformer를 Image에 직접적으로 적용시키는 연구를 진행.
## Research Objectives
1. ImageNet보다도 큰 dataset에서 Transformer가 CNN을 능가할 수 있는지 확인
2. Transformer를 바로 CV 분야에 적용
3. Medium-Resolution 이미지에도 모델을 바로 적용 : 기존의 Attention을 응용한 연구가 저해상도 이미지로 제한되었던 점을 극복
## Method
### Architecture
- 이미지를 1차원의 벡터(h x w x c)로 만든 뒤, **Linear Projection**을 통해 D차원으로 변환
- **CLS token** 추가 : BERT의 그것과 유사.
- **Position Embedding** : patch간 상대적 위치 정보를 알 수 없는 1차원 벡터의 한계 극복
- **Transformer Encoder** : LN + Multi-Head Self-Attention -> Residual Connection, LN -> MLP (GELU) + Residual Connection의 구조.
### Fine-Tuning + etc.
- Pre-trained model에서 Prediction Head 제거 후 새 FC Layer 추가 -> downstream task에 맞추어 학습
- Pre-training보다 높은 해상도로 Fine-tuning 시 성능 증가.
- 패치 수가 변할 시 Pre-trained Position Embedding 불일치 -> 2D Interpolation으로 해결.
## Experiment
### Comparison to SOTA
-  기존 SOTA 대비 **높은 정확도** 기록 및 **학습 연산량 감소**
### Position Embedding
- 적용하지 않을 시 성능 저하, 방식에 따른 차이 (1D, 2D, etc.)는 크지 않음. -> 1D로도 충분.
### Data Requirement
- Data가 많을 수록 성능 증가
- 모델이 복잡할 수록 많은 데이터 요구
### Self-Supervision
- 지도 학습에 비해서는 약 4% 낮은 정확도를 보이지만, scratch보다는 약 3% 높은 정확도를 보임. -> 아직 지도 학습을 대체할 수는 없지만, 성능 향상의 가능성을 보여줌.
## Conclusion
CV에 Transformer를 접목하여 SOTA를 달성한 연구이며, 아래와 같은 기여를 함.
- 기존의 Self-Attention을 접목하였던 CV 연구들의 한계를 극복.
- SOTA를 달성하는 우수한 성능을 보였고, 사전 학습 비용 또한 저렴함.
