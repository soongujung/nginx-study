# 참고자료, 주교재

[Nginx HTTP 서버](http://www.yes24.com/Product/Goods/89607082)



# Chapter01. Nginx 설치하기

nginx 를 설치하는 방식은 두가지 방식이 있다.  

- 패키지 매니저 (apt-get, yum, yast (수세suse 리눅스), Aptitude) 를 이용한 방식 

- - apt-get install nginx  
  - yum install nginx  

- 엔진엑스 소스코드를 다운로드해서 컴파일하여 설치하는 방식  

   

여기서 정리할 내용은 위의 두가지 방식 중 **엔진엑스 소스코드를 직접 다운로드하여 컴파일해 설치하는 방식**이다. 이렇게 직접 다운로드해 컴파일해서 설치하면, 아래의 장점이 있다.  

  

- 배포본을 구하기 어려울 때 직접 다운로드 받아 설치해버린다.  

- - 사용 중인 서버 머신의 리눅스 배포판의 리눅스 배포본 저장소에 Nginx 패키지가 없을 경우 직접 컴파일해 설치할 수도 있다.  

- 업그레이드 또는 버전 변경 등이 필요할 때  

- - 또는 Nginx 를 원하는 버전으로 업그레이드 해서 사용할 경우 굳이 패키지 매니저에 등록되기를 기다리기 보다는 직접 소스코드를 다운로드 받아서 원하는 버전의 프로그램을 컴파일해 설치하면 되기 때문이다.  

- 컴파일 옵션을 다양하게 활용해서 엔진엑스를 구성할 수 있다는 장점  

  

설치과정에서 필요한 패키지들은 아래와 같다. 아래의 패키지들을 설치한 후에 Ngnix를 컴파일할 수 있다.

- GCC

- PCRE (Perl Compatible Regular Expression), 펄 호환 정규 표현식

- zlib 라이브러리

- - 압축 알고리즘을 개발자에게 제공한다.

- OpenSSL

  

# GCC (GCC : GNU Compiler Collection)

엔진엑스는 C로 작성된 프로그램이므로 GCC와 같은 컴파일러 도구를 먼저 OS에 설치해야 한다. GCC 는 대부분의 패키지관리자의 기본 저장소에 기본으로 존재한다. 이런 이유로 패키지 관리자를 이용해서 gcc를 설치하는 것이 가능하다.  

각 리눅스 배포판 별로 기본으로 제공되는 gcc 패키지의 이름은 아래와 같다.  

- redhat

- - "Development Tools"

- ubuntu, Debian (우분투/데비안)

- - build-essential

이렇게 리눅스 종류별로 제공되는 gcc 라이브러리가 포함된 "Development Tools", build-essential 에는 gcc만 포함되어 있는 것은 아니다. 코드 헤더 파일 및 의존성이 있는 종속성 패키지들을 모두 다운로드해서 설치하게 된다.  

  

## gcc 버전확인, 설치여부 확인

```bash
// gcc 버전 확인, 설치 여부 확인
$ gcc
```

또는 

```bash
$ gcc -v
```

  

## 설치

```bash
// redhat 기반
$ sudo yum groupinstall "Development Tools"

// 데비안/우분투 기반
$ sudo apt-get install build-essential
```



# PCRE

nginx 를 컴파일하는데에 PCRE(펄 호환 정규표현식) 라이브러리가 필요하다. 

엔진엑스 내의 아래의 두 가지 모듈은 PCRE를 정규식 구문에 사용한다.

- 엔진엑스의 URL 재작성(rewrite) 모듈
- HTTP 핵심모듈

  

pcre 라이브러리를 설치하기 위해 실제로 설치하게 되는 패키지는 아래의 두가지이다.

- pcre

- - pcre 라이브러리의 컴파일된 버전을 제공한다.

- pcre-devel

- - 프로젝트를 컴파일하는데에 필요한 헤더파일, 소스를 제공한다.  



## 설치

```bash
// redhat
$ sudo yum install pcre*

// ubuntu, Debian
$ sudo apt-get install libpcre3 libpcre3-dev
```



# zlib 라이브러리

zlib 라이브러리는 압축 알고리즘을 개발자에게 제공한다.  

엔진엑스의 다양한 모듈에서 gzip 압축을 하는 데에 zlib 라이브러리가 필요하다.  

pcre 설치시 처럼 zlib, zlib-devel 을 설치하면 된다.  

따로 다운로드 받을 필요 없이 패키지 매니저를 통해 간편하게 다운로드 받아 설치가능하다.  

(zlib 라이브러리는 리눅스 배포판의 기본 저장소에서 기본으로 제공되는 라이브러리이기 때문에 zlib 라이브러리를 따로 다운로드 받아 설치할 필요 없이 패키지 매니저로 다운로드 받을 수 있다.)   

  

## 설치

```bash
// redhat
$ sudo yum install zlib zlib-devel

// 우분투, Debian
$ sudo apt-get install zlib1g zlib1g-dev
```

  

# OpenSSL

강력한 범용 암호 라이브러리와 함께 보안 소켓 계층 (SSL v2/v3)과 전송 계층 보안(TLS v1) 프로토콜을 구현하고 있는 라이브러리이다.  

OpenSSL 은 전 세계 곳곳에서 자원봉사자의 공동체에 의해 관리되고 있다.  

인터넷으로 의견을 나누고, 계획하고, OpenSSL 툴킷과 관련된 문서를 개발한다.  

http://www.openssl.org 를 방문하면 추가정보를 얻을 수 있다. (https://www.openssl.org/)  

  

## 설치

```bash
// redhat
$ sudo yum install openssl openssl-devel

// 우분투, Debian
$ sudo apt-get install openssl libssl-dev
```

  

# nginx 다운로드

[nginx.org](http://nginx.org) 의 공식 홈페이지는 https://nginx.org/ 이다.

- download 페이지에 들어가서 적절한 버전을 다운로드 받고 
- configure 명령을 통해 컴파일을 하고
- make, make install 명령을 통해 설치한다.

  

```bash
$ cd nginx-study

$ mkdir src && cd src

$ wget https://nginx.org/download/nginx-1.19.7.tar.gz

$ tar xvzf nginx-1.19.7.tar.gz
```

  

# nginx configure (구성)

```bash
$ ./configure
```

nginx.conf 파일의 위치를 /etc/nginx/nginx.conf 로 명시적으로 지정하여 configure 하는 방식이다.  

```bash
$ ./ configure --conf-path=/etc/nginx/nginx.conf
```

  

# 애플리케이션 컴파일&설치

일반 사용자가 애플리케이션을 컴파일해서 설치하는 것은 쉬운일은 아니긴 하다. 리눅스에 배포되는 프로그램은 일반적인 상용프로그램과는 다르게 소스코드를 열어서 볼수도 있고 직접 옵션을 수정하여 컴파일 또한 가능하고 설치 또한 직접한다. 이 말은 배포하는 것이 오픈소스를 개발한 사람의 몫이 아니라 사용자의 몫이라는 의미이다. 

소스코드를 이용해서 애플리케이션을 구축하는 데에는 보통 아래의 세 단계를 거친다.

- 구성 (configure)
- 컴파일 (make)
- 설치 (make install)

구성 단계에서는 프로그램이 구축된 후에는 변경할 수 없는 다수의 옵션을 선택할 수 있다. 이런 옵션은 프로젝트 바이너리 파일의 실행에 직접적인 영향을 끼치게 된다.

따라서 구성단계는 특정 모듈이 빠지거나, 엉뚱한 폴더에 구성파일이 저장된다거나 하느일로 나중에 당황하지 않으려면 주의해서 따라야 하는 매우 중요한 단계이다.

이 과정은 소스코드에 함께 따라오는 configure 명령에 몇 가지 스위치를 추가하는 작업으로 이뤄진다. 사용할 수 있는 세 가지 유형의 스위치는 나중으로 미루고 일단 가장 쉬운 처리 방법을 배워두자.  

  

## 단순 설치방식

이렇게 설치하면 nginx 에서 지정하는 기본설정에 따라 설치하게 된다.

```bash
$ ./configure
$ make
$ make install
```

  

## 커스터마이징 설치 :: (1) 경로 지정 옵션

configure 명령을 실행할 때 특정 요소의 디렉터리, 파일 경로를 지정할 수 있는 몇가지 스위치를 사용 할 수 있다.

이것을 구성 스위치라고 하는 것이 정확한 용어인지는 모르겠다. 아무튼 옵션이나 이런것들을 주는 것을 스위치라고 이해해두자. 앞으로도 꾸준히 사용될 용어인 듯 해보인다.

configure --help 명령을 실행하면 사용할 수 있는 모든 스위치(옵션) 목록을 출력한다. 

예를 들어 --conf-path 옵션을 사용할 때의 예제는 아래와 같다. nginx.conf 파일을 /etc/nginx/nginx.conf 에 지정해두겠다는 옵션을 두어 configure 하고 있다.

```bash
$ ./configure --conf-path=/etc/nginx/nginx.conf
```

  

구성 명령시 사용할 수 있는 경로지정에 관련된 스위치(옵션)의 목록은 14 개 정도이다.

- --prefix
- --sbin-path
- --conf-path
- --error-log-path
- --modules-path
- --pid-path
- --lock-path
- --with-perl_modules_path
- --with-perl
- --http-log-path
- --http-client-body-temp-path
- --http-proxy-temp-path
- --http-fastcgi-temp-path
- --http-uwsgi-temp-path
- --http-scgi-temp-path
- --builddir

  

## 커스터마이징 설치 :: 사전 구성 요소 옵션

  

## 커스터마이징 설치 :: 기준 경로 옵션



  