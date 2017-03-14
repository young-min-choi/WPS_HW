# 38일차 

####실수 리스트 

- STORAGE_DIRS should be a tuple or list.



---
####더 찾아봐야할 내용 및 질문 


- docker image와 container의 기본개념 [docker](https://www.slideshare.net/pyrasis/docker-docker-38286477)


---

####수업로드 

- Docker해보기
 	- django project폴더 이름을 config로 수정하기

- AWS Elastic Beanstalk
	- 파이썬 가상환경 로컬 폴더내에서 pip install awsebcli 실행
	
---
####수업진행 포인트

- config폴더로 바꾸기
	- manage.py, settings.py, wsgi.py에 있는 프로젝트명을 코드를 config로 수정한다. 
	- django_app내의 [프로젝트명_폴더]를 'config'로 수정한다.
	
- 이미지열고 서버 설정 
	- docker run --rm -it ubuntu:16.04 /bin/bash 실행
	- 이후 ec2에서 한것 처럼 서버설정을 한다. 
		- apt-get update 실행
		- apt-get install python3 (python3를 쓸 것이므로) 
		- apt-get install python3-pip 
		- ec2와 다르게 정말 기본적인 설정만 있기 때문에 ec2의 ubuntu 계정도 없다. 기본적인 root계정만 처음에 존재한다.
		- apt-get install nginx 
		- /srv에서 app/폴더를 만든다.
		- pip3 install django
		- pip3 install uwsgi
		- django-admin startproject eb 
		
	- 여기까지 실행하고 서버에서 나갔다가 들어오면 container가 꺼져버려서 모든 것들이 지워진다.
		- 그러므로 dockerfile을 작성한다. 일종의 스크립트이다?
		- 다음과 같은 도커파일 스크립트를 작성한다.
		- > FROM        ubuntu:16.04
MAINTAINER  archoiym@gmail.com
RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y python3-pip
RUN         apt-get -y install nginx
WORKDIR     /srv
RUN         mkdir app
WORKDIR     /srv/app
		- docker build -t eb . 를 실행한다. 
		- -y라는 옵션을 넣어서 물어볼때마다 yes를 자동적으로 실행한다. 
		
	- 과정이 비효율적이므로 dockerfiles_base를 만든다.
		- > FROM        ubuntu:16.04
MAINTAINER  archoiym@gmail.com
RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y install python3-pip
RUN         apt-get -y install nginx
		- 기존 dockerfiles에서는 from 을 eb-base라고 변경한다.
		- docker build . -t eb-base -f Dockerfile_base 실행
		- docker images로 생성된 것을 확인한다. 
		- image를 지우려면 docker rmi [ID입력] 실행
		- container에서 사용중이면 -f 라는 옵션을 추가한다. 
	- 만들어진 이미지를 활용해 컨테이너를 만든다.
		- docker run --rm -it [이미지이름] /bin/bash 실행
		- 다음과 같이 수정 
		- > RUN         pip3 install -r requirements.txt
RUN         pip3 install uwsgi
WORKDIR     /srv/app/django_app
RUN         python3 manage.py runserver 0:8080

		- dockerfile을 바꿀 때마다 build를 다시 해야한다. 
	- 이제 로컬에서 서버로 접속한다. 
		- docker run -p 4567:8080 eb
		- 이것의 의미는 로컬 포트 4567로 서버 8080에 연결한다는 의미. 
	- image의 이름을 tag라고 한다. 
	- tag 가 none인 이미지 한번에 지우기
		- docker rmi -f $(docker images --filter "dangling=true" -q) 실행

- uwsgi 로 하기
	- 설정변경 스크립트 
	- uwsgi-app.ini 작성
	- docker run --rm -it -p 4040:8080 eb 실행
	- 기본적인 설정을 미리 작성하여 build를 빠르게 한다. (eb-base를 추가한다.)
	- >FROM        ubuntu:16.04
MAINTAINER  archoiym@gmail.com
RUN         apt-get -y update
RUN         apt-get -y install python3
RUN         apt-get -y install python3-pip
RUN         apt-get -y install nginx
RUN         pip3 install django
RUN         pip3 install uwsgi


- docker컨테이너에 접속하기 
	- uwsgi 실행하기
	- 다른 커맨드라인에서 docker ps
	- docker exec -it f3e6e84e982e[컨테이너 id] /bin/bash 
	- /bin/bash는 커맨드쉘도 프로그램이기 때문에 이를 실행하기 위한 것이다. 
	
- 트러블 슛팅 
	- uwsgi --http :4040 -i /etc/uwsgi/sites/app.ini / uwsgi에 문제있는지
	- /var/log/nginx cat error.log 어떤 에러가 있는지 확인 
 
 - eb deploy 트러블슛팅 
 
	