# 26일차 

####실수목록  

- 쉼표 빼먹지 말기 
```
urlpatterns = [
url(), 
url(),
] 
```
- urlpatterns 는 리스트 여야 한다. urlpatterns = [] (O) // urlpatterns = {} (X)

- urls.py 파일에 views함수 이름 오기입. 

- like_users = models.ManyToManyField(
        MyUser,
        through='PostLike',
        related_name= 'likers'
    )
-  url(r'')이렇게 하면 아무거나 다 잡아먹는다. 조심하기 
   
- <!--{% if post.comment_set.count = 1 %}-->
---

더 찾아봐야할 내용 

- 데코레이터 
- ~~media URL과 ROOT의 관계~~
https://timmyomahony.com/blog/static-vs-media-and-root-vs-path-in-django/

- PermissionsMixin

---

수업로드 

- post리스트 만들기 
	- views.py
	- post-list.html
	
- ImageField 
	- MEDIA_ROOT
	- MEDIA_URL 
	
---
수업진행 포인트

- time_measurement() 
	- decorator 함수 
	- 각 함수의 성능 알아보기 
```
import time

BIG_NUM = 100000

def time_measurement(func):
    def wrap(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        diff = time.time() - start
        print(diff)
        return result
    return wrap

@time_measurement
def func_loop():
    char = ''
    for i in range(BIG_NUM):
        char += 'a'
    return char

@time_measurement
def func_list():
    l = []
    for i in range(BIG_NUM):
        l.append('a')
    return ''.join(l)

@time_measurement
def func_list_comprehension():
    return ''.join(['a' for i in range(BIG_NUM)])

@time_measurement
def func_multi():
    return 'a'* BIG_NUM

func_loop()
func_list()
```

- 미디어 URL로 왔을때는 urls.py로 가는게 아니라 MEDIA_ROOT로 가도록 해주는 것을 위해 MEDIA_URL이 필요하다. 


- urlpatterns 에 붙여준다. (개발자 서버로 할 경우) 
	- URL 절대경로 
```
+ static(settings.MEDIA_URL,
                         document_root=settings.MEDIA_ROOT
                         )
``` 
	- settings 에 media함수, def media(request): 
	return {'media_url' : MEDIA_URL } 
	항상 저 키값과 밸류값을 딕셔너리형태로 보내주는 context processsor 
	
- 파일 업로드 관련 
```
# Bound form with an image field
>>> from django.core.files.uploadedfile import SimpleUploadedFile
>>> data = {'subject': 'hello',
...         'message': 'Hi there',
...         'sender': 'foo@example.com',
...         'cc_myself': True}
>>> file_data = {'mugshot': SimpleUploadedFile('face.jpg', <file data>)}
>>> f = ContactFormWithMugshot(data, file_data)
```


- 이미지 파일 셋팅 
	- settings.py MEDIA_ROOT = 설정하기
	- settings.py context processor: 'django.template.context_processors.media',
	- settings.pt MEDIA_URL = '/media/'
	- media 폴더 만들기, 하위 폴더로 post 만들기 
	-     photo = models.ImageField(upload_to='post', blank=True)
