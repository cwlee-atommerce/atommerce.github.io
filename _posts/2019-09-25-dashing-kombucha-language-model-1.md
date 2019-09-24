---
layout: post
title: 언어 모델이 무엇이고 왜 필요한가
tags: [언어모델, language model, conditional probability, smoothing, interpolation]
author-id: dashing-kombucha
excerpt_separator: <!--more-->
---

<br><br>
#### 목차:
**1. 언어 모델의 정의와 필요성
2. 언어 모델의 종류와 원칙
3. Count based 언어 모델의 이론
4. Count-based 언어 모델의 현실
4.1. Count-based 언어 모델의 현실: 히스토리 제한
4.2. Count-based 언어 모델의 현실: Smoothing
4.3. Count-based 언어 모델의 현실: 타당성
5. 언어 모델 평가
6. 마무리하며**

<br><br>
#### 1. 언어 모델의 정의와 필요성

언어 모델이라고 할 때, 직관적으로는 특정 언어를 시스템화, 단순화를 해서 온톨로지 같은 구조를 만드는 것처럼 들릴 수 있지만, NLP에서는 언어 모델이란 글자, 형태소, 어절, 표현 등 언어 단위(token)의 확률 분포를 계산하는 것을 말합니다.

위와 같이 정의한 언어 모델은 음성 인식, spell check, auto-filling, 기계 번역, 자연어 생성 등 다양한 범위에서 적용되어왔습니다. 또한 요즘은, 질의응답, 추론, 텍스트 분류, 대화 모델링 등 Deep Learning을 이용하는 hard 하다고 하는 NLP 태스크에 적용하는 데에 있어서 언어 모델은 특별한 의미가 생기는 것 같기도 합니다. 그래서 이제까지 몇 시대를 걸쳐 발전되어온 언어 모델에 대해서 포스팅 시리즈를 한 편씩 올리려고 합니다. 이 포스팅에서는 언어 모델의 조상인 count-based 모델부터 정리해 보겠습니다.

#### 2. 언어 모델의 종류와 원칙

언어 모델은 크게 보아 a) 출현 빈도 기반 (count-based, 또는 n-gram), b) 신경망 기반 (neural network based, 또는 continuous space) 두 가지로 나눌 수 있습니다. 이 두 가지는 확률 분포를 예측하는 방식은 다르지만, Firth라는 언어학자(!)가 정리한 “단어는 이웃을 보면 안다.”라는 전제에서 출발한 거라고 보면 될 것 같습니다.

