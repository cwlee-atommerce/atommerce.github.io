---
layout: post
title: 안드로이드 스튜디오 코틀린 시작하기
tags: [android, android-studio, kotlin]
author-id: Woodroid
excerpt_separator: <!--more-->
---

<br><br>
코틀린은 구글에서 안드로이드 공식 언어로 채택 할 정도로 강력하게 지원하고 있는 언어입니다.

자바와의 호환성이 매우 좋고, 기존 자바 코드를 변환하는 것도 매우 간편합니다.

따라서 오늘은 안드로이드 스튜디오에서 코틀린 개발을 위한 환경설정까지 진행해 보도록 하겠습니다.
<!--more-->

일단 안드로이드 앱 제작을 위해서는 안드로이드 스튜디오가 필요합니다. 아래 링크는 설치법과 가이드입니다.
> <a href="https://developer.android.com/studio/install?hl=ko" target="_blank">https://developer.android.com/studio/install?hl=ko</a>

<br><br>
#### 새로운 코틀린 프로젝트 시작하기

안드로이드 스튜디오를 설치하시고 처음 실행하시면 다음과 같은 화면이 나옵니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/1.png" | relative_url}})
<br><br>

해당 화면이 보여졌다면 'Start a new Android Studio project'를 클릭합니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/2.png" | relative_url}})
<br><br>

위에 보이는 화면에서는 기본 프로젝트 구성을 선택할 수 있습니다.
지금은 따로 사용하길 원하는 항목이 없으므로 맨 앞 Basic Activity를 선택해 주시겠습니다.
만약 다른 걸 원하신다면 선택하셔도 상관없습니다.

선택을 완료하셨으면 Next를 눌러 다음 화면으로 이동하겠습니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/3.png" | relative_url}})
<br><br>

위에 보이는 화면은 프로젝트의 기본 설정이라고 보시면 될 것 같습니다.

Name은 말 그대로 이름을 설정하는 필드이고 Package Name도 마찬가지로 패키지의 이름입니다.

지금은 테스트이므로 기본 설정 그대로 가겠습니다.

Save location은 원하는 경로로 지정을 해 주시면 되겠습니다.

자, 이제 제일 필수적인 Language입니다. (어차피 둘 중에 하난 골라야 함)
정말 편리하게도 Java 또는 Kotlin 두 가지 밖에 없네요. 저희는 Kotlin을 시작해야되니 Kotlin을 선택하시면 되겠죠??
물론 Java를 선택하셔도 나중에 환경설정이 따로 가능하니 실수하셔도 상관없습니다. (새 프로젝트 만드는 게 빠름)

그리고 나서 그다음에 보이는 Minimum API level 항목은 내가 빌드할 기기의 최소 API 버전입니다.
여기서 설정하는 버전에 따라서 나중에 호환되는 기기가 제한되기도 합니다.
이 항목에 대해서는 따로 정리를 할 기회가 있을 것 같아 여기서는 생략하겠습니다.

그럼 다음으로 넘어가 보겠습니다. Finish 클릭!


두둥!
<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/4.png" | relative_url}})
<br><br>

여러분들 앞에 멋진 화면이 나타났습니다. 벌써 얼추 개발자 냄새가 슬슬 나기 시작하죠?? (짝짝)

기본 설정이라면 여러분들 앞에 나타난 화면은 흰둥이가 맞습니다.

저는 기본 테마를 바꿔 이렇게 어두침침한 화면을 보게 되었죠.

기본 테마 설정은 안드로이드 스튜디오 상단 툴바에 Android Studio > Preferences... > Appearance & Behavior 항목 안에 Appearance에 보시면 Theme이라는 항목이 보이실 겁니다. 클릭해서 리스트를 내려 보시면 Light, Darcula, High constrast 세 가지 항목이 있는데요, Light는 밝은 거, Darcula는 어두운 거라고 보시면 됩니다. 나머지 항목은 관심도 없고... 해서 설명은 생략.

다시 넘어와서,
이 공간에서만큼은 여러분이 창조자가 되어 마음껏 넣고 꾸미고 만들어낼 수 있습니다.
물론 그에 따른 문법에 대한 기초 지식은 분명히 필요할 것입니다.
문법에 관한 것도지속해서 포스팅하도록 하겠습니다. (꾸벅)


<br><br>
#### 가상 시뮬레이터(에뮬레이터)를 통해 빌드하기

앞서 코틀린을 이용해 안드로이드 스튜디오에 기본적인 설정을 해 보았는데요.
여기서 끝나기는 좀 아쉽기도하고.. 

