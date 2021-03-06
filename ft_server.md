지식 전무한 상황에서 시작했던 ft_server...

## ft_server
- 시스템 관리 개념을 소개하기 위한 과제
- Docker 기술을 학습하고 완전한 웹 서버 구축
- 추후 ft_service와 이어지는 부분이 있다.

### ft_server 서브젝트 설명
- LEMP 스택 : 동적 웹 어플리케이션을 구현하기 위해 필요한 Linux + Nginx + MySQL + PHP을 모아서 부르는 단어인데 서브젝트에서 이걸 요구한다.
- Docker container 안에 __Nginx__ 웹 서버를 설치할 것
- container의 OS는 __Debian Buster__ 일 것
- 연동해야할 서비스
  - wordpress
  - phpMyAdmin
  - MySQL
- SSL 프로토콜을 사용해야 한다
- URL redirection
- autoindex


+ 알아듣기 쉽게 설명..
###📚 전반적인 설명
<img src= "https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjYbCv%2FbtqC4t6LHE2%2FWqu39thIjmjhnvnqzh6Ck0%2Fimg.jpg">
####1. 도커
  - __설명__ : 기존의 하드웨어 스택을 가상화하는 가상 머신과 다르게, 컨테이너 방식은 운영체제 수준에서 가상화를 실시하고 다수의 컨테이너는 OS 커널에서 직접구동한다. 컨테이너를 통해 프로세스를 격리해서 실행하고 관리하며, 효율적으로 이미지(=특정 프로세스의 실행환경, 여기서는 웹서버)를 구축할 수 있도록 해준다.
  - __장점__ : 이러한 방식은 시작이 훨씬 빠르고 운영체제 전체 부팅보다 메모리를 훨씬 적게 차지한다. 이러한 실행환경을 
  - __요약__ : 쉽게 말해 특정 프로세스의 실행환경, 이 과제에서는 웹서버를 효율적으로 저장하고 관리할 수 있는 플랫폼.
  - 많은 설정과 업데이트 등의 과정을 생략하고 원하는 환경 설정을 위해 한 번에 자동화해주는 도구

__1.1 컨테이너__
- 가상화 기술의 하나로 격리된 공간에서 프로세스를 동작하는 기술
- 여러 개의 컨테이너(서버 등의 프로세스 실행환경)를 하나의 기기에서 실행시킬 수 있으면서 메모리를 덜 먹는 방식이 된다.

__1.2 이미지__
- 이미지는 컨테이너 실행에 필요한 파일과 설정값 등을 포함하고 있는 것.
- 컨테이너를 실행한 상태가 이미지이며, 추가되거나 변하는 값은 컨테이너에 저장된다.
- 한 이미지에서 여러 컨테이너 생성이 가능하다.
- __요약__ : 따라서, 실행환경을 구축함에 있어서 새로 뭐 이것저것 설치할 것 없이 한 번 만들어 놓은 이미지로 컨테이너를 생성해서 서버에서 추가하면 되는 것!

### 2. 데비안 (Debian Buster)
- 우분투와 같은 리눅스 OS 종류 중 하나.(= 서브젝트에서 사용하라 명시)
- 안정성을 중시하는 OS라 많이 웹 서버 등에 많이 사용한다고 한다.

### 3. Nginx
- 엔진엑스는 무료로 제공되는 오픈소스 웹 서버 프로그램
- 웹 서버는 클라이언트로부터 요청이 발생했을 때 요청에 맞는 정적콘테츠를 보내주는 역할
- http의 웹 서버의 역할, proxy 서버의 역할도 가능
- 엔진엑스 서버에 SSL, autoindex 등 설치
  - 웹 서버가 하는 일
  1. 커넥션을 맺는다
  2. 요청을 받는다
  3. 요청을 처리한다
  4. 리소스에 접근한다
  5. 응답을 만든다
  6. 응답을 보낸다
  7. 트랙잭션을 로그로 남긴다