> You shall know a word by the company it keeps. 
[Firth, J. R., 1957](https://en.wikipedia.org/wiki/John_Rupert_Firth)

특정 환경에서 어느 token이 나타날 수 있는지를 알고 싶으면, 보통 이런 환경에서 어떤 token이 많이 나타나는지 알면 어느 정도 예측이 가능하다고 보는 관점입니다. 그럴 때 환경(또는 히스토리, 또는 콘텍스트)은 다른 token인데 count-based, neural network based 등 언어 모델링을 하는 접근 간의 차이는 이 환경을 모델링하는 방식에 있다고 볼 수 있는 것 같습니다.

#### 3. Count based 언어 모델의 이론

Count-based 언어 모델은 어떠한 token sequence 뒤에 어떠한 token이 나올 조건부 확률(conditional probability)를 계산하는 것으로 정리할 수 있습니다. 이론적으로, 어떠한 sequence 뒤에 어떠한 token이 나올 확률이 얼마나 되는지 알기 위해서는 충분한 크기의 말뭉치에서 히스토리가 출현하는 빈도, 그리고 히스토리 뒤에 token이 출현하는 빈도(즉, ‘히스토리+token’ sequence의 출현 빈도)만 알면 계산할 수 있습니다.

<center>P(token|history) = C(history, token) / C(history) </center>

예를 들면, ‘멋진’ 뒤에 ‘아저씨’가 나올 확률은:

<center>P(‘아저씨’|‘멋진’) = C(‘멋진 아저씨’) / C(‘멋진’) = 20 / 100 = 0.2</center>

확률은 20%에 불과하지만 ‘멋진 빨리’, ‘멋진 오면’ 등 언어의 체계에조차 안 맞는 sequence보다는 진정한 언어 모델에서 더 높은 스코어일 것입니다.

_[...] 당시 그 멋진 아저씨들이 원샷을 하던 잔은 지금까지 전혀 변하지 않은 형태로 식당에서 사용되고 있다. 이 유리잔의 용도는 정말 다양하다. 물잔, 맥주잔, ‘쏘맥’잔, 심지어 여기에 믹스 커피를 타주는 식당도 있다._

[**[서울 꼴라주, 이기진](https://lib.sen.go.kr/lib/intro/search/detail.do?vCtrl=5447163212&isbn=9788970417325&menu_idx=40)**]

이 것만 알아도 음성 인식을 할 때, ‘멋진’ 뒤에 ‘아저씨’ / ‘아 저.. 시…’ / ‘아주… ’ 등등 옵션 중에 언어 모델을 통해서 제일 그럴싸한 옵션을 고를 수 있게 됩니다.

다른 예를 들자면은, 가령 ‘당시 그 멋진 아저씨들이 원샷을 하던’이라는 히스토리 sequence 뒤에 ‘잔’이라는 token이 나올 확률을 알고 싶습니다.

<center>P(잔|당시 그 멋진 아저씨들이 원샷을 하던) = P(당시) * P(그|당시) * P(멋진|당시 그) * P(아저씨들이|당시 그 멋진) * P(원샷을|당시 그 멋진 아저씨들이) * P(하던|당시 그 멋진 아저씨들이 원샷을) * P(잔|당시 그 멋진 아저씨들이 원샷을 하던)</center>

그럴 때는, 히스토리의 빈도부터 모든 웹 데이터를 긁어서도 0으로 나올 수 있고 따라서 구하고자하는 확률도 계산을 못 합니다. 그 외에도 인간이 생성할 수 있는 sequence가 무한한데 위와 같은 방식으로 확률을 계산하면은 인간이 보기에 타당한 sequence이지만 그 sequence에 대해서 언어 모델은 할 말이 없게 됩니다.

#### 4. Count-based 언어 모델의 현실

이론적으로, token 하나를 예측할 때, 그 token 전의 모든 히스토리를 알면 좋지만 실제로는 히스토리가 길어질수록 출현 빈도가 0에 가까워지는 위와 같은 Out-of-Vocabulary(OOV) 문제가 생길 수 있고 만약에 sequence가 말뭉치에 존재해도 출현 빈도가 낮아서 분포가 타당하다고 할 수 없고 계산하는 것 자체도 복잡해집니다<sup>[1](#myfootnote1)</sup>.
 
 #### 4.1. Count-based 언어 모델의 현실: 히스토리 제한

그렇기 때문에, ‘미래는 현재만 알면 과거와는 무관하다‘라는 Markov assumption을 취합니다. Markov assumption에 따르면, 예측하고자 하는 token 앞에 n-1 token만 알고 있으면 됩니다.
<br><br>
![Slack]({{ "/assets/img/marina/language_models_1/img1.png" | relative_url}})
[Markov assumption](http://spark-public.s3.amazonaws.com/nlp/slides/languagemodeling.pdf)
<br><br>

그럴 때, ‘현재’를 대표하는 token수는 보통 2-5 사이로 잡고 bigram model (n=2), trigram model (n=3) model이라 명칭합니다. 그럼, ‘당시 그 멋진 아저씨들이 원샷을 하던’ 뒤에 ‘잔’이 나올 확률을 아래와 같이 히스토리에서 최근 2 token까지만 기억하는 trigram 언어 모델을 적용해서 계산할 수 있습니다.

<center>P(잔|당시 그 멋진 아저씨들이 원샷을 하던) = P(당시) * P(그|당시) * P(멋진|당시 그) * P(아저씨들이 | 그 멋진) * P(원샷을|멋진 아저씨들이) * P(하던|아저씨들이 원샷을) * P(잔|원샷을 하던)</center>

  
그 전 계산 방식과 비교하면 구하고자 하는 P()들은 훨씬 더 현실적으로 보이지요?

#### 4.2. Count-based 언어 모델의 현실: Smoothing

Markov assumption은 그래도 언어 모델링을 가능하게 만들 뿐이지 완벽하게 만들지는 못 합니다. 위에 언급한 '충분한 크기의 말뭉치'는 얼마나 커야 모든 n-gram의 빈도를 타당하게 계산할 수 있을까요? 크고 커도 안 나타나거나 빈도가 너무 낮은 n-gram이 분명히 있을 텐데 이 문제를 극복하기 위해서는 여러 가지 기법을 사용합니다.

일단, 학습1 말뭉치에 n-gram이 아예 안 나타난 OOV 문제부터 다뤄보자면, OOV n-gram에도 어떠한 확률을 할당할 수 있게끔 smoothing이라는 기법을 적용할 수 있습니다. 간단한 additive smoothing은 확률을 계산하고자 하는 모든 n-gram의 빈도에 어떠한 δ(일반적으로, δ=1)를 추가함으로써 말뭉치에 출현한 n-gram은 조금 더 자주, 아예 출현 안 한 n-gram은 적어도 δ번은 나타났다 가정하고 확률 분포를 조금 더 현실적으로 계산할 수 있습니다.

<br><br>
![Slack]({{ "/assets/img/marina/language_models_1/img2.png" | relative_url}})
[Additive smoothing](http://spark-public.s3.amazonaws.com/nlp/slides/languagemodeling.pdf)
<br><br>

그렇게 하면, 학습 말뭉치에 안 나타난 n-gram에 대해서도 1/|V|의 낮은 확률이지만 적어도 계산은 가능하게 돼서 OOV 문제를 일부 해결할 수 있습니다.

#### 4.3. Count-based 언어 모델의 현실: 타당성

N-gram의 n을 잡을 때, n-gram이 길수록 언어의 구조를 더 잘 잡을 수 있는 것은 좋지만 그 만큼 빈도가 낮은데, 그 대신 더 기억력이 떨어지지만 믿음직한 n-gram에 의존해야 될까요? 그리고, n-gram이 말뭉치에 몇 번 나타나야 언어를 타당하게 대표할 수 있다고 보아야 할까요? 정답을 찾기가 참 어려울 수 있습니다. (예를 들면, 구글은 1,024,908,267,229 token의 말뭉치에서 40번 이상 나타나면 타당한 5-gram이라고 가정했습니다.)

위 딜레마의 간단한 해결책은 n-gram이 언어를 대표할 수 있다고 믿는 출현 빈도 임계값을 지정하고 그보다 적게 출현하는 n-gram은 n을 빈도가 임계값을 넘을 때까지 반복적으로 1씩 깎아주는 back-off이라는 기법이 있습니다.

실용적으로는, 임계값보다 적게 나타나는 긴 n-gram도 믿어보겠지만, 단, 다른 말뭉치에서의 빈도 기반으로, 가중치를 주겠다는 interpolation이라는 기법이 더 많이 활용되는 것 같습니다. 그럴 때, unigram(1-gram)부터 최대로 잡은 n까지 모든 k-gram에 대한 정보를 같이 활용할 수 있어서 전체 n-gram의 확률을 보다 타당하게 계산할 수 있다고 합니다.

<br><br>
![Slack]({{ "/assets/img/marina/language_models_1/img3.png" | relative_url}})
![Slack]({{ "/assets/img/marina/language_models_1/img4.png" | relative_url}})
[Simple interpolation](http://spark-public.s3.amazonaws.com/nlp/slides/languagemodeling.pdf)
<br><br>

위와 같은 방식은 일반화가 잘 돼야 하는 언어 모델 학습에는 현실적인 확률 분포를 산출하기에는 부족할 수 있습니다. 실제로 활용되는 기법은 위 원칙들을 발전시킨 Kneser-Ney smoothing,Jelinek-Mercer interpolation 등을 예로 들 수 있습니다.

#### 5. 언어 모델 평가
언어 모델이 얼마나 잘 학습이 되었는지를 확인하기 위해서는 학습 과정이 어떻든, perplexity라는 수치로 평가할 수 있습니다. Perplexity의 개념은 언어 모델에 노출이 안 된 test sequence의 출현 확률의 예측을 얼마나 확신 있게, 즉 얼마나 높게 할 수 있는지를 계산하는 것입니다. 해당 언어의 멀쩡한 sequence라면, 진정한 언어 모델은 당혹하지 않고 자신 있게 높은 확률을 예측해야지 ‘perplexed’ 하게 되면은 좋은 모델은 아닙니다!
<br><br>
![Slack]({{ "/assets/img/marina/language_models_1/img5.png" | relative_url}})
[Perplexity](http://spark-public.s3.amazonaws.com/nlp/slides/languagemodeling.pdf)
<br><br>

#### 6. 마무리하며

Count-based 언어 모델은 적합한 말뭉치에 위 기법을 적용해도 본질적인 한계점이 있습니다. 하나는, 사람이 자연어를 생성, 이해할 때, 장거리 의존성을 활용할 수 있는 반면에, n-gram 언어 모델은 콘텍스트가 적게만, 또는 일방적으로만 고려가 될 수 있습니다. 위 예시에서, ‘여기’가 의미하는 ‘유리잔’은 8 걸음 전에 언급되는데 그 정보까지 모델에 포함 시키려면 적어도 9-gram 언어 모델을 학습해야 합니다. 하지만 9-gram 수가 너무 많다는 curse of dimensionality 문제, 그리고 그 9-gram 중에 대부분은 출현 빈도가 너무 낮다는 sparseness 문제 때문에 그러한 언어 특성은 count-based 모델로는 실제로 해결이 어렵습니다.

더 하나는, 의미론적인 정보가 고려가 안 된다는 점도 문제입니다. 위 예시에서 ‘물잔’, ‘맥주잔’, ‘‘쏘맥’잔’ 등은 출현 빈도로 봤을 때, 서로 많이 다른 수치로 표현되겠지만 의미상으로 비슷하다는 점은 놓치게 되는 것이 하나의 예시입니다.


그럼, 다음 포스팅에서 위와 같은 한계점을 Neural Network으로 극복하려는 접근들을 살펴보도록 하겠습니다.

감사합니다!


#### Footnotes
<a name="myfootnote1">1</a>:  어휘 수가 100 000 개 되는 말뭉치에서 10-gram 모델을 학습하면 가능한 10-gram의 수는 105나 됩니다.
<a name="myfootnote2">2</a>: Count-based 모델의 학습은 확률을 계산하는 것이고 error backpropagation을 통한 신경망 기반 모델의 학습과는 다릅니다.