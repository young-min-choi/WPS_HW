# 35일차 

####더 찾아봐야할 내용 및 질문 

- Cloud Computing개념 [공식싸이트](https://aws.amazon.com/ko/what-is-cloud-computing/)

- EC2의 개념 

- virtual server 와 serverless의 차이 

- 공개키 암호키 (비대칭 암호화 방식) 

- MITM공격 

- tokenization과 encryption의 차이 [싸이트](http://claimvantage.com/tokenization-vs-encryption-whats-difference/)

- 소켓이란? 
	- socket 은 IP address 와 Port 넘버가 합쳐진, 네트워크 상에서 서버 프로그램과 클리어언트 프로그램이 통신을 할 수 있도록 해주는 소프트웨어 장치이다. 
[출처] Socket 이란?|작성자 오바나딩요

---

####수업로드 

- IAM
 

---
####수업진행 포인트

- EC2의 개념 
	- 인스턴스가 컴퓨터 한대라고 생각하면 된다. (서버구축에서) 

	- ec2는 가상 컴퓨터라고 생각하면 된다. 

	- pip install awcli

	- state: terminate / stop /running 

- IAM 

	- ROOT ACCESS KEY는 모든것을 할 수 있는 권한. 최대한 사용하지 않는 것이 좋다. 
	- 로컬이라는 것의 의미? 로컬은 내 컴퓨터이다. 로컬에서 모든 것을 다 실행할 수 있게 스크립트를 짜야 한다. 결국에는 서비스가 확대되면 여러 대의 서버를 쓰게 되는데 이를 각 서버에 들어가서 통제할 수 없다. 로컬에서 여러 대의 서버를 컨트롤할 수 있게 해야 한다. 
	
	- ACCESS KEY ID와 SECRET ACCESS KEY란? 

```
Programmatic access
Enables an access key ID and secret access key for the AWS API, CLI, SDK, and other development tools.

AWS Management Console access
Enables a password that allows users to sign-in to the AWS Management Console.
```

- 서버 접속방법

```
ssh -i cym.pem ubuntu@ec2-13-124-55-200.ap-northeast-2.compute.amazonaws.com

```
