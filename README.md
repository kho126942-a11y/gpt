gpt 2.0
Tiny Shakespeare 데이터셋을 이용해서 문자(character) 단위 언어모델(Language Model)을 학습하기 위한 데이터셋을 만드는 과정
<img width="1795" height="924" alt="image" src="https://github.com/user-attachments/assets/bb490228-6107-4953-9fee-d34f05f7c5d7" />

1. Multi-head attention
Multi-Head Attention이란
<img width="223" height="382" alt="image" src="https://github.com/user-attachments/assets/d7ca0b78-ae01-4abb-9ac7-4fbe363b300d" />

<img width="1453" height="786" alt="image" src="https://github.com/user-attachments/assets/b41fac70-3012-4046-ab2b-b72d64fe3916" />

2. Feedforward + Block
Feedforward란 Attention 이후에 각 토큰을 독립적으로 처리하는 MLP(다층 신경망)
Block이란 Transformer의 한 층(layer)을 의미
<img width="205" height="382" alt="image" src="https://github.com/user-attachments/assets/13286568-e024-4664-a426-dd55dc29752b" />

<img width="1054" height="532" alt="image" src="https://github.com/user-attachments/assets/ef961f37-8e57-4c30-8643-05898e2bff22" />

3. Tiny GPT

<img width="197" height="187" alt="image" src="https://github.com/user-attachments/assets/d516d902-10bb-4f01-bd0d-6d8f9bd051d2" />
순으로 조립하여 Tiny GPT 모델을 만드는 부분이다.

전체 구조
<img width="205" height="385" alt="image" src="https://github.com/user-attachments/assets/dd68550a-230b-461b-8fb2-b42f62cdb6ec" />

<img width="1029" height="581" alt="image" src="https://github.com/user-attachments/assets/f23dd0e4-d4ef-4ea2-a903-99e5e46d9bfa" />

4. 학습
<img width="286" height="304" alt="image" src="https://github.com/user-attachments/assets/8f26366c-a8d5-4f9a-abbe-8c3dbd2c88e9" />

<img width="1689" height="697" alt="image" src="https://github.com/user-attachments/assets/52f6b0b3-adf2-4e8a-a999-7b67cf477c35" />
<img width="301" height="760" alt="image" src="https://github.com/user-attachments/assets/28c0591f-0a72-4518-90a6-91316b38289e" />

5. Sampling
학습된 TinyGPT로 실제 문장을 생성(Text Generation)하는 코드
<img width="811" height="419" alt="image" src="https://github.com/user-attachments/assets/d48c54a9-e86c-48fa-b6ce-90ad2e80bf3a" />
<img width="419" height="404" alt="image" src="https://github.com/user-attachments/assets/a427b101-824b-4f12-9643-52cf274ba9bc" />




