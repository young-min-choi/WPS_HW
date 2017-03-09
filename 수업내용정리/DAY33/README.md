# 33일차 

#### 실수 LOG

- redirect_uri=http://localhost:8000/member/login/facebook/
끝에 /를 빼먹으면 안된다. 

- authenticate 임포트 할때 auth.로 하면 만능으로 다 된다. 

---
#### 질문 및 더 찾아봐야할 내용 

- core 와 template의 차이??
```
'django.template.context_processors.media',
```

- 

---

####수업로드 

- FACEBOOK GRAPH 탐색기 

- Customizing Authetication (Django)
	- Custom Backend 만들기 

- COOLSMS사용하기
	- 문자메세지 보내기
	- 웹사이트에서 문자메세지 보내기 기능 구현 
	
---
####수업진행 포인트

- ID는 각 앱 마다 다르다. 

-모든 만들어진 백엔드를 체크한다. 
```
settings.py
AUTHENTICATION_BACKENDS = ['django.contrib.auth.backends.ModelBackend',
                           'member.backends.FacebookBackend']
                           
```
- 
```
file_ext =re.sub(r'(\.[^?]+).*', r'\1', file_ext)
```

- 
```
    def authenticate(self, facebook_id, extra_fields):
        url_profile = extra_fields.get('https://graph.facebook.com/{user_id}/picture'.format(
            user_id=facebook_id,
        ))
        params = {
            'type':'large',
            'width':'500',
            'height':'500',
        }
        #메모리 상에 임시파일 생성
        temp_file = NamedTemporaryFile(delete=False)
        # 프로필 이미지의 URL에 Get요청, stream에 True지정
        r= requests.get(url_profile, params, stream=True)
        _, file_ext = os.path.splitext(r.url)
        file_ext =re.sub(r'(\.[^?]+).*', r'\1', file_ext)
        file_name = '{}{}'.format(
            facebook_id,
            file_ext,
        )

        for chunk in r.iter_content(1024):
            temp_file.write(chunk)

        # 페이스북 ID가 username이거나 MyUser를 갖고오거나 
        # elvhfcmrkqtdmf dldydgotj todtjd 
        defaults = {
            'first_name': extra_fields.get('first_name'),
            'last_name': extra_fields.get('last_name'),
            'email': extra_fields.get('email', ''),
        }

        user, user_created = MyUser.objects.get_or_create(username=facebook_id, 
                                                          defaults=defaults)
        # ImageField의 save 메쏘드에 파일명과 Django에서 지원하는 File객체를 전달
        user.img_profile.save(file_name, File(temp_file))
        return user
```

- as써서 충돌 피하기 
```
from django.contrib.staticfiles import views
from facebook import views as facebook_views
```


- 바로 ROOT로 디렉토리 날아가기 
```
root_path = Path(__file__).parent[3]
conf_file = Path(root_path).joinpath('.conf', 'settings_local.json')
return json.loads(conf_file.open().read())
```

- 번호 validation 
```
class SMSForm(forms.Form):
    recipient_numbers = forms.CharField(
        max_length=500,
        widget=forms.TextInput(
            attrs={
            'size': '100',
            }
        )
    )
    content = forms.CharField(widget=forms.Textarea)
    
    def clean_recepient_numbers(self):
        cleaned_numbers = []
        p = re.compile(r'^0\d{9}\d?')
        number_string = self.cleaned_data['recepient_numbers']
        numbers = number_string.replace('-', '').replace(' ', '').split(',')
        for number in numbers:
            if re.match(p, number):
                cleaned_numbers.append(number)
            else:
                raise ValidationError('Invalid phone number format! {}'.format(number))
        return cleaned_numbers
```