# 36일차 

####실수 리스트 

- scp가 먹히지 않는 원인 (permission denied) 
	- 서버에서 srv의 권한자를 안바꾸었다. 

- Migration admin.0001_initial is applied before its dependency member.0001_initial on database 'default'.


-  sudo -u postgres createdb deploy_ec2 --owner=cym가 먹히지 않던 이유 ( is being accessed by others)
	- pageadmin으로 들어가서 postgreSQL: localhost 컨넥션을 끊어야 한다. 

- db.sqlite3파일의 존재 <-> sudo -u postgres dropdb deploy_ec2  

- uWSGI관련 에러 
```
your processes number limit is 3908
your memory page size is 4096 bytes
detected max file descriptor number: 1024
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8080 fd 3
chown(): Operation not permitted [core/utils.c line 2623]

```
sudo /home/ubuntu/.pyenv/versions/mysite/bin/uwsgi --http :8080 -i /etc/uwsgi/sites/mysite.ini 으로 실행해야 한다. 

---
####더 찾아봐야할 내용 및 질문 

- deploy-ec2 명령어 바꿀 수 있지 않나? 어느 폴더에서나 실행될 수 있게 

- sudo apt-get install make 하는 이유? 

- postgre

---

####수업로드 

- POSTGRE SQL설치 및 설정
 

---
####수업진행 포인트

- pgadmin4-1.2-py3-none-any.whl 설치 
	- ./manage : DB를 관리해주는 
	
- sudo -u postgres createuser -s -P cym

- sudo -u postgres createdb deploy_ec2 --owner=cym 

-pgadmin설치 
```
The configuration database has been created at /home/ym/.pgadmin/pgadmin4.db
Starting pgAdmin 4. Please navigate to http://localhost:5050 in your browser.

```
- STATIC ROOT수정하기 
모든 스태틱을 한 곳에 몰아 넣기.
동적으로 하면 느려진다. 

```
STATIC_ROOT = os.path.join(ROOT_DIR, 'static_root')
```

- Nginx와 uWSGI는 static관련 설정과 장고관련 설정이 바뀔 때 마다 restart시켜줘야 한다. 