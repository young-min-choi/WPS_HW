# 37일차 

####실수 리스트 

- STORAGE_DIRS should be a tuple or list.

	- STORAGE_DIRS를 중복선언한 경우. 

---
####더 찾아봐야할 내용 및 질문 

- STATIC vs DYNAMIC website [참고](http://nicholaskuechler.com/2016/12/04/create-a-static-website-at-amazon-aws-using-python-s3-and-route53/)
	- STATIC은 DYNAMIC웹사이트 보다 훨씬 빠르고 안전하다.
> Static vs Dynamic
A static web site is a website made up of “flat” or “stationary” files, that are delivered to the end user exactly as stored. Most commonly, static websites are a collection of plain .html files.
Dynamic web sites, on the other hand, are generated for the user on the fly by an application server. An example of a dynamic web site would be any WordPress site.

---

####수업로드 

- S3 설치 및 설정 
	- S3로 STATIC을 관리하기 위해서 하는 것이다. 
 

---
####수업진행 포인트

- settings.py에 수정 

```
# AWS
AWS_ACCESS_KEY_ID = config['aws']['access_key_id']
AWS_SECRET_ACCESS_KEY = config['aws']['secret_access_key']
AWS_STORAGE_BUCKET_NAME = config['aws']['s3_storage_bucket_name']
```

- manage3 파일작성
```
(deploy_ec2_env) ➜  deploy_ec2 git:(master) ✗ vim ~/.scripts/manage3
(deploy_ec2_env) ➜  deploy_ec2 git:(master) ✗ sudo vim ~/.scripts/manage3
[sudo] password for ym: 
(deploy_ec2_env) ➜  deploy_ec2 git:(master) ✗ ..
(deploy_ec2_env) ➜  django ..
(deploy_ec2_env) ➜  projects ..
(deploy_ec2_env) ➜  ~ mkdir .scripts
(deploy_ec2_env) ➜  ~ cd .scripts 
(deploy_ec2_env) ➜  .scripts vim manages3
(deploy_ec2_env) ➜  .scripts 

```

- settings.py에 수정 

```
pip install django-storages 실행 

INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',

    'storages',

    'member.apps.MemberConfig',
]
```

- 다음과 같은 주소로 static 파일에 접근 가능
```
http://s3-ap-northeast-2.amazonaws.com/archoiym-bucket/images/1.jpg
```

- 다음과 같이 서버를 실행한다. 
```
MODE='DEBUG' STORAGE='S3' ./manage.py runserver
```

- *aws의 secret_key, access_key_id의 관리에 각별히 주의한다*

- zsh 의 위치를 확인하는 명령어
```
which zsh
```


- zshrc설정

```
/.zshrc에서의 PATH 설정 

export SCRIPTS_PATH="$HOME/.scripts"
export PATH="$SCRIPTS_PATH:$PATH"
```


```
source ~/.zshrc
```
새로운 설정으로 쉡을 다시 실행하겠다는 뜻이다. 



- 장고에서 우리가 원하는 곳으로 스토리지를 모두 설정한다???
	- static과 media 파일들이 구분이 안되는 문제점이 발생한다. 

```
storages.py에서

from storages.backends.s3boto3 import S3Boto3Storage


class StaticStorage(S3Boto3Storage):
    location = 'static'
```


```
settings.py에서
if STORAGE_S3:
    
    DEFAULT_FILE_STORAGE = 'storages.backends.s3boto3.S3Boto3Storage'
    STATICFILES_STORAGE = 'deploy_ec2.storages.StaticStorage'
```