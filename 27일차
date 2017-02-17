# 27일차 

실수목록 

- 반드시 urls patterns 에서 ^$ 명시해주기. 그렇지 않으면 앞에 것이 다 잡아 먹는다. 
- html extend시 {% block content %} 만들어주기 
- STATICFILES_DIR 헷갈리지 말기 

- views함수에 return 안적고 render나  HttpResponse 적기

-(CSS) 클래스 명을 틀렸다. // .comment-delete 
---
질문사항 

- 왜 post_list.min.css는 안먹히고 post_list.nested.css는 먹히는건가?

- http://localhost:8000/media/ 404 (Not Found) 에러 왜 뜨는건지? 

- sprite image에서 위치 찾기 

- sprtie 크기 지정 

- Initial 써서 한 세션 내에서 같은 유저네임을 사용하는것은 initial 값을 동적으로 할당하는 것인가?

- header.scss는 어떻게 base.html에 적용이 되는것인가? 

- {% ifequal %} 왜 이따구로 만들었을까? 

--- 
더 찾아봐야할 내용
 
- decorator 


---

수업로드 

- 인스타그램 프로젝트 
	- form으로 코멘트 작성 만들기 
	- like 버튼 만들기 
	- 코멘트 지우기 
	- user가 누구인지 항상 확인하기  
	
- 인스타그램 css 구현 
	- 스프라이트 쓰기 
	- 폴더 분할
---
수업진행 포인트

- 상속받아서 진행 
	- 로그인해주세요. 
	- 로그인했으면 회원 보여주기. 
```
<div>
    {% if user.is_authenticated %}
    {{ user.username }} 님 환영합니다!
    {% else %}
    로그인 해주세요. 
    {% endif %}
</div>
```

- comment delete 하는 법 2가지 
	- comment.delete()
	- Common.objects.filter().delete()
	
