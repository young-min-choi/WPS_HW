# 30일차 
####실수 list 

- urls() 맨처음 부분은 끝을 열어놔야 한다. ex) /$(x)   / (O)

- int() argument must be a string, a bytes-like object or a number, not 'SimpleLazyObject' : request.user에서 문제가 생겼다. lazy하다는 것은 request.user.id까지 끝까지 가야 한다. 


- No Reverse Match for ~ {% user_id = 2 %} ~ tried ~ 
__(?P<user_id>[0-9]+)__  __urls.py에서 실수가 있었다. urlpatterns에 저 부분이 반드시 들어가서 입력 값을 url에서 받을 수 있어야 한다.__ 	


- Suspicious Operational Error 
```
The joined path (/video/1.png) is located outside of the base path component
```
해결책: upload_to = 에 //를 뻇어야 한다. 


- {{ videos.query }} 라고 치니까 난리 났다. 실제 쿼리가 나와버린다. 

- AttributeError: 'PostManager' object has no attribute '_queryset_class'

- Parent module '' not loaded, cannot perform relative import 


---
####질문사항 

- 리팩토링이라 할 수 있는가? 
```
# context = {
    #     'img_profile':img_profile,
    #     'author_id' : author.id,
    #     'post_count' : post_count,
    #     'follower_count':follower_count,
    #     'following_count':following_count,
    # }
```

- 굳이 .conf가 django_app 밖에 있어야 하는 필요가 있는가? 


---
####더 찾아봐야할 내용 

- ~~파이참에서 빨간색으로 표시된 파일의 의미~~
	-  git에 추적되지 않고 있음을 뜻함. [파이참 공식싸이트](https://www.jetbrains.com/help/pycharm/2016.3/file-status-highlights.html)
	- 갈색, 검은색 등 다른 색의 의미는? 


- 인스턴스 메쏘드 이면 def aaa(self)이고 그렇지 않으면 self를 빼야 한다? 
	- client 자체에서 요청을 하면 함수 내 인자로 (self.client?) 

- csrf 

---

####수업로드 

- 인스타그램 프로필 편집 
	- 숙제 진행 
	- 모델폼으로 해보기 
	
- 유투브 API
	- API key관리하기 
	- POSTMAN 설치하기
	- requests 패키지를 활용해서 url과 params 입력시키면 검색결과 데이터 받기
	- json활용해서 파일을 파이썬 객체 형태로 전환
	- dict를 써칭해서 원하는 키-밸류 값만 뽑아내기
- 유투브 API를 활용해서 검색어를 입력하면 검색결과를 화면에 띄우는 웹 기능 구현
	
---
####수업진행 포인트

- SignupForm __굉장히 유용한 형태의 모델폼! 가입을 굉장히 간편하게 구현할 수 있다.__

```
class SignupForm(forms.Form):
    username = forms.CharField(max_length=30)
    password1 = forms.CharField(widget=forms.PasswordInput)
    password2 = forms.CharField(widget=forms.PasswordInput)
    gender = forms.ChoiceField(choices=MyUser.CHOICES_GENDER)
    email = forms.EmailField(max_length=30)

    def clean_username(self):
        username = self.cleaned_data['username']
        if MyUser.objects.filter(username=username).exists():
            raise forms.ValidationError('Username already exists')
        return username

    def clean_password2(self):
        password1 = self.cleaned_data['password1']
        password2 = self.cleaned_data['password2']
        validate_password(password1)
        if password1 != password2:
            raise forms.ValidationError('Password not identical')
        return password2

    def create_user(self):
        username = self.cleaned_data['username']
        password2 = self.cleaned_data['password2']
        gender = self.cleaned_data['gender']
        email = self.cleaned_data['email']
        user = MyUser.objects.create_user(username, password2, gender, email)
        user.save()
        return user
```
