# 25일차 
TroubleShooting

- login(request, user) // request 빼먹지 말기
- url('^member/login/', views.user_login, 'user-login') // views.user_login
- html 구성 이해 
	- form 태그 작성 
	- csrf_token 작성 
	- form 내부에 {{ form }} 만들기 
	- {% for field in form %}
---

더 찾아봐야할 내용 

1. add_error()
 
2. ~~return redirect('/admin/') 처음에 /넣고 안넣고 차이~~ 
	- /가 있으면 8000: 뒤에 붙고, /가 없으면 현재 url에 더해서 붙인다. 

3. dependency issues (migration) 

4. When you define a foreign key or many-to-many relations to the user model, you should specify the custom model using the AUTH_USER_MODEL setting.

5. SQL Injection

6. ~~context란 무엇인가~~
	- 장고 템플릿에서 미래의 사용을 위해 필요한 데이터를 미리 컴파일 해놓은 사전이다. 

---

수업로드 

- Template (documentation) 

- Working with forms (documentation)
	- 폼의 기능 및 뜻 
	- 가장 기본적인 폼의 형태
	- widget
	- bound and unbound form 

- Instagram
	- member.views
		- form.add_error() 

- Customizing authentication system (__User__)
	- Substuting a custom User model
	- get_user_model 
	- setting.AUTH_USER_MODEL  
	- 
- class MyUser(AbstractUserModel)

- PermissionMixin

- __A Full Example__
---
수업진행 포인트

1. Context processors
모든 뷰에서 활용되는 변수들을 여기서 사용하기 쉽게 처리해준다. 

2.  필터 자체를 통해 변수를 바꾸는게 아니라 변수가 어떻게 보일지를 조작한다. 
{{ list | ', '.join }} 

3.  Automatic HTML escaping 
XSS: Cross Site Scripting 

4. template에서는 method에 ()가 필요 없이 실행 된다. 

5. {% load humanize %} 데이터를 좀더 읽기 쉬운 형태로 변환해주기 위해. 

6. action: "admin/login/?next=/admin"
POST로 보내도 GET으로 받을 수 있다. 
시스템의 상태를 변경하는데 사용할 수 있는 요청 POST, 시스템 상태에 영향을 주지 않는다면 GET
__조회시에는 GET__
__삭제, 생성 시에는 POST__ 
POST를 쓰면 장고 내의 기본적인 보안 시스템을 활용할 수 있다.

7. form 자체는 action과 method를 가지지 않기 때문에 이는 우리가 HTML에서 만들어줘야 한다. 

8. form을 처리하는 뷰의 가장 기본적인 형태 

9. form.cleaned_data를 쓰는 이유. form에 이외에도 데이터 전환을 위한 다른 데이터들이 들어 있을 수있다. 


10. Lower활용해서 case-insensitive하게 order하기 
order_by(Lower('content), desc()) 
order_by내에 쿼리 함수가 들어갈 수 있다. 

11. 
```
q = MyUser.objects.annotate(number_of_entries = Count('entry'))
q[0].number_of_entries

12. 

is_active()

---
###Substituting a custom User model

- settings.py건드리기 
AUTH_USER_MODEL

- custom user model by inheriting from AbstractBaseUser.
	- USERNAME_FIELD
	- is_staff
	- objects = MyUserManager()
	- The User model has a custom manager that has the following helper methods