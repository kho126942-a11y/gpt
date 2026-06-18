gpt 2.0<br>
Tiny Shakespeare 데이터셋을 이용해 문자(character) 단위 언어모델을 학습하는 Tiny GPT 프로젝트입니다.  <br>
이 프로젝트는 Transformer 구조를 기반으로 하며, 논문 *Attention Is All You Need*의 핵심 아이디어인 **Self-Attention**, **Multi-Head Attention**, **Feed Forward Network**, **Residual Connection**, **Layer Normalization**을 간단한 GPT 모델로 구현하는 것을 목표로 합니다.<br>
<br>
1. 데이터셋 생성<br>
Tiny Shakespeare 텍스트 데이터를 사용하여 문자 단위 언어모델 학습용 데이터셋을 만듭니다.<br>
<br>
전체 텍스트에서 등장하는 문자들을 vocabulary로 만들고, 각 문자를 정수 인덱스로 변환합니다.  <br>
모델은 이전 문자들의 흐름을 보고 다음 문자를 예측하도록 학습됩니다.<br>
<br>
<img width="1795" height="924" alt="image" src="https://github.com/user-attachments/assets/bb490228-6107-4953-9fee-d34f05f7c5d7" /><br>
<br>
<br>
2. Multi-Head Attention<br>
<br>
Transformer의 핵심은 Attention입니다.<br>
Attention은 현재 토큰이 문장 안의 다른 토큰들을 얼마나 참고해야 하는지를 계산하는 방식입니다.<br>
Attention(Q, K, V) = softmax(QKᵀ / √dₖ)V<br>
논문에서는 Scaled Dot-Product Attention을 사용합니다.<br>
여기서 Q는 Query, K는 Key, V는 Value를 의미합니다.<br>
Query와 Key의 유사도를 계산한 뒤 softmax를 통해 중요도를 만들고, 그 중요도에 따라 Value를 가중합합니다.<br>
<br>
Multi-Head Attention은 하나의 attention만 사용하는 것이 아니라 여러 개의 attention head를 병렬로 사용하는 방식입니다.<br>
각 head는 서로 다른 관점에서 토큰 간 관계를 학습할 수 있습니다. 예를 들어 어떤 head는 가까운 문자 관계를 보고, 다른 head는 더 먼 문맥 정보를 볼 수 있습니다.<br>
<br>
GPT에서는 Encoder-Decoder 구조 전체를 사용하지 않고, Decoder 스타일의 masked self-attention을 사용합니다.<br>
즉, 현재 위치의 토큰은 미래 토큰을 볼 수 없고, 이전 토큰들만 참고할 수 있습니다. 이것은 다음 문자를 예측하는 언어모델에서 매우 중요합니다.<br>
<br>
Multi-Head Attention이란<br>
<img width="223" height="382" alt="image" src="https://github.com/user-attachments/assets/d7ca0b78-ae01-4abb-9ac7-4fbe363b300d" /><br>
<br>
<img width="1453" height="786" alt="image" src="https://github.com/user-attachments/assets/b41fac70-3012-4046-ab2b-b72d64fe3916" /><br>
<br>
<br>
3. Feedforward + Block<br><br>
Attention을 통과한 뒤에는 각 토큰 위치마다 독립적으로 Feed Forward Network를 적용합니다.<br>
<br>
Feed Forward Network는 각 토큰 벡터를 더 복잡한 표현으로 변환하는 MLP입니다.<br>
논문에서는 다음과 같은 구조를 사용합니다.<br>
FFN(x) = max(0, xW₁ + b₁)W₂ + b₂<br>
<br>
Block은 Transformer의 한 층을 의미합니다.<br>
일반적으로 하나의 Block은 다음 순서로 구성됩니다.<br>
<img width="251" height="157" alt="image" src="https://github.com/user-attachments/assets/fd4aae45-9455-4ab5-b2e9-dfae99129408" /><br>
<br>
Residual Connection은 기존 입력을 출력에 더해주는 구조입니다.<br>
이를 통해 깊은 모델에서도 정보 손실을 줄이고 학습을 안정화할 수 있습니다.<br>
<br>
<img width="205" height="382" alt="image" src="https://github.com/user-attachments/assets/13286568-e024-4664-a426-dd55dc29752b" /><br>
<br>
<img width="1054" height="532" alt="image" src="https://github.com/user-attachments/assets/ef961f37-8e57-4c30-8643-05898e2bff22" /><br>
<br>

