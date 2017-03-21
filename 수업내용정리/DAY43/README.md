# 43일차 

####실수 리스트 

-  ImportError: cannot import name 'PostPhotoSerializer'
serializers라는 패키지 내에 post 와 post_photo로 파이썬 파일을 나누고 각각에 serializer 클래스를 정의해놨다.
이경우 PostSerializer에서 PostPhotoSerializer를 임포트 해오는데서 문제가 생긴다. 
문제는 다음에 있다.

```
from .post import *
from .post_photo import *
```
다음과 같이 수정 
```
in post/serializers/post.py

from post.serializers.post_photo import PostPhotoSerializer
```

---
####더 찾아봐야할 내용 및 질문 

- ~~CursorPagination에 관해서 읽기~~
> The cursor-based pagination presents an opaque "cursor" indicator that the client may use to page through the result set. This pagination style only presents forward and reverse controls, and does not allow the client to navigate to arbitrary positions.


---

####수업로드 

- 인스타그램 WEB API만들기 (REST활용) 심화 
	- serializer간에 관계 설정하는 방법 
	- pagination
	- follow, block, friends 모델 구현하기


	
---
####수업진행 포인트

- depth 지정 ModelSerializer에서 relation설정 시 
	- depth 설정 시 자신이 원하는 필드만을 보낼 수가 없고 모든 필드가 나오기 때문에 문제가 생길 수 있다. 
	- 이경우 customize를 해야 한다. 
	
- primitive representation 기본자료형 (파이썬 자료형태, dict, str등 ) 

- Serializer를 써서 두 Serializer의 관계를 나타내기 (복잡한 객체를 나타내기)
```
class UserSerializer 

class CommentSerializer
user = UserSerializer

``` 


-author의 객체를 출력하는지 확인하는 테스트 코드 짜기 

```
        for item in response.data:
            self.assertIn('author', item)
            item_author = item['author']
            self.assertIn('pk', item_author)
            self.assertIn('username', item_author)
```


- 다음의 오류가 뜨는 이유 

```
{
  "author": [
    "이 항목을 채워주십시오."
  ]
}
```

유저를 재정의 했기 떄문에 오류가 뜬다.
read_only= True설정을 해줘야 한다. 

다음과 같이 작성한다.

```
class PostSerializer(serializers.ModelSerializer):
    author = UserSerializer(read_only=True)

    class Meta:
        model = Post
        fields = (
            'pk',
            'author',
            'created_date',
        )
        read_only_fields = (
            'created_date',
            # 'author', 위에 read_only=True를 설정했기 때문에 지워도 무방. 
        )
```

위와 같이 read_only=True 가 되면 author값이 추가로 없어도 된다????


- 가장 간단하게 pagination을 적용하는 방법

```
in settings.py 

REST_FRAMEWORK = {
    'DEFAULT_PAGINATION_CLASS': 'rest_framework.pagination.LimitOffsetPagination',
    'PAGE_SIZE': 10
}
```
그러나 원하지 않는 곳까지 모두 적용되버리는 부작용이 있다. 

- Customized 된 pagination 구현하기 

```
in utils/pagination.py 

class PostPagination(CursorPagination):
    page_size = 5
    ordering = '-created-date'
```
위와 같이 pagination에 관련된 부분은 따로 utils  폴더를 만들어 여기서 관리한다. 

- 오프셋을 활용하면 아무리 큰 데이터 여도 메모리 부하를 줄일 수 있다.

- 페이지네이션 종류 3개 

	- CursorPagination (인스타 그램에 가장 적당)
	- Custom Pagination
	- LimitSetoff Pagination

- 한줄 작성하기


```
    def create_dummy_users(self, num):
        return [MyUser.objects.create(username='user{}'.format(i)) for i in range(num)]
        
        users = []
        for i in range(num):
            user = MyUser.objects.create(
                username = 'user{}'.format(i),
                password = 'test_password'
            )
            users.append(user)
            return users
```
