# 31일차 
####실수리스트 

- settings.py에 있는 변수를 가져 오고 싶으면 __from app_name.settings.py import variable_name 으로 하면 안된다!!__ 
	- 대신 __from django.conf import settings__

- 사전형식은 key와 value가 공백 없이 다 붙어있다. 보기 편한 형태로 공백이 있는것은 사전이 아니기 때문에 json.loads로 다시 파이썬 객체로 가져와야 한다. 
```
import json
import os
import requests

current_path = os.path.dirname(os.path.dirname(os.path.dirname(os.path.abspath(__file__))))
conf_path = os.path.join(current_path, '.conf')
json_path = os.path.join(conf_path, 'settings_local.json')
youtube_key = json.loads(open(json_path).read())['youtube']['API_KEY_YOUTUBE']


def show_info(query):
    url = 'https://www.googleapis.com/youtube/v3/search'
    payload = {
        'part': 'snippet',
        'q': query,
        'key': youtube_key,
    }
    data = requests.get(url=url, params=payload)
    youtube_dict_json = data.text
    title = []
    youtube_dict = json.loads(youtube_dict_json)
    for items_list in youtube_dict['items']:
        title.append(items_list['snippet']['title'])
    return title

print(show_info('민아'))
#
# for items_list in youtube_dict['items']:
#     for i in items_list:
#         print(i['snippet']['title'])
# return title
```

- app_label, model_name = app_label.split('.')
ValueError: not enough values to unpack (expected 2, got 1)
	- AUTH_USER_MODEL에서 오류가 난 경우로 앞에 앱 이름을 넣어줘야 한다. 올바른 경우는 다음과 같다.
```
AUTH_USER_MODEL = 'member.MyUser' 
```


---
#### 질문사항 및 더 찾아봐야할 내용 

 - ~~css가 변경되는 것을 파이참이 반영을 못할 때 어떻게 고치는가~~ [stackoverflow](http://stackoverflow.com/questions/13446325/django-css-is-not-not-working)
 	- 브라우저에서 이미 css가 캐쉬에 저장이 되어 변경된 사항을 반영하지 않는 것이 원인이다. 
 	- template css load하는 부분에 추가적인 파라미터를 입력해서 이를 해결할 수 있다.
 	- 추천하는 방법으로는 ?{% now 'c' %}을 입력하면 된다. 
 
- cleaned_data의 의미는


- 이미지 url을 알때 이것으로 파일을 save하기 위해 쓰이는 urllib.urlretrieve의 역할은

---

####수업로드 

- 유투브 쿼리 입력시 결과값 리스트로 보여주기(어제 숙제 진행)
	- __json파일을 불러오는 함수 utils.settings에서 따로 만들기__
	- 디비에 중복입력 방지 
	
- AbstractUser를 상속받아 CustomUser구현 (Using a custom user model when starting a project)
	- member app생성
	- AbstractUser를 상속받은 MyUser를 생성
	- AUTH_USER_MODEL에 등록 
	- Django admin에 MyUser등록 
	- 기본 UserAdmin 을 상속받아 사용자 관련 모듈이 잘 작동하도록 설정 (기본값으로 두면 패스워드 해싱 등이 동작하지않음) 
	
- 회원기능을 구현하고 검색결과중 맘에 드는것을 북마크 하여 저장할 수 있게 
	- Post가 아니라 get방식으로 검색하여 결과를 출력할 수 있게 한다. 

- Next, Prev버튼 추가 
	- Youtube Search API에 요청을 보낸 후 , 결과에 
		- nextPageToken만 올 경우네는 첫번째 페이지
		- 둘다 올 경우에는 중간 어딘가 
		- prevPageToken만 올 경우에는 마지막 페이지 임을 알 수있음. 
	- 템플릿에 두 토큰을 전달해서 해당 토큰 값이 있을 경우에 따라 각각 다음 또는 이전 버튼을 구현함. 


---
####수업진행 포인트
(오전 pair-programming)

- .conf폴더가 밖에 있어야 하는 이유.
	- key를 보호 하기 위해
	- 다른 프론트엔드 등 팀원들과 협업을 위한 JSON 혹은 XML파일 저장
	
- dirname을 중복해서 쓰는 방법. 
```
b = os.path.dirname(os.path.dirname(os.path.dirname(a))) 
```

- json파일 불러오고 키를 받아오는 로직 따로 구성하기
```
import json
import os

def get_setting(): 
```

- 어떻게 하면 중복된 DB(query값이 같은 것을 여러번 입력할때 이것을 막는 것) 입력을 막을 것인가. 
	- get_or_create을 사용하기
	- 이때에는 default매개변수를 주어야 한다. 
```
    try:
        video = Video.objects.get(youtube_id=videoId)
    except Video.DoesNotExists:
        video= Video.objects.create(
            어쩌구 저쩌구
        )
    
    defaults = {
        'title':title, 
        'description':decscription, 
        'publihsed_date':published_date, 
    }
    video, video_created = Video.objects.get_or_create(
        youtube_id = videoId,
        defaults = defaults, 
    )
```

- django_app git:(master) ✗ rm -rf db.sqlite3 지워버리기 디펜던시 에러 


- AbstracUser와 AbstractBaseUser 차이 
Useradmin 상속 차이 

- admin.site.register(앞에거, 뒤에꺼)
```
def register(self, model_or_iterable, admin_class=None, **options):
```
앞에거, 뒤에거, 이름이 달라도 상관없다 
register가 어떤 역할을 하는지 참조할것. 

지금처럼 AbstractUser를 상속받을때 가장 편하게 admin까지 관리할 수 있다. 
AbstractBaseUser를 상속받으면 피곤해진다? 

- GET동작에서 검색결과 없을때 
```
        query = request.GET.get('query', '').strip()
        if query != '':
```

- 만약에 pageToken값이 없는 경우에는 어떻게 처리할것인가? 
잘못된 방법
```
            if 'nextPageToken' in entire_dict:
                next_page_token = entire_dict['nextPageToken']
            else:
                next_page_token = None
```
잘된 방법
```
next_page_token = entire_dict.get('nextPageToken')
```
entire_dict.get()을활용한다. 

