---
layout: post
title: 안드로이드 App Bundle 생성 및 테스트하기
tags: [android, kotlin]
author-id: Woodroid
excerpt_separator: <!--more-->
---

앱을 개발하면서 개인적으로 매우 중요하다 생각되는 부분이 앱 크기입니다.
<br>그래서 이번에는 불필요한 앱 크기를 줄여주는 App Bundle에 대해서 간략하게 알아보고 생성 및 테스트를 진행해 보겠습니다.

<!--more-->

<br>
### App Bundle?

App Bundle은 앱을 보다 효율적으로 빌드하고 출시 할 수 있는 Android의 새로운 공식 게시 형식입니다.
App Bundle을 사용하면 더 작은 앱 크기에서 더 나은 환경을 더욱 쉽게 ​​제공 할 수 있어 설치 성공률을 높이고 제거를 줄일 수 있습니다. 

Bundle 형태로 전환하기 위해 기존의 코드를 리팩터링 할 필요가 없습니다.
그리고 전환이 된 후에는 모듈 형 앱 개발 및 맞춤형 기능 제공을 활용할 수 있습니다.

App Bundle은 Android Studio 3.2 또는 그 이상에서 사용 가능합니다.

<br>
### 앱 번들 생성 방법

App Bundle을 생성하는 방법은 인증된 apk를 생성하는 것과 별다른 차이가 없습니다.

그림을 보시면서 순서대로 차근차근 알아보겠습니다.

<br>
![Slack]({{ "/assets/img/woodroid/android_app_bundle/1.png" | relative_url}})
<br>
처음은 apk 생성과 동일합니다. 상단 메뉴에 Build > Generate Signed Bundle / APK... 를 클릭해주세요.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_app_bundle/2.png" | relative_url}})
<br>
위의 화면에서 보시는 바와 같이 App Bundle의 장점에 대한 설명과 선택을 할 수 있는 화면이 나타납니다.
장점에 대한 자세한 설명은 맨 아래 첨부된 링크에서 확인하실 수 있습니다.
Android App Bundle을 선택해 주세요.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_app_bundle/3.png" | relative_url}})
<br>
익숙한 화면이 나타났습니다. Apk와 다를 것 없이 key store path와 나머지 항목들을 입력해 주세요.
<br>모든 항목을 입력하셨다면 아래 선택사항을 확인해 주세요.
<br>Remember password는 말 그대로 다음번에 빌드할 때 비밀번호를 저장해 두는 것이고
아래에 있는 항목은 앱 인증 시 플레이 스토어에 키를 등록해놓고 사용할 때 필요한 키를 추출하는 기능에 대해 선택하는 항목입니다.

비밀번호는 선택사항 키는 나중에 App Bundle을 등록할 때 필요하므로 선택하고 다음으로 넘어가겠습니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_app_bundle/4.png" | relative_url}})
<br>
이제 빌드할 버전을 선택하겠습니다. 모두 아시겠지만 저는 release 버전이 필요하므로 release를 선택하겠습니다.
위에 Destination Folder는 자신이 App Bundle을 생성할 위치를 선택해 주시면 되겠습니다.

<br><br>
![Slack]({{ "/assets/img/woodroid/android_app_bundle/5.png" | relative_url}})
<br>
Finish를 누르고 최대 몇 분 정도 지나면 오른쪽 밑에 보이는 것처럼 알림 창이 하나 나타납니다.
앱 번들이 성공적으로 생성되었다는 표시이고, 해당 알림 창에서는 두 가지를 확인할 수가 있죠.
App Bundle이 생성된 위치를 확인할 수 있는 locate와 생성된 App Bundle 항목을 분석할 수 있는 analyze입니다.

<br><br><br><br>
### App Bundle 테스트하기

위에 과정을 통해서 우리는 App Bundle을 성공적으로 생성을 해보았습니다.
<br>자 이제 이대로 앱 스토어에 배포를 바로 진행해 보겠습니다.

.<br>
.<br>
.<br>
.<br>
.<br>
.<br>
.<br>
.<br>
.<br>
.<br>
.<br>

