---
layout: post
title: MySQL에서 이모지 사용하기
tags: [utf8, utf8mb4, mysql, 유니코드, 이모지, UTF-8]
author-id: momotony
excerpt_separator: <!--more-->
---

<br>
올해 초 각고의 노력 끝에 아토머스의 오랜 숙원 사업인 '마인드카페 프로'를 런칭하게 되었습니다.<br>
마인드카페 프로는 기존의 오프라인 대면 심리 상담 방식을 온라인 익명 채팅 및 전화상담으로 구현한 서비스입니다.<br>
오늘은 마인드카페 프로의 기능 중 채팅 상담을 구현하면서 겪은 이슈인 이모지 사용을 위한 MySQL 인코딩 설정 방법에 대해 말씀드리려고 합니다.<br><br>
<!--more-->

사실 필자는 오랜 기간동안 백엔드 프로그래밍을 해오면서, MySQL에서 다국어 지원을 위해서는 'utf8' 을 사용해야 한다고 알고 있었습니다.<br>
그래서, 항상 기본 인코딩으로 'utf8'로 사용해 왔습니다. <br>
그런데, 채팅에서 이모지 사용을 테스트하던 중 이모지가 제대로 표현되지 않는 오류를 발견하고 이를 분석하던 중 새로운 사실을 알게 되어 공유드리려고 합니다.
<br><br>

본론에 들어가기 전에 우선 인코딩에 대해서 간략하게 정리해 보도록 하겠습니다.
<br><br>

#### 히스토리

컴퓨터에서 문자를 표시하기 위해 각각의 문자에 수치화된 코드값을 부여한 것을 '문자 집합'이라고 합니다.<br>
그리고, 이 문자 집합을 컴퓨터가 이해할 수 있도록 비트 단위로 변환하는 작업을 '인코딩'이라고 합니다.
<br><br>

컴퓨터가 발명된 후, 각 나라마다 자국의 언어에 맞는 문자 집합을 정의해서 사용해왔는데, <br>
이러다 보니 인코딩 방식이 서로 달라서 여러 국가의 언어를 동시에 표시하는데 문제가 발생하게 됩니다.
<br><br>
이에 따라, 전 세계 모든 문자에 대한 표준 문자 집합인 '유니코드'가 탄생하게 되었습니다. <br>
유니코드에 대한 자세한 설명은 <a href="https://ko.wikipedia.org/wiki/%EC%9C%A0%EB%8B%88%EC%BD%94%EB%93%9C">위키백과의 설명</a>을 참고하세요.
<br><br>

유니코드의 주요 인코딩 방식에는 UTF-8, UTF-16, UTF-32등이 있습니다.<br>
이 중 UTF-16은 기본 다국어 평면(BMP, Basic multilingual plane)에 속하는 기본 문자들은 16비트(즉, 2바이트)로 그 이상의 문자는 32비트로 인코딩하는 방식을 말합니다.<br> 
UTF-32는 모든 문자를 32비트(즉, 4바이트)의 고정된 크기로 인코딩하도록 정의되어 있습니다. <br>
그 중, UTF-8은 문자에 따라 8비트부터 32비트(즉, 1바이트~4바이트)까지 표현되는 가변 길이 문자 인코딩 방식으로 ASCII 코드와 호환되는 동시에 유니코드를 표현할 수 있어 가장 많이 사용된다고 합니다.<br><br>

그렇다면, MySQL 'utf8'이라는 인코딩이 무엇을 의미하는지 부터 알아보겠습니다.<br>
MySQL에서 유니코드에 대한 지원은 2003년에 발표된 <a href="http://mysql.localhost.net.ar/doc/refman/4.1/en/news-4-1-0.html">MySQL 4.1 버전</a>부터이며, 위에서 설명드린 유니코드의 표준 인코딩 방식 중 UTF-8 방식을 'utf8'이라는 명칭으로 지원하면서 시작되었습니다.
<br><br>

그런데, 문제는 처음 MySQL에서 'utf8'방식을 내놓은 시기가 현재의 UTF-8 표준인 <a href="https://tools.ietf.org/html/rfc3629">RFC 3629</a>가 제정되기 이전이어서, 당시의 UTF-8 표준인 <a href="https://www.ietf.org/rfc/rfc2279.txt">RFC 2279</a>를 바탕으로 개발이 진행되었다는 것입니다. <br><br>

RFC 2279에서는 한 문자당 6바이트를 할당하는 방식이었고, MySQL 개발자들은 (정확한 이유는 밝혀지지 않았지만) 3바이트까지만으로 제한한 방식으로 'utf8'을 구현한 것 입니다. <br>
이에 대한 내용을 분석한 <a href="https://medium.com/@adamhooper/in-mysql-never-use-utf8-use-utf8mb4-11761243e434">다음 블로그</a>에서 꽤 설득력 있는 원인을 찾을 수 있습니다.<br><br>

MySQL에서는 그 이후에도 별다른 공지 없이 UTF-8을 'utf8'로 사용하면 된다는 식으로 가이드를 제공해왔습니다. <br>
따라서, 많은 개발자가 별다른 고민없이 이를 따르게 됐어요.<br><br>

그러던 중, MySQL에서는 2010년에 현재 표준에 맞게 4바이트를 모두 지원하는 인코딩 방식인 'utf8mb4'가 포함된 <a href="https://dev.mysql.com/doc/relnotes/mysql/5.5/en/news-5-5-3.html">MySQL 5.5.3 버전</a>을 공식 발표하게 됩니다.<br><br>

