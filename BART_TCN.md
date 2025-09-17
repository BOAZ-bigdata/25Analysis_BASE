# BART: Denoising Sequence-to-Sequence Pre-training  


## 개요
- **BART**는 **시퀀스-투-시퀀스(Seq2Seq) 사전학습 모델**  
- 텍스트를 **노이즈로 변형 → 원래 문장 복원**하는 방식으로 학습  
- Transformer 기반 구조  
  - **양방향 인코더 (BERT와 유사)**  
  - **좌→우 디코더 (GPT와 유사)**  

## 주요 아이디어
- **노이징 기법**
  - 문장 순서 무작위 섞기  
  - **Span in-filling**: 연속된 텍스트를 마스크 토큰으로 대체  
- 기존 BERT, GPT를 일반화하는 구조  

## 성능
- **생성 태스크**(요약, 대화, 질의응답 등)에서 특히 효과적  
- **GLUE, SQuAD** → RoBERTa 수준 성능 달성  
- **요약/대화/QA**에서 새로운 SOTA 기록 (ROUGE 최대 +6)  
- **번역**에서도 BLEU +1.1 향상  

## 의의
- 단순한 구조지만 **범용성** 높음  
- 다양한 기존 사전학습 스킴을 **하나의 프레임워크**로 통합해 실험 가능  
- **분해(ablations)** 실험으로 어떤 요인이 성능에 기여하는지 분석  

# Temporal Convolutional Networks (TCN) 발표 요약

## 1. Introduction
- 기존: 시퀀스 모델링 = RNN (LSTM/GRU)  
- 최근: CNN도 오디오 합성, 번역, 언어모델 등에서 SOTA 달성  
- 연구 질문: RNN만의 영역인가, CNN도 보편적으로 강력한가?  

## 2. Background (핵심 포인트)
1. CNN도 오래 전부터 시퀀스에 활용, 최근 성과 뛰어남  
2. RNN은 강력하지만 학습이 어려워 LSTM/GRU 필요  
3. 내부 비교 결과 LSTM이 여전히 표준  
4. CNN-RNN 하이브리드도 있으나, 본 논문은 **순수 CNN vs RNN** 비교  
5. 기존 연구는 classification 중심 → 본 논문은 **sequence generation**에 초점  

## 3. TCN 구조
- **구성 요소**  
  - 1D Fully Convolutional Network (길이 보존)  
  - **Causal convolution** (미래 정보 누출 방지)  
  - **Dilated convolution** (지수적으로 수용영역 확장)  
  - **Residual connection** (안정적 학습)  
- 특징: 단순하면서도 긴 메모리 처리 가능  

## 4. Sequence Modeling Tasks
- **Synthetic tasks**  
  - Adding Problem, SeqMNIST, Copy Memory → TCN이 압도적 성능  
- **Real-world tasks**  
  - 음악 예측 (JSB/Nottingham): TCN ≈ LSTM/GRU  
  - 언어모델링  
    - Word-level PTB: LSTM > TCN (예외 케이스)  
    - WikiText-103, LAMBADA, text8: TCN ≫ LSTM/GRU  

## 5. Experiments
- 비교: TCN vs RNN (LSTM, GRU) with 비슷한 파라미터 수  
- 공정성: gating/skip 등 추가 트릭 사용하지 않음  
- 결과:  
  - Synthetic: TCN 빠르고 정확  
  - Real-world: 대부분 TCN 우세, PTB(word-level)만 LSTM 우세  
  - Long memory: TCN은 긴 시퀀스에서도 안정적, RNN은 급격히 성능 하락  

## 6. Conclusion
- **주요 결과**: TCN이 RNN 대부분을 능가, 특히 긴 문맥에서 탁월  
- RNN의 "무한 기억"은 실제로 거의 발휘되지 않음  
- TCN은 단순하면서 강력 → 시퀀스 모델링의 새로운 출발점 가능성  