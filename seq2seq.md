Week 02 Assignment
==

LSTM 기반 Seq2Seq 모델에서 디코딩할 때 사용하는 Beam Search 동작 방식에 대해서 설명.
--

Best First Search에서 기억 노드의 수를 제한하며 실행하는 search algorithm
- 몇개 까지 기억하게 할지가 beam size가 된다. 
- 최적해를 보장하진 않지만 충분히 좋은 값을 return하고 memory efficient 하다. 

Seq2Seq with LSTM 모델은 Attention이 없던 시절 제안된 구조임. 기본 Seq2Seq 모델의 한계와 이후 Attention 메커니즘이 이 한계를 어떻게 보완했는지 설명.
--

Seq2Seq의 문제는 크게 2가지가 있다.
1. 정보 손실: Encoder에서 fixed sized context vector을 하나 만들다보니, input의 사이즈에 관계 없이 무조건 fixed 됨 -> input size 클수록 정보 손실 가능성 크다. 
2. Information Bottleneck: 특정 단어가 입력 문장에 어떤 단어와 연관이 깊은지에 대한 정보를 알수 없어

따라서 attention mechanism은 위 문제를 다음과 같이 해결 하였다. 
- 모든 시점의 hidden state를 그대로 유지하여 decoder한테 넘겨줘
- Decoder가 매 시점 다음 단어를 예측하기 전에 자신이 예측하려는 단어와 가장 관련성이 높은 입력 단어가 무엇인지 계산 (Attention)
- 위 2개를 바탕으로 dynamic context vector 생성 (using weighted sum)