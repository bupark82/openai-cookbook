# 신뢰성을 향상시키는 기술

GPT-3가 작업에 실패하면 어떻게 해야 합니까?

- 보다 신뢰할 수 있는 답변을 이끌어내는 더 나은 프롬프트를 찾고 계십니까?
- 맞춤형 모델을 미세 조정하기 위해 수천 개의 예제에 투자하시겠습니까?
- 모델이 작업을 수행할 수 없다고 가정하고 계속 진행합니까?

간단한 답은 없습니다. 상황에 따라 다릅니다. 그러나 작업에 논리적 추론이나 복잡성이 포함된 경우 이 문서의 기술을 시도하여 보다 안정적이고 성능이 우수한 프롬프트를 작성하는 것이 좋습니다.

## 복잡한 작업에서 GPT-3가 실패하는 이유

13에 17을 곱하라는 요청을 받으면 즉시 답이 떠오를까요? 우리 대부분은 그렇지 않을 것입니다. 그렇다고 해서 인간이 두 자릿수 곱셈을 할 수 없다는 의미는 아닙니다. 몇 초, 약간의 펜과 종이만 있으면 13 x 17 = 130 + 70 + 21 = 221이라는 것을 계산하는 것이 어렵지 않습니다.

마찬가지로 GPT-3에게 다음 토큰을 계산하는 데 걸리는 시간에 수행하기에는 너무 복잡한 작업을 제공하면 잘못된 추측을 만들 수 있습니다. 그러나 인간과 마찬가지로 이것이 반드시 모델이 작업을 수행할 수 없다는 것을 의미하지는 않습니다. 문제를 추론할 시간과 공간이 있으면 모델이 여전히 안정적으로 대답할 수 있습니다.

예를 들어 `text-davinci-002`에 다음과 같은 저글링 공에 대한 수학 문제를 묻는 경우 잘못 대답합니다.:

```text-davinci-002
Q: A juggler has 16 balls. Half of the balls are golf balls and half of the golf balls are blue. How many blue golf balls are there?
A:
```

```text-davinci-002
There are 8 blue golf balls.
```

이것은 GPT-3가 간단한 수학 문제를 풀 수 없다는 것을 의미합니까? 아니요; 사실, `단계적으로 생각하자`라는 모델을 제시함으로써 모델이 안정적으로 문제를 해결한다는 것이 밝혀졌습니다.:

```text-davinci-002
Q: A juggler has 16 balls. Half of the balls are golf balls and half of the golf balls are blue. How many blue golf balls are there?
A: Let's think step by step.
```

```text-davinci-002
There are 16 balls in total. 
Half of the balls are golf balls. 
That means that there are 8 golf balls. 
Half of the golf balls are blue. 
That means that there are 4 blue golf balls.
```

물론 이 `단계별로 생각하자` 트릭이 실제로 일반적으로 작동하는지 또는 이 특정 문제에 대해 운이 좋은지 단 하나의 예만으로 구분하기는 어렵습니다. 하지만 정말 효과가 있습니다. 단어 수학 문제의 벤치마크에서 `차근차근 생각해보자` 트릭은 GPT-3의 해결률을 18%에서 79%로 엄청나게 높였습니다!

## 모델 기능은 컨텍스트에 따라 다릅니다.

GPT-3으로 작업하는 방법을 배울 때 흔히 발생하는 개념적 실수 중 하나는 GPT-3의 기능이 모든 컨텍스트에서 고정되어 있다고 믿는 것입니다. 예를 들어, GPT-3가 간단한 논리 질문을 잘못 받으면 간단한 논리를 할 수 없음에 틀림없다.

그러나 `단계적으로 생각하자` 예에서 알 수 있듯이 GPT-3의 명백한 오류는 때때로 모델이 올바른 출력을 향해 스스로를 조종하는 데 도움이 되는 더 나은 프롬프트로 해결할 수 있습니다.

## 복잡한 작업의 안정성을 향상시키는 방법

이 문서의 나머지 부분에서는 복잡한 작업에서 대규모 언어 모델의 안정성을 개선하는 기술을 공유합니다. 일부 기술은 특정 유형의 문제에만 적용되지만 대부분은 다음과 같이 광범위한 작업에 적용할 수 있는 일반 원칙을 기반으로 합니다, 예제,:

- 더 명확한 지침을 제공합니다.
- 복잡한 작업을 더 간단한 하위 작업으로 분할
- 모델이 작업을 계속할 수 있도록 지침을 구조화합니다.
- 대답하기 전에 모델에게 설명을 요청하십시오.
- 가능한 많은 답변에 대한 근거를 묻고 종합합니다.
- 많은 출력을 생성한 다음 모델을 사용하여 최상의 출력을 선택합니다.
- 맞춤형 모델을 미세 조정하여 성능 극대화

## 복잡한 작업을 간단한 작업으로 나누기

모델에게 생각할 시간과 공간을 더 많이 주는 한 가지 방법은 작업을 더 간단한 조각으로 나누는 것입니다.

예를 들어 모델에게 일부 텍스트(이 경우 Clue 게임)에 대한 객관식 질문을 묻는 작업을 고려하십시오. 직접 질문을 하면 `text-davinci-002`는 단서 3과 5를 조합할 수 없으며 잘못된 대답을 합니다.:
 
```text-davinci-002
Use the following clues to answer the following multiple-choice question.
 
Clues:
1. Miss Scarlett was the only person in the lounge.
2. The person with the pipe was in the kitchen.
3. Colonel Mustard was the only person in the observatory.
4. Professor Plum was not in the library nor the billiard room.
5. The person with the candlestick was in the observatory.
 
Question: Was Colonel Mustard in the observatory with the candlestick?
(a) Yes; Colonel Mustard was in the observatory with the candlestick
(b) No; Colonel Mustard was not in the observatory with the candlestick
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick
 
Solution:
```
 
