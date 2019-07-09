---
layout: post
title: 코틀린 기본문법 이해하기
tags: [android, kotlin]
author-id: Woodroid
excerpt_separator: <!--more-->
---

오늘은 코틀린에서 사용되는 아주 기본적인 문법에 대해서 다루어 보겠습니다.

기존에 사용하는 자바 코드와 비교하면서 진행해 보도록 하겠습니다.
<!--more-->

<br>
### 변수선언

자바에서는 변수를 선언할때 이렇게 합니다.
```jsx
String a = "hello Java";
int b = 0;
boolean c = false;
```
<br>
하지만 코틀린에서는 이렇게 사용됩니다.
```jsx
val a: String = "hello Kotlin"
val b: Int = 0
val c: Boolean = false
```

코틀린은 자바와는 다르게 코드 끝에 세미콜론을 사용하지 않습니다.

매번 코드작성 시 세미콜른이 빠져 오류가 나는 경우가 많았는데요.

코틀린은 이 부분에 있어 제약이 없습니다.

<br>

### val, var
코틀린에서는 기본적으로 val, var을 사용해 변수를 생성합니다.

두가지의 차이점은 다음과 같습니다.

>var : 변경이 가능한 변수(mutable)

>val : 변경이 불가능한 변수(immutable)

<br>
Java와 비교한 예제로도 한번 보시겠습니다.

```jsx
// java
String a = "hello Java";

a = "hello Java!!"; // 정상적인 프로세스

final String a = "hello Java";

a = "hello Java!!"; // 문법 오류 발생



// kotlin
var a: String = "hello Kotlin"

a = "hello Kotlin!!" // 정상적인 프로세스

val a: String = "hello Kotlin"

a = "hello Kotlin!!" // 문법 오류 발생
```

<br><br>
### 타입 추론방식(Type inference)

코틀린 컴파일러는 타입 추론방식을 통해 자동으로 타입이 지정될 수 있습니다.
타입을 직접 명시하나 안 하냐는 취향에 맞게 골라서 하시면 되겠습니다.

예제로 보겠습니다.

```jsx
var s = "나는 String" // String으로 추론
var i = 0 // Int로 추론
var b = false // Boolean으로 추론
```

위와 같이 타입을 명시하지 않아도 변수를 선언하는 방법이 있습니다.

<br><br>
### 함수 생성

함수 생성은 예제를 통해 바로 비교해 보도록 하겠습니다.
<br><br>
자바에서의 함수 생성 방식

```jsx
// 리턴타입이 없는경우
void showMessage() {
    // ToastMessage 혹은 Log
}

// 리턴타입이 int인 경우
int sum(int a, int b) {
    return a + b;
}
```

코틀린에서의 생성 방식

```jsx
// 리턴타입이 없는경우
fun showMessage() {
    // ToastMessage 혹은 Log
}

// 리턴타입이 Int인 경우
fun sum(a: Int, b: Int): Int {
    return a + b
}
```

<br><br>
추가적으로 코틀린은 코드를 더 줄일수 있습니다!

어떻게요?? 바로 이렇게요.

```jsx
// 타입을 명시
fun sum(a: Int, b: Int): Int = a + b

// 타입을 추론
fun sum(a: Int, b: Int): = a + b
```
확실히 심플합니다. 아주 좋습니다!


<br><br>
### 문자열 템플릿

자바에서 문자열 템플릿을 사용할 경우에 인자가 많은 경우 혼동이 오기 쉽습니다.
코틀린에서는 $를 사용하면 이 부분에서 굉장히 깔끔한 구현이 가능합니다.

자바
```jsx
void showMessage(String message) {
    System.out.println(String.format("message: $s", message);
}
```

코틀린
```jsx
fun showMessage(message: String) {
    System.out.println("message: $message")
}
```

위에 보시는 바와 같이 굉장히 깔끔한 형태로 떨어지게 됩니다.

앞서 말씀드린 것처럼 한가지의 인자가 아닌 수많은 인자를 사용하게 될 경우 아주 유용하게 사용할 수 있을 것 같습니다.


<br><br>
### NULL 처리

자바에서는 아주 골치 아픈 녀석이 하나 있습니다. 바로 NULL을 처리해주는 것이죠.
하지만 코틀린에서는 기본적으로 null을 허용하지 않게끔 개발되어 null을 사용해도 안전하게 사용할 수 있습니다.

기본적인 변수 선언 시 사용 방법
```jsx
var a: Int = 0 // nonNull
var b: Int? = null // nullable

a = 1 // 문법오류
b = 1 // 정상 프로세스
```
<br><br>
* Safe Calls

코틀린에서 대표적으로 null 처리를 할 때 쓰는 방식입니다.


```jsx
val a = "Kotlin"
val b: String? = null
println(b?.length) // null을 허용하는 경우에는 ?를 통해 안전한 호출이 가능
println(a?.length) // safe call을 할 필요가 없음
```

함수 사용시
```jsx
var a: Int? = 1
var b: Int? = 2

// nonNull
fun sum(a: Int, b: Int): Int {
    return a + b
}

sum(a, b) // 문법적 오류 발생
```

```jsx
var a: Int? = 1
var b: Int? = 2

// nullable
fun sum(a: int?, b: Int?) {
    return a + b
}

sum(a, b) // 정상적인 프로세스
```
위 두 가지의 차이점은, 첫 번째는 null 값을 허용하지 않았기 때문에 nullable로 생성한 변수를 넣을 수 없다는 점입니다.

하지만 이렇게 하면 문법적 오류는 없앨 수 있습니다.
```jsx
sum(a!!, b!!)
```
단, 이렇게 문법적 오류를 해결할 경우 a 또는 b 값이 null인 경우 NullPointerException 이 발생하게 됩니다.

변숫값이 null이 아니라는 것을 장담할 수 없는 경우에는 null check 후 사용하시면 되겠습니다.

하지만 가급적이면 사용하지 않는 게 좋겠죠?

<br><br>
#### 참고 사이트

* <a href="https://kotlinlang.org/docs/reference/null-safety.html">코틀린 공식 문서 : Null Safty</a>

<br><br><br><br>