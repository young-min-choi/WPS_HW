# 29일차 

#### 실수list 
- 항상 /끝에 붙여주자. url = '/media/' 

- tests.py를 리팩토링 하여 패키지로 나누는 경우, 기존의 tests.py는 지워버려야 한다. 

- author.save() 빼먹지 말기 
```
def change_profile_image(request):

    if request.method == "POST":

        form = ProfileImageForm(request.POST, request.FILES)
        id = request.user.id
        if form.is_valid():
            author = MyUser.objects.get(id=id)
            author.img_profile = (request.FILES['img_profile'])
            author.save()
            print('2')
            return redirect('member:profile')
    form = ProfileImageForm()
    context = {
        'ProfileImageForm' : form
    }
    return render(request, 'member/change_profile_image.html', context)

```
- template에 포토 사진을 업로드 할때 {{ MEDIA_URL }} 앞에 붙이기  

- TemplateDoesNotExist오류가 뜬다. 잘못됐다. render 'member/~.html' 

- MyUserAdmin
	- fieldsets
		- __MyUserAdmin fieldset에 추가하니까 갑자기 admin사이트에  img_profile이 뜬다!__ 
	- KeyError
	- "Key 'date_joined' not found in 'MyUserForm'. Choices are: email, gender, groups, is_active, is_staff, is_superuser, last_login, password, user_permissions, username."


---
####더 찾아봐야할 내용 

- ~~/로 닫고 안닫고의 차이~~ [장고 다큐멘테이션 APPEND SLASH](https://docs.djangoproject.com/en/dev/ref/settings/#append-slash)
[장고 다큐멘테이션 url dispatcher](https://docs.djangoproject.com/en/1.10/topics/http/urls/)
	- __APPEND_SLASH__ 는 좋은 설정이 아니다? 
	- __기본적으로 항상 /를 붙여주자__
	-  웹 브라우저는 항상 

- CSRF_token 
	- 매 request마다 token에 임의의 다른 값을 넣어줘서 (다른 사용자들이 알 수 없는) CSRF를 막는다. 
	- request=request를 넣어도 해당 request가 같은 token 값을 찾아서 넣는 것을 불가능하다. 
	
- TransactionTestCase

- 왜 함수 내에 self가 들어가야 하는지? 
    def test_unfollow(self):
        users = make_dummy_users(self)

- 데코레이터 [국내사이트](http://jonnung.github.io/python/2015/08/17/python-decorator/)
	- __파이썬의 함수는 일급 객체(First class object)이다.__ [퍼스트클래스](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%8D%BC%EC%8A%A4%ED%8A%B8%ED%81%B4%EB%9E%98%EC%8A%A4-%ED%95%A8%EC%88%98-first-class-function/)
		- 함수를 인자로 받을 수 있음의 강력한 예. ex) def my_map(func, list)
	- 클로저를 통해 원래 함수를 지워버려도 지역변수가 살아있다. 클로저. [클로져 이해](http://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%ED%81%B4%EB%A1%9C%EC%A0%80-closure/)
	


---

####수업로드 

- 유저가 로그인하면 list로 아니면 signup으로 보내기 
	- is_anonymous
	
- tests.py작성
	- 접속시 redirect되는지
		- user 새로 만들기
		- 로그인 시키고 redirect 제대로 되는지 

- MTM, Reculsive, Intermediate Model 만들기 

- FT로 재작성
	- (리팩토링) setup, teadrdown 테스트 환경설정 
	- (리팩토링) 테스트용 유저 만들고 로그인 시키기
	- 유저 팔로우 기능 구현하기 
	- 프로필 페이지에서 게시물, 팔로워, 팔로우 숫자 확인하기
		- (리팩토링) 각각의 테스트 3개 자동화 
	
---
####수업진행 포인트

```
    def make_test_user_and_login(self):
        
        # 유저를 생성
        test_username = 'test_user'
        test_password = 'test_password'
        test_email = 'abc@naver.com'
        test_gender = 'M'
        user = MyUser.objects.create_user(test_username, test_password, test_email, test_gender)

        # 유저를 로그인 시킴
        self.browser.get(self.make_url('/member/login/'))
        input_username = self.browser.find_element_by_id('id_username')
        input_password = self.browser.find_element_by_id('id_password')
        input_username.send_keys(test_username)
        input_username.send_keys(Keys.ENTER)
        input_password.send_keys(test_password)
        input_password.send_keys(Keys.ENTER)

        return user
```

- 데코레이터 
	- in settings.py, LOGIN_URL = 'member:login' 

```
@login_required
def profile(request):
```