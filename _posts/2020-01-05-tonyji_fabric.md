---
layout: post
title: fabric으로 배포 스크립트 작성하기
tags: [fabric]
author-id: momotony
excerpt_separator: <!--more-->
---

#### 들어가며

오늘은 배포 자동화를 위한 유용한 툴 중에 원격 배포 스크립트 작성에 활용할 수 있는 fabric 라이브러리에 대해서 알아보려고 합니다.

fabric은 파이썬으로 스크립트를 작성해서 원격으로 SSH 접속 및 커맨드 라인 명령 실행, 파일 전송 등을 실행할 수 있게 해주는 파이썬 오픈 소스 라이브러리를 말합니다. fabric은 몇 가지 외부 라이브러리들과의 조합을 통해 구현되었습니다. 이 때문에 코딩시에 간혹 invoke나 paramiko 패키지로 부터 import 하기도 합니다. Invoke 라이브러리는 주로 쉘 커맨드 실행부분에 사용되고, Paramiko 라이브러리는 로우레벨 SSH 기능들을 구현하는 데 사용됩니다.


<br>
<!--more-->

#### 설치하기

fabric은 파이썬 기반으로 작성된 패키지이기 때문에 다음과 같이 pip으로 설치하시면 됩니다.
```
$ pip install fabric
```
<br>

#### 실행 테스트

fabric 설치를 완료했다면, 다음과 같이 테스트가 가능합니다.
```
$ python
>>> from fabric import Connection
>>> c = Connection(host='원격서버IP주소', user='유저명', port='포트번호')
>>> c.run('uname -a')

```

Connection 파라미터를 다음과 같이 한번에 입력해도 됩니다.
```
>>> c = Connection('유저명@IP주소:포트')
```

다음은 AWS 상에 설치된 인스턴스에 키파일을 사용해서 접속한 후 명령을 실행하는 예제입니다.
```
$ python
>>> from fabric import Connection
>>> c = Connection(host='IP주소', user='유저명', connect_kwargs={"key_filename":"로컬PC에 있는 키파일 전체 경로"})
>>> c.run('uname -a')
```

Connection 클래스의 connect_kwargs에 아마존 키파일('*.pem') 전체 경로를 기술하면 됩니다.

<br>

#### ssh config를 활용한 접속 간소화

원격 서버 접속시 위 테스트 예제와 같이 Connection 클래스에 host, user, key 파일 경로 등을 기술하는게 번거롭다면 다음과 같이 ssh config 설정을 통해서 간소화할 수 있습니다.

```
$ vi ~/.ssh/config
Host 서버닉네임
User 접속계정 유저명
HostName 서버IP주소
IdentityFile 로컬 키파일 전체 경로
```

작성예) IP주소가 123.456.789.123이고, 키파일이 /Users/momotony/aws/keys/test.pem인 경우

```
Host dev1
	User ec2-user
	HostName 123.456.789.123
	IdentityFile /Users/momotony/aws/keys/test.pem
```

위와 같이 설정 파일을 추가하고 나면, 아래와 같이 원격서버에 접속이 가능해집니다.

```
$ ssh dev1
```

자, 이제 fabric으로 다시 한번 원격 접속을 해보시죠.
이번엔 Connection 클래스에 host 파라미터만 기술하면 됩니다. 이때, ssh config의 Host 필드에 기술한 서버닉네임을 전달하면 됩니다.
```
$ python
>>> from fabric import Connection
>>> c = Connection(host="dev1")
>>> c.run('uname -a')
```

<br>

#### 배포 스크립트 작성

이제 모든 준비가 끝났습니다.
그럼, 본격적으로 배포 스크립트를 작성해 볼까요?

