---
title: Large Language Model
date: 2024-01-30 09:05:26
tags: python
categories:
    - Python
---
# 대규모 언어 모델(LLM)

기본적으로 셀프 어텐션(self-attention) 기능을 갖춘 인코더와 디코더로 구성된 신경망 세트<br>
인코더와 디코더는 텍스트에서 의미를 추출하고 텍스트 내의 단어와 구문 간의 관계를 이해함.

- 비지도 학습이 가능하지만 더 정확한 설명은 트랜스포머가 자체 학습을 수행
- 이 과정을 통해 트랜스포머는 기본 문법, 언어 및 지식을 이해
- 입력을 순차적으로 처리하는 이전의 순환 신경망(RNN)과 달리 트랜스포머 전체 시퀀스를 병렬로 처리

LLM은 인간 언어의 입력 프롬프트를 기반으로 콘텐츠를 생성하는 생성형 AI(인공 지능)에 사용

- ChatGPT는 데이터에서 패턴을 식별하고 자연스럽고 읽기 쉬운 결과를 생성


- 카피라이팅
- 지식 기반 답변
- 텍스트 분류
- 코드 생성
- 텍스트 생성

## 학습 모델

- 제로샷 학습 : 기본 LLM은 응답 정확도는 다르지만 대개 프롬프트를 통해 명시적인 훈련 없이 광범위한 요청에 응답
- 퓨샷 학습 : 몇 가지 관련 훈련 예제를 제공하면 해당 영역에서 기본 모델 성능이 크게 향상
- 미세 조정 : 데이터 사이언티스트과 특정 애플리케이션과 관련된 추가 데이터로 파라미터를 조정하도록 기본 모델을 훈련한다는 점에서 퓨샷 학습의 연장

```python
pip install openai
```
```python
import openai

# Set your OpenAI GPT-3 API key
openai.api_key = 'your-api-key'

# Example prompt and completion
prompt = "Translate the following English text to French:"

response = openai.Completion.create(
    engine="text-davinci-002",  # You may choose a different engine
    prompt=prompt,
    max_tokens=50  # Adjust based on your requirements
)

# Extract the generated text from the API response
generated_text = response['choices'][0]['text']

print(generated_text)
```
GPT에게 질문하여 영어 텍스트를 프랑스어로 번역하도록 요청 하는 예시<br>
(OpenAI API 키 필요)

출처 : https://aws.amazon.com/ko/what-is/large-language-model/