### * SSL(Secure Socket Layer)
- HTTPS(Hypertext Transfer Protocol over Secure Socket Layer)는 SSL위에서 돌아가는 HTTP의 평문 전송 대신에 암호화된 통신을 하는 프로토콜이다.
- 이런 HTTPS 통신을 서버에서 구현하기 위해서는 신뢰할 수 있는 상위기업이 발급한 인증서가 필요하며 이런 기관을 CA(Certificate authority)라고 한다.
- 인터넷 상에서 웹 브라우저와 웹 서버간에 데이터를 안전하게 주고 받기 위해 서로 암호화하여 통신한다. SSL 인증서는 사용자의 웹 브라우저와 인터넷 사이트의 웹 서버 사이 암호화 통신을 가능하게 하는 프로토콜.
- self-signed SSL 인증서는 자체적으로 발급받은 인증서이며, 로그인 및 기타 개인 계정 인증정보를 암호화한다. 브라우저는 신뢰할 수 없다고 판단해 접속시 보안 경고가 발생
- https를 위해 필요한 개인키(.key), 서면요청파일(.csr), 인증서파일(.crt)를 openssl로 발급한다.
- __OPEENSSL__ 
  - ssl 프로토콜을 위한 도구로, 이 프로그램을 사용해 인증서에 필요한 키를 생성하고 스스로 사인한 인증서를 만들 수 있다.
### 4. MySQL
- 오픈소스 관계형 데이터베이스 관리 시스템
- SQL은 쉽게 말해 웹사이트에서 데이터베이스 저장된 여러 데이터들을 조작하고 조회하는 걸 가능하게 해주는 프로그래밍언어 (ex. 로그인 기능 활성화를 위해 비밀번호가 담긴 데이버테이스의 데이터를 관리)
- 관계형 데이터베이스 쉽게 말해 테이블 형태로 관리되는 데이터의 집합
- 같은 종류의 관계성으로 연결해두고 쉽게 접근할 수 있게 하는 느낌(분류?)
- 결론적으로 MYSQL은 관계형 데이터베이스를 생성하고 관리할 수 있도록 도와주는 프로그램이라고 한다.
- 데이터베이스를 효율적으로 관리해주는 시스템

### 5. phpMyAdmin
- php를 기반으로 생성된 __MYSQL의 GUI로서__ 웹에서 실행할 수 있는 프로그램
  - __php : 서버 사이드 스크립트 언어__
- 데이터베이스를 GUI로 편하게 관리할 수 있도록 도와주는 서비스
- CGI란 웹 서버에서 요청을 받아 외부 프로그램에 이를 넘겨주면, 외부 프로그램이 그 파일을 읽어 html 문서로 변환하는 단계를 거치는 것을 의미한다. 즉, 웹 서버와 외부 프로그램 사이에서 정보를 주고받는 방법이나 규약.
- __```php-fpm```은 CGI보다 빠른 버전의 관리도구로 php-fpm을 통해 nginx와 php를 연동시켜 우리의 웹 서버가 정적 콘텐츠 뿐만 아니라 동적 콘테츠를 다룰 수 있도록 만드는 것__

### 6. Wordpress
- WordPress는 CMS(Content Management System)의 일종이다.
- 말 그대로 컨테츠를 관리하는 시스템
- 프론트엔드를 담당한다고 생각하면 된다. 설정, 변경, 컨텐츠 업로드 및 수정 삭제 등의 기능을 지원
#### 7. Docker Hub?
- 도커 이밎의 용량이 크기에 Docker hub가 무료로 관리
- 공개된 이미지를 다운받거나 저장소를 직접만들어 관리가 가능하다
#### 8. Dockerfile?
- Dockerfile은 필요한 최소한의 패키지를 설치하고 동작하기 위한 자신만의 설정을 담은 파일.
이 파일로 이미지를 빌드하게 된다.
- 패키지 설치, 환경 변수 변경, 설정 파일 변경등의 다양한 작업을 컨테이너를 만들고 설정을 적용할 필요없이 dockerfile을 통해 적용 가능.

#### ft_server의 구현
0. Docker & Docker hub
    - docker login
    - docker ps -a : 종료된 컨테이너 확인
    - docker commit [Container ID] [image name[:tag name]] : 현재 컨테이너에서 도커 이미지 만들기
    - docker images : 저장된 이미지 확인
    - docker tag [이미지 id] [레포지토리 id/레포지토리명 : 태그] : 깃 커밋과 비슷한 개념
    - docker push [레포지토리 id/레포지토리명:태그] : 도커허브에 푸쉬
    - docker start [컨테이너 id]
    - docker attach [컨테이너 id]
