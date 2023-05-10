# 대규모 언어 모델로 작업하는 방법

## 대규모 언어 모델의 작동 방식

[대규모 언어 모델][Large language models Blog Post]은 텍스트를 텍스트로 매핑하는 기능입니다. 텍스트 입력 문자열이 주어지면 대규모 언어 모델이 다음에 올 텍스트를 예측합니다.

대규모 언어 모델의 마법은 방대한 양의 텍스트에 대한 이러한 예측 오류를 최소화하도록 훈련함으로써 모델이 결국 이러한 예측에 유용한 개념을 학습하게 된다는 것입니다. 예를 들어 다음을 배웁니다.

* 어떻게 쓰는지
* 문법이 작동하는 방식
* 의역 방법
* 질문에 답하는 방법
* 대화를 유지하는 방법
* 여러 언어로 쓰는 방법
* 코딩하는 방법
* etc.

이러한 기능 중 어느 것도 명시적으로 프로그래밍되어 있지 않으며 모두 교육의 결과로 나타납니다.

GPT-3는 생산성 앱, 교육 앱, 게임 등을 포함하여 [수백 개의 소프트웨어 제품][GPT3 Apps Blog Post]을 지원합니다.

## 대규모 언어 모델을 제어하는 ​​방법

대규모 언어 모델에 대한 모든 입력 중에서 지금까지 가장 영향력 있는 것은 텍스트 프롬프트입니다.

대규모 언어 모델은 몇 가지 방법으로 출력을 생성하라는 메시지를 표시할 수 있습니다.

* **설명**: 모델에게 원하는 것을 말하세요.
* **완성**: 모델이 원하는 것의 시작 부분을 완성하도록 유도합니다.
* **데모**: 다음 중 하나를 사용하여 원하는 모델을 보여줍니다.
  * 프롬프트의 몇 가지 예
  * 미세 조정 훈련 데이터 세트의 수백 또는 수천 가지 예

각각의 예가 아래에 나와 있습니다.

### 프롬프트 교수법

명령을 따르는 모델(예: `text-davinci-003` 또는 `text-`로 시작하는 모든 모델)은 명령을 따르도록 특별히 설계되었습니다. 프롬프트 상단(또는 하단 또는 둘 다)에 교수법을 작성하고 모델이 최선을 다해 교수법을 따른 다음 중지합니다. 교수법은 상세할 수 있으므로 원하는 출력을 명시적으로 자세히 설명하는 단락을 작성하는 것을 두려워하지 마십시오.

프롬프트 교수법 예제:

```text
Extract the name of the author from the quotation below.

“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”
― Ted Chiang, Exhalation
```

출력:

```text
Ted Chiang
```

### 완료 프롬프트 예

완성 스타일 프롬프트는 언어 모델이 다음에 올 가능성이 가장 높다고 생각하는 텍스트를 작성하려고 시도하는 방식을 활용합니다. 모델을 조정하려면 보고 싶은 출력으로 완성될 패턴이나 문장을 시작해보세요. 직접 지시에 비해 대규모 언어 모델을 조정하는 이 모드는 더 많은 주의와 실험이 필요할 수 있습니다. 또한 모델은 중지 위치를 반드시 알 수 없으므로 원하는 출력 이상으로 생성된 텍스트를 잘라내기 위해 중지 시퀀스 또는 사후 처리가 필요한 경우가 많습니다.

완료 프롬프트 예제:

```text
“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”
― Ted Chiang, Exhalation

The author of this quote is
```

Output:

```text
 Ted Chiang
```

### 데모 프롬프트 예(퓨샷 학습)

완성 스타일 프롬프트와 유사하게 데모는 모델이 원하는 작업을 보여줄 수 있습니다. 이 접근 방식은 모델이 프롬프트에 제공된 몇 가지 예에서 학습하므로 퓨샷 학습이라고도 합니다.

예제 데모 프롬프트:

```text
Quote:
“When the reasoning mind is forced to confront the impossible again and again, it has no choice but to adapt.”
― N.K. Jemisin, The Fifth Season
Author: N.K. Jemisin

Quote:
“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”
― Ted Chiang, Exhalation
Author:
```

결과:

```text
 Ted Chiang
```

### 미세 조정된 프롬프트 예