```text-davinci-002
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick
```
 
단서 3과 5는 Mustard 대령이 천문대에 있는 유일한 사람이고 천문대에 있는 사람이 촛대를 가지고 있다는 것을 입증하지만, 모델은 그것들을 (a) 예라는 정답으로 결합하지 못합니다.
 
그러나 답을 직접 묻는 대신 작업을 세 부분으로 나눌 수 있습니다.:
 
- 먼저 단서를 하나씩 살펴보고 단서가 잠재적으로 관련이 있는지 고려하십시오.
- 둘째, 관련 단서를 결합하여 질문에 대한 답변을 추론합니다.
- 셋째, 최종 답변을 작성합니다: (a), (b) 또는 (c)
 
```text-davinci-002
Use the following clues to answer the following multiple-choice question, using the following procedure:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant
(2) Second, combine the relevant clues to reason out the answer to the question
(3) Third, map the answer to one of the multiple choice answers: either (a), (b), or (c)
 
Clues:
1. Miss Scarlett was the only person in the lounge.
2. The person with the pipe was in the kitchen.
3. Colonel Mustard was the only person in the observatory.
4. Professor Plum was not in the library nor the billiard room.
5. The person with the candlestick was in the observatory.
 
Question: Was Colonel Mustard in the observatory with the candlestick?
(a) Yes; Colonel Mustard was in the observatory with the candlestick
(b) No; Colonel Mustard was not in the observatory with the candlestick
(c) Unknown; there is not enough information to determine whether Colonel Mustard was in the observatory with the candlestick
 
Solution:
(1) First, go through the clues one by one and consider whether the clue is potentially relevant:
```
 
```text-davinci-002
Clue 1: Miss Scarlett was the only person in the lounge. This clue is not relevant.
Clue 2: The person with the pipe was in the kitchen. This clue is not relevant.
Clue 3: Colonel Mustard was the only person in the observatory. This clue is relevant.
Clue 4: Professor Plum was not in the library nor the billiard room. This clue is not relevant.
Clue 5: The person with the candlestick was in the observatory. This clue is relevant.
 
(2) Second, combine the relevant clues to reason out the answer to the question:
Clue 3 and Clue 5 both say that the person with the candlestick was in the observatory. Colonel Mustard was the only person in the observatory, so Colonel Mustard was the person with the candlestick.
 
(3) Third, write the final answer:
(a) Yes; Colonel Mustard was in the observatory with the candlestick
```
 
모델에게 생각할 시간과 공간을 더 주고 추론 계획에 따라 안내함으로써 모델은 (a) 예라는 정답을 알아낼 수 있습니다.

복잡한 명령을 더 작은 하위 작업으로 분할하는 또 다른 이점은 모델이 각 하위 작업에 집중할 수 있도록 도와준다는 것입니다.

예를 들어 `text-davinci-002`에 텍스트를 원래 언어로 요약하도록 요청하면 모델이 다시 영어로 전환될 수 있습니다.:

```text-davinci-002
Summarize the text using the original language of the text. The summary should be one sentence long.

Text:
"""
La estadística (la forma femenina del término alemán Statistik, derivado a su vez del italiano statista, "hombre de Estado")​ es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.​ La estadística es una ciencia formal deductiva, con un conocimiento propio, dinámico y en continuo desarrollo obtenido a través del método científico formal. En ocasiones, las ciencias fácticas necesitan utilizar técnicas estadísticas durante su proceso de investigación factual, con el fin de obtener nuevos conocimientos basados en la experimentación y en la observación. En estos casos, la aplicación de la estadística permite el análisis de datos provenientes de una muestra representativa, que busca explicar las correlaciones y dependencias de un fenómeno físico o natural, de ocurrencia en forma aleatoria o condicional.
"""

Summary:
```

```text-davinci-002
The text explains that statistics is a science that studies the variability, collection, organization, analysis, interpretation, and presentation of data, as well as the random process that generates them following the laws of probability.
 ```

 그러나 먼저 모델에 텍스트의 언어를 식별하도록 요청한 다음 텍스트를 요약하면 더 신뢰할 수 있습니다.:

```text-davinci-002
First, identify the language of the text. Second, summarize the text using the original language of the text. The summary should be one sentence long.

Text:
"""
La estadística (la forma femenina del término alemán Statistik, derivado a su vez del italiano statista, "hombre de Estado")​ es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.​ La estadística es una ciencia formal deductiva, con un conocimiento propio, dinámico y en continuo desarrollo obtenido a través del método científico formal. En ocasiones, las ciencias fácticas necesitan utilizar técnicas estadísticas durante su proceso de investigación factual, con el fin de obtener nuevos conocimientos basados en la experimentación y en la observación. En estos casos, la aplicación de la estadística permite el análisis de datos provenientes de una muestra representativa, que busca explicar las correlaciones y dependencias de un fenómeno físico o natural, de ocurrencia en forma aleatoria o condicional.
"""

Language:
```

```text-davinci-002
Spanish

La estadística es una ciencia que estudia la variabilidad, colección, organización, análisis, interpretación, y presentación de los datos, así como el proceso aleatorio que los genera siguiendo las leyes de la probabilidad.
```

## 대답하기 전에 모델에게 설명을 요청하십시오.

답변의 신뢰성을 향상시키는 또 다른 강력한 기술은 모델이 최종 답변으로 즉시 이동하는 대신 점진적으로 답변을 추론하도록 유도하는 것입니다. `소리내어 생각`함으로써 모델은 정답에 도달할 가능성이 훨씬 더 높아질 수 있습니다.

