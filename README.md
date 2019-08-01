### 개발환경구성(MacOS)

1.ruby가 설치되어있는지 확인

>$ ruby -v

1-1. 설치가 안되어있으면 설치

>$ brew install ruby

2.bundle & jekyll설치

>$ sudo gem install bundler jekyll

3.블로그 프로젝트 클론 (적절한 위치에서 ex) ~/document/blog )

>$ git clone https://github.com/atommerce/atommerce.github.io.git

4.프로젝트 폴더로 이동

>$ cd atommerce.github.io

5.프로젝트에 필요한 모듈 설치

>$ bundler install

6.프로젝트 시작(성공시 localhost:4000으로 서비스 됨)

>$ bundler exec jekyll serve

### 포스트 작성

1.프로젝트 폴더 안 _posts폴더안 에 “날짜_제목.md”  파일 생성 (다른 포스트 참조)

2.프로젝트 저장(프로젝트 폴더에서)

>$ git add .
>$ git commit -m “커밋메세지 ex)레이아웃 포스트 완성”

3.최신 프로젝트 가져오기

>$ git pull origin master

4.프로젝트 배포

>$ git push origin master