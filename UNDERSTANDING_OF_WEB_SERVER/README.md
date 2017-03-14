# 웹서버의 이해 

- 서버라는 것은 어떻게 구동되는가? 
	- 서버의 종류: Apache, Nginx

- 기본적으로 서버는 그냥 컴퓨터이다. 그러나 다른 컴퓨터와 다르게 특별한 역할을 하는 컴퓨터이다. 요청된 웹페이지를 보여주기 위한 역할을 한다. 

- 이러한 역할을 하는 서버는 OS의 종류에 따라 UNIX, APACHE, 등이 있다. 

- 포트가 무슨 역할인지 궁금해졌다. 

> a port is an endpoint of communication in an operating system.
A port is always associated with an IP address of a host and the protocol type of the communication, and thus completes the destination or origination network address of a communication session. A port is identified for each address and protocol by a 16-bit number, commonly known as the port number. For example, an address may be "protocol: TCP, IP address: 1.2.3.4, port number: 80", which may be written 1.2.3.4:80 when the protocol is known from context.

> Applications implementing common services often use specifically reserved well-known port numbers for receiving service requests from clients. This process is known as listening, and involves the receipt of a request on the well-known port and establishing a one-to-one server-client dialog, using the same local port number. Other clients may continue to connect to the listening port; this works because a TCP connection is identified by a tuple consisting of the local address, the local port, the remote address, and the remote port.

- 포트라는 것은 OS상에서 네트워킹을 위한 주소의 마지막 부분에 해당한다. 주소와 관련이 있기 때문에 포트는 항상 Protocol, IP와 같이 등장하는 경우가 많다. 포트를 쓰는 이유는 네트워크에서 서버가 한 개 이상의 프로그램을 수행하기 때문이다. 클라이언트가 다양한 프로그램 실행 요청을 하는데 이때 최적의 효율을 내기 위해서는 이러한 요청들을 포트를 통해 분류를 할 필요성이 생긴 것이다. 서버가 특정 포트에 대해 클라이언트의 요청을 계속 기다리는 것을 "리스닝"이라고 한다. 결국 포트라는 것은 프로세스 혹은 네트워크 서비스의 종류를 식별하기 위한 가상의 논리적 구성체이다. 

- ec2에 대해서 알아보자.
	- ec2는 elastic cloud computing의 약자이다. 
	- cloud computing이라는 강력한 기술을 기반으로 웹 서버? 서비스?를 제공하는 것이다.
	- 인스턴스 하나는 가상의 컴퓨터 한대를 의미한다고 보면 된다.
	- 공식적으로는 "가상 컴퓨팅 환경"이라고 되어 있다. 
	- security group은 일종의 방화벽으로 security group의 설정을 통해 인스턴스에 접근할 수 있는 protocol, ip 등을 설정한다. 
	- key pair는 인스턴스에 로그인하기 위해 필요한 것으로 공개키/개인키 방식으로 암호화 된다. 
	- Identity and Access Management(IAM)서비스를 통해 새로운 유저를 만들고 이 유저에게 권한을 주면, 이 유저 또한 인스턴스에 접근할 수 있다. 
	
- 호스팅은 무엇인가? 
	- 웹서버를 임대하는 것. 
	
	
---
실전

1. (로컬) 디렉토리를 하나 새로 만든다. 

2. (로컬) 디렉토리에 파이썬 가상환경을 새로 만든다. 

3. (로컬) 가상환경을 적용하고 장고를 설치한다. 

4. (로컬) ec2 인스턴스를 새로 생성한다.
	- 생성된 key_pair를 .ssh폴더로 옮긴다. 명령어 cp를 이용하여 cp [key_pair_file위치] [~/.ssh] 실행 

5. (로컬) 장고의 기본적인 셋팅을 진행한다. 
	- startproject [프로젝트명]
	- 새로 생긴 상위 폴더 [프로젝트명]을 django_app으로 변경한다. 
	- 파이참을 실행한다. 
	- django_app을 소스루트로 지정한다. 
	- .conf폴더를 만든다. 
	- 폴더 내에 settings_local.json, settings_common.json을 생성한다. 
	- 장고 시크릿 키를 json파일로 이동시킨다. 
	- allowed_hosts설정을 해준다. 
	- aws관련 키 값을 json파일로 이동시킨다. 
	- settings.py에 각종 DIR위치를 os.path를 활용해 설정한다. 
	- config파일 2개를 for문을 이용해 합친다. 
6. git설정을 해주고 첫번째 커밋을 해준다.
	- git init
	- .gitignore
	- pip freeze > requirements.txt 

**주의: aws access key id와 secret key는 key pair와 전혀 상관이 없다**

- 서버에 접속해본다. 
	- 먼저 그 전에 ~/.ssh에 있는 인증키 파일을 read-only로 바꾸어야 한다. 
	- chmod 400 [.pem파일] 실행 
	- ssh -i pem파일 ubuntu@[public id 주소]	
	- sudo apt-get update를 실행한다. 
	- sudo apt-get install 로 pip, python3, zsh, 언어팩, pyenv 등을 설치한다. 
	
- (로컬) 장고 파일을 서버로 옮긴다. 
	- (로컬) 로컬에서 장고 파일을 서버의 .srv/app으로 복사한다. 
	- 옮기는 명령어 scp -r -i [.pem파일] . ubuntu@[aws_ec2_public_domain]:/srv/app/
	- (서버) 서버에서 srv의 권한을 바꿔줘야 한다. 
	- (서버) sudo chown -R ubuntu:ubuntu /srv/을 실행한다. 
	- (서버) pyenv 설정을 해준다.
	- (서버) pyenv install 3.5.2 실행 (그래야 pyenv virtualenv 3.5.2 [env_name] 실행가능] 
	- (서버) ./manage.py runserver 0:8080 
	- 위 실행문의 의미는? 