<br>
1. 도커로 데비안 버스터 이미지 만들기
    - docker pull debain:buster
    - debain:buster 이미지를 가져온다 
    - 데비안 버스터 환경을 불러와서 작업 후 새 컨테이너를 생성
    - docker images
      - 불러온 이미지 확인
<br>
2. 도커로 데비안 버스터 환경에 들어가기
     - docker run -it -p 80:80 -p 443:443 debian:buster
       - ```-i``` : 표준 입력을 유지. host 컴퓨터 운영체제의 쉘이 아닌, 컨테이너의 쉘에 명령을 입력하겠다는 의미
       - ```-t``` : TTY 활성화. 터미널과 상호작용하는 tty라는 콘솔의 한 종류를 연다는 의미.
       - ```-p``` : publish의 약자로, 사용하고자 하는 포트 번호를 가리킨다.
       - 80은 http의 기본 포트, https의 기본 포트는 443
<br>
3. nginx 설치 및 서버 연결 확인
     - 패키지 목록을 최신으로 업데이트, 업글이드(apt-get update&upgrade) 후 nginx 설치(apt-get install nginx)
     - service nginx start, service nginx status
     - localhost의 접속 확인
     - 서버 블록은 지정된 도메인의 포트에서 드러오는 연결을 수신
<br>
1. Openssl로 selg-signed SSL 생성
     - https를 위해 필요한 개인키(.key), 서면요청파일(.csr), 인증서파일(.crt)를 openssl로 발급한다.
     - apt-get install openssl
     - ```openssl req -newkey rsa:4096 -days 365 -nodes -x509 -subf "/C..."```
       - ```openssl req -newkey``` : newkey를 요청. crt 생성한다.
       - ```-days``` : 유효한 일 수
       - ```-nodes``` : 리부팅 시 암호 입력 필요없게 해준다.
       - ```x509``` : 자체 서명된 인증서 출력 옵션
    - 생성된 .crt와 .key를 etc/ssl/certs/와 etc/ssl/private/폴더로 옮긴 후 권한 설정 600
<br>
5. nginx에 ssl을 더하기 위한 default 파일 설정 변경
    - 443 포트에 ssl 관련 사항들을 수정하여 적용시키면 https로 localhost를 열어볼 수 있다.
<br>
6. php-fpm 설치
      - apt-get -y install php-fpm
      - php 사용을 위한 default파일 내용 수정(주석 블럭 해제)
<br>
7. mysql(MariaDB) 및 phpmyadmin 설치
8. phpmyadim 설정
      - 쿠키 권한을 위한 blowfish 암호 설정
        - config.sample.inc.php 파일을 복사해 config.inc.php를 만든 후 블로피시 암호를 만들어 넣는다.
      - phmyadimn을 위한 DB테이블 생성
        - create_table.sql 파일을 mysql로 리다이렉션 시켜준다.
<br>
9. wordpress 설치 및 설정
    - wp-config.php에 설정 수정
<br>
10. autoindex 추가
11. url redirection : http에서 https로 리다이렉션하게 변경



## 출처 및 참조
>- https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcjYbCv%2FbtqC4t6LHE2%2FWqu39thIjmjhnvnqzh6Ck0%2Fimg.jpg (이미지 출처 1)
>- https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html (도커란 무엇인가)
>- https://nirsa.tistory.com/63 (도커파일 개념, 명령어, 이미지빌드)
>- https://velog.io/@hidaehyunlee/ftserver-%EC%84%A0%ED%96%89%EC%A7%80%EC%8B%9D-Docker-Debian-Buster-Nginx- (hidaehyunlee)
>- https://earthkingman.tistory.com/108 (하루코딩일기)
>- https://watermelonlike.tistory.com/115 (수박수님)
>- https://yeosong1.github.io/ft_server.html (yeosong님)
>- https://velog.io/@chaewonkang/ftserver-1%EC%9D%BC%EC%B0%A8 (ckang님)