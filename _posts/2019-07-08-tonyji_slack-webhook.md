---
layout: post
title: 슬랙에서 웹훅 연동하기
tags: [슬랙, 웹훅, slack, webhook]
author-id: momotony
excerpt_separator: <!--more-->
---

#### 들어가며

스타트업에서 신속하고 효율적인 커뮤니케이션은 아주 중요한 요소 중 하나입니다. 이번에 공유해 드릴 내용은 슬랙에서 제공하는 API 중에 웹훅(Webhook)을 활용한 알림 기능입니다. 이 기능은 외부 시스템에서 특정 작업을 실행한 후, 이를 슬랙을 통해 공유할 수 있도록 합니다.<br><!--more-->

기업에서 활용할 수 있는 많은 커뮤니케이션 툴 중에서 슬랙은 현재 많은 스타트업에서 활용 중인 활용도 높은 서비스로 기본적으로 무료로 제공됩니다 다만, 몇 가지 프리미엄 기능을 사용하기 위해서는 유료 서비스를 신청하셔야 합니다. 무료 서비스는 주고 받는 메시지의 최대 기록이 일정 수(현재 10,000개)로 제한되어 그 수를 넘게 되면 과거의 메시지들은 조회할 수 없게 됩니다. 또한, 파일 공유를 위한 스토리지 용량이 제한(현재는 5G)되는데 이 제한을 넘게되면 과거의 파일부터 기록에서 삭제됩니다. 이 특성만 고려한다면 무료로도 충분히 잘 활용할 수 있습니다.<br>

개인적으로 슬랙은 즉각적이거나 혹은 일정 기간동안만 유지되는 커뮤니케이션을 위해 활용하고, 기록으로 남기는 부분은 이메일, 지라(Jira), 트렐로(Trello), 구글 드라이브 등의 다른 서비스들을 활용하는게 좋다고 생각합니다.<br>

이제 본격적으로 슬랙의 웹훅 기능을 알아보도록 하겠습니다.
우선, 슬랙의 웹훅을 활용한 예를 들어 볼까요? 매일 아침 10시에 전날 서비스 가입자, 리텐션 등 각종 통계 정보를 슬랙을 통해 팀에게 공유한다면 어떨까요? 혹은 서비스에 크리티컬한 오류가 발생할 경우, 슬랙에 알림을 보내는 것도 유용하겠죠? 이처럼 슬랙의 웹훅 기능은 여러 방면에 다양하게 활용할 수 있습니다.
<br>

<b>[참고]</b>

> Webhook(웹훅)이란? 서버에서 특정 작업이 수행 되었을 때, 해당 작업이 수행되었음을 HTTP POST 방식으로 알리는 개념을 말합니다. 

<br>
이제부터 본격적으로 슬랙의 웹훅 기능을 구현해보도록 해보겠습니다.
<br>

먼저, 다음 링크를 통해서 슬랙 API 웹사이트에 접속합니다.<br>
> <a href="https://api.slack.com/incoming-webhooks" target="_blank">https://api.slack.com/incoming-webhooks</a>

<br>
화면 중앙의 “Create your Slack app” 버튼을 클릭해서 새로운 슬랙 앱을 생성합니다. 
<br><br>
![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-1.png" | relative_url}})
<br><br>
 
그런 다음 원하는 이름으로 “App Name”을 기술하고, “Development Slack Workspace”에 팀이 사용중인 workspace를 선택합니다. 기존에 사용하는 workspace가 없는 경우 새로 생성한 후에 작업을 계속하면 됩니다. 기입이 다 완료되면 하단의 "Create App" 버튼을 클릭합니다.<br>
<br>
![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-2.png" | relative_url}})
<br>
이후 슬랙을 위한 기능들을 추가하는 화면이 나오는데, 우리는 여기서 "Incoming Webhooks"를 선택합니다. 선택을 마치셨다면 하단의 "Save Changes"를 클릭합니다.
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-3.png" | relative_url}})

<br>

다음으로 웹훅 설정화면에서 “Activate Imcoming Webhook”를 “On” 상태로 바꿉니다. 
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-4.png" | relative_url}})

<br>

맨 하단의 "Add New Webhook to Workspace"를 클릭하면, 다음과 같이 알림을 받을 채널을 선택하는 화면이 뜹니다. 원하는 채널을 선택한 후 "Install" 버튼을 클릭하면 해당 채널로 웹훅 알림이 전달됩니다.
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-5.png" | relative_url}})

<br>

자, 이제 모든 설정을 완료했습니다. <br>

설정이 잘 되었는지 확인하기 위해, 화면 중앙의 "Sample curl request to post to a channel"에 나온 curl 실행 명령을 복사합니다. 
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-6.png" | relative_url}})

<br>
 터미널에서 복사한 curl 명령을 붙여넣기한 후 실행해보면 지정한 채널로 "Hello, World!"라는 텍스트가 발송되는 것을 확인하실 수 있습니다. 성공입니다!
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-7.png" | relative_url}})

<br>
우리는 "# general" 채널로 텍스트를 발송했으므로 아래와 같이 "# general" 채널로 "Hello, World!"라는 텍스트가 올라온 것을 볼 수 있습니다. 발송자는 맨 처음에 "App Name"으로 지정했던 "Super Service"로 나타납니다.
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-8.png" | relative_url}})

<br>
이제 슬랙 웹훅 사용을 위한 모든 준비가 끝났습니다. <br>
웹훅 설정 화면에서 아래 화면과 같이 "Webhook URL"에서 Copy 버튼을 클릭해서 복사해 두세요.
<br>

![Slack]({{ "/assets/img/tonyji/slack-webhook/screenshot-9.png" | relative_url}})

<br>

이제 남은 작업은 여러분이 원하는 정보를 생성하는 데몬 프로그램을 구현해서 원하는 주기로 위에서 복사하셨던 "Webhook URL"로 POST 방식으로 전송하기만 하면 됩니다. 
<br>
맨 처음에 예를 든 일간 서비스 가입자 통계 정보를 공유하는 방식의 경우, 서버에서 전날 가입자의 통계 정보를 추출한 후, 매일 아침 9시에 발송하는 데몬 프로그램을 구현해서 슬랙의 웹훅 URL로 POST로 전송하면 매일 아침 9시에 전날 가입자의 통계 정보를 확인할 수 있는 것입니다. 생각보다 유용한 기능인데 슬랙을 활용하니 참 쉽죠^^<br>

슬랙의 웹훅 기능은 정말 유용한 기능이에요. 아직 활용하지 않았던 스타트업이 있으면 적극적으로 활용해시길 권장드립니다.<br>

세상의 모든 스타트업을 응원합니다!<br>
감사합니다.<br><br>