라고 하면 안 되겠지요??
<br><br>아무리 완벽하게 개발을 했다고는 하나 저희는 큰 필수 과정이 하나 남아있습니다.
바로 기기 테스트입니다.
<br>기존 방식대로라면 생성한 apk를 가지고 바로 테스트 기기 또는 가상 시뮬레이터에 설치가 가능한데요.
<br>App Bundle은 대체 어떻게 테스트 기기에 설치하고 확인해 볼 수 있는지 궁금해집니다.
<br>지금 바로 알아보겠습니다. 

<br>
#### Bundletool 사용하기

<br>
공식문서에서는 생성된 App bundle를 bundletool을 사용해 테스트하라고 명시되어있습니다.
<br>bundletool을 사용하는 방법에 대해서 알아보겠습니다.

<br>해당 명령어를 사용하기 위해서는 아래 링크에서 jar파일을 받아주시기 바랍니다.
> <a href="https://github.com/google/bundletool/releases" target="_blank">https://github.com/google/bundletool/releases</a>
bundletool을 바로 사용하기 위해 환경설정을 해주겠습니다.

<br>
에디터를 통해 열기
> open -e ~/.bash_profile

<br>
에디터가 열리면 아래 값을 입력해 주세요.

> alias bundletool="java -jar ./DOWNLOAD_PATH/bundle_tool.jar"

저장을 하시고 터미널을 재시작합니다. (재시작하지 않으면 동작하지 않습니다)

<br><br>
바로 사용해 보도록 하겠습니다.
```jsx
bundletool build-apks --bundle=./YOUR_PATH/app.aab --output=./YOUR_PATH/test-app.apks
```
* --bundle은 경우에는 App Bundle의 위치
* --output은 생성할 apks의 생성될 위치

<br>

추가적으로 환경설정 없이 사용하는 방법도 있습니다.
```jsx
java -jar ./Downloads/bundletool-all-0.10.2.jar build-apks --bundle=./YOUR_PATH/app.aab --output=./YOUR_PATH/test-app.apks
```

바로 이렇게 사용하시면 됩니다.

<br>
apks가 생성되는게 확인이 되셨나요??
<br>그렇다면 이제 apks를 연결된 기기에 설치하는 작업을 해보겠습니다.
```jsx
bundletool install-apks --apks=./DOWNLOAD_PATH/test-app.apks
```
* 주의사항 : 해당 앱이 기존에 받아져 있는 경우 에러가 납니다. 제거하고 진행해 주세요.


<br>
이때 adb관련 설정을 해주지 않으신 분들은 동작하지 않습니다.
<br>그런 경우에는 아래 설명을 통해 추가해 주시고 다시 진행해 주시면 되겠습니다.

<br>
bundletool숏컷을 추가했던 ./bash_profile 파일을 다시 열어주겠습니다.
<br>에디터가 열렸다면 아래 값을 입력하고 저장해 주세요.
(해당 환경설정값은 안드로이드 스튜디오 버전 3.0 기준으로 작성되었습니다.)

> export PATH=${PATH}:/Users/USER_NAME/library/android/sdk/tools:${PATH}:/Users/USER_NAME/library/android/sdk/platform-tools

<br>
에디터를 저장하고 종료합니다.

다시 돌아가 위에 사용했던 명령어를 입력해 주시면 연결되어있는 기기에 어플리케이션이 정상적으로 설치되는 것을 볼 수 있습니다. 또한 앱 설정에서 빌드된 앱 크기가 이전에 사용하던 앱 크기와 많이 차이가 나는 것도 보실 수 있습니다.

<br>모든 준비가 완료되었으니, 이제 앱 배포를 시작하시면 되겠습니다.

<br>
### App Bundle을 마치며..

안드로이드 스튜디오는 매년 새로운 기능들이 많이 추가되고 있습니다.
오늘 포스팅한 App Bundle은 여기에 쓰여 있는 것들 외에 굉장히 많은 기능들을 가지고 있습니다.
필요하다면 아래 공식 문서들을 통해 서비스에 필요한 기능들을 추가하실 수 있을 거라 생각합니다.

<br><br>
#### 참고 사이트

* <a href="https://developer.android.com/platform/technology/app-bundle">Android App Bundle</a>
* <a href="https://developer.android.com/studio/command-line/bundletool">bundletool</a>


<br><br><br><br>