### 제로샷

#### 방법

[Kojima Takeshi et al. in 2022](https://arxiv.org/abs/2205.11916) 모델이 답을 추론하도록 유도하는 가장 쉬운 방법은 단순히 `단계별로 생각해보자`를 답 앞에 추가하는 것입니다. 그림 2는 예를 보여줍니다.

[![제로샷 추론 예시](images/zero-shot_reasoners_fig2.png)
<br>출처: *Large Language Models are Zero-Shot Reasoners* Takeshi Kojima et al. (2022).](https://arxiv.org/abs/2205.11916)

#### 결과

이 간단한 트릭을 MultiArith 수학 데이터 세트에 적용하면 저자는 `단계별로 생각하자`가 정확도를 18%에서 79%로 4배로 높인다는 사실을 발견했습니다!

[![제로샷 추론 예시](images/zero-shot_reasoners_tab5.png)
<br>출처: *Large Language Models are Zero-Shot Reasoners* Takeshi Kojima et al. (2022).](https://arxiv.org/abs/2205.11916)

#### 시사점

[![제로샷 추론 예시](images/zero-shot_reasoners_tab1.png)
<br>출처: *Large Language Models are Zero-Shot Reasoners* Takeshi Kojima et al. (2022).](https://arxiv.org/abs/2205.11916)

자세한 내용은 [전체 논문](https://arxiv.org/abs/2205.11916)를 참조하십시오.

이 기술을 자신의 작업에 적용하는 경우 지침을 사용자 지정하는 실험을 두려워하지 마십시오. `단계적으로 생각하자`는 다소 일반적이므로 사용 사례에 맞게 사용자 정의된 더 엄격한 형식을 따르는 지침으로 더 나은 성능을 찾을 수 있습니다. 예를 들어 `먼저 X가 참일 수 있는 이유에 대해 단계별로 생각하십시오. 둘째, Y가 참일 수 있는 이유에 대해 단계별로 생각하십시오. 셋째, X와 Y 중 어떤 것이 더 합리적인지 차근차근 생각해 보세요.`. 그리고 모델을 추적하는 데 도움이 되는 예제 형식을 제공할 수도 있습니다. 예를 들면 다음과 같습니다.:

```text-davinci-002
Using the IRS guidance below, answer the following questions using this format:
(1) For each criterion, determine whether it is met by the vehicle purchase
- {Criterion} Let's think step by step. {explanation} {yes or no, or if the question does not apply then N/A}.
(2) After considering each criterion in turn, phrase the final answer as "Because of {reasons}, the answer is likely {yes or no}."

IRS guidance:
"""
You may be eligible for a federal tax credit under Section 30D if you purchased a car or truck that meets the following criteria:
- Does the vehicle have at least four wheels?
- Does the vehicle weigh less than 14,000 pounds?
- Does the vehicle draw energy from a battery with at least 4 kilowatt hours that may be recharged from an external source?
- Was the vehicle purchased in a year before 2022?
  - If so, has the manufacturer sold less than 200,000 qualifying vehicles? (Tesla and GM have sold more than 200,000 qualifying vehicles.)
- Was the vehicle purchased in a year after 2022?
  - If so, is the vehicle present in the following list of North American-assembled vehicles? (The only electric vehicles assembled in North America are the Audi Q5, BMW 330e, BMW X5, Chevrolet Bolt EUV, Chevrolet Bolt EV, Chrysler Pacifica PHEV, Ford Escape PHEV, Ford F Series, Ford Mustang MACH E, Ford Transit Van, GMC Hummer Pickup, GMC Hummer SUV, Jeep Grand Cherokee PHEV, Jeep Wrangler PHEV, Lincoln Aviator PHEV, Lincoln Corsair Plug-in, Lucid Air, Nissan Leaf, Rivian EDV, Rivian R1S, Rivian R1T, Tesla Model 3, Tesla Model S, Tesla Model X, Tesla Model Y, Volvo S60, BMW 330e, Bolt EV, Cadillac Lyriq, Mercedes EQS SUV, and Nissan Leaf.)
"""

Question: Can I claim a federal tax credit for my Toyota Prius Prime bought in 2021?

Solution:

(1) For each criterion, determine whether it is met by the vehicle purchase
- Does the vehicle have at least four wheels? Let's think step by step.
```

```text-davinci-002
 The Toyota Prius Prime has four wheels, so the answer is yes.
- Does the vehicle weigh less than 14,000 pounds? Let's think step by step. The Toyota Prius Prime weighs less than 14,000 pounds, so the answer is yes.
- Does the vehicle draw energy from a battery with at least 4 kilowatt hours that may be recharged from an external source? Let's think step by step. The Toyota Prius Prime has a battery with at least 4 kilowatt hours that may be recharged from an external source, so the answer is yes.
- Was the vehicle purchased in a year before 2022? Let's think step by step. The Toyota Prius Prime was purchased in 2021, which is before 2022, so the answer is yes.
- Was the vehicle purchased in a year after 2022? N/A
- If so, is the vehicle present in the following list of North American-assembled vehicles? N/A

(2) After considering each criterion in turn, phrase the final answer as "Because of {reasons}, the answer is likely {yes or no}."

Because the Toyota Prius Prime meets all of the criteria for a federal tax credit, the answer is likely yes.
```

### 퓨샷 예

#### 방법

모델이 답을 추론하도록 유도하는 방법은 여러 가지가 있습니다. 한 가지 방법은 몇 가지 예(`few-shot`)로 시연하는 것입니다. [Google에서 Jason Wei와 Denny Zhou et al.](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)등이 연구했습니다. 다음은 몇 번의 생각 사슬 프롬프트의 예입니다.

[![생각의 사슬 예시](images/chain_of_thought_fig1.png)
<br>출처: *사고 유도 사슬은 대규모 언어 모델에서 추론을 유도합니다* Jason Wei 및 Denny Zhou 외. (2022)](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

인간 라벨러가 작성한 추론 체인의 추가 데모:

[![생각의 사슬 예시](images/chain_of_thought_fig3.png)
<br>출처: *사고 유도 사슬은 대규모 언어 모델에서 추론을 유도합니다* Jason Wei 및 Denny Zhou 외. (2022)](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

[(배가 실제로 떠 있는지 여부에 대한 의문이 제기되었습니다.)](https://twitter.com/Meaningness/status/1561062170074370048?s=20&t=mpHt8f3RRboztXxdhLFnWQ)

#### 결과

초등학교 수학 문제를 테스트한 결과, 저자들은 일련의 생각 프롬프트가 해결률을 18%에서 57%로 세 배로 높인다는 사실을 발견했습니다.

[![생각의 사슬 예시](images/chain_of_thought_fig5.png)
<br>출처: *사고 유도 사슬은 대규모 언어 모델에서 추론을 유도합니다* Jason Wei 및 Denny Zhou 외. (2022)](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

수학 문제 외에도 일련의 사고 프롬프트는 스포츠 이해, 동전 던지기 추적 및 마지막 문자 연결과 관련된 질문에 대한 성과를 높였습니다. 대부분의 경우 성능 향상을 포화시키는 데 필요한 예제가 많지 않았습니다(8 미만 정도).

[![생각의 사슬 예시](images/chain_of_thought_fig11.png)
<br>출처: *사고 유도 사슬은 대규모 언어 모델에서 추론을 유도합니다* Jason Wei 및 Denny Zhou 외. (2022)](https://ai.googleblog.com/2022/05/language-models-perform-reasoning-via.html)

자세한 내용은 [전체 논문](https://arxiv.org/abs/2201.11903)를 참조하십시오.

#### 시사점

`단계적으로 생각하자` 기법과 비교하여 소수 예제 기반 접근 방식의 한 가지 장점은 모델이 최종 답에 도달하기 전에 수행할 추론의 형식, 길이 및 스타일을 더 쉽게 ​​지정할 수 있다는 것입니다. 이는 모델이 처음에 올바른 방식이나 깊이로 추론하지 않는 경우에 특히 유용할 수 있습니다.

### 미세조정

#### 방법

일반적으로 작업에서 최대 성능을 얻으려면 사용자 지정 모델을 미세 조정해야 합니다. 그러나 설명을 사용하여 모델을 미세 조정하려면 작성하는 데 비용이 많이 드는 수천 개의 예제 설명이 필요할 수 있습니다.

2022년 Eric Zelikman과 Yuhuai Wu et al. 모델을 미세 조정하는 데 사용할 수 있는 설명 데이터 세트를 생성하기 위해 몇 번의 프롬프트를 사용하는 영리한 절차를 게시했습니다. 아이디어는 몇 번의 프롬프트를 사용하여 후보 설명을 생성하고 정답을 생성하는 설명만 유지하는 것입니다. 그런 다음 일부 오답에 대한 추가 설명을 얻으려면 질문의 일부로 정답을 제공하면서 몇 번의 프롬프트를 다시 시도합니다. 저자는 그들의 절차를 STaR(Self-taught Reasoner)라고 불렀습니다.

[![STaR 절차](images/star_fig1.png)
<br>출처: *STaR: 추론을 통한 부트스트래핑 추론* 작성자: Eric Zelikman 및 Yujuai Wu 외. (2022)](https://arxiv.org/abs/2203.14465)

이 기술을 사용하면 수천 개의 예제 설명을 작성할 필요 없이 미세 조정의 이점과 사고 사슬 프롬프트의 이점을 결합할 수 있습니다.

#### Results

저자는 이 기술을 Common Sense Q&A 데이터 세트에 적용한 후 STaR이 일련의 사고 프롬프트 단독(73% > 37%)과 미세 조정 단독(73% > 60%) 모두를 능가한다는 것을 발견했습니다.:

[![STaR results](images/star_tab1.png)
<br>Source: *STaR: Bootstrapping Reasoning With Reasoning* by Eric Zelikman and Yujuai Wu et al. (2022)](https://arxiv.org/abs/2203.14465)

자세한 내용은 [전체 논문](https://arxiv.org/abs/2203.14465)를 참조하십시오.

#### 시사점

미세 조정 데이터 세트를 확장하거나 수정하기 위해 몇 번의 프롬프트를 사용하는 것은 설명 작성을 넘어 일반화할 수 있는 아이디어입니다. 예를 들어 훈련하려는 구조화되지 않은 텍스트가 많은 경우 프롬프트를 사용하여 구조화되지 않은 텍스트에서 구조화 데이터 세트를 추출한 다음 해당 구조화 데이터 세트에서 사용자 지정 모델을 미세 조정할 기회를 찾을 수 있습니다.

## 생각의 사슬 프롬프트에 대한 확장

생각의 사슬 프롬프트의 여러 확장도 출판되었습니다.

### 선택 추론 프롬프트

#### 방법

Antonia Creswell 등이 발표한 사고 사슬 기술의 한 가지 확장은 설명과 답변을 생성하기 위한 단일 프롬프트를 더 작은 부분으로 분할하는 것입니다. 첫째, 프롬프트는 텍스트에서 관련 사실 하위 집합을 선택합니다(`선택 프롬프트`). 그런 다음 두 번째 프롬프트가 선택된 사실에서 결론을 추론합니다(`추론 프롬프트`). 그런 다음 이러한 프롬프트는 루프에서 번갈아 가며 여러 단계의 추론을 생성하고 결국 최종 답변에 도달합니다. 저자는 다음 그림에서 아이디어를 설명합니다.

[![선택-추론 프롬프트](images/selection-inference_fig1.png)
<br>출처: *선택-추론: 해석 가능한 논리적 추론을 위한 대규모 언어 모델 활용* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2205.09712)

#### 결과

7B-매개변수 모델에 적용했을 때 저자는 선택 추론 프롬프트가 bAbi 및 Proof Writer 벤치마크 작업(둘 다 더 긴 추론 단계 시퀀스가 ​​필요함)에 대한 일련의 생각 프롬프트에 비해 성능이 크게 향상되었음을 발견했습니다. 그들이 달성한 최고의 성능은 선택-추론 프롬프트와 미세 조정을 결합한 것입니다.

[![선택-추론 프롬프트](images/selection-inference_fig4.png)
<br>출처: *선택-추론: 해석 가능한 논리적 추론을 위한 대규모 언어 모델 활용* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2205.09712)

#### 시사점

이러한 벤치마크의 이득은 컸지만, 이러한 벤치마크는 더 긴 추론 시퀀스가 ​​필요했기 때문에 특별히 선택되었습니다. 많은 단계의 추론이 필요하지 않은 문제에서는 이득이 더 적을 수 있습니다.

결과는 대규모 언어 모델 작업에 대한 몇 가지 일반적인 교훈을 강조합니다. 첫째, 복잡한 작업을 더 작은 작업으로 나누는 것은 안정성과 성능을 향상시키는 좋은 방법입니다. 작업이 원자적일수록 모델이 오류를 범할 여지가 줄어듭니다. 둘째, 최대 성능을 얻는 것은 종종 선택한 접근 방식과 미세 조정을 결합하는 것을 의미합니다.

자세한 내용은 [전체 논문](https://arxiv.org/abs/2205.09712)를 참조하십시오.

### 충실한 추론 아키텍처

선택-추론 촉진 기법을 발표한 지 몇 달 후, 저자는 후속 논문에서 다음과 같은 아이디어로 기법을 확장했습니다.

- 선택-추론 주기가 언제 중단되거나 계속되어야 하는지 알아내기
- 여러 추론 경로를 검색하는 데 도움이 되는 가치 함수 추가
- 문장 자체를 작성하는 대신 문장 레이블(예: sen1)에 대해 추론하도록 모델을 미세 조정하여 가짜 사실의 환각을 줄입니다.

#### 방법

원래의 선택-추론 기술에서는 특수한 `선택` 및 `추론` 프롬프트가 번갈아 가며 사실을 선택하고 이러한 사실로부터 추론을 수행하며 일련의 추론 단계를 생성하기 위해 결합됩니다.

저자는 이 기술을 두 가지 추가 구성 요소로 확장합니다.

첫째, 저자는 각 추론 단계 후에 지금까지의 추론이 질문에 답하기에 충분한지 묻는 `고지` 모델을 추가합니다. 그렇다면 모델이 최종 답변을 생성합니다.

고삐 모델은 다음과 같은 몇 가지 이점을 제공합니다.

- 필요에 따라 선택-추론 프로세스를 중지하거나 계속 진행하도록 지시할 수 있습니다.
- 프로세스가 중단되지 않으면 답을 얻지 못할 것입니다. 이는 종종 환각적인 추측보다 선호됩니다.

[![성실한 추론](images/faithful-reasoning_fig3.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

[![성실한 추론](images/faithful-reasoning_fig5.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

둘째, 저자는 추론 단계의 품질을 평가하고 여러 추론 궤적을 검색하는 데 사용되는 가치 함수를 추가합니다. 이는 안정성 향상에 대한 공통 주제를 반영합니다. 모델에서 단일 답변을 생성하는 대신 일련의 답변을 생성한 다음 일부 유형의 가치 함수/판별자/검증자 모델을 사용하여 최상의 것을 선택합니다.

[![성실한 추론](images/faithful-reasoning_fig7.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

이 두 가지 확장 외에도 저자는 가짜 사실의 환각을 줄이기 위해 트릭을 사용합니다. 모델에게 사실에 입각한 문장을 작성하도록 요청하는 대신 문장 레이블(예: sen1)로 작업하도록 모델을 미세 조정합니다. 이렇게 하면 모델이 프롬프트 컨텍스트에서 언급되지 않은 가짜 사실을 환각하는 것을 방지할 수 있습니다.

[![성실한 추론](images/faithful-reasoning_fig4.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

#### 결과

저자는 ProofWriter 작업(표시되지 않음) 및 [EntailmentBankQA](https://allenai.org/data/entailmentbank)(표시됨)의 두 가지 벤치마크에서 기술을 평가했습니다. 이 기술은 특히 더 어려운 추론 문제에서 정확도를 크게 높였습니다.

[![성실한 추론](images/faithful-reasoning_tab2.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

또한 그들의 문장 레이블 조작 트릭은 본질적으로 환각을 제거했습니다!

[![성실한 추론](images/faithful-reasoning_tab5.png)
<br>출처: *대규모 언어 모델을 사용한 충실한 추론* 작성자: Antonia Creswell et al. (2022)](https://arxiv.org/abs/2208.14271)

#### 시사점

이 백서에서는 대규모 언어 모델의 안정성을 개선하는 데 도움이 되는 여러 가지 교훈을 설명합니다.

- 복잡한 작업을 더 작고 안정적인 하위 작업으로 분할
- 단계별 방식으로 답을 생성하고 그 과정에서 답을 평가합니다.
- 가능한 답을 많이 생성하고 다른 모델이나 기능을 사용하여 가장 잘 보이는 답을 선택합니다.
- 모델이 말할 수 있는 것을 제한하여 환각을 줄입니다(예: 문장 대신 문장 레이블 사용).
- 특화된 작업에 따라 모델을 미세 조정하여 모델 성능 극대화

자세한 내용은 [전체 논문](https://arxiv.org/abs/2205.09712)를 참조하십시오.

### 최소에서 최대 프롬프트

긴 추론 사슬(선택-추론이 빛을 발하는 곳)에서 제대로 수행하지 못하는 것 외에도 생각 사슬 프롬프트는 예제가 짧지만 작업이 길 때 특히 어려움을 겪을 수 있습니다.

#### 방법

Least-to-Most 프롬프트는 추론 작업을 더 작고 더 신뢰할 수 있는 하위 작업으로 분할하는 또 다른 기술입니다. 아이디어는 다음과 같은 프롬프트를 통해 모델에서 하위 작업을 도출하는 것입니다. `{질문}을 해결하려면 먼저 해결해야 한다: "` 그런 다음 해당 하위 작업을 사용하여 모델이 솔루션을 생성할 수 있습니다. 해결책은 원래 질문에 추가되고 최종 답변이 생성될 때까지 프로세스가 반복됩니다.

[![Least-to-most 프롬프트](images/least-to-most_fig1.png)
<br>출처: *Least-to-Most Prompting Enables Complex Reasoning in Large Language Models* 작성자: Denny Zhou et al. (2022)](https://arxiv.org/abs/2205.10625)

#### 결과

코드에 최적화되어 있지만 여전히 텍스트를 이해할 수 있는 `code-davinci-002`를 사용하여 긴 추론 체인을 포함하는 벤치마크에 적용했을 때 저자는 16% -> 99.7%의 이득을 측정했습니다!

[
![마지막 문자 연결 작업에 대한 Least-to-most 프롬프트 결과](images/least-to-most_tab4.png)
![SCAN에서 Least-to-most 프롬프트 결과](images/least-to-most_tab9.png)
![DROP 수치 추론에 대한 Least-to-most 프롬프트 결과](images/least-to-most_tab11.png)
<br>출처: *Least-to-Most Prompting Enables Complex Reasoning in Large Language Models* 작성자: Denny Zhou et al. (2022)](https://arxiv.org/abs/2205.10625)

#### 시사점

최소에서 최대까지의 프롬프트에서 위의 이점이 인상적이지만 긴 추론 체인이 필요한 매우 좁은 작업 세트에서 측정됩니다.

그럼에도 불구하고 공통 주제를 보여줍니다. (a) 복잡한 작업을 더 작은 하위 작업으로 나누고 (b) 답을 찾기 위해 모델에 더 많은 시간과 공간을 제공하여 신뢰성을 높입니다.

자세한 내용은 [전체 논문](https://arxiv.org/abs/2205.10625)를 참조하십시오.

## 관련 아이디어

### 마이유틱 프롬팅

#### 방법

정답의 가능성을 최대화하려고 시도하는 이전 기술과 달리, 또 다른 접근 방식은 GPT-3을 사용하여 가능한 설명 트리(*정답 및 오답* 모두)를 생성한 다음 관계를 분석하여 어떤 집합이 올바른지 추측하는 것입니다. 이 기법은 [Jaehun Jung et al. 2022년 5월](https://arxiv.org/abs/2205.11822)에 의해 마이유틱 프롬프트로 만들어졌습니다.(마이유틱은 아이디어를 도출하기 위해 질문하는 소크라테스식 방법과 관련된 의미).

이 방법은 복잡하며 다음과 같이 작동합니다.

- 먼저, 각 노드가 참 또는 거짓일 수 있는 진술인 maieutic 트리를 구축합니다.
  - 객관식 질문 또는 참/거짓 진술로 시작합니다(예: '전쟁은 비길 수 없습니다').
  - 질문에 대한 각각의 가능한 대답에 대해 모델을 사용하여 해당 설명을 생성합니다(`전쟁은 동점일 수 없습니까? True, because`와 같은 프롬프트 사용).
  - 그런 다음 질문과 생성된 설명으로 모델을 유도하고 답을 생성하도록 요청합니다. 설명을 뒤집으면(`{explanation}`과 같은 접두사를 사용하여) 답변이 뒤집히면 설명이 '논리적으로 통합'된 것으로 간주됩니다.
  - 설명이 논리적으로 통합되지 않은 경우 각 설명이 참 또는 거짓 질문으로 바뀌면서 위의 과정을 재귀적으로 반복하고 각각의 새로운 질문에 대해 더 많은 설명을 생성합니다.
  - 모든 재귀적 설명이 끝나면 설명 트리로 끝납니다. 여기서 트리의 각 리프에는 설명을 뒤집으면 모델의 대답이 뒤집히는 속성이 있습니다.
- 둘째, 트리를 관계 그래프로 변환합니다.
  - 트리의 각 노드에 대해 각 노드에 대한 모델의 상대적 신뢰도를 계산합니다(주어진 설명에 대해 `True` 응답을 얻을 확률에서 유추).
  - 트리의 각 노드 쌍에 대해 모델을 사용하여 수반(암시) 또는 모순 여부를 식별합니다.
- 셋째, 가장 일관된 믿음을 찾고 그것을 사실로 받아들입니다.
  - 구체적으로 각 노드에 대한 믿음의 강도와 노드 간의 논리적 관계를 사용하여 문제를 가중 최대 만족 문제(MAX-SAT)로 공식화합니다.
  - 솔버를 사용하여 가장 일관성 있는 신념 세트를 찾고 이를 사실로 받아들입니다.

[
  ![Maieutic 프롬프트](images/maieutic_fig2.png)
  ![Maieutic 프롬프트](images/maieutic_fig6.png)
<br>출처: *Maieutic Prompting: Logical Consistent Reasoning with Recursive Explanations* by 정재훈 외. (2022)](https://arxiv.org/abs/2205.11822)


#### 결과

[![Maieutic 프롬프트 결과](images/maieutic_tab1.png)
<br>출처: *Maieutic Prompting: Logical Consistent Reasoning with Recursive Explanations* by 정재훈 외. (2022)](https://arxiv.org/abs/2205.11822)

#### 시사점

복잡성 외에도 이 방법의 한 가지 제한은 객관식으로 제시될 수 있는 질문에만 적용되는 것으로 보인다는 것입니다.

자세한 내용은 [전체 논문](https://arxiv.org/abs/2205.11822)를 참조하십시오.

## 확장

### 자기 일관성

#### 방법

개별 답변 세트가 있는 작업의 경우 신뢰도를 향상시키는 한 가지 간단한 방법은 모델에서 여러 설명 및 답변을 샘플링한 다음(양의 온도 사용) 가장 자주 나타나는 최종 답변을 선택하는 것입니다.

[![자기 일관성 방법](images/self-consistency_fig1.png)
<br>출처: *Self-Consistency Improves Chain of Thought Reasoning in Language Models* by Xuezhi Wang et al. (2022)](https://arxiv.org/abs/2203.11171)

#### 결과

이 기술은 일련의 수학 및 추론 벤치마크에서 정확도를 1~24% 포인트 높였습니다. (아래 도표는 Google LaMDA 모델의 결과입니다. Google의 더 큰 PaLM 모델을 사용하면 기준선이 더 높았지만 이득은 약간 작았습니다.)

[![자체 일관성 결과](images/self-consistency_fig3.png)
<br>출처: *Self-Consistency Improves Chain of Thought Reasoning in Language Models* by Xuezhi Wang et al. (2022)](https://arxiv.org/abs/2203.11171)

#### 시사점

이 기술은 구현이 간단하지만 비용이 많이 들 수 있습니다. 10개의 답변 세트를 생성하면 비용이 10배 증가합니다.

또한 이러한 많은 기술과 마찬가지로 답이 제한된 작업에만 적용됩니다. 각 답변이 고유한 개방형 작업(예: 시 쓰기)의 경우 가장 일반적인 답변을 선택하는 것이 무엇을 의미하는지 명확하지 않습니다.

마지막으로, 이 기술은 답변에 도달하기 위한 여러 경로나 구문이 있을 때 가장 유용해야 합니다. 경로가 하나뿐이면 기술이 전혀 도움이 되지 않을 수 있습니다. 극단적인 예: 작업이 단일 토큰 응답을 생성하는 것이라면 100세대에서 가장 일반적인 토큰을 가져오는 것은 가장 높은 logprobs(온도=0에서 단일 세대로 얻을 수 있음)가 있는 토큰을 가져오는 것과 다를 바 없습니다.

### 검증자

작업 성능을 개선하기 위한 또 다른 핵심 기술은 주요 생성 모델의 출력을 평가하기 위해 검증자 또는 판별자 모델을 훈련시키는 것입니다. 판별자가 출력을 거부하면 허용 가능한 출력을 얻을 때까지 생성 모델을 다시 샘플링할 수 있습니다. 많은 경우에 답을 만드는 것보다 답을 판단하는 것이 더 쉽기 때문에 이 방법의 힘을 설명하는 데 도움이 됩니다.

#### 방법

2021년에 OpenAI 연구원은 다음 절차를 사용하여 이 기술을 초등학교 수학 문제에 적용했습니다.

- 첫째, 그들은 질문과 해결책에 대한 모델을 미세 조정했습니다.
- 훈련 세트의 각 문제에 대해 100개의 솔루션을 생성했습니다.
- 100개의 솔루션 각각은 최종 답변이 올바른지 여부에 따라 자동으로 정답 또는 오답으로 분류되었습니다.
- 이러한 솔루션을 사용하여 일부는 옳고 일부는 올바르지 않은 것으로 분류하여 검증자 모델을 미세 조정하여 질문 및 후보 솔루션이 올바른지 그른지를 분류했습니다.
- 마지막으로 테스트 시간에 생성 모델이 각 문제에 대해 100개의 솔루션을 생성하고 검증자 모델에 따라 가장 높은 점수를 받은 것을 최종 답변으로 선택합니다.

[![검증 방법](images/verifiers_fig3.png)
<br>출처: *수학 단어 문제 해결을 위한 교육 검증기* by Karl Cobbe et al. (2021)](https://arxiv.org/abs/2110.14168)

#### Results

With a 175B GPT-3 model and 8,000 training examples, this technique substantially lifted grade school math accuracy from ~33% to ~55%.

[![Verifier results](images/verifiers_fig5.png)
<br>Source: *Training Verifiers to Solve Math Word Problems* by Karl Cobbe et al. (2021)](https://arxiv.org/abs/2110.14168)

#### 시사점

자기 일관성 기술과 유사하게 이 방법은 작업당 100개의 솔루션을 생성하면 비용이 약 100배 증가하므로 비용이 많이 들 수 있습니다.

## 신뢰성 이론

위의 기술은 접근 방식이 다르지만 모두 복잡한 작업의 안정성을 개선한다는 목표를 공유합니다. 주로 다음을 수행합니다.:

- 신뢰할 수 없는 작업을 더 작고 더 안정적인 작업으로 분해(예: 선택-추론 프롬프팅)
- 여러 단계 또는 여러 관계를 사용하여 시스템의 신뢰성을 개별 구성 요소보다 크게 만듭니다(예: 기계 프롬프트).

### 확률 그래픽 모델

덜 신뢰할 수 있는 구성 요소로 신뢰할 수 있는 시스템을 구축하려는 이러한 패러다임은 확률 프로그래밍을 연상시키며 해당 분야의 많은 분석 기술이 여기에 적용될 수 있습니다.

논문 *Language Model Cascades*에서 David Dohan et al. 확률적 그래픽 모델의 패러다임에서 위의 기술을 해석합니다.:

#### 생각의 사슬

[![생각의 사슬에 대한 그래픽 모델](images/lm_cascades_fig1.png)
<br>출처: David Dohan et al.의 *Language Model Cascades* (2022)](https://arxiv.org/abs/2207.10342)

#### 미세 조정된 사고 유도 사슬 / 독학으로 추론

[![미세 조정된 사고 프롬프트의 그래픽 모델](images/lm_cascades_fig3.png)
<br>출처: David Dohan et al.의 *Language Model Cascades* (2022)](https://arxiv.org/abs/2207.10342)

#### 선택 추론 프롬프트

[![선택-추론 프롬프트의 그래픽 모델](images/lm_cascades_fig4.png)
<br>출처: David Dohan et al.의 *Language Model Cascades* (2022)](https://arxiv.org/abs/2207.10342)

#### 검증자

[![검증기의 그래픽 모델](images/lm_cascades_fig5.png)
<br>출처: David Dohan et al.의 *Language Model Cascades* (2022)](https://arxiv.org/abs/2207.10342)

#### 시사점

이러한 기술을 확률적 그래픽 모델로 공식화하는 것은 특정 문제를 해결하는 데 즉시 유용하지 않을 수 있지만 프레임워크는 새로운 기술을 선택, 결합 및 발견하는 데 도움이 될 수 있습니다.

## 마무리 생각

대규모 언어 모델에 대한 연구는 매우 활발하고 빠르게 발전하고 있습니다. 연구자들은 계속해서 모델을 개선할 뿐만 아니라 모델을 가장 잘 활용하는 방법에 대한 이해도 지속적으로 향상시킵니다. 이러한 발전 속도를 강조하기 위해 위에서 공유한 모든 논문은 지난 12개월 이내에 출판되었습니다(2022년 9월에 작성함).

앞으로 더 나은 모델과 더 나은 기술이 게시될 것으로 기대합니다. 여기에 있는 특정 기술이 미래의 모범 사례에 의해 가려지더라도 그 뒤에 있는 일반 원칙은 모든 전문 사용자 도구 키트의 핵심 부분으로 남을 것입니다.

## Bibliography

| Lesson                                                                                                                         | Paper                                                                                                                                     | Date     |
|--------------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------|----------|
| Break complex tasks into simpler subtasks (and consider exposing the intermediate outputs to users)                            | [AI Chains: Transparent and Controllable Human-AI Interaction by Chaining Large Language Model Prompts](https://arxiv.org/abs/2110.01691) | 2021 Oct |
| You can improve output by generating many candidates, and then picking the one that looks best                                 | [Training Verifiers to Solve Math Word Problems](https://arxiv.org/abs/2110.14168)                                                        | 2021 Oct |
| On reasoning tasks, models do better when they reason step-by-step before answering                                            | [Chain of Thought Prompting Elicits Reasoning in Large Language Models](https://arxiv.org/abs/2201.11903)                                 | 2022 Jan |
| You can improve step-by-step reasoning by generating many explanation-answer outputs, and picking the most popular answer      | [Self-Consistency Improves Chain of Thought Reasoning in Language Models](https://arxiv.org/abs/2203.11171)                               | 2022 Mar |
| If you want to fine-tune a step-by-step reasoner, you can do it with multiple-choice question & answer data alone              | [STaR: Bootstrapping Reasoning With Reasoning](https://arxiv.org/abs/2203.14465)                                                          | 2022 Mar |
| The step-by-step reasoning method works great even with zero examples                                                          | [Large Language Models are Zero-Shot Reasoners](https://arxiv.org/abs/2205.11916)                                                         | 2022 May |
| You can do better than step-by-step reasoning by alternating a ‘selection’ prompt and an ‘inference’ prompt                    | [Selection-Inference: Exploiting Large Language Models for Interpretable Logical Reasoning](https://arxiv.org/abs/2205.09712)             | 2022 May |
| On long reasoning problems, you can improve step-by-step reasoning by splitting the problem into pieces to solve incrementally | [Least-to-most Prompting Enables Complex Reasoning in Large Language Models](https://arxiv.org/abs/2205.10625)                            | 2022 May |
| You can have the model analyze both good and bogus explanations to figure out which set of explanations are most consistent    | [Maieutic Prompting: Logically Consistent Reasoning with Recursive Explanations](https://arxiv.org/abs/2205.11822)                        | 2022 May |
| You can think about these techniques in terms of probabilistic programming, where systems comprise unreliable components       | [Language Model Cascades](https://arxiv.org/abs/2207.10342)                                                                               | 2022 Jul |
| You can eliminate hallucination with sentence label manipulation, and you can reduce wrong answers with a 'halter' prompt   | [Faithful Reasoning Using Large Language Models](https://arxiv.org/abs/2208.14271)                                                                               | 2022 Aug |
