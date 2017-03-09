# 34일차 

####더 찾아봐야할 내용 및 질문 

- uwsgi 로 실행시 css 가 적용되지 않는 이유 

---

####수업로드 

- AWS배포하기 

- WSGI (Web Server Gateway Interface) 

- 깃헙 문서에 없는 내용 
	- 서버에 접속하기 
	- pyenv설치 시 생기는 문제점 해결
	- 로컬에 설치된 장고 앱을 서버 쪽으로 복붙하기 

---
####수업진행 포인트

- 사전 2개 합치기 
```
for key, key_dict in config_common.items():
    if not config.get(key):
        config[key] = {}
    for inner_key, inner_key_dict in key_dict.items():
        config[key][inner_key] = inner_key_dict
```

- 0.0.0.0/0
	- 누구든지 접속할 수 있다는 뜻

- 특정 파일  나만 읽을 수 있게 
```
chmod 400 [파일]
```
- Security Group 
	- 이 그룹 내에 인스턴스 들이 여러개 속할 수 있다는 의미 

- 접근하는 방법 
	- Public DNS: ec2-13-124-38-251.ap-northeast-2.compute.amazonaws.com
	
- 접근 권한 막기 

```
➜  .ssh chmod 400 cym.pem 
➜  .ssh l
합계 12K
drwxrwxr-x  2 ym ym 4.0K  3월  6 12:31 .
drwxr-xr-x 44 ym ym 4.0K  3월  6 14:09 ..
-r--------  1 ym ym 1.7K  3월  6 12:31 cym.pem

```

- zshrc 
```
➜  ~ source ~/.zshrc
```

- sudo는 현재 명령어를 루트계정의 권한으로 실행한다는 뜻이다. 

- 다음 명령어의 의미 
	- r은 recursive
	- -i 다음의 인증을 사용한다. 
	- .은 현재폴더
	

```
scp -r -i ~/.ssh/cym.pem . ubuntu@ec2-13-124-38-251.ap-northeast-2.compute.amazonaws.com:/srv/app
```

- alias설정하기 

```
vim ~/.zshrc
안에 들어가서 alias [축약어]="명령어"
source ~/.zshrc
```

- 서버 리부트 시키기 
```
sudo shutdown -r now
```
