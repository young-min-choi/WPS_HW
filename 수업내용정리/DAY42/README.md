# 42일차 

####실수 리스트 

-  
```
django.db.utils.IntegrityError: null value in column "author_id" violates not-null constraint
```
test_cannot_post에서 post추가 요청을 하는데 author값이 비어있기 때문에 이를 만들어줘야 한다.
PostSerializer에 다음을 추가한다. 

```
    def perform_create(self, serializer):
        serializer.save(author=self.request.user)
```



---
####더 찾아봐야할 내용 및 질문 

- Django REST tutorial에서 serialization관련 읽기


---

####수업로드 

- 인스타그램 WEB API만들기 (REST활용) 



	
---
####수업진행 포인트

- app_name을 삭제하는 이유 
	- rest를 쓰려면 namespace를 쓰게 되고 그 경우 app_name이 남아 있다면 복잡해진다. 


- api_url이 존재하는지 체크하는 테스트 

```
    def test_apis_url_exist(self):
        try:
            resolve('/api/post/')
            resolve('/api/post/1/')
            # reverse('api:post-detail')
        except NoReverseMatch as e:
            self.fail(e)
```
resolve를 활용하여 해당 url이 존재하는지 확인한다. 
reverse같은 경우에는 post-detail이 argument를 요구하기 때문에 이 경우 적합하지 않다. 

- 패키지 만들어서 구조화된 디렉토리 만들기 

```
in apis/post.py
__all__ = (
    'PostList',
    'PostDetail',
)
```

```
in apis/__init__.py
from .post import *
from .post_photo import *

```

- 사진으로 테스트 하기위한 방법 어떻게 파일을 올릴 것인가? 

```
# 생성한 Post에 PostPhoto를 추가.
url = reverse('api:photo-create')
file_path = os.path.join(os.path.dirname(__file__), 'test_image.jpeg')
with open(file_path, 'rb') as fp:
    data = {
        'post': post.id,
        'photo': fp
    }
    response = self.client.post(url, data)

self.assertEqual(response.status_code, status.HTTP_201_CREATED)
```