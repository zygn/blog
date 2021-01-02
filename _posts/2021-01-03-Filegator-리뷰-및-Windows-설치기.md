---
title: Filegator 리뷰 및 Windows 설치기
updated: 2021/01/03
---
### Filegator 리뷰 및 Windows 설치기

![filegator](https://raw.githubusercontent.com/filegator/filegator/master/dist/img/logo.gif)

#### 발단

학교 연구실에서 프린터 서버 위에 파일서버를 구성하려다가, SMB는 보안이슈 (랜섬웨어 때문에 말이 많기도 하고.. 3.0에서도 취약점 발견), [HFS](http://www.rejetto.com/hfs/)는 뭔가 안이쁘기도 하고... 베리즈는 배포 중단하지가 어연 10년이 다되어 가는데다가 하다못해 VirtualBox를 사용해서 헤놀로지를 올리려니까 나중에 문제 생길것 같고... 하면서 Github를 뒤적거리다가 발견한것이 [Filegator](https://docs.filegator.io/) 되시겠다.

#### 환경

우선 연구실 프린터 서버 환경은 다음과 같다.
- Windows 10 x64
- AMD Six-core CPU (1055T)
- SSD 128GB, HDD 2TB
- 1Gbps x1
- 삼성 복합기 한대

원래는 Ubuntu 18.04 LTS 를 올려서 사용하다가, 평생 윈도우만 써서 우분투를 쓸줄 모르는 사람 
도(결국 다 쓰더라 ㅋㅋ) 있었고, 복합기 스캐너 잡는게 너무 귀찮아서 그냥 윈도우 올려서 사용하고 있는 실정이다.

#### Filegator 는 뭐길래?

Filegator는 PHP 기반의 파일서버 라고 할 수 있겠다. 구성은 매우 간단해서, DBMS도 사용하지 않는다. (무척 마음에 든다) 그냥 압축파일 받아서 바인딩만 해주면 끝. 업로드, 다운로드, 유저 설정은 기본이고, 한글도 지원한다. 더군다나 우리 연구실은 데이터 무결성이 필요한것도 아니며, Critical 하게 사용할 것도 아니라(그냥 학교 외부망이 더럽게 느려서 용량이 좀 나가는거 넣어놓는 용도일뿐...) 더더욱 맘에 들더라. 

#### 근데 설치 방법 보니까 Debian (Ubuntu) 이던데?

그렇다. 설치방법은 Debian (Ubuntu)를 기준으로 명세해놔서 조금 난해 할 수 있는데, 하지만 PHP랑 Apache는 Windows 환경에서도 잘 돌아간다. **그럼 뭐 되지 않겠는가?** 애초에 OS-driven 한 패키지도 아니고. Apache+PHP만 설치하면 되겠지? 라는 생각으로 설치해봤다.

![설치된 스크린샷](https://raw.githubusercontent.com/zygn/blog/master/_posts/img/2021-01-03-Filegator-%EB%A6%AC%EB%B7%B0-%EB%B0%8F-Windows-%EC%84%A4%EC%B9%98%EA%B8%B0/img01.jpg)

Timber! 잘 작동 하는것을 볼수 있다.

#### 어떻게 설치 하지?

필요한 프로그램 및 파일은 다음과 같다.

- [XAMPP](https://www.apachefriends.org/download.html) (Apache + PHP 7.4.13)
- [filegator](https://docs.filegator.io/install.html) (latest)

설치법은 되게 간단했다. XAMPP 설치는 상당히 간단하니 스킵하도록 하고 (구글링하면 10분컷이다.), filegator 설치하는 방법을 설명하도록 한다.

1. filegator 사이트를 들어가서 [Download](https://github.com/filegator/static/raw/master/builds/filegator_v7.4.6.zip)를 한다. (기준 7.4.6 버전, 아마 PHP 버전 따라가는듯?)
2. 원하는 폴더에다 압축을 해제 한다. (본인은 D:/www에 했음)
3. 그리고 XAMPP Control Panel에서 Config을 수정해준다.
   ![XAMPP 컨트롤 패널](https://raw.githubusercontent.com/zygn/blog/master/_posts/img/2021-01-03-Filegator-%EB%A6%AC%EB%B7%B0-%EB%B0%8F-Windows-%EC%84%A4%EC%B9%98%EA%B8%B0/img02.jpg)
   1. Config 탭에서 **Apache(httpd.conf)** 를 눌러 수정에 들어간다.
   ![httpd.conf 수정](https://raw.githubusercontent.com/zygn/blog/master/_posts/img/2021-01-03-Filegator-%EB%A6%AC%EB%B7%B0-%EB%B0%8F-Windows-%EC%84%A4%EC%B9%98%EA%B8%B0/img03.jpg)
   1. 원래있던 **DocumentRoot**와 **Directory**를 주석 처리하고 위의 스크린샷 처럼 **DocumentRoot "본인이_압축푼_경로)/dist"**, **<Directory "본인이_압축푼_경로)/dist>"** 로 수정하고 저장한다.
   2. 그러고 XAMPP Control Panel에서 Apache를 재시작 해주면 적용완료!
4.  **설치완료!**

그리고, 초기 설정을 위해 admin/admin123로 로그인 하면 접속이 가능하다.

#### 한글... 한글을 주시오...

한글이나, 파일 업로드 크기 같은 설정은 파일을 직접 수정해야한다. 
**본인이_압축푼_경로**를 들어가서 **configuration.php** 파일을 손봐주자.

![설정](https://raw.githubusercontent.com/zygn/blog/master/_posts/img/2021-01-03-Filegator-%EB%A6%AC%EB%B7%B0-%EB%B0%8F-Windows-%EC%84%A4%EC%B9%98%EA%B8%B0/img04.jpg)

13번 줄의 language를 korean으로 설정하면 한글이 적용되고,
15번, 16번 줄을 수정하면 업로드 가능한 파일 크기를 설정 할 수 있다. 청크 사이즈는 업로드 크기에 따라 가변적으로 설정하면 되겠다. (본인은 업로드 크기만 설정하고 청크사이즈는 그냥 냅뒀다.)

#### 며칠 써본 결과

업로드가 그렇게 빠르진 않다. 그래도 쓸만한듯? 연구실 사람들도 그럭저럭 잘 쓰고 있는것 같기도 하니. 문제만 안생기면 그만이겠다.