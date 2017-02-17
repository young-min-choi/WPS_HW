# 28일차 

#### 실수사항 
- no such column as post_post_isvisible: 마이그레이션을 안했다. 
- __models.py__ 건드릴때마다 반드시 migration하기. 

---
####질문사항

-  Initial 써서 한 세션 내에서 같은 유저네임을 사용하는것은 initial 값을 동적으로 할당하는 것인가?

- form.as_ul 이런게 실질적으로 쓰이는가? 

- redirect('/admin/')
	

---
####더 찾아봐야할 내용 

- settings.AUTH_USER_MODEL 어떤 기능을 하는것인가 

- django.contrib.messages / contenttypes / session의 역할 


---

####수업로드 

- 댓글 delete 기능 구현  
	- views.패키지 만들어서 views함수 나누기 
	- models도 패키지로 나누기 
	- __init__ 파일 설정하기 // urls.py에서 편리하게 기존의 것을 사용하기 위해 
	- 다음과 같은 사례를 막기 위해 views.post.post_list 
	
- post delete기능 구현 
	- post_delete(request) 함수 만들기 
	- 지우는게 아니라 안보이게 하기 

- admin 입장에서  visible, invisible 나누어서 보기 

- 이미지 업로드 여러개 하기 (장고 doc참고해서 혼자해보기) 

- 회원가입 받기
	- 비밀번호 2개 받아서 같은지 다른지
	- 회원가입 view함수 리팩토링하기 
	- 회원가입되면 바로 로그인시키고 post:list로 보내기 

---
####수업진행 포인트

- __init.py__
```
from .comment import *
from .post import *
```


- 데이터 자체가 매우 중요할 경우 이것을 사용자가 지워버려도 DB에 남기기
	- is_active, is_visible같은 기능을 만들기 
	- 다음을 조심하기. post-list가져올때도 .all()로 모든 것을 가져오는 것이 아니라, filter를 사용해서 is_visible=True만 가져와야 한다. 
```
def post_list(request):

    if request.method =="POST":
        pass

    posts = Post.objects.filter(is_visible=True)
```

```
class PostManager(models.Manager):
    def visible(self):
        return super().get_queryset().filter(is_visible=True)


class PostUserVisibleManager(models.Manager):
    def get_queryset(self):
        return super().get_queryset().filter(is_visible=True)
```

```
    objects = PostManager()
    visibles = PostUserVisibleManager()

    #모든거 가져오기
    #Post.objects에 visible 메쏘드만 추가한 것. 
    #Post.objects.all()

    #비지블 한거 가져오기
    #Post.objects.visible()
    #Post.visibles.all()
```


- settings.AUTH_USER_MODEL 를 MyUser대신 사용하기. 
	- 직접 가져다 쓰는게 아니라, 설정값에 있는 애를 가져다 써야 한다. 
	- 어느쪽으로 써도 동작은 하나, 두가지 경우에 모두 대비하기 위해? 
	- settings.py에 보면 AUTH_USER_MODEL = 'member.MyUser'라고 적혀 있는데 얘를 쓰기 위한 것이다. 
	
	
- import 주의하기 
	- from post.models import Post 하려고하면 Comment를 먼저 임포트해오고 
	- import할때 최대한 상대경로를 사용하는 것이 좋다. 왜냐하면 바깥쪽 패키지의 이름이 바뀔 수 있기 때문에. 

- 미래에 대비하기 (db_delete=False라는 쓰지 않는 인자 만들어놓기)
```
def post_delete(request, post_id, db_delete=False):
    if request.method =="POST":
        post = Post.objects.get(id=post_id)
        if request.user == post.author:
            if db_delete:
                post.delete()
            else:
                post.is_visible = False
                post.save()
        return redirect('post:list')
```

- 
```
class PostAdmin(admin.ModelAdmin):
    list_display = ('id', 'author', 'is_visible')

admin.site.register(Post, PostAdmin)
```

- 강사님 migrations파일 가져올때의 문제점. 
	- is_visible이 없고 있고의 차이. 
	- fake옵션 쓰기. 
	
- 함부 내에 내부 함수를 만드는 이유
	- 함수 내에서 내부적으로 쓰이는 로직을 밖으로 빼면, 더욱 간명하게 보인다. 
```
    def create_post_comment(file, comment_content):
        post= Post(author=request.user, photo=file)
        post.save()
        if comment_content != '':
            post.add_coment(user=request.user, content=comment_content)
``` 

- 

```
    {{ form.non_field_errors }}
    <div>
        {% for field in form %}
        {{ field.label_tag }}
        {{ field }}
        {{ field.errors }}
    </div>
    {% endfor %}
```