(분량도 좀 채우...)
큼큼
그래도 이왕 만든거 내가 만든게 잘 올라가는지도 확인은 해 봐야겠지요!?(무시)

빌드는 두 가지의 방법이 있습니다.
* 실제 기기를 통해 빌드하는 방법
* 가상 기기를 통해 빌드하는 방법

이번엔 실제 테스트 기기가 없다는 가정하에 가상 기기를 통해 빌드해 보도록 하겠습니다.

일단 실행을 하려면 실행 버튼을 찾아야겠죠?? 찾아줍시다.
실행 버튼도 두 가지가 있습니다.
프로그램 설정 탭에 Run이라는 항목 안에 Run 'app' 이 있구요.
툴바에 초록색 화살표 버튼이 있습니다 (숏컷).

우리는 바로 앞에 보이는 툴바에 있는 숏컷 버튼을 눌러주도록 하겠습니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/6.png" | relative_url}})
<br><br>

음??
바로 실행이 될 줄 알았는데 무슨 이상한 화면이 나타납니다.
위에 보이는 것처럼 다이얼로그(모달) 창이 하나 나타났는데요. 해당 다이얼로그는 앞서 말씀드린 빌드하는 방법을 선택하는 창입니다.
내가 실제 연결된 기기를 통해 빌드할 것인지, 아니면 가상 기기를 통해 빌드할 것인지를 선택 할 수 있습니다.

처음에 말씀드린 것처럼 저희는 실제 테스트 기기가 없는 상황입니다.
그렇다면 저희는 가상 기기가 필요한데, 지금 보는 화면에서는 가상기기조차 보이지 않습니다. 
그럼 무엇을 해야 하는가. 너무 당연하죠. 만들면 됩니다. (어렵지 않아요)

이제 가상 기기를 만들어 보겠습니다!
다이얼로그 왼쪽 하단에 보이는 Create New Virtual(가상) Device(기기) 버튼을 눌러주세요.


<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/7.png" | relative_url}})
<br><br>

자 그럼 위와 같은 화면이 나왔습니다. Select Hardware 말 그대로 하드웨어 선택입니다. 여러분이 사용하실 기기를 선택하라는 말입니다.
다른 기기도 좋지만 지금 제일 무난한 사이즈인 nexus 6P를 선택하고 Next를 눌러 주시면 되겠습니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/8.png" | relative_url}})
<br><br>

이번에는 System Image입니다. 이 기기가 사용하게 될 API 버전을 선택한다고 생각하시면 되겠습니다.
지금 빨갛게 에러가 난 이유는 제가 사용하려는 Oreo 버전 SDK가 지금 스튜디오에 다운로드된 게 없기 때문입니다. 
없으면? 받아줍시다! Download를 자신 있게 눌러주세요.
그러면 다이얼로그가 하나 더 생성되어 다운로드 진행 상황에 대해 나타납니다.
여기는 원하시는 버전에 맞게 알맞게 설정해 주시면 되겠습니다.

기다려주세요..
<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/9.png" | relative_url}})
<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/10.png" | relative_url}})
<br><br>

자 끝났습니다! Finish를 눌러줍시다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/11.png" | relative_url}})
<br><br>

이제 보시면 아까와 같은 에러는 말끔하게 사라졌습니다.
하지만 다른 버전을 선택할 경우에 해당 SDK도 다시 받아야 합니다.
저희는 일단 Pie를 사용해 빌드하겠습니다. 선택하시고 Next를 눌러주세요.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/12.png" | relative_url}})
<br><br>

이제 마지막 단계입니다. 힘내세요!
이번에는 가상기기의 속성을 설정할 차례입니다. 여기서는 크게 중요한 부분은 없습니다.
이름을 설정하고 기기의 방향을 설정할 수 있습니다. 여기는 무난하게 지나가 주겠습니다. Next!

<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/13.png" | relative_url}})
<br><br>

자 이제 다이얼로그에 Available Virtual Device에 제가 만든 가상 기기가 나타났습니다.
마우스를 가져다가 선택하고 ok를 눌러주시면 되겠습니다.

<br><br>
쨔잔! 조금만 기다리시면 여러분이 만든 프로젝트가 가상기기에서 잘 돌아가는 것을 확인할 수 있습니다.
<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/14.png" | relative_url}})
<br><br>
![Slack]({{ "/assets/img/woodroid/android_start_with_kotlin/15.png" | relative_url}})
<br><br>



참 쉽죠?? 이제 당신은 코틀린 개발자입니다.(진짜에요)