상식적으로 'utf8'은 UTF-8을 완전히 지원하지 않기 때문에 이를 완전히 지원하는 'utf8mb4'를 사용하도록 충분한 가이드가 제공되었어야한다고 보는데, MySQL은 그러질 않았어요.<br>
그래서, 현재까지도 많은 개발자가 MySQL에서 UTF-8을 'utf8'로 잘못 인식해서 이모지와 같은 일부 유니코드 문자들이 제대로 표시되지 않는 황당한 오류를 겪게 되는 것이죠. 
<br><br>

자, 이제 정리가 되셨죠? 그렇다면 이제 본격적으로 기존의 utf8 인코딩을 utf8mb4로 바꾸는 방법을 알아보도록 하시죠.
<br><br>

#### utf8mb4 설정하기

우선, MySQL의 my.cnf 설정파일을 다음과 같이 수정합니다.<br>
 
 > $ sudo vi /etc/my.cnf

<br><br>

{% highlight ruby %}
   [client]
   default-character-set = utf8mb4
 
   [mysqld]
   character-set-server = utf8mb4
   collation-server = utf8mb4_unicode_ci
 
   [mysql]
   default-character-set = utf8mb4
{% endhighlight %}

<br><br>

설정을 변경 한 후에는 다음과 같이 MySQL 서버를 재시작 시켜주세요.<br>

> $ sudo service mysqld restart

<br><br>

그런 다음, 다음과 같이 MySQL 터미널로 접속해서 변수들이 잘 변경되었는지 확인해 봅니다. <br>

> $ mysql -uroot -p
> mysql> SHOW VARIABLES LIKE 'char%'; SHOW VARIABLES LIKE 'collation%';

<br><br>

![MySQL Variables]({{ "/assets/img/tonyji/utf8mb4/mysql_show_variables.png" | relative_url}})

<br><br>

위와 같이 보이신다면 잘 변경된 것입니다. 이때, character_set_system은 'utf8'로 표시되는 것이 정상입니다.
<br><br>

#### 업데이트를 위한 체크 사항

기존 테이블을 업데이트하려면 다음 사항을 고려하셔야 합니다.<br>
기본적으로 utf8mb4는 utf8과 100% 호환되기 때문에 문자가 깨질 염려는 하지 않으셔도 됩니다. <br>
다만, 최대 3바이트였던 자릿수가 최대 4바이트로 늘어나면서 칼럼의 최대 길이와 인덱스 키의 길이를 체크해야 합니다.
<br><br>

예를 들어, TINYTEXT 칼럼은 255바이트까지 저장할 수 있는데,  utf8인 경우 3바이트 문자 기준 85개, utf8mb4의 경우 4바이트 문자 기준 63개를 저장할 수 있습니다.<br>
따라서, 기존에 TINYTEXT 칼럼을 사용한다면 최대 입력 문자 수를 줄이거나 더 많은 바이트를 저장할 수 있는 TEXT와 같은 필드로 자료형을 수정해야겠죠. 
<br><br>

인덱스의 경우도 MySQL은 문자열 인덱싱에 prefix indexing이라고 해서 문자열의 앞에서부터 일정 부분만 인덱싱을 합니다<br>
InnoDB 스토리지 엔진은 최대 767바이트까지만 인덱스를 저장할 수 있는데, 이를 3바이트(utf8인 경우)으로 나누면 255자, 4바이트(utf8mb4)로 나누면 191자까지만 인덱싱이 가능하다는 의미입니다. <br>
이 경우, VARCHAR(255)로 정의된 필드에 인덱스가 걸려있었다면, 이 필드 전체에 인덱스가 적용되기 위해서는 VARCHAR(191)로 수정해야 합니다. 
<br><br>

위와 같은 체크가 완료되었다면 다음과 같이 테이블을 업데이트시켜 주시면 됩니다.
<br><br>

> mysql> use 데이터베이스명
> mysql> ALTER TABLE 테이블명 CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;

<br><br>

참고로 파이썬에서 SQLAlchemy를 사용하실때 utf8mb4 사용을 위한 설정은 다음과 같습니다.
<br><br>

> $ vi config.py
> SQLALCHEMY_DATABASE_URI=mysql+mysqlconnector://{username}:{password}@{hostname}/{databasename}?charset=utf8mb4

<br><br>

여기까지 잘 따라오셨다면, 이제부터 이모지 문자가 잘 저장되는 것을 확인하실 수 있으실 겁니다.<br>
프로그래밍의 세계에서 인코딩은 난해한 영역에 해당합니다.<br>
따라서, 이 부분을 더 많이 리서치하고 디테일하게 파헤쳐보면 좀 더 고급 엔지니어로 한 걸음 더 다가설 수 있습니다.<br><br>

더 깊은 내용을 공부하고 싶으신 분들을 위해 아래 참고 링크를 정리했습니다.<br>
시간 되실 때 정독하시면 좋을 것 같네요.<br><br>
감사합니다.<br>

<br>
<br>
#### 참고 자료

* <a href="https://en.wikipedia.org/wiki/Unicode">유니코드 설명(출처:위키백과)</a>
* <a href="https://en.wikipedia.org/wiki/UTF-8">UTF-8 설명(출처:위키백과)</a>
* <a href="https://d2.naver.com/helloworld/19187">한글 인코딩의 이해(출처:네이버D2 기술블로그)</a>
* <a href="https://mathiasbynens.be/notes/mysql-utf8mb4">How to support full Unicode in MySQL databases</a>
<br><br>