학습 예시가 충분하면 사용자 지정 모델을 [미세 조정][Fine Tuning Docs]할 수 있습니다. 이 경우 모델이 제공된 교육 데이터에서 작업을 학습할 수 있으므로 지침이 필요하지 않습니다. 그러나 구분자 시퀀스(예: `->` 또는 `###` 또는 일반적으로 입력에 나타나지 않는 문자열)를 포함하여 프롬프트가 종료되고 출력이 시작되어야 할 때를 모델에 알리는 것이 도움이 될 수 있습니다. 구분자 시퀀스가 ​​없으면 모델이 보려는 답변에서 시작하지 않고 입력 텍스트에서 계속 정교화될 위험이 있습니다.

미세 조정된 프롬프트 예(유사한 프롬프트-완성 쌍에 대해 사용자 지정 학습된 모델의 경우):

```text
“Some humans theorize that intelligent species go extinct before they can expand into outer space. If they're correct, then the hush of the night sky is the silence of the graveyard.”
― Ted Chiang, Exhalation

###


```

출력:

```text
 Ted Chiang
```

## 코드 기능

대규모 언어 모델은 텍스트뿐만 아니라 코드에서도 훌륭할 수 있습니다. OpenAI의 특화된 코드 모델을 [Codex]라고 합니다.

Codex는 다음을 포함하여 [70개 이상의 제품][Codex Apps Blog Post]에 힘을 실어줍니다.

* [GitHub Copilot] (autocompletes code in VS Code and other IDEs)
* [Pygma](https://pygma.app/) (turns Figma designs into code)
* [Replit](https://replit.com/) (has an 'Explain code' button and other features)
* [Warp](https://www.warp.dev/) (a smart terminal with AI command search)
* [Machinet](https://machinet.net/) (writes Java unit test templates)

명령을 따르는 텍스트 모델(예: `text-davinci-002`)과 달리 Codex는 교수법을 따르도록 훈련되지 *않았습니다*. 결과적으로 좋은 프롬프트를 설계하는 데 더 많은 주의가 필요할 수 있습니다.

### 더 신속한 조언

더 신속한 예를 보려면 [OpenAI 예제][OpenAI Examples]를 방문하세요.

일반적으로 입력 프롬프트는 모델 출력을 개선하는 데 가장 좋은 수단입니다. 다음과 같은 트릭을 시도할 수 있습니다.:

* **보다 명확한 지침을 제공하십시오.** 예를 들어 출력을 쉼표로 구분된 목록으로 표시하려면 쉼표로 구분된 목록을 반환하도록 요청하십시오. 답을 모를 때 "모르겠어요"라고 말하고 싶다면 '모르면 "모르겠다"라고 말하세요.'
* **더 나은 예를 제공하십시오.** 프롬프트에서 예를 시연하는 경우 예가 다양하고 고품질인지 확인하십시오.
* **모델에게 전문가인 것처럼 답하도록 요청합니다.** 모델에게 명시적으로 고품질 출력물을 생성하도록 요청하거나 전문가가 작성한 것처럼 출력하면 모델이 전문가라고 생각하는 더 높은 품질의 답변을 제공하도록 유도할 수 있습니다. 쓸 것이다. 예: "다음 답변은 정확하고 고품질이며 전문가가 작성했습니다."
* **모델이 추론을 설명하는 일련의 단계를 기록하라는 메시지를 표시합니다.** 예를 들어, 대답 앞에 "[단계적으로 생각해 봅시다](https://arxiv.org/pdf/2205.11916v1.pdf )"와 같은 것을 추가합니다. 최종 답이 나오기 전에 추론에 대한 설명을 제공하라는 메시지가 표시되면 최종 답이 일관되고 정확할 가능성이 높아질 수 있습니다.



[Fine Tuning Docs]: https://beta.openai.com/docs/guides/fine-tuning
[Codex Apps Blog Post]: https://openai.com/blog/codex-apps/
[Large language models Blog Post]: https://openai.com/blog/better-language-models/
[GitHub Copilot]: https://copilot.github.com/
[Codex]: https://openai.com/blog/openai-codex/
[GPT3 Apps Blog Post]: https://openai.com/blog/gpt-3-apps/
[OpenAI Examples]: https://beta.openai.com/examples