```
$ vi fabfile.py

from fabric import Connection

c = Connection(host='dev1')

def build():
	with cd('/home/ec2/my-application'):
		run('ls -al')

def applyChanges():
	with cd('/home/ec2/my-application'):
		run('git pull')

def start():
	with cd('/home/ec2/my-application'):
		run('uwsgi uwsgi.ini') # uwsgi instance start

def stop():
	with cd('/home/ec2/my-application'):
		run('uwsgi --stop /tmp/uwsgi.pid') # uwsgi instance stop

def reload():
	with cd('/home/ec2/my-application'):
		run('uwsgi --reload /uwsgi.pid') # uwsgi reload

def deploy():
	#execute(applyChanges)
	execute(build)
	#execute(stop)
	#execute(copy)
	#execute(start)
```

fabric에서 사용하는 주요 명령어에 대해 알아보겠습니다.

#### fabric 주요 명령어
* with cd(경로) : 경로에 해당하는 디렉토리를 기반으로 명령어 실행
* run(명령어) : 원격 서버에 명령어 실행
* local(명령어) : 로컬 서버에 명령어 실행
* sudo(명령어) : 원격 서버에 루트 권한으로 명령어 실행
* put(경로) : 로컬 서버의 파일을 원격 서버로 전송
* get(경로) : 원격 서버의 파일을 로컬 서버로 전송
* execute(명령어) : 명령 실행

<br>

#### python2.x 에러처리

python2.x에서 아래와 같은 에러가 발생할 경우, 다음과 같이 대처하시면 됩니다.
```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "<decorator-gen-3>", line 2, in run
  File "/usr/local/lib/python2.7/site-packages/fabric/connection.py", line 29, in opens
    self.open()
  File "/usr/local/lib/python2.7/site-packages/fabric/connection.py", line 615, in open
    self.client.connect(**kwargs)
  File "/usr/local/lib/python2.7/site-packages/paramiko/client.py", line 406, in connect
    t.start_client(timeout=timeout)
  File "/usr/local/lib/python2.7/site-packages/paramiko/transport.py", line 660, in start_client
    raise e
AttributeError: Raw
```

cryptography 버전이 2.6 이상에서 발생함 => 2.5이하로 낮춤<br>
(Ref: https://github.com/paramiko/paramiko/issues/1472)
```
$ pip show cryptography
$ pip install cryptography==2.5
```

이후, 아래와 같은 에러가 발생한다면,
```
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/local/lib/python2.7/site-packages/fabric/__init__.py", line 3, in <module>
    from .connection import Config, Connection
  File "/usr/local/lib/python2.7/site-packages/fabric/connection.py", line 16, in <module>
    from paramiko.agent import AgentRequestHandler
  File "/usr/local/lib/python2.7/site-packages/paramiko/__init__.py", line 22, in <module>
    from paramiko.transport import SecurityOptions, Transport
  File "/usr/local/lib/python2.7/site-packages/paramiko/transport.py", line 34, in <module>
    from cryptography.hazmat.primitives.ciphers import algorithms, Cipher, modes
  File "/usr/local/lib/python2.7/site-packages/cryptography/hazmat/primitives/ciphers/__init__.py", line 7, in <module>
    from cryptography.hazmat.primitives.ciphers.base import (
  File "/usr/local/lib/python2.7/site-packages/cryptography/hazmat/primitives/ciphers/base.py", line 12, in <module>
    from cryptography.exceptions import (
  File "/usr/local/lib/python2.7/site-packages/cryptography/exceptions.py", line 7, in <module>
    from enum import Enum
  ImportError: cannot import name Enum
```

다음과 같이 enum34를 재설치 해주세요.
```
$ pip uninstall enum34
$ pip install enum34
```

더 자세한 내용은 아래 레퍼런스를 참고해 주세요

<br>

#### 레퍼런스

* 공식 홈페이지 : https://www.fabfile.org/
* API Documentations : http://docs.fabfile.org/

<br>

#### 맺음말

이상과 같이 fabric을 활용한 원격 배포 스크립트 작성을 마쳤습니다.
다음 시간엔 자동 테스트 환경을 포함한 배포 자동화 구현에 대해서 알아보도록 하겠습니다.

<br>
