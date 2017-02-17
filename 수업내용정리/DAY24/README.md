#24일차 

####실수 리스트
- 반드시 urls patterns 에서 ^$ 명시해주기. 그렇지 않으면 앞에 것이 다 잡아 먹는다. 
- html extend시 {% block content %} 만들어주기 
- STATICFILES_DIR 헷갈리지 말기 

- views함수에 return 안적고 render나  HttpResponse 적기

-(CSS) 클래스 명을 틀렸다. // .comment-delete 

---

####더 찾아봐야할 내용 

1. ~~unique_together~~ [장고 다큐멘테이션](https://docs.djangoproject.com/en/1.10/ref/models/options/)
	- Sets of field names that, taken together, must be unique:
2. 파이썬 삼항연산자
3. queryset 연산자 delete / remove
4. ~~namespace~~ [stack overflow](http://stackoverflow.com/questions/19171570/a-real-example-of-url-namespace)
	- Typically, they are used to put each application's URLs into their own namespace. This prevents the reverse() Django function and the {% url %} template function from returning the wrong URL because the URL-pattern name happened to match in another app.
5. ~~form 개요에 대해 이해하기~~ [장고 다큐멘테이션](https://docs.djangoproject.com/en/1.10/topics/forms/) 
	- As well as its __input__ elements, a form must specify two things:
	- In a similar way that a model class’s fields map to database fields, a form class’s fields map to HTML form __input__ elements.
	- Instantiating, processing, and rendering forms
		- get hold of it in the view (fetch it from the database, for example)
		- pass it to the template context
		- expand it to HTML markup using template variables
---

####수업 로드 

- member/models.py
	- class MyUser
		- username, last_name, first_name, nickname, email, date_joined, last_modified, following
		- __str__
		- follow 
		- unfollow 
		- followers
		- change nickname
		- create_dummy_user
		- assign_global_variables 

- post/models.py 
	- class Post 
		- author, photo, content, created_date, like_users 
		- like_count 
		- comment_count 
		- add_comment 
		- toggle_like 
	- PostLike
		- user, post, cre
		- Meta class
		
	- class Comment 
		- author, post, content, created_date 
		- __str__ 

- member.views.py
	- def user_login(request)
		- authenticate() 의 활용 
		- request.method에 따라 if문 설정하기 
---
####수업내용포인트


1. MTM는 DB상에 존재하지 않는, 장고에서의 설정이다. 
그러므로 UNIQUE자체가 의미가 없다. UNIQUE는 DB상의 처리이기 때문이다. 

2. Pickle은 장기로 보존할 자료에는 적용해서는 안된다. 
피클을 이용하여 모든 파이썬 데이터 객체를 저장하고 읽을 수 있다. 
장기간 보존을 원한다면 SQL DUMP를 하는 방법 등이 있다. 
피클 사용방법.
필요한 모듈을 임포트
Dump()를 사용해서 데이터를 저장.
Load()로 다시 읽어 오면 끝.
※피클 파일로 작업할 때는 파일을 이진 접근 모드로 열어야 한다.
http://kshyun87.tistory.com/23 [빅데이터를 꿈꾸다.]

3. verbose_name은 설정해서 어디에 쓰이는건지? 
	- 우선 admin 에 테이블을 보면 그 이름으로 뜬다.
	- 폼에서도 쓰인다고 하는데? 
	
4. 더미 유저 생성하기 
```
    def create_dummy_user(num):
        
        import random
        
        last_name_list = ['방', '이', '박', '김', '최']
        first_name_list = ['민아', '혜리', '소진', '유라']
        nick_name_list = ['빵', '리헬', '쏘지', '율곰']
        created_count = 0 
        
        for i in range(num):
            _, created = MyUser.objects.get_or_create(
                username='User{}'.format(i+1),
                last_name = random.choice(last_name_list),
                first_name = random.choice(first_name_list),
                nickname = random.choice(nick_name_list),
            )
            if created:
                created_count += 1 
            return created_count
```

5. 파이썬 인터프리터를 다루는 모듈 

```
    @staticmethod
    def assign_global_variables():
        import sys
        module = sys.modules['__main__']
        users = MyUser.objects.filter(username__startswith='User')
        for index, user in enumerate(users):
            setattr(module, 'u{}'.format(index+1), user)            
```

users를 순회하며 메인 모듈에 u1,u2,u3이름으로 각 MyUser객체를 할당
실행되고 있는 모듈이 바로 메인 모듈이 된다. 
쉘에서 적는 것이 사실 글로벌 모듈에 적는 것이다. 

6. globals() 

	- 현재 사용가능한 전역 변수를 dict형식으로 보여준다. 
	- ㅇ
	
7. post.id / post_id 
post.id 는 조인을 한번 한 이후 id를 가져온다 

8. instance에서는 filter가 적용이 되지 않는다. 
Post.objects.filter() 가능 
post.objects.filter() 불가능 

9. 두개의 가능성 
```
    def add_comment(self, user, content):
        #Comment.objects.create(post=self, content=content, author=user)
        return self.comment_set.create(
            user=user,
            content=content,
        )
```     

10. page_not_found() 
handler404 = 'mysite.views.my_custom ~' 

11. namespace란? 

>URL namespaces allow you to uniquely reverse named URL patterns even if different applications use the same URL names. It’s a good practice for third-party apps to always use namespaced URLs (as we did in the tutorial). Similarly, it also allows you to reverse URLs if multiple instances of an application are deployed. In other words, since multiple instances of a single application will share named URLs, namespaces provide a way to tell these named URLs apart.
Django applications that make proper use of URL namespacing can be deployed more than once for a particular site. For example django.contrib.admin has an AdminSite class which allows you to easily deploy more than one instance of the admin. In a later example, we’ll discuss the idea of deploying the polls application from the tutorial in two different locations so we can serve the same functionality to two different audiences (authors and publishers).
A URL namespace comes in two parts, both of which are strings:
application namespace
This describes the name of the application that is being deployed. Every instance of a single application will have the same application namespace. For example, Django’s admin application has the somewhat predictable application namespace of 'admin'.

12.
``` 
from django.db import IntegrityError
from django.shortcuts import render_to_response

try:
    # code that produces error
except IntegrityError as e:
    return render_to_response("template.html", {"message": e.message})
```

13. 
    class Meta:
        unique_together = ( ('user', 'post'), )
        
       