4. Tiny GPT<br>
<br>
<img width="197" height="187" alt="image" src="https://github.com/user-attachments/assets/d516d902-10bb-4f01-bd0d-6d8f9bd051d2" /><br>
순으로 조립하여 Tiny GPT 모델을 만드는 부분이다.<br>
<br>
전체 구조<br>
<img width="230" height="160" alt="image" src="https://github.com/user-attachments/assets/38c35380-a58b-4cb8-992a-7c7ca3320011" /><br>
<br>
Token Embedding은 각 문자를 벡터로 바꾸는 역할을 합니다.<br>
Positional Embedding은 문자들의 위치 정보를 추가합니다.<br>
<br>
Transformer는 RNN처럼 순서대로 입력을 처리하지 않기 때문에 위치 정보가 필요합니다.<br>
<br>
최종적으로 Linear Layer는 각 위치에서 다음 문자로 올 확률 분포를 계산하기 위한 logits를 출력합니다.<br>
<br>
<img width="1029" height="581" alt="image" src="https://github.com/user-attachments/assets/f23dd0e4-d4ef-4ea2-a903-99e5e46d9bfa" /><br>
<br>
5. 학습<br>
학습 과정에서는 모델이 입력 sequence를 보고 다음 문자를 맞히도록 훈련됩니다.<br>
모델은 각 위치에서 다음 문자를 예측하고, 예측 결과와 실제 정답 사이의 Cross Entropy Loss를 계산합니다.<br>
<br>
학습 과정은 다음과 같습니다.<br>
<br>
<img width="584" height="178" alt="image" src="https://github.com/user-attachments/assets/c7749c32-5df7-4cd7-8a7a-f20938b30886" /><br>

<img width="286" height="304" alt="image" src="https://github.com/user-attachments/assets/8f26366c-a8d5-4f9a-abbe-8c3dbd2c88e9" /><br>
<br>
<img width="1689" height="697" alt="image" src="https://github.com/user-attachments/assets/52f6b0b3-adf2-4e8a-a999-7b67cf477c35" /><br>
<img width="301" height="760" alt="image" src="https://github.com/user-attachments/assets/28c0591f-0a72-4518-90a6-91316b38289e" /><br>
<br>
6. Sampling<br>
학습된 TinyGPT로 실제 문장을 생성(Text Generation)하는 코드<br>
<br>
처음에는 시작 문자 또는 빈 입력을 넣고, 모델이 다음 문자의 확률 분포를 출력합니다.<br>
그 확률 분포에서 하나의 문자를 샘플링한 뒤, 그 문자를 다시 입력에 추가합니다.<br>
<br>
이 과정을 반복하면 새로운 문장이 생성됩니다.<br>

<img width="811" height="419" alt="image" src="https://github.com/user-attachments/assets/d48c54a9-e86c-48fa-b6ce-90ad2e80bf3a" /><br>
<img width="419" height="404" alt="image" src="https://github.com/user-attachments/assets/a427b101-824b-4f12-9643-52cf274ba9bc" /><br>
<br>
<br>
<br>
<br>
TinyStories라는 다른 데이터를 이용하여 문자 단위 언어모델 만들기<br>
<br>
<br>
<img width="1101" height="696" alt="image" src="https://github.com/user-attachments/assets/30ce60dc-e5d9-4b3e-a2dc-b198b727556e" /><br>
<br>
<img width="1740" height="170" alt="image" src="https://github.com/user-attachments/assets/c7c9226f-39c8-499d-9208-62cd6a2819dd" /><br>
<br>
<img width="1039" height="603" alt="image" src="https://github.com/user-attachments/assets/c9dece16-9128-485a-8f79-8c27c80f84d0" /><br>
<br>
<img width="1703" height="757" alt="image" src="https://github.com/user-attachments/assets/4e1f6444-cb83-47c6-a3ca-bf4a669c7e5c" /><br>
<br>
<img width="1602" height="571" alt="image" src="https://github.com/user-attachments/assets/5ce78080-50a0-4e8f-a2a7-b8126e188545" /><br>
<br>
<br>
<br>
<br>
전래동화를 이용해 생성해보기
<br>
<img width="1452" height="781" alt="image" src="https://github.com/user-attachments/assets/08c6b90e-727d-40c1-857f-c9d237b15262" />
데이터가 작고 epoch가 많아서 모델이 창작하는 게 아니라 원문을 외워버렸다.<br>
<br>
<img width="1367" height="755" alt="image" src="https://github.com/user-attachments/assets/8bf29e33-e8d9-4514-a897-f433d0dbf464" />
<br>
<br>
<br>
기존 데이터 27,000자에서 120,000자로 늘렸다.<br>
<br>


<img width="1403" height="745" alt="image" src="https://github.com/user-attachments/assets/5e4eccc7-bbcc-4dea-86c4-750310f92e83" />

